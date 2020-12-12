---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661080"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c3aad-101">Dans cette section, vous allez créer une application Java de base.</span><span class="sxs-lookup"><span data-stu-id="c3aad-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="c3aad-102">Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet.</span><span class="sxs-lookup"><span data-stu-id="c3aad-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="c3aad-103">Exécutez la commande suivante pour créer un projet Gradle.</span><span class="sxs-lookup"><span data-stu-id="c3aad-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="c3aad-104">Une fois le projet créé, vérifiez qu’il fonctionne en exécutant la commande suivante pour exécuter l’application dans votre interface CLI.</span><span class="sxs-lookup"><span data-stu-id="c3aad-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="c3aad-105">Si elle fonctionne, l’application doit produire une sortie `Hello World.` .</span><span class="sxs-lookup"><span data-stu-id="c3aad-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="c3aad-106">Installer les dépendances</span><span class="sxs-lookup"><span data-stu-id="c3aad-106">Install dependencies</span></span>

<span data-ttu-id="c3aad-107">Avant de poursuivre, ajoutez des dépendances supplémentaires que vous utiliserez plus tard.</span><span class="sxs-lookup"><span data-stu-id="c3aad-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="c3aad-108">[Bibliothèque d’authentification Microsoft (MSAL) pour Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) pour authentifier l’utilisateur et acquérir des jetons d’accès.</span><span class="sxs-lookup"><span data-stu-id="c3aad-108">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="c3aad-109">[Kit de développement logiciel (SDK) Microsoft Graph pour Java](https://github.com/microsoftgraph/msgraph-sdk-java) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c3aad-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="c3aad-110">[Liaison NOP SLF4J](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) pour supprimer la journalisation à partir de MSAL.</span><span class="sxs-lookup"><span data-stu-id="c3aad-110">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

1. <span data-ttu-id="c3aad-111">Ouvrez **./Build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="c3aad-111">Open **./build.gradle**.</span></span> <span data-ttu-id="c3aad-112">Mettez à jour la `dependencies` section pour ajouter ces dépendances.</span><span class="sxs-lookup"><span data-stu-id="c3aad-112">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. <span data-ttu-id="c3aad-113">Ajoutez les éléments suivants à la fin de **./Build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="c3aad-113">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="c3aad-114">Lors de la prochaine génération du projet, Gradle télécharge ces dépendances.</span><span class="sxs-lookup"><span data-stu-id="c3aad-114">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="c3aad-115">Concevoir l’application</span><span class="sxs-lookup"><span data-stu-id="c3aad-115">Design the app</span></span>

1. <span data-ttu-id="c3aad-116">Ouvrez le fichier **./src/main/Java/graphtutorial/App.Java** et remplacez son contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="c3aad-116">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

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

    <span data-ttu-id="c3aad-117">Cela implémente un menu de base et lit le choix de l’utilisateur à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="c3aad-117">This implements a basic menu and reads the user's choice from the command line.</span></span>
