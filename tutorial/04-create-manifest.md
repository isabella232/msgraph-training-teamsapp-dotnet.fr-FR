<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4d897-101">Le manifeste de l’application décrit la façon dont l’application s’intègre à Microsoft teams et est nécessaire pour installer des applications.</span><span class="sxs-lookup"><span data-stu-id="4d897-101">The app manifest describes how the app integrates with Microsoft Teams and is required to install apps.</span></span> <span data-ttu-id="4d897-102">Dans cette section, vous allez utiliser app Studio dans le client Microsoft teams pour générer un manifeste.</span><span class="sxs-lookup"><span data-stu-id="4d897-102">In this section you'll use App Studio in the Microsoft Teams client to generate a manifest.</span></span>

1. <span data-ttu-id="4d897-103">Si vous n’avez pas encore installé App Studio dans Teams, [Installez-le maintenant](/microsoftteams/platform/concepts/build-and-test/app-studio-overview).</span><span class="sxs-lookup"><span data-stu-id="4d897-103">If you do not already have App Studio installed in Teams, [install it now](/microsoftteams/platform/concepts/build-and-test/app-studio-overview).</span></span>

1. <span data-ttu-id="4d897-104">Lancez App Studio dans Microsoft teams et sélectionnez l' **éditeur de manifeste**.</span><span class="sxs-lookup"><span data-stu-id="4d897-104">Launch App Studio in Microsoft Teams and select the **Manifest editor**.</span></span>

1. <span data-ttu-id="4d897-105">Sélectionnez **créer une application**.</span><span class="sxs-lookup"><span data-stu-id="4d897-105">Select **Create a new app**.</span></span>

    ![Capture d’écran de l’éditeur de manifeste dans App Studio dans Microsoft teams](images/app-studio-01.png)

1. <span data-ttu-id="4d897-107">Sur la page Détails de l' **application** , renseignez les champs obligatoires.</span><span class="sxs-lookup"><span data-stu-id="4d897-107">On the **App details** page, fill in the required fields.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4d897-108">Vous pouvez utiliser les icônes par défaut dans la section **personnalisation** ou télécharger les vôtres.</span><span class="sxs-lookup"><span data-stu-id="4d897-108">You can use the default icons in the **Branding** section or upload your own.</span></span>

1. <span data-ttu-id="4d897-109">Dans le menu de gauche, sélectionnez **tabulations** sous **fonctionnalités**.</span><span class="sxs-lookup"><span data-stu-id="4d897-109">On the left-hand menu, select **Tabs** under **Capabilities**.</span></span>

1. <span data-ttu-id="4d897-110">Sélectionnez **Ajouter** sous **Ajouter un onglet personnel**.</span><span class="sxs-lookup"><span data-stu-id="4d897-110">Select **Add** under **Add a personal tab**.</span></span>

    ![Capture d’écran de la page onglets dans App Studio](images/app-studio-02.png)

1. <span data-ttu-id="4d897-112">Renseignez les champs comme suit, où `YOUR_NGROK_URL` est l’URL de transfert que vous avez copiée dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="4d897-112">Fill in the fields as follows, where `YOUR_NGROK_URL` is the forwarding URL you copied in the previous section.</span></span> <span data-ttu-id="4d897-113">Sélectionnez **Enregistrer** lorsque vous avez fini.</span><span class="sxs-lookup"><span data-stu-id="4d897-113">Select **Save** when done.</span></span>

    - <span data-ttu-id="4d897-114">**Nom :**`Create event`</span><span class="sxs-lookup"><span data-stu-id="4d897-114">**Name:** `Create event`</span></span>
    - <span data-ttu-id="4d897-115">**ID d’entité :**`createEventTab`</span><span class="sxs-lookup"><span data-stu-id="4d897-115">**Entity ID:** `createEventTab`</span></span>
    - <span data-ttu-id="4d897-116">**URL du contenu :**`YOUR_NGROK_URL/newevent`</span><span class="sxs-lookup"><span data-stu-id="4d897-116">**Content URL:** `YOUR_NGROK_URL/newevent`</span></span>

1. <span data-ttu-id="4d897-117">Sélectionnez **Ajouter** sous **Ajouter un onglet personnel**.</span><span class="sxs-lookup"><span data-stu-id="4d897-117">Select **Add** under **Add a personal tab**.</span></span>

1. <span data-ttu-id="4d897-118">Renseignez les champs comme suit, où `YOUR_NGROK_URL` est l’URL de transfert que vous avez copiée dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="4d897-118">Fill in the fields as follows, where `YOUR_NGROK_URL` is the forwarding URL you copied in the previous section.</span></span> <span data-ttu-id="4d897-119">Sélectionnez **Enregistrer** lorsque vous avez fini.</span><span class="sxs-lookup"><span data-stu-id="4d897-119">Select **Save** when done.</span></span>

    - <span data-ttu-id="4d897-120">**Nom :**`Graph calendar`</span><span class="sxs-lookup"><span data-stu-id="4d897-120">**Name:** `Graph calendar`</span></span>
    - <span data-ttu-id="4d897-121">**ID d’entité :**`calendarTab`</span><span class="sxs-lookup"><span data-stu-id="4d897-121">**Entity ID:** `calendarTab`</span></span>
    - <span data-ttu-id="4d897-122">**URL du contenu :**`YOUR_NGROK_URL`</span><span class="sxs-lookup"><span data-stu-id="4d897-122">**Content URL:** `YOUR_NGROK_URL`</span></span>

1. <span data-ttu-id="4d897-123">Dans le menu de gauche, sélectionnez **domaines et autorisations** sous **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="4d897-123">On the left-hand menu, select **Domains and permissions** under **Finish**.</span></span>

1. <span data-ttu-id="4d897-124">Définissez l' **ID** de l’application AAD sur l’ID de l’application à partir de l’inscription de votre application.</span><span class="sxs-lookup"><span data-stu-id="4d897-124">Set the **AAD App ID** to the application ID from your app registration.</span></span>

1. <span data-ttu-id="4d897-125">Définissez le champ d' **authentification unique** sur l’URI de l’ID de l’application à partir de votre inscription de l’application.</span><span class="sxs-lookup"><span data-stu-id="4d897-125">Set the **Single-Sign-On** field to the application ID URI from your app registration.</span></span>

1. <span data-ttu-id="4d897-126">Dans le menu de gauche, sélectionnez **test et distribuer** sous **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="4d897-126">On the left-hand menu, select **Test and distribute** under **Finish**.</span></span> <span data-ttu-id="4d897-127">Sélectionnez **Télécharger**.</span><span class="sxs-lookup"><span data-stu-id="4d897-127">Select **Download**.</span></span>

1. <span data-ttu-id="4d897-128">Créez un répertoire dans la racine du projet nommé **Manifest**.</span><span class="sxs-lookup"><span data-stu-id="4d897-128">Create a new directory in the root of the project named **Manifest**.</span></span> <span data-ttu-id="4d897-129">Extrayez le contenu du fichier ZIP téléchargé dans ce répertoire.</span><span class="sxs-lookup"><span data-stu-id="4d897-129">Extract the contents of the downloaded ZIP file to this directory.</span></span>
