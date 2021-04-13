---
ms.openlocfilehash: bbb71d370fea90a3f7d2eae2f27d04f7e19d3d94
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695785"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="503a6-101">Dans cette section, vous allez ajouter la possibilité de créer des événements sur le calendrier de l'utilisateur.</span><span class="sxs-lookup"><span data-stu-id="503a6-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="503a6-102">Ouvrez **./graphtutorial/src/main/java/graphtutorial/Graph.java** et ajoutez la fonction suivante à la **classe Graph.**</span><span class="sxs-lookup"><span data-stu-id="503a6-102">Open **./graphtutorial/src/main/java/graphtutorial/Graph.java** and add the following function to the **Graph** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. <span data-ttu-id="503a6-103">Ouvrez **./graphtutorial/src/main/java/graphtutorial/App.java** et ajoutez la fonction suivante à la **classe App.**</span><span class="sxs-lookup"><span data-stu-id="503a6-103">Open **./graphtutorial/src/main/java/graphtutorial/App.java** and add the following function to the **App** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    <span data-ttu-id="503a6-104">Cette fonction demande à l'utilisateur l'objet, les participants, le début, la fin et le corps, puis utilise ces valeurs pour appeler `Graph.createEvent` .</span><span class="sxs-lookup"><span data-stu-id="503a6-104">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `Graph.createEvent`.</span></span>

1. <span data-ttu-id="503a6-105">Ajoutez ce qui suit juste après `// Create a new event` le commentaire dans la `Main` fonction.</span><span class="sxs-lookup"><span data-stu-id="503a6-105">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```java
    createEvent(user.mailboxSettings.timeZone, input);
    ```

1. <span data-ttu-id="503a6-106">Enregistrez toutes vos modifications et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="503a6-106">Save all of your changes and run the app.</span></span> <span data-ttu-id="503a6-107">Choisissez **l'option Ajouter un** événement.</span><span class="sxs-lookup"><span data-stu-id="503a6-107">Choose the **Add an event** option.</span></span> <span data-ttu-id="503a6-108">Répondez aux invites pour créer un événement sur le calendrier de l'utilisateur.</span><span class="sxs-lookup"><span data-stu-id="503a6-108">Respond to the prompts to create a new event on the user's calendar.</span></span>
