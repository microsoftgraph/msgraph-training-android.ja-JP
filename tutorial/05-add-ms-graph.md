<!-- markdownlint-disable MD002 MD041 -->

この演習では、Microsoft Graph をアプリケーションに組み込む必要があります。 このアプリケーションでは [、Microsoft Graph SDK for](https://github.com/microsoftgraph/msgraph-sdk-java) Javaを使用して Microsoft Graph を呼び出します。

## <a name="get-calendar-events-from-outlook"></a>Outlook からカレンダー イベントを取得する

このセクションでは、クラスを拡張して、現在の週のユーザーのイベントを取得する関数を追加し、これらの新しい関数を使用 `GraphHelper` `CalendarFragment` する更新を行います。

1. **GraphHelper を開** き、ファイルの一番上に次 `import` のステートメントを追加します。

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

1. 次の関数をクラスに追加 `GraphHelper` します。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/GraphHelper.java" id="GetEventsSnippet":::

    > [!NOTE]
    > コードの実行を `getCalendarView` 検討します。
    >
    > - 呼び出される URL は `/v1.0/me/calendarview` です。
    >   - The `startDateTime` and query parameters define the start and end of the calendar `endDateTime` view.
    >   - このヘッダーにより、Microsoft Graph はユーザーのタイム ゾーン内の各イベントの開始時刻と終了 `Prefer: outlook.timezone` 時刻を返します。
    >   - `select` 関数は、各イベントに返されるフィールドを、ビューで実際に使用されるフィールドだけに制限します。
    >   - 関数 `orderby` は、開始時刻で結果を並べ替える。
    >   - この `top` 関数は、1 ページあたり 25 の結果を要求します。
    > - 使用可能な結果が他にもありますか確認し、必要に応じて追加のページを要求するコールバック `pagingCallback` が定義されています ( ) 。

1. **app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。 クラスに名前を付 `GraphToIana` け **、[OK] を選択します**。

1. 新しいファイルを開き、その内容を次のファイルに置き換えてください。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/GraphToIana.java" id="GraphToIanaSnippet":::

1. `import` **CalendarFragment** ファイルの一番上に次のステートメントを追加します。

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

1. 次のメンバーをクラスに追加 `CalendarFragment` します。

    ```java
    private List<Event> mEventList = null;
    ```

1. 次の関数をクラスに追加 `CalendarFragment` して、進行状況バーを非表示にし、表示します。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="ProgressBarSnippet":::

1. 次の関数を追加して、関数のコールバックを `getCalendarView` 提供します `GraphHelper` 。

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

1. クラス内の既存 `onCreateView` の関数を次 `CalendarFragment` の関数に置き換える。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="OnCreateViewSnippet":::

    このコードの動作に注意してください。 最初に、アクセス `acquireTokenSilently` トークンを取得するために呼び出します。 アクセス トークンが必要になる度にこのメソッドを呼び出すのは、MSAL のキャッシュ機能とトークン更新機能を利用するベスト プラクティスです。 内部的には、MSAL はキャッシュされたトークンをチェックし、有効期限が切れているか確認します。 トークンが存在し、有効期限が切れていない場合は、キャッシュされたトークンを返すだけです。 有効期限が切れている場合は、トークンを返す前に更新を試みる。

    トークンが取得されると、コードはメソッドを呼び出 `getCalendarView` してユーザーのイベントを取得します。

1. アプリを実行し、サインインして、メニューの **[予定表** ] ナビゲーション 項目をタップします。 Android Studio のデバッグ ログにイベントの JSON ダンプが表示されます。

## <a name="display-the-results"></a>結果の表示

これで、JSON ダンプを何かに置き換え、結果をユーザー に分け親しみのある方法で表示できます。 このセクションでは、予定表フラグメントに追加し、各アイテムのレイアウトを作成し、それぞれのフィールドをビュー内の適切なフィールドにマップするカスタム リスト アダプターを作成します。 `ListView` `ListView` `ListView` `Event` `TextView`

1. in `TextView` **app/res/layout/fragment_calendar.xml** を a に置き換える `ListView` 。

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/fragment_calendar.xml" highlight="6-11":::

1. アプリ **/res/layout フォルダーを右クリック** し、[新規]、次に [レイアウト]**リソース****ファイルの順に選択します**。

1. ファイルに名前を `event_list_item` 付け **、Root 要素** を次に変更 `RelativeLayout` して **、[OK] を選択します**。

1. 新しいファイル **event_list_item.xml** 開き、その内容を次の内容に置き換えてください。

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/event_list_item.xml":::

1. **app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。

1. クラスに名前を付 `EventListAdapter` け **、[OK] を選択します**。

1. **EventListAdapter ファイルを開** き、その内容を次の内容に置き換えてください。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/EventListAdapter.java" id="EventListAdapterSnippet":::

1. **CalendarFragment クラスを開** き、次の関数をクラスに追加します。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="AddEventsToListSnippet":::

1. オーバーライドから一時的なデバッグ コードを置き `success` 換えます `addEventsToList();` 。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/CalendarFragment.java" id="SuccessSnippet" highlight="5":::

1. アプリを実行し、サインインして、[予定表] ナビゲーション 項目 **を** タップします。 イベントの一覧が表示されます。

    ![イベント表のスクリーンショット](./images/calendar-list.png)
