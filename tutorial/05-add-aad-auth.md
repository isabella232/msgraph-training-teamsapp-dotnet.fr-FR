<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="cc077-101">Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification unique avec Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc077-101">In this exercise you will extend the application from the previous exercise to support single sign-on authentication with Azure AD.</span></span> <span data-ttu-id="cc077-102">Cette opération est obligatoire pour obtenir le jeton d’accès OAuth nécessaire pour appeler l’API Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="cc077-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph API.</span></span> <span data-ttu-id="cc077-103">Dans cette étape, vous allez configurer la bibliothèque [Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) .</span><span class="sxs-lookup"><span data-stu-id="cc077-103">In this step you will configure the [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) library.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cc077-104">Pour éviter le stockage de l’ID et de la clé secrète de l’application dans la source, utilisez le [Gestionnaire de secret .net](/aspnet/core/security/app-secrets) pour stocker ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="cc077-104">To avoid storing the application ID and secret in source, you will use the [.NET Secret Manager](/aspnet/core/security/app-secrets) to store these values.</span></span> <span data-ttu-id="cc077-105">Le gestionnaire de secrets est destiné uniquement à des fins de développement, les applications de production doivent utiliser un gestionnaire de secret approuvé pour le stockage de secrets.</span><span class="sxs-lookup"><span data-stu-id="cc077-105">The Secret Manager is for development purposes only, production apps should use a trusted secret manager for storing secrets.</span></span>

1. <span data-ttu-id="cc077-106">Ouvrez **./appsettings.jssur** et remplacez son contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="cc077-106">Open **./appsettings.json** and replace its contents with the following.</span></span>

    :::code language="json" source="../demo/GraphTutorial/appsettings.example.json" highlight="2-8":::

1. <span data-ttu-id="cc077-107">Ouvrez la CLI dans le répertoire où se trouve **GraphTutorial. csproj** , puis exécutez les commandes suivantes, en remplaçant `YOUR_APP_ID` votre ID d’application dans le portail Azure, et `YOUR_APP_SECRET` votre clé d’application secrète.</span><span class="sxs-lookup"><span data-stu-id="cc077-107">Open your CLI in the directory where **GraphTutorial.csproj** is located, and run the following commands, substituting `YOUR_APP_ID` with your application ID from the Azure portal, and `YOUR_APP_SECRET` with your application secret.</span></span>

    ```Shell
    dotnet user-secrets init
    dotnet user-secrets set "AzureAd:ClientId" "YOUR_APP_ID"
    dotnet user-secrets set "AzureAd:ClientSecret" "YOUR_APP_SECRET"
    ```

## <a name="implement-sign-in"></a><span data-ttu-id="cc077-108">Implémentation de la connexion</span><span class="sxs-lookup"><span data-stu-id="cc077-108">Implement sign-in</span></span>

<span data-ttu-id="cc077-109">Tout d’abord, implémentez l’authentification unique dans le code JavaScript de l’application.</span><span class="sxs-lookup"><span data-stu-id="cc077-109">First, implement single sign-on in the app's JavaScript code.</span></span> <span data-ttu-id="cc077-110">Vous allez utiliser le [Kit de développement logiciel (SDK) Microsoft teams JavaScript](/javascript/api/overview/msteams-client) pour obtenir un jeton d’accès qui permet au code JavaScript exécuté dans le client teams d’effectuer des appels AJAX vers l’API Web que vous allez implémenter ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="cc077-110">You will use the [Microsoft Teams JavaScript SDK](/javascript/api/overview/msteams-client) to get an access token which allows the JavaScript code running in the Teams client to make AJAX calls to Web API you will implement later.</span></span>

