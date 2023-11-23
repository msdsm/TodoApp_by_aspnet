# ASP.NETでToDoアプリの開発

## ソース
- Udemy:入門者向け ASP.NET MVCでWebアプリ開発のノウハウを学ぼう！

## 作業進捗
### プロジェクトの作成
- Visual StudioからASPNET Webアプリケーション(.NET Framework)でプロジェクトを新規作成
    - Visual C#であることを確認
- 名前をTodoAPPにする
- 空を選択して、フォルダー及びコア参照でMVCを選択
- EntityFrameworkインストール
    - 参照からnugetを選択
    - EntityFrameworkインストール

### Modelの作成
- Modelsを右クリックして追加->クラスを選択->`Todo.cs`を作成
- `TodoesContext.cs`を作成

### Viewの作成
- Viewsを右クリックして追加->MVC5レイアウトページ(razer)を選択、名前はデフォルトのまま
    - HTMLにC#を埋め込むもの
    - @がサーバーサイドで実行？
- ビルドしてみる
    - ビルドからソリューションのビルドを選択

### Controllerの作成
- Controllersを右クリックして追加->コントローラー->EntityFrameworkを使用したビューがあるMVC5コントローラーを選択
    - モデルクラス:Todo作成
    - データコンテキストクラス:TodoesContext
    - ビュー:Views/_LayoutPage1.cshtml
- スキャフォールディングが始まる
    - CRUD処理を行う画面のコードをデータモデルをもとにして自動生成する機能
- ViewsとControllersの中に各ファイルが自動で生成される

### デフォルトページの設定
- `App_start/RouteConfig.cs`を編集
    - defaultsで`Controller="todoes"`に変更

### 動作確認
- デバッグしてサイト起動するか確認
- データの追加削除やってみる
- 実際にデータベース見てみる
    - コマンドプロンプトで`sqllocaldb i`
    - 出力結果コピー
    - サーバーエクスプローラーから接続
    - テーブルの中のTodoesから表示
    - あるか確認
### Twitter Bootstrapの導入
- Twitterが開発しているオープンソースのCSSフレームワーク
- asp.netはbootstrapを使うことが想定されている
- 以下作業
    - 参照を右クリック
    - NuGetパッケージの管理をクリック
    - Bootstrapと検索をかけて、最新の安定板となっていることを確認してインストール
        - Contentフォルダが作成されbootstrapファイルが大量に作成される
    - `_LayoutPage1.cshtml`を編集

### メンバーシップフレームワークによる認証機能の実装
- Modelsを右クリックして追加->クラス
    - `CustomMembershipProvider.cs`を作成
    - MembershipProviderを継承
        - クイックから必要なものusingしてもらう
    - CustomMembershipProviderに波船が出るのでクイックで対応
        - MembershipProviderが抽象クラスになっていて継承するメソッドの中身を書かないといけないから怒られている
        - クイック操作->抽象クラスの実装をクリックすると自動で全部作られる(Visual Studioすごい)
            - 中身はすべて例外を投げるようなもの
    - ValidateUserのみを使うのでこれだけ中身書いていく
        - ユーザー名とパスワードが一致するならtrue返す
    
- 同じように`Models/CustomRoleProviders.cs`を作成
    - GetRolesForUserとIsUserInRoleの中身を記述

### ログイン機能の実装
- `Models/LoginViewprovider.cs`を作成
    - データ定義
- Controllersを右クリックしてコントローラー->MVC5空->`LoginController.cs`を作成
    - AlloWanonymousでログインしていなくてもアクセスできるようにする
    - ログイン機能の実装
        - クッキーを利用
    - ログアウト機能の実装

### ログインページの実装
- LoginController.csのactionresult　Indexを選択して右クリック->ビューの追加
    - ビュー名:index
    - テンプレート:Create
    - モデルクラス:LoginViewModel
- 編集
- 共通レイアウトファイルをログインしているかどうかによって表示内容変更するように編集
- TodoesControllerにはサインインしていないとアクセスできないようにAuthorizeアノテーション追加
- Web.configの編集(Views/web.configではないことに注意)
    - authentication,membership,rolemanagerタグの編集
    - 

