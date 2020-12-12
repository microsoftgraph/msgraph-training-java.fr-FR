---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661080"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez créer une application Java de base.

1. Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet. Exécutez la commande suivante pour créer un projet Gradle.

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. Une fois le projet créé, vérifiez qu’il fonctionne en exécutant la commande suivante pour exécuter l’application dans votre interface CLI.

    ```Shell
    ./gradlew --console plain run
    ```

    Si elle fonctionne, l’application doit produire une sortie `Hello World.` .

## <a name="install-dependencies"></a>Installer les dépendances

Avant de poursuivre, ajoutez des dépendances supplémentaires que vous utiliserez plus tard.

- [Bibliothèque d’authentification Microsoft (MSAL) pour Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) pour authentifier l’utilisateur et acquérir des jetons d’accès.
- [Kit de développement logiciel (SDK) Microsoft Graph pour Java](https://github.com/microsoftgraph/msgraph-sdk-java) pour effectuer des appels à Microsoft Graph.
- [Liaison NOP SLF4J](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) pour supprimer la journalisation à partir de MSAL.

1. Ouvrez **./Build.gradle**. Mettez à jour la `dependencies` section pour ajouter ces dépendances.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. Ajoutez les éléments suivants à la fin de **./Build.gradle**.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

Lors de la prochaine génération du projet, Gradle télécharge ces dépendances.

## <a name="design-the-app"></a>Concevoir l’application

1. Ouvrez le fichier **./src/main/Java/graphtutorial/App.Java** et remplacez son contenu par ce qui suit.

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

    Cela implémente un menu de base et lit le choix de l’utilisateur à partir de la ligne de commande.
