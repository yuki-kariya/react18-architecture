# アーキテクチャ

## 本ドキュメントで取り扱う範囲

- React を主軸とした **小〜中規模** Web アプリケーション
- DDD / Clean Architecture の原則を **現実的なコストで適用** すること
- **ディレクトリ構成・命名規約・依存方向ルール** を後続章で詳細化

## 取り扱わない範囲

- 個別ライブラリ（React Router, Redux など）の詳細 API
- CI/CD, インフラ自動化手順
- 大規模マイクロサービス間の分散トランザクション設計

## 基本ルール

## 構成要素

### Component(汎用 UI コンポーネント)

ドメイン知識を一切持たない UI コンポーネント。

例) Input, Select, Popup など

### Feature Component(機能 UI コンポーネント)

機能単位の UI コンポーネント。

例) ProfileForm, UserSearchTable

### Page Component(画面 UI コンポーネント)

ルーティング、画面遷移、ページスコープ副作用（例：\<meta\>タグの更新、window.addEventListenner、history の更新など）を管理する。

UI は Component、および Feature Component を使用し実装する。

同サービスの API、および外部サービスの接続には Controller を使用する。

エラーバウンダリや、SEO メタなどにも責務を持つ。

例) TopPage, ProfilePage

### Layout Component(レイアウト UI コンポーネント)

複数のページに共通する外枠。

UI は Component、および Feature Component を使用し実装する。

同サービスの API、および外部サービスの接続には Controller を使用する。

### Controller

UI からの入力（フォーム値、イベントなど）を受け付け、UI の代わりに Use Case 呼び出しや Port 接続を行うメソッドを、UI に提供する。

メソッド内では、接続に必要な Input データの変換作業、Port から返却された値の ViewModel への変換を Presenter を呼び出し実行する。

メソッド内部でビジネスロジックの呼び出しが必要な場合は Use Case を呼び出し、外部サービスへの接続が必要な場合は、直接 Port に接続する。

カスタムフックで実装され、React コンポーネントと Use Case、Presenter との仲介を行う。

### Use Case

一連のビジネスルール（≒ ユースケース）を実行し、結果を返す。

ビジネスルールの実行には Port を経由し BE で処理する。

Port から返却された Output Dto を呼び出し元に返す

例) fetchUserProfile

### Presenter

Use Case の結果を受け取り、ViewModel に変換する。

一部画面コンポーネントの状態を受け取り、そのステートに適した ViewModel を適宜作成する。

### ViewModel

アプリケーションの関心ごと(≒ ドメイン)を、FE の UI 要件に合わせて再定義したデータモデル。

モデリングする対象に応じて、以下 2 つに分類することができる

### エンティティ ViewModel

抽象化する対象が、

- データの生成から永続層への保存、更新、破棄までのライフサイクルで連続性（≒ 同一性）を持つ -アプリケーションのユーザーにとって重要な区別が属性から同区利している

場合はエンティティに該当し、さらに UI 実装のために再モデル化したものがエンティティ ViewModel である。

`id`といった識別子を持たせ、連続性を持たせるオブジェクトが該当する。

### 値オブジェクト ViewModel

抽象化する対象が

- ドメインにおける記述的な側面、つまり属性を表現するための値
- 概念的な同一性を持たない

場合は値オブジェクトに該当し、さらに UI 実装のために再モデル化したものが値オブジェクト ViewModel である。

エンティティの属性、もしくはほかの値オブジェクトの属性や、オブジェクト間のメッセージのパラメータとして使用されることが多い。

### Port

アプリが外部に期待する 抽象インタフェース。

API のみならず、LocalStorage や外部サービスとの接続も対象。

例) UserApiPort, JsBridgePort

### Adapter

外部 API の呼び出しロジックの詳細(Port の実装)。

外部 API には以下のようなものが含まれる。

- 同システムの BE が提供している API
- 外部ベンダーが提供している API
- sessionStorage / localStorage

外部サービスから渡ってきたデータを、加工せず呼び出し元（多くの場合 Use Case）に連携する。

### Config

アプリケーション設定。

環境変数を型月でエクスポートするなど。

### Lib

外部ライブラリの 初期化・共通設定 をここに集約。

### Utility

日付計算・フォーマッタなど ドメイン知識を伴わない汎用関数。

### Locales

## 構成イメージ

TODO : 各クラスの関係性を図でまとめる

## ディレクトリ構成

```txt
src/
├── components/
│
├── feature-components/
│   └── {機能名}/
│
├── pages/
│   └── {画面名}Page/
│
├── layouts/
│   └── {レイアウト名}Layout/
│
├── presenters/
│   └── useZipSearchPresenter.ts
│
├── usecases/
│   ├── fetchUser.ts
│   └── updateProfile.ts
│
├── viewmodels/
│   └── UserVM.ts
│
├── ports/
|   ├── api/
|   |   ├── types/
|   |   |  ├── schemas/
|   |   |  |   └── User.ts
|   |   |  ├── FetchUserRequest.ts
|   |   |  └── FetchUserResponse.ts
│   |   └── userApi
│   └── storage/
├── adapters/
│   └── userApiAdapter.ts
│
├── config/
│   └── env.ts
│
├── lib/
│   ├── axiosClient.ts
│   └── reactQueryClient.ts
│
├── utils/
│   └── date.ts
│
├── locales/
│   ├── ja/
│   │   └── common.json
│   └── en/
│       └── common.json
│
└── tests/
    └── …

```
