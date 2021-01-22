<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="37e0a-101">この演習では、前の演習のアプリケーションを拡張して、Azure AD での認証をサポートします。</span><span class="sxs-lookup"><span data-stu-id="37e0a-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="37e0a-102">これは、Microsoft Graph を呼び出すのに必要な OAuth アクセス トークンを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="37e0a-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="37e0a-103">これを行うには [、Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) をアプリケーションに統合します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-103">To do this, you will integrate the [Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) into the application.</span></span>

1. <span data-ttu-id="37e0a-104">res フォルダーを **右クリックし、[新規]、** 次に [Android リソース ディレクトリ]**の順に選択します**。 </span><span class="sxs-lookup"><span data-stu-id="37e0a-104">Right-click the **res** folder and select **New**, then **Android Resource Directory**.</span></span>

1. <span data-ttu-id="37e0a-105">リソースの種類 **を変更し** `raw` **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="37e0a-105">Change the **Resource type** to `raw` and select **OK**.</span></span>

1. <span data-ttu-id="37e0a-106">新しい未加工フォルダーを **右クリックし、[** 新規]、次に [ **ファイル**] の順に **選択します**。</span><span class="sxs-lookup"><span data-stu-id="37e0a-106">Right-click the new **raw** folder and select **New**, then **File**.</span></span>

1. <span data-ttu-id="37e0a-107">ファイルに名前を付 `msal_config.json` け **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="37e0a-107">Name the file `msal_config.json` and select **OK**.</span></span>

1. <span data-ttu-id="37e0a-108">次のコードをファイルの **msal_config.js追加** します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-108">Add the following to the **msal_config.json** file.</span></span>

    :::code language="json" source="../demo/GraphTutorial/msal_config.json.example":::

1. <span data-ttu-id="37e0a-109">アプリ `YOUR_APP_ID_HERE` 登録のアプリ ID に置き換え、プロジェクトのパッケージ名 `com.example.graphtutorial` に置き換える。</span><span class="sxs-lookup"><span data-stu-id="37e0a-109">Replace `YOUR_APP_ID_HERE` with the app ID from your app registration, and replace `com.example.graphtutorial` with your project's package name.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="37e0a-110">git などのソース管理を使っている場合は、アプリ ID が誤って漏洩しないように、ファイルをソース管理から除外する良い時期です `msal_config.json` 。</span><span class="sxs-lookup"><span data-stu-id="37e0a-110">If you're using source control such as git, now would be a good time to exclude the `msal_config.json` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="37e0a-111">サインインの実装</span><span class="sxs-lookup"><span data-stu-id="37e0a-111">Implement sign-in</span></span>

<span data-ttu-id="37e0a-112">このセクションでは、マニフェストを更新して、MSAL がブラウザーを使用してユーザーを認証し、リダイレクト URI をアプリによって処理されるとして登録し、認証ヘルパー クラスを作成し、サインインとサインアウトを行うアプリを更新します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-112">In this section you will update the manifest to allow MSAL to use a browser to authenticate the user, register your redirect URI as being handled by the app, create an authentication helper class, and update the app to sign in and sign out.</span></span>

