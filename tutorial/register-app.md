---
ms.openlocfilehash: 97763f135ab7529cfbcebf2eaf4d4d98d234fe10
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634730"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez créer une nouvelle application Azure AD à l’aide du centre d’administration Azure Active Directory.

1. Ouvrez un navigateur, accédez au [Centre d’administration Azure Active Directory ](https://aad.portal.azure.com) et connectez-vous à l’aide d’un **compte personnel** (ou compte Microsoft) ou d’un **compte professionnel ou scolaire**.

1. Sélectionnez **Azure Active Directory** dans le volet de navigation de gauche, puis sélectionnez **inscriptions des applications** sous **gérer**.

    ![Capture d’écran des inscriptions d’application ](./images/aad-portal-app-registrations.png)

1. Sélectionnez **Nouvelle inscription**. Sur la page **Inscrire une application**, définissez les valeurs comme suit.

    - Définissez le **Nom** sur `Java Graph Tutorial`.
    - Définissez les types de comptes **pris en charge** sur **les comptes dans n’importe quel annuaire d’organisation**.
    - Laissez **Redirect URI** vide.

    ![Capture d’écran de la page inscrire une application](./images/aad-register-an-app.png)

1. Choisissez **Inscrire**. Sur la page **didacticiel de Graph Java** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin à l’étape suivante.

    ![Capture d’écran de l’ID d’application de la nouvelle inscription de l’application](./images/aad-application-id.png)

1. Sélectionnez le lien **Ajouter un URI de redirection** . Sur la page **URI** de redirection, recherchez la section **URI de redirection suggérée pour les clients publics (mobile, bureau)** . Sélectionnez l' `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.

    ![Capture d’écran de la page des URI de redirection](./images/aad-redirect-uris.png)

1. Recherchez la section **type de client par défaut** et modifiez le paramètre **traiter l’application comme un client public** sur **Oui**, puis cliquez sur **Enregistrer**.

    ![Capture d’écran de la section type de client par défaut](./images/aad-default-client-type.png)
