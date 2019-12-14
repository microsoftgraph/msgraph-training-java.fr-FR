---
ms.openlocfilehash: 3fb56d613dbaa56474c9af58ebdd0b157c53bf1a
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018806"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="155c5-101">Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application.</span><span class="sxs-lookup"><span data-stu-id="155c5-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="155c5-102">Pour cette application, vous allez utiliser le [Kit de développement logiciel (SDK) Microsoft Graph pour Java](https://github.com/microsoftgraph/msgraph-sdk-java) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="155c5-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="155c5-103">Implémenter un fournisseur d’authentification</span><span class="sxs-lookup"><span data-stu-id="155c5-103">Implement an authentication provider</span></span>

<span data-ttu-id="155c5-104">Le kit de développement logiciel (SDK) Microsoft Graph pour `IAuthenticationProvider` Java nécessite une implémentation `GraphServiceClient` de l’interface pour instancier son objet.</span><span class="sxs-lookup"><span data-stu-id="155c5-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span> <span data-ttu-id="155c5-105">Commencez par créer une classe simple pour ajouter le jeton d’accès aux demandes sortantes.</span><span class="sxs-lookup"><span data-stu-id="155c5-105">Start by creating a simple class to add the access token to outgoing requests.</span></span> <span data-ttu-id="155c5-106">Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/com/contoso** nommé **SimpleAuthProvider. Java** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="155c5-106">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

```java
package com.contoso;

import com.microsoft.graph.authentication.IAuthenticationProvider;
import com.microsoft.graph.http.IHttpRequest;

/**
 * SimpleAuthProvider
 */
public class SimpleAuthProvider implements IAuthenticationProvider {

    private String accessToken = null;

    public SimpleAuthProvider(String accessToken) {
        this.accessToken = accessToken;
    }

    @Override
    public void authenticateRequest(IHttpRequest request) {
        // Add the access token in the Authorization header
        request.addHeader("Authorization", "Bearer " + accessToken);
    }
}
```

## <a name="get-user-details"></a><span data-ttu-id="155c5-107">Obtenir les détails de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="155c5-107">Get user details</span></span>

<span data-ttu-id="155c5-108">Tout d’abord, ajoutez une nouvelle classe pour contenir toutes les fonctionnalités Graph.</span><span class="sxs-lookup"><span data-stu-id="155c5-108">First, add a new class to contain all of the Graph functionality.</span></span> <span data-ttu-id="155c5-109">Créez un fichier dans le répertoire **./graphtutorial/src/main/Java/com/contoso** nommé **Graph. Java** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="155c5-109">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **Graph.java** and add the following code.</span></span>

```java
package com.contoso;

import com.microsoft.graph.logger.DefaultLogger;
import com.microsoft.graph.logger.LoggerLevel;
import com.microsoft.graph.models.extensions.IGraphServiceClient;
import com.microsoft.graph.models.extensions.User;
import com.microsoft.graph.requests.extensions.GraphServiceClient;
import java.util.LinkedList;
import java.util.List;import com.microsoft.graph.models.extensions.Event;import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
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

<span data-ttu-id="155c5-110">Ajoutez le code suivant dans **app. Java** juste avant la `Scanner input = new Scanner(System.in);` ligne pour obtenir l’utilisateur et générer le nom complet de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="155c5-110">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

```java
// Greet the user
User user = Graph.getUser(accessToken);
System.out.println("Welcome " + user.displayName);
System.out.println();
```

<span data-ttu-id="155c5-111">Si vous exécutez l’application maintenant, une fois que vous vous êtes en train de vous reconnecter à l’application, vous vous félicitez par son nom.</span><span class="sxs-lookup"><span data-stu-id="155c5-111">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="155c5-112">Obtenir des événements de calendrier à partir d’Outlook</span><span class="sxs-lookup"><span data-stu-id="155c5-112">Get calendar events from Outlook</span></span>

<span data-ttu-id="155c5-113">Ajoutez les instructions `import` suivantes à **Graph. Java**.</span><span class="sxs-lookup"><span data-stu-id="155c5-113">Add the following `import` statements to **Graph.java**.</span></span>

```java
import java.util.LinkedList;
import java.util.List;
import com.microsoft.graph.models.extensions.Event;
import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
import com.microsoft.graph.requests.extensions.IEventCollectionPage;
```

<span data-ttu-id="155c5-114">Ajoutez la fonction suivante à la `Graph` classe dans **Graph. Java** pour obtenir des événements à partir du calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="155c5-114">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

```java
public static List<Event> getEvents(String accessToken) {
    ensureGraphClient(accessToken);

    // Use QueryOption to specify the $orderby query parameter
    final List<Option> options = new LinkedList<Option>();
    // Sort results by createdDateTime, get newest first
    options.add(new QueryOption("orderby", "createdDateTime DESC"));

    // GET /me/events
    IEventCollectionPage eventPage = graphClient
        .me()
        .events()
        .buildRequest(options)
        .select("subject,organizer,start,end")
        .get();

    return eventPage.getCurrentPage();
}
```

<span data-ttu-id="155c5-115">Examinez ce que fait ce code.</span><span class="sxs-lookup"><span data-stu-id="155c5-115">Consider what this code is doing.</span></span>

- <span data-ttu-id="155c5-116">L’URL qui sera appelée est `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="155c5-116">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="155c5-117">La `select` fonction limite les champs renvoyés pour chaque événement à ceux que l’application utilisera réellement.</span><span class="sxs-lookup"><span data-stu-id="155c5-117">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="155c5-118">Un `QueryOption` est utilisé pour trier les résultats en fonction de la date et de l’heure de leur création, avec l’élément le plus récent en premier.</span><span class="sxs-lookup"><span data-stu-id="155c5-118">A `QueryOption` is used to sort the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="155c5-119">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="155c5-119">Display the results</span></span>

<span data-ttu-id="155c5-120">Commencez par ajouter les instructions `import` suivantes dans **app. Java**.</span><span class="sxs-lookup"><span data-stu-id="155c5-120">Start by adding the following `import` statements in **App.java**.</span></span>

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.util.List;
```

<span data-ttu-id="155c5-121">Ajoutez ensuite la fonction suivante à la `App` classe pour mettre en forme les propriétés [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph dans un format convivial.</span><span class="sxs-lookup"><span data-stu-id="155c5-121">Then add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

```java
private static String formatDateTimeTimeZone(DateTimeTimeZone date) {
    LocalDateTime dateTime = LocalDateTime.parse(date.dateTime);

    return dateTime.format(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT)) + " (" + date.timeZone + ")";
}
```

<span data-ttu-id="155c5-122">Ensuite, ajoutez la fonction suivante à la `App` classe pour obtenir les événements de l’utilisateur et les sortir dans la console.</span><span class="sxs-lookup"><span data-stu-id="155c5-122">Next, add the following function to the `App` class to get the user's events and output them to the console.</span></span>

```java
private static void listCalendarEvents(String accessToken) {
    // Get the user's events
    List<Event> events = Graph.getEvents(accessToken);

    System.out.println("Events:");

    for (Event event : events) {
        System.out.println("Subject: " + event.subject);
        System.out.println("  Organizer: " + event.organizer.emailAddress.name);
        System.out.println("  Start: " + formatDateTimeTimeZone(event.start));
        System.out.println("  End: " + formatDateTimeTimeZone(event.end));
    }

    System.out.println();
}
```

<span data-ttu-id="155c5-123">Enfin, ajoutez le code suivant juste après `// List the calendar` le commentaire dans `main` la fonction.</span><span class="sxs-lookup"><span data-stu-id="155c5-123">Finally, add the following just after the `// List the calendar` comment in the `main` function.</span></span>

```java
listCalendarEvents(accessToken);
```

<span data-ttu-id="155c5-124">Enregistrez toutes vos modifications et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="155c5-124">Save all of your changes and run the app.</span></span> <span data-ttu-id="155c5-125">Choisissez l’option **liste des événements de calendrier** pour afficher la liste des événements de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="155c5-125">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
