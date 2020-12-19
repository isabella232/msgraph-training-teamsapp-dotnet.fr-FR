<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="77100-101">Dans cette section, vous allez incorporer Microsoft Graph dans l’application.</span><span class="sxs-lookup"><span data-stu-id="77100-101">In this section you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="77100-102">Pour cette application, vous allez utiliser la [bibliothèque cliente Microsoft Graph pour .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="77100-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

# <a name="get-a-calendar-view"></a><span data-ttu-id="77100-103">Obtenir un affichage Calendrier</span><span class="sxs-lookup"><span data-stu-id="77100-103">Get a calendar view</span></span>

<span data-ttu-id="77100-104">Un affichage Calendrier est un ensemble d’événements provenant du calendrier de l’utilisateur qui se produit entre deux moments.</span><span class="sxs-lookup"><span data-stu-id="77100-104">A calendar view is a set of events from the user's calendar that occur between two points of time.</span></span> <span data-ttu-id="77100-105">Vous l’utiliserez pour obtenir les événements de l’utilisateur pour la semaine en cours.</span><span class="sxs-lookup"><span data-stu-id="77100-105">You'll use this to get the user's events for the current week.</span></span>

1. <span data-ttu-id="77100-106">Ouvrez **./Controllers/CalendarController.cs** et ajoutez la fonction suivante à la classe **CalendarController** .</span><span class="sxs-lookup"><span data-stu-id="77100-106">Open **./Controllers/CalendarController.cs** and add the following function to the **CalendarController** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetStartOfWeekSnippet":::

1. <span data-ttu-id="77100-107">Ajoutez la fonction suivante pour gérer les exceptions renvoyées par les appels Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="77100-107">Add the following function to handle exceptions returned from Microsoft Graph calls.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="HandleGraphExceptionSnippet":::

1. <span data-ttu-id="77100-108">Remplacez la fonction `Get` existante par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="77100-108">Replace the existing `Get` function with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetSnippet" highlight="2,14-57":::

    <span data-ttu-id="77100-109">Passez en revue les modifications.</span><span class="sxs-lookup"><span data-stu-id="77100-109">Review the changes.</span></span> <span data-ttu-id="77100-110">Cette nouvelle version de la fonction :</span><span class="sxs-lookup"><span data-stu-id="77100-110">This new version of the function:</span></span>

    - <span data-ttu-id="77100-111">Renvoie à la `IEnumerable<Event>` place de `string` .</span><span class="sxs-lookup"><span data-stu-id="77100-111">Returns `IEnumerable<Event>` instead of `string`.</span></span>
    - <span data-ttu-id="77100-112">Obtient les paramètres de boîte aux lettres de l’utilisateur à l’aide de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="77100-112">Gets the user's mailbox settings using Microsoft Graph.</span></span>
    - <span data-ttu-id="77100-113">Utilise le fuseau horaire de l’utilisateur pour calculer le début et la fin de la semaine en cours.</span><span class="sxs-lookup"><span data-stu-id="77100-113">Uses the user's time zone to calculate the start and end of the current week.</span></span>
    - <span data-ttu-id="77100-114">Obtient une vue de calendrier</span><span class="sxs-lookup"><span data-stu-id="77100-114">Gets a calendar view</span></span>
        - <span data-ttu-id="77100-115">Utilise la `.Header()` fonction pour inclure un `Prefer: outlook.timezone` en-tête, ce qui entraîne la conversion des événements renvoyés en fuseau horaire de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="77100-115">Uses the `.Header()` function to include a `Prefer: outlook.timezone` header, which causes the returned events to have their start and end times converted to the user's timezone.</span></span>
        - <span data-ttu-id="77100-116">Utilise la `.Top()` fonction pour demander au plus 50 événements.</span><span class="sxs-lookup"><span data-stu-id="77100-116">Uses the `.Top()` function to request at most 50 events.</span></span>
        - <span data-ttu-id="77100-117">Utilise la `.Select()` fonction pour demander uniquement les champs utilisés par l’application.</span><span class="sxs-lookup"><span data-stu-id="77100-117">Uses the `.Select()` function to request just the fields used by the app.</span></span>
        - <span data-ttu-id="77100-118">Utilise la `OrderBy()` fonction pour trier les résultats en fonction de l’heure de début.</span><span class="sxs-lookup"><span data-stu-id="77100-118">Uses the `OrderBy()` function to sort the results by the start time.</span></span>

1. <span data-ttu-id="77100-119">Enregistrez vos modifications, puis redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="77100-119">Save your changes and restart the app.</span></span> <span data-ttu-id="77100-120">Actualisez l’onglet dans Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="77100-120">Refresh the tab in Microsoft Teams.</span></span> <span data-ttu-id="77100-121">L’application affiche une liste JSON des événements.</span><span class="sxs-lookup"><span data-stu-id="77100-121">The app displays a JSON listing of the events.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="77100-122">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="77100-122">Display the results</span></span>

<span data-ttu-id="77100-123">À présent, vous pouvez afficher la liste des événements de manière plus conviviale.</span><span class="sxs-lookup"><span data-stu-id="77100-123">Now you can display the list of events in a more user friendly way.</span></span>

1. <span data-ttu-id="77100-124">Ouvrez **./pages/index.cshtml** et ajoutez les fonctions suivantes à l’intérieur de la `<script>` balise.</span><span class="sxs-lookup"><span data-stu-id="77100-124">Open **./Pages/Index.cshtml** and add the following functions inside the `<script>` tag.</span></span>

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="RenderHelpersSnippet":::

1. <span data-ttu-id="77100-125">Remplacez la fonction `renderCalendar` existante par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="77100-125">Replace the existing `renderCalendar` function with the following.</span></span>

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="RenderCalendarSnippet":::

1. <span data-ttu-id="77100-126">Enregistrez vos modifications, puis redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="77100-126">Save your changes and restart the app.</span></span> <span data-ttu-id="77100-127">Actualisez l’onglet dans Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="77100-127">Refresh the tab in Microsoft Teams.</span></span> <span data-ttu-id="77100-128">L’application affiche les événements sur le calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="77100-128">The app displays events on the user's calendar.</span></span>

    ![Capture d’écran de l’application affichant le calendrier de l’utilisateur](images/calendar-view.png)
