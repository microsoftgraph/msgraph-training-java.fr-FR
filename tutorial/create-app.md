---
ms.openlocfilehash: e731c1caff6986556a1bfec6b669ddcb35a3af4c
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634758"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="34811-101">Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet.</span><span class="sxs-lookup"><span data-stu-id="34811-101">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="34811-102">Exécutez la commande suivante pour créer un projet Maven.</span><span class="sxs-lookup"><span data-stu-id="34811-102">Run the following command to create a new Maven project.</span></span>

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> <span data-ttu-id="34811-103">Vous pouvez entrer des valeurs différentes pour l’ID de`DgroupId` groupe (paramètre) et l'`DartifactId` ID d’artefact (paramètre) que les valeurs spécifiées ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="34811-103">You can enter different values for the group ID (`DgroupId` parameter) and artifact ID (`DartifactId` parameter) than the values specified above.</span></span> <span data-ttu-id="34811-104">L’exemple de code de ce didacticiel suppose que l’ID `com.contoso` de groupe a été utilisé.</span><span class="sxs-lookup"><span data-stu-id="34811-104">The sample code in this tutorial assumes that the group ID `com.contoso` was used.</span></span> <span data-ttu-id="34811-105">Si vous utilisez une autre valeur, veillez à remplacer `com.contoso` dans n’importe quel exemple de code avec votre ID de groupe.</span><span class="sxs-lookup"><span data-stu-id="34811-105">If you use a different value, be sure to replace `com.contoso` in any sample code with your group ID.</span></span>

<span data-ttu-id="34811-106">Lorsque vous y êtes invité, confirmez la configuration, puis attendez que le projet soit créé.</span><span class="sxs-lookup"><span data-stu-id="34811-106">When prompted, confirm the configuration, then wait for the project to be created.</span></span> <span data-ttu-id="34811-107">Une fois le projet créé, vérifiez qu’il fonctionne en exécutant les commandes suivantes pour empaqueter et exécuter l’application dans votre interface CLI.</span><span class="sxs-lookup"><span data-stu-id="34811-107">Once the project is created, verify that it works by running the following commands to package and run the app in your CLI.</span></span>

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

<span data-ttu-id="34811-108">Si elle fonctionne, l’application doit produire `Hello World!`une sortie.</span><span class="sxs-lookup"><span data-stu-id="34811-108">If it works, the app should output `Hello World!`.</span></span> <span data-ttu-id="34811-109">Avant de poursuivre, ajoutez des dépendances supplémentaires que vous utiliserez plus tard.</span><span class="sxs-lookup"><span data-stu-id="34811-109">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="34811-110">[Bibliothèque d’authentification Microsoft (MSAL) pour Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) pour authentifier l’utilisateur et acquérir des jetons d’accès.</span><span class="sxs-lookup"><span data-stu-id="34811-110">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="34811-111">[Kit de développement logiciel (SDK) Microsoft Graph pour Java](https://github.com/microsoftgraph/msgraph-sdk-java) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="34811-111">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="34811-112">[Liaison NOP SLF4J](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) pour supprimer la journalisation à partir de MSAL.</span><span class="sxs-lookup"><span data-stu-id="34811-112">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

<span data-ttu-id="34811-113">Ouvrez **./graphtutorial/POM.xml**.</span><span class="sxs-lookup"><span data-stu-id="34811-113">Open **./graphtutorial/pom.xml**.</span></span> <span data-ttu-id="34811-114">Ajoutez les éléments suivants à `<dependencies>` l’intérieur de l’élément.</span><span class="sxs-lookup"><span data-stu-id="34811-114">Add the following inside the `<dependencies>` element.</span></span>

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

<span data-ttu-id="34811-115">Lors de la prochaine génération du projet, Maven télécharge ces dépendances.</span><span class="sxs-lookup"><span data-stu-id="34811-115">The next time you build the project, Maven will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="34811-116">Concevoir l’application</span><span class="sxs-lookup"><span data-stu-id="34811-116">Design the app</span></span>

<span data-ttu-id="34811-117">Ouvrez le fichier **./graphtutorial/src/main/Java/com/contoso/App.Java** et remplacez son contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="34811-117">Open the **./graphtutorial/src/main/java/com/contoso/App.java** file and replace its contents with the following.</span></span>

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

<span data-ttu-id="34811-118">Cela implémente un menu de base et lit le choix de l’utilisateur à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="34811-118">This implements a basic menu and reads the user's choice from the command line.</span></span>
