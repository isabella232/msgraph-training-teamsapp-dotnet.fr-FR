<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez ajouter la possibilité de créer des événements dans le calendrier de l’utilisateur.

## <a name="create-the-new-event-tab"></a>Créer l’onglet nouvel événement

1. Créez un fichier dans le répertoire **./pages** nommé **NewEvent. cshtml** et ajoutez le code suivant.

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.cshtml":::

    Cela permet d’implémenter un formulaire simple et d’ajouter du code JavaScript pour publier les données de formulaire dans l’API Web.

## <a name="implement-the-web-api"></a>Implémenter l’API Web

1. Créez un répertoire nommé **modèles** dans la racine du projet.

1. Créez un fichier dans le répertoire **./Models** nommé **NewEvent.cs** et ajoutez le code suivant.

    :::code language="csharp" source="../demo/GraphTutorial/Models/NewEvent.cs" id="NewEventSnippet":::

1. Ouvrez **./Controllers/CalendarController.cs** et ajoutez l' `using` instruction suivante en haut du fichier.

    ```csharp
    using GraphTutorial.Models;
    ```

1. Ajoutez la fonction suivante à la classe **CalendarController** .

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="PostSnippet":::

    Cela permet l’envoi d’un billet HTTP à l’API Web avec les champs du formulaire.

1. Enregistrez toutes vos modifications et redémarrez l’application. Actualisez l’application dans Microsoft Teams, puis sélectionnez l’onglet **créer un événement** . Remplissez le formulaire et sélectionnez **créer** pour ajouter un événement au calendrier de l’utilisateur.

    ![Capture d’écran de l’onglet créer un événement](images/create-event.png)
