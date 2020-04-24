# 01.2

https://codelabs.developers.google.com/codelabs/kotlin-android-training-app-anatomy/index.html?index=..%2F..android-kotlin-fundamentals#7

- `AppCompatActivity`を継承した`MainActivity`が生成される。

  - `Activity`は UI 描画と input イベントを取得する責務を持つ
  - Activity はコンストラクタで初期化しない。`onCreate()`というライフサイクルメソッドで行う。

- `R.layout.activity_main`について

  - クラス`R`は app のすべてのアセットを表す。res ディレクトリのコンテンツ。（res の R？）
  - `R.layout.activity_main`は res/layout/activity_main.xml を参照していることになる

- res/layout は app の見た目を定義する

  - `match_parent`: 親要素にあわせる
  - `wrap_content`: いい感じのサイズになる（？）
  - LinearLayout では`layout_gravity`とかつかう
  - サイズのハードコーディングは避ける。ここはあとでよく調べる

- `strings.xml`で文字列を管理するのがベストプラクティス

  - `android:text`に記述したところから Opt+Enter で抽出できる

- code で button の参照を得る
  - `MainActivity`内で View のコンポーネントの参照を得たい
  - button に`android:id`で id を振る、`findViewById()`を使って id 経由で参照を得る
  - `R.id`に id 情報が付く
  - Button インスタンスに`setOnClickListener`で click 時の挙動を定義

# 01.3

https://codelabs.developers.google.com/codelabs/kotlin-android-training-images-compat/index.html?index=..%2F..android-kotlin-fundamentals#8

- res/drawable に画像ファイルを置く
  - xml 形式だと劣化しなくてよい
  - ImageView
- UI 画面の placeholder として`tools:src`を使うといい

  - `tools`という prefix はエディタで編集時に適用されるだけ。実行時には無視される(02.3)

- Activity はコンストラクタで初期化出来ないので、メンバ変数は nullable にするしかなくなる
  - 毎回 null チェックが必要になって面倒
  - `lateinit`を使う

# 01.4

https://codelabs.developers.google.com/codelabs/kotlin-android-training-available-resources/index.html?index=..%2F..android-kotlin-fundamentals#5

# 02.1

https://codelabs.developers.google.com/codelabs/kotlin-android-training-linear-layout/index.html?index=..%2F..android-kotlin-fundamentals#10

- padding などは dimension.xml で管理
- font は res/font
- style もまとめて xml で管理できる。使い回しができる
- ScrollView で囲むとスクロール可能になる

# 02.2

- attribute の inputType から email や password などの形式を選べる
  - html の input タグの type と同じ

# 02.3

https://codelabs.developers.google.com/codelabs/kotlin-android-training-constraint-layout/index.html#14

- ConstraintLayout
- Chains
  - ２つのコンポーネントを結びつけて constraint を一元管理できるみたいなイメージ
  - CSS の flexbox みたいなデザインにしたいときに使う
- text の baseline を揃えたい
  - 右クリック →show baseline→ ドラッグして結びつける
  - layout_constraintVertical_bias よくわからず

# 02.4

https://codelabs.developers.google.com/codelabs/kotlin-android-training-data-binding-basics/index.html#5

- 毎回 findViewById しているとパフォーマンスに影響する,data と view が  密結合
- → Data binding オブジェクトで管理する
- `build.gradle`から dataBinding を enabled にする
  - JDK9 以上だと deprecated な物を使う（？）ので別途パッケージが必要
  - gradle の dependency に書いたがうまくビルドできなかった
  - JDK8 にすることでとりあえず解決
- setContentView を`DataBindingUtil`クラスのものを使う
  - `binding = DataBindingUtil.setContentView(this, R.layout.activity_main)`
  - この`binding`変数が view への参照を持つ
    - `binding.some_button.setOnClickListener {...}`
    - `binding.some_text.text = ...`
  - たくさんの binding 参照へアクセスするときは`binding.apply{}`構文をつかう（調べる）
- `<data><variable name=...></data>`とクラスと参照を定義
- View では`@={}`ディレクティブで参照を得る
- binding オブジェクトを変更したときは`invalidateAll()`
- fragment 内で binding を得るには、create 時に`DataBindingUtil.inflate`

# 03.1

https://codelabs.developers.google.com/codelabs/kotlin-android-training-create-and-add-fragment/index.html#5

- Fragment
  - よくわからず

# 03.2 ?

https://codelabs.developers.google.com/codelabs/kotlin-android-training-add-navigation/index.html#11

- navigation
  - NavHostFragment を作り、res/navigation で画面遷移を管理できる
  - `view.findNavController().navigate(R.id.some_fragment)`で遷移
