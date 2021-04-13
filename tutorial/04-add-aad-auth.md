---
ms.openlocfilehash: 5e5168a6ccaf1374bc720f38a71a72d80d9d2b32
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695792"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3208b-101">Dans cet exercice, vous allez étendre l'application de l'exercice précédent pour prendre en charge l'authentification avec Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3208b-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="3208b-102">Cette étape est nécessaire pour obtenir le jeton d'accès OAuth nécessaire pour appeler Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="3208b-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="3208b-103">Dans cette étape, vous allez intégrer la bibliothèque d'authentification [Microsoft (MSAL) pour](https://github.com/AzureAD/microsoft-authentication-library-for-java) Java dans l'application.</span><span class="sxs-lookup"><span data-stu-id="3208b-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="3208b-104">Créez un répertoire nommé **graphtutorial** dans le répertoire **./src/main/resources.**</span><span class="sxs-lookup"><span data-stu-id="3208b-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="3208b-105">Créez un fichier dans le répertoire **./src/main/resources/graphtutorial** nommé **oAuth.properties** et ajoutez le texte suivant dans ce fichier.</span><span class="sxs-lookup"><span data-stu-id="3208b-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span> <span data-ttu-id="3208b-106">Remplacez `YOUR_APP_ID_HERE` par l'ID d'application que vous avez créé dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="3208b-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="3208b-107">La valeur contient `app.scopes` les étendues d'autorisation dont l'application a besoin.</span><span class="sxs-lookup"><span data-stu-id="3208b-107">The value of `app.scopes` contains the permission scopes the application requires.</span></span>

    - <span data-ttu-id="3208b-108">**User.Read permet** à l'application d'accéder au profil de l'utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3208b-108">**User.Read** allows the app to access the user's profile.</span></span>
    - <span data-ttu-id="3208b-109">**MailboxSettings.Read permet** à l'application d'accéder aux paramètres de la boîte aux lettres de l'utilisateur, y compris le fuseau horaire configuré par l'utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3208b-109">**MailboxSettings.Read** allows the app to access settings from the user's mailbox, including the user's configured time zone.</span></span>
    - <span data-ttu-id="3208b-110">**Calendars.ReadWrite permet** à l'application de ré lister le calendrier de l'utilisateur et d'ajouter de nouveaux événements au calendrier.</span><span class="sxs-lookup"><span data-stu-id="3208b-110">**Calendars.ReadWrite** allows the app to list the user's calendar and add new events to the calendar.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="3208b-111">Si vous utilisez un contrôle source tel que Git, il est temps d'exclure le fichier **oAuth.properties** du contrôle source afin d'éviter toute fuite accidentelle de votre ID d'application.</span><span class="sxs-lookup"><span data-stu-id="3208b-111">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="3208b-112">Ouvrez **App.java** et ajoutez les `import` instructions suivantes.</span><span class="sxs-lookup"><span data-stu-id="3208b-112">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="3208b-113">Ajoutez le code suivant juste avant la `Scanner input = new Scanner(System.in);` ligne pour charger le fichier **oAuth.properties.**</span><span class="sxs-lookup"><span data-stu-id="3208b-113">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="3208b-114">Implémentation de la connexion</span><span class="sxs-lookup"><span data-stu-id="3208b-114">Implement sign-in</span></span>

1. <span data-ttu-id="3208b-115">Créez un fichier dans le répertoire **./graphtutorial/src/main/java/graphtutorial** nommé **Graph.java** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="3208b-115">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    ```java
    package graphtutorial;

    import java.net.URL;
    import java.time.LocalDateTime;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Set;

    import okhttp3.Request;

    import com.azure.identity.DeviceCodeCredential;
    import com.azure.identity.DeviceCodeCredentialBuilder;

    import com.microsoft.graph.authentication.TokenCredentialAuthProvider;
    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.Attendee;
    import com.microsoft.graph.models.DateTimeTimeZone;
    import com.microsoft.graph.models.EmailAddress;
    import com.microsoft.graph.models.Event;
    import com.microsoft.graph.models.ItemBody;
    import com.microsoft.graph.models.User;
    import com.microsoft.graph.models.AttendeeType;
    import com.microsoft.graph.models.BodyType;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.GraphServiceClient;
    import com.microsoft.graph.requests.EventCollectionPage;
    import com.microsoft.graph.requests.EventCollectionRequestBuilder;

    public class Graph {

        private static GraphServiceClient<Request> graphClient = null;
        private static TokenCredentialAuthProvider authProvider = null;

        public static void initializeGraphAuth(String applicationId, List<String> scopes) {
            // Create the auth provider
            final DeviceCodeCredential credential = new DeviceCodeCredentialBuilder()
                .clientId(applicationId)
                .challengeConsumer(challenge -> System.out.println(challenge.getMessage()))
                .build();

            authProvider = new TokenCredentialAuthProvider(scopes, credential);

            // Create default logger to only log errors
            DefaultLogger logger = new DefaultLogger();
            logger.setLoggingLevel(LoggerLevel.ERROR);

            // Build a Graph client
            graphClient = GraphServiceClient.builder()
                .authenticationProvider(authProvider)
                .logger(logger)
                .buildClient();
        }

        public static String getUserAccessToken()
        {
            try {
                URL meUrl = new URL("https://graph.microsoft.com/v1.0/me");
                return authProvider.getAuthorizationTokenAsync(meUrl).get();
            } catch(Exception ex) {
                return null;
            }
        }
    }
    ```

1. <span data-ttu-id="3208b-116">Dans **App.java,** ajoutez le code suivant juste avant la `Scanner input = new Scanner(System.in);` ligne pour obtenir un jeton d'accès.</span><span class="sxs-lookup"><span data-stu-id="3208b-116">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Initialize Graph with auth settings
    Graph.initializeGraphAuth(appId, appScopes);
    final String accessToken = Graph.getUserAccessToken();
    ```

1. <span data-ttu-id="3208b-117">Ajoutez la ligne suivante après le `// Display access token` commentaire.</span><span class="sxs-lookup"><span data-stu-id="3208b-117">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="3208b-118">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="3208b-118">Run the app.</span></span> <span data-ttu-id="3208b-119">L'application affiche une URL et un code de périphérique.</span><span class="sxs-lookup"><span data-stu-id="3208b-119">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="3208b-120">Ouvrez un navigateur et accédez à l'URL affichée.</span><span class="sxs-lookup"><span data-stu-id="3208b-120">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="3208b-121">Entrez le code fourni et connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="3208b-121">Enter the provided code and sign in.</span></span> <span data-ttu-id="3208b-122">Une fois l'application terminée, revenir à l'application et choisir **le 1. Afficher l'option de jeton** d'accès pour afficher le jeton d'accès.</span><span class="sxs-lookup"><span data-stu-id="3208b-122">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

> [!TIP]
> <span data-ttu-id="3208b-123">Les jetons d'accès pour les comptes scolaires ou de travail Microsoft peuvent être parés à des fins de dépannage à l'aide de [https://jwt.ms](https://jwt.ms) .</span><span class="sxs-lookup"><span data-stu-id="3208b-123">Access tokens for Microsoft work or school accounts can be parsed for troubleshooting purposes at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="3208b-124">Les jetons d'accès pour les comptes Microsoft personnels utilisent un format propriétaire et ne peuvent pas être utilisés.</span><span class="sxs-lookup"><span data-stu-id="3208b-124">Access tokens for personal Microsoft accounts use a proprietary format and cannot be parsed.</span></span>
