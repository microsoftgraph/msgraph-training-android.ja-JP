<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="cefd4-101">この演習では、Microsoft Graph をアプリケーションに組み込みます。</span><span class="sxs-lookup"><span data-stu-id="cefd4-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="cefd4-102">このアプリケーションでは、microsoft graph [SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java)を使用して microsoft graph を呼び出すことにします。</span><span class="sxs-lookup"><span data-stu-id="cefd4-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="cefd4-103">Outlook から予定表のイベントを取得する</span><span class="sxs-lookup"><span data-stu-id="cefd4-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="cefd4-104">最初に、 `GraphHelper`クラスを拡張して、ユーザーのイベントを取得する関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-104">Start by extending the `GraphHelper` class to add a function to get the user's events.</span></span> <span data-ttu-id="cefd4-105">**graphhelper**ファイルを開き、次`import`のステートメントをファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-105">Open the **GraphHelper** file and add the following `import` statements to the top of the file.</span></span>

```java
import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
import com.microsoft.graph.requests.extensions.IEventCollectionPage;
import java.util.LinkedList;
import java.util.List;
```

<span data-ttu-id="cefd4-106">次の関数を`GraphHelper`クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-106">Add the following functions to the `GraphHelper` class.</span></span>

```java
public void getEvents(String accessToken, ICallback<IEventCollectionPage> callback) {
    mAccessToken = accessToken;

    // Use query options to sort by created time
    final List<Option> options = new LinkedList<Option>();
    options.add(new QueryOption("orderby", "createdDateTime DESC"));


    // GET /me/events
    mClient.me().events().buildRequest(options)
            .select("subject,organizer,start,end")
            .get(callback);

}

// Debug function to get the JSON representation of a Graph
// object
public String serializeObject(Object object) {
    return mClient.getSerializer().serializeObject(object);
}
```

<span data-ttu-id="cefd4-107">のコードに`getEvents`ついて検討します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-107">Consider what the code in `getEvents` is doing.</span></span>

- <span data-ttu-id="cefd4-108">呼び出し先の URL は`/v1.0/me/events`になります。</span><span class="sxs-lookup"><span data-stu-id="cefd4-108">The URL that will be called is `/v1.0/me/events`.</span></span>
- <span data-ttu-id="cefd4-109">関数`select`は、各イベントに対して返されるフィールドを、ビューが実際に使用するものだけに制限します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-109">The `select` function limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="cefd4-110">名前`QueryOption`付き`orderby`を使用して、作成された日付と時刻で結果を並べ替え、最新のアイテムを最初に表示します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-110">The `QueryOption` named `orderby` is used to sort the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="cefd4-111">これで`CalendarFragment` 、これらの新機能を使用するように更新されました。</span><span class="sxs-lookup"><span data-stu-id="cefd4-111">Now update `CalendarFragment` to use these new functions.</span></span> <span data-ttu-id="cefd4-112">次`import`のステートメントを**calendarfragment**ファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-112">Add the following `import` statements to the top of the **CalendarFragment** file.</span></span>

```java
import android.util.Log;
import android.widget.ListView;
import android.widget.ProgressBar;
import com.microsoft.graph.concurrency.ICallback;
import com.microsoft.graph.core.ClientException;
import com.microsoft.graph.models.extensions.Event;
import com.microsoft.graph.requests.extensions.IEventCollectionPage;
import com.microsoft.identity.client.AuthenticationCallback;
import com.microsoft.identity.client.AuthenticationResult;
import com.microsoft.identity.client.exception.MsalException;
import java.util.List;
```

<span data-ttu-id="cefd4-113">次のメンバーを`CalendarFragment`クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-113">Add the following members to the `CalendarFragment` class.</span></span>

```java
private List<Event> mEventList = null;
private ProgressBar mProgress = null;
```

<span data-ttu-id="cefd4-114">次の関数を`CalendarFragment`クラスに追加して、進行状況バーの表示と非表示を切り替え、で`getEvents` `GraphHelper`関数のコールバックを提供します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-114">Now add the following functions to the `CalendarFragment` class to hide and show the progress bar, and to provide a callback for the `getEvents` function in `GraphHelper`.</span></span>

