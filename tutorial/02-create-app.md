<!-- markdownlint-disable MD002 MD041 -->

Les applications d’onglets Microsoft teams disposent de plusieurs options pour authentifier l’utilisateur et appeler Microsoft Graph. Dans cet exercice, vous allez implémenter un onglet qui effectue une [authentification unique](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso) pour obtenir un jeton d’authentification sur le client, puis utilise le flux de la [part de](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) sur le serveur pour échanger ce jeton afin d’obtenir l’accès à Microsoft Graph.

Pour d’autres alternatives, consultez les rubriques suivantes.

- [Créer un onglet Microsoft teams à l’aide de Microsoft Graph Toolkit](/graph/toolkit/get-started/build-a-microsoft-teams-tab). Cet exemple est entièrement côté client et utilise Microsoft Graph Toolkit pour gérer l’authentification et effectuer des appels à Microsoft Graph.
- [Exemple d’authentification Microsoft teams](https://github.com/OfficeDev/microsoft-teams-sample-auth-node). Cet exemple contient plusieurs exemples qui couvrent différents scénarios d’authentification.

## <a name="create-the-project"></a>Créez le projet

Commencez par créer une application Web ASP.NET principale.

1. Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet. Exécutez la commande suivante.

    ```Shell
    dotnet new webapp -o GraphTutorial
    ```

1. Une fois le projet créé, vérifiez qu’il fonctionne en remplaçant le répertoire actuel par le répertoire **GraphTutorial** et en exécutant la commande suivante dans votre CLI.

    ```Shell
    dotnet run
    ```

1. Ouvrez votre navigateur et accédez à `https://localhost:5001` . Si tout fonctionne, vous devez voir une page principale de ASP.NET par défaut.

> [!IMPORTANT]
> Si vous recevez un avertissement indiquant que le certificat de **localhost** n’est pas approuvé, vous pouvez utiliser l’infrastructure CLI .net pour installer et approuver le certificat de développement. Pour obtenir des instructions sur des systèmes d’exploitation spécifiques, voir [Enforce https in ASP.net Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) .

## <a name="add-nuget-packages"></a>Ajouter des packages NuGet

Avant de poursuivre, installez des packages NuGet supplémentaires que vous utiliserez plus tard.

- [Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) pour l’authentification et la demande de jetons d’accès.
- [Microsoft. Identity. Web. MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph/) pour l’ajout de la prise en charge de Microsoft Graph configurée avec Microsoft. Identity. Web.
- [Microsoft. Graph](https://www.nuget.org/packages/Microsoft.Graph/) pour mettre à jour la version de ce package installé par Microsoft. Identity. Web. MicrosoftGraph.
- [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) pour activer les convertisseurs JSON Newtonsoft pour la compatibilité avec le kit de développement logiciel (SDK) Microsoft Graph.
- [TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) pour la traduction des identificateurs de fuseau horaire Windows en identificateurs IANA.

1. Exécutez les commandes suivantes dans votre interface CLI pour installer les dépendances.

    ```Shell
    dotnet add package Microsoft.Identity.Web --version 1.1.0
    dotnet add package Microsoft.Identity.Web.MicrosoftGraph --version 1.1.0
    dotnet add package Microsoft.Graph --version 3.16.0
    dotnet add package Microsoft.AspNetCore.Mvc.NewtonsoftJson
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a>Concevoir l’application

Dans cette section, vous allez créer la structure de base de l’interface utilisateur de l’application.

> [!TIP]
> Vous pouvez utiliser n’importe quel éditeur de texte pour modifier les fichiers sources de ce didacticiel. Toutefois, [Visual Studio code](https://code.visualstudio.com/) fournit des fonctionnalités supplémentaires, telles que le débogage et IntelliSense.

1. Ouvrez **./Pages/Shared/_Layout. cshtml** et remplacez l’intégralité de son contenu par le code suivant pour mettre à jour la disposition globale de l’application.

    :::code language="cshtml" source="../demo/GraphTutorial/Pages/Shared/_Layout.cshtml" id="LayoutSnippet":::

    Cela remplace le bootstrap avec l' [interface utilisateur Fluent](https://developer.microsoft.com/fluentui), ajoute le [Kit de développement logiciel Microsoft teams](/javascript/api/overview/msteams-client)et simplifie la mise en page.

1. Ouvrez **/wwwroot/js/site.js** et ajoutez le code suivant.

    :::code language="javascript" source="../demo/GraphTutorial/wwwroot/js/site.js" id="ThemeSupportSnippet":::

    Cela ajoute un gestionnaire simple de modification de thème pour modifier la couleur du texte par défaut pour les thèmes sombres et à contraste élevé.

1. Ouvrez **./wwwroot/CSS/site.CSS** et remplacez son contenu par ce qui suit.

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/site.css" id="CssSnippet":::

1. Ouvrez **./pages/index.cshtml** et remplacez son contenu par le code suivant.

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

1. Ouvrez **./Startup.cs** et supprimez la `app.UseHttpsRedirection();` ligne dans la `Configure` méthode. Cela est nécessaire pour que le tunneling ngrok fonctionne.

## <a name="run-ngrok"></a>Exécuter ngrok

Microsoft Teams ne prend pas en charge l’hébergement local pour les applications. Le serveur qui héberge votre application doit être disponible dans le nuage à l’aide de points de terminaison HTTPs. Pour le débogage local, vous pouvez utiliser ngrok pour créer une URL publique pour votre projet hébergé localement.

1. Ouvrez votre interface CLI et exécutez la commande suivante pour démarrer ngrok.

    ```Shell
    ngrok http 5000
    ```

1. Une fois le ngrok démarré, copiez l’URL de transfert HTTPs. Elle doit ressembler à `https://50153897dd4d.ngrok.io` . Vous en aurez besoin plus tard.

> [!IMPORTANT]
> Si vous utilisez la version gratuite d’ngrok, l’URL de transfert change chaque fois que vous redémarrez ngrok. Il est recommandé de laisser ngrok en cours d’exécution jusqu’à ce que vous ayez terminé ce didacticiel pour conserver la même URL. Si vous devez redémarrer ngrok, vous devez mettre à jour votre URL partout où elle est utilisée et réinstaller l’application dans Microsoft Teams.
