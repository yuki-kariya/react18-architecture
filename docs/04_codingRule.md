# コーディング規約

## フォーマット規約

| 対象                                               | ケース         | サンプル                     |
| -------------------------------------------------- | -------------- | ---------------------------- |
| 変数、ローカルスコープの定数、関数、クラスメンバー | lowerCamelCase | userName, plainPassword      |
| グローバルスコープの定数                           | CONSTANT_CASE  | API_URL                      |
| クラス、React コンポーネント                       | PascalCase     | UserProfile, EditProfilePage |

## 命名

### 明確な単語を選ぶ

```ts
function getUserProfile(userId: UserID) {
  /* do something */
}
```

「get」という単語からだと、どこから情報を取得するのかわからない。

データの取得先にはローカルキャッシュやインターネットなど、様々考えられる。

インターネットからとってくるのであれば fetchUserInfo や、donwloadUserInfo のほうがより明確。

### より適切な単語を選ぶ

エンティティの値や目的をあらわした名前を選ぶ。

| 単語      | 類義語                                                                                                              | 使い分け                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| --------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| make      | create, build, new, get, fetch, load, generate, compose                                                             | **make** は汎用語のため極力避け、以下を状況で使い分ける：<br>• **create** … 一意の新規作成（DB レコード・ファイルなど）<br>• **build** … 依存物を組み立てて再利用可能な成果物を生成（オブジェクトツリー、バイナリ）<br>• **new** … クラスや構造体のインスタンス化を強調（OOP コンストラクタ）<br>• **get** … キャッシュを意識せず即時取得（メモリ上の値）<br>• **fetch** … 外部 I/O／ネットワーク経由で取得（API, DB リモート呼び出し）<br>• **load** … ストレージからメモリへ読み込む（ファイル、ローカル DB）<br>• **generate** … アルゴリズムで動的に生成（乱数、レポート、コード）<br>• **compose** … 既存要素を合成して作る（関数合成、UI コンポーネント）                                                                                                                      |
| convert   | transform, map, cast, parse, serialize, deserialize, encode, decode                                                 | **convert** は「型やフォーマットを別表現へ置き換える」汎用動詞。<br>• **transform** … 内容を意味的・構造的に加工（正規化、リサイズなど）<br>• **map** … コレクション要素を 1 対 1 で写像（`map(func, list)`）<br>• **cast** … 型システムに従いプリミティブ型を変換（`int → float`）<br>• **parse** … 文字列を構造化データへ解析（JSON → オブジェクト）<br>• **serialize / deserialize** … オブジェクト ↔ バイト列・JSON へ永続化／復元<br>• **encode / decode** … 符号化方式や圧縮形式を変換（Base64, URL エンコード）                                                                                                                                                                                                                                                               |
| calculate | compute, evaluate, determine, estimate, derive, assess, figure, quantify, tally, total, solve, measure, approximate | **compute** … 数値を機械的に演算して厳密な結果を得る<br>**evaluate** … 数式・論理式・ラムダなどを評価して値や真偽を求める<br>**determine** … 条件や入力から状態・値を決定づける判断系ロジック<br>**estimate** … 統計やヒューリスティックで近似値・予測値を算出<br>**derive** … 既知の数式・法則から値を導出する理論計算<br>**assess** … 指標やリスク・コストを算定し評価スコアを返す<br>**figure** … 口語的に原因や答えを割り出す軽量計算・解析<br>**quantify** … 定性的な要素を数値化するメトリクス化処理<br>**tally** … 項目を逐次加算して集計<br>**total** … 最終的な総計や合計値を算出<br>**solve** … 方程式・最適化問題を解いて正確な解を求める<br>**measure** … 実測・計測して値を取得するセンサー／時間計測<br>**approximate** … 厳密解の代わりに近似アルゴリズムで値を求める |
| validate  | check, verify, confirm, ensure, assert, authenticate, vet, certify, test                                            | **check** … 値や状態をざっと点検して問題の有無を調べる軽量な検査<br>**verify** … 期待値や外部ソースと照合し一致を確認する厳密な検証<br>**confirm** … ユーザーや外部主体から最終的な同意・承認を得る<br>**ensure** … 前提条件を満たさない場合に例外を投げるなど保証を伴う確認<br>**assert** … 実行時やデバッグ時に不変条件を強制する内部チェック<br>**authenticate** … ID・パスワードやトークンで主体の真正性を確認する認証<br>**vet** … 安全性・適格性を多角的に精査するレビューや調査<br>**certify** … 公的機関や第三者が正式に有効性を認定<br>**test** … ユニット・インテグレーション等のテストで仕様通り動作するか検証                                                                                                                                                            |
| update    | modify, change, edit, revise, refresh, upgrade, patch, amend, replace, adjust, synchronize, renew                   | **modify** … 機能や挙動の一部を調整する部分的変更<br>**change** … 状態や値を別のものへ置き換える一般的更新<br>**edit** … テキストや設定を手動で書き換える細かな修正<br>**revise** … レビュー後に内容を見直して改訂するドキュメント更新<br>**refresh** … キャッシュや UI を最新状態へ同期させる再読み込み<br>**upgrade** … バージョンを上げ機能追加や性能向上を図る更新<br>**patch** … 不具合や脆弱性を最小限差分で修正<br>**amend** … 公式記録を訂正・補足する正規修正<br>**replace** … 旧要素を完全に置き換えて新しいものに差し替える<br>**adjust** … パラメータや設定を微調整して最適化<br>**synchronize** … 外部ソースや複数システム間でデータを一致させる<br>**renew** … ライセンスや有効期限を更新・延長                                                                        |
| execute   | run, perform, invoke, operate, trigger, launch, process, conduct, implement, administer                             | **run** … プログラムやスクリプトを通常モードで起動・継続実行<br>**perform** … 手順やオペレーションを順守して処理を遂行<br>**invoke** … 関数・メソッド・リモート API を呼び出して即座に実行<br>**operate** … 機械・システムを制御しながら動作させる<br>**trigger** … イベントやフックを発火させて後続処理を開始<br>**launch** … アプリケーションやサービスを立ち上げ<br>**process** … データやジョブをパイプラインで処理<br>**conduct** … 実験・調査・取引などを公式に実施<br>**implement** … 仕様や計画を具現化して動作させる実装フェーズ<br>**administer** … システム管理権限でタスクを実行・管理                                                                                                                                                                                   |
| send      | transmit, dispatch, deliver, publish, post, push, forward, submit, emit, mail, convey, ship                         | **transmit** … ネットワークや電波でデータ信号を送る<br>**dispatch** … 複数宛先やキューへ即時に送り出す<br>**deliver** … 受信側で確実に受け渡すことを強調<br>**publish** … サブスクライバー向けにブロードキャスト配信<br>**post** … SNS や掲示板など公開チャネルへ投稿<br>**push** … 非同期プッシュ通知やリアルタイム更新<br>**forward** … 受信メッセージを別の宛先へ転送<br>**submit** … フォームやジョブを受け付けシステムへ提出<br>**emit** … イベント駆動でシグナルやログを発火<br>**mail** … 電子メールプロトコルでメッセージを送信<br>**convey** … 意思や情報を広い意味で伝達<br>**ship** … 物理品やリリース版ソフトを出荷                                                                                                                                                      |
| init      | initialize, setup, start, configure, boot, bootstrap, prepare, prime, seed, instantiate, begin, establish           | **initialize** … リソース割り当てと既定値設定<br>**setup** … 環境や依存関係を整える構築<br>**start** … サービスやスレッドを実行状態へ移行<br>**configure** … オプションやパラメータを設定<br>**boot** … OS やファームウェアを最初から起動<br>**bootstrap** … 最小コードで立ち上げ依存をロード<br>**prepare** … データや前提条件を用意<br>**prime** … キャッシュを温め初回遅延を抑制<br>**seed** … 初期データや乱数シードを投入<br>**instantiate** … クラスからオブジェクト生成<br>**begin** … トランザクションやタイムウィンドウを開始<br>**establish** … ネットワーク接続やセッションを確立                                                                                                                                                                                         |