```java
private void showProgressBar() {
    getActivity().runOnUiThread(new Runnable() {
        @Override
        public void run() {
            mProgress.setVisibility(View.VISIBLE);
        }
    });
}

private void hideProgressBar() {
    getActivity().runOnUiThread(new Runnable() {
        @Override
        public void run() {
            mProgress.setVisibility(View.GONE);
        }
    });
}

private ICallback<IEventCollectionPage> getEventsCallback() {
    return new ICallback<IEventCollectionPage>() {
        @Override
        public void success(IEventCollectionPage iEventCollectionPage) {
            mEventList = iEventCollectionPage.getCurrentPage();

            // Temporary for debugging
            String jsonEvents = GraphHelper.getInstance().serializeObject(mEventList);
            Log.d("GRAPH", jsonEvents);

            hideProgressBar();
        }

        @Override
        public void failure(ClientException ex) {
            Log.e("GRAPH", "Error getting events", ex);
            hideProgressBar();
        }
    };
}
```

<span data-ttu-id="cefd4-115">最後に、 `GraphHelper`クラス`onCreate`の関数をオーバーライドして、Microsoft Graph からユーザーのイベントを取得します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-115">Finally, override the `onCreate` function in the `GraphHelper` class to get the user's events from Microsoft Graph.</span></span>

```java
@Override
public void onCreate(@Nullable Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    mProgress = getActivity().findViewById(R.id.progressbar);
    showProgressBar();

    // Get a current access token
    AuthenticationHelper.getInstance()
            .acquireTokenSilently(new AuthenticationCallback() {
                @Override
                public void onSuccess(AuthenticationResult authenticationResult) {
                    final GraphHelper graphHelper = GraphHelper.getInstance();

                    // Get the user's events
                    graphHelper.getEvents(authenticationResult.getAccessToken(),
                            getEventsCallback());
                }

                @Override
                public void onError(MsalException exception) {
                    Log.e("AUTH", "Could not get token silently", exception);
                    hideProgressBar();
                }

                @Override
                public void onCancel() {
                    hideProgressBar();
                }
            });
}
```

<span data-ttu-id="cefd4-116">このコードで行われていることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="cefd4-116">Notice what this code does.</span></span> <span data-ttu-id="cefd4-117">最初に、アクセス`acquireTokenSilently`トークンを取得するために呼び出します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-117">First, it calls `acquireTokenSilently` to get the access token.</span></span> <span data-ttu-id="cefd4-118">アクセストークンが必要になるたびに、このメソッドを呼び出すことをお勧めします。これは、msal のキャッシュとトークンの更新機能を活用するため、ベストプラクティスです。</span><span class="sxs-lookup"><span data-stu-id="cefd4-118">Calling this method every time an access token is needed is a best practice because it takes advantage of MSAL's caching and token refresh abilities.</span></span> <span data-ttu-id="cefd4-119">内部的に、msal はキャッシュされたトークンをチェックし、有効期限が切れているかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-119">Internally, MSAL checks for a cached token, then checks if it is expired.</span></span> <span data-ttu-id="cefd4-120">トークンが存在し、期限切れになっていない場合は、キャッシュされたトークンを返します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-120">If the token is present and not expired, it just returns the cached token.</span></span> <span data-ttu-id="cefd4-121">有効期限が切れている場合は、トークンを返す前に更新を試行します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-121">If it is expired, it attempts to refresh the token before returning it.</span></span>

<span data-ttu-id="cefd4-122">トークンが取得されると、コードは`getEvents`メソッドを呼び出してユーザーのイベントを取得します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-122">Once the token is retrieved, the code then calls the `getEvents` method to get the user's events.</span></span>

