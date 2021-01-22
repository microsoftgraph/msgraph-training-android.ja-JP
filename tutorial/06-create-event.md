<!-- markdownlint-disable MD002 MD041 -->

このセクションでは、ユーザーの予定表にイベントを作成する機能を追加します。

1. **GraphHelper を開** き、ファイルの一番上に次 `import` のステートメントを追加します。

    ```java
    import com.microsoft.graph.models.extensions.Attendee;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.EmailAddress;
    import com.microsoft.graph.models.extensions.ItemBody;
    import com.microsoft.graph.models.generated.AttendeeType;
    import com.microsoft.graph.models.generated.BodyType;
    ```

1. 次の関数をクラスに追加 `GraphHelper` して、新しいイベントを作成します。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/GraphHelper.java" id="CreateEventSnippet":::

## <a name="update-new-event-fragment"></a>新しいイベント フラグメントを更新する

1. **app/java/com.example.graphtu読み込み** フォルダーを右クリックし、[新規] を選択し、[クラス] **Javaします**。 クラスに名前を付 `EditTextDateTimePicker` け **、[OK] を選択します**。

1. 新しいファイルを開き、その内容を次のファイルに置き換えてください。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/EditTextDateTimePicker.java" id="DateTimePickerSnippet":::

    このクラスは、コントロールをラップし、ユーザーがコントロールをタップすると日付と時刻の選択コントロールを表示し、日付と時刻を選択して値を `EditText` 更新します。

1. アプリ **/res/layout/fragment_new_event.xml開** き、その内容を次の内容に置き換えてください。

    :::code language="xml" source="../demo/GraphTutorial/app/src/main/res/layout/fragment_new_event.xml":::

1. **NewEventFragment を** 開き、ファイルの上部に `import` 次のステートメントを追加します。

    ```java
    import android.util.Log;
    import android.widget.Button;
    import com.google.android.material.snackbar.BaseTransientBottomBar;
    import com.google.android.material.snackbar.Snackbar;
    import com.google.android.material.textfield.TextInputLayout;
    import com.microsoft.graph.concurrency.ICallback;
    import com.microsoft.graph.core.ClientException;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.identity.client.AuthenticationCallback;
    import com.microsoft.identity.client.IAuthenticationResult;
    import com.microsoft.identity.client.exception.MsalException;
    import java.time.ZoneId;
    import java.time.ZonedDateTime;
    ```

1. 次のメンバーをクラスに追加 `NewEventFragment` します。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/NewEventFragment.java" id="InputsSnippet":::

1. 進行状況バーを表示または非表示にする次の関数を追加します。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/NewEventFragment.java" id="ProgressBarSnippet":::

1. 次の関数を追加して、入力コントロールから値を取得し、関数を呼び出 `GraphHelper.createEvent` します。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/NewEventFragment.java" id="CreateEventSnippet":::

1. 既存のファイルを次 `onCreateView` のコードに置き換える。

    :::code language="java" source="../demo/GraphTutorial/app/src/main/java/com/example/graphtutorial/NewEventFragment.java" id="OnCreateViewSnippet":::

1. 変更内容を保存し、アプリを再起動します。 [新しい **イベント] メニュー** 項目を選択し、フォームに入力して、[作成] を選択 **します**。

    ![アプリのイベント作成フォームのスクリーンショット](images/create-event.png)
