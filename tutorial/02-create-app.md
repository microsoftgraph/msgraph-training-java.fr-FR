---
ms.openlocfilehash: b80de156a5ed1708ccafbaabf34b49b119f4099c
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695799"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez créer une application console de Java de base.

1. Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet. Exécutez la commande suivante pour créer un projet Gradle.

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. Une fois le projet créé, vérifiez qu'il fonctionne en exécutant la commande suivante pour exécuter l'application dans votre CLI.

    ```Shell
    ./gradlew --console plain run
    ```

    Si elle fonctionne, l'application doit être `Hello World.` sortie.

## <a name="install-dependencies"></a>Installer les dépendances

Avant de passer à autre chose, ajoutez des dépendances supplémentaires que vous utiliserez ultérieurement.

- [Bibliothèque cliente Azure Identity pour Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) authentifier l'utilisateur et acquérir des jetons d'accès.
- [Microsoft Graph SDK pour Java](https://github.com/microsoftgraph/msgraph-sdk-java) pour effectuer des appels à Microsoft Graph.

1. Ouvrez **./build.gradle**. Mettez à jour `dependencies` la section pour ajouter ces dépendances.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-8":::

1. Ajoutez ce qui suit à la fin **de ./build.gradle**.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

La prochaine fois que vous créerez le projet, Gradle téléchargera ces dépendances.

## <a name="design-the-app"></a>Concevoir l’application

1. Ouvrez **le fichier ./src/main/java/graphtutorial/App.java** et remplacez son contenu par ce qui suit.

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

    Cela implémente un menu de base et lit le choix de l'utilisateur à partir de la ligne de commande.
