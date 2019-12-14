<!-- markdownlint-disable MD002 MD041 -->

最初に、新しい Android Studio プロジェクトを作成します。

1. [Android Studio] を開き、[ようこそ] 画面で [**新しい Android studio プロジェクトの開始**] を選択します。

1. [**新しいプロジェクトの作成**] ダイアログで、[**空のアクティビティ**] を選択し、[**次へ**] を選択します。

    ![Android Studio の [新しいプロジェクトの作成] ダイアログのスクリーンショット](./images/choose-project.png)

1. [**プロジェクトの構成**] ダイアログで、[**名前**] `Graph Tutorial`をに設定し、[ **Language** ] `Java`フィールドがに設定されていることを`API 29: Android 10.0 (Q)`確認して、[**最小 API レベル**] がに設定されていることを確認します。 必要に応じて、**パッケージ名**と**保存場所**を変更します。 **[完了]** を選択します。

    ![[プロジェクトの構成] ダイアログのスクリーンショット](./images/configure-project.png)

> [!IMPORTANT]
> このチュートリアルのコードと手順では、パッケージ名の**チュートリアル**を使用します。 プロジェクトを作成するときに別のパッケージ名を使用する場合は、この値が表示されているすべての場所にパッケージ名を使用してください。

## <a name="install-dependencies"></a>依存関係のインストール

に進む前に、後で使用する追加の依存関係をインストールします。

