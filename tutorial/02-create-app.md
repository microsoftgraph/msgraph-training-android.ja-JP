<!-- markdownlint-disable MD002 MD041 -->

まず、新しい Android Studio プロジェクトを作成します。

1. Android Studio を開き、ようこそ **画面で** [新しい Android Studio プロジェクトを開始する] を選択します。

1. [新しい **プロジェクトの作成] ダイアログで**、[空のアクティビティ] を選択し、[次へ] を **選択します**。 

    ![Android Studio の [新しいプロジェクトの作成] ダイアログのスクリーンショット](./images/choose-project.png)

1. [プロジェクト **の構成**] ダイアログで、[**名前**] を設定し、[言語] フィールドが設定され、[最小 API] レベルが設定されている `Graph Tutorial`  `Java` 必要があります `API 29: Android 10.0 (Q)` 。 必要に **応じて、パッケージ名****と保存場所** を変更します。 **[完了]** を選択します。

    ![[プロジェクトの構成] ダイアログのスクリーンショット](./images/configure-project.png)

> [!IMPORTANT]
> このチュートリアルのコードと手順では、パッケージ名 **com.example.graphtu tutoriall を使用します**。 プロジェクトの作成時に別のパッケージ名を使用する場合は、この値が表示されている場所で必ずパッケージ名を使用してください。

## <a name="install-dependencies"></a>依存関係をインストールする

次に進む前に、後で使用する追加の依存関係をインストールします。

