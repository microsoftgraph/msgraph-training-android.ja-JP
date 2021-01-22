<!-- markdownlint-disable MD002 MD041 -->

この演習では、前の演習のアプリケーションを拡張して、Azure AD での認証をサポートします。 これは、Microsoft Graph を呼び出すのに必要な OAuth アクセス トークンを取得するために必要です。 これを行うには [、Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) をアプリケーションに統合します。

1. res フォルダーを **右クリックし、[新規]、** 次に [Android リソース ディレクトリ]**の順に選択します**。 

1. リソースの種類 **を変更し** `raw` **、[OK] を選択します**。

1. 新しい未加工フォルダーを **右クリックし、[** 新規]、次に [ **ファイル**] の順に **選択します**。

1. ファイルに名前を付 `msal_config.json` け **、[OK] を選択します**。

1. 次のコードをファイルの **msal_config.js追加** します。

    :::code language="json" source="../demo/GraphTutorial/msal_config.json.example":::

1. アプリ `YOUR_APP_ID_HERE` 登録のアプリ ID に置き換え、プロジェクトのパッケージ名 `com.example.graphtutorial` に置き換える。

    > [!IMPORTANT]
    > git などのソース管理を使っている場合は、アプリ ID が誤って漏洩しないように、ファイルをソース管理から除外する良い時期です `msal_config.json` 。

## <a name="implement-sign-in"></a>サインインの実装

このセクションでは、マニフェストを更新して、MSAL がブラウザーを使用してユーザーを認証し、リダイレクト URI をアプリによって処理されるとして登録し、認証ヘルパー クラスを作成し、サインインとサインアウトを行うアプリを更新します。