### 動作確認
- ログインできない
    - LoginController.csいろいろいじった結果以下のことがわかった
        - `ModelState.IsValid`がfalseを返している
        - `thies.membershipProvider.ValidateUesr(model.UserName, model.Password`がfalseを返している
    - 解決した
        - `Include = "UserName.Password"`ではなく`Include = "UserName,Password"`だった

### 認証情報のEntityframework
- `Role.cs`,`User.cs`作成
- `TodoesContext.cs`編集
- `CustomerMemvershipProvider.cs`と`CustomRoleProvider.cs`編集
    - 固定のユーザー名とパスワードで認証していた
    - データベースにアクセスして認証するように変更する
- マイグレーションの設定(UserやRoleなど変更した際にデータベースが自動で変更される仕組み)
    - ツール->Nugetパッケージ->パッケージマネージャーコンソール
    - `Enable-Migrations -EnableAutomaticMigrations`
    - エラーメッセージ？`initialcreateが見つからない？`
    - `Migrations/Configuration.cs`が作成される
- `Migrations/COnfiguration.cs`編集
    - `Seed`を変更していく
        - マイグレーション時に最初に呼ばれる関数？
        - 各カラムの値の初期化を行う
- `Global.asax.cs/Global.asax.cs`を編集
    - このファイルの`App_Start`がTodoアプリを起動したときに最初に呼ばれる関数
- 動作確認してうまくログインできていることを確認した

### ユーザー管理機能の実装





### 構造
- App_start
    - `RouteConfig.cs`
- Controllers
    - `TodoesControllers.cs`
- Models
    - `Todo.cs`
    - `TodoesContext.cs`
    - `CustomMembershipProvider.cs`
    - `CustomRoleProvider.cs`
    - `LoginViewModel.cs`
    - ``
- Views
    - Todoes
        - `Create.cshtml`
         `Delete.cshtml`
        - `Details.cshtml`
        - `Edit.cshtml`
        - `Index.cshtml`
    - `_LayoutPage1.cshtml`
        - linkタグでbootstrap読み込む
        - divタグにcontainer追加(左右に適切な余白を作るため)


## ソースコード説明

### App_startRouteConfig.cs
- ルーティングを行う
### Controllers/TodoesControllers.cs
### Models/Todo.cs
- データ構造定義
- 変数名とは別に`DisplayName`で表示名を変更可能
### Models/TodoesContext.cs
### Views/Todoes/Todoes/Create.cshtml
### Views/Todoes/Delete.cshtml
### Views/Todoes/Details.cshtml
### Views/Todoes/Edit.cshtml
### Views/Todoes/Index.cshtml
### Views/_LayoutPage1.cshtml
- @で始まるところはサーバーサイドで実行
- 共通レイアウトを設定している
- Views/Todoes/*.cshtmlのソースコードを見ると、この共通レイアウトを読み込んでいる

### Models/CustomMembershipProvider.cs
### Models/CustomRoleProvider.cs

### Models/LoginViewModel.cs
- ログインのためのデータ定義
    - password
    - username

### Controllers/LoginController.cs

## セキュリティ対策

### クロスサイトリクエストフォージェリ(CSRF)
#### 攻撃内容
- 文字通り「サイト横断的に（Cross Site）リクエストを偽装（Request Forgeries）する」攻撃
ログイン状態を利用され悪意のある偽サイトなどからの送信データを本人になりすませてサーバーに入り込める
#### 対策方法
- サイトにトークンを埋め込んでサーバー側もトークンに一致しなければブロックする
- トークンはランダムに生成すればよい

### 過多ポスティング攻撃
- クライアントからの入力値を自動的にモデルに割り当てるモデルバインドという機能がある
- これがセキュリティホールの原因となりうる
- クライアントが変更することのできないプロパティの値を含んだデータを送信すると、Createメソッドはその値をモデルに割り当ててしまう
#### 対策方法
- モデルバインドするプロパティを明示することにより防止