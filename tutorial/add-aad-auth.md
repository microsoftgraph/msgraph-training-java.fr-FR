---
ms.openlocfilehash: 44426ee29265e8730ea3bc19f21091aa0180fd6e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634744"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="350fd-101">Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD.</span><span class="sxs-lookup"><span data-stu-id="350fd-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="350fd-102">Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="350fd-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="350fd-103">Dans cette étape, vous allez intégrer la [bibliothèque d’authentification Microsoft (MSAL) pour Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="350fd-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

<span data-ttu-id="350fd-104">Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/com/contoso** nommé **OAuth. Properties**.</span><span class="sxs-lookup"><span data-stu-id="350fd-104">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **oAuth.properties**.</span></span> <span data-ttu-id="350fd-105">Ajoutez le texte suivant dans ce fichier.</span><span class="sxs-lookup"><span data-stu-id="350fd-105">Add the following text in that file.</span></span>

```INI
app.id=YOUR_APP_ID_HERE
app.scopes=User.Read,Calendars.Read
```

<span data-ttu-id="350fd-106">Remplacez `YOUR_APP_ID_HERE` par l’ID d’application que vous avez créé dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="350fd-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="350fd-107">Si vous utilisez le contrôle de code source tel que git, il est maintenant recommandé d’exclure le fichier **OAuth. Properties** du contrôle de code source afin d’éviter une fuite accidentelle de votre ID d’application.</span><span class="sxs-lookup"><span data-stu-id="350fd-107">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="350fd-108">Ouvrez **app. Java** et ajoutez les instructions `import` suivantes.</span><span class="sxs-lookup"><span data-stu-id="350fd-108">Open **App.java** and add the following `import` statements.</span></span>

```java
import java.io.IOException;
import java.util.Properties;
```

<span data-ttu-id="350fd-109">Ajoutez ensuite le code suivant juste avant la `Scanner input = new Scanner(System.in);` ligne pour charger le fichier **OAuth. Properties** .</span><span class="sxs-lookup"><span data-stu-id="350fd-109">Then add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

```java
// Load OAuth settings
final Properties oAuthProperties = new Properties();
try {
    oAuthProperties.load(App.class.getResourceAsStream("oAuth.properties"));
} catch (IOException e) {
    System.out.println("Unable to read OAuth configuration. Make sure you have a properly formatted oAuth.properties file. See README for details.");
    return;
}

final String appId = oAuthProperties.getProperty("app.id");
final String[] appScopes = oAuthProperties.getProperty("app.scopes").split(",");
```

## <a name="implement-sign-in"></a><span data-ttu-id="350fd-110">Mettre en œuvre la connexion</span><span class="sxs-lookup"><span data-stu-id="350fd-110">Implement sign-in</span></span>

<span data-ttu-id="350fd-111">Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/com/contoso** nommé **Authentication. Java** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="350fd-111">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **Authentication.java** and add the following code.</span></span>

```java
package com.contoso;
import java.net.MalformedURLException;
import java.util.Set;
import java.util.function.Consumer;

import com.microsoft.aad.msal4j.DeviceCode;
import com.microsoft.aad.msal4j.DeviceCodeFlowParameters;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.PublicClientApplication;

/**
 * Authentication
 */
public class Authentication {

    private static String applicationId;
    // Set authority to allow only organizational accounts
    // Device code flow only supports organizational accounts
    private final static String authority = "https://login.microsoftonline.com/organizations/";

    public static void initialize(String applicationId) {
        Authentication.applicationId = applicationId;
    }

    public static String getUserAccessToken(String[] scopes) {
        if (applicationId == null) {
            System.out.println("You must initialize Authentication before calling getUserAccessToken");
            return null;
        }

        Set<String> scopeSet = Set.of(scopes);

        PublicClientApplication app;
        try {
            // Build the MSAL application object with
            // app ID and authority
            app = PublicClientApplication.builder(applicationId)
                .authority(authority)
                .build();
        } catch (MalformedURLException e) {
            return null;
        }

        // Create consumer to receive the DeviceCode object
        // This method gets executed during the flow and provides
        // the URL the user logs into and the device code to enter
        Consumer<DeviceCode> deviceCodeConsumer = (DeviceCode deviceCode) -> {
            // Print the login information to the console
            System.out.println(deviceCode.message());
        };

        // Request a token, passing the requested permission scopes
        IAuthenticationResult result = app.acquireToken(
            DeviceCodeFlowParameters
                .builder(scopeSet, deviceCodeConsumer)
                .build()
        ).exceptionally(ex -> {
            System.out.println("Unable to authenticate - " + ex.getMessage());
            return null;
        }).join();

        if (result != null) {
            return result.accessToken();
        }

        return null;
    }
}
```

<span data-ttu-id="350fd-112">Dans **app. Java**, ajoutez le code suivant juste avant la `Scanner input = new Scanner(System.in);` ligne pour obtenir un jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="350fd-112">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

```java
// Get an access token
Authentication.initialize(appId);
final String accessToken = Authentication.getUserAccessToken(appScopes);
```

<span data-ttu-id="350fd-113">Ajoutez ensuite la ligne suivante après le `// Display access token` commentaire.</span><span class="sxs-lookup"><span data-stu-id="350fd-113">Then add the following line after the `// Display access token` comment.</span></span>

```java
System.out.println("Access token: " + accessToken);
```

<span data-ttu-id="350fd-114">Générez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="350fd-114">Build and run the app.</span></span> <span data-ttu-id="350fd-115">L’application affiche une URL et un code de périphérique.</span><span class="sxs-lookup"><span data-stu-id="350fd-115">The application displays a URL and device code.</span></span>

```Shell
Java Graph Tutorial

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
```

<span data-ttu-id="350fd-116">Ouvrez un navigateur et accédez à l’URL affichée.</span><span class="sxs-lookup"><span data-stu-id="350fd-116">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="350fd-117">Entrez le code fourni et connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="350fd-117">Enter the provided code and sign in.</span></span> <span data-ttu-id="350fd-118">Une fois terminé, revenez à l’application et choisissez le **1. Afficher** l’option de jeton d’accès pour afficher le jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="350fd-118">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
