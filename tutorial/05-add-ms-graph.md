---
ms.openlocfilehash: 93688a97872ad640c12c7137f4cc09ede4a98416
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612067"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application. Pour cette application, vous allez utiliser le [Kit de développement logiciel (SDK) Microsoft Graph pour Java](https://github.com/microsoftgraph/msgraph-sdk-java) pour effectuer des appels à Microsoft Graph.

## <a name="implement-an-authentication-provider"></a>Implémenter un fournisseur d’authentification

Le kit de développement logiciel (SDK) Microsoft Graph pour `IAuthenticationProvider` Java nécessite une implémentation `GraphServiceClient` de l’interface pour instancier son objet.

1. Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/graphtutorial** nommé **SimpleAuthProvider. Java** et ajoutez le code suivant.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a>Obtenir les détails de l’utilisateur

1. Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/graphtutorial** nommé **Graph. Java** et ajoutez le code suivant.

    ```java
    package graphtutorial;

    import java.util.LinkedList;
    import java.util.List;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;

    /**
     * Graph
     */
    public class Graph {

        private static IGraphServiceClient graphClient = null;
        private static SimpleAuthProvider authProvider = null;

        private static void ensureGraphClient(String accessToken) {
            if (graphClient == null) {
                // Create the auth provider
                authProvider = new SimpleAuthProvider(accessToken);

                // Create default logger to only log errors
                DefaultLogger logger = new DefaultLogger();
                logger.setLoggingLevel(LoggerLevel.ERROR);

                // Build a Graph client
                graphClient = GraphServiceClient.builder()
                    .authenticationProvider(authProvider)
                    .logger(logger)
                    .buildClient();
            }
        }

        public static User getUser(String accessToken) {
            ensureGraphClient(accessToken);

            // GET /me to get authenticated user
            User me = graphClient
                .me()
                .buildRequest()
                .get();

            return me;
        }
    }
    ```

1. Ajoutez l’instruction `import` suivante en haut de **app. Java**.

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. Ajoutez le code suivant dans **app. Java** juste avant la `Scanner input = new Scanner(System.in);` ligne pour obtenir l’utilisateur et générer le nom complet de l’utilisateur.

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println();
    ```

1. Exécutez l’application. Une fois que vous êtes connecté à l’application, vous vous félicitez par son nom.

## <a name="get-calendar-events-from-outlook"></a>Récupérer les événements de calendrier à partir d’Outlook

1. Ajoutez la fonction suivante à la `Graph` classe dans **Graph. Java** pour obtenir des événements à partir du calendrier de l’utilisateur.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

Que fait ce code ?

- L’URL qui sera appelée est `/me/events`.
- La `select` fonction limite les champs renvoyés pour chaque événement à ceux que l’application utilisera réellement.
- Un `QueryOption` est utilisé pour trier les résultats en fonction de la date et de l’heure de leur création, avec l’élément le plus récent en premier.

## <a name="display-the-results"></a>Afficher les résultats

1. Ajoutez les instructions `import` suivantes dans **app. Java**.

    ```java
    import java.time.LocalDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.FormatStyle;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. Ajoutez la fonction suivante à la `App` classe pour mettre en forme les propriétés [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph dans un format convivial.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. Ajoutez la fonction suivante à la `App` classe pour obtenir les événements de l’utilisateur et les sortir dans la console.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. Ajoutez les éléments suivants juste après `// List the calendar` le commentaire dans `main` la fonction.

    ```java
    listCalendarEvents(accessToken);
    ```

1. Enregistrez toutes vos modifications, générez l’application, puis exécutez-la. Choisissez l’option **liste des événements de calendrier** pour afficher la liste des événements de l’utilisateur.

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. List calendar events
    2
    Events:
    Subject: Team meeting
      Organizer: Adele Vance
      Start: 5/22/19, 3:00 PM (UTC)
      End: 5/22/19, 4:00 PM (UTC)
    Subject: Team Lunch
      Organizer: Adele Vance
      Start: 5/24/19, 6:30 PM (UTC)
      End: 5/24/19, 8:00 PM (UTC)
    Subject: Flight to Redmond
      Organizer: Adele Vance
      Start: 5/26/19, 4:30 PM (UTC)
      End: 5/26/19, 7:00 PM (UTC)
    Subject: Let's meet to discuss strategy
      Organizer: Patti Fernandez
      Start: 5/27/19, 10:00 PM (UTC)
      End: 5/27/19, 10:30 PM (UTC)
    Subject: All-hands meeting
      Organizer: Adele Vance
      Start: 5/28/19, 3:30 PM (UTC)
      End: 5/28/19, 5:00 PM (UTC)
    ```
