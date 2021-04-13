---
ms.openlocfilehash: 397d564fc3389f341e06977bd4cba861dd1c9e5b
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695806"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez incorporer Microsoft Graph dans l'application. Pour cette application, vous allez utiliser le [SDK Microsoft Graph pour](https://github.com/microsoftgraph/msgraph-sdk-java) Java pour effectuer des appels à Microsoft Graph.

## <a name="get-user-details"></a>Obtenir les détails de l’utilisateur

1. Créez un fichier dans le répertoire **./graphtutorial/src/main/java/graphtutorial** nommé **Graph.java** et ajoutez le code suivant.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetUserSnippet":::

1. Ajoutez `import` l'instruction suivante en haut **de App.java**.

    ```java
    import com.microsoft.graph.models.User;
    ```

1. Ajoutez le code suivant **dans App.java** juste avant la ligne pour obtenir l'utilisateur et afficher le nom complet `Scanner input = new Scanner(System.in);` de l'utilisateur.

    ```java
    // Greet the user
    User user = Graph.getUser();
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. Exécutez l’application. Une fois que vous vous connectez à l'application, vous êtes le bienvenu par son nom.

## <a name="get-calendar-events-from-outlook"></a>Récupérer les événements de calendrier à partir d’Outlook

1. Ajoutez la fonction suivante à la classe dans Graph.java pour obtenir des événements à partir `Graph` du calendrier de l'utilisateur. 

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

Que fait ce code ?

- L’URL qui sera appelée est `/me/calendarview`.
  - `QueryOption` sont utilisés pour ajouter les paramètres et définir le début et la `startDateTime` `endDateTime` fin de l'affichage Calendrier.
  - Un `QueryOption` objet est utilisé pour ajouter le `$orderby` paramètre, triant les résultats par heure de début.
  - Un `HeaderOption` objet est utilisé pour ajouter l'en-tête, ce qui a pour effet d'ajuster les heures de début et de fin au `Prefer: outlook.timezone` fuseau horaire de l'utilisateur.
  - La fonction limite les champs renvoyés pour chaque événement à ceux que `select` l'application utilisera réellement.
  - La `top` fonction limite le nombre d'événements dans la réponse à un maximum de 25.
- La fonction est utilisée pour demander des pages de résultats supplémentaires s'il y a plus de 25 événements `getNextPage` dans la semaine en cours.

1. Créez un fichier dans le répertoire **./graphtutorial/src/main/java/graphtutorial** nommé **GraphToIana.java** et ajoutez le code suivant.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    Cette classe implémente une recherche simple pour convertir des noms de fuseau horaire Windows en identificateurs IANA et pour générer un **ZoneId** basé sur un nom de fuseau horaire Windows.

## <a name="display-the-results"></a>Afficher les résultats

1. Ajoutez les `import` instructions suivantes **dans App.java**.

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
    import com.microsoft.graph.models.DateTimeTimeZone;
    import com.microsoft.graph.models.Event;
    ```

1. Ajoutez la fonction suivante à la classe pour mettre en forme les `App` [propriétés dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph dans un format convivial.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. Ajoutez la fonction suivante à la classe pour obtenir les événements de l'utilisateur et les sortir `App` dans la console.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. Ajoutez ce qui suit juste après `// List the calendar` le commentaire dans la `main` fonction.

    ```java
    listCalendarEvents(user.mailboxSettings.timeZone);
    ```

1. Enregistrez toutes vos modifications, créez l'application, puis exécutez-la. Choisissez **l'option Lister les** événements de calendrier pour voir la liste des événements de l'utilisateur.

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
