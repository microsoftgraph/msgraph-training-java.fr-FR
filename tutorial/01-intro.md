---
ms.openlocfilehash: f77f0518aa22d015cf5e915886f8f24a487e7bab
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612074"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="57cd9-101">Ce didacticiel vous apprend à créer une application de console Java qui utilise l’API Microsoft Graph pour récupérer des informations de calendrier pour un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="57cd9-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="57cd9-102">Si vous préférez télécharger simplement le didacticiel terminé, vous pouvez télécharger ou cloner le [référentiel GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="57cd9-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="57cd9-103">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="57cd9-103">Prerequisites</span></span>

<span data-ttu-id="57cd9-104">Avant de commencer ce didacticiel, le [Kit de développement Java (JDK)](https://java.com/en/download/faq/develop.xml) et [Gradle](https://gradle.org/) doit être installé sur votre ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="57cd9-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="57cd9-105">Si vous n’avez pas le JDK ou Maven, reportez-vous aux liens précédents sur les options de téléchargement.</span><span class="sxs-lookup"><span data-stu-id="57cd9-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span>

<span data-ttu-id="57cd9-106">Vous devez également disposer d’un compte Microsoft personnel disposant d’une boîte aux lettres sur Outlook.com ou d’un compte professionnel ou scolaire Microsoft.</span><span class="sxs-lookup"><span data-stu-id="57cd9-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="57cd9-107">Si vous n’avez pas de compte Microsoft, vous disposez de deux options pour obtenir un compte gratuit :</span><span class="sxs-lookup"><span data-stu-id="57cd9-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="57cd9-108">Vous pouvez vous [inscrire pour obtenir un nouveau compte Microsoft personnel](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="57cd9-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="57cd9-109">Vous pouvez vous [inscrire au programme pour les développeurs office 365](https://developer.microsoft.com/office/dev-program) pour obtenir un abonnement gratuit à Office 365.</span><span class="sxs-lookup"><span data-stu-id="57cd9-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="57cd9-110">Ce didacticiel a été écrit avec OpenJDK version 14.0.0.36 et Gradle 6,3.</span><span class="sxs-lookup"><span data-stu-id="57cd9-110">This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.3.</span></span> <span data-ttu-id="57cd9-111">Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.</span><span class="sxs-lookup"><span data-stu-id="57cd9-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="57cd9-112">Commentaires</span><span class="sxs-lookup"><span data-stu-id="57cd9-112">Feedback</span></span>

<span data-ttu-id="57cd9-113">Veuillez fournir des commentaires sur ce didacticiel dans le [référentiel GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="57cd9-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
