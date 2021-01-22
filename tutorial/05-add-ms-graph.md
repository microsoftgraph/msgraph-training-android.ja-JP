<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="824f9-101">この演習では、Microsoft Graph をアプリケーションに組み込む必要があります。</span><span class="sxs-lookup"><span data-stu-id="824f9-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="824f9-102">このアプリケーションでは [、Microsoft Graph SDK for](https://github.com/microsoftgraph/msgraph-sdk-java) Javaを使用して Microsoft Graph を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="824f9-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="824f9-103">Outlook からカレンダー イベントを取得する</span><span class="sxs-lookup"><span data-stu-id="824f9-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="824f9-104">このセクションでは、クラスを拡張して、現在の週のユーザーのイベントを取得する関数を追加し、これらの新しい関数を使用 `GraphHelper` `CalendarFragment` する更新を行います。</span><span class="sxs-lookup"><span data-stu-id="824f9-104">In this section you will extend the `GraphHelper` class to add a function to get the user's events for the current week and update `CalendarFragment` to use these new functions.</span></span>

1. <span data-ttu-id="824f9-105">**GraphHelper を開** き、ファイルの一番上に次 `import` のステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="824f9-105">Open **GraphHelper** and add the following `import` statements to the top of the file.</span></span>

    ```java
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;
    import com.microsoft.graph.requests.extensions.IEventCollectionRequestBuilder;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    ```

1. <span data-ttu-id="824f9-106">次の関数をクラスに追加 `GraphHelper` します。</span><span class="sxs-lookup"><span data-stu-id="824f9-106">Add the following functions to the `GraphHelper` class.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/GraphHelper.java" id="GetEventsSnippet":::

    > [!NOTE]
    > <span data-ttu-id="824f9-107">コードの実行を `getCalendarView` 検討します。</span><span class="sxs-lookup"><span data-stu-id="824f9-107">Consider what the code in `getCalendarView` is doing.</span></span>
    >
    > - <span data-ttu-id="824f9-108">呼び出される URL は `/v1.0/me/calendarview` です。</span><span class="sxs-lookup"><span data-stu-id="824f9-108">The URL that will be called is `/v1.0/me/calendarview`.</span></span>
    >   - <span data-ttu-id="824f9-109">The `startDateTime` and query parameters define the start and end of the calendar `endDateTime` view.</span><span class="sxs-lookup"><span data-stu-id="824f9-109">The `startDateTime` and `endDateTime` query parameters define the start and end of the calendar view.</span></span>
    >   - <span data-ttu-id="824f9-110">このヘッダーにより、Microsoft Graph はユーザーのタイム ゾーン内の各イベントの開始時刻と終了 `Prefer: outlook.timezone` 時刻を返します。</span><span class="sxs-lookup"><span data-stu-id="824f9-110">the `Prefer: outlook.timezone` header causes the Microsoft Graph to return the start and end times of each event in the user's time zone.</span></span>
    >   - <span data-ttu-id="824f9-111">`select` 関数は、各イベントに返されるフィールドを、ビューで実際に使用されるフィールドだけに制限します。</span><span class="sxs-lookup"><span data-stu-id="824f9-111">The `select` function limits the fields returned for each events to just those the view will actually use.</span></span>
    >   - <span data-ttu-id="824f9-112">関数 `orderby` は、開始時刻で結果を並べ替える。</span><span class="sxs-lookup"><span data-stu-id="824f9-112">The `orderby` function sorts the results by start time.</span></span>
    >   - <span data-ttu-id="824f9-113">この `top` 関数は、1 ページあたり 25 の結果を要求します。</span><span class="sxs-lookup"><span data-stu-id="824f9-113">The `top` function requests 25 results per page.</span></span>
    > - <span data-ttu-id="824f9-114">使用可能な結果が他にもありますか確認し、必要に応じて追加のページを要求するコールバック `pagingCallback` が定義されています ( ) 。</span><span class="sxs-lookup"><span data-stu-id="824f9-114">A callback is defined (`pagingCallback`) to check if there are more results available and to request additional pages if needed.</span></span>

1. <span data-ttu-id="824f9-115">**app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。</span><span class="sxs-lookup"><span data-stu-id="824f9-115">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span> <span data-ttu-id="824f9-116">クラスに名前を付 `GraphToIana` け **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="824f9-116">Name the class `GraphToIana` and select **OK**.</span></span>

1. <span data-ttu-id="824f9-117">新しいファイルを開き、その内容を次のファイルに置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="824f9-117">Open the new file and replace its contents with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/GraphToIana.java" id="GraphToIanaSnippet":::