### 値の単位や属性をつける

```ts
// NG
const elapsed = start - end;

// OK
const elapsedSeconds = start - end;
```

時間の他にも

- bytes, mb, gb などのデータサイズ
- px, inch, cm などの大きさ

### その他注意事項

取り扱に注意を要する値の場合、変数名から判別可能にする。

```ts
const unsafeHtml = /* 無加工なhtml */
const urlBase64Enc = /* Base64エンコードされたURL */
```

### 名前の長さ

変数スコープが小さい場合、コード上でその変数を見かける箇所も少なくなる。

そのためある程度短い変数でも問題ない。

ただグローバルスコープのような、コードの広範にわたって影響する変数名には、多少長くても正確な名前を付ける。

### 単語の省略

初めてソースコードを見るような新しいメンバーが一目見て意味が分かる省略形なら良い。

ドメインに関する用語といった、プロジェクト固有の省略は NG。

documents を doc、string を str、evaluation を eval などは ok

### 誤解を招かない単語をえらぶ

filter(conditions: any) という関数があった場合、引数の条件に合う項目が返されるのか、それとも引数の条件に合致しない項目が除外されるのかわからない。

条件に合致する項目を返すなら select、除外するなら exclude にするなど、読み手に誤解を与えない単語をえらぶ。

| 用途   | 推奨する単語         | 説明                                                                                                       |
| ------ | -------------------- | ---------------------------------------------------------------------------------------------------------- |
| 限界値 | min, max             | limit だと、その値が範囲内なのか外なのか判別がつかない。最大・最小値は常に値が包含されていることがわかる。 |
| 範囲   | first,last begin,end | 同上                                                                                                       |
| 真偽値 | is,has,can,should    | 接頭辞に is などがあれば、一目見てフラグであることがわかる                                                 |