- 戻る制御
  - popUpTo: バックキーでどこまで戻るかを指定
  - popUpToInclusive: true にすると popUpTo で指定した前まで戻る
- MainActivity では初期化と UI セット、hostFragment を表示
  - hostFragment は実体を持たず、navigation に従っている
  - 一番最初の navigation が表示される
  - `view.findNavController().navigate`で遷移

# 03.3

https://codelabs.developers.google.com/codelabs/kotlin-android-training-start-external-activity/index.html#6

- fragment 間のデータ渡し
  - 型安全などの観点で safe args を使う
  - key-value の構造でやりとりする
- `navigate(R.id.some_id)`を`navigate(SomeDirections.hogeToFoo())`に置き換える
  - `SomeDirections`は自動生成（だよね？）
  - arguments を設定すると、 `hogeToFoo(arg1, arg2)`みたいに渡せる
  - `val args = SomeFragmentArgs.fromBundle(arguments!!)`で受け取る
- intent

# 04.1 lifecycle

https://codelabs.developers.google.com/codelabs/kotlin-android-training-lifecycles-logging/index.html#7

- onCreate() to create the app.
- onStart() to start it and make it visible on the screen.
- onResume() to give the activity focus and make it ready for the user to interact with it

# 04.2 lifecycle

https://codelabs.developers.google.com/codelabs/kotlin-android-training-complex-lifecycle/index.html#7

# 05.1 MVVVM

https://codelabs.developers.google.com/codelabs/kotlin-android-training-view-model/#9

- UI Controller
  - 表示に関するロジックしか持たない。もらったデータを表示するだけ
- ViewModel

  - 表示するデータを整形するロジックを持つ

- 画面が回転したときに View が再生成され、データが消えてしまう
  - ViewModel は消えないので ViewModel で管理する
  - ViewModel を直接インスタンス化しても毎回別のインスタンスとなるので、ViewModelProvider でインスタンス化する
    - `ViewModelProviders.of(this).get`でいい感じに全開のインスタンスをとってくる

# 05.2 LiveData

https://codelabs.developers.google.com/codelabs/kotlin-android-training-live-data/#10

- `val hoge = MutableLiveData<String>`と LiveData でラップする
- 初期化は`hoge.value = ""`
- LiveData を ViewModel で定義し、UIController で observe する
  - `viewModel.hoge.observe(viewLifecycleOwner, Observer { newHoge -> }`
- MutableLiveData と LiveData がある

  - `private var _str = MutableLiveData<String>`
  - `val str: LiveData<String>`
  - `get() = _str`
  - のように mutable 塗装でないもので分けるパターン（？）

- Observer Pattern
  - LiveData でラップしている変数が変化したときに Observer に通知が来る
  - LiveData は VM, observer は UIController

# 05.3

https://codelabs.developers.google.com/codelabs/kotlin-android-training-live-data-data-binding/#6

- 今；Button(V) - SomeFragment(UIController) - SomeViewModel
  - button のクリックは fragment で setListener され、処理は VM が担当する
  - V と VM は直接やり取りしていない
- V と VM でやりとりする
  - some_fragment に`<data><variable name="someViewModel"></variable></data>`内に VM を書く
  - SomeFragment で`binding.someViewModel = viewModel`
  - some_fragment 内で`android:onClick="@{() -> someViewModel.onClick()}"`
  - LiveData も Fragment を経由（observe）せず V で observe する
    - react の reactive に似てる
    - `binding.lifecycleOwner = viewLifecycleOwner`と owner を渡す
    - V 内で format できる`android:text="@{@string/quote_format(gameViewModel.word)}"`
    - EditText などで 2waybinding する場合は`text="@={vm.text}"`

# 05.4

https://codelabs.developers.google.com/codelabs/kotlin-android-training-live-data-transformations/#7

- `Transformations.map(LiveData) { data -> }`は LiveData を受け取って LiveData を返す
  - LiveData の変更を検知してこれも変化する

# 06.1

https://codelabs.developers.google.com/codelabs/kotlin-android-training-room-database/index.html?index=..%2F..android-kotlin-fundamentals#8

- Room
  - データを永続化するローカル DB
- Entity
  - オブジェクト、そのプロパティを表現する
  - entity class は table, instance は row、プロパティは column に対応する
- query
  - DBtable から data を取るリクエスト
- DAO (Data Access Object)
  - DB へアクセスするインターフェースを定義するオブジェクト
  - @Insert,@Update など関数を定義していく
  - SQL と関数のマッピングを行うオブジェクト

# 07.1 RecyclerView

https://codelabs.developers.google.com/codelabs/kotlin-android-training-recyclerview-fundamentals/index.html?index=..%2F..android-kotlin-fundamentals#7

