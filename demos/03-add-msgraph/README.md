# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="8f910-101">完了したプロジェクトを実行する方法</span><span class="sxs-lookup"><span data-stu-id="8f910-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f910-102">前提条件</span><span class="sxs-lookup"><span data-stu-id="8f910-102">Prerequisites</span></span>

<span data-ttu-id="8f910-103">このフォルダーで完了したプロジェクトを実行するには、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="8f910-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="8f910-104">開発用コンピューターにインストールされている[Android Studio](https://developer.android.com/studio/) 。</span><span class="sxs-lookup"><span data-stu-id="8f910-104">[Android Studio](https://developer.android.com/studio/) installed on your development machine.</span></span> <span data-ttu-id="8f910-105">(**メモ:** このチュートリアルは、1.8.0 JRE および android 9.0 SDK を使用して android Studio バージョン3.3.1 で記述されています。</span><span class="sxs-lookup"><span data-stu-id="8f910-105">(**Note:** This tutorial was written with Android Studio version 3.3.1 with the 1.8.0 JRE and the Android 9.0 SDK.</span></span> <span data-ttu-id="8f910-106">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。)</span><span class="sxs-lookup"><span data-stu-id="8f910-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="8f910-107">Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または microsoft 職場または学校のアカウントのいずれか。</span><span class="sxs-lookup"><span data-stu-id="8f910-107">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="8f910-108">Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="8f910-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="8f910-109">[新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。</span><span class="sxs-lookup"><span data-stu-id="8f910-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="8f910-110">[office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="8f910-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-an-application-with-the-azure-portal"></a><span data-ttu-id="8f910-111">Azure Portal にアプリケーションを登録する</span><span class="sxs-lookup"><span data-stu-id="8f910-111">Register an application with the Azure Portal</span></span>

1. <span data-ttu-id="8f910-112">ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動して、**個人用アカウント** (別名: Microsoft アカウント)、または**職場/学校アカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="8f910-112">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="8f910-113">左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録 (プレビュー)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8f910-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="8f910-114">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="8f910-114">A screenshot of the App registrations</span></span> ](../../tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="8f910-115">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8f910-115">Select **New registration**.</span></span> <span data-ttu-id="8f910-116">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="8f910-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="8f910-117">`Android Graph Tutorial` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="8f910-117">Set **Name** to `Android Graph Tutorial`.</span></span>
    - <span data-ttu-id="8f910-118">**[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="8f910-118">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="8f910-119">**[リダイレクト URI]** を空のままにします。</span><span class="sxs-lookup"><span data-stu-id="8f910-119">Leave **Redirect URI** empty.</span></span>

    ![[アプリケーションの登録] ページのスクリーンショット](../../tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="8f910-121">**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8f910-121">Choose **Register**.</span></span> <span data-ttu-id="8f910-122">**Xamarin Graph のチュートリアル**ページで、**アプリケーション (クライアント) ID**の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="8f910-122">On the **Xamarin Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリの登録のアプリケーション ID のスクリーンショット](../../tutorial/images/aad-application-id.png)

1. <span data-ttu-id="8f910-124">[**リダイレクト URI を追加する**] リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="8f910-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="8f910-125">[**リダイレクト uri** ] ページで、[**パブリッククライアント (モバイル、デスクトップ)] セクションの推奨されるリダイレクト uri**を見つけます。</span><span class="sxs-lookup"><span data-stu-id="8f910-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="8f910-126">で`msal`始まる URI を選択してコピーし、[**保存**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="8f910-126">Select the URI that begins with `msal` and copy it, then choose **Save**.</span></span> <span data-ttu-id="8f910-127">コピーしたリダイレクト URI を保存するには、次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="8f910-127">Save the copied redirect URI, you will need it in the next step.</span></span>

    ![リダイレクト uri ページのスクリーンショット](../../tutorial/images/aad-redirect-uris.png)

## <a name="run-the-sample"></a><span data-ttu-id="8f910-129">サンプルを実行する</span><span class="sxs-lookup"><span data-stu-id="8f910-129">Run the sample</span></span>

<span data-ttu-id="8f910-130">Android Studio で、[**実行**] メニューの [ **' app ' の実行**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="8f910-130">In Android Studio, choose **Run 'app'** on the **Run** menu.</span></span>