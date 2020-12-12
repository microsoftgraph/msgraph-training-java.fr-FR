---
ms.openlocfilehash: 50206b77504979c1cf67b4d0daa0b6f6576bde32
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661052"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="d97ff-101">Exécution du projet terminé</span><span class="sxs-lookup"><span data-stu-id="d97ff-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d97ff-102">Conditions requises</span><span class="sxs-lookup"><span data-stu-id="d97ff-102">Prerequisites</span></span>

<span data-ttu-id="d97ff-103">Pour exécuter le projet terminé dans ce dossier, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="d97ff-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="d97ff-104">[Kit de développement Java (JDK)](https://java.com/en/download/faq/develop.xml) et [Gradle](https://gradle.org/) installé sur votre ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="d97ff-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="d97ff-105">Si vous n’avez pas le JDK ou Gradle, reportez-vous aux liens précédents sur les options de téléchargement.</span><span class="sxs-lookup"><span data-stu-id="d97ff-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span> <span data-ttu-id="d97ff-106">(**Remarque :** ce didacticiel a été écrit avec openjdk version 14.0.0.36 et Gradle 6.7.1.</span><span class="sxs-lookup"><span data-stu-id="d97ff-106">(**Note:** This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.7.1.</span></span> <span data-ttu-id="d97ff-107">Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.)</span><span class="sxs-lookup"><span data-stu-id="d97ff-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="d97ff-108">Un compte professionnel ou scolaire Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d97ff-108">A Microsoft work or school account.</span></span>

<span data-ttu-id="d97ff-109">Si vous n’avez pas de compte Microsoft, vous pouvez vous [inscrire au programme pour les développeurs office 365](https://developer.microsoft.com/office/dev-program) pour obtenir un abonnement gratuit à Office 365.</span><span class="sxs-lookup"><span data-stu-id="d97ff-109">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="d97ff-110">Enregistrer une application Web avec le centre d’administration Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d97ff-110">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="d97ff-111">Ouvrez un navigateur et accédez au [Centre d’administration Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d97ff-111">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="d97ff-112">Connectez-vous à l’aide d’un **compte personnel** (compte Microsoft) ou d’un **compte professionnel ou scolaire**.</span><span class="sxs-lookup"><span data-stu-id="d97ff-112">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="d97ff-113">Sélectionnez **Azure Active Directory** dans le volet de navigation gauche, puis sélectionnez **Inscriptions d’applications** sous **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="d97ff-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="d97ff-114">Une capture d’écran des inscriptions d’applications</span><span class="sxs-lookup"><span data-stu-id="d97ff-114">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="d97ff-115">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="d97ff-115">Select **New registration**.</span></span> <span data-ttu-id="d97ff-116">Sur la page **Inscrire une application**, définissez les valeurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="d97ff-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="d97ff-117">Définissez le **Nom** sur `Java Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="d97ff-117">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="d97ff-118">Définissez les types de comptes **pris en charge** sur **les comptes dans n’importe quel annuaire d’organisation**.</span><span class="sxs-lookup"><span data-stu-id="d97ff-118">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="d97ff-119">Laissez **Redirect URI** vide.</span><span class="sxs-lookup"><span data-stu-id="d97ff-119">Leave **Redirect URI** empty.</span></span>

    ![Capture d’écran de la page Inscrire une application](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="d97ff-121">Choisissez **Inscrire**.</span><span class="sxs-lookup"><span data-stu-id="d97ff-121">Choose **Register**.</span></span> <span data-ttu-id="d97ff-122">Sur la page **didacticiel de Graph Java** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="d97ff-122">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Une capture d’écran de l’ID d’application de la nouvelle inscription d'application](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="d97ff-124">Sélectionnez le lien **Ajouter un URI de redirection** .</span><span class="sxs-lookup"><span data-stu-id="d97ff-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="d97ff-125">Sur la page **URI de redirection** , recherchez la section **URI de redirection suggérée pour les clients publics (mobile, bureau)** .</span><span class="sxs-lookup"><span data-stu-id="d97ff-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="d97ff-126">Sélectionnez l' `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span><span class="sxs-lookup"><span data-stu-id="d97ff-126">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Capture d’écran de la page des URI de redirection](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="d97ff-128">Recherchez la section **type de client par défaut** et modifiez le paramètre **traiter l’application comme un client public** sur **Oui**, puis cliquez sur **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="d97ff-128">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Capture d’écran de la section type de client par défaut](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="d97ff-130">Configurer l’exemple</span><span class="sxs-lookup"><span data-stu-id="d97ff-130">Configure the sample</span></span>

1. <span data-ttu-id="d97ff-131">Renommez le `oAuth.properties.example` fichier `oAuth.properties` .</span><span class="sxs-lookup"><span data-stu-id="d97ff-131">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
1. <span data-ttu-id="d97ff-132">Modifiez le `oAuth.properties` fichier et effectuez les modifications suivantes.</span><span class="sxs-lookup"><span data-stu-id="d97ff-132">Edit the `oAuth.properties` file and make the following changes.</span></span>
    1. <span data-ttu-id="d97ff-133">Remplacez `YOUR_APP_ID_HERE` par l' **ID d’application** que vous avez obtenu à partir du portail d’inscription des applications.</span><span class="sxs-lookup"><span data-stu-id="d97ff-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="d97ff-134">Création et exécution de l’exemple</span><span class="sxs-lookup"><span data-stu-id="d97ff-134">Build and run the sample</span></span>

<span data-ttu-id="d97ff-135">Dans votre interface de ligne de commande (CLI), accédez au répertoire du projet et exécutez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="d97ff-135">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
./gradlew --console plain run
```
