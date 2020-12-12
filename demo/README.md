---
ms.openlocfilehash: 50206b77504979c1cf67b4d0daa0b6f6576bde32
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661052"
---
# <a name="how-to-run-the-completed-project"></a>Exécution du projet terminé

## <a name="prerequisites"></a>Conditions requises

Pour exécuter le projet terminé dans ce dossier, vous avez besoin des éléments suivants :

- [Kit de développement Java (JDK)](https://java.com/en/download/faq/develop.xml) et [Gradle](https://gradle.org/) installé sur votre ordinateur de développement. Si vous n’avez pas le JDK ou Gradle, reportez-vous aux liens précédents sur les options de téléchargement. (**Remarque :** ce didacticiel a été écrit avec openjdk version 14.0.0.36 et Gradle 6.7.1. Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.)
- Un compte professionnel ou scolaire Microsoft.

Si vous n’avez pas de compte Microsoft, vous pouvez vous [inscrire au programme pour les développeurs office 365](https://developer.microsoft.com/office/dev-program) pour obtenir un abonnement gratuit à Office 365.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Enregistrer une application Web avec le centre d’administration Azure Active Directory

1. Ouvrez un navigateur et accédez au [Centre d’administration Azure Active Directory](https://aad.portal.azure.com). Connectez-vous à l’aide d’un **compte personnel** (compte Microsoft) ou d’un **compte professionnel ou scolaire**.

1. Sélectionnez **Azure Active Directory** dans le volet de navigation gauche, puis sélectionnez **Inscriptions d’applications** sous **Gérer**.

    ![Une capture d’écran des inscriptions d’applications ](/tutorial/images/aad-portal-app-registrations.png)

1. Sélectionnez **Nouvelle inscription**. Sur la page **Inscrire une application**, définissez les valeurs comme suit.

    - Définissez le **Nom** sur `Java Graph Tutorial`.
    - Définissez les types de comptes **pris en charge** sur **les comptes dans n’importe quel annuaire d’organisation**.
    - Laissez **Redirect URI** vide.

    ![Capture d’écran de la page Inscrire une application](/tutorial/images/aad-register-an-app.png)

1. Choisissez **Inscrire**. Sur la page **didacticiel de Graph Java** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin à l’étape suivante.

    ![Une capture d’écran de l’ID d’application de la nouvelle inscription d'application](/tutorial/images/aad-application-id.png)

1. Sélectionnez le lien **Ajouter un URI de redirection** . Sur la page **URI de redirection** , recherchez la section **URI de redirection suggérée pour les clients publics (mobile, bureau)** . Sélectionnez l' `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.

    ![Capture d’écran de la page des URI de redirection](/tutorial/images/aad-redirect-uris.png)

1. Recherchez la section **type de client par défaut** et modifiez le paramètre **traiter l’application comme un client public** sur **Oui**, puis cliquez sur **Enregistrer**.

    ![Capture d’écran de la section type de client par défaut](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>Configurer l’exemple

1. Renommez le `oAuth.properties.example` fichier `oAuth.properties` .
1. Modifiez le `oAuth.properties` fichier et effectuez les modifications suivantes.
    1. Remplacez `YOUR_APP_ID_HERE` par l' **ID d’application** que vous avez obtenu à partir du portail d’inscription des applications.

## <a name="build-and-run-the-sample"></a>Création et exécution de l’exemple

Dans votre interface de ligne de commande (CLI), accédez au répertoire du projet et exécutez les commandes suivantes.

```Shell
./gradlew --console plain run
```
