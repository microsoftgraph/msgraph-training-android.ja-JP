<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="07b21-101">まず、新しい Android Studio プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="07b21-101">Begin by creating a new Android Studio project.</span></span>

1. <span data-ttu-id="07b21-102">Android Studio を開き、ようこそ **画面で** [新しい Android Studio プロジェクトを開始する] を選択します。</span><span class="sxs-lookup"><span data-stu-id="07b21-102">Open Android Studio, and select **Start a new Android Studio project** on the welcome screen.</span></span>

1. <span data-ttu-id="07b21-103">[新しい **プロジェクトの作成] ダイアログで**、[空のアクティビティ] を選択し、[次へ] を **選択します**。 </span><span class="sxs-lookup"><span data-stu-id="07b21-103">In the **Create New Project** dialog, select **Empty Activity**, then select **Next**.</span></span>

    ![Android Studio の [新しいプロジェクトの作成] ダイアログのスクリーンショット](./images/choose-project.png)

1. <span data-ttu-id="07b21-105">[プロジェクト **の構成**] ダイアログで、[**名前**] を設定し、[言語] フィールドが設定され、[最小 API] レベルが設定されている `Graph Tutorial`  `Java` 必要があります `API 29: Android 10.0 (Q)` 。</span><span class="sxs-lookup"><span data-stu-id="07b21-105">In the **Configure your project** dialog, set the **Name** to `Graph Tutorial`, ensure the **Language** field is set to `Java`, and ensure the **Minimum API level** is set to `API 29: Android 10.0 (Q)`.</span></span> <span data-ttu-id="07b21-106">必要に **応じて、パッケージ名\*\*\*\*と保存場所** を変更します。</span><span class="sxs-lookup"><span data-stu-id="07b21-106">Modify the **Package name** and **Save location** as needed.</span></span> <span data-ttu-id="07b21-107">**[完了]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="07b21-107">Select **Finish**.</span></span>

    ![[プロジェクトの構成] ダイアログのスクリーンショット](./images/configure-project.png)

> [!IMPORTANT]
> <span data-ttu-id="07b21-109">このチュートリアルのコードと手順では、パッケージ名 **com.example.graphtu tutoriall を使用します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-109">The code and instructions in this tutorial use the package name **com.example.graphtutorial**.</span></span> <span data-ttu-id="07b21-110">プロジェクトの作成時に別のパッケージ名を使用する場合は、この値が表示されている場所で必ずパッケージ名を使用してください。</span><span class="sxs-lookup"><span data-stu-id="07b21-110">If you use a different package name when creating the project, be sure to use your package name wherever you see this value.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="07b21-111">依存関係をインストールする</span><span class="sxs-lookup"><span data-stu-id="07b21-111">Install dependencies</span></span>

