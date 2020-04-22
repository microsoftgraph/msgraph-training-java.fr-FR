---
ms.openlocfilehash: 3e4d7f11c89947da29873c85ab2808279c94265a
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612060"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a91a9-101">Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a91a9-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="a91a9-102">Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="a91a9-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="a91a9-103">Dans cette étape, vous allez intégrer la [bibliothèque d’authentification Microsoft (MSAL) pour Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="a91a9-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="a91a9-104">Créez un nouveau répertoire nommé **graphtutorial** dans le répertoire **./src/main/Resources**</span><span class="sxs-lookup"><span data-stu-id="a91a9-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="a91a9-105">Créez un fichier dans le répertoire **./src/main/Resources/graphtutorial** nommé **OAuth. Properties**et ajoutez le texte suivant dans ce fichier.</span><span class="sxs-lookup"><span data-stu-id="a91a9-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="a91a9-106">Remplacez `YOUR_APP_ID_HERE` par l’ID d’application que vous avez créé dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="a91a9-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a91a9-107">Si vous utilisez le contrôle de code source tel que git, il est maintenant recommandé d’exclure le fichier **OAuth. Properties** du contrôle de code source afin d’éviter une fuite accidentelle de votre ID d’application.</span><span class="sxs-lookup"><span data-stu-id="a91a9-107">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="a91a9-108">Ouvrez **app. Java** et ajoutez les instructions `import` suivantes.</span><span class="sxs-lookup"><span data-stu-id="a91a9-108">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="a91a9-109">Ajoutez le code suivant juste avant la `Scanner input = new Scanner(System.in);` ligne pour charger le fichier **OAuth. Properties** .</span><span class="sxs-lookup"><span data-stu-id="a91a9-109">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="a91a9-110">Implémentation de la connexion</span><span class="sxs-lookup"><span data-stu-id="a91a9-110">Implement sign-in</span></span>

1. <span data-ttu-id="a91a9-111">Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/graphtutorial** nommé **Authentication. Java** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="a91a9-111">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="a91a9-112">Dans **app. Java**, ajoutez le code suivant juste avant la `Scanner input = new Scanner(System.in);` ligne pour obtenir un jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="a91a9-112">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="a91a9-113">Ajoutez la ligne suivante après le `// Display access token` commentaire.</span><span class="sxs-lookup"><span data-stu-id="a91a9-113">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="a91a9-114">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="a91a9-114">Run the app.</span></span> <span data-ttu-id="a91a9-115">L’application affiche une URL et un code de périphérique.</span><span class="sxs-lookup"><span data-stu-id="a91a9-115">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="a91a9-116">Ouvrez un navigateur et accédez à l’URL affichée.</span><span class="sxs-lookup"><span data-stu-id="a91a9-116">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="a91a9-117">Entrez le code fourni et connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="a91a9-117">Enter the provided code and sign in.</span></span> <span data-ttu-id="a91a9-118">Une fois terminé, revenez à l’application et choisissez le **1. Afficher** l’option de jeton d’accès pour afficher le jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="a91a9-118">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