<span data-ttu-id="cefd4-123">これで、アプリを実行し、サインインすると、メニューの [**予定表**] ナビゲーション項目をタップできるようになります。</span><span class="sxs-lookup"><span data-stu-id="cefd4-123">You can now run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="cefd4-124">Android Studio のデバッグログに、イベントの JSON ダンプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cefd4-124">You should see a JSON dump of the events in the debug log in Android Studio.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="cefd4-125">結果を表示する</span><span class="sxs-lookup"><span data-stu-id="cefd4-125">Display the results</span></span>

<span data-ttu-id="cefd4-126">これで、JSON ダンプを、ユーザーにわかりやすい方法で結果を表示するためのものに置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="cefd4-126">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="cefd4-127">最初に、 `TextView` [ **app/res/layout/fragment_calendar** ] をに置き換え`ListView`ます。</span><span class="sxs-lookup"><span data-stu-id="cefd4-127">Start by replacing the `TextView` in **app/res/layout/fragment_calendar.xml** with a `ListView`.</span></span>

```xml
<ListView
    android:id="@+id/eventlist"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:divider="@color/colorPrimary"
    android:dividerHeight="1dp" />
```

<span data-ttu-id="cefd4-128">次に、の各アイテムのレイアウトを作成`ListView`します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-128">Next, create a layout for each item in the `ListView`.</span></span> <span data-ttu-id="cefd4-129">**app/res/layout**フォルダーを右クリックし、[**新規作成**]、[**リソースファイルのレイアウト**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-129">Right-click the **app/res/layout** folder and choose **New**, then **Layout resource file**.</span></span> <span data-ttu-id="cefd4-130">ファイル`event_list_item`に名前を指定し、**ルート要素**をに`RelativeLayout`変更して、[ **OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-130">Name the file `event_list_item`, change the **Root element** to `RelativeLayout`, and choose **OK**.</span></span> <span data-ttu-id="cefd4-131">**event_list_item**ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="cefd4-131">Open the **event_list_item.xml** file and replace its contents with the following.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">

    <TextView
        android:id="@+id/eventsubject"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Subject"
        android:textSize="20sp" />

    <TextView
        android:id="@+id/eventorganizer"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/eventsubject"
        android:text="Adele Vance"
        android:textSize="15sp" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/eventorganizer"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:paddingEnd="2sp"
            android:text="Start:"
            android:textSize="15sp"
            android:textStyle="bold" />

        <TextView
            android:id="@+id/eventstart"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="1:30 PM 2/19/2019"
            android:textSize="15sp" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:paddingStart="5sp"
            android:paddingEnd="2sp"
            android:text="End:"
            android:textSize="15sp"
            android:textStyle="bold" />

        <TextView
            android:id="@+id/eventend"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="1:30 PM 2/19/2019"
            android:textSize="15sp" />
    </LinearLayout>
</RelativeLayout>
```

<span data-ttu-id="cefd4-132">用のカスタムリストアダプターを作成し`ListView`ます。</span><span class="sxs-lookup"><span data-stu-id="cefd4-132">Now create a custom list adapter for the `ListView`.</span></span> <span data-ttu-id="cefd4-133">この操作を行うには、リスト内のアイテムのビューを作成し、それぞれ`Event`のフィールドをビューの適切`TextView`なものにマップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="cefd4-133">This is necessary to create the views for the items in the list, and to map the fields of each `Event` to the appropriate `TextView` in the view.</span></span>

<span data-ttu-id="cefd4-134">[ **app/java/com/com. 例**] のチュートリアルフォルダーを右クリックし、[**新規**]、[ **java クラス**] の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-134">Right-click the **app/java/com.example.graphtutorial** folder and choose **New**, then **Java Class**.</span></span> <span data-ttu-id="cefd4-135">クラス`EventListAdapter`の名前を指定し、 **[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-135">Name the class `EventListAdapter` and choose **OK**.</span></span> <span data-ttu-id="cefd4-136">**eventlistadapter**ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="cefd4-136">Open the **EventListAdapter** file and replace its contents with the following.</span></span>

```java
package com.example.graphtutorial;

import android.content.Context;
import android.support.annotation.NonNull;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.TextView;

import com.microsoft.graph.models.extensions.DateTimeTimeZone;
import com.microsoft.graph.models.extensions.Event;

import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.util.List;
import java.util.TimeZone;

public class EventListAdapter extends ArrayAdapter<Event> {
    private Context mContext;
    private int mResource;
    private ZoneId mLocalTimeZoneId;

    // Used for the ViewHolder pattern
    // https://developer.android.com/training/improving-layouts/smooth-scrolling
    static class ViewHolder {
        TextView subject;
        TextView organizer;
        TextView start;
        TextView end;
    }

    public EventListAdapter(Context context, int resource, List<Event> events) {
        super(context, resource, events);
        mContext = context;
        mResource = resource;
        mLocalTimeZoneId = TimeZone.getDefault().toZoneId();
    }

    @NonNull
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        Event event = getItem(position);

        ViewHolder holder;

        if (convertView == null) {
            LayoutInflater inflater = LayoutInflater.from(mContext);
            convertView = inflater.inflate(mResource, parent, false);

            holder = new ViewHolder();
            holder.subject = convertView.findViewById(R.id.eventsubject);
            holder.organizer = convertView.findViewById(R.id.eventorganizer);
            holder.start = convertView.findViewById(R.id.eventstart);
            holder.end = convertView.findViewById(R.id.eventend);

            convertView.setTag(holder);
        } else {
            holder = (ViewHolder) convertView.getTag();
        }

        holder.subject.setText(event.subject);
        holder.organizer.setText(event.organizer.emailAddress.name);
        holder.start.setText(getLocalDateTimeString(event.start));
        holder.end.setText(getLocalDateTimeString(event.end));

        return convertView;
    }

    // Convert Graph's DateTimeTimeZone format to
    // a LocalDateTime, then return a formatted string
    private String getLocalDateTimeString(DateTimeTimeZone dateTime) {
        ZonedDateTime localDateTime = LocalDateTime.parse(dateTime.dateTime)
                .atZone(ZoneId.of(dateTime.timeZone))
                .withZoneSameInstant(mLocalTimeZoneId);

        return String.format("%s %s",
                localDateTime.format(DateTimeFormatter.ofLocalizedDate(FormatStyle.MEDIUM)),
                localDateTime.format(DateTimeFormatter.ofLocalizedTime(FormatStyle.SHORT)));
    }
}
```

<span data-ttu-id="cefd4-137">最後に、を`CalendarFragment`使用`EventListAdapter`してを初期化する`ListView`ようにクラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-137">Finally, update the `CalendarFragment` class to use the `EventListAdapter` to initialize the `ListView`.</span></span> <span data-ttu-id="cefd4-138">**calendarfragment**クラスを開き、次の関数をクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-138">Open the **CalendarFragment** class and add the following function to the class.</span></span>

```java
private void addEventsToList() {
    getActivity().runOnUiThread(new Runnable() {
        @Override
        public void run() {
            ListView eventListView = getView().findViewById(R.id.eventlist);

            EventListAdapter listAdapter = new EventListAdapter(getActivity(),
                    R.layout.event_list_item, mEventList);

            eventListView.setAdapter(listAdapter);
        }
    });
}
```

<span data-ttu-id="cefd4-139">行の`success` `mEventList = iEventCollectionPage.getCurrentPage();`後に、次のコード行をオーバーライドに追加します。</span><span class="sxs-lookup"><span data-stu-id="cefd4-139">Add the following line of code in the `success` override after the `mEventList = iEventCollectionPage.getCurrentPage();` line.</span></span>

```java
addEventsToList();
```

<span data-ttu-id="cefd4-140">アプリを実行し、サインインして、**予定表**のナビゲーションアイテムをタップします。</span><span class="sxs-lookup"><span data-stu-id="cefd4-140">Run the app, sign in, and tap the **Calendar** navigation item.</span></span> <span data-ttu-id="cefd4-141">イベントの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="cefd4-141">You should see the list of events.</span></span>

![イベントの表のスクリーンショット](./images/calendar-list.png)
