---
title: Introduction aux conteneurs et à Docker
description: Obtenez une vue d’ensemble des principaux avantages de l’utilisation de Docker.
ms.date: 04/15/2020
ms.openlocfilehash: 79b4752ef4d2f0aa6199414aea5cf4ef357c86d3
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915948"
---
# <a name="introduction-to-containers-and-docker"></a><span data-ttu-id="76e87-103">Présentation des conteneurs et de Docker</span><span class="sxs-lookup"><span data-stu-id="76e87-103">Introduction to containers and Docker</span></span>

<span data-ttu-id="76e87-104">*La mise en conteneur est une approche du développement logiciel dans laquelle une application ou un service, ses dépendances et sa configuration (abstraite comme fichiers manifeste de déploiement) sont regroupés sous la forme d’une image conteneur. Vous pouvez ensuite tester l’application en conteneur en tant qu’unité et la déployer en tant qu’instance d’image de conteneur dans le système d’exploitation hôte.*</span><span class="sxs-lookup"><span data-stu-id="76e87-104">*Containerization is an approach to software development in which an application or service, its dependencies, and its configuration (abstracted as deployment manifest files) are packaged together as a container image. You then can test the containerized application as a unit and deploy it as a container image instance to the host operating system (OS).*</span></span>

<span data-ttu-id="76e87-105">De la même manière que les conteneurs de transport permettent de transporter des marchandises par bateau, par train ou par camion indépendamment de la nature de la cargaison, les conteneurs logiciels agissent comme une unité de déploiement logiciel standard qui peut contenir du code et des dépendances différents.</span><span class="sxs-lookup"><span data-stu-id="76e87-105">Just as shipping containers allow goods to be transported by ship, train, or truck regardless of the cargo inside, software containers act as a standard unit of software deployment that can contain different code and dependencies.</span></span> <span data-ttu-id="76e87-106">Cette façon de mettre les logiciels en conteneur permet aux développeurs et aux informaticiens de les déployer dans les environnements avec peu ou pas de modifications.</span><span class="sxs-lookup"><span data-stu-id="76e87-106">Containerizing software this way enables developers and IT professionals to deploy them across environments with little or no modification.</span></span>

<span data-ttu-id="76e87-107">Par ailleurs, les conteneurs isolent les applications les unes des autres sur un SE partagé.</span><span class="sxs-lookup"><span data-stu-id="76e87-107">Containers also isolate applications from each other on a shared OS.</span></span> <span data-ttu-id="76e87-108">Les applications en conteneur s’exécutent sur un hôte de conteneurs qui à son tour s’exécute sur le SE (Linux ou Windows).</span><span class="sxs-lookup"><span data-stu-id="76e87-108">Containerized applications run on top of a container host that in turn runs on the OS (Linux or Windows).</span></span> <span data-ttu-id="76e87-109">Par conséquent, les conteneurs ont un encombrement bien moindre que les images de machine virtuelle.</span><span class="sxs-lookup"><span data-stu-id="76e87-109">Containers therefore have a much smaller footprint than virtual machine (VM) images.</span></span>

<span data-ttu-id="76e87-110">Chaque conteneur peut exécuter une application web ou un service dans son intégralité, comme l’illustre la figure 1-1.</span><span class="sxs-lookup"><span data-stu-id="76e87-110">Each container can run a whole web application or a service, as shown in Figure 1-1.</span></span> <span data-ttu-id="76e87-111">Dans cet exemple, l’hôte Docker est un hôte de conteneurs, et App1, App2, Svc1 et Svc2 sont des applications ou des services en conteneur.</span><span class="sxs-lookup"><span data-stu-id="76e87-111">In this example, Docker host is a container host, and App1, App2, Svc1, and Svc2 are containerized applications or services.</span></span>

![Diagramme montrant quatre conteneurs exécutés sur une machine virtuelle ou un serveur.](./media/introduction-to-containers-and-docker/multiple-containers-single-host.png)

<span data-ttu-id="76e87-113">**Figure 1-1**.</span><span class="sxs-lookup"><span data-stu-id="76e87-113">**Figure 1-1**.</span></span> <span data-ttu-id="76e87-114">plusieurs conteneurs s’exécutant sur un hôte de conteneurs</span><span class="sxs-lookup"><span data-stu-id="76e87-114">Multiple containers running on a container host</span></span>

<span data-ttu-id="76e87-115">L’autre avantage qui dérive de la mise en conteneur est la scalabilité.</span><span class="sxs-lookup"><span data-stu-id="76e87-115">Another benefit you can derive from containerization is scalability.</span></span> <span data-ttu-id="76e87-116">Vous pouvez rapidement monter en charge en créant des conteneurs pour des tâches à court terme.</span><span class="sxs-lookup"><span data-stu-id="76e87-116">You can scale out quickly by creating new containers for short-term tasks.</span></span> <span data-ttu-id="76e87-117">Du point de vue de l’application, instancier une image (c.-à-d., créer un conteneur) revient à instancier un processus comme un service ou une application web.</span><span class="sxs-lookup"><span data-stu-id="76e87-117">From an application point of view, instantiating an image (creating a container) is similar to instantiating a process like a service or web app.</span></span> <span data-ttu-id="76e87-118">Cependant, dans un souci de fiabilité, quand il s’agit d’exécuter plusieurs instances d’une même image sur plusieurs serveurs hôtes, il est en principe préférable que chaque conteneur (instance d’image) s’exécute sur un serveur hôte différent ou sur une machine virtuelle dans différents domaines d’erreur.</span><span class="sxs-lookup"><span data-stu-id="76e87-118">For reliability, however, when you run multiple instances of the same image across multiple host servers, you typically want each container (image instance) to run in a different host server or VM in different fault domains.</span></span>

<span data-ttu-id="76e87-119">Pour résumer, les conteneurs offrent les avantages de l’isolation, de la portabilité, de l’agilité, de l’extensibilité et du contrôle dans l’ensemble du flux de travail du cycle de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="76e87-119">In short, containers offer the benefits of isolation, portability, agility, scalability, and control across the entire application lifecycle workflow.</span></span> <span data-ttu-id="76e87-120">L’avantage le plus important est l’isolation de l’environnement fourni entre le développement et les opérations.</span><span class="sxs-lookup"><span data-stu-id="76e87-120">The most important benefit is the environment isolation provided between Dev and Ops.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="76e87-121">[Précédent](index.md) 
> [Suivant](what-is-docker.md)</span><span class="sxs-lookup"><span data-stu-id="76e87-121">[Previous](index.md)
[Next](what-is-docker.md)</span></span>