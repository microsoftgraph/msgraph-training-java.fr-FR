---
ms.openlocfilehash: 5e5168a6ccaf1374bc720f38a71a72d80d9d2b32
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695792"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez étendre l'application de l'exercice précédent pour prendre en charge l'authentification avec Azure AD. Cette étape est nécessaire pour obtenir le jeton d'accès OAuth nécessaire pour appeler Microsoft Graph. Dans cette étape, vous allez intégrer la bibliothèque d'authentification [Microsoft (MSAL) pour](https://github.com/AzureAD/microsoft-authentication-library-for-java) Java dans l'application.

1. Créez un répertoire nommé **graphtutorial** dans le répertoire **./src/main/resources.**

1. Créez un fichier dans le répertoire **./src/main/resources/graphtutorial** nommé **oAuth.properties** et ajoutez le texte suivant dans ce fichier. Remplacez `YOUR_APP_ID_HERE` par l'ID d'application que vous avez créé dans le portail Azure.

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    La valeur contient `app.scopes` les étendues d'autorisation dont l'application a besoin.

    - **User.Read permet** à l'application d'accéder au profil de l'utilisateur.
    - **MailboxSettings.Read permet** à l'application d'accéder aux paramètres de la boîte aux lettres de l'utilisateur, y compris le fuseau horaire configuré par l'utilisateur.
    - **Calendars.ReadWrite permet** à l'application de ré lister le calendrier de l'utilisateur et d'ajouter de nouveaux événements au calendrier.

    > [!IMPORTANT]
    > Si vous utilisez un contrôle source tel que Git, il est temps d'exclure le fichier **oAuth.properties** du contrôle source afin d'éviter toute fuite accidentelle de votre ID d'application.

1. Ouvrez **App.java** et ajoutez les `import` instructions suivantes.

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. Ajoutez le code suivant juste avant la `Scanner input = new Scanner(System.in);` ligne pour charger le fichier **oAuth.properties.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>Implémentation de la connexion

1. Créez un fichier dans le répertoire **./graphtutorial/src/main/java/graphtutorial** nommé **Graph.java** et ajoutez le code suivant.

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

1. Dans **App.java,** ajoutez le code suivant juste avant la `Scanner input = new Scanner(System.in);` ligne pour obtenir un jeton d'accès.

    ```java
    // Initialize Graph with auth settings
    Graph.initializeGraphAuth(appId, appScopes);
    final String accessToken = Graph.getUserAccessToken();
    ```

1. Ajoutez la ligne suivante après le `// Display access token` commentaire.

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. Exécutez l’application. L'application affiche une URL et un code de périphérique.

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. Ouvrez un navigateur et accédez à l'URL affichée. Entrez le code fourni et connectez-vous. Une fois l'application terminée, revenir à l'application et choisir **le 1. Afficher l'option de jeton** d'accès pour afficher le jeton d'accès.

> [!TIP]
> Les jetons d'accès pour les comptes scolaires ou de travail Microsoft peuvent être parés à des fins de dépannage à l'aide de [https://jwt.ms](https://jwt.ms) . Les jetons d'accès pour les comptes Microsoft personnels utilisent un format propriétaire et ne peuvent pas être utilisés.
