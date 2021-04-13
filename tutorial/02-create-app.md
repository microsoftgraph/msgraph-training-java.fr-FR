---
ms.openlocfilehash: b80de156a5ed1708ccafbaabf34b49b119f4099c
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695799"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1fb63-101">Dans cette section, vous allez créer une application console de Java de base.</span><span class="sxs-lookup"><span data-stu-id="1fb63-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="1fb63-102">Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet.</span><span class="sxs-lookup"><span data-stu-id="1fb63-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="1fb63-103">Exécutez la commande suivante pour créer un projet Gradle.</span><span class="sxs-lookup"><span data-stu-id="1fb63-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="1fb63-104">Une fois le projet créé, vérifiez qu'il fonctionne en exécutant la commande suivante pour exécuter l'application dans votre CLI.</span><span class="sxs-lookup"><span data-stu-id="1fb63-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="1fb63-105">Si elle fonctionne, l'application doit être `Hello World.` sortie.</span><span class="sxs-lookup"><span data-stu-id="1fb63-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="1fb63-106">Installer les dépendances</span><span class="sxs-lookup"><span data-stu-id="1fb63-106">Install dependencies</span></span>

<span data-ttu-id="1fb63-107">Avant de passer à autre chose, ajoutez des dépendances supplémentaires que vous utiliserez ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="1fb63-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="1fb63-108">[Bibliothèque cliente Azure Identity pour Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) authentifier l'utilisateur et acquérir des jetons d'accès.</span><span class="sxs-lookup"><span data-stu-id="1fb63-108">[Azure Identity client library for Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="1fb63-109">[Microsoft Graph SDK pour Java](https://github.com/microsoftgraph/msgraph-sdk-java) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="1fb63-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>

1. <span data-ttu-id="1fb63-110">Ouvrez **./build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="1fb63-110">Open **./build.gradle**.</span></span> <span data-ttu-id="1fb63-111">Mettez à jour `dependencies` la section pour ajouter ces dépendances.</span><span class="sxs-lookup"><span data-stu-id="1fb63-111">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-8":::

1. <span data-ttu-id="1fb63-112">Ajoutez ce qui suit à la fin **de ./build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="1fb63-112">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="1fb63-113">La prochaine fois que vous créerez le projet, Gradle téléchargera ces dépendances.</span><span class="sxs-lookup"><span data-stu-id="1fb63-113">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="1fb63-114">Concevoir l’application</span><span class="sxs-lookup"><span data-stu-id="1fb63-114">Design the app</span></span>

1. <span data-ttu-id="1fb63-115">Ouvrez **le fichier ./src/main/java/graphtutorial/App.java** et remplacez son contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="1fb63-115">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

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
                System.out.println("2. View this week's calendar");
                System.out.println("3. Add an event");

                try {
                    choice = input.nextInt();
                } catch (InputMismatchException ex) {
                    // Skip over non-integer input
                }

                input.nextLine();

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
                    case 3:
                        // Create a new event
                        break;
                    default:
                        System.out.println("Invalid choice");
                }
            }

            input.close();
        }
    }
    ```

    <span data-ttu-id="1fb63-116">Cela implémente un menu de base et lit le choix de l'utilisateur à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="1fb63-116">This implements a basic menu and reads the user's choice from the command line.</span></span>
