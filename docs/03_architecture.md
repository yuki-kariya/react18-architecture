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

### Component

ドメイン知識を一切持たない UI コンポーネント。

例) Input, Select, Popup など

### Feature Component

機能単位の UI コンポーネント。

例) ProfileForm, UserSearchTable

### Page Component

画面 UI コンポーネント。

ルーティング、画面遷移、ページスコープ副作用（例：\<meta\>タグの更新、window.addEventListenner、history の更新など）を管理する。

UI は Component、および Feature Component を使用し実装する。

同サービスの API、および外部サービスの接続には Use Case を使用する。

エラーバウンダリや、SEO メタなどにも責務を持つ。

例) TopPage, ProfilePage

### Layout Component

複数のページに共通する外枠。

UI は Component、および Feature Component を使用し実装する。

同サービスの API、および外部サービスの接続には Use Case を使用する。

### Usecase

１ユーザーストーリーを実現する アプリケーションサービス関数。

以下の処理フローを実行する。

- Port を介して API / ストレージを呼び出し DTO を取得
- DTO → ViewModel へマッピング
- 権限・整合性チェックなど “ルール” を適用して返却

例) useFetchUser, useUpdateProfile

### Presenter

UI と Usecase の橋渡しを行う React カスタムフック 群。

### ViewModel

FE 用に再定義したドメインモデルの型とユーティリティ。

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

外部サービスから渡ってきたデータを、加工せず呼び出し元（多くの場合 Usecase）に連携する。

### Conig

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
├── components/                # Pure UI
│
├── feature-components/        # 機能 UI
│   └── {機能名}/
│
├── pages/                     # 画面コンポーネント
│   └── {画面名}Page/
│
├── layouts/                   # 共通レイアウト枠
│   └── {レイアウト名}Layout/
│
├── presenters/                # UI ↔ Usecase 橋渡し
│   └── useZipSearchPresenter.ts
│
├── usecases/                  # 1 ユーザーストーリー単位ロジック :contentReference[oaicite:19]{index=19}
│   ├── fetchUser.ts
│   └── updateProfile.ts
│
├── viewmodels/                # UI 専用型・派生値保持 :contentReference[oaicite:20]{index=20}
│   └── UserVM.ts
│
├── ports/                     # 抽象インタフェース (I/O) :contentReference[oaicite:21]{index=21}
│   └── UserApiPort.ts
│
├── adapters/                  # Port 実装（Axios 等） :contentReference[oaicite:22]{index=22}
│   └── userApiAdapter.ts
│
├── config/                    # 環境変数など設定 :contentReference[oaicite:23]{index=23}
│   └── env.ts
│
├── lib/                       # 外部ライブラリ共通設定 :contentReference[oaicite:24]{index=24}
│   ├── axiosClient.ts
│   └── reactQueryClient.ts
│
├── utils/                     # 汎用ヘルパー関数 :contentReference[oaicite:25]{index=25}
│   └── date.ts
│
├── locales/                   # i18n リソース :contentReference[oaicite:26]{index=26}
│   ├── ja/
│   │   └── common.json
│   └── en/
│       └── common.json
│
└── tests/                     # ユニット / IT テスト
    └── …

```
