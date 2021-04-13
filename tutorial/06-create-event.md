---
ms.openlocfilehash: bbb71d370fea90a3f7d2eae2f27d04f7e19d3d94
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695785"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez ajouter la possibilité de créer des événements sur le calendrier de l'utilisateur.

1. Ouvrez **./graphtutorial/src/main/java/graphtutorial/Graph.java** et ajoutez la fonction suivante à la **classe Graph.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. Ouvrez **./graphtutorial/src/main/java/graphtutorial/App.java** et ajoutez la fonction suivante à la **classe App.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    Cette fonction demande à l'utilisateur l'objet, les participants, le début, la fin et le corps, puis utilise ces valeurs pour appeler `Graph.createEvent` .

1. Ajoutez ce qui suit juste après `// Create a new event` le commentaire dans la `Main` fonction.

    ```java
    createEvent(user.mailboxSettings.timeZone, input);
    ```

1. Enregistrez toutes vos modifications et exécutez l'application. Choisissez **l'option Ajouter un** événement. Répondez aux invites pour créer un événement sur le calendrier de l'utilisateur.
