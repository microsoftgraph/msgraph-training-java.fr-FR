---
ms.openlocfilehash: c26e5b8ab0b7c5c62b926e3f5416b94e3f10b601
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661045"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application. Pour cette application, vous allez utiliser le [Kit de développement logiciel (SDK) Microsoft Graph pour Java](https://github.com/microsoftgraph/msgraph-sdk-java) pour effectuer des appels à Microsoft Graph.

## <a name="implement-an-authentication-provider"></a>Implémenter un fournisseur d’authentification

Le kit de développement logiciel (SDK) Microsoft Graph pour Java nécessite une implémentation de l' `IAuthenticationProvider` interface pour instancier son `GraphServiceClient` objet.

1. Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/graphtutorial** nommé **SimpleAuthProvider. Java** et ajoutez le code suivant.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a>Obtenir les détails de l’utilisateur

1. Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/graphtutorial** nommé **Graph. Java** et ajoutez le code suivant.

    ```java
    package graphtutorial;

    import java.time.LocalDateTime;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Set;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Attendee;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.EmailAddress;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.ItemBody;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.models.generated.AttendeeType;
    import com.microsoft.graph.models.generated.BodyType;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;
    import com.microsoft.graph.requests.extensions.IEventCollectionRequestBuilder;

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
                .select("displayName,mailboxSettings")
                .get();

            return me;
        }
    }
    ```

1. Ajoutez l' `import` instruction suivante en haut de **app. Java**.

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. Ajoutez le code suivant dans **app. Java** juste avant la `Scanner input = new Scanner(System.in);` ligne pour obtenir l’utilisateur et générer le nom complet de l’utilisateur.

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. Exécutez l’application. Une fois que vous êtes connecté à l’application, vous vous félicitez par son nom.

## <a name="get-calendar-events-from-outlook"></a>Récupérer les événements de calendrier à partir d’Outlook

1. Ajoutez la fonction suivante à la `Graph` classe dans **Graph. Java** pour obtenir des événements à partir du calendrier de l’utilisateur.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

Que fait ce code ?

- L’URL qui sera appelée est `/me/calendarview`.
  - `QueryOption` les objets sont utilisés pour ajouter `startDateTime` les `endDateTime` paramètres et, définissant le début et la fin de l’affichage Calendrier.
  - Un `QueryOption` objet est utilisé pour ajouter le `$orderby` paramètre, en triant les résultats par heure de début.
  - Un `HeaderOption` objet est utilisé pour ajouter l' `Prefer: outlook.timezone` en-tête, ce qui entraîne l’ajustement des heures de début et de fin au fuseau horaire de l’utilisateur.
  - La `select` fonction limite les champs renvoyés pour chaque événement à ceux que l’application utilisera réellement.
  - La `top` fonction limite le nombre d’événements dans la réponse à un maximum de 25.
- La `getNextPage` fonction est utilisée pour demander des pages supplémentaires de résultats s’il y a plus de 25 événements dans la semaine en cours.

1. Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/graphtutorial** nommé **GraphToIana. Java** et ajoutez le code suivant.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    Cette classe implémente une recherche simple pour convertir les noms des fuseaux horaires Windows en identificateurs IANA et pour générer un **ID** de zone basé sur un nom de fuseau horaire Windows.

## <a name="display-the-results"></a>Afficher les résultats

1. Ajoutez les `import` instructions suivantes dans **app. Java**.

    ```java
    import java.time.DayOfWeek;
    import java.time.LocalDateTime;
    import java.time.ZoneId;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.DateTimeParseException;
    import java.time.format.FormatStyle;
    import java.time.temporal.ChronoUnit;
    import java.time.temporal.TemporalAdjusters;
    import java.util.HashSet;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. Ajoutez la fonction suivante à la `App` classe pour mettre en forme les propriétés [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph dans un format convivial.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. Ajoutez la fonction suivante à la `App` classe pour obtenir les événements de l’utilisateur et les sortir dans la console.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. Ajoutez les éléments suivants juste après le `// List the calendar` commentaire dans la `main` fonction.

    ```java
    listCalendarEvents(accessToken);
    ```

1. Enregistrez toutes vos modifications, générez l’application, puis exécutez-la. Choisissez l’option **liste des événements de calendrier** pour afficher la liste des événements de l’utilisateur.

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. View this week's calendar
    3. Add an event
    2
    Events:
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 12/7/20, 2:00 PM (Pacific Standard Time)
      End: 12/7/20, 3:00 PM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/7/20, 4:00 PM (Pacific Standard Time)
      End: 12/7/20, 5:30 PM (Pacific Standard Time)
    Subject: Tailspin Toys Proposal Review + Lunch
      Organizer: Lidia Holloway
      Start: 12/8/20, 12:00 PM (Pacific Standard Time)
      End: 12/8/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/8/20, 3:00 PM (Pacific Standard Time)
      End: 12/8/20, 4:30 PM (Pacific Standard Time)
    Subject: Company Meeting
      Organizer: Christie Cline
      Start: 12/9/20, 8:30 AM (Pacific Standard Time)
      End: 12/9/20, 11:00 AM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/9/20, 4:00 PM (Pacific Standard Time)
      End: 12/9/20, 5:30 PM (Pacific Standard Time)
    Subject: Project Team Meeting
      Organizer: Lidia Holloway
      Start: 12/10/20, 8:00 AM (Pacific Standard Time)
      End: 12/10/20, 9:30 AM (Pacific Standard Time)
    Subject: Weekly Marketing Lunch
      Organizer: Adele Vance
      Start: 12/10/20, 12:00 PM (Pacific Standard Time)
      End: 12/10/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/10/20, 3:00 PM (Pacific Standard Time)
      End: 12/10/20, 4:30 PM (Pacific Standard Time)
    Subject: Lunch?
      Organizer: Lynne Robbins
      Start: 12/11/20, 12:00 PM (Pacific Standard Time)
      End: 12/11/20, 1:00 PM (Pacific Standard Time)
    Subject: Friday Unwinder
      Organizer: Megan Bowen
      Start: 12/11/20, 4:00 PM (Pacific Standard Time)
      End: 12/11/20, 5:00 PM (Pacific Standard Time)
    ```
