<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez créer une inscription de l’application Web Azure AD à l’aide du centre d’administration Azure Active Directory.

1. Ouvrez un navigateur et accédez au [Centre d’administration Azure Active Directory](https://aad.portal.azure.com). Connectez-vous à l’aide d’un **compte personnel** (compte Microsoft) ou d’un **compte professionnel ou scolaire**.

1. Sélectionnez **Azure Active Directory** dans le volet de navigation gauche, puis sélectionnez **Inscriptions d’applications** sous **Gérer**.

    ![Une capture d’écran des inscriptions d’applications ](./images/aad-portal-app-registrations.png)

1. Sélectionnez **Nouvelle inscription**. Sur la page **inscrire une application** , définissez les valeurs comme suit, où `YOUR_NGROK_URL` est l’URL de transfert ngrok que vous avez copiée dans la section précédente.

    - Définissez le **Nom** sur `Teams Graph Tutorial`.
    - Définissez les **Types de comptes pris en charge** sur **Comptes dans un annuaire organisationnel et comptes personnels Microsoft**.
    - Sous **URI de redirection**, définissez la première flèche déroulante sur `Web`, et la valeur sur `YOUR_NGROK_URL/authcomplete`.

    ![Capture d’écran de la page Inscrire une application](./images/aad-register-an-app.png)

1. Sélectionner **Inscription**. Sur la page **Didacticiel des graphiques teams** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin à l’étape suivante.

    ![Une capture d’écran de l’ID d’application de la nouvelle inscription d'application](./images/aad-application-id.png)

1. Sous **Gérer**, sélectionnez **Authentification**. Recherchez la section **Grant implicite** et activez les **jetons d’accès** et les **jetons ID**. Sélectionnez **Enregistrer**.

1. Sélectionnez **Certificats et secrets** sous **Gérer**. Sélectionnez le bouton **Nouveau secret client**. Entrez une valeur dans **Description**, sélectionnez une des options pour **Expire le**, puis sélectionnez **Ajouter**.

1. Copiez la valeur du secret client avant de quitter cette page. Vous en aurez besoin à l’étape suivante.

    > [!IMPORTANT]
    > Ce secret client n’apparaîtra plus jamais, aussi veillez à le copier maintenant.

1. Sélectionnez **autorisations d’API** sous **gérer**, puis sélectionnez **Ajouter une autorisation**.

1. Sélectionnez **Microsoft Graph**, puis **autorisations déléguées**.

1. Sélectionnez les autorisations suivantes, puis **Ajouter des autorisations**.

    - **Calendars. ReadWrite** : permet à l’application de lire et d’écrire dans le calendrier de l’utilisateur.
    - **MailboxSettings. Read** : permet à l’application d’obtenir le fuseau horaire, le format de la date et le format de l’heure de l’utilisateur à partir de leurs paramètres de boîte aux lettres.

    ![Capture d’écran des autorisations configurées](images/aad-configured-permissions.png)

## <a name="configure-teams-single-sign-on"></a>Configuration de l’authentification unique teams

Dans cette section, vous allez mettre à jour l’inscription de l’application pour qu’elle prenne en charge l' [authentification unique dans teams](/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso).

1. Sélectionnez **exposer une API**. Sélectionnez le lien **définir** en regard de **URI ID d’application**. Insérez votre nom de domaine de l’URL de transfert ngrok (avec une barre oblique « / » ajoutée à la fin) entre les doubles barres obliques et le GUID. L’ID entier doit ressembler à ce qui suit : `api://50153897dd4d.ngrok.io/ae7d8088-3422-4c8c-a351-6ded0f21d615` .

1. Dans la section **étendues définies par cette API** , sélectionnez **Ajouter une étendue**. Renseignez les champs comme suit, puis sélectionnez **Ajouter une étendue**.

    - **Nom de l’étendue :**`access_as_user`
    - **Qui peut consentir ?: administrateurs et utilisateurs**
    - **Nom d’affichage du consentement de l’administrateur :**`Access the app as the user`
    - **Description du consentement administratif :**`Allows Teams to call the app's web APIs as the current user.`
    - **Nom d’affichage du consentement de l’utilisateur :**`Access the app as you`
    - **Description du consentement de l’utilisateur :**`Allows Teams to call the app's web APIs as you.`
    - **État : activé**

    ![Capture d’écran du formulaire ajouter une étendue](images/aad-add-scope.png)

1. Dans la section **applications clientes autorisées** , sélectionnez **Ajouter une application cliente**. Entrez un ID client dans la liste suivante, activez l’étendue sous les **étendues autorisées**, puis sélectionnez **Ajouter une application**. Répétez cette procédure pour chaque ID client dans la liste.

    - `1fec8e78-bce4-4aaf-ab1b-5451cc387264` (Team mobile/application de bureau)
    - `5e3ce6c0-2b1f-4285-8d4b-75ee78787346` (Application Web Teams)
