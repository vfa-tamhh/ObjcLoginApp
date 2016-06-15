# 【iOS Objective-C】アプリにログイン機能をつけよう！
![画像1](/readme-img/001.png)

## 概要
* [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)の『会員管理機能』を利用してObjective-Cアプリにログイン機能を実装したサンプルプロジェクトです
* 簡単な操作ですぐに [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)の機能を体験いただけます★☆

## ニフティクラウドmobile backendって何？？
スマートフォンアプリのバックエンド機能（プッシュ通知・データストア・会員管理・ファイルストア・SNS連携・位置情報検索・スクリプト）が**開発不要**、しかも基本**無料**(注1)で使えるクラウドサービス！

注1：詳しくは[こちら](http://mb.cloud.nifty.com/price.htm)をご覧ください

![画像2](/readme-img/002.png)

## 動作環境
* Mac OS X 10.10(Yosemite)
* Xcode ver. 7.2.1
* Simulator ver. 9.2

※上記内容で動作確認をしています。


## 手順
### 1. [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)の会員登録とログイン→アプリ作成

* 上記リンクから会員登録（無料）をします。登録ができたらログインをすると下図のように「アプリの新規作成」画面が出るのでアプリを作成します

![画像3](/readme-img/003.png)

* アプリ作成されると下図のような画面になります
* この２種類のAPIキー（アプリケーションキーとクライアントキー）はXcodeで作成するiOSアプリに[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)を紐付けるために使用します

![画像4](/readme-img/004.png)

* 動作確認後に会員情報が保存される場所も確認しておきましょう

![画像5](/readme-img/005.png)

### 2. [GitHub](https://github.com/natsumo/ObjcLoginApp.git)からサンプルプロジェクトのダウンロード

* この画面([GitHub](https://github.com/natsumo/ObjcLoginApp.git))の![画像10](/readme-img/010.PNG)ボタンをクリックし、さらに![画像11](/readme-img/011.PNG)ボタンをクリックしてサンプルプロジェクトをMacにダウンロードします

### 3. Xcodeでアプリを起動

* ダウンロードしたフォルダを開き、![画像09](/readme-img/009.png)をダブルクリックしてXcode開きます　![画像08](/readme-img/008.png)

![画像6](/readme-img/006.png)

### 4. APIキーの設定

* `AppDelegate.m`を編集します
* 先程[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)のダッシュボード上で確認したAPIキーを貼り付けます

![画像07](/readme-img/007.png)

* それぞれ`YOUR_NCMB_APPLICATION_KEY`と`YOUR_NCMB_CLIENT_KEY`の部分を書き換えます
 * このとき、ダブルクォーテーション（`"`）を消さないように注意してください！
* 書き換え終わったら`command + s`キーで保存をします

### 5. 動作確認
* Xcode画面で左上の実行ボタン（さんかくの再生マーク）をクリックします

![画像12](/readme-img/012.png)

* シミュレーターが起動したら、Login画面が表示されます
* 初回は__`SignUp`__ ボタンをクリックして、会員登録を行います

![画像14](/readme-img/014.png)

* `User Name`と`Password`を２つ入力して![画像13](/readme-img/013.png)ボタンをタップします
* 会員登録が成功するとログインされ、下記画面が表示されます
 * このときmBaaS上に会員情報が作成されます！
 * ログインに失敗した場合は画面にエラーコードが表示されます
 * 万が一エラーが発生した場合は、[こちら](http://mb.cloud.nifty.com/doc/current/rest/common/error.html)よりエラー内容を確認いただけます

![画像15](/readme-img/015.png)

* __`Logout`__ ボタンをタップするとログアウトし、元の画面に戻ります
* 登録された会員情報を使ってLogin画面からログインが可能です（操作は同様です）

-----

* 保存に成功したら、[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)のダッシュボードから「会員管理」を確認してみましょう！

![画像1](/readme-img/001.png)

## 解説
サンプルプロジェクトに実装済みの内容のご紹介

#### SDKのインポートと初期設定
* ニフティクラウドmobile backend の[ドキュメント（クイックスタート）](http://mb.cloud.nifty.com/doc/current/introduction/quickstart_ios.html)をご覧ください
 * このDEMOアプリは「CocoaPods」を利用する方法でSDKをインポートしています

#### ロジック
 * `Main.storyboard`でデザインを作成し、`LoginViewController.m`,`SignUpViewController.m`,`LogoutViewController.m`にロジックを書いています
 * ログイン、会員登録、ログアウト部分の処理は以下のように記述されます　※ただし、左記処理以外のコードは除いています

`LoginViewController.m`

```objc
/** ログイン **/
[NCMBUser logInWithUsernameInBackground:self.userNameTextField.text password:self.passwordTextField.text block:^(NCMBUser *user, NSError *error) {
    if (error) {
        // ログイン失敗時の処理
        
    
    }else{
        // ログイン成功時の処理
        
    }
}];
```

`SignUpViewController.m`

```objc
/** 会員登録 **/
//NCMBUserのインスタンスを作成
NCMBUser *user = [NCMBUser user];
//ユーザー名を設定
user.userName = self.userNameTextField.text;
//パスワードを設定
user.password = self.passwordTextField.text;

//会員の登録を行う
[user signUpInBackgroundWithBlock:^(NSError *error) {
    if (error) {
        // 新規登録失敗時の処理
        
    } else {
        // 新規登録成功時の処理
        
    }
}];
```

`LogoutViewController.m`

```objc
/** ログアウト **/
[NCMBUser logOut];
```

## 参考
* 同じ内容の【Swift】版もご用意しています
 * https://github.com/natsumo/SwiftPushApp
* ニフティクラウドmobile backend の[ドキュメント（会員管理）](http://mb.cloud.nifty.com/doc/current/user/basic_usage_ios.html)をSwift版に書き換えたドキュメントをご用意していますので、ご活用ください
 * [Swiftでログイン機能をつけてみよう！](http://qiita.com/natsumo/items/25c97182fab4db5629b1)