1. <span data-ttu-id="824f9-118">`import` **CalendarFragment** ファイルの一番上に次のステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="824f9-118">Add the following `import` statements to the top of the **CalendarFragment** file.</span></span>

    ```java
    import android.util.Log;
    import android.widget.ListView;
    import com.google.android.material.snackbar.BaseTransientBottomBar;
    import com.google.android.material.snackbar.Snackbar;
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.core.ClientException;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IAuthenticationResult;
    import com.microsoft.identity.client.exception.MsalException;
    import java.time.DayOfWeek;
    import java.time.ZoneId;
    import java.time.ZonedDateTime;
    import java.time.temporal.ChronoUnit;
    import java.time.temporal.TemporalAdjusters;
    import java.util.List;
    ```

1. <span data-ttu-id="824f9-119">次のメンバーをクラスに追加 `CalendarFragment` します。</span><span class="sxs-lookup"><span data-stu-id="824f9-119">Add the following member to the `CalendarFragment` class.</span></span>

    ```java
    private List<Event> mEventList = null;
    ```

1. <span data-ttu-id="824f9-120">次の関数をクラスに追加 `CalendarFragment` して、進行状況バーを非表示にし、表示します。</span><span class="sxs-lookup"><span data-stu-id="824f9-120">Add the following functions to the `CalendarFragment` class to hide and show the progress bar.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="ProgressBarSnippet":::

1. <span data-ttu-id="824f9-121">次の関数を追加して、関数のコールバックを `getCalendarView` 提供します `GraphHelper` 。</span><span class="sxs-lookup"><span data-stu-id="824f9-121">Add the following function to provide a callback for the `getCalendarView` function in `GraphHelper`.</span></span>

    ```java
    private ICallback<List<Event>> getCalendarViewCallback() {
        return new ICallback<List<Event>>() {
            @Override
            public void success(List<Event> eventList) {
                mEventList = eventList;

                // Temporary for debugging
                String jsonEvents = GraphHelper.getInstance().serializeObject(mEventList);
                Log.d("GRAPH", jsonEvents);

                hideProgressBar();
            }

            @Override
            public void failure(ClientException ex) {
                hideProgressBar();
                Log.e("GRAPH", "Error getting events", ex);
                Snackbar.make(getView(),
                    ex.getMessage(),
                    BaseTransientBottomBar.LENGTH_LONG).show();
            }
        };
    }
    ```

1. <span data-ttu-id="824f9-122">クラス内の既存 `onCreateView` の関数を次 `CalendarFragment` の関数に置き換える。</span><span class="sxs-lookup"><span data-stu-id="824f9-122">Replace the existing `onCreateView` function in the `CalendarFragment` class with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="OnCreateViewSnippet":::

    <span data-ttu-id="824f9-123">このコードの動作に注意してください。</span><span class="sxs-lookup"><span data-stu-id="824f9-123">Notice what this code does.</span></span> <span data-ttu-id="824f9-124">最初に、アクセス `acquireTokenSilently` トークンを取得するために呼び出します。</span><span class="sxs-lookup"><span data-stu-id="824f9-124">First, it calls `acquireTokenSilently` to get the access token.</span></span> <span data-ttu-id="824f9-125">アクセス トークンが必要になる度にこのメソッドを呼び出すのは、MSAL のキャッシュ機能とトークン更新機能を利用するベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="824f9-125">Calling this method every time an access token is needed is a best practice because it takes advantage of MSAL's caching and token refresh abilities.</span></span> <span data-ttu-id="824f9-126">内部的には、MSAL はキャッシュされたトークンをチェックし、有効期限が切れているか確認します。</span><span class="sxs-lookup"><span data-stu-id="824f9-126">Internally, MSAL checks for a cached token, then checks if it is expired.</span></span> <span data-ttu-id="824f9-127">トークンが存在し、有効期限が切れていない場合は、キャッシュされたトークンを返すだけです。</span><span class="sxs-lookup"><span data-stu-id="824f9-127">If the token is present and not expired, it just returns the cached token.</span></span> <span data-ttu-id="824f9-128">有効期限が切れている場合は、トークンを返す前に更新を試みる。</span><span class="sxs-lookup"><span data-stu-id="824f9-128">If it is expired, it attempts to refresh the token before returning it.</span></span>

    <span data-ttu-id="824f9-129">トークンが取得されると、コードはメソッドを呼び出 `getCalendarView` してユーザーのイベントを取得します。</span><span class="sxs-lookup"><span data-stu-id="824f9-129">Once the token is retrieved, the code then calls the `getCalendarView` method to get the user's events.</span></span>

