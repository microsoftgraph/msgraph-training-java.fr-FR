---
ms.openlocfilehash: 16e96edc78ed2f6955bc14654edba1cb26323648
ms.sourcegitcommit: 2c0e0d2d6de994022dfa0faa10131582fb10e9b1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/21/2021
ms.locfileid: "49919528"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c5c70-101">Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application.</span><span class="sxs-lookup"><span data-stu-id="c5c70-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="c5c70-102">Pour cette application, vous allez utiliser le [SDK Microsoft Graph pour](https://github.com/microsoftgraph/msgraph-sdk-java) Java pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c5c70-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="c5c70-103">Implémenter un fournisseur d’authentification</span><span class="sxs-lookup"><span data-stu-id="c5c70-103">Implement an authentication provider</span></span>

<span data-ttu-id="c5c70-104">Le SDK Microsoft Graph pour Java nécessite une implémentation de `IAuthenticationProvider` l’interface pour insérer son `GraphServiceClient` objet.</span><span class="sxs-lookup"><span data-stu-id="c5c70-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="c5c70-105">Créez un fichier dans le répertoire **./graphtutorial/src/main/java/graphtutorial** nommé **SimpleAuthProvider.java** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="c5c70-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="c5c70-106">Obtenir les détails de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="c5c70-106">Get user details</span></span>

1. <span data-ttu-id="c5c70-107">Créez un fichier dans le répertoire **./graphtutorial/src/main/java/graphtutorial** nommé **Graph.java** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="c5c70-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

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

1. <span data-ttu-id="c5c70-108">Ajoutez `import` l’instruction suivante en haut **de App.java**.</span><span class="sxs-lookup"><span data-stu-id="c5c70-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="c5c70-109">Ajoutez le code suivant **dans App.java** juste avant la ligne pour obtenir l’utilisateur et afficher le nom complet `Scanner input = new Scanner(System.in);` de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c5c70-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="c5c70-110">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="c5c70-110">Run the app.</span></span> <span data-ttu-id="c5c70-111">Une fois que vous vous connectez à l’application, vous êtes le bienvenu par son nom.</span><span class="sxs-lookup"><span data-stu-id="c5c70-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="c5c70-112">Récupérer les événements de calendrier à partir d’Outlook</span><span class="sxs-lookup"><span data-stu-id="c5c70-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="c5c70-113">Ajoutez la fonction suivante à `Graph` la classe dans **Graph.java** pour obtenir des événements à partir du calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c5c70-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="c5c70-114">Que fait ce code ?</span><span class="sxs-lookup"><span data-stu-id="c5c70-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="c5c70-115">L’URL qui sera appelée est `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="c5c70-115">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="c5c70-116">`QueryOption` sont utilisés pour ajouter les paramètres et définir le début et la `startDateTime` `endDateTime` fin de l’affichage Calendrier.</span><span class="sxs-lookup"><span data-stu-id="c5c70-116">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="c5c70-117">Un `QueryOption` objet est utilisé pour ajouter le `$orderby` paramètre, triant les résultats par heure de début.</span><span class="sxs-lookup"><span data-stu-id="c5c70-117">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="c5c70-118">Un `HeaderOption` objet est utilisé pour ajouter l’en-tête, ce qui a pour effet d’ajuster les heures de début et de fin au `Prefer: outlook.timezone` fuseau horaire de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c5c70-118">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="c5c70-119">La fonction limite les champs renvoyés pour chaque événement à ceux que `select` l’application utilisera réellement.</span><span class="sxs-lookup"><span data-stu-id="c5c70-119">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="c5c70-120">La `top` fonction limite le nombre d’événements dans la réponse à un maximum de 25.</span><span class="sxs-lookup"><span data-stu-id="c5c70-120">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="c5c70-121">La fonction est utilisée pour demander des pages de résultats supplémentaires s’il y a plus de 25 événements `getNextPage` dans la semaine en cours.</span><span class="sxs-lookup"><span data-stu-id="c5c70-121">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="c5c70-122">Créez un fichier dans le répertoire **./graphtutorial/src/main/java/graphtutorial** nommé **GraphToIana.java** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="c5c70-122">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="c5c70-123">Cette classe implémente une recherche simple pour convertir des noms de fuseau horaire Windows en identificateurs IANA et pour générer un **ZoneId** basé sur un nom de fuseau horaire Windows.</span><span class="sxs-lookup"><span data-stu-id="c5c70-123">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="c5c70-124">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="c5c70-124">Display the results</span></span>

1. <span data-ttu-id="c5c70-125">Ajoutez les `import` instructions suivantes **dans App.java**.</span><span class="sxs-lookup"><span data-stu-id="c5c70-125">Add the following `import` statements in **App.java**.</span></span>

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

1. <span data-ttu-id="c5c70-126">Ajoutez la fonction suivante à la classe pour mettre en forme les `App` [propriétés dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph dans un format convivial.</span><span class="sxs-lookup"><span data-stu-id="c5c70-126">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="c5c70-127">Ajoutez la fonction suivante à la classe pour obtenir les événements `App` de l’utilisateur et les sortir dans la console.</span><span class="sxs-lookup"><span data-stu-id="c5c70-127">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="c5c70-128">Ajoutez ce qui suit juste après `// List the calendar` le commentaire dans la `main` fonction.</span><span class="sxs-lookup"><span data-stu-id="c5c70-128">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken, user.mailboxSettings.timeZone);
    ```

1. <span data-ttu-id="c5c70-129">Enregistrez toutes vos modifications, créez l’application, puis exécutez-la.</span><span class="sxs-lookup"><span data-stu-id="c5c70-129">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="c5c70-130">Choisissez **l’option Lister les** événements de calendrier pour voir la liste des événements de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c5c70-130">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
