---
ms.openlocfilehash: 4c04317462240ff0696ac1381fae886481db7847
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661066"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD. Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph. Dans cette étape, vous allez intégrer la [bibliothèque d’authentification Microsoft (MSAL) pour Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) dans l’application.

1. Créez un nouveau répertoire nommé **graphtutorial** dans le répertoire **./src/main/Resources**

1. Créez un fichier dans le répertoire **./src/main/Resources/graphtutorial** nommé **OAuth. Properties** et ajoutez le texte suivant dans ce fichier. Remplacez `YOUR_APP_ID_HERE` par l’ID d’application que vous avez créé dans le portail Azure.

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    La valeur de `app.scopes` contient les étendues d’autorisation requises par l’application.

    - **User. Read** permet à l’application d’accéder au profil de l’utilisateur.
    - **MailboxSettings. Read** permet à l’application d’accéder aux paramètres à partir de la boîte aux lettres de l’utilisateur, y compris le fuseau horaire configuré par l’utilisateur.
    - **Calendars. ReadWrite** permet à l’application de répertorier le calendrier de l’utilisateur et d’ajouter de nouveaux événements dans le calendrier.

    > [!IMPORTANT]
    > Si vous utilisez le contrôle de code source tel que git, il est maintenant recommandé d’exclure le fichier **OAuth. Properties** du contrôle de code source afin d’éviter une fuite accidentelle de votre ID d’application.

1. Ouvrez **app. Java** et ajoutez les `import` instructions suivantes.

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. Ajoutez le code suivant juste avant la `Scanner input = new Scanner(System.in);` ligne pour charger le fichier **OAuth. Properties** .

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>Implémentation de la connexion

1. Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/graphtutorial** nommé **Authentication. Java** et ajoutez le code suivant.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. Dans **app. Java**, ajoutez le code suivant juste avant la `Scanner input = new Scanner(System.in);` ligne pour obtenir un jeton d’accès.

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. Ajoutez la ligne suivante après le `// Display access token` commentaire.

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. Exécutez l’application. L’application affiche une URL et un code de périphérique.

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. Ouvrez un navigateur et accédez à l’URL affichée. Entrez le code fourni et connectez-vous. Une fois terminé, revenez à l’application et choisissez le **1. Afficher** l’option de jeton d’accès pour afficher le jeton d’accès.

> [!TIP]
> Les jetons d’accès pour les comptes professionnels ou scolaires de Microsoft peuvent être analysés à des fins de dépannage à l’adresse [https://jwt.ms](https://jwt.ms) . Les jetons d’accès pour les comptes Microsoft personnels utilisent un format propriétaire et ne peuvent pas être analysés.
