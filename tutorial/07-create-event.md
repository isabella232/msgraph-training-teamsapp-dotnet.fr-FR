<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1e148-101">Dans cette section, vous allez ajouter la possibilité de créer des événements dans le calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="1e148-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="create-the-new-event-tab"></a><span data-ttu-id="1e148-102">Créer l’onglet nouvel événement</span><span class="sxs-lookup"><span data-stu-id="1e148-102">Create the new event tab</span></span>

1. <span data-ttu-id="1e148-103">Créez un fichier dans le répertoire **./pages** nommé **NewEvent. cshtml** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="1e148-103">Create a new file in the **./Pages** directory named **NewEvent.cshtml** and add the following code.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.cshtml":::

    <span data-ttu-id="1e148-104">Cela permet d’implémenter un formulaire simple et d’ajouter du code JavaScript pour publier les données de formulaire dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="1e148-104">This implements a simple form, and adds JavaScript to post the form data to the Web API.</span></span>

## <a name="implement-the-web-api"></a><span data-ttu-id="1e148-105">Implémenter l’API Web</span><span class="sxs-lookup"><span data-stu-id="1e148-105">Implement the Web API</span></span>

1. <span data-ttu-id="1e148-106">Créez un répertoire nommé **modèles** dans la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="1e148-106">Create a new directory named **Models** in the root of the project.</span></span>

1. <span data-ttu-id="1e148-107">Créez un fichier dans le répertoire **./Models** nommé **NewEvent.cs** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="1e148-107">Create a new file in the **./Models** directory named **NewEvent.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Models/NewEvent.cs" id="NewEventSnippet":::

1. <span data-ttu-id="1e148-108">Ouvrez **./Controllers/CalendarController.cs** et ajoutez l' `using` instruction suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="1e148-108">Open **./Controllers/CalendarController.cs** and add the following `using` statement at the top of the file.</span></span>

    ```csharp
    using GraphTutorial.Models;
    ```

1. <span data-ttu-id="1e148-109">Ajoutez la fonction suivante à la classe **CalendarController** .</span><span class="sxs-lookup"><span data-stu-id="1e148-109">Add the following function to the **CalendarController** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="PostSnippet":::

    <span data-ttu-id="1e148-110">Cela permet l’envoi d’un billet HTTP à l’API Web avec les champs du formulaire.</span><span class="sxs-lookup"><span data-stu-id="1e148-110">This allows an HTTP POST to the Web API with the fields of the form.</span></span>

1. <span data-ttu-id="1e148-111">Enregistrez toutes vos modifications et redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="1e148-111">Save all of your changes and restart the application.</span></span> <span data-ttu-id="1e148-112">Actualisez l’application dans Microsoft Teams, puis sélectionnez l’onglet **créer un événement** . Remplissez le formulaire et sélectionnez **créer** pour ajouter un événement au calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="1e148-112">Refresh the app in Microsoft Teams, and select the **Create event** tab. Fill out the form and select **Create** to add an event to the user's calendar.</span></span>

    ![Capture d’écran de l’onglet créer un événement](images/create-event.png)