- `com.google.android.material:material`を選択すると、アプリで[ナビゲーションビュー](https://material.io/develop/android/components/navigation-view/)を使用できるようになります。
- Azure AD 認証とトークン管理を処理するため[の、Android 用 Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-android) 。
- Microsoft graph [SDK For Java](https://github.com/microsoftgraph/msgraph-sdk-java)を使用して microsoft graph を呼び出すことができます。

1. [ **Gradle Scripts**] を展開し、 **Gradle (Module: app)** ファイルを開きます。

1. 値の`dependencies`中に次の行を追加します。

    ```Gradle
    implementation 'com.google.android.material:material:1.0.0'
    implementation 'com.microsoft.identity.client:msal:1.0.0'
    implementation 'com.microsoft.graph:microsoft-graph:1.6.0'
    ```

1. `packagingOptions` **Gradle (Module: app)** ファイル内の値の`android`中に値を追加します。

    ```Gradle
    packagingOptions {
        pickFirst 'META-INF/jersey-module-version'
    }
    ```

1. 変更内容を保存します。 [**ファイル**] メニューの [**プロジェクトを Gradle ファイルと同期**] を選択します。

## <a name="design-the-app"></a>アプリを設計する

アプリケーションは、ナビゲーションドロアーを使用して、さまざまなビュー間を移動します。 この手順では、ナビゲーションドロアーレイアウトを使用するようにアクティビティを更新し、ビューのフラグメントを追加します。

### <a name="create-a-navigation-drawer"></a>ナビゲーションドロアーを作成する

このセクションでは、アプリのナビゲーションメニューのアイコンを作成し、アプリケーションのメニューを作成します。さらに、ナビゲーションドロアーとの互換性を持たせるためにアプリケーションのテーマとレイアウトを更新します。

#### <a name="create-icons"></a>アイコンを作成する

1. **App/res/** 作成用フォルダーを右クリックし、[**新規**]、[**ベクトルアセット**] の順に選択します。

1. [**クリップアート**] の横にあるアイコンボタンをクリックします。

1. **[アイコンの選択**] ウィンドウで`home` 、検索バーに入力し、[**ホーム**] アイコンを選択して、[ **OK]** を選択します。

1. **名前**をに`ic_menu_home`変更します。

    ![[ベクトルアセットの構成] ウィンドウのスクリーンショット](./images/create-icon.png)

1. [**次へ**] を選択し、[**完了**] を選択します。

1. 前の手順を繰り返して、さらに3つのアイコンを作成します。

    - 名前: `ic_menu_calendar`、アイコン:`event`
    - 名前: `ic_menu_signout`、アイコン:`exit to app`
    - 名前: `ic_menu_signin`、アイコン:`person add`

#### <a name="create-the-menu"></a>メニューを作成する

1. **Res**フォルダーを右クリックし、[**新規作成**]、[ **Android リソースディレクトリ**] の順に選択します。

1. リソースの**種類**をに`menu`変更し、[ **OK]** を選択します。

1. 新しい**メニュー**フォルダーを右クリックし、[**新規作成**]、[**メニューのリソースファイル**] の順に選択します。

1. ファイル`drawer_menu`の名前を指定して、[ **OK]** を選択します。

1. ファイルが開いたら、[**テキスト**] タブを選択して XML を表示し、コンテンツ全体を次のように置き換えます。

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

#### <a name="update-application-theme-and-layout"></a>アプリケーションのテーマとレイアウトを更新する

1. **App/res/values/styles xml**ファイルを開き、をに`Theme.AppCompat.Light.DarkActionBar` `Theme.AppCompat.Light.NoActionBar`置き換えます。

1. 要素内に`style`次の行を追加します。

    ```xml
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
    ```

1. **App/res/layout**フォルダーを右クリックします。

1. [**新規**]、[**レイアウトリソースファイル**] の順に選択します。

1. ファイル`nav_header`に名前を指定し、**ルート要素**をに`LinearLayout`変更して、[ **OK]** を選択します。

1. **Nav_header .xml**ファイルを開き、[**テキスト**] タブを選択します。内容全体を次のように置き換えます。

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

1. **App/res/layout/activity_main .xml**ファイルを開き、既存の xml を次の`DrawerLayout`ように置き換えてレイアウトをに更新します。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
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

            <androidx.appcompat.widget.Toolbar
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

        <com.google.android.material.navigation.NavigationView
            android:id="@+id/nav_view"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_gravity="start"
            app:headerLayout="@layout/nav_header"
            app:menu="@menu/drawer_menu" />

    </androidx.drawerlayout.widget.DrawerLayout>
    ```

1. **App/res/values/strings/strings**を開き、要素内に`resources`次の要素を追加します。

    ```xml
    <string name="navigation_drawer_open">Open navigation drawer</string>
    <string name="navigation_drawer_close">Close navigation drawer</string>
    ```

1. **App/java/com/example/graphtutorial/MainActivity**ファイルを開き、内容全体を次のように置き換えます。

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

### <a name="add-fragments"></a>フラグメントを追加する

このセクションでは、ホームビューと予定表ビューのフラグメントを作成します。

1. **App/res/layout**フォルダーを右クリックし、[**新規作成**]、[**リソースファイルのレイアウト**] の順に選択します。

1. ファイル`fragment_home`に名前を指定し、**ルート要素**をに`RelativeLayout`変更して、[ **OK]** を選択します。

1. **Fragment_home .xml**ファイルを開き、その内容を次のように置き換えます。

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

1. **App/res/layout**フォルダーを右クリックし、[**新規作成**]、[**リソースファイルのレイアウト**] の順に選択します。

1. ファイル`fragment_calendar`に名前を指定し、**ルート要素**をに`RelativeLayout`変更して、[ **OK]** を選択します。

1. **Fragment_calendar .xml**ファイルを開き、その内容を次のように置き換えます。

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

1. [ **App/java/com/com. 例**] のチュートリアルフォルダーを右クリックし、[**新規**]、[ **java クラス**] の順に選択します。

1. クラス`HomeFragment`の名前を指定し、**スーパー**クラスをに`androidx.fragment.app.Fragment`設定して、[ **OK]** を選択します。

1. [**ホーム] フラグメント**ファイルを開き、そのコンテンツを次のように置き換えます。

    ```java
    package com.example.graphtutorial;

    import android.os.Bundle;
    import android.view.LayoutInflater;
    import android.view.View;
    import android.view.ViewGroup;
    import android.widget.TextView;
    import androidx.annotation.NonNull;
    import androidx.annotation.Nullable;
    import androidx.fragment.app.Fragment;

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

1. [ **App/java/com/com. 例**] のチュートリアルフォルダーを右クリックし、[**新規**]、[ **java クラス**] の順に選択します。

1. クラス`CalendarFragment`の名前を指定し、**スーパー**クラスをに`androidx.fragment.app.Fragment`設定して、[ **OK]** を選択します。

1. **Calendarfragment**ファイルを開き、その内容を次のように置き換えます。

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

        @Nullable
        @Override
        public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
            return inflater.inflate(R.layout.fragment_calendar, container, false);
        }
    }
    ```

1. **Mainactivity .java**ファイルを開き、次の関数をクラスに追加します。

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

1. 既存の `onNavigationItemSelected` 関数を、以下の関数で置換します。

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

1. アプリの起動時にホームフラグメントを読み込む`onCreate`には、関数の最後に次のように追加します。

    ```java
    // Load the home fragment by default on startup
    if (savedInstanceState == null) {
        openHomeFragment(mUserName);
    }
    ```

1. すべての変更を保存します。

1. [**実行**] メニューの [ **' App ' の実行**] を選択します。

アプリのメニューは、2つのフラグメント間を移動し、[**サインイン**] または [**サインアウト**] ボタンをタップすると、変更されます。

![アプリケーションのスクリーンショット](./images/app-screens.png)
