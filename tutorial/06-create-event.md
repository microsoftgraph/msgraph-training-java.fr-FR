---
ms.openlocfilehash: c03403209c6985dd3235488891a263e447c72149
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661110"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez ajouter la possibilité de créer des événements dans le calendrier de l’utilisateur.

1. Ouvrez **./graphtutorial/src/main/Java/graphtutorial/Graph.Java** et ajoutez la fonction suivante à la classe **Graph** .

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. Ouvrez **./graphtutorial/src/main/Java/graphtutorial/App.Java** et ajoutez la fonction suivante à la classe **app** .

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    Cette fonction invite l’utilisateur à indiquer l’objet, les participants, le début, la fin et le corps, puis utilise ces valeurs pour appeler `Graph.createEvent` .

1. Ajoutez les éléments suivants juste après le `// Create a new event` commentaire dans la `Main` fonction.

    ```java
    createEvent(accessToken, user.mailboxSettings.timeZone, input);
    ```

1. Enregistrez toutes vos modifications et exécutez l’application. Choisissez l’option **Ajouter un événement** . Répondez aux invites pour créer un événement dans le calendrier de l’utilisateur.
