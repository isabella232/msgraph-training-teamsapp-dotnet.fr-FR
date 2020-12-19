<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a364e-101">Les applications d’onglets Microsoft teams disposent de plusieurs options pour authentifier l’utilisateur et appeler Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="a364e-101">Microsoft Teams tab applications have multiple options to authenticate the user and call Microsoft Graph.</span></span> <span data-ttu-id="a364e-102">Dans cet exercice, vous allez implémenter un onglet qui effectue une [authentification unique](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso) pour obtenir un jeton d’authentification sur le client, puis utilise le flux de la [part de](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) sur le serveur pour échanger ce jeton afin d’obtenir l’accès à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="a364e-102">In this exercise, you'll implement a tab that does [single sign-on](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso) to get an auth token on the client, then uses the [on-behalf-of flow](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) on the server to exchange that token to get access to Microsoft Graph.</span></span>

<span data-ttu-id="a364e-103">Pour d’autres alternatives, consultez les rubriques suivantes.</span><span class="sxs-lookup"><span data-stu-id="a364e-103">For other alternatives, see the following.</span></span>

- <span data-ttu-id="a364e-104">[Créer un onglet Microsoft teams à l’aide de Microsoft Graph Toolkit](/graph/toolkit/get-started/build-a-microsoft-teams-tab).</span><span class="sxs-lookup"><span data-stu-id="a364e-104">[Build a Microsoft Teams tab with the Microsoft Graph Toolkit](/graph/toolkit/get-started/build-a-microsoft-teams-tab).</span></span> <span data-ttu-id="a364e-105">Cet exemple est entièrement côté client et utilise Microsoft Graph Toolkit pour gérer l’authentification et effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="a364e-105">This sample is completely client-side, and uses the Microsoft Graph Toolkit to handle authentication and making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="a364e-106">[Exemple d’authentification Microsoft teams](https://github.com/OfficeDev/microsoft-teams-sample-auth-node).</span><span class="sxs-lookup"><span data-stu-id="a364e-106">[Microsoft Teams Authentication Sample](https://github.com/OfficeDev/microsoft-teams-sample-auth-node).</span></span> <span data-ttu-id="a364e-107">Cet exemple contient plusieurs exemples qui couvrent différents scénarios d’authentification.</span><span class="sxs-lookup"><span data-stu-id="a364e-107">This sample contains multiple examples covering different authentication scenarios.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="a364e-108">Créez le projet</span><span class="sxs-lookup"><span data-stu-id="a364e-108">Create the project</span></span>

<span data-ttu-id="a364e-109">Commencez par créer une application Web ASP.NET principale.</span><span class="sxs-lookup"><span data-stu-id="a364e-109">Start by creating an ASP.NET Core web app.</span></span>

1. <span data-ttu-id="a364e-110">Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet.</span><span class="sxs-lookup"><span data-stu-id="a364e-110">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="a364e-111">Exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="a364e-111">Run the following command.</span></span>

    ```Shell
    dotnet new webapp -o GraphTutorial
    ```

1. <span data-ttu-id="a364e-112">Une fois le projet créé, vérifiez qu’il fonctionne en remplaçant le répertoire actuel par le répertoire **GraphTutorial** et en exécutant la commande suivante dans votre CLI.</span><span class="sxs-lookup"><span data-stu-id="a364e-112">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

1. <span data-ttu-id="a364e-113">Ouvrez votre navigateur et accédez à `https://localhost:5001` .</span><span class="sxs-lookup"><span data-stu-id="a364e-113">Open your browser and browse to `https://localhost:5001`.</span></span> <span data-ttu-id="a364e-114">Si tout fonctionne, vous devez voir une page principale de ASP.NET par défaut.</span><span class="sxs-lookup"><span data-stu-id="a364e-114">If everything is working, you should see a default ASP.NET Core page.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a364e-115">Si vous recevez un avertissement indiquant que le certificat de **localhost** n’est pas approuvé, vous pouvez utiliser l’infrastructure CLI .net pour installer et approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="a364e-115">If you receive a warning that the certificate for **localhost** is un-trusted you can use the .NET Core CLI to install and trust the development certificate.</span></span> <span data-ttu-id="a364e-116">Pour obtenir des instructions sur des systèmes d’exploitation spécifiques, voir [Enforce https in ASP.net Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) .</span><span class="sxs-lookup"><span data-stu-id="a364e-116">See [Enforce HTTPS in ASP.NET Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) for instructions for specific operating systems.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="a364e-117">Ajouter des packages NuGet</span><span class="sxs-lookup"><span data-stu-id="a364e-117">Add NuGet packages</span></span>

<span data-ttu-id="a364e-118">Avant de poursuivre, installez des packages NuGet supplémentaires que vous utiliserez plus tard.</span><span class="sxs-lookup"><span data-stu-id="a364e-118">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="a364e-119">[Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) pour l’authentification et la demande de jetons d’accès.</span><span class="sxs-lookup"><span data-stu-id="a364e-119">[Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) for authenticating and requesting access tokens.</span></span>
- <span data-ttu-id="a364e-120">[Microsoft. Identity. Web. MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph/) pour l’ajout de la prise en charge de Microsoft Graph configurée avec Microsoft. Identity. Web.</span><span class="sxs-lookup"><span data-stu-id="a364e-120">[Microsoft.Identity.Web.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph/) for adding Microsoft Graph support configured with Microsoft.Identity.Web.</span></span>
- <span data-ttu-id="a364e-121">[Microsoft. Graph](https://www.nuget.org/packages/Microsoft.Graph/) pour mettre à jour la version de ce package installé par Microsoft. Identity. Web. MicrosoftGraph.</span><span class="sxs-lookup"><span data-stu-id="a364e-121">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) to update the version of this package installed by Microsoft.Identity.Web.MicrosoftGraph.</span></span>
- <span data-ttu-id="a364e-122">[Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) pour activer les convertisseurs JSON Newtonsoft pour la compatibilité avec le kit de développement logiciel (SDK) Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="a364e-122">[Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) to enable Newtonsoft JSON converters for compatibility with the Microsoft Graph SDK.</span></span>
- <span data-ttu-id="a364e-123">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) pour la traduction des identificateurs de fuseau horaire Windows en identificateurs IANA.</span><span class="sxs-lookup"><span data-stu-id="a364e-123">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

1. <span data-ttu-id="a364e-124">Exécutez les commandes suivantes dans votre interface CLI pour installer les dépendances.</span><span class="sxs-lookup"><span data-stu-id="a364e-124">Run the following commands in your CLI to install the dependencies.</span></span>

    ```Shell
    dotnet add package Microsoft.Identity.Web --version 1.1.0
    dotnet add package Microsoft.Identity.Web.MicrosoftGraph --version 1.1.0
    dotnet add package Microsoft.Graph --version 3.16.0
    dotnet add package Microsoft.AspNetCore.Mvc.NewtonsoftJson
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a><span data-ttu-id="a364e-125">Concevoir l’application</span><span class="sxs-lookup"><span data-stu-id="a364e-125">Design the app</span></span>

<span data-ttu-id="a364e-126">Dans cette section, vous allez créer la structure de base de l’interface utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="a364e-126">In this section you will create the basic UI structure of the application.</span></span>

> [!TIP]
> <span data-ttu-id="a364e-127">Vous pouvez utiliser n’importe quel éditeur de texte pour modifier les fichiers sources de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="a364e-127">You can use any text editor to edit the source files for this tutorial.</span></span> <span data-ttu-id="a364e-128">Toutefois, [Visual Studio code](https://code.visualstudio.com/) fournit des fonctionnalités supplémentaires, telles que le débogage et IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="a364e-128">However, [Visual Studio Code](https://code.visualstudio.com/) provides additional features, such as debugging and Intellisense.</span></span>

1. <span data-ttu-id="a364e-129">Ouvrez **./Pages/Shared/_Layout. cshtml** et remplacez l’intégralité de son contenu par le code suivant pour mettre à jour la disposition globale de l’application.</span><span class="sxs-lookup"><span data-stu-id="a364e-129">Open **./Pages/Shared/_Layout.cshtml** and replace its entire contents with the following code to update the global layout of the app.</span></span>

    :::code language="cshtml" source="../demo/GraphTutorial/Pages/Shared/_Layout.cshtml" id="LayoutSnippet":::

    <span data-ttu-id="a364e-130">Cela remplace le bootstrap avec l' [interface utilisateur Fluent](https://developer.microsoft.com/fluentui), ajoute le [Kit de développement logiciel Microsoft teams](/javascript/api/overview/msteams-client)et simplifie la mise en page.</span><span class="sxs-lookup"><span data-stu-id="a364e-130">This replaces Bootstrap with [Fluent UI](https://developer.microsoft.com/fluentui), adds the [Microsoft Teams SDK](/javascript/api/overview/msteams-client), and simplifies the layout.</span></span>

1. <span data-ttu-id="a364e-131">Ouvrez **/wwwroot/js/site.js** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="a364e-131">Open **./wwwroot/js/site.js** and add the following code.</span></span>

    :::code language="javascript" source="../demo/GraphTutorial/wwwroot/js/site.js" id="ThemeSupportSnippet":::

    <span data-ttu-id="a364e-132">Cela ajoute un gestionnaire simple de modification de thème pour modifier la couleur du texte par défaut pour les thèmes sombres et à contraste élevé.</span><span class="sxs-lookup"><span data-stu-id="a364e-132">This adds a simple theme change handler to change the default text color for dark and high contrast themes.</span></span>

1. <span data-ttu-id="a364e-133">Ouvrez **./wwwroot/CSS/site.CSS** et remplacez son contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="a364e-133">Open **./wwwroot/css/site.css** and replace its contents with the following.</span></span>

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/site.css" id="CssSnippet":::

1. <span data-ttu-id="a364e-134">Ouvrez **./pages/index.cshtml** et remplacez son contenu par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="a364e-134">Open **./Pages/Index.cshtml** and replace its contents with the following code.</span></span>

    ```cshtml
    @page
    @model IndexModel
    @{
      ViewData["Title"] = "Home page";
    }

    <div id="tab-container">
      <h1 class="ms-fontSize-24 ms-fontWeight-semibold">Loading...</h1>
    </div>

    @section Scripts
    {
      <script>
      </script>
    }
    ```

1. <span data-ttu-id="a364e-135">Ouvrez **./Startup.cs** et supprimez la `app.UseHttpsRedirection();` ligne dans la `Configure` méthode.</span><span class="sxs-lookup"><span data-stu-id="a364e-135">Open **./Startup.cs** and remove the `app.UseHttpsRedirection();` line in the `Configure` method.</span></span> <span data-ttu-id="a364e-136">Cela est nécessaire pour que le tunneling ngrok fonctionne.</span><span class="sxs-lookup"><span data-stu-id="a364e-136">This is necessary for ngrok tunneling to work.</span></span>

## <a name="run-ngrok"></a><span data-ttu-id="a364e-137">Exécuter ngrok</span><span class="sxs-lookup"><span data-stu-id="a364e-137">Run ngrok</span></span>

<span data-ttu-id="a364e-138">Microsoft Teams ne prend pas en charge l’hébergement local pour les applications.</span><span class="sxs-lookup"><span data-stu-id="a364e-138">Microsoft Teams does not support local hosting for apps.</span></span> <span data-ttu-id="a364e-139">Le serveur qui héberge votre application doit être disponible dans le nuage à l’aide de points de terminaison HTTPs.</span><span class="sxs-lookup"><span data-stu-id="a364e-139">The server hosting your app must be available from the cloud using HTTPS endpoints.</span></span> <span data-ttu-id="a364e-140">Pour le débogage local, vous pouvez utiliser ngrok pour créer une URL publique pour votre projet hébergé localement.</span><span class="sxs-lookup"><span data-stu-id="a364e-140">For debugging locally, you can use ngrok to create a public URL for your locally-hosted project.</span></span>

1. <span data-ttu-id="a364e-141">Ouvrez votre interface CLI et exécutez la commande suivante pour démarrer ngrok.</span><span class="sxs-lookup"><span data-stu-id="a364e-141">Open your CLI and run the following command to start ngrok.</span></span>

    ```Shell
    ngrok http 5000
    ```

1. <span data-ttu-id="a364e-142">Une fois le ngrok démarré, copiez l’URL de transfert HTTPs.</span><span class="sxs-lookup"><span data-stu-id="a364e-142">Once ngrok starts, copy the HTTPS Forwarding URL.</span></span> <span data-ttu-id="a364e-143">Elle doit ressembler à `https://50153897dd4d.ngrok.io` .</span><span class="sxs-lookup"><span data-stu-id="a364e-143">It should look like `https://50153897dd4d.ngrok.io`.</span></span> <span data-ttu-id="a364e-144">Vous en aurez besoin plus tard.</span><span class="sxs-lookup"><span data-stu-id="a364e-144">You'll need this value in later steps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a364e-145">Si vous utilisez la version gratuite d’ngrok, l’URL de transfert change chaque fois que vous redémarrez ngrok.</span><span class="sxs-lookup"><span data-stu-id="a364e-145">If you are using the free version of ngrok, the forwarding URL changes every time you restart ngrok.</span></span> <span data-ttu-id="a364e-146">Il est recommandé de laisser ngrok en cours d’exécution jusqu’à ce que vous ayez terminé ce didacticiel pour conserver la même URL.</span><span class="sxs-lookup"><span data-stu-id="a364e-146">It's recommended that you leave ngrok running until you complete this tutorial to keep the same URL.</span></span> <span data-ttu-id="a364e-147">Si vous devez redémarrer ngrok, vous devez mettre à jour votre URL partout où elle est utilisée et réinstaller l’application dans Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="a364e-147">If you have to restart ngrok, you'll need to update your URL everywhere that it is used and reinstall the app in Microsoft Teams.</span></span>