## 見た目

### 重複コード排除(DRY)

同一ロジックが 2 か所以上で検出された場合、ロジックの共通化を検討する。

#### そのロジックは本当に「共通」か？

同一ロジックとして共通化すべきコードの特徴は

構造が似ているという理由だけで異なる目的の処理を同じ関数の中に入れてしまうとかえって保守性を下げる結果を招く

1. 早すぎる共通化 (Hasty Abstraction)

   目的の異なるロジックを 1 つの関数に押し込め、分岐が肥大化する。

   ```ts
   // 「プロフィール写真」か「商品画像」かで処理が分岐
   function saveImage(
     type: "profile" | "product",
     ref: { id: string; url: string }
   ) {
     if (type === "profile") {
       resizeForAvatar(ref);
       uploadToBucket("avatars", ref);
     } else {
       optimizeForCatalog(ref);
       uploadToBucket("products", ref);
     }
   }
   ```

   type が増えれば複雑性を増すことが容易に想像できる。

   type ごとにどんな処理が実施されるのか判断しづらい。

2. フラグパラメータ地獄 (Flag Argument Hell)

   共通化の副作用で複数の真偽値フラグを持つ関数が誕生し、呼び出し側の意図がコードから読み取れなくなる。

   ```ts
   export function sendNotification(
     user: User,
     isAdmin: boolean,
     isTrial: boolean
   ) {
     if (isAdmin) {
       sendAdminNotice(user);
     } else if (isTrial) {
       sendTrialNotice(user);
     } else {
       sendRegularNotice(user);
     }
   }
   // 呼び出し側
   sendNotification(user, false, true); // ← 何の通知か一目でわかりにくい
   ```

3. 多目的ユーティリティクラス (God Utility)

   "とりあえず util" にメソッドを追加し続け、巨大クラスになってしまう。

   ```ts
   class AppUtils {
     static formatMoney(yen: number): string {
       /_ ... _/;
     }
     static escapeHtml(text: string): string {
       /_ ... _/;
     }
     static toAge(birthday: Date): number {
       /_ ... _/;
     }
     static calcTax(amount: number): number {
       /_ ... _/;
     }
     // ...さらに用途の異なるメソッドが追加され続ける
   }
   ```

   依存が集中しテスト困難。

   変更の影響範囲が見えにくい。

   そもそも何に使う関数なのかわからない。

### コードのシルエット統一

似た処理は同じ構造・順序・命名パターンで書き、読み手が構造を視覚的に把握できるようにする。

関連する処理は段落に分けて記述する。

```ts
function renderPaidLeaveSummary() {
  // ユーザーの有給休暇残日数を取得
  const currentUser = globalStore.getCurrentUser();
  const paidLeave = paidLeaveRepository.findByUserId(currentUser.id);

  // 現在日時点での残日数と、次回付与時点で失効する日数を計算する
  const nowRemaining = paidLeave.getRemainingDays(new Date());
  const willExpireNext = paidLeave.getDaysExpiringNextGrant();

  // 計算結果を UI に反映する
  paidLeaveView.show({
    remainingDays: nowRemaining,
    expiringSoonDays: willExpireNext,
  });
}
```

別のサンプルケースとして、React コンポーネント実装を例に考えてみる。

依存しているカスタムフックを先に記述し、ローカル State と useMemo や useReducer といった派生データを続けて記述することで、「外部データ → 内部状態」が把握しやすいようにする。

```tsx
export const UserProfileCard: React.FC = () => {
  // グローバルデータ／カスタムフック
  const currentUser = useCurrentUser();
  const {
    data: user,
    error, : fetchUserError,
    isPending : isFetchingUser
  } = useFetchUser(currentUser.id);

  // ローカル State
  const [isEditing, setIsEditing] = useState(false);

  // 派生データ (useMemo / useCallback)
  const fullName = useMemo(() => `${user.firstName} ${user.lastName}`, [user]);

  // 副作用 (useEffect)
  useEffect(() => {
    refresh();
  }, [refresh]);

  // ハンドラ
  const handleEditClick = () => setIsEditing(true);
  const handleClose = () => setIsEditing(false);

  // Render
  if (isFetchingUser) {
    return <Loading />
  }
  return (
    <>
      <ProfileCard
        name={fullName}
        avatarUrl={user.avatarUrl}
        onEdit={handleEditClick}
      />
      {isEditing && <EditProfileModal user={user} onClose={handleClose} />}
    </>
  );
};
```