1. <span data-ttu-id="824f9-130">アプリを実行し、サインインして、メニューの **[予定表** ] ナビゲーション 項目をタップします。</span><span class="sxs-lookup"><span data-stu-id="824f9-130">Run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="824f9-131">Android Studio のデバッグ ログにイベントの JSON ダンプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="824f9-131">You should see a JSON dump of the events in the debug log in Android Studio.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="824f9-132">結果の表示</span><span class="sxs-lookup"><span data-stu-id="824f9-132">Display the results</span></span>

<span data-ttu-id="824f9-133">これで、JSON ダンプを何かに置き換え、結果をユーザー に分け親しみのある方法で表示できます。</span><span class="sxs-lookup"><span data-stu-id="824f9-133">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="824f9-134">このセクションでは、予定表フラグメントに追加し、各アイテムのレイアウトを作成し、それぞれのフィールドをビュー内の適切なフィールドにマップするカスタム リスト アダプターを作成します。 `ListView` `ListView` `ListView` `Event` `TextView`</span><span class="sxs-lookup"><span data-stu-id="824f9-134">In this section, you will add a `ListView` to the calendar fragment, create a layout for each item in the `ListView`, and create a custom list adapter for the `ListView` that maps the fields of each `Event` to the appropriate `TextView` in the view.</span></span>

1. <span data-ttu-id="824f9-135">in `TextView` **app/res/layout/fragment_calendar.xml** を a に置き換える `ListView` 。</span><span class="sxs-lookup"><span data-stu-id="824f9-135">Replace the `TextView` in **app/res/layout/fragment_calendar.xml** with a `ListView`.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/fragment_calendar.xml" highlight="6-11":::

1. <span data-ttu-id="824f9-136">アプリ **/res/layout フォルダーを右クリック** し、[新規]、次に [レイアウト]**リソース\*\*\*\*ファイルの順に選択します**。</span><span class="sxs-lookup"><span data-stu-id="824f9-136">Right-click the **app/res/layout** folder and select **New**, then **Layout resource file**.</span></span>

1. <span data-ttu-id="824f9-137">ファイルに名前を `event_list_item` 付け **、Root 要素** を次に変更 `RelativeLayout` して **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="824f9-137">Name the file `event_list_item`, change the **Root element** to `RelativeLayout`, and select **OK**.</span></span>

1. <span data-ttu-id="824f9-138">新しいファイル **event_list_item.xml** 開き、その内容を次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="824f9-138">Open the **event_list_item.xml** file and replace its contents with the following.</span></span>

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/event_list_item.xml":::

1. <span data-ttu-id="824f9-139">**app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。</span><span class="sxs-lookup"><span data-stu-id="824f9-139">Right-click the **app/java/com.example.graphtutorial** folder and select **New**, then **Java Class**.</span></span>

1. <span data-ttu-id="824f9-140">クラスに名前を付 `EventListAdapter` け **、[OK] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="824f9-140">Name the class `EventListAdapter` and select **OK**.</span></span>

1. <span data-ttu-id="824f9-141">**EventListAdapter ファイルを開** き、その内容を次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="824f9-141">Open the **EventListAdapter** file and replace its contents with the following.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/EventListAdapter.java" id="EventListAdapterSnippet":::

1. <span data-ttu-id="824f9-142">**CalendarFragment クラスを開** き、次の関数をクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="824f9-142">Open the **CalendarFragment** class and add the following function to the class.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="AddEventsToListSnippet":::

1. <span data-ttu-id="824f9-143">オーバーライドから一時的なデバッグ コードを置き `success` 換えます `addEventsToList();` 。</span><span class="sxs-lookup"><span data-stu-id="824f9-143">Replace the temporary debugging code from the `success` override with `addEventsToList();`.</span></span>

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="SuccessSnippet" highlight="5":::

1. <span data-ttu-id="824f9-144">アプリを実行し、サインインして、[予定表] ナビゲーション 項目 **を** タップします。</span><span class="sxs-lookup"><span data-stu-id="824f9-144">Run the app, sign in, and tap the **Calendar** navigation item.</span></span> <span data-ttu-id="824f9-145">イベントの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="824f9-145">You should see the list of events.</span></span>

    ![イベント表のスクリーンショット](./images/calendar-list.png)
