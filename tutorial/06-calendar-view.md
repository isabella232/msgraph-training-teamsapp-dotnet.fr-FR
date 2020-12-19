<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez incorporer Microsoft Graph dans l’application. Pour cette application, vous allez utiliser la [bibliothèque cliente Microsoft Graph pour .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) pour effectuer des appels à Microsoft Graph.

# <a name="get-a-calendar-view"></a>Obtenir un affichage Calendrier

Un affichage Calendrier est un ensemble d’événements provenant du calendrier de l’utilisateur qui se produit entre deux moments. Vous l’utiliserez pour obtenir les événements de l’utilisateur pour la semaine en cours.

1. Ouvrez **./Controllers/CalendarController.cs** et ajoutez la fonction suivante à la classe **CalendarController** .

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetStartOfWeekSnippet":::

1. Ajoutez la fonction suivante pour gérer les exceptions renvoyées par les appels Microsoft Graph.

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="HandleGraphExceptionSnippet":::

1. Remplacez la fonction `Get` existante par ce qui suit.

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetSnippet" highlight="2,14-57":::

    Passez en revue les modifications. Cette nouvelle version de la fonction :

    - Renvoie à la `IEnumerable<Event>` place de `string` .
    - Obtient les paramètres de boîte aux lettres de l’utilisateur à l’aide de Microsoft Graph.
    - Utilise le fuseau horaire de l’utilisateur pour calculer le début et la fin de la semaine en cours.
    - Obtient une vue de calendrier
        - Utilise la `.Header()` fonction pour inclure un `Prefer: outlook.timezone` en-tête, ce qui entraîne la conversion des événements renvoyés en fuseau horaire de l’utilisateur.
        - Utilise la `.Top()` fonction pour demander au plus 50 événements.
        - Utilise la `.Select()` fonction pour demander uniquement les champs utilisés par l’application.
        - Utilise la `OrderBy()` fonction pour trier les résultats en fonction de l’heure de début.

1. Enregistrez vos modifications, puis redémarrez l’application. Actualisez l’onglet dans Microsoft Teams. L’application affiche une liste JSON des événements.

## <a name="display-the-results"></a>Afficher les résultats

À présent, vous pouvez afficher la liste des événements de manière plus conviviale.

1. Ouvrez **./pages/index.cshtml** et ajoutez les fonctions suivantes à l’intérieur de la `<script>` balise.

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="RenderHelpersSnippet":::

1. Remplacez la fonction `renderCalendar` existante par ce qui suit.

    :::code language="javascript" source="../demo/GraphTutorial/Pages/Index.cshtml" id="RenderCalendarSnippet":::

1. Enregistrez vos modifications, puis redémarrez l’application. Actualisez l’onglet dans Microsoft Teams. L’application affiche les événements sur le calendrier de l’utilisateur.

    ![Capture d’écran de l’application affichant le calendrier de l’utilisateur](images/calendar-view.png)