1. <span data-ttu-id="37e0a-113">アプリ **/マニフェスト フォルダーを展開** し、アプリを **開** AndroidManifest.xml。</span><span class="sxs-lookup"><span data-stu-id="37e0a-113">Expand the **app/manifests** folder and open **AndroidManifest.xml**.</span></span> <span data-ttu-id="37e0a-114">要素の上に次の要素を追加 `application` します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-114">Add the following elements above the `application` element.</span></span>

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ```

    > [!NOTE]
    > <span data-ttu-id="37e0a-115">MSAL ライブラリがユーザーを認証するには、これらのアクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="37e0a-115">These permissions are required in order for the MSAL library to authenticate the user.</span></span>

1. <span data-ttu-id="37e0a-116">要素内に次の要素を `application` 追加し、文字列をパッケージ名 `YOUR_PACKAGE_NAME_HERE` に置き換える。</span><span class="sxs-lookup"><span data-stu-id="37e0a-116">Add the following element inside the `application` element, replacing the `YOUR_PACKAGE_NAME_HERE` string with your package name.</span></span>

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

1. <span data-ttu-id="37e0a-117">**app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。</span><span class="sxs-lookup"><span data-stu-id="37e0a-117">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span> <span data-ttu-id="37e0a-118">種類を **インターフェイスに** 変更 **します**。</span><span class="sxs-lookup"><span data-stu-id="37e0a-118">Change the **Kind** to **Interface**.</span></span> <span data-ttu-id="37e0a-119">インターフェイスに名前を付 `IAuthenticationHelperCreatedListener` け **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="37e0a-119">Name the interface `IAuthenticationHelperCreatedListener` and select **OK**.</span></span>

1. <span data-ttu-id="37e0a-120">新しいファイルを開き、その内容を次のファイルに置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="37e0a-120">Open the new file and replace its contents with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/IAuthenticationHelperCreatedListener.java" id="ListenerSnippet":::

1. <span data-ttu-id="37e0a-121">**app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。</span><span class="sxs-lookup"><span data-stu-id="37e0a-121">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span> <span data-ttu-id="37e0a-122">クラスに名前を付 `AuthenticationHelper` け **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="37e0a-122">Name the class `AuthenticationHelper` and select **OK**.</span></span>

1. <span data-ttu-id="37e0a-123">新しいファイルを開き、その内容を次のファイルに置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="37e0a-123">Open the new file and replace its contents with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/AuthenticationHelper.java" id="AuthHelperSnippet":::

1. <span data-ttu-id="37e0a-124">**MainActivity を開** き、次のステートメント `import` を追加します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-124">Open **MainActivity** and add the following `import` statements.</span></span>

    ```java
    import android.util.Log;

    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IAuthenticationResult;
    import com.microsoft.identity.client.exception.MsalClientException;
    import com.microsoft.identity.client.exception.MsalException;
    import com.microsoft.identity.client.exception.MsalServiceException;
    import com.microsoft.identity.client.exception.MsalUiRequiredException;
    ```

1. <span data-ttu-id="37e0a-125">次のメンバー プロパティをクラスに追加 `MainActivity` します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-125">Add the following member properties to the `MainActivity` class.</span></span>

    ```java
    private AuthenticationHelper mAuthHelper = null;
    private boolean mAttemptInteractiveSignIn = false;
    ```

1. <span data-ttu-id="37e0a-126">次の関数を `onCreate` 関数の最後に追加します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-126">Add the following code to the end of the `onCreate` function.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="InitialLoginSnippet":::

1. <span data-ttu-id="37e0a-127">次の関数をクラスに追加 `MainActivity` します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-127">Add the following functions to the `MainActivity` class.</span></span>

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

1. <span data-ttu-id="37e0a-128">既存の関数と関数 `signIn` を `signOut` 次に置き換える。</span><span class="sxs-lookup"><span data-stu-id="37e0a-128">Replace the existing `signIn` and `signOut` functions with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="SignInAndOutSnippet":::

    > [!NOTE]
    > <span data-ttu-id="37e0a-129">メソッドはサイレント サインイン ( `signIn` 経由) を行います `doSilentSignIn` 。</span><span class="sxs-lookup"><span data-stu-id="37e0a-129">Notice that the `signIn` method does a silent sign-in (via `doSilentSignIn`).</span></span> <span data-ttu-id="37e0a-130">このメソッドのコールバックは、サイレント モードが失敗した場合に対話型のサインインを実行します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-130">The callback for this method will do an interactive sign-in if the silent one fails.</span></span> <span data-ttu-id="37e0a-131">これにより、ユーザーがアプリを起動する度にプロンプトを表示する必要が回避されます。</span><span class="sxs-lookup"><span data-stu-id="37e0a-131">This avoids having to prompt the user every time they launch the app.</span></span>

1. <span data-ttu-id="37e0a-132">変更内容を保存し、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-132">Save your changes and run the app.</span></span>

1. <span data-ttu-id="37e0a-133">[サインイン] メニュー **項目を** タップすると、ブラウザーが Azure AD開きます。</span><span class="sxs-lookup"><span data-stu-id="37e0a-133">When you tap the **Sign in** menu item, a browser opens to the Azure AD login page.</span></span> <span data-ttu-id="37e0a-134">自分のアカウントでサインインします。</span><span class="sxs-lookup"><span data-stu-id="37e0a-134">Sign in with your account.</span></span>

<span data-ttu-id="37e0a-135">アプリが再開すると、Android Studio のデバッグ ログにアクセス トークンが出力されます。</span><span class="sxs-lookup"><span data-stu-id="37e0a-135">Once the app resumes, you should see an access token printed in the debug log in Android Studio.</span></span>

![Android Studio の Logcat ウィンドウのスクリーンショット](./images/debugger-access-token.png)

## <a name="get-user-details"></a><span data-ttu-id="37e0a-137">ユーザーの詳細情報を取得する</span><span class="sxs-lookup"><span data-stu-id="37e0a-137">Get user details</span></span>

<span data-ttu-id="37e0a-138">このセクションでは、Microsoft Graph へのすべての呼び出しを保持するヘルパー クラスを作成し、この新しいクラスを使用してログイン ユーザーを取得するクラスを `MainActivity` 更新します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-138">In this section you will create a helper class to hold all of the calls to Microsoft Graph and update the `MainActivity` class to use this new class to get the logged-in user.</span></span>

1. <span data-ttu-id="37e0a-139">**app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。</span><span class="sxs-lookup"><span data-stu-id="37e0a-139">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span> <span data-ttu-id="37e0a-140">クラスに名前を付 `GraphHelper` け **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="37e0a-140">Name the class `GraphHelper` and select **OK**.</span></span>

1. <span data-ttu-id="37e0a-141">新しいファイルを開き、その内容を次のファイルに置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="37e0a-141">Open the new file and replace its contents with the following.</span></span>

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
    > <span data-ttu-id="37e0a-142">このコードの動作を検討します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-142">Consider what this code does.</span></span>
    >
    > - <span data-ttu-id="37e0a-143">送信 HTTP 要求のヘッダーにアクセス トークンを挿入するインターフェイス `IAuthenticationProvider` `Authorization` を実装します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-143">It implements the `IAuthenticationProvider` interface to insert the access token in the `Authorization` header on outgoing HTTP requests.</span></span>
    > - <span data-ttu-id="37e0a-144">Graph エンドポイントから `getUser` ログインしているユーザーの情報を取得する関数を `/me` 公開します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-144">It exposes a `getUser` function to get the logged-in user's information from the `/me` Graph endpoint.</span></span>

1. <span data-ttu-id="37e0a-145">`import`MainActivity ファイルの一番上に次の **ステートメントを追加** します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-145">Add the following `import` statements to the top of the **MainActivity** file.</span></span>

    ```java
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.core.ClientException;
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="37e0a-146">Graph 呼び出し用の関数 `MainActivity` を生成するには、次の `ICallback` 関数をクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-146">Add the following function to the `MainActivity` class to generate an `ICallback` for the Graph call.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="GetUserCallbackSnippet":::

1. <span data-ttu-id="37e0a-147">ユーザー名と電子メールを設定する次の行を削除します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-147">Remove the following lines that set the user name and email:</span></span>

    ```java
    // For testing
    mUserName = "Megan Bowen";
    mUserEmail = "meganb@contoso.com";
    ```

1. <span data-ttu-id="37e0a-148">次の `onSuccess` オーバーライドを置き `AuthenticationCallback` 換える。</span><span class="sxs-lookup"><span data-stu-id="37e0a-148">Replace the `onSuccess` override in the `AuthenticationCallback` with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="OnSuccessSnippet":::

1. <span data-ttu-id="37e0a-149">変更内容を保存し、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="37e0a-149">Save your changes and run the app.</span></span> <span data-ttu-id="37e0a-150">サインイン後、UI はユーザーの表示名と電子メール アドレスで更新されます。</span><span class="sxs-lookup"><span data-stu-id="37e0a-150">After sign-in the UI is updated with the user's display name and email address.</span></span>
