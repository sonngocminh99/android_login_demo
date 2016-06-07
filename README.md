# 概要

１）[ニフティクラウド mobile backend - mBaaS](http://mb.cloud.nifty.com/)での会員の認証方法は以下の4つがあります。

 * ユーザ名・パスワードでの認証
 * メールアドレス・パスワードでの認証
    * [ドキュメント](http://mb.cloud.nifty.com/doc/current/user/authorize_email_android.html)
 * SNSアカウントでの認証
   * [ドキュメント（Facebookアカウント）](http://mb.cloud.nifty.com/doc/current/sns/facebook_android.html)
   * [ドキュメント（Twitterアカウント）](http://mb.cloud.nifty.com/doc/current/sns/twitter_android.html)
   * [ドキュメント（Googleアカウント）](http://mb.cloud.nifty.com/doc/current/sns/google_android.html)
 * 匿名認証
   * [ドキュメント](http://mb.cloud.nifty.com/doc/current/user/authorize_anonymous_android.html)

２）今回はAndroidで、ユーザ名・パスワードでの認証方法について説明していきます。
イメージ的は以下のようになります。
![device.png](https://qiita-image-store.s3.amazonaws.com/0/126379/82ae570f-45ec-824c-e8f1-94c588b6034e.png)


# 準備

* Android Studio
* mBaaSの[アカウント作成](http://mb.cloud.nifty.com/signup.htm)

# 手順

* テンプレートプロジェクトをダウンロード
* SDKを追加（済み・最新SDKを利用したい場合、更新作業として行ってください）
* アプリ作成し、キーを設定
* 動作確認

# STEP 1. テンプレートプロジェクト

* プロジェクトの[Githubページ](https://github.com/ncmbadmin/android_login_demo)から「Download ZIP」をクリックします。
* プロジェクトを解凍します。
* AndroidStudioを開きます、解凍プロジェクトを開くことを選択します。
![OpenFileProject.png](https://qiita-image-store.s3.amazonaws.com/0/126379/ce219fcc-c51b-8d3b-7698-14970e2d62b7.png)

プロジェクトを選択し開きます。
![MainDesing.png](https://qiita-image-store.s3.amazonaws.com/0/126379/41091466-c5d5-10a6-86e3-656713734c7a.png)


# STEP 2. SDKを追加と設定 (済み)

SDKとはニフティクラウドmobile backendが提供している「データストア」「プッシュ通知」などの機能をAndroidからも簡単にコード書ける（数行ぐらい）ライブラリーのものです。

![002.png](https://qiita-image-store.s3.amazonaws.com/0/18698/75b7512c-7dec-9931-b8f6-66f6dd5a73af.png)

mBaaSでは、Android, iOS, Unity, JavaScript SDKを提供しています。
今回Android SDKの追加し方と設定を紹介します。
※ダウンロードしたプロジェクトには既に設定済みですが、最新ではない場合、ご自身で入れ替えてください！またご自身のプロジェクトにもSDKを追加したい場合も同じく実装必要です。

* SDKダウンロード
SDKはここ（SDK[リリースページ](https://github.com/NIFTYCloud-mbaas/ncmb_android/releases)）から取得してください.
  - NCMB.jarファイルがダウンロードします。
* SDKをインポート
  - app/libsフォルダにNCMB.jarをコピーします
* 設定追加
  - app/build.gradleファイルに以下を追加します

```
dependencies {
    compile 'com.google.code.gson:gson:2.3.1'
    compile files('libs/NCMB.jar')
}
```
  - androidManifestの設定

<application>タグの直前に以下のpermissionを追加します。

```
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```


# STEP 3. アプリキー設定

* 会員登録（無料）をし、登録ができたらログインをすると下図のように「アプリの新規作成」画面出るのでアプリを作成します。
![mBass1-2.png](https://qiita-image-store.s3.amazonaws.com/0/126379/3088b146-737b-1726-38ba-e16d6010d774.png)

* アプリ作成されると下図のような画面になります。
* この２種類のAPIキー（アプリケーションキーとクライアントキー）は先ほどインポートしたAndroidStudioで作成するAndroidアプリにニフティクラウドmobile backendの紐付けるため、あとで使います.
![mBass3-4.png](https://qiita-image-store.s3.amazonaws.com/0/126379/8531d19c-99ca-47e3-628c-dcd8baff6070.png)

この後動作確認でデータが保存される場所も確認しておきましょう。
![mBass5-6.png](https://qiita-image-store.s3.amazonaws.com/0/126379/fec223ae-7af3-8605-0436-4dd375c0e69f.png)

* AndroidStudioで取得APIキー(アプリケーションキー、クライントキー)を設定する。
![MainActivity.png](https://qiita-image-store.s3.amazonaws.com/0/126379/d0996a98-cf26-a367-c8bd-c93febc592ba.png)
![APIKey.png](https://qiita-image-store.s3.amazonaws.com/0/126379/48c45a2a-9536-d110-72eb-5fccf7454b95.png)

* AndroidStudioからビルドする。
    * 「プロジェクト場所」\app\build\outputs\apk\ ***.apk ファイルが生成される

# STEP 4. 確認

アプリにてボタンをタブし、新規登録、ログインする事が確認出来ます。
![AccountPattern.png](https://qiita-image-store.s3.amazonaws.com/0/126379/bc2a4349-defe-5d5d-59d4-136959ee269b.png)
![LoginPattern.png](https://qiita-image-store.s3.amazonaws.com/0/126379/c3bf4f83-b12c-3ebd-7af4-9350db6212bd.png)

mBaaS側も会員管理データが保存されたことを確認しています！
![mBassMember.png](https://qiita-image-store.s3.amazonaws.com/0/126379/399d5dbe-ceaa-1a7a-6bb4-306f6fcbad94.png)


# コード説明

* SDKおよび必要なライブラリーをインポートします

```java:MainActivity.java

import com.nifty.cloud.mb.core.DoneCallback;
import com.nifty.cloud.mb.core.NCMB;
import com.nifty.cloud.mb.core.NCMBException;
import com.nifty.cloud.mb.core.NCMBUser;
```

* SDKを初期化

MainActivityのOnCreateメソッドに実装、ここでAPIキーを渡します。

```java:MainActivity.java

 @Override
    protected void onCreate(Bundle savedInstanceState) {
       <省略>
        //**************** APIキーの設定とSDKの初期化 **********************
        NCMB.initialize(this, "YOUR_APPLICATION_KEY", "YOUR_CLIENT_KEY");
    }
```

１）会員の新規登録実装

* mBaaSのAndroid SDKが提供するNCMBUserクラスが会員管理を操作するためのクラス。データを保存するには、このクラスが提供するsignUpInBackgroundメソッドを利用し、登録、ログインします。
* 入力ユーザ名とパスワードの妥当性を確認し、設定したユーザ名(userName)とパスワード(password)で会員登録を行います。
* signUpInBackground()を実施することで、非同期に保存が行われます。非同期実施するため、DoneCallBack()を使って、成功・失敗処理を指定します。
 - 成功する場合、ログイン成功ページを表示します。
 - 失敗する場合、アラートでログイン失敗を表示します。

```java:SignupActivity.java

      public void signup() {
　　　　<省略>
 　　　　// TODO: Implement your own signup logic here.
        //NCMBUserのインスタンスを作成
        NCMBUser user = new NCMBUser();
        //ユーザ名を設定
        user.setUserName(name);
        //パスワードを設定
        user.setPassword(password);
        //設定したユーザ名とパスワードで会員登録を行う
        user.signUpInBackground(new DoneCallback() {
            @Override
            public void done(NCMBException e) {
                if (e != null) {
                    //会員登録時にエラーが発生した場合の処理
                    onSignupFailed();
                } else {
                    new android.os.Handler().postDelayed(
                            new Runnable() {
                                public void run() {
                                    // On complete call either onSignupSuccess or onSignupFailed
                                    // depending on success
                                    onSignupSuccess();
                                    // onSignupFailed();
                                    progressDialog.dismiss();
                                }
                            }, 3000);
                }
            }
        });
    }
```

２）既存会員のログイン実装

* mBaaSのAndroid SDKが提供するNCMBUserクラスが会員管理操作するためのクラス。このクラスが提供するloginInBackgroundメソッドを利用し、ログインします。
* 入力ユーザ名とパスワードの妥当性を確認し、ユーザ名とパスワードでログインを実行します。
* loginInBackground()を実施結果で、
 - 成功する場合、ログイン成功ページを表示します。
 - 失敗する場合、アラートでログイン失敗を表示します。

```java:LoginActivity.java

      public void login() {
　　　　<省略>
        // TODO: Implement your own authentication logic here.
        //ユーザ名とパスワードを指定してログインを実行
        try {
            NCMBUser.loginInBackground(name, password, new LoginCallback() {
                @Override
                public void done(NCMBUser user, NCMBException e) {
                    if (e != null) {
                        //エラー時の処理
                        onLoginFailed();
                    } else {
                        new android.os.Handler().postDelayed(
                                new Runnable() {
                                    public void run() {
                                        // On complete call either onLoginSuccess or onLoginFailed
                                        onLoginSuccess();
                                        // onLoginFailed();
                                        progressDialog.dismiss();
                                    }
                                }, 3000);
                    }
                }
            });
        } catch (NCMBException e) {
            e.printStackTrace();
        }

    }
```

# 参考

サンプルコードをカスタマイズすることで、様々な機能を実装できます！
データ保存・データ検索・会員管理・プッシュ通知などの機能を実装したい場合には、
以下のドキュメントもご参考ください。

* [ドキュメント](http://mb.cloud.nifty.com/doc/current/)
* [ドキュメント・データストア](http://mb.cloud.nifty.com/doc/current/datastore/basic_usage_android.html)
* [ドキュメント・会員管理](http://mb.cloud.nifty.com/doc/current/user/basic_usage_android.html)
* [ドキュメント・プッシュ通知](http://mb.cloud.nifty.com/doc/current/push/basic_usage_android.html)

# 最後に

データを保存するってサーバを立てたり、自分でサーバ運用とか、設計とか、アプリからサーバーとのやりとりも色々考慮しなければなりません。
最短方法というのは、このようにmBaaSサービスを使って、運用、設計など自分でやらなくて済む、開発も数行コード書けばいいという便利なものはいかがでしょうか？

# Contributing

*    Fork it!
*    Create your feature branch: git checkout -b my-new-feature
*    Commit your changes: git commit -am 'Add some feature'
*    Push to the branch: git push origin my-new-feature
*    Submit a pull request :D

# License

    MITライセンス
    NIFTY Cloud mobile backendのAndroid SDKのライセンス