## コメント

コメントの目的は、書き手に読み手の意図を知らせることにある。

コードを書くときに考えたことを読み手に伝える。

### 何をコメントすべきか

- コードを見てもすぐに理解できない処理内容の説明(意図や要約など)や背景
- 名前を見てもその値が何か理解できない定数の説明や背景
- 外部サービス接続、計算量など読み手が気にすべきことを表明する
- クラスやファイルの全体像を説明する

### 何をコメントすべきでないか

- コードを見たらすぐにわかること

### プラクティス

#### コメントの内容を示す接頭辞をつける。

| 記法  | 意味                       |
| ----- | -------------------------- |
| TODO  | 後で手を付ける             |
| FIXME | 既知の不具合があるコード   |
| HACK  | あまりきれいではない解決策 |
| XXX   | 問題のあるコード           |

#### 実装意図の説明

良い例)

`ConsultationRecord` はドメインやアプリ仕様（このケースだと医療機関の診察予約）に関する知識ないと理解するのに時間がかかるため、説明を入れている。

データ構造に関する要約をコメントしている。

`STATUS_ORDER`のような Map は、key-value が何のペアなのかを説明し、value について説明することで、どうやって使われるのかをイメージしやすいようにしている。

`sortRecords`には関数の処理概要と注意事項を記述している。

```ts
// 診察記録：予約→支払い→配送までを 1 レコードで管理
interface ConsultationRecord {
  status: "診察待ち" | "会計待ち" | "配送待ち" | "完了";
  // ステータスに応じた予定日時 (例: 診察予定日時、支払期日)
  // 「完了」の場合は常にnull
  scheduledAt: Date | null;
  // ステータスごとの実施日時 (例: 診察完了日時、配達完了日時)
  // 「完了」の場合は常にnull
  doneAt: Date | null;
}

// key => 診察ステータス、value => 優先順位
// 優先順位が低いほど先に表示する
const STATUS_ORDER: Record<ConsultationRecord["status"], number> = {
  診察待ち: 0,
  会計待ち: 1,
  配送待ち: 2,
  完了: 3,
};

// ステータス -> 予定日時の順に診察記録をソート
// 引数recordsはin-placeで更新されず、shallow-copyした配列を返す
function sortRecords(records: ConsultationRecord[]): ConsultationRecord[] {
  return records
    .filter((r) => r.status !== "完了")
    .sort((a, b) => {
      if (a.status === b.status) {
        // 「完了」以外のステータスでは、scheduledAtは常にDate
        const dateA = a.scheduledAt as Date;
        const dateB = b.scheduledAt as Date;
        return dateA.getTime() - dateB.getTime();
      }

      return STATUS_ORDER[a.status] - STATUS_ORDER[b.status];
    });
}
```

悪い例)

`ConsultationRecord` のコメントがなく、ビジネス上のどんな事象をモデル化したものなのかわからない。

`STATUS_ORDER` のコメントがない。
key が`ConsultationRecord`の status なのはわかるが、value の値が何かわからない。
実際に`STATUS_ORDER`を使用している処理で、初めて値が小さいほど先に表示されることがわかる。

`sortOrder`関数内のコメントについては、コードを見ればわかる内容なのでコメントを入れなくてよい。

```ts
interface ConsultationRecord {
  status: "診察待ち" | "会計待ち" | "配送待ち" | "完了";
  scheduledAt: Date | null;
  doneAt: Date | null;
}

const STATUS_ORDER: Record<ConsultationRecord["status"], number> = {
  診察待ち: 0,
  会計待ち: 1,
  配送待ち: 2,
  完了: 3,
};

function sortRecords(records: ConsultationRecord[]): ConsultationRecord[] {
  return records
    .filter((r) => r.status !== "完了")
    .sort((a, b) => {
      // statusが同じ値の場合は、scheduledAtの昇順で並び替える
      if (a.status === b.status) {
        const dateA = a.scheduledAt as Date;
        const dateB = b.scheduledAt as Date;
        return dateA.getTime() - dateB.getTime();
      }

      // STATUS_ORDERのvalueを比較する
      return STATUS_ORDER[a.status] - STATUS_ORDER[b.status];
    });
}
```
