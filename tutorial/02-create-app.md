---
ms.openlocfilehash: 381e4166f07e1dbc51c072645f17002e43f6cc16
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612018"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4a3ca-101">Dans cette section, vous allez créer une application Java de base.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="4a3ca-102">Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="4a3ca-103">Exécutez la commande suivante pour créer un projet Gradle.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="4a3ca-104">Une fois le projet créé, vérifiez qu’il fonctionne en exécutant la commande suivante pour exécuter l’application dans votre interface CLI.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="4a3ca-105">Si elle fonctionne, l’application doit produire `Hello World.`une sortie.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="4a3ca-106">Installer les dépendances</span><span class="sxs-lookup"><span data-stu-id="4a3ca-106">Install dependencies</span></span>

<span data-ttu-id="4a3ca-107">Avant de poursuivre, ajoutez des dépendances supplémentaires que vous utiliserez plus tard.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="4a3ca-108">[Bibliothèque d’authentification Microsoft (MSAL) pour Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) pour authentifier l’utilisateur et acquérir des jetons d’accès.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-108">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="4a3ca-109">[Kit de développement logiciel (SDK) Microsoft Graph pour Java](https://github.com/microsoftgraph/msgraph-sdk-java) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="4a3ca-110">[Liaison NOP SLF4J](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) pour supprimer la journalisation à partir de MSAL.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-110">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

1. <span data-ttu-id="4a3ca-111">Ouvrez **./Build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-111">Open **./build.gradle**.</span></span> <span data-ttu-id="4a3ca-112">Mettez à `dependencies` jour la section pour ajouter ces dépendances.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-112">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. <span data-ttu-id="4a3ca-113">Ajoutez les éléments suivants à la fin de **./Build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-113">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="4a3ca-114">Lors de la prochaine génération du projet, Gradle télécharge ces dépendances.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-114">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="4a3ca-115">Concevoir l’application</span><span class="sxs-lookup"><span data-stu-id="4a3ca-115">Design the app</span></span>

1. <span data-ttu-id="4a3ca-116">Ouvrez le fichier **./src/main/Java/graphtutorial/App.Java** et remplacez son contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-116">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

    ```java
    package graphtutorial;

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
                        break;
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

    <span data-ttu-id="4a3ca-117">Cela implémente un menu de base et lit le choix de l’utilisateur à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="4a3ca-117">This implements a basic menu and reads the user's choice from the command line.</span></span>
