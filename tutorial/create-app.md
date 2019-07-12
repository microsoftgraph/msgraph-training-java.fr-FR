---
ms.openlocfilehash: e731c1caff6986556a1bfec6b669ddcb35a3af4c
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634758"
---
<!-- markdownlint-disable MD002 MD041 -->

Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet. Exécutez la commande suivante pour créer un projet Maven.

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> Vous pouvez entrer des valeurs différentes pour l’ID de`DgroupId` groupe (paramètre) et l'`DartifactId` ID d’artefact (paramètre) que les valeurs spécifiées ci-dessus. L’exemple de code de ce didacticiel suppose que l’ID `com.contoso` de groupe a été utilisé. Si vous utilisez une autre valeur, veillez à remplacer `com.contoso` dans n’importe quel exemple de code avec votre ID de groupe.

Lorsque vous y êtes invité, confirmez la configuration, puis attendez que le projet soit créé. Une fois le projet créé, vérifiez qu’il fonctionne en exécutant les commandes suivantes pour empaqueter et exécuter l’application dans votre interface CLI.

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

Si elle fonctionne, l’application doit produire `Hello World!`une sortie. Avant de poursuivre, ajoutez des dépendances supplémentaires que vous utiliserez plus tard.

- [Bibliothèque d’authentification Microsoft (MSAL) pour Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) pour authentifier l’utilisateur et acquérir des jetons d’accès.
- [Kit de développement logiciel (SDK) Microsoft Graph pour Java](https://github.com/microsoftgraph/msgraph-sdk-java) pour effectuer des appels à Microsoft Graph.
- [Liaison NOP SLF4J](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) pour supprimer la journalisation à partir de MSAL.

Ouvrez **./graphtutorial/POM.xml**. Ajoutez les éléments suivants à `<dependencies>` l’intérieur de l’élément.

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-nop</artifactId>
  <version>1.8.0-beta4</version>
</dependency>

<dependency>
  <groupId>com.microsoft.graph</groupId>
  <artifactId>microsoft-graph</artifactId>
  <version>1.4.0</version>
</dependency>

<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>msal4j</artifactId>
  <version>0.4.0-preview</version>
</dependency>
```

Lors de la prochaine génération du projet, Maven télécharge ces dépendances.

## <a name="design-the-app"></a>Concevoir l’application

Ouvrez le fichier **./graphtutorial/src/main/Java/com/contoso/App.Java** et remplacez son contenu par ce qui suit.

```java
package com.contoso;

import java.util.InputMismatchException;
import java.util.Scanner;

/**
 * Graph Tutorial
 *
 */
public class App {
    public static void main(String[] args) {
        System.out.println("Java Graph Tutorial");
        System.out.println();

        Scanner input = new Scanner(System.in);

        int choice = -1;

        while (choice != 0) {
            System.out.println("Please choose one of the following options:");
            System.out.println("0. Exit");
            System.out.println("1. Display access token");
            System.out.println("2. List calendar events");

            try {
                choice = input.nextInt();
            } catch (InputMismatchException ex) {
                // Skip over non-integer input
                input.nextLine();
            }

            // Process user choice
            switch(choice) {
                case 0:
                    // Exit the program
                    System.out.println("Goodbye...");
                    break;
                case 1:
                    // Display access token
                case 2:
                    // List the calendar
                    break;
                default:
                    System.out.println("Invalid choice");
            }
        }

        input.close();
    }
}
```

Cela implémente un menu de base et lit le choix de l’utilisateur à partir de la ligne de commande.
