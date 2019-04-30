<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7a59a-101">[android studio] を開き、[ようこそ] 画面で [**新しい android studio プロジェクトの開始**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-101">Open Android Studio, and select **Start a new Android Studio project** on the welcome screen.</span></span> <span data-ttu-id="7a59a-102">[**新しいプロジェクトの作成**] ダイアログで、[**空のアクティビティ**] を選択し、[**次へ**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-102">In the **Create New Project** dialog, select **Empty Activity**, then choose **Next**.</span></span>

![Android Studio の [新しいプロジェクトの作成] ダイアログのスクリーンショット](./images/choose-project.png)

<span data-ttu-id="7a59a-104">[**プロジェクトの構成**] ダイアログで、[**名前**] `Graph Tutorial`をに設定し、[ **Language** ] `Java`フィールドがに設定されていることを`API 27: Android 8.1 (Oreo)`確認して、[**最小 API レベル**] がに設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-104">In the **Configure your project** dialog, set the **Name** to `Graph Tutorial`, ensure the **Language** field is set to `Java`, and ensure the **Minimum API level** is set to `API 27: Android 8.1 (Oreo)`.</span></span> <span data-ttu-id="7a59a-105">必要に応じて、**パッケージ名**と**保存場所**を変更します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-105">Modify the **Package name** and **Save location** as needed.</span></span> <span data-ttu-id="7a59a-106">**[完了]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-106">Select **Finish**.</span></span>

![[プロジェクトの構成] ダイアログのスクリーンショット](./images/configure-project.png)

> [!IMPORTANT]
> <span data-ttu-id="7a59a-108">これらのラボ手順で指定したプロジェクトの名前と完全に同じ名前を入力してください。</span><span class="sxs-lookup"><span data-stu-id="7a59a-108">Ensure that you enter the exact same name for the project that is specified in these lab instructions.</span></span> <span data-ttu-id="7a59a-109">プロジェクト名は、コード内の名前空間の一部になります。</span><span class="sxs-lookup"><span data-stu-id="7a59a-109">The project name becomes part of the namespace in the code.</span></span> <span data-ttu-id="7a59a-110">これらの手順内のコードは、この手順で指定したプロジェクト名に一致する名前空間に依存します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-110">The code inside these instructions depends on the namespace matching the project name specified in these instructions.</span></span> <span data-ttu-id="7a59a-111">別のプロジェクト名を使用すると、プロジェクトの作成時に入力したプロジェクト名に一致するすべての名前空間を調整しない限り、コードはコンパイルされません。</span><span class="sxs-lookup"><span data-stu-id="7a59a-111">If you use a different project name the code will not compile unless you adjust all the namespaces to match the project name you enter when you create the project.</span></span>

<span data-ttu-id="7a59a-112">に進む前に、後で使用する追加の依存関係をインストールします。</span><span class="sxs-lookup"><span data-stu-id="7a59a-112">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="7a59a-113">`com.android.support:design`を選択して、ナビゲーションドロアーレイアウトをアプリで使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="7a59a-113">`com.android.support:design` to make the navigation drawer layouts available to the app.</span></span>
- <span data-ttu-id="7a59a-114">Azure AD 認証とトークン管理を処理するため[の、Android 用 Microsoft Authentication Library (msal)](https://github.com/AzureAD/microsoft-authentication-library-for-android) 。</span><span class="sxs-lookup"><span data-stu-id="7a59a-114">[Microsoft Authentication Library (MSAL) for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android) to handle Azure AD authentication and token management.</span></span>
- <span data-ttu-id="7a59a-115">microsoft graph [SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java)を使用して microsoft graph を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-115">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) for making calls to the Microsoft Graph.</span></span>

<span data-ttu-id="7a59a-116">[ **Gradle Scripts**] を展開し、 **Gradle (Module: app)** ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-116">Expand **Gradle Scripts**, then open the **build.gradle (Module: app)** file.</span></span> <span data-ttu-id="7a59a-117">値の`dependencies`中に次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-117">Add the following lines inside the `dependencies` value.</span></span>

```Gradle
implementation 'com.android.support:design:28.0.0'
implementation 'com.microsoft.graph:microsoft-graph:1.1.+'
implementation 'com.microsoft.identity.client:msal:0.2.+'
```

> [!NOTE]
> <span data-ttu-id="7a59a-118">別のバージョンの SDK を使用している場合は、を`28.0.0`変更して、gradle に`com.android.support:appcompat-v7`既に存在する\*\*\*\* 依存関係のバージョンと一致するようにしてください。</span><span class="sxs-lookup"><span data-stu-id="7a59a-118">If you are using a different SDK version, make sure to change the `28.0.0` to match the version of the `com.android.support:appcompat-v7` dependency already present in **build.gradle**.</span></span>

<span data-ttu-id="7a59a-119">`packagingOptions` **gradle (Module: app)** ファイル内の値の`android`内部を追加します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-119">Add a `packagingOptions` inside the `android` value in the **build.gradle (Module: app)** file.</span></span>

```Gradle
packagingOptions {
    pickFirst 'META-INF/jersey-module-version'
}
```

<span data-ttu-id="7a59a-120">変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-120">Save your changes.</span></span> <span data-ttu-id="7a59a-121">[**ファイル**] メニューの [**プロジェクトを Gradle ファイルと同期**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-121">On the **File** menu, select **Sync Project with Gradle Files**.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="7a59a-122">アプリを設計する</span><span class="sxs-lookup"><span data-stu-id="7a59a-122">Design the app</span></span>

<span data-ttu-id="7a59a-123">アプリケーションは、[ナビゲーションドロアー](https://developer.android.com/training/implementing-navigation/nav-drawer)を使用して、さまざまなビュー間を移動します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-123">The application will use a [navigation drawer](https://developer.android.com/training/implementing-navigation/nav-drawer) to navigate between different views.</span></span> <span data-ttu-id="7a59a-124">この手順では、ナビゲーションドロアーレイアウトを使用するようにアクティビティを更新し、ビューのフラグメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-124">In this step you will update the activity to use a navigation drawer layout, and add fragments for the views.</span></span>

### <a name="create-a-navigation-drawer"></a><span data-ttu-id="7a59a-125">ナビゲーションドロアーを作成する</span><span class="sxs-lookup"><span data-stu-id="7a59a-125">Create a navigation drawer</span></span>

<span data-ttu-id="7a59a-126">最初に、アプリのナビゲーションメニューのアイコンを作成します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-126">Start by creating icons for the app's navigation menu.</span></span> <span data-ttu-id="7a59a-127">**app/res/** 作成用フォルダーを右クリックし、[**新規**]、[**ベクトルアセット**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-127">Right-click the **app/res/drawable** folder and select **New**, then **Vector Asset**.</span></span> <span data-ttu-id="7a59a-128">[**クリップアート**] の横にあるアイコンボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a59a-128">Click the icon button next to **Clip Art**.</span></span> <span data-ttu-id="7a59a-129">**[アイコンの選択**] ウィンドウで`home` 、検索バーに入力し、[**ホーム**] アイコンを選択して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-129">In the **Select Icon** window, type `home` in the search bar, then select the **Home** icon and choose **OK**.</span></span> <span data-ttu-id="7a59a-130">**名前**をに`ic_menu_home`変更します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-130">Change the **Name** to `ic_menu_home`.</span></span>

![[ベクトルアセットの構成] ウィンドウのスクリーンショット](./images/create-icon.png)

<span data-ttu-id="7a59a-132">[**次へ**] を選択し、[**完了**] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a59a-132">Choose **Next**, then **Finish**.</span></span> <span data-ttu-id="7a59a-133">この手順を繰り返して、さらに2つのアイコンを作成します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-133">Repeat this step to create two more icons.</span></span>

- <span data-ttu-id="7a59a-134">名前: `ic_menu_calendar`、アイコン:`event`</span><span class="sxs-lookup"><span data-stu-id="7a59a-134">Name: `ic_menu_calendar`, Icon: `event`</span></span>
- <span data-ttu-id="7a59a-135">名前: `ic_menu_signout`、アイコン:`exit to app`</span><span class="sxs-lookup"><span data-stu-id="7a59a-135">Name: `ic_menu_signout`, Icon: `exit to app`</span></span>
- <span data-ttu-id="7a59a-136">名前: `ic_menu_signin`、アイコン:`person add`</span><span class="sxs-lookup"><span data-stu-id="7a59a-136">Name: `ic_menu_signin`, Icon: `person add`</span></span>

<span data-ttu-id="7a59a-137">次に、アプリケーションのメニューを作成します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-137">Next, create a menu for the application.</span></span> <span data-ttu-id="7a59a-138">**res**フォルダーを右クリックし、[**新規作成**]、[ **Android リソースディレクトリ**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-138">Right-click the **res** folder and choose **New**, then **Android Resource Directory**.</span></span> <span data-ttu-id="7a59a-139">リソースの**種類**をに`menu`変更し、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-139">Change the **Resource type** to `menu` and choose **OK**.</span></span>

<span data-ttu-id="7a59a-140">新しい**メニュー**フォルダーを右クリックし、[**新規作成**]、[**メニューのリソースファイル]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-140">Right-click the new **menu** folder and choose **New**, then **Menu resource file**.</span></span> <span data-ttu-id="7a59a-141">ファイル`drawer_menu`に名前を指定して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-141">Name the file `drawer_menu` and choose **OK**.</span></span> <span data-ttu-id="7a59a-142">ファイルが開いたら、[**テキスト**] タブを選択して XML を表示し、コンテンツ全体を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-142">When the file opens, choose the **Text** tab to view the XML, then replace the entire contents with the following.</span></span>

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

<span data-ttu-id="7a59a-143">次に、ナビゲーションドロアーと互換性を持つようにアプリケーションのテーマを更新します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-143">Now update the application's theme to be compatible with a navigation drawer.</span></span> <span data-ttu-id="7a59a-144">**app/res/values/styles xml**ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-144">Open the **app/res/values/styles.xml** file.</span></span> <span data-ttu-id="7a59a-145">を`Theme.AppCompat.Light.DarkActionBar`に`Theme.AppCompat.Light.NoActionBar`置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-145">Replace `Theme.AppCompat.Light.DarkActionBar` with `Theme.AppCompat.Light.NoActionBar`.</span></span> <span data-ttu-id="7a59a-146">その後、要素内に`style`次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-146">Then add the following lines inside the `style` element.</span></span>

```xml
<item name="windowActionBar">false</item>
<item name="windowNoTitle">true</item>
<item name="android:statusBarColor">@android:color/transparent</item>
```

<span data-ttu-id="7a59a-147">次に、メニューのヘッダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-147">Next, create a header for the menu.</span></span> <span data-ttu-id="7a59a-148">**app/res/layout**フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="7a59a-148">Right-click the **app/res/layout** folder.</span></span> <span data-ttu-id="7a59a-149">[**新規**] を選択し、[**リソースファイルのレイアウト**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-149">Choose **New**, then **Layout resource file**.</span></span> <span data-ttu-id="7a59a-150">ファイル`nav_header`に名前を指定し、**ルート要素**をに`LinearLayout`変更します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-150">Name the file `nav_header` and change the **Root element** to `LinearLayout`.</span></span> <span data-ttu-id="7a59a-151">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a59a-151">Choose **OK**.</span></span>

<span data-ttu-id="7a59a-152">**nav_header**ファイルを開き、[**テキスト**] タブを選択します。内容全体を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-152">Open the **nav_header.xml** file and choose the **Text** tab. Replace the entire contents with the following.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<<?xml version="1.0" encoding="utf-8"?>
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

<span data-ttu-id="7a59a-153">ここで、 **app/res/layout/activity_main**ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-153">Now, open the **app/res/layout/activity_main.xml** file.</span></span> <span data-ttu-id="7a59a-154">既存の XML を次`DrawerLayout`のように置き換えて、レイアウトをに更新します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-154">Update the layout to a `DrawerLayout` by replacing the existing XML with the following.</span></span>

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

<span data-ttu-id="7a59a-155">次に、 **app/res/values/strings/xml**を開きます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-155">Next, open **app/res/values/strings.xml**.</span></span> <span data-ttu-id="7a59a-156">要素内に`resources`次の要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-156">Add the following elements inside the `resources` element.</span></span>

```xml
<string name="navigation_drawer_open">Open navigation drawer</string>
<string name="navigation_drawer_close">Close navigation drawer</string>
```

<span data-ttu-id="7a59a-157">最後に、 **app/java/com/example/graphtutorial/mainactivity**ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-157">Finally, open the **app/java/com.example/graphtutorial/MainActivity** file.</span></span> <span data-ttu-id="7a59a-158">内容全体を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-158">Replace the entire contents with the following.</span></span>

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

### <a name="add-fragments"></a><span data-ttu-id="7a59a-159">フラグメントを追加する</span><span class="sxs-lookup"><span data-stu-id="7a59a-159">Add fragments</span></span>

<span data-ttu-id="7a59a-160">**app/res/layout**フォルダーを右クリックし、[**新規作成**]、[**リソースファイルのレイアウト**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-160">Right-click the **app/res/layout** folder and choose **New**, then **Layout resource file**.</span></span> <span data-ttu-id="7a59a-161">ファイル`fragment_home`に名前を指定し、**ルート要素**をに`RelativeLayout`変更します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-161">Name the file `fragment_home` and change the **Root element** to `RelativeLayout`.</span></span> <span data-ttu-id="7a59a-162">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a59a-162">Choose **OK**.</span></span>

<span data-ttu-id="7a59a-163">**fragment_home**ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-163">Open the **fragment_home.xml** file and replace its contents with the following.</span></span>

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

<span data-ttu-id="7a59a-164">次に、 **app/res/layout**フォルダーを右クリックし、[**新規作成**]、[**リソースファイルのレイアウト**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-164">Next, right-click the **app/res/layout** folder and choose **New**, then **Layout resource file**.</span></span> <span data-ttu-id="7a59a-165">ファイル`fragment_calendar`に名前を指定し、**ルート要素**をに`RelativeLayout`変更します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-165">Name the file `fragment_calendar` and change the **Root element** to `RelativeLayout`.</span></span> <span data-ttu-id="7a59a-166">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a59a-166">Choose **OK**.</span></span>

<span data-ttu-id="7a59a-167">**fragment_calendar**ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-167">Open the **fragment_calendar.xml** file and replace its contents with the following.</span></span>

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

<span data-ttu-id="7a59a-168">次に、[ **app/java/com. 例**] というチュートリアルフォルダーを右クリックし、[**新規**]、[ **java クラス**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-168">Now, right-click the **app/java/com.example.graphtutorial** folder and choose **New**, then **Java Class**.</span></span> <span data-ttu-id="7a59a-169">クラス`HomeFragment`に名前を指定し、**スーパー**クラスをに`android.support.v4.app.Fragment`設定します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-169">Name the class `HomeFragment` and set the **Superclass** to `android.support.v4.app.Fragment`.</span></span> <span data-ttu-id="7a59a-170">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a59a-170">Choose **OK**.</span></span> <span data-ttu-id="7a59a-171">[**ホーム] フラグメント**ファイルを開き、そのコンテンツを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-171">Open the **HomeFragment** file and replace its contents with the following.</span></span>

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

<span data-ttu-id="7a59a-172">次に、[ **app/java/com. 例**] を右クリックし、[**新規作成**]、[ **java クラス**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-172">Next, right-click the **app/java/com.example.graphtutorial** folder and choose **New**, then **Java Class**.</span></span> <span data-ttu-id="7a59a-173">クラス`CalendarFragment`に名前を指定し、**スーパー**クラスをに`android.support.v4.app.Fragment`設定します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-173">Name the class `CalendarFragment` and set the **Superclass** to `android.support.v4.app.Fragment`.</span></span> <span data-ttu-id="7a59a-174">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a59a-174">Choose **OK**.</span></span>

<span data-ttu-id="7a59a-175">**calendarfragment**ファイルを開き、次の関数を`CalendarFragment`クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-175">Open the **CalendarFragment** file and add the following function to the `CalendarFragment` class.</span></span>

```java
@Nullable
@Override
public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
    return inflater.inflate(R.layout.fragment_calendar, container, false);
}
```

<span data-ttu-id="7a59a-176">フラグメントが実装されたので、 `MainActivity` `onNavigationItemSelected`イベントを処理するようにクラスを更新し、フラグメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-176">Now that the fragments are implemented, update the `MainActivity` class to handle the `onNavigationItemSelected` event and use the fragments.</span></span> <span data-ttu-id="7a59a-177">最初に、次の関数をクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-177">First, add the following functions to the class.</span></span>

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

<span data-ttu-id="7a59a-178">次に、既存`onNavigationItemSelected`の関数を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-178">Next, replace the existing `onNavigationItemSelected` function with the following.</span></span>

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

<span data-ttu-id="7a59a-179">最後に、次のように`onCreate`関数の末尾にを追加して、アプリの起動時にホームフラグメントを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-179">Finally, add the following at the end of the `onCreate` function to load the home fragment when the app starts.</span></span>

```java
// Load the home fragment by default on startup
if (savedInstanceState == null) {
    openHomeFragment(mUserName);
}
```

<span data-ttu-id="7a59a-180">すべての変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-180">Save all of your changes.</span></span> <span data-ttu-id="7a59a-181">[**実行**] メニューの [ **' app ' の実行**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="7a59a-181">On the **Run** menu, choose **Run 'app'**.</span></span> <span data-ttu-id="7a59a-182">アプリのメニューは、2つのフラグメント間を移動し、[**サインイン**] または [**サインアウト**] ボタンをタップすると、変更されます。</span><span class="sxs-lookup"><span data-stu-id="7a59a-182">The app's menu should work to navigate between the two fragments and change when you tap the **Sign in** or **Sign out** buttons.</span></span>

![アプリケーションのスクリーンショット](./images/welcome-page.png)