1. <span data-ttu-id="cc077-111">Ouvrez **./pages/index.cshtml** et ajoutez le code suivant à l’intérieur de la `<script>` balise.</span><span class="sxs-lookup"><span data-stu-id="cc077-111">Open **./Pages/Index.cshtml** and add the following code inside the `<script>` tag.</span></span>

    ```javascript
    (function () {
      if (microsoftTeams) {
        microsoftTeams.initialize();

        microsoftTeams.authentication.getAuthToken({
          successCallback: (token) => {
            // TEMPORARY: Display the access token for debugging
            $('#tab-container').empty();

            $('<code/>', {
              text: token,
              style: 'word-break: break-all;'
            }).appendTo('#tab-container');
          },
          failureCallback: (error) => {
            renderError(error);
          }
        });
      }
    })();

    function renderError(error) {
      $('#tab-container').empty();

      $('<h1/>', {
        text: 'Error'
      }).appendTo('#tab-container');

      $('<code/>', {
        text: JSON.stringify(error, Object.getOwnPropertyNames(error)),
        style: 'word-break: break-all;'
      }).appendTo('#tab-container');
    }
    ```

    <span data-ttu-id="cc077-112">Cela appelle le `microsoftTeams.authentication.getAuthToken` pour s’authentifier silencieusement en tant qu’utilisateur connecté à Teams.</span><span class="sxs-lookup"><span data-stu-id="cc077-112">This calls the `microsoftTeams.authentication.getAuthToken` to silently authenticate as the user that is signed in to Teams.</span></span> <span data-ttu-id="cc077-113">Il n’existe généralement aucune invite d’interface utilisateur, sauf si l’utilisateur doit consentir.</span><span class="sxs-lookup"><span data-stu-id="cc077-113">There is typically not any UI prompts involved, unless the user has to consent.</span></span> <span data-ttu-id="cc077-114">Le code affiche ensuite le jeton sous l’onglet.</span><span class="sxs-lookup"><span data-stu-id="cc077-114">The code then displays the token in the tab.</span></span>

1. <span data-ttu-id="cc077-115">Enregistrez vos modifications et démarrez votre application en exécutant la commande suivante dans votre interface CLI.</span><span class="sxs-lookup"><span data-stu-id="cc077-115">Save your changes and start your application by running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="cc077-116">Si vous avez redémarré ngrok et que votre URL ngrok a changé, veillez à mettre à jour la valeur ngrok à l’emplacement suivant **avant** de procéder au test.</span><span class="sxs-lookup"><span data-stu-id="cc077-116">If you have restarted ngrok and your ngrok URL has changed, be sure to update the ngrok value in the following place **before** you test.</span></span>
    >
    > - <span data-ttu-id="cc077-117">L’URI de redirection dans l’inscription de votre application</span><span class="sxs-lookup"><span data-stu-id="cc077-117">The redirect URI in your app registration</span></span>
    > - <span data-ttu-id="cc077-118">URI de l’ID d’application dans l’inscription de votre application</span><span class="sxs-lookup"><span data-stu-id="cc077-118">The application ID URI in your app registration</span></span>
    > - <span data-ttu-id="cc077-119">`contentUrl` dans manifest.jssur</span><span class="sxs-lookup"><span data-stu-id="cc077-119">`contentUrl` in manifest.json</span></span>
    > - <span data-ttu-id="cc077-120">`validDomains` dans manifest.jssur</span><span class="sxs-lookup"><span data-stu-id="cc077-120">`validDomains` in manifest.json</span></span>
    > - <span data-ttu-id="cc077-121">`resource` dans manifest.jssur</span><span class="sxs-lookup"><span data-stu-id="cc077-121">`resource` in manifest.json</span></span>

1. <span data-ttu-id="cc077-122">Créez un fichier ZIP avec **manifest.js**, **color.png** et **outline.png**.</span><span class="sxs-lookup"><span data-stu-id="cc077-122">Create a ZIP file with **manifest.json**, **color.png**, and **outline.png**.</span></span>

