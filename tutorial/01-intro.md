---
ms.openlocfilehash: 3eec5a727d21e481525e2a2892e33a0a30a9bb25
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018805"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b95fd-101">Ce didacticiel vous apprend à créer une application de console Java qui utilise l’API Microsoft Graph pour récupérer des informations de calendrier pour un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b95fd-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="b95fd-102">Si vous préférez télécharger simplement le didacticiel terminé, vous pouvez télécharger ou cloner le [référentiel GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="b95fd-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b95fd-103">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="b95fd-103">Prerequisites</span></span>

<span data-ttu-id="b95fd-104">Avant de commencer ce didacticiel, le [Kit de développement Java (JDK)](https://java.com/en/download/faq/develop.xml) et [Maven](https://maven.apache.org/) doit être installé sur votre ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="b95fd-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Maven](https://maven.apache.org/) installed on your development machine.</span></span> <span data-ttu-id="b95fd-105">Si vous n’avez pas le JDK ou Maven, reportez-vous aux liens précédents sur les options de téléchargement.</span><span class="sxs-lookup"><span data-stu-id="b95fd-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="b95fd-106">Ce didacticiel a été écrit avec OpenJDK version 12.0.1 et Maven 3.6.1.</span><span class="sxs-lookup"><span data-stu-id="b95fd-106">This tutorial was written with OpenJDK version 12.0.1 and Maven 3.6.1.</span></span> <span data-ttu-id="b95fd-107">Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.</span><span class="sxs-lookup"><span data-stu-id="b95fd-107">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="b95fd-108">Commentaires</span><span class="sxs-lookup"><span data-stu-id="b95fd-108">Feedback</span></span>

<span data-ttu-id="b95fd-109">Veuillez fournir des commentaires sur ce didacticiel dans le [référentiel GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="b95fd-109">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
