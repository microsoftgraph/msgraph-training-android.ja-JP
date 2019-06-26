# <a name="how-to-run-the-completed-project"></a>完了したプロジェクトを実行する方法

## <a name="prerequisites"></a>前提条件

このフォルダーで完了したプロジェクトを実行するには、次のものが必要です。

- 開発用コンピューターにインストールされている[Android Studio](https://developer.android.com/studio/) 。 (**メモ:** このチュートリアルは、1.8.0 JRE および ANDROID 9.0 SDK を使用して android Studio バージョン3.3.1 で記述されています。 このガイドの手順は、他のバージョンでは動作しますが、テストされていません。)
- Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または Microsoft 職場または学校のアカウントのいずれか。

Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。

- [新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。
- [Office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。

## <a name="register-an-application-with-the-azure-portal"></a>Azure Portal にアプリケーションを登録する

1. ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動して、**個人用アカウント** (別名: Microsoft アカウント)、または**職場/学校アカウント**を使用してログインします。

1. 左側のナビゲーションで [ **Azure Active Directory** ] を選択し、[**管理**] の下にある [**アプリの登録**] を選択します。

    ![アプリの登録のスクリーンショット ](../../tutorial/images/aad-portal-app-registrations.png)

1. **[新規登録]** を選択します。 **[アプリケーションを登録]** ページで、次のように値を設定します。

    - `Android Graph Tutorial` に **[名前]** を設定します。
    - [**サポートされているアカウントの種類**] を [**任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント**] に設定します。
    - **[リダイレクト URI]** を空のままにします。

    ![[アプリケーションの登録] ページのスクリーンショット](../../tutorial/images/aad-register-an-app.png)

1. [**登録**] を選択します。 **Xamarin Graph のチュートリアル**ページで、**アプリケーション (クライアント) ID**の値をコピーして保存します。次の手順で必要になります。

    ![新しいアプリの登録のアプリケーション ID のスクリーンショット](../../tutorial/images/aad-application-id.png)

1. [**リダイレクト URI を追加する**] リンクを選択します。 [**リダイレクト uri** ] ページで、[**パブリッククライアント (モバイル、デスクトップ)] セクションの推奨されるリダイレクト uri**を見つけます。 で`msal`始まる URI を選択してコピーし、[**保存**] を選択します。 コピーしたリダイレクト URI を保存するには、次の手順で必要になります。

    ![リダイレクト Uri ページのスクリーンショット](../../tutorial/images/aad-redirect-uris.png)

## <a name="run-the-sample"></a>サンプルを実行する

Android Studio で、[**実行**] メニューの [ **' アプリの実行**] を選択します。