<span data-ttu-id="07b21-112">次に進む前に、後で使用する追加の依存関係をインストールします。</span><span class="sxs-lookup"><span data-stu-id="07b21-112">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="07b21-113">`com.google.android.material:material` をクリックして [、ナビゲーション ビュー](https://material.io/develop/android/components/navigation-view/) をアプリで使用できます。</span><span class="sxs-lookup"><span data-stu-id="07b21-113">`com.google.android.material:material` to make the [navigation view](https://material.io/develop/android/components/navigation-view/) available to the app.</span></span>
- <span data-ttu-id="07b21-114">[Azure 認証とトークン管理を処理する Android AD Microsoft Authentication Library (MSAL)。](https://github.com/AzureAD/microsoft-authentication-library-for-android)</span><span class="sxs-lookup"><span data-stu-id="07b21-114">[Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) to handle Azure AD authentication and token management.</span></span>
- <span data-ttu-id="07b21-115">[Microsoft Graph SDK for](https://github.com/microsoftgraph/msgraph-sdk-java) Java Microsoft Graph を呼び出す方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="07b21-115">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) for making calls to the Microsoft Graph.</span></span>

1. <span data-ttu-id="07b21-116">**[Gradle スクリプト] を展開** し **、build.gradle (モジュール: Graph_Tutorial.app) を開きます**。</span><span class="sxs-lookup"><span data-stu-id="07b21-116">Expand **Gradle Scripts**, then open **build.gradle (Module: Graph_Tutorial.app)**.</span></span>

1. <span data-ttu-id="07b21-117">値の内部に次の行を追加 `dependencies` します。</span><span class="sxs-lookup"><span data-stu-id="07b21-117">Add the following lines inside the `dependencies` value.</span></span>

    :::code language="gradle" source="../demo/GraphTutorial/app/build.gradle" id="DependenciesSnippet":::

1. <span data-ttu-id="07b21-118">`packagingOptions` `android` build.gradle の値内に値 **を追加します (モジュール: Graph_Tutorial.app)。**</span><span class="sxs-lookup"><span data-stu-id="07b21-118">Add a `packagingOptions` value inside the `android` value in **build.gradle (Module: Graph_Tutorial.app)**.</span></span>

    ```Gradle
    packagingOptions {
        pickFirst 'META-INF/jersey-module-version'
    }
    ```

1. <span data-ttu-id="07b21-119">MSAL の依存関係である MicrosoftDeviceSDK ライブラリの Azure Maven リポジトリを追加します。</span><span class="sxs-lookup"><span data-stu-id="07b21-119">Add the Azure Maven repository for the MicrosoftDeviceSDK library, a dependency of MSAL.</span></span> <span data-ttu-id="07b21-120">**build.gradle (Project: Graph_Tutorial) を開きます**。</span><span class="sxs-lookup"><span data-stu-id="07b21-120">Open **build.gradle (Project: Graph_Tutorial)**.</span></span> <span data-ttu-id="07b21-121">値内の値に `repositories` 次を追加 `allprojects` します。</span><span class="sxs-lookup"><span data-stu-id="07b21-121">Add the following to the `repositories` value inside the `allprojects` value.</span></span>

    ```Gradle
    maven {
        url 'https://pkgs.dev.azure.com/MicrosoftDeviceSDK/DuoSDK-Public/_packaging/Duo-SDK-Feed/maven/v1'
    }
    ```

1. <span data-ttu-id="07b21-122">変更内容を保存します。</span><span class="sxs-lookup"><span data-stu-id="07b21-122">Save your changes.</span></span> <span data-ttu-id="07b21-123">[ファイル] **メニューの** [プロジェクトと **Gradle ファイルの同期] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-123">On the **File** menu, select **Sync Project with Gradle Files**.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="07b21-124">アプリを設計する</span><span class="sxs-lookup"><span data-stu-id="07b21-124">Design the app</span></span>

<span data-ttu-id="07b21-125">アプリケーションは、ナビゲーション ドロワーを使用して、さまざまなビュー間を移動します。</span><span class="sxs-lookup"><span data-stu-id="07b21-125">The application will use a navigation drawer to navigate between different views.</span></span> <span data-ttu-id="07b21-126">この手順では、ナビゲーション ドロワー レイアウトを使用するアクティビティを更新し、ビューにフラグメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="07b21-126">In this step you will update the activity to use a navigation drawer layout, and add fragments for the views.</span></span>

### <a name="create-a-navigation-drawer"></a><span data-ttu-id="07b21-127">ナビゲーション ドロワーを作成する</span><span class="sxs-lookup"><span data-stu-id="07b21-127">Create a navigation drawer</span></span>

<span data-ttu-id="07b21-128">このセクションでは、アプリのナビゲーション メニューのアイコンを作成し、アプリケーションのメニューを作成し、ナビゲーション ドロワーと互換性を持つアプリケーションのテーマとレイアウトを更新します。</span><span class="sxs-lookup"><span data-stu-id="07b21-128">In this section you will create icons for the app's navigation menu, create a menu for the application, and update the application's theme and layout to be compatible with a navigation drawer.</span></span>

#### <a name="create-icons"></a><span data-ttu-id="07b21-129">アイコンを作成する</span><span class="sxs-lookup"><span data-stu-id="07b21-129">Create icons</span></span>

1. <span data-ttu-id="07b21-130">アプリ **/res/drawable** フォルダーを右クリックし、[新規]、次に **[Vector Asset] の順に選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-130">Right-click the **app/res/drawable** folder and select **New**, then **Vector Asset**.</span></span>

1. <span data-ttu-id="07b21-131">[クリップ アート] の横にあるアイコン ボタン **をクリックします**。</span><span class="sxs-lookup"><span data-stu-id="07b21-131">Click the icon button next to **Clip Art**.</span></span>

1. <span data-ttu-id="07b21-132">[アイコンの **選択] ウィンドウ** で検索バーに入力し、[ホーム] アイコンを選択 `home` して **[OK]** を選択します。 </span><span class="sxs-lookup"><span data-stu-id="07b21-132">In the **Select Icon** window, type `home` in the search bar, then select the **Home** icon and select **OK**.</span></span>

1. <span data-ttu-id="07b21-133">名前を **次に変更** します `ic_menu_home` 。</span><span class="sxs-lookup"><span data-stu-id="07b21-133">Change the **Name** to `ic_menu_home`.</span></span>

    ![[ベクター アセットの構成] ウィンドウのスクリーンショット](./images/create-icon.png)

1. <span data-ttu-id="07b21-135">[次 **へ] を選択** し、[完了 **] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-135">Select **Next**, then **Finish**.</span></span>

1. <span data-ttu-id="07b21-136">前の手順を繰り返して、さらに 4 つのアイコンを作成します。</span><span class="sxs-lookup"><span data-stu-id="07b21-136">Repeat the previous step to create four more icons.</span></span>

    - <span data-ttu-id="07b21-137">名前: `ic_menu_calendar` 、アイコン: `event`</span><span class="sxs-lookup"><span data-stu-id="07b21-137">Name: `ic_menu_calendar`, Icon: `event`</span></span>
    - <span data-ttu-id="07b21-138">名前: `ic_menu_add_event` 、アイコン: `add box`</span><span class="sxs-lookup"><span data-stu-id="07b21-138">Name: `ic_menu_add_event`, Icon: `add box`</span></span>
    - <span data-ttu-id="07b21-139">名前: `ic_menu_signout` 、アイコン: `exit to app`</span><span class="sxs-lookup"><span data-stu-id="07b21-139">Name: `ic_menu_signout`, Icon: `exit to app`</span></span>
    - <span data-ttu-id="07b21-140">名前: `ic_menu_signin` 、アイコン: `person add`</span><span class="sxs-lookup"><span data-stu-id="07b21-140">Name: `ic_menu_signin`, Icon: `person add`</span></span>

#### <a name="create-the-menu"></a><span data-ttu-id="07b21-141">メニューを作成する</span><span class="sxs-lookup"><span data-stu-id="07b21-141">Create the menu</span></span>

1. <span data-ttu-id="07b21-142">res フォルダーを **右クリックし、[新規]、** 次に [Android リソース ディレクトリ]**の順に選択します**。 </span><span class="sxs-lookup"><span data-stu-id="07b21-142">Right-click the **res** folder and select **New**, then **Android Resource Directory**.</span></span>

1. <span data-ttu-id="07b21-143">リソースの種類 **を変更し** `menu` **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-143">Change the **Resource type** to `menu` and select **OK**.</span></span>

1. <span data-ttu-id="07b21-144">新しいメニュー フォルダーを右 **クリック** し、[新規] **を選択し**、[メニュー リソース ファイル] **を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-144">Right-click the new **menu** folder and select **New**, then **Menu resource file**.</span></span>

1. <span data-ttu-id="07b21-145">ファイルに名前を付 `drawer_menu` け **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-145">Name the file `drawer_menu` and select **OK**.</span></span>

1. <span data-ttu-id="07b21-146">ファイルが開いたら、[コード]タブを選択して XML を表示し、コンテンツ全体を次の内容に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="07b21-146">When the file opens, select the **Code** tab to view the XML, then replace the entire contents with the following.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/menu/drawer_menu.xml":::

#### <a name="update-application-theme-and-layout"></a><span data-ttu-id="07b21-147">アプリケーションのテーマとレイアウトを更新する</span><span class="sxs-lookup"><span data-stu-id="07b21-147">Update application theme and layout</span></span>

1. <span data-ttu-id="07b21-148">open the **app/res/values/styles.xml** file and replace `Theme.AppCompat.Light.DarkActionBar` with `Theme.AppCompat.Light.NoActionBar` .</span><span class="sxs-lookup"><span data-stu-id="07b21-148">Open the **app/res/values/styles.xml** file and replace `Theme.AppCompat.Light.DarkActionBar` with `Theme.AppCompat.Light.NoActionBar`.</span></span>

1. <span data-ttu-id="07b21-149">要素内に次の行を追加 `style` します。</span><span class="sxs-lookup"><span data-stu-id="07b21-149">Add the following lines inside the `style` element.</span></span>

    ```xml
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
    ```

1. <span data-ttu-id="07b21-150">アプリ **/res/layout フォルダーを右クリック** します。</span><span class="sxs-lookup"><span data-stu-id="07b21-150">Right-click the **app/res/layout** folder.</span></span>

1. <span data-ttu-id="07b21-151">[新規 **] を** 選択し、[レイアウト] リソース **ファイルを選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-151">Select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="07b21-152">ファイルに名前を `nav_header` 付け **、Root 要素** を次に変更し `LinearLayout` **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-152">Name the file `nav_header` and change the **Root element** to `LinearLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="07b21-153">ファイルを **nav_header.xml** し、[テキスト] タブ **を選択** します。すべての内容を次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="07b21-153">Open the **nav_header.xml** file and select the **Text** tab. Replace the entire contents with the following.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/nav_header.xml":::

1. <span data-ttu-id="07b21-154">**app/res/layout/activity_main.xml** ファイルを開き、既存の XML を次の XML に置き換え、レイアウトを a に `DrawerLayout` 更新します。</span><span class="sxs-lookup"><span data-stu-id="07b21-154">Open the **app/res/layout/activity_main.xml** file and update the layout to a `DrawerLayout` by replacing the existing XML with the following.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/activity_main.xml":::

1. <span data-ttu-id="07b21-155">アプリ **/res/values/strings.xml** を開き、要素内に次の要素を追加 `resources` します。</span><span class="sxs-lookup"><span data-stu-id="07b21-155">Open **app/res/values/strings.xml** and add the following elements inside the `resources` element.</span></span>

    ```xml
    <string name="navigation_drawer_open">Open navigation drawer</string>
    <string name="navigation_drawer_close">Close navigation drawer</string>
    ```

1. <span data-ttu-id="07b21-156">**app/java/com.example/graphtutorial/MainActivity** ファイルを開き、コンテンツ全体を次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="07b21-156">Open the **app/java/com.example/graphtutorial/MainActivity** file and replace the entire contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.os.Bundle;
    import android.view.Menu;
    import android.view.MenuItem;
    import android.view.View;
    import android.widget.FrameLayout;
    import android.widget.ProgressBar;
    import android.widget.TextView;
    import androidx.annotation.NonNull;
    import androidx.appcompat.app.ActionBarDrawerToggle;
    import androidx.appcompat.app.AppCompatActivity;
    import androidx.appcompat.widget.Toolbar;
    import androidx.core.view.GravityCompat;
    import androidx.drawerlayout.widget.DrawerLayout;
    import com.google.android.material.navigation.NavigationView;

    public class MainActivity extends AppCompatActivity implements NavigationView.OnNavigationItemSelectedListener {
        private static final String SAVED_IS_SIGNED_IN = "isSignedIn";
        private static final String SAVED_USER_NAME = "userName";
        private static final String SAVED_USER_EMAIL = "userEmail";
        private static final String SAVED_USER_TIMEZONE = "userTimeZone";

        private DrawerLayout mDrawer;
        private NavigationView mNavigationView;
        private View mHeaderView;
        private boolean mIsSignedIn = false;
        private String mUserName = null;
        private String mUserEmail = null;
        private String mUserTimeZone = null;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            // Set the toolbar
            Toolbar toolbar = findViewById(R.id.toolbar);
            setSupportActionBar(toolbar);

            mDrawer = findViewById(R.id.drawer_layout);

            // Add the hamburger menu icon
            ActionBarDrawerToggle toggle = new ActionBarDrawerToggle(this, mDrawer, toolbar,
                    R.string.navigation_drawer_open, R.string.navigation_drawer_close);
            mDrawer.addDrawerListener(toggle);
            toggle.syncState();

            mNavigationView = findViewById(R.id.nav_view);

            // Set user name and email
            mHeaderView = mNavigationView.getHeaderView(0);
            setSignedInState(mIsSignedIn);

            // Listen for item select events on menu
            mNavigationView.setNavigationItemSelectedListener(this);

            if (savedInstanceState == null) {
                // Load the home fragment by default on startup
                openHomeFragment(mUserName);
            } else {
                // Restore state
                mIsSignedIn = savedInstanceState.getBoolean(SAVED_IS_SIGNED_IN);
                mUserName = savedInstanceState.getString(SAVED_USER_NAME);
                mUserEmail = savedInstanceState.getString(SAVED_USER_EMAIL);
                mUserTimeZone = savedInstanceState.getString(SAVED_USER_TIMEZONE);
                setSignedInState(mIsSignedIn);
            }
        }

        @Override
        protected void onSaveInstanceState(@NonNull Bundle outState) {
            super.onSaveInstanceState(outState);
            outState.putBoolean(SAVED_IS_SIGNED_IN, mIsSignedIn);
            outState.putString(SAVED_USER_NAME, mUserName);
            outState.putString(SAVED_USER_EMAIL, mUserEmail);
            outState.putString(SAVED_USER_TIMEZONE, mUserTimeZone);
        }

        @Override
        public boolean onNavigationItemSelected(@NonNull MenuItem menuItem) {
            // TEMPORARY
            return false;
        }

        @Override
        public void onBackPressed() {
            if (mDrawer.isDrawerOpen(GravityCompat.START)) {
                mDrawer.closeDrawer(GravityCompat.START);
            } else {
                super.onBackPressed();
            }
        }

        public void showProgressBar()
        {
            FrameLayout container = findViewById(R.id.fragment_container);
            ProgressBar progressBar = findViewById(R.id.progressbar);
            container.setVisibility(View.GONE);
            progressBar.setVisibility(View.VISIBLE);
        }

        public void hideProgressBar()
        {
            FrameLayout container = findViewById(R.id.fragment_container);
            ProgressBar progressBar = findViewById(R.id.progressbar);
            progressBar.setVisibility(View.GONE);
            container.setVisibility(View.VISIBLE);
        }

        // Update the menu and get the user's name and email
        private void setSignedInState(boolean isSignedIn) {
            mIsSignedIn = isSignedIn;

            mNavigationView.getMenu().clear();
            mNavigationView.inflateMenu(R.menu.drawer_menu);

            Menu menu = mNavigationView.getMenu();

            // Hide/show the Sign in, Calendar, and Sign Out buttons
            if (isSignedIn) {
                menu.removeItem(R.id.nav_signin);
            } else {
                menu.removeItem(R.id.nav_home);
                menu.removeItem(R.id.nav_calendar);
                menu.removeItem(R.id.nav_create_event);
                menu.removeItem(R.id.nav_signout);
            }

            // Set the user name and email in the nav drawer
            TextView userName = mHeaderView.findViewById(R.id.user_name);
            TextView userEmail = mHeaderView.findViewById(R.id.user_email);

            if (isSignedIn) {
                // For testing
                mUserName = "Lynne Robbins";
                mUserEmail = "lynner@contoso.com";
                mUserTimeZone = "Pacific Standard Time";

                userName.setText(mUserName);
                userEmail.setText(mUserEmail);
            } else {
                mUserName = null;
                mUserEmail = null;
                mUserTimeZone = null;

                userName.setText("Please sign in");
                userEmail.setText("");
            }
        }
    }
    ```

### <a name="add-fragments"></a><span data-ttu-id="07b21-157">フラグメントの追加</span><span class="sxs-lookup"><span data-stu-id="07b21-157">Add fragments</span></span>

<span data-ttu-id="07b21-158">このセクションでは、ホーム ビューとカレンダー ビューのフラグメントを作成します。</span><span class="sxs-lookup"><span data-stu-id="07b21-158">In this section you will create fragments for the home and calendar views.</span></span>

1. <span data-ttu-id="07b21-159">アプリ **/res/layout フォルダーを右クリック** し、[新規]、次に [レイアウト]**リソース\*\*\*\*ファイルの順に選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-159">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="07b21-160">ファイルに名前を `fragment_home` 付け **、Root 要素** を次に変更し `RelativeLayout` **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-160">Name the file `fragment_home` and change the **Root element** to `RelativeLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="07b21-161">新しいファイル **fragment_home.xml** 開き、その内容を次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="07b21-161">Open the **fragment_home.xml** file and replace its contents with the following.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/fragment_home.xml":::

1. <span data-ttu-id="07b21-162">アプリ **/res/layout フォルダーを右クリック** し、[新規]、次に [レイアウト]**リソース\*\*\*\*ファイルの順に選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-162">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="07b21-163">ファイルに名前を `fragment_calendar` 付け **、Root 要素** を次に変更し `RelativeLayout` **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-163">Name the file `fragment_calendar` and change the **Root element** to `RelativeLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="07b21-164">新しいファイル **fragment_calendar.xml** 開き、その内容を次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="07b21-164">Open the **fragment_calendar.xml** file and replace its contents with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"
            android:text="Calendar"
            android:textSize="30sp" />

    </RelativeLayout>
    ```

1. <span data-ttu-id="07b21-165">アプリ **/res/layout フォルダーを右クリック** し、[新規]、次に [レイアウト]**リソース\*\*\*\*ファイルの順に選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-165">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="07b21-166">ファイルに名前を `fragment_new_event` 付け **、Root 要素** を次に変更し `RelativeLayout` **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-166">Name the file `fragment_new_event` and change the **Root element** to `RelativeLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="07b21-167">新しいファイル **fragment_new_event.xml** 開き、その内容を次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="07b21-167">Open the **fragment_new_event.xml** file and replace its contents with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"
            android:text="New Event"
            android:textSize="30sp" />

    </RelativeLayout>
    ```

1. <span data-ttu-id="07b21-168">**app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。</span><span class="sxs-lookup"><span data-stu-id="07b21-168">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="07b21-169">クラスに名前を付 `HomeFragment` け **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-169">Name the class `HomeFragment`, then select **OK**.</span></span>

1. <span data-ttu-id="07b21-170">**HomeFragment ファイルを開** き、その内容を次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="07b21-170">Open the **HomeFragment** file and replace its contents with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/HomeFragment.java" id="HomeSnippet":::

1. <span data-ttu-id="07b21-171">**app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。</span><span class="sxs-lookup"><span data-stu-id="07b21-171">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="07b21-172">クラスに名前を付 `CalendarFragment` け **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-172">Name the class `CalendarFragment`, then select **OK**.</span></span>

1. <span data-ttu-id="07b21-173">**CalendarFragment ファイルを開** き、その内容を次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="07b21-173">Open the **CalendarFragment** file and replace its contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.os.Bundle;
    import android.view.LayoutInflater;
    import android.view.View;
    import android.view.ViewGroup;
    import androidx.annotation.NonNull;
    import androidx.annotation.Nullable;
    import androidx.fragment.app.Fragment;

    public class CalendarFragment extends Fragment {
        private static final String TIME_ZONE = "timeZone";

        private String mTimeZone;

        public CalendarFragment() {}

        public static CalendarFragment createInstance(String timeZone) {
            CalendarFragment fragment = new CalendarFragment();

            // Add the provided time zone to the fragment's arguments
            Bundle args = new Bundle();
            args.putString(TIME_ZONE, timeZone);
            fragment.setArguments(args);
            return fragment;
        }

        @Override
        public void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            if (getArguments() != null) {
                mTimeZone = getArguments().getString(TIME_ZONE);
            }
        }

        @Nullable
        @Override
        public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
            return inflater.inflate(R.layout.fragment_calendar, container, false);
        }
    }
    ```

1. <span data-ttu-id="07b21-174">**app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。</span><span class="sxs-lookup"><span data-stu-id="07b21-174">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="07b21-175">クラスに名前を付 `NewEventFragment` け **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-175">Name the class `NewEventFragment`, then select **OK**.</span></span>

1. <span data-ttu-id="07b21-176">**NewEventFragment ファイルを開** き、その内容を次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="07b21-176">Open the **NewEventFragment** file and replace its contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.os.Bundle;
    import android.view.LayoutInflater;
    import android.view.View;
    import android.view.ViewGroup;
    import androidx.annotation.NonNull;
    import androidx.annotation.Nullable;
    import androidx.fragment.app.Fragment;

    public class NewEventFragment extends Fragment {
        private static final String TIME_ZONE = "timeZone";

        private String mTimeZone;

        public NewEventFragment() {}

        public static NewEventFragment createInstance(String timeZone) {
            NewEventFragment fragment = new NewEventFragment();

            // Add the provided time zone to the fragment's arguments
            Bundle args = new Bundle();
            args.putString(TIME_ZONE, timeZone);
            fragment.setArguments(args);
            return fragment;
        }

        @Override
        public void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            if (getArguments() != null) {
                mTimeZone = getArguments().getString(TIME_ZONE);
            }
        }

        @Nullable
        @Override
        public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
            return inflater.inflate(R.layout.fragment_new_event, container, false);
        }
    }
    ```

1. <span data-ttu-id="07b21-177">**MainActivity.java ファイルを開** き、次の関数をクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="07b21-177">Open the **MainActivity.java** file and add the the following functions to the class.</span></span>

    ```java
    // Load the "Home" fragment
    public void openHomeFragment(String userName) {
        HomeFragment fragment = HomeFragment.createInstance(userName);
        getSupportFragmentManager().beginTransaction()
                .replace(R.id.fragment_container, fragment)
                .commit();
        mNavigationView.setCheckedItem(R.id.nav_home);
    }

    // Load the "Calendar" fragment
    private void openCalendarFragment(String timeZone) {
        CalendarFragment fragment = CalendarFragment.createInstance(timeZone);
        getSupportFragmentManager().beginTransaction()
                .replace(R.id.fragment_container, fragment)
                .commit();
        mNavigationView.setCheckedItem(R.id.nav_calendar);
    }

    // Load the "New Event" fragment
    private void openNewEventFragment(String timeZone) {
        NewEventFragment fragment = NewEventFragment.createInstance(timeZone);
        getSupportFragmentManager().beginTransaction()
                .replace(R.id.fragment_container, fragment)
                .commit();
        mNavigationView.setCheckedItem(R.id.nav_create_event);
    }

    private void signIn() {
        setSignedInState(true);
        openHomeFragment(mUserName);
    }

    private void signOut() {
        setSignedInState(false);
        openHomeFragment(mUserName);
    }
    ```

1. <span data-ttu-id="07b21-178">既存の `onNavigationItemSelected` 関数を、以下の関数で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="07b21-178">Replace the existing `onNavigationItemSelected` function with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="OnNavItemSelectedSnippet":::

1. <span data-ttu-id="07b21-179">すべての変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="07b21-179">Save all of your changes.</span></span>

1. <span data-ttu-id="07b21-180">[実行] **メニューで** 、[アプリの **実行] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="07b21-180">On the **Run** menu, select **Run 'app'**.</span></span>

<span data-ttu-id="07b21-181">アプリのメニューが動作して 2 つのフラグメント間を移動し、[サインイン]ボタンまたは [サインアウト] ボタンをタップ **すると変更されます**。</span><span class="sxs-lookup"><span data-stu-id="07b21-181">The app's menu should work to navigate between the two fragments and change when you tap the **Sign in** or **Sign out** buttons.</span></span>

![アプリケーションのスクリーンショット](./images/app-screens.png)
