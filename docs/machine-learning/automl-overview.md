---
title: Machine learning automatisé avec ML.NET
description: Vue d’ensemble de la sélection et de l’entraînement automatiques d’un modèle
author: natke
ms.date: 05/01/2019
ms.topic: overview
ms.custom: mvc
ms.author: nakersha
ms.openlocfilehash: e6654718f174c9d8b628b05d85d74952c222eb66
ms.sourcegitcommit: 4c10802ad003374641a2c2373b8a92e3c88babc8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65452734"
---
# <a name="automated-machine-learning-with-mlnet"></a><span data-ttu-id="b0b26-103">Machine learning automatisé avec ML.NET</span><span class="sxs-lookup"><span data-stu-id="b0b26-103">Automated machine learning with ML.NET</span></span>

<span data-ttu-id="b0b26-104">Le machine learning automatisé est une fonctionnalité de ML.NET qui sélectionne et entraîne un modèle automatiquement.</span><span class="sxs-lookup"><span data-stu-id="b0b26-104">Automated machine learning is a feature of ML.NET that performs automatic model selection and training.</span></span> <span data-ttu-id="b0b26-105">Vous spécifiez la tâche de machine learning et fournissez un jeu de données, après quoi le machine learning automatisé choisit pour vous le modèle présentant les meilleures métriques.</span><span class="sxs-lookup"><span data-stu-id="b0b26-105">You specify the machine learning task and supply a dataset, and automated ML chooses the model with the best metrics.</span></span> <span data-ttu-id="b0b26-106">Il génère :</span><span class="sxs-lookup"><span data-stu-id="b0b26-106">It outputs:</span></span>
- <span data-ttu-id="b0b26-107">un fichier de modèle qui peut être chargé dans votre application de prédiction</span><span class="sxs-lookup"><span data-stu-id="b0b26-107">a model file that can be loaded into your prediction application</span></span>
- <span data-ttu-id="b0b26-108">le code d’application nécessaire pour effectuer des prédictions</span><span class="sxs-lookup"><span data-stu-id="b0b26-108">application code to make predictions</span></span>
- <span data-ttu-id="b0b26-109">le code source utilisé pour la sélection des caractéristiques et l’entraînement du modèle (pour comprendre le modèle)</span><span class="sxs-lookup"><span data-stu-id="b0b26-109">the source code used for feature selection and model training (to understand the model)</span></span>

> [!NOTE]
> <span data-ttu-id="b0b26-110">Cette fonctionnalité étant en préversion, elle est susceptible d’être modifiée.</span><span class="sxs-lookup"><span data-stu-id="b0b26-110">This feature is currently in Preview, and material may be subject to change.</span></span> 

<span data-ttu-id="b0b26-111">Le ML automatisé est actuellement disponible uniquement pour les [tâches](resources/tasks.md) de machine learning de classification binaire, de classification multiclasse et de régression.</span><span class="sxs-lookup"><span data-stu-id="b0b26-111">Automated ML is currently limited to the machine learning [tasks](resources/tasks.md) of binary classification, multiclass classification, and regression.</span></span> <span data-ttu-id="b0b26-112">Les autres tâches de machine learning seront prises en charge dans les versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="b0b26-112">The other machine learning tasks will be supported in future releases.</span></span>

<span data-ttu-id="b0b26-113">Il y a trois manières d’utiliser le ML automatisé :</span><span class="sxs-lookup"><span data-stu-id="b0b26-113">There are three ways to use automated ML:</span></span>
1. <span data-ttu-id="b0b26-114">À partir de la ligne de commande, avec l’interface [CLI ML.NET](automate-training-with-cli.md)</span><span class="sxs-lookup"><span data-stu-id="b0b26-114">On the command line, with the [ML.NET CLI](automate-training-with-cli.md)</span></span>
1. <span data-ttu-id="b0b26-115">Par le biais d’une application, avec l’[API de ML automatisé](how-to-guides/how-to-use-the-automl-api.md)</span><span class="sxs-lookup"><span data-stu-id="b0b26-115">Via an application, with the [automated ML API](how-to-guides/how-to-use-the-automl-api.md)</span></span>
1. <span data-ttu-id="b0b26-116">Dans une interface graphique utilisateur, avec le Générateur de modèles ML.NET</span><span class="sxs-lookup"><span data-stu-id="b0b26-116">With a graphical user interface, with the the ML.NET Model Builder</span></span>