1. <span data-ttu-id="cc077-123">Dans Microsoft Teams, sélectionnez **applications** dans la barre de gauche, sélectionnez **Télécharger une application personnalisée**, puis **Télécharger pour moi ou My teams**.</span><span class="sxs-lookup"><span data-stu-id="cc077-123">In Microsoft Teams, select **Apps** in the left-hand bar, select **Upload a custom app**, then select **Upload for me or my teams**.</span></span>

    ![Capture d’écran du lien Télécharger une application personnalisée dans Microsoft teams](images/upload-custom-app.png)

1. <span data-ttu-id="cc077-125">Naviguez jusqu’au fichier ZIP que vous avez créé précédemment, puis sélectionnez **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="cc077-125">Browse to the ZIP file you created previously and select **Open**.</span></span>

1. <span data-ttu-id="cc077-126">Passez en revue les informations relatives à l’application, puis sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="cc077-126">Review the application information and select **Add**.</span></span>

1. <span data-ttu-id="cc077-127">L’application s’ouvre dans teams et affiche un jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="cc077-127">The application opens in Teams and displays an access token.</span></span>

<span data-ttu-id="cc077-128">Si vous copiez le jeton, vous pouvez le coller dans [JWT.ms](https://jwt.ms).</span><span class="sxs-lookup"><span data-stu-id="cc077-128">If you copy the token, you can paste it into [jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="cc077-129">Vérifiez que l’audience (la `aud` revendication) est l’ID de votre application et que la seule portée (la `scp` revendication) est l’étendue de l' `access_as_user` API que vous avez créée.</span><span class="sxs-lookup"><span data-stu-id="cc077-129">Verify that the audience (the `aud` claim) is your application ID, and the only scope (the `scp` claim) is the `access_as_user` API scope you created.</span></span> <span data-ttu-id="cc077-130">Cela signifie que ce jeton n’accorde pas d’accès direct à Microsoft Graph !</span><span class="sxs-lookup"><span data-stu-id="cc077-130">That means that this token does not grant direct access to Microsoft Graph!</span></span> <span data-ttu-id="cc077-131">Au lieu de cela, l’API Web que vous implémenterez bientôt devra échanger ce jeton à l’aide du flux de la [part de](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) pour obtenir un jeton qui fonctionnera avec les appels Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="cc077-131">Instead, the Web API you will implement soon will need to exchange this token using the [on-behalf-of flow](/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) to get a token that will work with Microsoft Graph calls.</span></span>

## <a name="configure-authentication-in-the-aspnet-core-app"></a><span data-ttu-id="cc077-132">Configurer l’authentification dans l’application principale de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cc077-132">Configure authentication in the ASP.NET Core app</span></span>

<span data-ttu-id="cc077-133">Commencez par ajouter les services de plateforme d’identité Microsoft à l’application.</span><span class="sxs-lookup"><span data-stu-id="cc077-133">Start by adding the Microsoft Identity platform services to the application.</span></span>

1. <span data-ttu-id="cc077-134">Ouvrez le fichier **./Startup.cs** et ajoutez l' `using` instruction suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="cc077-134">Open the **./Startup.cs** file and add the following `using` statement to the top of the file.</span></span>

    ```csharp
    using Microsoft.Identity.Web;
    ```

1. <span data-ttu-id="cc077-135">Ajoutez la ligne suivante juste avant la `app.UseAuthorization();` ligne dans la `Configure` fonction.</span><span class="sxs-lookup"><span data-stu-id="cc077-135">Add the following line just before the `app.UseAuthorization();` line in the `Configure` function.</span></span>

    ```csharp
    app.UseAuthentication();
    ```

1. <span data-ttu-id="cc077-136">Ajoutez la ligne suivante juste après la `endpoints.MapRazorPages();` ligne dans la `Configure` fonction.</span><span class="sxs-lookup"><span data-stu-id="cc077-136">Add the following line just after the `endpoints.MapRazorPages();` line in the `Configure` function.</span></span>

    ```csharp
    endpoints.MapControllers();
    ```

1. <span data-ttu-id="cc077-137">Remplacez la fonction `ConfigureServices` existante par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="cc077-137">Replace the existing `ConfigureServices` function with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Startup.cs" id="ConfigureServicesSnippet":::

    <span data-ttu-id="cc077-138">Ce code configure l’application pour permettre aux appels aux API Web d’être authentifiés en fonction du jeton du porteur JWT dans l' `Authorization` en-tête.</span><span class="sxs-lookup"><span data-stu-id="cc077-138">This code configures the application to allow calls to Web APIs to be authenticated based on the JWT bearer token in the `Authorization` header.</span></span> <span data-ttu-id="cc077-139">Il ajoute également les services d’acquisition de jetons qui peuvent échanger ce jeton via le flux de la part de.</span><span class="sxs-lookup"><span data-stu-id="cc077-139">It also adds the token acquisition services that can exchange that token via the on-behalf-of flow.</span></span>

## <a name="create-the-web-api-controller"></a><span data-ttu-id="cc077-140">Créer le contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="cc077-140">Create the Web API controller</span></span>

1. <span data-ttu-id="cc077-141">Créez un répertoire dans la racine du projet nommé **contrôleurs**.</span><span class="sxs-lookup"><span data-stu-id="cc077-141">Create a new directory in the root of the project named **Controllers**.</span></span>

1. <span data-ttu-id="cc077-142">Créez un fichier dans le répertoire **./Controllers** nommé **CalendarController.cs** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="cc077-142">Create a new file in the **./Controllers** directory named **CalendarController.cs** and add the following code.</span></span>

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Net;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Authorization;
    using Microsoft.AspNetCore.Http;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Logging;
    using Microsoft.Identity.Web;
    using Microsoft.Identity.Web.Resource;
    using Microsoft.Graph;
    using TimeZoneConverter;

    namespace GraphTutorial.Controllers
    {
        [ApiController]
        [Route("[controller]")]
        [Authorize]
        public class CalendarController : ControllerBase
        {
            private static readonly string[] apiScopes = new[] { "access_as_user" };

            private readonly GraphServiceClient _graphClient;
            private readonly ITokenAcquisition _tokenAcquisition;
            private readonly ILogger<CalendarController> _logger;

            public CalendarController(ITokenAcquisition tokenAcquisition, GraphServiceClient graphClient, ILogger<CalendarController> logger)
            {
                _tokenAcquisition = tokenAcquisition;
                _graphClient = graphClient;
                _logger = logger;
            }

            [HttpGet]
            public async Task<string> Get()
            {
                // This verifies that the access_as_user scope is
                // present in the bearer token, throws if not
                HttpContext.VerifyUserHasAnyAcceptedScope(apiScopes);

                // To verify that the identity libraries have authenticated
                // based on the token, log the user's name
                _logger.LogInformation($"Authenticated user: {User.GetDisplayName()}");

                try
                {
                    // TEMPORARY
                    // Get a Graph token via OBO flow
                    var token = await _tokenAcquisition
                        .GetAccessTokenForUserAsync(new[]{
                            "User.Read",
                            "MailboxSettings.Read",
                            "Calendars.ReadWrite" });

                    // Log the token
                    _logger.LogInformation($"Access token for Graph: {token}");
                    return "{ \"status\": \"OK\" }";
                }
                catch (MicrosoftIdentityWebChallengeUserException ex)
                {
                    _logger.LogError(ex, "Consent required");
                    // This exception indicates consent is required.
                    // Return a 403 with "consent_required" in the body
                    // to signal to the tab it needs to prompt for consent
                    HttpContext.Response.ContentType = "text/plain";
                    HttpContext.Response.StatusCode = (int)HttpStatusCode.Forbidden;
                    await HttpContext.Response.WriteAsync("consent_required");
                    return null;
                }
                catch (Exception ex)
                {
                    _logger.LogError(ex, "Error occurred");
                    return null;
                }
            }
        }
    }
    ```

    <span data-ttu-id="cc077-143">Cela implémente une API Web ( `GET /calendar` ) qui peut être appelée à partir de l’onglet Teams. Pour l’instant, il essaie simplement d’échanger le jeton du porteur pour un jeton de graphique.</span><span class="sxs-lookup"><span data-stu-id="cc077-143">This implements a Web API (`GET /calendar`) that can be called from the Teams tab. For now it simply tries to exchange the bearer token for a Graph token.</span></span> <span data-ttu-id="cc077-144">La première fois qu’un utilisateur charge l’onglet, cette opération échoue car elle n’a pas encore accepté d’autoriser l’accès de l’application à Microsoft Graph en son nom.</span><span class="sxs-lookup"><span data-stu-id="cc077-144">The first time a user loads the tab, this will fail because they have not yet consented to allow the app access to Microsoft Graph on their behalf.</span></span>

1. <span data-ttu-id="cc077-145">Ouvrez **./pages/index.cshtml** et remplacez la `successCallback` fonction par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="cc077-145">Open **./Pages/Index.cshtml** and replace the `successCallback` function with the following.</span></span>

    ```javascript
    successCallback: (token) => {
      // TEMPORARY: Call the Web API
      fetch('/calendar', {
        headers: {
          'Authorization': `Bearer ${token}`
        }
      }).then(response => {
        response.text()
          .then(body => {
            $('#tab-container').empty();
            $('<code/>', {
              text: body
            }).appendTo('#tab-container');
          });
      }).catch(error => {
        console.error(error);
        renderError(error);
      });
    }
    ```

    <span data-ttu-id="cc077-146">Cela appelle l’API Web et affiche la réponse.</span><span class="sxs-lookup"><span data-stu-id="cc077-146">This will call the Web API and display the response.</span></span>

1. <span data-ttu-id="cc077-147">Enregistrez vos modifications, puis redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="cc077-147">Save your changes and restart the app.</span></span> <span data-ttu-id="cc077-148">Actualisez l’onglet dans Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="cc077-148">Refresh the tab in Microsoft Teams.</span></span> <span data-ttu-id="cc077-149">La page doit s’afficher `consent_required` .</span><span class="sxs-lookup"><span data-stu-id="cc077-149">The page should display `consent_required`.</span></span>

1. <span data-ttu-id="cc077-150">Examinez la sortie du journal dans votre interface CLI.</span><span class="sxs-lookup"><span data-stu-id="cc077-150">Review the log output in your CLI.</span></span> <span data-ttu-id="cc077-151">Notez deux choses.</span><span class="sxs-lookup"><span data-stu-id="cc077-151">Notice two things.</span></span>

    - <span data-ttu-id="cc077-152">Une entrée comme `Authenticated user: MeganB@contoso.com` .</span><span class="sxs-lookup"><span data-stu-id="cc077-152">An entry like `Authenticated user: MeganB@contoso.com`.</span></span> <span data-ttu-id="cc077-153">L’API Web a authentifié l’utilisateur en fonction du jeton envoyé avec la demande d’API.</span><span class="sxs-lookup"><span data-stu-id="cc077-153">The Web API has authenticated the user based on the token sent with the API request.</span></span>
    - <span data-ttu-id="cc077-154">Une entrée comme `AADSTS65001: The user or administrator has not consented to use the application with ID...` .</span><span class="sxs-lookup"><span data-stu-id="cc077-154">An entry like `AADSTS65001: The user or administrator has not consented to use the application with ID...`.</span></span> <span data-ttu-id="cc077-155">Cela est normal, car l’utilisateur n’a pas encore été invité à accepter les étendues d’autorisation Microsoft Graph demandées.</span><span class="sxs-lookup"><span data-stu-id="cc077-155">This is expected, since the user has not yet been prompted to consent for the requested Microsoft Graph permission scopes.</span></span>

## <a name="implement-consent-prompt"></a><span data-ttu-id="cc077-156">Mettre en œuvre une invite de consentement</span><span class="sxs-lookup"><span data-stu-id="cc077-156">Implement consent prompt</span></span>

<span data-ttu-id="cc077-157">Étant donné que l’API Web ne peut pas inviter l’utilisateur, l’onglet teams doit mettre en œuvre une invite.</span><span class="sxs-lookup"><span data-stu-id="cc077-157">Because the Web API cannot prompt the user, the Teams tab will need to implement a prompt.</span></span> <span data-ttu-id="cc077-158">Cette opération ne doit être exécutée qu’une seule fois pour chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cc077-158">This will only need to be done once for each user.</span></span> <span data-ttu-id="cc077-159">Une fois qu’un utilisateur est accepté, il n’a pas besoin de reverser le consentement, sauf s’il refuse explicitement l’accès à votre application.</span><span class="sxs-lookup"><span data-stu-id="cc077-159">Once a user consents, they do not need to reconsent unless they explicitly revoke access to your application.</span></span>

1. <span data-ttu-id="cc077-160">Créez un fichier dans le répertoire **./pages** nommé **Authenticate.cshtml.cs** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="cc077-160">Create a new file in the **./Pages** directory named **Authenticate.cshtml.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Authenticate.cshtml.cs" id="AuthenticateModelSnippet":::

1. <span data-ttu-id="cc077-161">Créez un fichier dans le répertoire **./pages** nommé **Authenticate. cshtml** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="cc077-161">Create a new file in the **./Pages** directory named **Authenticate.cshtml** and add the following code.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Authenticate.cshtml":::

1. <span data-ttu-id="cc077-162">Créez un fichier dans le répertoire **./pages** nommé **AuthComplete. cshtml** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="cc077-162">Create a new file in the **./Pages** directory named **AuthComplete.cshtml** and add the following code.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/AuthComplete.cshtml":::

1. <span data-ttu-id="cc077-163">Ouvrez **./pages/index.cshtml** et ajoutez les fonctions suivantes à l’intérieur de la `<script>` balise.</span><span class="sxs-lookup"><span data-stu-id="cc077-163">Open **./Pages/Index.cshtml** and add the following functions inside the `<script>` tag.</span></span>

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="LoadUserCalendarSnippet":::

1. <span data-ttu-id="cc077-164">Ajoutez la fonction suivante dans la `<script>` balise pour afficher un résultat réussi de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="cc077-164">Add the following function inside the `<script>` tag to display a successful result from the Web API.</span></span>

    ```javascript
    function renderCalendar(events) {
      $('#tab-container').empty();

      $('<pre/>').append($('<code/>', {
        text: JSON.stringify(events, null, 2),
        style: 'word-break: break-all;'
      })).appendTo('#tab-container');
    }
    ```

1. <span data-ttu-id="cc077-165">Remplacez le `successCallback` code existant par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="cc077-165">Replace the existing `successCallback` with the following code.</span></span>

    ```javascript
    successCallback: (token) => {
      loadUserCalendar(token, (events) => {
        renderCalendar(events);
      });
    }
    ```

1. <span data-ttu-id="cc077-166">Enregistrez vos modifications, puis redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="cc077-166">Save your changes and restart the app.</span></span> <span data-ttu-id="cc077-167">Actualisez l’onglet dans Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="cc077-167">Refresh the tab in Microsoft Teams.</span></span> <span data-ttu-id="cc077-168">L’onglet doit s’afficher `{ "status": "OK" }` .</span><span class="sxs-lookup"><span data-stu-id="cc077-168">The tab should display `{ "status": "OK" }`.</span></span>

1. <span data-ttu-id="cc077-169">Examinez la sortie du journal.</span><span class="sxs-lookup"><span data-stu-id="cc077-169">Review the log output.</span></span> <span data-ttu-id="cc077-170">L’entrée doit apparaître `Access token for Graph` .</span><span class="sxs-lookup"><span data-stu-id="cc077-170">You should see the `Access token for Graph` entry.</span></span> <span data-ttu-id="cc077-171">Si vous analysez ce jeton, vous remarquerez qu’il contient les étendues Microsoft Graph configurées dans **appsettings.jsactivé**.</span><span class="sxs-lookup"><span data-stu-id="cc077-171">If you parse that token, you'll notice that it contains the Microsoft Graph scopes configured in **appsettings.json**.</span></span>

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="cc077-172">Stockage et actualisation des jetons</span><span class="sxs-lookup"><span data-stu-id="cc077-172">Storing and refreshing tokens</span></span>

<span data-ttu-id="cc077-173">À ce stade, votre application a un jeton d’accès, qui est envoyé dans l' `Authorization` en-tête des appels d’API.</span><span class="sxs-lookup"><span data-stu-id="cc077-173">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="cc077-174">Il s’agit du jeton qui permet à l’application d’accéder à Microsoft Graph pour le compte de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cc077-174">This is the token that allows the app to access Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="cc077-175">Cependant, ce jeton est de courte durée.</span><span class="sxs-lookup"><span data-stu-id="cc077-175">However, this token is short-lived.</span></span> <span data-ttu-id="cc077-176">Le jeton expire une heure après son émission.</span><span class="sxs-lookup"><span data-stu-id="cc077-176">The token expires an hour after it is issued.</span></span> <span data-ttu-id="cc077-177">C’est là que le jeton d’actualisation devient utile.</span><span class="sxs-lookup"><span data-stu-id="cc077-177">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="cc077-178">Le jeton d’actualisation permet à l’application de demander un nouveau jeton d’accès sans obliger l’utilisateur à se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="cc077-178">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="cc077-179">Étant donné que l’application utilise la bibliothèque Microsoft. Identity. Web, vous n’avez pas besoin d’implémenter de logique d’actualisation ou de stockage de jetons.</span><span class="sxs-lookup"><span data-stu-id="cc077-179">Because the app is using the Microsoft.Identity.Web library, you do not have to implement any token storage or refresh logic.</span></span>

<span data-ttu-id="cc077-180">L’application utilise le cache de jetons en mémoire, ce qui est suffisant pour les applications qui n’ont pas besoin de faire persister les jetons lors du redémarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="cc077-180">The app uses the in-memory token cache, which is sufficient for apps that do not need to persist tokens when the app restarts.</span></span> <span data-ttu-id="cc077-181">Les applications de production peuvent utiliser à la place les [options de cache distribué](https://github.com/AzureAD/microsoft-identity-web/wiki/token-cache-serialization) dans la bibliothèque Microsoft. Identity. Web.</span><span class="sxs-lookup"><span data-stu-id="cc077-181">Production apps may instead use the [distributed cache options](https://github.com/AzureAD/microsoft-identity-web/wiki/token-cache-serialization) in the Microsoft.Identity.Web library.</span></span>

<span data-ttu-id="cc077-182">La `GetAccessTokenForUserAsync` méthode gère l’expiration du jeton et l’actualisation pour vous.</span><span class="sxs-lookup"><span data-stu-id="cc077-182">The `GetAccessTokenForUserAsync` method handles token expiration and refresh for you.</span></span> <span data-ttu-id="cc077-183">Il vérifie d’abord le jeton mis en cache et, s’il n’a pas expiré, il le renvoie.</span><span class="sxs-lookup"><span data-stu-id="cc077-183">It first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="cc077-184">Si elle a expiré, elle utilise le jeton d’actualisation mis en cache pour en obtenir une nouvelle.</span><span class="sxs-lookup"><span data-stu-id="cc077-184">If it is expired, it uses the cached refresh token to obtain a new one.</span></span>

<span data-ttu-id="cc077-185">La **GraphServiceClient** que les contrôleurs obtiennent via l’injection de dépendances est préconfigurée avec un fournisseur d’authentification qui se sert `GetAccessTokenForUserAsync` pour vous.</span><span class="sxs-lookup"><span data-stu-id="cc077-185">The **GraphServiceClient** that controllers get via dependency injection is pre-configured with an authentication provider that uses `GetAccessTokenForUserAsync` for you.</span></span>
