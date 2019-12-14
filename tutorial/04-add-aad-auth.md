<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="51afe-101">この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。</span><span class="sxs-lookup"><span data-stu-id="51afe-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="51afe-102">これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="51afe-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="51afe-103">これを行うには、 [Android 用 Microsoft 認証ライブラリ (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-android)をアプリケーションに統合します。</span><span class="sxs-lookup"><span data-stu-id="51afe-103">To do this, you will integrate the [Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) into the application.</span></span>

1. <span data-ttu-id="51afe-104">**Res**フォルダーを右クリックし、[**新規作成**]、[ **Android リソースディレクトリ**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="51afe-104">Right-click the **res** folder and select **New**, then **Android Resource Directory**.</span></span>

1. <span data-ttu-id="51afe-105">リソースの**種類**をに`raw`変更し、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="51afe-105">Change the **Resource type** to `raw` and select **OK**.</span></span>

1. <span data-ttu-id="51afe-106">新しい**raw**フォルダーを右クリックし、[**新規**]、[**ファイル**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="51afe-106">Right-click the new **raw** folder and select **New**, then **File**.</span></span>

1. <span data-ttu-id="51afe-107">ファイル`msal_config.json`の名前を指定して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="51afe-107">Name the file `msal_config.json` and select **OK**.</span></span>

1. <span data-ttu-id="51afe-108">次のものを**msal_config json**ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="51afe-108">Add the following to the **msal_config.json** file.</span></span>

    ```json
    {
      "client_id" : "YOUR_APP_ID_HERE",
      "redirect_uri" : "msauth://YOUR_PACKAGE_NAME_HERE/callback",
      "broker_redirect_uri_registered": false,
      "account_mode": "SINGLE",
      "authorities" : [
        {
          "type": "AAD",
          "audience": {
            "type": "AzureADandPersonalMicrosoftAccount"
          },
          "default": true
        }
      ]
    }
    ```

    <span data-ttu-id="51afe-109">を`YOUR_APP_ID_HERE`アプリ登録のアプリ ID で置き換えて、プロジェクトの`YOUR_PACKAGE_NAME_HERE`パッケージ名に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="51afe-109">Replace `YOUR_APP_ID_HERE` with the app ID from your app registration, and replace `YOUR_PACKAGE_NAME_HERE` with your project's package name.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="51afe-110">Git などのソース管理を使用している場合は、この時点で、ソース管理`msal_config.json`からファイルを除外して、アプリ ID が誤ってリークしないようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="51afe-110">If you're using source control such as git, now would be a good time to exclude the `msal_config.json` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="51afe-111">サインインの実装</span><span class="sxs-lookup"><span data-stu-id="51afe-111">Implement sign-in</span></span>

<span data-ttu-id="51afe-112">このセクションでは、マニフェストを更新して、MSAL がブラウザーを使用してユーザーを認証できるようにし、リダイレクト URI をアプリによって処理されるものとして登録し、認証ヘルパークラスを作成し、サインインしてサインアウトするためにアプリを更新します。</span><span class="sxs-lookup"><span data-stu-id="51afe-112">In this section you will update the manifest to allow MSAL to use a browser to authenticate the user, register your redirect URI as being handled by the app, create an authentication helper class, and update the app to sign in and sign out.</span></span>

1. <span data-ttu-id="51afe-113">[ **App/manifest** ] フォルダーを展開し、 **Androidmanifest**を開きます。</span><span class="sxs-lookup"><span data-stu-id="51afe-113">Expand the **app/manifests** folder and open **AndroidManifest.xml**.</span></span> <span data-ttu-id="51afe-114">要素の`application`上に次の要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="51afe-114">Add the following elements above the `application` element.</span></span>

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ```

    > [!NOTE]
    > <span data-ttu-id="51afe-115">これらのアクセス許可は、MSAL ライブラリでユーザーを認証するために必要です。</span><span class="sxs-lookup"><span data-stu-id="51afe-115">These permissions are required in order for the MSAL library to authenticate the user.</span></span>

1. <span data-ttu-id="51afe-116">`application`要素内に次の要素を追加し、 `YOUR_PACKAGE_NAME_HERE`文字列をパッケージ名に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="51afe-116">Add the following element inside the `application` element, replacing the `YOUR_PACKAGE_NAME_HERE` string with your package name.</span></span>

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

1. <span data-ttu-id="51afe-117">[ **App/java/com/com. 例**] のチュートリアルフォルダーを右クリックし、[**新規**]、[ **java クラス**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="51afe-117">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span> <span data-ttu-id="51afe-118">クラス`AuthenticationHelper`の名前を指定して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="51afe-118">Name the class `AuthenticationHelper` and select **OK**.</span></span>

1. <span data-ttu-id="51afe-119">新しいファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="51afe-119">Open the new file and replace its contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.app.Activity;
    import android.content.Context;
    import android.util.Log;
    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IPublicClientApplication;
    import com.microsoft.identity.client.ISingleAccountPublicClientApplication;
    import com.microsoft.identity.client.PublicClientApplication;
    import com.microsoft.identity.client.exception.MsalException;

    // Singleton class - the app only needs a single instance
    // of PublicClientApplication
    public class AuthenticationHelper {
        private static AuthenticationHelper INSTANCE = null;
        private ISingleAccountPublicClientApplication mPCA = null;
        private String[] mScopes = { "User.Read", "Calendars.Read" };

        private AuthenticationHelper(Context ctx) {
            PublicClientApplication.createSingleAccountPublicClientApplication(ctx, R.raw.msal_config,
                new IPublicClientApplication.ISingleAccountApplicationCreatedListener() {
                    @Override
                    public void onCreated(ISingleAccountPublicClientApplication application) {
                        mPCA = application;
                    }

                    @Override
                    public void onError(MsalException exception) {
                        Log.e("AUTHHELPER", "Error creating MSAL application", exception);
                    }
                });
        }

        public static synchronized AuthenticationHelper getInstance(Context ctx) {
            if (INSTANCE == null) {
                INSTANCE = new AuthenticationHelper(ctx);
            }

            return INSTANCE;
        }

        // Version called from fragments. Does not create an
        // instance if one doesn't exist
        public static synchronized AuthenticationHelper getInstance() {
            if (INSTANCE == null) {
                throw new IllegalStateException(
                    "AuthenticationHelper has not been initialized from MainActivity");
            }

            return INSTANCE;
        }

        public void acquireTokenInteractively(Activity activity, AuthenticationCallback callback) {
            mPCA.signIn(activity, null, mScopes, callback);
        }

        public void acquireTokenSilently(AuthenticationCallback callback) {
            // Get the authority from MSAL config
            String authority = mPCA.getConfiguration().getDefaultAuthority().getAuthorityURL().toString();
            mPCA.acquireTokenSilentAsync(mScopes, authority, callback);
        }

        public void signOut() {
            mPCA.signOut(new ISingleAccountPublicClientApplication.SignOutCallback() {
                @Override
                public void onSignOut() {
                    Log.d("AUTHHELPER", "Signed out");
                }

                @Override
                public void onError(@NonNull MsalException exception) {
                    Log.d("AUTHHELPER", "MSAL error signing out", exception);
                }
            });
        }
    }
    ```

1. <span data-ttu-id="51afe-120">**Mainactivity**を開き、次`import`のステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="51afe-120">Open **MainActivity** and add the following `import` statements.</span></span>

    ```java
    import android.util.Log;

    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IAuthenticationResult;
    import com.microsoft.identity.client.exception.MsalClientException;
    import com.microsoft.identity.client.exception.MsalException;
    import com.microsoft.identity.client.exception.MsalServiceException;
    import com.microsoft.identity.client.exception.MsalUiRequiredException;
    ```

1. <span data-ttu-id="51afe-121">次のメンバープロパティを`MainActivity`クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="51afe-121">Add the following member property to the `MainActivity` class.</span></span>

    ```java
    private AuthenticationHelper mAuthHelper = null;
    ```

1. <span data-ttu-id="51afe-122">次のものを`onCreate`関数の末尾に追加します。</span><span class="sxs-lookup"><span data-stu-id="51afe-122">Add the following to the end of the `onCreate` function.</span></span>

    ```java
    // Get the authentication helper
    mAuthHelper = AuthenticationHelper.getInstance(getApplicationContext());
    ```

1. <span data-ttu-id="51afe-123">次の関数を`MainActivity`クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="51afe-123">Add the following functions to the `MainActivity` class.</span></span>

    ```java
    // Silently sign in - used if there is already a
    // user account in the MSAL cache
    private void doSilentSignIn() {
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
                    doInteractiveSignIn();

                } else if (exception instanceof MsalClientException) {
                    if (exception.getErrorCode() == "no_current_account") {
                        Log.d("AUTH", "No current account, interactive login required");
                        doInteractiveSignIn();
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

1. <span data-ttu-id="51afe-124">既存`signIn`のと`signOut`関数を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="51afe-124">Replace the existing `signIn` and `signOut` functions with the following.</span></span>

    ```java
    private void signIn() {
        showProgressBar();
        // Attempt silent sign in first
        // if this fails, the callback will handle doing
        // interactive sign in
        doSilentSignIn();
    }

    private void signOut() {
        mAuthHelper.signOut();

        setSignedInState(false);
        openHomeFragment(mUserName);
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="51afe-125">`signIn`メソッドがサイレントサインイン (via `doSilentSignIn`) を実行することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="51afe-125">Notice that the `signIn` method does a silent sign-in (via `doSilentSignIn`).</span></span> <span data-ttu-id="51afe-126">このメソッドのコールバックは、無音が失敗した場合に対話型サインインを行います。</span><span class="sxs-lookup"><span data-stu-id="51afe-126">The callback for this method will do an interactive sign-in if the silent one fails.</span></span> <span data-ttu-id="51afe-127">これにより、アプリを起動するたびにユーザーにメッセージを表示する必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="51afe-127">This avoids having to prompt the user every time they launch the app.</span></span>

1. <span data-ttu-id="51afe-128">変更内容を保存し、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="51afe-128">Save your changes and run the app.</span></span>

1. <span data-ttu-id="51afe-129">[**サインイン**] メニュー項目をタップすると、ブラウザーが Azure AD ログインページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="51afe-129">When you tap the **Sign in** menu item, a browser opens to the Azure AD login page.</span></span> <span data-ttu-id="51afe-130">自分のアカウントでサインインします。</span><span class="sxs-lookup"><span data-stu-id="51afe-130">Sign in with your account.</span></span>

<span data-ttu-id="51afe-131">アプリが再開されると、Android Studio のデバッグログに、アクセストークンが出力されていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="51afe-131">Once the app resumes, you should see an access token printed in the debug log in Android Studio.</span></span>

![Android Studio の Logcat ウィンドウのスクリーンショット](./images/debugger-access-token.png)

## <a name="get-user-details"></a><span data-ttu-id="51afe-133">ユーザーの詳細を取得する</span><span class="sxs-lookup"><span data-stu-id="51afe-133">Get user details</span></span>

<span data-ttu-id="51afe-134">このセクションでは、Microsoft Graph へのすべての呼び出しを保持するヘルパークラスを作成し、 `MainActivity`この新しいクラスを使用してログインしたユーザーを取得するようにクラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="51afe-134">In this section you will create a helper class to hold all of the calls to Microsoft Graph and update the `MainActivity` class to use this new class to get the logged-in user.</span></span>

1. <span data-ttu-id="51afe-135">[ **App/java/com/com. 例**] のチュートリアルフォルダーを右クリックし、[**新規**]、[ **java クラス**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="51afe-135">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="51afe-136">クラス`GraphHelper`の名前を指定して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="51afe-136">Name the class `GraphHelper` and select **OK**.</span></span>

1. <span data-ttu-id="51afe-137">新しいファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="51afe-137">Open the new file and replace its contents with the following.</span></span>

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
            mClient.me().buildRequest().get(callback);
        }
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="51afe-138">このコードの内容を検討してください。</span><span class="sxs-lookup"><span data-stu-id="51afe-138">Consider what this code does.</span></span>
    >
    > - <span data-ttu-id="51afe-139">インターフェイスを`IAuthenticationProvider`実装して、送信 HTTP 要求の`Authorization`ヘッダーにアクセストークンを挿入します。</span><span class="sxs-lookup"><span data-stu-id="51afe-139">It implements the `IAuthenticationProvider` interface to insert the access token in the `Authorization` header on outgoing HTTP requests.</span></span>
    > - <span data-ttu-id="51afe-140">Graph エンドポイント`getUser`からログインユーザーの情報を取得する関数が公開されています。 `/me`</span><span class="sxs-lookup"><span data-stu-id="51afe-140">It exposes a `getUser` function to get the logged-in user's information from the `/me` Graph endpoint.</span></span>

1. <span data-ttu-id="51afe-141">次`import`のステートメントを**mainactivity**ファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="51afe-141">Add the following `import` statements to the top of the **MainActivity** file.</span></span>

    ```java
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.core.ClientException;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="51afe-142">次の関数を`MainActivity`クラスに追加して、 `ICallback` Graph 呼び出しのを生成します。</span><span class="sxs-lookup"><span data-stu-id="51afe-142">Add the following function to the `MainActivity` class to generate an `ICallback` for the Graph call.</span></span>

    ```java
    private ICallback<User> getUserCallback() {
        return new ICallback<User>() {
            @Override
            public void success(User user) {
                Log.d("AUTH", "User: " + user.displayName);

                mUserName = user.displayName;
                mUserEmail = user.mail == null ? user.userPrincipalName : user.mail;

                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        hideProgressBar();

                        setSignedInState(true);
                        openHomeFragment(mUserName);
                    }
                });

            }

            @Override
            public void failure(ClientException ex) {
                Log.e("AUTH", "Error getting /me", ex);
                mUserName = "ERROR";
                mUserEmail = "ERROR";

                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        hideProgressBar();

                        setSignedInState(true);
                        openHomeFragment(mUserName);
                    }
                });
            }
        };
    }
    ```

1. <span data-ttu-id="51afe-143">ユーザー名と電子メールを設定する次の行を削除します。</span><span class="sxs-lookup"><span data-stu-id="51afe-143">Remove the following lines that set the user name and email:</span></span>

    ```java
    // For testing
    mUserName = "Megan Bowen";
    mUserEmail = "meganb@contoso.com";
    ```

1. <span data-ttu-id="51afe-144">の`onSuccess` `AuthenticationCallback`上書きを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="51afe-144">Replace the `onSuccess` override in the `AuthenticationCallback` with the following.</span></span>

    ```java
    @Override
    public void onSuccess(IAuthenticationResult authenticationResult) {
        // Log the token for debug purposes
        String accessToken = authenticationResult.getAccessToken();
        Log.d("AUTH", String.format("Access token: %s", accessToken));

        // Get Graph client and get user
        GraphHelper graphHelper = GraphHelper.getInstance();
        graphHelper.getUser(accessToken, getUserCallback());
    }
    ```

<span data-ttu-id="51afe-145">変更を保存してすぐにアプリを実行すると、サインイン後にユーザーの表示名と電子メールアドレスで UI が更新されます。</span><span class="sxs-lookup"><span data-stu-id="51afe-145">If you save your changes and run the app now, after sign-in the UI is updated with the user's display name and email address.</span></span>
