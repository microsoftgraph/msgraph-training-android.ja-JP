# <a name="completed-module-add-azure-ad-authentication"></a>完了したモジュール: Azure AD 認証を追加する

このディレクトリにあるプロジェクトのバージョンは、「 [AZURE AD 認証を追加](https://docs.microsoft.com/graph/tutorials/android?tutorial-step=3)する」のチュートリアルを完了することを反映しています。 このバージョンのプロジェクトを使用する場合は、「[予定表データを取得](https://docs.microsoft.com/graph/tutorials/android?tutorial-step=4)する」からチュートリアルの残りの部分を完了する必要があります。

> **注:**「[アプリをポータルに登録](https://docs.microsoft.com/graph/tutorials/android?tutorial-step=2)する」で示されているように、Azure portal にアプリケーションを登録済みであることを前提としています。 このバージョンのサンプルは、次のように構成する必要があります。
>
> 1. ファイルの`./GraphTutorial/msal_config.json.example`名前を`msal_config.json`に変更します。
> 1. `msal_config.json`ファイルを`./GraphTutorial/app/src/main/res/raw`ディレクトリに移動します。
> 1. `msal_config.json`ファイルを編集し、次のように変更します。
>     1. を`YOUR_APP_ID_HERE` Azure ポータルから取得した**アプリケーション Id**に置き換えます。
