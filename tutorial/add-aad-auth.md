<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="23231-101">この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。</span><span class="sxs-lookup"><span data-stu-id="23231-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="23231-102">これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="23231-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="23231-103">この手順では、 [Android 用 Microsoft 認証ライブラリ (msal)](https://github.com/AzureAD/microsoft-authentication-library-for-android)をアプリケーションに統合します。</span><span class="sxs-lookup"><span data-stu-id="23231-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) into the application.</span></span>

<span data-ttu-id="23231-104">**app/res/values**フォルダーを右クリックし、[**新規作成**]、[値] [**リソースファイル**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="23231-104">Right-click the **app/res/values** folder and choose **New**, then **Values resource file**.</span></span> <span data-ttu-id="23231-105">ファイル`oauth_strings`に名前を指定して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="23231-105">Name the file `oauth_strings` and choose **OK**.</span></span> <span data-ttu-id="23231-106">`resources`要素に次の値を追加します。</span><span class="sxs-lookup"><span data-stu-id="23231-106">Add the following values to the `resources` element.</span></span>

```xml
<string name="oauth_app_id">YOUR_APP_ID_HERE</string>
<string name="oauth_redirect_uri">msalYOUR_APP_ID_HERE</string>
<string-array name="oauth_scopes">
    <item>User.Read</item>
    <item>Calendars.Read</item>
</string-array>
```

> [!IMPORTANT]
> <span data-ttu-id="23231-107">git などのソース管理を使用している場合は、この時点で、ソース管理`oauth_strings.xml`からファイルを除外して、アプリ ID が誤ってリークしないようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="23231-107">If you're using source control such as git, now would be a good time to exclude the `oauth_strings.xml` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="23231-108">サインインの実装</span><span class="sxs-lookup"><span data-stu-id="23231-108">Implement sign-in</span></span>

<span data-ttu-id="23231-109">[ **app/manifest** ] フォルダーを展開し、 **androidmanifest**を開きます。</span><span class="sxs-lookup"><span data-stu-id="23231-109">Expand the **app/manifests** folder and open **AndroidManifest.xml**.</span></span> <span data-ttu-id="23231-110">要素の`application`上に次の要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="23231-110">Add the following elements above the `application` element.</span></span>

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

<span data-ttu-id="23231-111">これらのアクセス許可は、msal ライブラリでユーザーを認証するために必要です。</span><span class="sxs-lookup"><span data-stu-id="23231-111">These permissions are required in order for the MSAL library to authenticate the user.</span></span>

<span data-ttu-id="23231-112">次に、要素内に`application`次の要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="23231-112">Now add the following element inside the `application` element.</span></span>

```xml
<activity android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />

        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <data
            android:host="auth"
            android:scheme="@string/oauth_redirect_uri" />
    </intent-filter>
</activity>
```

<span data-ttu-id="23231-113">これにより、msal はブラウザーを使用してユーザーを認証し、リダイレクト URI をアプリによって処理されるものとして登録することができます。</span><span class="sxs-lookup"><span data-stu-id="23231-113">This allows MSAL to use a browser to authenticate the user, and registers your redirect URI as being handled by the app.</span></span>

### <a name="implement-an-authentication-helper"></a><span data-ttu-id="23231-114">認証ヘルパーを実装する</span><span class="sxs-lookup"><span data-stu-id="23231-114">Implement an authentication helper</span></span>

<span data-ttu-id="23231-115">[ **app/java/com/com. 例**] のチュートリアルフォルダーを右クリックし、[**新規**]、[ **java クラス**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="23231-115">Right-click the **app/java/com.example.graphtutorial** folder and choose **New**, then **Java Class**.</span></span> <span data-ttu-id="23231-116">クラス`AuthenticationHelper`の名前を指定し、 **[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="23231-116">Name the class `AuthenticationHelper` and choose **OK**.</span></span> <span data-ttu-id="23231-117">新しいファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="23231-117">Open the new file and replace its contents with the following.</span></span>

```java
package com.example.graphtutorial;

import android.app.Activity;
import android.content.Context;
import android.content.Intent;

import com.microsoft.identity.client.AuthenticationCallback;
import com.microsoft.identity.client.IAccount;
import com.microsoft.identity.client.PublicClientApplication;

// Singleton class - the app only needs a single instance
// of PublicClientApplication
public class AuthenticationHelper {
    private static AuthenticationHelper INSTANCE = null;
    private PublicClientApplication mPCA = null;
    private String[] mScopes;

    private AuthenticationHelper(Context ctx) {
        String appId = ctx.getResources().getString(R.string.oauth_app_id);
        mScopes = ctx.getResources().getStringArray(R.array.oauth_scopes);
        mPCA = new PublicClientApplication(ctx, appId);
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

    public boolean hasAccount() {
        return !mPCA.getAccounts().isEmpty();
    }

    public void handleRedirect(int requestCode, int resultCode, Intent data) {
        mPCA.handleInteractiveRequestRedirect(requestCode, resultCode, data);
    }

    public void acquireTokenInteractively(Activity activity, AuthenticationCallback callback) {
        mPCA.acquireToken(activity, mScopes, callback);
    }

    public void acquireTokenSilently(AuthenticationCallback callback) {
        mPCA.acquireTokenSilentAsync(mScopes, mPCA.getAccounts().get(0), callback);
    }

    public void signOut() {
        for (IAccount account : mPCA.getAccounts()) {
            mPCA.removeAccount(account);
        }
    }
}
```

<span data-ttu-id="23231-118">これで`MainActivity` 、この新しいクラスを使用するように更新されました。</span><span class="sxs-lookup"><span data-stu-id="23231-118">Now update `MainActivity` to use this new class.</span></span> <span data-ttu-id="23231-119">**mainactivity**を開き、次`import`のステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="23231-119">Open **MainActivity** and add the following `import` statements.</span></span>

```java
import android.content.Intent;
import android.support.annotation.Nullable;
import android.util.Log;

import com.microsoft.identity.client.AuthenticationCallback;
import com.microsoft.identity.client.AuthenticationResult;
import com.microsoft.identity.client.exception.MsalClientException;
import com.microsoft.identity.client.exception.MsalException;
import com.microsoft.identity.client.exception.MsalServiceException;
import com.microsoft.identity.client.exception.MsalUiRequiredException;
```

<span data-ttu-id="23231-120">次に、次のメンバープロパティを`MainActivity`クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="23231-120">Next, add the following member property to the `MainActivity` class.</span></span>

```java
private AuthenticationHelper mAuthHelper = null;
```

<span data-ttu-id="23231-121">設定`onCreate` `mAuthHelper`を更新します。</span><span class="sxs-lookup"><span data-stu-id="23231-121">Update `onCreate` to set `mAuthHelper`.</span></span> <span data-ttu-id="23231-122">次のものを`onCreate`関数の末尾に追加します。</span><span class="sxs-lookup"><span data-stu-id="23231-122">Add the following to the end of the `onCreate` function.</span></span>

```java
// Get the authentication helper
mAuthHelper = AuthenticationHelper.getInstance(getApplicationContext());
```

<span data-ttu-id="23231-123">認証応答を処理`onActivityResult`するためのオーバーライドを追加します。</span><span class="sxs-lookup"><span data-stu-id="23231-123">Add an override for `onActivityResult` to handle authentication responses.</span></span>

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    mAuthHelper.handleRedirect(requestCode, resultCode, data);
}
```

<span data-ttu-id="23231-124">ここで、次の関数を`MainActivity`クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="23231-124">Now, add the following functions to the `MainActivity` class.</span></span>

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
        public void onSuccess(AuthenticationResult authenticationResult) {
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
                // Exception inside MSAL, more info inside MsalError.java
                Log.e("AUTH", "Client error authenticating", exception);
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

<span data-ttu-id="23231-125">最後に、既存`signIn`のと`signOut`関数を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="23231-125">Finally, replace the existing `signIn` and `signOut` functions with the following.</span></span>

```java
private void signIn() {
    showProgressBar();
    if (mAuthHelper.hasAccount()) {
        doSilentSignIn();
    } else {
        doInteractiveSignIn();
    }
}

private void signOut() {
    mAuthHelper.signOut();

    setSignedInState(false);
    openHomeFragment(mUserName);
}
```

<span data-ttu-id="23231-126">このメソッドは`signIn` 、最初に msal キャッシュに既にユーザーアカウントがあるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="23231-126">Notice that the `signIn` method first checks if there is a user account already in the MSAL cache.</span></span> <span data-ttu-id="23231-127">存在する場合は、トークンを自動的に更新しようとして、アプリを起動するたびにユーザーに確認を求める必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="23231-127">If there is, it attempts to refresh its tokens silently, avoiding having to prompt the user every time they launch the app.</span></span>

<span data-ttu-id="23231-128">変更内容を保存し、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="23231-128">Save your changes and run the app.</span></span> <span data-ttu-id="23231-129">[**サインイン**] メニュー項目をタップすると、ブラウザーが Azure AD ログインページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="23231-129">When you tap the **Sign in** menu item, a browser opens to the Azure AD login page.</span></span> <span data-ttu-id="23231-130">自分のアカウントでサインインします。</span><span class="sxs-lookup"><span data-stu-id="23231-130">Sign in with your account.</span></span> <span data-ttu-id="23231-131">アプリが再開されると、Android Studio のデバッグログに、アクセストークンが出力されていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="23231-131">Once the app resumes, you should see an access token printed in the debug log in Android Studio.</span></span>

![Android Studio の logcat ウィンドウのスクリーンショット](./images/debugger-access-token.png)

## <a name="get-user-details"></a><span data-ttu-id="23231-133">ユーザーの詳細を取得する</span><span class="sxs-lookup"><span data-stu-id="23231-133">Get user details</span></span>

<span data-ttu-id="23231-134">最初に、Microsoft Graph へのすべての呼び出しを保持するヘルパークラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="23231-134">Start by creating a helper class to hold all of the calls to Microsoft Graph.</span></span> <span data-ttu-id="23231-135">[ **app/java/com/com. 例**] のチュートリアルフォルダーを右クリックし、[**新規**]、[ **java クラス**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="23231-135">Right-click the **app/java/com.example.graphtutorial** folder and choose **New**, then **Java Class**.</span></span> <span data-ttu-id="23231-136">クラス`GraphHelper`の名前を指定し、 **[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="23231-136">Name the class `GraphHelper` and choose **OK**.</span></span> <span data-ttu-id="23231-137">新しいファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="23231-137">Open the new file and replace its contents with the following.</span></span>

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

<span data-ttu-id="23231-138">このコードで行われていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="23231-138">Note what this code does.</span></span>

- <span data-ttu-id="23231-139">インターフェイスを`IAuthenticationProvider`実装して、送信 HTTP 要求の`Authorization`ヘッダーにアクセストークンを挿入します。</span><span class="sxs-lookup"><span data-stu-id="23231-139">It implements the `IAuthenticationProvider` interface to insert the access token in the `Authorization` header on outgoing HTTP requests.</span></span>
- <span data-ttu-id="23231-140">Graph エンドポイント`getUser`からログインユーザーの情報を取得する関数が公開されています。 `/me`</span><span class="sxs-lookup"><span data-stu-id="23231-140">It exposes a `getUser` function to get the logged-in user's information from the `/me` Graph endpoint.</span></span>

<span data-ttu-id="23231-141">この新しいクラス`MainActivity`を使用してログインユーザーを取得するようにクラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="23231-141">Now update the `MainActivity` class to use this new class to get the logged-in user.</span></span> <span data-ttu-id="23231-142">次`import`のステートメントを**mainactivity**ファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="23231-142">Add the following `import` statements to the top of the **MainActivity** file.</span></span>

```java
import com.microsoft.graph.concurrency.ICallback;
import com.microsoft.graph.core.ClientException;
import com.microsoft.graph.models.extensions.IGraphServiceClient;
import com.microsoft.graph.models.extensions.User;
```

<span data-ttu-id="23231-143">次に、次の関数を`MainActivity`クラスに追加して`ICallback` 、Graph 呼び出しのを生成します。</span><span class="sxs-lookup"><span data-stu-id="23231-143">Next, add the following function to the `MainActivity` class to generate an `ICallback` for the Graph call.</span></span>

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

<span data-ttu-id="23231-144">ユーザー名と電子メールを設定する次の行を削除します。</span><span class="sxs-lookup"><span data-stu-id="23231-144">Remove the following lines that set the user name and email:</span></span>

```java
// For testing
mUserName = "Megan Bowen";
mUserEmail = "meganb@contoso.com";
```

<span data-ttu-id="23231-145">最後に、の`onSuccess` `AuthenticationCallback`上書きを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="23231-145">Finally, replace the `onSuccess` override in the `AuthenticationCallback` with the following.</span></span>

```java
@Override
public void onSuccess(AuthenticationResult authenticationResult) {
    // Log the token for debug purposes
    String accessToken = authenticationResult.getAccessToken();
    Log.d("AUTH", String.format("Access token: %s", accessToken));

    // Get Graph client and get user
    GraphHelper graphHelper = GraphHelper.getInstance();
    graphHelper.getUser(accessToken, getUserCallback());
}
```

<span data-ttu-id="23231-146">変更を保存してすぐにアプリを実行すると、サインイン後にユーザーの表示名と電子メールアドレスで UI が更新されます。</span><span class="sxs-lookup"><span data-stu-id="23231-146">If you save your changes and run the app now, after sign-in the UI is updated with the user's display name and email address.</span></span>
