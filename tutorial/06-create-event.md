---
ms.openlocfilehash: c03403209c6985dd3235488891a263e447c72149
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661110"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="72433-101">Dans cette section, vous allez ajouter la possibilité de créer des événements dans le calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="72433-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="72433-102">Ouvrez **./graphtutorial/src/main/Java/graphtutorial/Graph.Java** et ajoutez la fonction suivante à la classe **Graph** .</span><span class="sxs-lookup"><span data-stu-id="72433-102">Open **./graphtutorial/src/main/java/graphtutorial/Graph.java** and add the following function to the **Graph** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. <span data-ttu-id="72433-103">Ouvrez **./graphtutorial/src/main/Java/graphtutorial/App.Java** et ajoutez la fonction suivante à la classe **app** .</span><span class="sxs-lookup"><span data-stu-id="72433-103">Open **./graphtutorial/src/main/java/graphtutorial/App.java** and add the following function to the **App** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    <span data-ttu-id="72433-104">Cette fonction invite l’utilisateur à indiquer l’objet, les participants, le début, la fin et le corps, puis utilise ces valeurs pour appeler `Graph.createEvent` .</span><span class="sxs-lookup"><span data-stu-id="72433-104">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `Graph.createEvent`.</span></span>

1. <span data-ttu-id="72433-105">Ajoutez les éléments suivants juste après le `// Create a new event` commentaire dans la `Main` fonction.</span><span class="sxs-lookup"><span data-stu-id="72433-105">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```java
    createEvent(accessToken, user.mailboxSettings.timeZone, input);
    ```

1. <span data-ttu-id="72433-106">Enregistrez toutes vos modifications et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="72433-106">Save all of your changes and run the app.</span></span> <span data-ttu-id="72433-107">Choisissez l’option **Ajouter un événement** .</span><span class="sxs-lookup"><span data-stu-id="72433-107">Choose the **Add an event** option.</span></span> <span data-ttu-id="72433-108">Répondez aux invites pour créer un événement dans le calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="72433-108">Respond to the prompts to create a new event on the user's calendar.</span></span>