- adapter pattern
  - app のデータを RecyclerView が表示できるよう変換するもの。コンセントの変換器みたいなあれ
  - app のデータ構造（DB）を変えずにいろんな RecyclerView に使えるよね
- RecyclerView はリスト UI のコンテナみたいなイメージ
  - デザインは LayoutManager が渡す
    - `app:layoutManager`
  - リスト１つは RecyclerView.ViewHolder を拡張したクラスのインスタンス
  - Adapter は ViewHolder へデータを整形して流す

# 07.2

https://codelabs.developers.google.com/codelabs/kotlin-android-training-diffutil-databinding/#8

- DiffUtil
  - 既存の setter で`notifyDataSetChanged()`で RecyclerView に変更を通知するのは非効率
  - 2 つの list を効率的に比較する DiffUtil を使って通知する
    - DiffUtil.ItemCallback を継承した Callback 内でリストアイテムの新旧の比較を行う
  - Adapter は`ListAdapter<DataClass, ViewHolder>(SomeCallback())`を継承させる
    - このとき Adapter 内ではリストのデータを保持する必要はなくなり、`getItem(position)`で取得できる
- Binding adapter
  - Transformations を使って LiveData を変換していた。これは複雑な変換に不向き
  - `@BindingAdapter("sleepDurationFormatted")`
  - `app:sleepDurationFormatted`と使えるようになる

# 07.3

https://codelabs.developers.google.com/codelabs/kotlin-android-training-grid-layout/#5

- GridLayout を使うとグリッドリストができる。custom LayoutManager
- 07.2 では`app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"`とした
- １つのアイテムを span と定義し、１行にいくつ span を置くかを定義する
  - １行３ span とし、あるアイテムの span を３つ分とれば、１行１アイテム、１行３アイテムといったレイアウトが可能
  - `val manager = GridLayoutManager(activity, spanSize)`
  - `binding.hoge.layoutManager = manager`

# 07.4

https://codelabs.developers.google.com/codelabs/kotlin-android-training-interacting-with-items/#6

- Clickable にする
- ViewHolder にクリックリスナーを貼る
- ViewModel がクリック時の責務を負う（RecyclerView が負うこともあるがここでは触れない）
- clickListener を RecyclerView の ConstraintLayout 全体につける（xml）
- Adapter に ClickListener を渡し、`holder.bind(item, clickListener)`とバインドする

# 08.1

https://codelabs.developers.google.com/codelabs/kotlin-android-training-internet-data/index.html?index=..%2F..android-kotlin-fundamentals#7

- coroutine
  - js の async await みたいな

# 08.2

https://codelabs.developers.google.com/codelabs/kotlin-android-training-internet-images/#6

- 画像読み込みライブラリ Glide の説明

# MVVM

https://speakerdeck.com/star_zero/databindingteshi-xian-surumvvm-architecture?slide=85

- View

  - Fragment, Activity, XML
  - ViewModel の状態を反映
    - `android:text="@{viewModel.title}"`
    - `app:imageFromUrl="@{viewModel.imageUrl}"`：カスタムセッター
  - View の入力を VM に伝える
    - 双方向バインディング
    - `android:text="@={viewModel.title}"`
  - VM にイベントの処理を委譲
    - `android:onClick="@{() => viewModel.onClick()}"`
    - メニュー処理など、DataBinding が使えない場合
      - `onOptionsItemSelected()`で`viewModel.someAction()`を呼ぶだけにする

- ViewModel

  - View のための状態保持と公開
    - LiveData, getter
      LiveData は lifecycle と同期しているので、自動で開放などされる
  - Model の処理呼び出し
    - イベントに対する反応と、戻り値のないメソッドの呼び出しのみ（WIP)
    - `model.someOperation()`
  - View への変更通知
    - LiveData の場合`=`で`.set`を呼んでいる？明示的に通知しない
    - property の場合`notifyPropertyChanged`
    - ダイアログなど直接通知できない場合？？
  - VM から API を叩いていいんじゃないの？
    https://developer.android.com/jetpack/docs/guide#fetch-data
    - VM の責務が大きくなり関心の分離原則に反する
    - VM のスコープは V のライフサイクルに紐付いているので、V の終了時にデータも消える
      これは好ましくない（キャッシュ？）（fragment 変えるたびに talk 情報が消える）
    - repository に委譲

- Model
  - V と VM 以外の処理
    - ドメイン・ビジネスロジック
    - DB,API,その他
  - VM への変更通知
    - VM では戻り値のないメソッドを呼んでいるので、通知する必要がある
    - model の状態を変更させる、model が変更されたら変更通知を送る
    - VM はそれを受け取るだけ
    - 例外処理も Model で処理して通知イベントを送る
