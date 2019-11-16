# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="0cd79-101">完了したプロジェクトを実行する方法</span><span class="sxs-lookup"><span data-stu-id="0cd79-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0cd79-102">前提条件</span><span class="sxs-lookup"><span data-stu-id="0cd79-102">Prerequisites</span></span>

<span data-ttu-id="0cd79-103">このフォルダーで完了したプロジェクトを実行するには、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="0cd79-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="0cd79-104">開発用コンピューターにインストールされている[Android Studio](https://developer.android.com/studio/) 。</span><span class="sxs-lookup"><span data-stu-id="0cd79-104">[Android Studio](https://developer.android.com/studio/) installed on your development machine.</span></span> <span data-ttu-id="0cd79-105">(**メモ:** このチュートリアルは、android Studio バージョン3.5.1 および ANDROID 10.0 SDK で記述されています。</span><span class="sxs-lookup"><span data-stu-id="0cd79-105">(**Note:** This tutorial was written with Android Studio version 3.5.1 and the Android 10.0 SDK.</span></span> <span data-ttu-id="0cd79-106">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。)</span><span class="sxs-lookup"><span data-stu-id="0cd79-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="0cd79-107">Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または Microsoft 職場または学校のアカウントのいずれか。</span><span class="sxs-lookup"><span data-stu-id="0cd79-107">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="0cd79-108">Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="0cd79-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="0cd79-109">[新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。</span><span class="sxs-lookup"><span data-stu-id="0cd79-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="0cd79-110">[Office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="0cd79-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-an-application-with-the-azure-portal"></a><span data-ttu-id="0cd79-111">Azure Portal にアプリケーションを登録する</span><span class="sxs-lookup"><span data-stu-id="0cd79-111">Register an application with the Azure Portal</span></span>

1. <span data-ttu-id="0cd79-112">ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動して、**個人用アカウント** (別名: Microsoft アカウント)、または**職場/学校アカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="0cd79-112">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="0cd79-113">左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0cd79-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="0cd79-114">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="0cd79-114">A screenshot of the App registrations</span></span> ](../../tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="0cd79-115">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0cd79-115">Select **New registration**.</span></span> <span data-ttu-id="0cd79-116">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="0cd79-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="0cd79-117">`Android Graph Tutorial` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="0cd79-117">Set **Name** to `Android Graph Tutorial`.</span></span>
    - <span data-ttu-id="0cd79-118">**[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="0cd79-118">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="0cd79-119">[**リダイレクト URI**] で、ドロップダウンを [**パブリッククライアント/ネイティブ (モバイル & デスクトップ)** ] に設定`msauth://YOUR_PACKAGE_NAME/callback`し、 `YOUR_PACKAGE_NAME`値をに設定して、プロジェクトのパッケージ名に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0cd79-119">Under **Redirect URI**, set the dropdown to **Public client/native (mobile & desktop)** and set the value to `msauth://YOUR_PACKAGE_NAME/callback`, replacing `YOUR_PACKAGE_NAME` with your project's package name.</span></span>

    ![[アプリケーションの登録] ページのスクリーンショット](../../tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="0cd79-121">[**登録**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="0cd79-121">Select **Register**.</span></span> <span data-ttu-id="0cd79-122">[ **Android Graph のチュートリアル**] ページで、**アプリケーション (クライアント) ID**の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="0cd79-122">On the **Android Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリの登録のアプリケーション ID のスクリーンショット](../../tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a><span data-ttu-id="0cd79-124">サンプルを構成する</span><span class="sxs-lookup"><span data-stu-id="0cd79-124">Configure the sample</span></span>

1. <span data-ttu-id="0cd79-125">ファイルの`msal_config.json.example`名前を`msal_config.json`に変更して、ファイル`GraphTutorial/app/src/main/res/raw`をディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="0cd79-125">Rename the `msal_config.json.example` file to `msal_config.json` and move the file into the `GraphTutorial/app/src/main/res/raw` directory.</span></span>
1. <span data-ttu-id="0cd79-126">`msal_config.json`ファイルを編集し、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="0cd79-126">Edit the `msal_config.json` file and make the following changes.</span></span>
    1. <span data-ttu-id="0cd79-127">を`YOUR_APP_ID_HERE` Azure ポータルから取得した**アプリケーション Id**に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0cd79-127">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the Azure portal.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="0cd79-128">サンプルを実行する</span><span class="sxs-lookup"><span data-stu-id="0cd79-128">Run the sample</span></span>

<span data-ttu-id="0cd79-129">Android Studio で、[**実行**] メニューの [ **' アプリの実行**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="0cd79-129">In Android Studio, select **Run 'app'** on the **Run** menu.</span></span>
