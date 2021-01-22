# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="49636-101">完成したプロジェクトを実行する方法</span><span class="sxs-lookup"><span data-stu-id="49636-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49636-102">前提条件</span><span class="sxs-lookup"><span data-stu-id="49636-102">Prerequisites</span></span>

<span data-ttu-id="49636-103">このフォルダーで完了したプロジェクトを実行するには、以下が必要です。</span><span class="sxs-lookup"><span data-stu-id="49636-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="49636-104">[開発用コンピューター](https://developer.android.com/studio/) にインストールされている Android Studio。</span><span class="sxs-lookup"><span data-stu-id="49636-104">[Android Studio](https://developer.android.com/studio/) installed on your development machine.</span></span> <span data-ttu-id="49636-105">(**注:** このチュートリアルは、Android Studio バージョン 3.6.2 および Android 10.0 SDK で記述されています。</span><span class="sxs-lookup"><span data-stu-id="49636-105">(**Note:** This tutorial was written with Android Studio version 3.6.2 and the Android 10.0 SDK.</span></span> <span data-ttu-id="49636-106">このガイドの手順は他のバージョンでも動作する可能性がありますが、テストは行ってはいではありません)。</span><span class="sxs-lookup"><span data-stu-id="49636-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="49636-107">メールボックスがメールボックスを持つ個人の Microsoft アカウントOutlook.com Microsoft の仕事用アカウントまたは学校アカウント。</span><span class="sxs-lookup"><span data-stu-id="49636-107">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="49636-108">Microsoft アカウントをお持ちない場合は、無料アカウントを取得するためのオプションが 2 つ提供されています。</span><span class="sxs-lookup"><span data-stu-id="49636-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="49636-109">新しい [個人用 Microsoft アカウントにサインアップできます](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="49636-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="49636-110">Office [365 開発者プログラムにサインアップして、365](https://developer.microsoft.com/office/dev-program) サブスクリプションを無料Office取得できます。</span><span class="sxs-lookup"><span data-stu-id="49636-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-an-application-with-the-azure-portal"></a><span data-ttu-id="49636-111">Azure Portal にアプリケーションを登録する</span><span class="sxs-lookup"><span data-stu-id="49636-111">Register an application with the Azure Portal</span></span>

1. <span data-ttu-id="49636-112">ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動して、**個人用アカウント** (別名: Microsoft アカウント)、または **職場/学校アカウント** を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="49636-112">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="49636-113">左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="49636-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="49636-114">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="49636-114">A screenshot of the App registrations</span></span> ](../../tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="49636-115">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="49636-115">Select **New registration**.</span></span> <span data-ttu-id="49636-116">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="49636-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="49636-117">`Android Graph Tutorial` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="49636-117">Set **Name** to `Android Graph Tutorial`.</span></span>
    - <span data-ttu-id="49636-118">**[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="49636-118">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="49636-119">[ **リダイレクト URI] で**、ドロップダウンを **パブリック クライアント/** ネイティブ (モバイル & デスクトップ) に設定し、値をプロジェクトのパッケージ名に置き `msauth://YOUR_PACKAGE_NAME/callback` `YOUR_PACKAGE_NAME` 換えてください。</span><span class="sxs-lookup"><span data-stu-id="49636-119">Under **Redirect URI**, set the dropdown to **Public client/native (mobile & desktop)** and set the value to `msauth://YOUR_PACKAGE_NAME/callback`, replacing `YOUR_PACKAGE_NAME` with your project's package name.</span></span>

    ![[アプリケーションを登録する] ページのスクリーンショット](../../tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="49636-121">**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="49636-121">Select **Register**.</span></span> <span data-ttu-id="49636-122">**[Android Graph チュートリアル]** ページで、アプリケーション **(クライアント) ID** の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="49636-122">On the **Android Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](../../tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a><span data-ttu-id="49636-124">サンプルを構成する</span><span class="sxs-lookup"><span data-stu-id="49636-124">Configure the sample</span></span>

1. <span data-ttu-id="49636-125">ファイルの `msal_config.json.example` 名前を変更 `msal_config.json` し、ファイルをディレクトリに移動 `GraphTutorial/app/src/main/res/raw` します。</span><span class="sxs-lookup"><span data-stu-id="49636-125">Rename the `msal_config.json.example` file to `msal_config.json` and move the file into the `GraphTutorial/app/src/main/res/raw` directory.</span></span>
1. <span data-ttu-id="49636-126">ファイルを `msal_config.json` 編集し、次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="49636-126">Edit the `msal_config.json` file and make the following changes.</span></span>
    1. <span data-ttu-id="49636-127">`YOUR_APP_ID_HERE`Azure portal から **取得した** アプリケーション ID に置き換える。</span><span class="sxs-lookup"><span data-stu-id="49636-127">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the Azure portal.</span></span>
    1. <span data-ttu-id="49636-128">パッケージ `com.example.graphtutorial` 名に置き換える。</span><span class="sxs-lookup"><span data-stu-id="49636-128">Replace `com.example.graphtutorial` with your package name.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="49636-129">サンプルを実行する</span><span class="sxs-lookup"><span data-stu-id="49636-129">Run the sample</span></span>

<span data-ttu-id="49636-130">Android Studio で、[実行] メニュー **の [アプリの実行]** **を選択** します。</span><span class="sxs-lookup"><span data-stu-id="49636-130">In Android Studio, select **Run 'app'** on the **Run** menu.</span></span>
