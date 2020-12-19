<!-- markdownlint-disable MD002 MD041 -->

Le manifeste de l’application décrit la façon dont l’application s’intègre à Microsoft teams et est nécessaire pour installer des applications. Dans cette section, vous allez utiliser app Studio dans le client Microsoft teams pour générer un manifeste.

1. Si vous n’avez pas encore installé App Studio dans Teams, [Installez-le maintenant](/microsoftteams/platform/concepts/build-and-test/app-studio-overview).

1. Lancez App Studio dans Microsoft teams et sélectionnez l' **éditeur de manifeste**.

1. Sélectionnez **créer une application**.

    ![Capture d’écran de l’éditeur de manifeste dans App Studio dans Microsoft teams](images/app-studio-01.png)

1. Sur la page Détails de l' **application** , renseignez les champs obligatoires.

    > [!NOTE]
    > Vous pouvez utiliser les icônes par défaut dans la section **personnalisation** ou télécharger les vôtres.

1. Dans le menu de gauche, sélectionnez **tabulations** sous **fonctionnalités**.

1. Sélectionnez **Ajouter** sous **Ajouter un onglet personnel**.

    ![Capture d’écran de la page onglets dans App Studio](images/app-studio-02.png)

1. Renseignez les champs comme suit, où `YOUR_NGROK_URL` est l’URL de transfert que vous avez copiée dans la section précédente. Sélectionnez **Enregistrer** lorsque vous avez fini.

    - **Nom :**`Create event`
    - **ID d’entité :**`createEventTab`
    - **URL du contenu :**`YOUR_NGROK_URL/newevent`

1. Sélectionnez **Ajouter** sous **Ajouter un onglet personnel**.

1. Renseignez les champs comme suit, où `YOUR_NGROK_URL` est l’URL de transfert que vous avez copiée dans la section précédente. Sélectionnez **Enregistrer** lorsque vous avez fini.

    - **Nom :**`Graph calendar`
    - **ID d’entité :**`calendarTab`
    - **URL du contenu :**`YOUR_NGROK_URL`

1. Dans le menu de gauche, sélectionnez **domaines et autorisations** sous **Terminer**.

1. Définissez l' **ID** de l’application AAD sur l’ID de l’application à partir de l’inscription de votre application.

1. Définissez le champ d' **authentification unique** sur l’URI de l’ID de l’application à partir de votre inscription de l’application.

1. Dans le menu de gauche, sélectionnez **test et distribuer** sous **Terminer**. Sélectionnez **Télécharger**.

1. Créez un répertoire dans la racine du projet nommé **Manifest**. Extrayez le contenu du fichier ZIP téléchargé dans ce répertoire.