- `com.google.android.material:material` をクリックして [、ナビゲーション ビュー](https://material.io/develop/android/components/navigation-view/) をアプリで使用できます。
- [Azure 認証とトークン管理を処理する Android AD Microsoft Authentication Library (MSAL)。](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [Microsoft Graph SDK for](https://github.com/microsoftgraph/msgraph-sdk-java) Java Microsoft Graph を呼び出す方法について説明します。

1. **[Gradle スクリプト] を展開** し **、build.gradle (モジュール: Graph_Tutorial.app) を開きます**。

1. 値の内部に次の行を追加 `dependencies` します。

    :::code language="gradle" source="../demo/GraphTutorial/app/build.gradle" id="DependenciesSnippet":::

1. `packagingOptions` `android` build.gradle の値内に値 **を追加します (モジュール: Graph_Tutorial.app)。**

    ```Gradle
    packagingOptions {
        pickFirst 'META-INF/jersey-module-version'
    }
    ```

1. MSAL の依存関係である MicrosoftDeviceSDK ライブラリの Azure Maven リポジトリを追加します。 **build.gradle (Project: Graph_Tutorial) を開きます**。 値内の値に `repositories` 次を追加 `allprojects` します。

    ```Gradle
    maven {
        url 'https://pkgs.dev.azure.com/MicrosoftDeviceSDK/DuoSDK-Public/_packaging/Duo-SDK-Feed/maven/v1'
    }
    ```

1. 変更内容を保存します。 [ファイル] **メニューの** [プロジェクトと **Gradle ファイルの同期] を選択します**。

## <a name="design-the-app"></a>アプリを設計する

アプリケーションは、ナビゲーション ドロワーを使用して、さまざまなビュー間を移動します。 この手順では、ナビゲーション ドロワー レイアウトを使用するアクティビティを更新し、ビューにフラグメントを追加します。

### <a name="create-a-navigation-drawer"></a>ナビゲーション ドロワーを作成する

このセクションでは、アプリのナビゲーション メニューのアイコンを作成し、アプリケーションのメニューを作成し、ナビゲーション ドロワーと互換性を持つアプリケーションのテーマとレイアウトを更新します。

#### <a name="create-icons"></a>アイコンを作成する

1. アプリ **/res/drawable** フォルダーを右クリックし、[新規]、次に **[Vector Asset] の順に選択します**。

1. [クリップ アート] の横にあるアイコン ボタン **をクリックします**。

1. [アイコンの **選択] ウィンドウ** で検索バーに入力し、[ホーム] アイコンを選択 `home` して **[OK]** を選択します。 

1. 名前を **次に変更** します `ic_menu_home` 。

    ![[ベクター アセットの構成] ウィンドウのスクリーンショット](./images/create-icon.png)

1. [次 **へ] を選択** し、[完了 **] を選択します**。

1. 前の手順を繰り返して、さらに 4 つのアイコンを作成します。

    - 名前: `ic_menu_calendar` 、アイコン: `event`
    - 名前: `ic_menu_add_event` 、アイコン: `add box`
    - 名前: `ic_menu_signout` 、アイコン: `exit to app`
    - 名前: `ic_menu_signin` 、アイコン: `person add`

#### <a name="create-the-menu"></a>メニューを作成する

1. res フォルダーを **右クリックし、[新規]、** 次に [Android リソース ディレクトリ]**の順に選択します**。 

1. リソースの種類 **を変更し** `menu` **、[OK] を選択します**。

1. 新しいメニュー フォルダーを右 **クリック** し、[新規] **を選択し**、[メニュー リソース ファイル] **を選択します**。

1. ファイルに名前を付 `drawer_menu` け **、[OK] を選択します**。

1. ファイルが開いたら、[コード]タブを選択して XML を表示し、コンテンツ全体を次の内容に置き換えます。

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/menu/drawer_menu.xml":::

#### <a name="update-application-theme-and-layout"></a>アプリケーションのテーマとレイアウトを更新する

1. open the **app/res/values/styles.xml** file and replace `Theme.AppCompat.Light.DarkActionBar` with `Theme.AppCompat.Light.NoActionBar` .

1. 要素内に次の行を追加 `style` します。

    ```xml
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
    ```

1. アプリ **/res/layout フォルダーを右クリック** します。

1. [新規 **] を** 選択し、[レイアウト] リソース **ファイルを選択します**。

1. ファイルに名前を `nav_header` 付け **、Root 要素** を次に変更し `LinearLayout` **、[OK] を選択します**。

1. ファイルを **nav_header.xml** し、[テキスト] タブ **を選択** します。すべての内容を次の内容に置き換えてください。

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/nav_header.xml":::

1. **app/res/layout/activity_main.xml** ファイルを開き、既存の XML を次の XML に置き換え、レイアウトを a に `DrawerLayout` 更新します。

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/activity_main.xml":::

1. アプリ **/res/values/strings.xml** を開き、要素内に次の要素を追加 `resources` します。

    ```xml
    <string name="navigation_drawer_open">Open navigation drawer</string>
    <string name="navigation_drawer_close">Close navigation drawer</string>
    ```

1. **app/java/com.example/graphtutorial/MainActivity** ファイルを開き、コンテンツ全体を次の内容に置き換えてください。

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

### <a name="add-fragments"></a>フラグメントの追加

このセクションでは、ホーム ビューとカレンダー ビューのフラグメントを作成します。

1. アプリ **/res/layout フォルダーを右クリック** し、[新規]、次に [レイアウト]**リソース****ファイルの順に選択します**。

1. ファイルに名前を `fragment_home` 付け **、Root 要素** を次に変更し `RelativeLayout` **、[OK] を選択します**。

1. 新しいファイル **fragment_home.xml** 開き、その内容を次の内容に置き換えてください。

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/fragment_home.xml":::

1. アプリ **/res/layout フォルダーを右クリック** し、[新規]、次に [レイアウト]**リソース****ファイルの順に選択します**。

1. ファイルに名前を `fragment_calendar` 付け **、Root 要素** を次に変更し `RelativeLayout` **、[OK] を選択します**。

1. 新しいファイル **fragment_calendar.xml** 開き、その内容を次の内容に置き換えてください。

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

1. アプリ **/res/layout フォルダーを右クリック** し、[新規]、次に [レイアウト]**リソース****ファイルの順に選択します**。

1. ファイルに名前を `fragment_new_event` 付け **、Root 要素** を次に変更し `RelativeLayout` **、[OK] を選択します**。

1. 新しいファイル **fragment_new_event.xml** 開き、その内容を次の内容に置き換えてください。

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

1. **app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。

1. クラスに名前を付 `HomeFragment` け **、[OK] を選択します**。

1. **HomeFragment ファイルを開** き、その内容を次の内容に置き換えてください。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/HomeFragment.java" id="HomeSnippet":::

1. **app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。

1. クラスに名前を付 `CalendarFragment` け **、[OK] を選択します**。

1. **CalendarFragment ファイルを開** き、その内容を次の内容に置き換えてください。

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

1. **app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。

1. クラスに名前を付 `NewEventFragment` け **、[OK] を選択します**。

1. **NewEventFragment ファイルを開** き、その内容を次の内容に置き換えてください。

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

1. **MainActivity.java ファイルを開** き、次の関数をクラスに追加します。

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

1. 既存の `onNavigationItemSelected` 関数を、以下の関数で置き換えます。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/MainActivity.java" id="OnNavItemSelectedSnippet":::

1. すべての変更を保存します。

1. [実行] **メニューで** 、[アプリの **実行] を選択します**。

アプリのメニューが動作して 2 つのフラグメント間を移動し、[サインイン]ボタンまたは [サインアウト] ボタンをタップ **すると変更されます**。

![アプリケーションのスクリーンショット](./images/app-screens.png)
