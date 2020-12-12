---
ms.openlocfilehash: 4c04317462240ff0696ac1381fae886481db7847
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661066"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="cb294-101">Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cb294-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="cb294-102">Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="cb294-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="cb294-103">Dans cette étape, vous allez intégrer la [bibliothèque d’authentification Microsoft (MSAL) pour Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="cb294-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="cb294-104">Créez un nouveau répertoire nommé **graphtutorial** dans le répertoire **./src/main/Resources**</span><span class="sxs-lookup"><span data-stu-id="cb294-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="cb294-105">Créez un fichier dans le répertoire **./src/main/Resources/graphtutorial** nommé **OAuth. Properties** et ajoutez le texte suivant dans ce fichier.</span><span class="sxs-lookup"><span data-stu-id="cb294-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span> <span data-ttu-id="cb294-106">Remplacez `YOUR_APP_ID_HERE` par l’ID d’application que vous avez créé dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="cb294-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="cb294-107">La valeur de `app.scopes` contient les étendues d’autorisation requises par l’application.</span><span class="sxs-lookup"><span data-stu-id="cb294-107">The value of `app.scopes` contains the permission scopes the application requires.</span></span>

    - <span data-ttu-id="cb294-108">**User. Read** permet à l’application d’accéder au profil de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cb294-108">**User.Read** allows the app to access the user's profile.</span></span>
    - <span data-ttu-id="cb294-109">**MailboxSettings. Read** permet à l’application d’accéder aux paramètres à partir de la boîte aux lettres de l’utilisateur, y compris le fuseau horaire configuré par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cb294-109">**MailboxSettings.Read** allows the app to access settings from the user's mailbox, including the user's configured time zone.</span></span>
    - <span data-ttu-id="cb294-110">**Calendars. ReadWrite** permet à l’application de répertorier le calendrier de l’utilisateur et d’ajouter de nouveaux événements dans le calendrier.</span><span class="sxs-lookup"><span data-stu-id="cb294-110">**Calendars.ReadWrite** allows the app to list the user's calendar and add new events to the calendar.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="cb294-111">Si vous utilisez le contrôle de code source tel que git, il est maintenant recommandé d’exclure le fichier **OAuth. Properties** du contrôle de code source afin d’éviter une fuite accidentelle de votre ID d’application.</span><span class="sxs-lookup"><span data-stu-id="cb294-111">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="cb294-112">Ouvrez **app. Java** et ajoutez les `import` instructions suivantes.</span><span class="sxs-lookup"><span data-stu-id="cb294-112">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="cb294-113">Ajoutez le code suivant juste avant la `Scanner input = new Scanner(System.in);` ligne pour charger le fichier **OAuth. Properties** .</span><span class="sxs-lookup"><span data-stu-id="cb294-113">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="cb294-114">Implémentation de la connexion</span><span class="sxs-lookup"><span data-stu-id="cb294-114">Implement sign-in</span></span>

1. <span data-ttu-id="cb294-115">Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/graphtutorial** nommé **Authentication. Java** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="cb294-115">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="cb294-116">Dans **app. Java**, ajoutez le code suivant juste avant la `Scanner input = new Scanner(System.in);` ligne pour obtenir un jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="cb294-116">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="cb294-117">Ajoutez la ligne suivante après le `// Display access token` commentaire.</span><span class="sxs-lookup"><span data-stu-id="cb294-117">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="cb294-118">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="cb294-118">Run the app.</span></span> <span data-ttu-id="cb294-119">L’application affiche une URL et un code de périphérique.</span><span class="sxs-lookup"><span data-stu-id="cb294-119">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="cb294-120">Ouvrez un navigateur et accédez à l’URL affichée.</span><span class="sxs-lookup"><span data-stu-id="cb294-120">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="cb294-121">Entrez le code fourni et connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="cb294-121">Enter the provided code and sign in.</span></span> <span data-ttu-id="cb294-122">Une fois terminé, revenez à l’application et choisissez le **1. Afficher** l’option de jeton d’accès pour afficher le jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="cb294-122">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

> [!TIP]
> <span data-ttu-id="cb294-123">Les jetons d’accès pour les comptes professionnels ou scolaires de Microsoft peuvent être analysés à des fins de dépannage à l’adresse [https://jwt.ms](https://jwt.ms) .</span><span class="sxs-lookup"><span data-stu-id="cb294-123">Access tokens for Microsoft work or school accounts can be parsed for troubleshooting purposes at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="cb294-124">Les jetons d’accès pour les comptes Microsoft personnels utilisent un format propriétaire et ne peuvent pas être analysés.</span><span class="sxs-lookup"><span data-stu-id="cb294-124">Access tokens for personal Microsoft accounts use a proprietary format and cannot be parsed.</span></span>
