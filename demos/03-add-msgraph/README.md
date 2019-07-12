---
ms.openlocfilehash: a890c296f4269b674356118463578c9b79001b0e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634716"
---
# <a name="how-to-run-the-completed-project"></a>Exécution du projet terminé

## <a name="prerequisites"></a>Conditions préalables

Pour exécuter le projet terminé dans ce dossier, vous avez besoin des éléments suivants:

- [Kit de développement Java (JDK)](https://java.com/en/download/faq/develop.xml) et [Maven](https://maven.apache.org/) installé sur votre ordinateur de développement. Si vous n’avez pas le JDK ou Maven, reportez-vous aux liens précédents sur les options de téléchargement. (**Remarque:** ce didacticiel a été écrit avec openjdk version 12.0.1 et Maven 3.6.1. Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.)
- Un compte professionnel ou scolaire Microsoft.

Si vous n’avez pas de compte Microsoft, vous pouvez vous [inscrire au programme pour les développeurs office 365](https://developer.microsoft.com/office/dev-program) pour obtenir un abonnement gratuit à Office 365.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Enregistrer une application Web avec le centre d’administration Azure Active Directory

1. Ouvrez un navigateur et accédez au [Centre d’administration Azure Active Directory](https://aad.portal.azure.com). Connectez-vous à l’aide d’un **compte personnel** (compte Microsoft) ou d’un **compte professionnel ou scolaire**.

1. Sélectionnez **Azure Active Directory** dans le volet de navigation de gauche, puis sélectionnez **inscriptions des applications** sous **gérer**.

    ![Capture d’écran des inscriptions d’application ](/tutorial/images/aad-portal-app-registrations.png)

1. Sélectionnez **Nouvelle inscription**. Sur la page **Inscrire une application**, définissez les valeurs comme suit.

    - Définissez le **Nom** sur `Java Graph Tutorial`.
    - Définissez les types de comptes **pris en charge** sur **les comptes dans n’importe quel annuaire d’organisation**.
    - Laissez **Redirect URI** vide.

    ![Capture d’écran de la page inscrire une application](/tutorial/images/aad-register-an-app.png)

1. Choisissez **Inscrire**. Sur la page **didacticiel de Graph Java** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin à l’étape suivante.

    ![Capture d’écran de l’ID d’application de la nouvelle inscription de l’application](/tutorial/images/aad-application-id.png)

1. Sélectionnez le lien **Ajouter un URI de redirection** . Sur la page **URI** de redirection, recherchez la section **URI de redirection suggérée pour les clients publics (mobile, bureau)** . Sélectionnez l' `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.

    ![Capture d’écran de la page des URI de redirection](/tutorial/images/aad-redirect-uris.png)

1. Recherchez la section **type de client par défaut** et modifiez le paramètre **traiter l’application comme un client public** sur **Oui**, puis cliquez sur **Enregistrer**.

    ![Capture d’écran de la section type de client par défaut](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>Configurer l’exemple

1. Renommez `oAuth.properties.example` le fichier `oAuth.properties`.
1. Modifiez le `oAuth.properties` fichier et effectuez les modifications suivantes.
    1. Remplacez `YOUR_APP_ID_HERE` par l' **ID d’application** que vous avez obtenu à partir du portail d’inscription des applications.

## <a name="build-and-run-the-sample"></a>Création et exécution de l’exemple

Dans votre interface de ligne de commande (CLI), accédez au répertoire du projet et exécutez les commandes suivantes.

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```
