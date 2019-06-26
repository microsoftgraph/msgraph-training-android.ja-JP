<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1465a-101">最初に、新しい Android Studio プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="1465a-101">Begin by creating a new Android Studio project.</span></span>

1. <span data-ttu-id="1465a-102">[Android Studio] を開き、[ようこそ] 画面で [**新しい Android studio プロジェクトの開始**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-102">Open Android Studio, and select **Start a new Android Studio project** on the welcome screen.</span></span>

1. <span data-ttu-id="1465a-103">[**新しいプロジェクトの作成**] ダイアログで、[**空のアクティビティ**] を選択し、[**次へ**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-103">In the **Create New Project** dialog, select **Empty Activity**, then select **Next**.</span></span>

    ![Android Studio の [新しいプロジェクトの作成] ダイアログのスクリーンショット](./images/choose-project.png)

1. <span data-ttu-id="1465a-105">[**プロジェクトの構成**] ダイアログで、[**名前**] `Graph Tutorial`をに設定し、[ **Language** ] `Java`フィールドがに設定されていることを`API 27: Android 8.1 (Oreo)`確認して、[**最小 API レベル**] がに設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1465a-105">In the **Configure your project** dialog, set the **Name** to `Graph Tutorial`, ensure the **Language** field is set to `Java`, and ensure the **Minimum API level** is set to `API 27: Android 8.1 (Oreo)`.</span></span> <span data-ttu-id="1465a-106">必要に応じて、**パッケージ名**と**保存場所**を変更します。</span><span class="sxs-lookup"><span data-stu-id="1465a-106">Modify the **Package name** and **Save location** as needed.</span></span> <span data-ttu-id="1465a-107">**[完了]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-107">Select **Finish**.</span></span>

    ![[プロジェクトの構成] ダイアログのスクリーンショット](./images/configure-project.png)

> [!IMPORTANT]
> <span data-ttu-id="1465a-109">これらのラボ手順で指定したプロジェクトの名前と完全に同じ名前を入力してください。</span><span class="sxs-lookup"><span data-stu-id="1465a-109">Ensure that you enter the exact same name for the project that is specified in these lab instructions.</span></span> <span data-ttu-id="1465a-110">プロジェクト名は、コード内の名前空間の一部になります。</span><span class="sxs-lookup"><span data-stu-id="1465a-110">The project name becomes part of the namespace in the code.</span></span> <span data-ttu-id="1465a-111">これらの手順内のコードは、この手順で指定したプロジェクト名に一致する名前空間に依存します。</span><span class="sxs-lookup"><span data-stu-id="1465a-111">The code inside these instructions depends on the namespace matching the project name specified in these instructions.</span></span> <span data-ttu-id="1465a-112">別のプロジェクト名を使用すると、プロジェクトの作成時に入力したプロジェクト名に一致するすべての名前空間を調整しない限り、コードはコンパイルされません。</span><span class="sxs-lookup"><span data-stu-id="1465a-112">If you use a different project name the code will not compile unless you adjust all the namespaces to match the project name you enter when you create the project.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="1465a-113">依存関係のインストール</span><span class="sxs-lookup"><span data-stu-id="1465a-113">Install dependencies</span></span>

<span data-ttu-id="1465a-114">に進む前に、後で使用する追加の依存関係をインストールします。</span><span class="sxs-lookup"><span data-stu-id="1465a-114">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="1465a-115">`com.android.support:design`を選択して、ナビゲーションドロアーレイアウトをアプリで使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="1465a-115">`com.android.support:design` to make the navigation drawer layouts available to the app.</span></span>
- <span data-ttu-id="1465a-116">Azure AD 認証とトークン管理を処理するため[の、Android 用 Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-android) 。</span><span class="sxs-lookup"><span data-stu-id="1465a-116">[Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) to handle Azure AD authentication and token management.</span></span>
- <span data-ttu-id="1465a-117">Microsoft graph [SDK For Java](https://github.com/microsoftgraph/msgraph-sdk-java)を使用して microsoft graph を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="1465a-117">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) for making calls to the Microsoft Graph.</span></span>

1. <span data-ttu-id="1465a-118">[ **Gradle Scripts**] を展開し、 **Gradle (Module: app)** ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="1465a-118">Expand **Gradle Scripts**, then open the **build.gradle (Module: app)** file.</span></span>

1. <span data-ttu-id="1465a-119">値の`dependencies`中に次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="1465a-119">Add the following lines inside the `dependencies` value.</span></span>

    ```Gradle
    implementation 'com.android.support:design:28.0.0'
    implementation 'com.microsoft.graph:microsoft-graph:1.4.0'
    implementation 'com.microsoft.identity.client:msal:0.2.2'
    ```

    > [!NOTE]
    > <span data-ttu-id="1465a-120">別のバージョンの SDK を使用している場合は、を`28.0.0`変更して、gradle に`com.android.support:appcompat-v7`既に存在する\*\*\*\* 依存関係のバージョンと一致するようにしてください。</span><span class="sxs-lookup"><span data-stu-id="1465a-120">If you are using a different SDK version, make sure to change the `28.0.0` to match the version of the `com.android.support:appcompat-v7` dependency already present in **build.gradle**.</span></span>

1. <span data-ttu-id="1465a-121">`packagingOptions` **Gradle (Module: app)** ファイル内の値の`android`内部を追加します。</span><span class="sxs-lookup"><span data-stu-id="1465a-121">Add a `packagingOptions` inside the `android` value in the **build.gradle (Module: app)** file.</span></span>

    ```Gradle
    packagingOptions {
        pickFirst 'META-INF/jersey-module-version'
    }
    ```

1. <span data-ttu-id="1465a-122">変更内容を保存します。</span><span class="sxs-lookup"><span data-stu-id="1465a-122">Save your changes.</span></span> <span data-ttu-id="1465a-123">[**ファイル**] メニューの [**プロジェクトを Gradle ファイルと同期**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-123">On the **File** menu, select **Sync Project with Gradle Files**.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="1465a-124">アプリを設計する</span><span class="sxs-lookup"><span data-stu-id="1465a-124">Design the app</span></span>

<span data-ttu-id="1465a-125">アプリケーションは、[ナビゲーションドロアー](https://developer.android.com/training/implementing-navigation/nav-drawer)を使用して、さまざまなビュー間を移動します。</span><span class="sxs-lookup"><span data-stu-id="1465a-125">The application will use a [navigation drawer](https://developer.android.com/training/implementing-navigation/nav-drawer) to navigate between different views.</span></span> <span data-ttu-id="1465a-126">この手順では、ナビゲーションドロアーレイアウトを使用するようにアクティビティを更新し、ビューのフラグメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="1465a-126">In this step you will update the activity to use a navigation drawer layout, and add fragments for the views.</span></span>

### <a name="create-a-navigation-drawer"></a><span data-ttu-id="1465a-127">ナビゲーションドロアーを作成する</span><span class="sxs-lookup"><span data-stu-id="1465a-127">Create a navigation drawer</span></span>

<span data-ttu-id="1465a-128">このセクションでは、アプリのナビゲーションメニューのアイコンを作成し、アプリケーションのメニューを作成します。さらに、ナビゲーションドロアーとの互換性を持たせるためにアプリケーションのテーマとレイアウトを更新します。</span><span class="sxs-lookup"><span data-stu-id="1465a-128">In this section you will create icons for the app's navigation menu, create a menu for the application, and update the application's theme and layout to be compatible with a navigation drawer.</span></span>

#### <a name="create-icons"></a><span data-ttu-id="1465a-129">アイコンを作成する</span><span class="sxs-lookup"><span data-stu-id="1465a-129">Create icons</span></span>

1. <span data-ttu-id="1465a-130">**App/res/** 作成用フォルダーを右クリックし、[**新規**]、[**ベクトルアセット**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-130">Right-click the **app/res/drawable** folder and select **New**, then **Vector Asset**.</span></span>

1. <span data-ttu-id="1465a-131">[**クリップアート**] の横にあるアイコンボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1465a-131">Click the icon button next to **Clip Art**.</span></span>

1. <span data-ttu-id="1465a-132">**[アイコンの選択**] ウィンドウで`home` 、検索バーに入力し、[**ホーム**] アイコンを選択して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-132">In the **Select Icon** window, type `home` in the search bar, then select the **Home** icon and select **OK**.</span></span>

1. <span data-ttu-id="1465a-133">**名前**をに`ic_menu_home`変更します。</span><span class="sxs-lookup"><span data-stu-id="1465a-133">Change the **Name** to `ic_menu_home`.</span></span>

    ![[ベクトルアセットの構成] ウィンドウのスクリーンショット](./images/create-icon.png)

1. <span data-ttu-id="1465a-135">[**次へ**] を選択し、[**完了**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-135">Select **Next**, then **Finish**.</span></span>

1. <span data-ttu-id="1465a-136">前の手順を繰り返して、さらに3つのアイコンを作成します。</span><span class="sxs-lookup"><span data-stu-id="1465a-136">Repeat the previous step to create three more icons.</span></span>

    - <span data-ttu-id="1465a-137">名前: `ic_menu_calendar`、アイコン:`event`</span><span class="sxs-lookup"><span data-stu-id="1465a-137">Name: `ic_menu_calendar`, Icon: `event`</span></span>
    - <span data-ttu-id="1465a-138">名前: `ic_menu_signout`、アイコン:`exit to app`</span><span class="sxs-lookup"><span data-stu-id="1465a-138">Name: `ic_menu_signout`, Icon: `exit to app`</span></span>
    - <span data-ttu-id="1465a-139">名前: `ic_menu_signin`、アイコン:`person add`</span><span class="sxs-lookup"><span data-stu-id="1465a-139">Name: `ic_menu_signin`, Icon: `person add`</span></span>

#### <a name="create-the-menu"></a><span data-ttu-id="1465a-140">メニューを作成する</span><span class="sxs-lookup"><span data-stu-id="1465a-140">Create the menu</span></span>

1. <span data-ttu-id="1465a-141">**Res**フォルダーを右クリックし、[**新規作成**]、[ **Android リソースディレクトリ**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-141">Right-click the **res** folder and select **New**, then **Android Resource Directory**.</span></span>

1. <span data-ttu-id="1465a-142">リソースの**種類**をに`menu`変更し、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-142">Change the **Resource type** to `menu` and select **OK**.</span></span>

1. <span data-ttu-id="1465a-143">新しい**メニュー**フォルダーを右クリックし、[**新規作成**]、[**メニューのリソースファイル**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-143">Right-click the new **menu** folder and select **New**, then **Menu resource file**.</span></span>

1. <span data-ttu-id="1465a-144">ファイル`drawer_menu`の名前を指定して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-144">Name the file `drawer_menu` and select **OK**.</span></span>

1. <span data-ttu-id="1465a-145">ファイルが開いたら、[**テキスト**] タブを選択して XML を表示し、コンテンツ全体を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1465a-145">When the file opens, select the **Text** tab to view the XML, then replace the entire contents with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <menu xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        tools:showIn="navigation_view">

        <group android:checkableBehavior="single">
            <item
                android:id="@+id/nav_home"
                android:icon="@drawable/ic_menu_home"
                android:title="Home" />

            <item
                android:id="@+id/nav_calendar"
                android:icon="@drawable/ic_menu_calendar"
                android:title="Calendar" />
        </group>

        <item android:title="Account">
            <menu>
                <item
                    android:id="@+id/nav_signin"
                    android:icon="@drawable/ic_menu_signin"
                    android:title="Sign In" />

                <item
                    android:id="@+id/nav_signout"
                    android:icon="@drawable/ic_menu_signout"
                    android:title="Sign Out" />
            </menu>
        </item>

    </menu>
    ```

#### <a name="update-application-theme-and-layout"></a><span data-ttu-id="1465a-146">アプリケーションのテーマとレイアウトを更新する</span><span class="sxs-lookup"><span data-stu-id="1465a-146">Update application theme and layout</span></span>

1. <span data-ttu-id="1465a-147">**App/res/values/styles xml**ファイルを開き、をに`Theme.AppCompat.Light.DarkActionBar` `Theme.AppCompat.Light.NoActionBar`置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1465a-147">Open the **app/res/values/styles.xml** file and replace `Theme.AppCompat.Light.DarkActionBar` with `Theme.AppCompat.Light.NoActionBar`.</span></span>

1. <span data-ttu-id="1465a-148">要素内に`style`次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="1465a-148">Add the following lines inside the `style` element.</span></span>

    ```xml
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
    ```

1. <span data-ttu-id="1465a-149">**App/res/layout**フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="1465a-149">Right-click the **app/res/layout** folder.</span></span>

1. <span data-ttu-id="1465a-150">[**新規**]、[**レイアウトリソースファイル**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-150">Select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="1465a-151">ファイル`nav_header`に名前を指定し、**ルート要素**をに`LinearLayout`変更して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-151">Name the file `nav_header` and change the **Root element** to `LinearLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="1465a-152">**Nav_header**ファイルを開き、[**テキスト**] タブを選択します。内容全体を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1465a-152">Open the **nav_header.xml** file and select the **Text** tab. Replace the entire contents with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="176dp"
        android:background="@color/colorPrimary"
        android:gravity="bottom"
        android:orientation="vertical"
        android:padding="16dp"
        android:theme="@style/ThemeOverlay.AppCompat.Dark">

        <ImageView
            android:id="@+id/user_profile_pic"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:src="@mipmap/ic_launcher" />

        <TextView
            android:id="@+id/user_name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:paddingTop="8dp"
            android:text="Test User"
            android:textAppearance="@style/TextAppearance.AppCompat.Body1" />

        <TextView
            android:id="@+id/user_email"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="test@contoso.com" />

    </LinearLayout>
    ```

1. <span data-ttu-id="1465a-153">**App/res/layout/activity_main**ファイルを開き、既存の xml を次のよう`DrawerLayout`に置き換えてレイアウトをに更新します。</span><span class="sxs-lookup"><span data-stu-id="1465a-153">Open the **app/res/layout/activity_main.xml** file and update the layout to a `DrawerLayout` by replacing the existing XML with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:id="@+id/drawer_layout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:fitsSystemWindows="true"
        tools:context=".MainActivity"
        tools:openDrawer="start">

        <RelativeLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">

            <ProgressBar
                android:id="@+id/progressbar"
                android:layout_width="75dp"
                android:layout_height="75dp"
                android:layout_centerInParent="true"
                android:visibility="gone"/>

            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                android:background="@color/colorPrimary"
                android:elevation="4dp"
                android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar" />

            <FrameLayout
                android:id="@+id/fragment_container"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_below="@+id/toolbar" />
        </RelativeLayout>

        <android.support.design.widget.NavigationView
            android:id="@+id/nav_view"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_gravity="start"
            app:headerLayout="@layout/nav_header"
            app:menu="@menu/drawer_menu" />

    </android.support.v4.widget.DrawerLayout>
    ```

1. <span data-ttu-id="1465a-154">**App/res/values/strings/strings**を開き、要素内に`resources`次の要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="1465a-154">Open **app/res/values/strings.xml** and add the following elements inside the `resources` element.</span></span>

    ```xml
    <string name="navigation_drawer_open">Open navigation drawer</string>
    <string name="navigation_drawer_close">Close navigation drawer</string>
    ```

1. <span data-ttu-id="1465a-155">**App/java/com/example/graphtutorial/MainActivity**ファイルを開き、内容全体を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1465a-155">Open the **app/java/com.example/graphtutorial/MainActivity** file and replace the entire contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.support.annotation.NonNull;
    import android.support.design.widget.NavigationView;
    import android.support.v4.view.GravityCompat;
    import android.support.v4.widget.DrawerLayout;
    import android.support.v7.app.ActionBarDrawerToggle;
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import android.support.v7.widget.Toolbar;
    import android.view.Menu;
    import android.view.MenuItem;
    import android.view.View;
    import android.widget.FrameLayout;
    import android.widget.ProgressBar;
    import android.widget.TextView;

    public class MainActivity extends AppCompatActivity implements NavigationView.OnNavigationItemSelectedListener {
        private DrawerLayout mDrawer;
        private NavigationView mNavigationView;
        private View mHeaderView;
        private boolean mIsSignedIn = false;
        private String mUserName = null;
        private String mUserEmail = null;

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

            Menu menu = mNavigationView.getMenu();

            // Hide/show the Sign in, Calendar, and Sign Out buttons
            menu.findItem(R.id.nav_signin).setVisible(!isSignedIn);
            menu.findItem(R.id.nav_calendar).setVisible(isSignedIn);
            menu.findItem(R.id.nav_signout).setVisible(isSignedIn);

            // Set the user name and email in the nav drawer
            TextView userName = mHeaderView.findViewById(R.id.user_name);
            TextView userEmail = mHeaderView.findViewById(R.id.user_email);

            if (isSignedIn) {
                // For testing
                mUserName = "Megan Bowen";
                mUserEmail = "meganb@contoso.com";

                userName.setText(mUserName);
                userEmail.setText(mUserEmail);
            } else {
                mUserName = null;
                mUserEmail = null;

                userName.setText("Please sign in");
                userEmail.setText("");
            }
        }
    }
    ```

### <a name="add-fragments"></a><span data-ttu-id="1465a-156">フラグメントを追加する</span><span class="sxs-lookup"><span data-stu-id="1465a-156">Add fragments</span></span>

<span data-ttu-id="1465a-157">このセクションでは、ホームビューと予定表ビューのフラグメントを作成します。</span><span class="sxs-lookup"><span data-stu-id="1465a-157">In this section you will create fragments for the home and calendar views.</span></span>

1. <span data-ttu-id="1465a-158">**App/res/layout**フォルダーを右クリックし、[**新規作成**]、[**リソースファイルのレイアウト**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-158">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="1465a-159">ファイル`fragment_home`に名前を指定し、**ルート要素**をに`RelativeLayout`変更して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-159">Name the file `fragment_home` and change the **Root element** to `RelativeLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="1465a-160">**Fragment_home**ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1465a-160">Open the **fragment_home.xml** file and replace its contents with the following.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <LinearLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"
            android:orientation="vertical">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center_horizontal"
                android:text="Welcome!"
                android:textSize="30sp" />

            <TextView
                android:id="@+id/home_page_username"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center_horizontal"
                android:paddingTop="8dp"
                android:text="Please sign in"
                android:textSize="20sp" />
        </LinearLayout>

    </RelativeLayout>
    ```

1. <span data-ttu-id="1465a-161">**App/res/layout**フォルダーを右クリックし、[**新規作成**]、[**リソースファイルのレイアウト**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-161">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="1465a-162">ファイル`fragment_calendar`に名前を指定し、**ルート要素**をに`RelativeLayout`変更して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-162">Name the file `fragment_calendar` and change the **Root element** to `RelativeLayout`, then select **OK**.</span></span>

1. <span data-ttu-id="1465a-163">**Fragment_calendar**ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1465a-163">Open the **fragment_calendar.xml** file and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="1465a-164">[ **App/java/com/com. 例**] のチュートリアルフォルダーを右クリックし、[**新規**]、[ **java クラス**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-164">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="1465a-165">クラス`HomeFragment`の名前を指定し、**スーパー**クラスをに`android.support.v4.app.Fragment`設定して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-165">Name the class `HomeFragment` and set the **Superclass** to `android.support.v4.app.Fragment`, then select **OK**.</span></span>

1. <span data-ttu-id="1465a-166">[**ホーム] フラグメント**ファイルを開き、そのコンテンツを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1465a-166">Open the **HomeFragment** file and replace its contents with the following.</span></span>

    ```java
    package com.example.graphtutorial;

    import android.os.Bundle;
    import android.support.annotation.NonNull;
    import android.support.annotation.Nullable;
    import android.support.v4.app.Fragment;
    import android.view.LayoutInflater;
    import android.view.View;
    import android.view.ViewGroup;
    import android.widget.TextView;

    public class HomeFragment extends Fragment {
        private static final String USER_NAME = "userName";

        private String mUserName;

        public HomeFragment() {

        }

        public static HomeFragment createInstance(String userName) {
            HomeFragment fragment = new HomeFragment();

            // Add the provided username to the fragment's arguments
            Bundle args = new Bundle();
            args.putString(USER_NAME, userName);
            fragment.setArguments(args);
            return fragment;
        }

        @Override
        public void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            if (getArguments() != null) {
                mUserName = getArguments().getString(USER_NAME);
            }
        }

        @Nullable
        @Override
        public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
            View homeView = inflater.inflate(R.layout.fragment_home, container, false);

            // If there is a username, replace the "Please sign in" with the username
            if (mUserName != null) {
                TextView userName = homeView.findViewById(R.id.home_page_username);
                userName.setText(mUserName);
            }

            return homeView;
        }
    }
    ```

1. <span data-ttu-id="1465a-167">[ **App/java/com/com. 例**] のチュートリアルフォルダーを右クリックし、[**新規**]、[ **java クラス**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-167">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="1465a-168">クラス`CalendarFragment`の名前を指定し、**スーパー**クラスをに`android.support.v4.app.Fragment`設定して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-168">Name the class `CalendarFragment` and set the **Superclass** to `android.support.v4.app.Fragment`, then select **OK**.</span></span>

1. <span data-ttu-id="1465a-169">**Calendarfragment**ファイルを開き、次の関数を`CalendarFragment`クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="1465a-169">Open the **CalendarFragment** file and add the following function to the `CalendarFragment` class.</span></span>

    ```java
    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_calendar, container, false);
    }
    ```

1. <span data-ttu-id="1465a-170">**Mainactivity .java**ファイルを開き、次の関数をクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="1465a-170">Open the **MainActivity.java** file and add the the following functions to the class.</span></span>

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
    private void openCalendarFragment() {
        getSupportFragmentManager().beginTransaction()
                .replace(R.id.fragment_container, new CalendarFragment())
                .commit();
        mNavigationView.setCheckedItem(R.id.nav_calendar);
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

1. <span data-ttu-id="1465a-171">既存の `onNavigationItemSelected` 関数を、以下の関数で置換します。</span><span class="sxs-lookup"><span data-stu-id="1465a-171">Replace the existing `onNavigationItemSelected` function with the following.</span></span>

    ```java
    @Override
    public boolean onNavigationItemSelected(@NonNull MenuItem menuItem) {
        // Load the fragment that corresponds to the selected item
        switch (menuItem.getItemId()) {
            case R.id.nav_home:
                openHomeFragment(mUserName);
                break;
            case R.id.nav_calendar:
                openCalendarFragment();
                break;
            case R.id.nav_signin:
                signIn();
                break;
            case R.id.nav_signout:
                signOut();
                break;
        }

        mDrawer.closeDrawer(GravityCompat.START);

        return true;
    }
    ```

1. <span data-ttu-id="1465a-172">アプリの起動時にホームフラグメントを読み込む`onCreate`には、関数の最後に次のように追加します。</span><span class="sxs-lookup"><span data-stu-id="1465a-172">Add the following at the end of the `onCreate` function to load the home fragment when the app starts.</span></span>

    ```java
    // Load the home fragment by default on startup
    if (savedInstanceState == null) {
        openHomeFragment(mUserName);
    }
    ```

1. <span data-ttu-id="1465a-173">すべての変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="1465a-173">Save all of your changes.</span></span>

1. <span data-ttu-id="1465a-174">[**実行**] メニューの [ **' App ' の実行**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="1465a-174">On the **Run** menu, select **Run 'app'**.</span></span>

<span data-ttu-id="1465a-175">アプリのメニューは、2つのフラグメント間を移動し、[**サインイン**] または [**サインアウト**] ボタンをタップすると、変更されます。</span><span class="sxs-lookup"><span data-stu-id="1465a-175">The app's menu should work to navigate between the two fragments and change when you tap the **Sign in** or **Sign out** buttons.</span></span>

![アプリケーションのスクリーンショット](./images/app-screens.png)
