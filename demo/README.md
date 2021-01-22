# <a name="how-to-run-the-completed-project"></a>完成したプロジェクトを実行する方法

## <a name="prerequisites"></a>前提条件

このフォルダーで完了したプロジェクトを実行するには、以下が必要です。

- [開発用コンピューター](https://developer.android.com/studio/) にインストールされている Android Studio。 (**注:** このチュートリアルは、Android Studio バージョン 3.6.2 および Android 10.0 SDK で記述されています。 このガイドの手順は他のバージョンでも動作する可能性がありますが、テストは行ってはいではありません)。
- メールボックスがメールボックスを持つ個人の Microsoft アカウントOutlook.com Microsoft の仕事用アカウントまたは学校アカウント。

Microsoft アカウントをお持ちない場合は、無料アカウントを取得するためのオプションが 2 つ提供されています。

- 新しい [個人用 Microsoft アカウントにサインアップできます](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。
- Office [365 開発者プログラムにサインアップして、365](https://developer.microsoft.com/office/dev-program) サブスクリプションを無料Office取得できます。

## <a name="register-an-application-with-the-azure-portal"></a>Azure Portal にアプリケーションを登録する

1. ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動して、**個人用アカウント** (別名: Microsoft アカウント)、または **職場/学校アカウント** を使用してログインします。

1. 左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。

    ![アプリの登録のスクリーンショット ](../../tutorial/images/aad-portal-app-registrations.png)

1. **[新規登録]** を選択します。 **[アプリケーションを登録]** ページで、次のように値を設定します。

    - `Android Graph Tutorial` に **[名前]** を設定します。
    - **[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。
    - [ **リダイレクト URI] で**、ドロップダウンを **パブリック クライアント/** ネイティブ (モバイル & デスクトップ) に設定し、値をプロジェクトのパッケージ名に置き `msauth://YOUR_PACKAGE_NAME/callback` `YOUR_PACKAGE_NAME` 換えてください。

    ![[アプリケーションを登録する] ページのスクリーンショット](../../tutorial/images/aad-register-an-app.png)

1. **[登録]** を選択します。 **[Android Graph チュートリアル]** ページで、アプリケーション **(クライアント) ID** の値をコピーして保存します。次の手順で必要になります。

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](../../tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a>サンプルを構成する

1. ファイルの `msal_config.json.example` 名前を変更 `msal_config.json` し、ファイルをディレクトリに移動 `GraphTutorial/app/src/main/res/raw` します。
1. ファイルを `msal_config.json` 編集し、次の変更を行います。
    1. `YOUR_APP_ID_HERE`Azure portal から **取得した** アプリケーション ID に置き換える。
    1. パッケージ `com.example.graphtutorial` 名に置き換える。

## <a name="run-the-sample"></a>サンプルを実行する

Android Studio で、[実行] メニュー **の [アプリの実行]** **を選択** します。