1. アプリ **/マニフェスト フォルダーを展開** し、アプリを **開** AndroidManifest.xml。 要素の上に次の要素を追加 `application` します。

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ```

    > [!NOTE]
    > MSAL ライブラリがユーザーを認証するには、これらのアクセス許可が必要です。

1. 要素内に次の要素を `application` 追加し、文字列をパッケージ名 `YOUR_PACKAGE_NAME_HERE` に置き換える。

    ```xml
    <!--Intent filter to capture authorization code response from the default browser on the
        device calling back to the app after interactive sign in -->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data
                android:scheme="msauth"
                android:host="YOUR_PACKAGE_NAME_HERE"
                android:path="/callback" />
        </intent-filter>
    </activity>
    ```

1. **app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。 種類を **インターフェイスに** 変更 **します**。 インターフェイスに名前を付 `IAuthenticationHelperCreatedListener` け **、[OK] を選択します**。

1. 新しいファイルを開き、その内容を次のファイルに置き換えてください。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/IAuthenticationHelperCreatedListener.java" id="ListenerSnippet":::

1. **app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。 クラスに名前を付 `AuthenticationHelper` け **、[OK] を選択します**。

1. 新しいファイルを開き、その内容を次のファイルに置き換えてください。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/AuthenticationHelper.java" id="AuthHelperSnippet":::

1. **MainActivity を開** き、次のステートメント `import` を追加します。

    ```java
    import android.util.Log;

    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IAuthenticationResult;
    import com.microsoft.identity.client.exception.MsalClientException;
    import com.microsoft.identity.client.exception.MsalException;
    import com.microsoft.identity.client.exception.MsalServiceException;
    import com.microsoft.identity.client.exception.MsalUiRequiredException;
    ```

1. 次のメンバー プロパティをクラスに追加 `MainActivity` します。

    ```java
    private AuthenticationHelper mAuthHelper = null;
    private boolean mAttemptInteractiveSignIn = false;
    ```

1. 次の関数を `onCreate` 関数の最後に追加します。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="InitialLoginSnippet":::

1. 次の関数をクラスに追加 `MainActivity` します。

    ```java
    // Silently sign in - used if there is already a
    // user account in the MSAL cache
    private void doSilentSignIn(boolean shouldAttemptInteractive) {
        mAttemptInteractiveSignIn = shouldAttemptInteractive;
        mAuthHelper.acquireTokenSilently(getAuthCallback());
    }

    // Prompt the user to sign in
    private void doInteractiveSignIn() {
        mAuthHelper.acquireTokenInteractively(this, getAuthCallback());
    }

    // Handles the authentication result
    public AuthenticationCallback getAuthCallback() {
        return new AuthenticationCallback() {

            @Override
            public void onSuccess(IAuthenticationResult authenticationResult) {
                // Log the token for debug purposes
                String accessToken = authenticationResult.getAccessToken();
                Log.d("AUTH", String.format("Access token: %s", accessToken));

                hideProgressBar();

                setSignedInState(true);
                openHomeFragment(mUserName);
            }

            @Override
            public void onError(MsalException exception) {
                // Check the type of exception and handle appropriately
                if (exception instanceof MsalUiRequiredException) {
                    Log.d("AUTH", "Interactive login required");
                    if (mAttemptInteractiveSignIn) {
                        doInteractiveSignIn();
                    }
                } else if (exception instanceof MsalClientException) {
                    if (exception.getErrorCode() == "no_current_account" ||
                        exception.getErrorCode() == "no_account_found") {
                        Log.d("AUTH", "No current account, interactive login required");
                        if (mAttemptInteractiveSignIn) {
                            doInteractiveSignIn();
                        }
                    } else {
                        // Exception inside MSAL, more info inside MsalError.java
                        Log.e("AUTH", "Client error authenticating", exception);
                    }
                } else if (exception instanceof MsalServiceException) {
                    // Exception when communicating with the auth server, likely config issue
                    Log.e("AUTH", "Service error authenticating", exception);
                }
                hideProgressBar();
            }

            @Override
            public void onCancel() {
                // User canceled the authentication
                Log.d("AUTH", "Authentication canceled");
                hideProgressBar();
            }
        };
    }
    ```

1. 既存の関数と関数 `signIn` を `signOut` 次に置き換える。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="SignInAndOutSnippet":::

    > [!NOTE]
    > メソッドはサイレント サインイン ( `signIn` 経由) を行います `doSilentSignIn` 。 このメソッドのコールバックは、サイレント モードが失敗した場合に対話型のサインインを実行します。 これにより、ユーザーがアプリを起動する度にプロンプトを表示する必要が回避されます。

1. 変更内容を保存し、アプリケーションを実行します。

1. [サインイン] メニュー **項目を** タップすると、ブラウザーが Azure AD開きます。 自分のアカウントでサインインします。

アプリが再開すると、Android Studio のデバッグ ログにアクセス トークンが出力されます。

![Android Studio の Logcat ウィンドウのスクリーンショット](./images/debugger-access-token.png)

## <a name="get-user-details"></a>ユーザーの詳細情報を取得する

このセクションでは、Microsoft Graph へのすべての呼び出しを保持するヘルパー クラスを作成し、この新しいクラスを使用してログイン ユーザーを取得するクラスを `MainActivity` 更新します。

1. **app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。 クラスに名前を付 `GraphHelper` け **、[OK] を選択します**。

1. 新しいファイルを開き、その内容を次のファイルに置き換えてください。

    ```java
    package com.example.graphtutorial;

    import com.microsoft.graph.authentication.IAuthenticationProvider;
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.http.IHttpRequest;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;

    // Singleton class - the app only needs a single instance
    // of the Graph client
    public class GraphHelper implements IAuthenticationProvider {
        private static GraphHelper INSTANCE = null;
        private IGraphServiceClient mClient = null;
        private String mAccessToken = null;

        private GraphHelper() {
            mClient = GraphServiceClient.builder()
                    .authenticationProvider(this).buildClient();
        }

        public static synchronized GraphHelper getInstance() {
            if (INSTANCE == null) {
                INSTANCE = new GraphHelper();
            }

            return INSTANCE;
        }

        // Part of the Graph IAuthenticationProvider interface
        // This method is called before sending the HTTP request
        @Override
        public void authenticateRequest(IHttpRequest request) {
            // Add the access token in the Authorization header
            request.addHeader("Authorization", "Bearer " + mAccessToken);
        }

        public void getUser(String accessToken, ICallback<User> callback) {
            mAccessToken = accessToken;

            // GET /me (logged in user)
            mClient.me().buildRequest()
                    .select("displayName,mail,mailboxSettings,userPrincipalName")
                    .get(callback);
        }
    }
    ```

    > [!NOTE]
    > このコードの動作を検討します。
    >
    > - 送信 HTTP 要求のヘッダーにアクセス トークンを挿入するインターフェイス `IAuthenticationProvider` `Authorization` を実装します。
    > - Graph エンドポイントから `getUser` ログインしているユーザーの情報を取得する関数を `/me` 公開します。

1. `import`MainActivity ファイルの一番上に次の **ステートメントを追加** します。

    ```java
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.core.ClientException;
    import com.microsoft.graph.models.extensions.User;
    ```

1. Graph 呼び出し用の関数 `MainActivity` を生成するには、次の `ICallback` 関数をクラスに追加します。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="GetUserCallbackSnippet":::

1. ユーザー名と電子メールを設定する次の行を削除します。

    ```java
    // For testing
    mUserName = "Megan Bowen";
    mUserEmail = "meganb@contoso.com";
    ```

1. 次の `onSuccess` オーバーライドを置き `AuthenticationCallback` 換える。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="OnSuccessSnippet":::

1. 変更内容を保存し、アプリケーションを実行します。 サインイン後、UI はユーザーの表示名と電子メール アドレスで更新されます。
