---
ms.openlocfilehash: c26e5b8ab0b7c5c62b926e3f5416b94e3f10b601
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661045"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="8c83d-101">Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application.</span><span class="sxs-lookup"><span data-stu-id="8c83d-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="8c83d-102">Pour cette application, vous allez utiliser le [Kit de développement logiciel (SDK) Microsoft Graph pour Java](https://github.com/microsoftgraph/msgraph-sdk-java) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="8c83d-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="8c83d-103">Implémenter un fournisseur d’authentification</span><span class="sxs-lookup"><span data-stu-id="8c83d-103">Implement an authentication provider</span></span>

<span data-ttu-id="8c83d-104">Le kit de développement logiciel (SDK) Microsoft Graph pour Java nécessite une implémentation de l' `IAuthenticationProvider` interface pour instancier son `GraphServiceClient` objet.</span><span class="sxs-lookup"><span data-stu-id="8c83d-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="8c83d-105">Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/graphtutorial** nommé **SimpleAuthProvider. Java** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="8c83d-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="8c83d-106">Obtenir les détails de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="8c83d-106">Get user details</span></span>

1. <span data-ttu-id="8c83d-107">Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/graphtutorial** nommé **Graph. Java** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="8c83d-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

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

1. <span data-ttu-id="8c83d-108">Ajoutez l' `import` instruction suivante en haut de **app. Java**.</span><span class="sxs-lookup"><span data-stu-id="8c83d-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="8c83d-109">Ajoutez le code suivant dans **app. Java** juste avant la `Scanner input = new Scanner(System.in);` ligne pour obtenir l’utilisateur et générer le nom complet de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8c83d-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="8c83d-110">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="8c83d-110">Run the app.</span></span> <span data-ttu-id="8c83d-111">Une fois que vous êtes connecté à l’application, vous vous félicitez par son nom.</span><span class="sxs-lookup"><span data-stu-id="8c83d-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="8c83d-112">Récupérer les événements de calendrier à partir d’Outlook</span><span class="sxs-lookup"><span data-stu-id="8c83d-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="8c83d-113">Ajoutez la fonction suivante à la `Graph` classe dans **Graph. Java** pour obtenir des événements à partir du calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8c83d-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="8c83d-114">Que fait ce code ?</span><span class="sxs-lookup"><span data-stu-id="8c83d-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="8c83d-115">L’URL qui sera appelée est `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="8c83d-115">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="8c83d-116">`QueryOption` les objets sont utilisés pour ajouter `startDateTime` les `endDateTime` paramètres et, définissant le début et la fin de l’affichage Calendrier.</span><span class="sxs-lookup"><span data-stu-id="8c83d-116">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="8c83d-117">Un `QueryOption` objet est utilisé pour ajouter le `$orderby` paramètre, en triant les résultats par heure de début.</span><span class="sxs-lookup"><span data-stu-id="8c83d-117">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="8c83d-118">Un `HeaderOption` objet est utilisé pour ajouter l' `Prefer: outlook.timezone` en-tête, ce qui entraîne l’ajustement des heures de début et de fin au fuseau horaire de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8c83d-118">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="8c83d-119">La `select` fonction limite les champs renvoyés pour chaque événement à ceux que l’application utilisera réellement.</span><span class="sxs-lookup"><span data-stu-id="8c83d-119">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="8c83d-120">La `top` fonction limite le nombre d’événements dans la réponse à un maximum de 25.</span><span class="sxs-lookup"><span data-stu-id="8c83d-120">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="8c83d-121">La `getNextPage` fonction est utilisée pour demander des pages supplémentaires de résultats s’il y a plus de 25 événements dans la semaine en cours.</span><span class="sxs-lookup"><span data-stu-id="8c83d-121">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="8c83d-122">Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/graphtutorial** nommé **GraphToIana. Java** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="8c83d-122">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="8c83d-123">Cette classe implémente une recherche simple pour convertir les noms des fuseaux horaires Windows en identificateurs IANA et pour générer un **ID** de zone basé sur un nom de fuseau horaire Windows.</span><span class="sxs-lookup"><span data-stu-id="8c83d-123">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="8c83d-124">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="8c83d-124">Display the results</span></span>

1. <span data-ttu-id="8c83d-125">Ajoutez les `import` instructions suivantes dans **app. Java**.</span><span class="sxs-lookup"><span data-stu-id="8c83d-125">Add the following `import` statements in **App.java**.</span></span>

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

1. <span data-ttu-id="8c83d-126">Ajoutez la fonction suivante à la `App` classe pour mettre en forme les propriétés [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph dans un format convivial.</span><span class="sxs-lookup"><span data-stu-id="8c83d-126">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="8c83d-127">Ajoutez la fonction suivante à la `App` classe pour obtenir les événements de l’utilisateur et les sortir dans la console.</span><span class="sxs-lookup"><span data-stu-id="8c83d-127">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="8c83d-128">Ajoutez les éléments suivants juste après le `// List the calendar` commentaire dans la `main` fonction.</span><span class="sxs-lookup"><span data-stu-id="8c83d-128">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken);
    ```

1. <span data-ttu-id="8c83d-129">Enregistrez toutes vos modifications, générez l’application, puis exécutez-la.</span><span class="sxs-lookup"><span data-stu-id="8c83d-129">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="8c83d-130">Choisissez l’option **liste des événements de calendrier** pour afficher la liste des événements de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8c83d-130">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
