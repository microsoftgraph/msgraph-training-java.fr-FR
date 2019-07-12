---
ms.openlocfilehash: 7118c8300b17adb66893324dd2f2269d9fb8d00b
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634702"
---
# <a name="completed-module-add-azure-ad-authentication"></a><span data-ttu-id="7485f-101">Module terminé: ajouter l’authentification Azure AD</span><span class="sxs-lookup"><span data-stu-id="7485f-101">Completed module: Add Azure AD authentication</span></span>

<span data-ttu-id="7485f-102">La version du projet dans ce répertoire reflète l’exécution du didacticiel via l' [authentification Azure ad](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=3).</span><span class="sxs-lookup"><span data-stu-id="7485f-102">The version of the project in this directory reflects completing the tutorial up through [Add Azure AD authentication](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=3).</span></span> <span data-ttu-id="7485f-103">Si vous utilisez cette version du projet, vous devez effectuer le reste du didacticiel en commençant par obtenir les [données du calendrier](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=4).</span><span class="sxs-lookup"><span data-stu-id="7485f-103">If you use this version of the project, you need to complete the rest of the tutorial starting at [Get calendar data](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=4).</span></span>

> <span data-ttu-id="7485f-104">**Remarque:** Nous partons du principe que vous avez déjà inscrit une application dans le portail d’inscription des applications, comme indiqué dans [enregistrer l’application dans le portail](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=2).</span><span class="sxs-lookup"><span data-stu-id="7485f-104">**Note:** It is assumed that you have already registered an application in the app registration portal as specified in [Register the app in the portal](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=2).</span></span> <span data-ttu-id="7485f-105">Vous devez configurer cette version de l’exemple comme suit:</span><span class="sxs-lookup"><span data-stu-id="7485f-105">You need to configure this version of the sample as follows:</span></span>
>
> 1. <span data-ttu-id="7485f-106">Renommez `oAuth.properties.example` le fichier `oAuth.properties`.</span><span class="sxs-lookup"><span data-stu-id="7485f-106">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
> 1. <span data-ttu-id="7485f-107">Modifiez le `oAuth.properties` fichier et effectuez les modifications suivantes.</span><span class="sxs-lookup"><span data-stu-id="7485f-107">Edit the `oAuth.properties` file and make the following changes.</span></span>
>     1. <span data-ttu-id="7485f-108">Remplacez `YOUR_APP_ID_HERE` par l' **ID d’application** que vous avez obtenu à partir du portail d’inscription des applications.</span><span class="sxs-lookup"><span data-stu-id="7485f-108">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
