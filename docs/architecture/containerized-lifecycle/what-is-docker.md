---
title: Qu’est-ce que Docker ?
description: Approfondissez un peu votre connaissance de Docker ; une analogie simple peut vous y aider.
ms.date: 08/06/2020
ms.openlocfilehash: 73b6032465583861169a8ac2bed81585027f42ec
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915061"
---
# <a name="what-is-docker"></a>Qu’est-ce que Docker ?

[Docker](https://www.docker.com/) est un [projet open source](https://github.com/docker/docker) permettant d’automatiser le déploiement d’applications en tant que conteneurs portables et autonomes exécutables sur le cloud ou localement. Docker est également une [entreprise](https://www.docker.com/) qui développe et diffuse cette technologie, en collaboration avec des fournisseurs de services cloud, Linux et Windows, notamment Microsoft.

![Diagramme montrant les emplacements que les conteneurs de l’ancrage peuvent exécuter.](./media/what-is-docker/docker-containers-run-anywhere.png)

**Figure 1-2**. Docker déploie des conteneurs dans toutes les couches du cloud hybride

Comme indiqué dans le diagramme ci-dessus, les conteneurs de l’arrimeur peuvent s’exécuter n’importe où, localement dans le centre de donnes client, dans un fournisseur de services externes ou dans le Cloud, sur Azure. Les conteneurs d’images d’ancrage peuvent également s’exécuter en mode natif sur Linux et Windows. Toutefois, les images Windows peuvent s’exécuter uniquement sur des hôtes Windows et les images Linux peuvent s’exécuter sur des hôtes Linux et des hôtes Windows (à l’aide d’une machine virtuelle Linux Hyper-V, jusqu’à présent), où le terme « hôte » désigne un serveur ou une machine virtuelle.

Les développeurs peuvent utiliser des environnements de développement sur Windows, Linux ou macOS. Sur l’ordinateur de développement, le développeur exécute un hôte Docker sur lequel sont déployées les images Docker, y compris l’application et ses dépendances. Les développeurs qui travaillent sur Linux ou sur Mac utilisent un hôte Docker qui est basé sur Linux et peuvent créer des images seulement pour les conteneurs Linux. (Les développeurs qui travaillent sur le Mac peuvent modifier le code ou exécuter l’interface de ligne de commande (CLI) de l’Ancreur à partir de macOS, mais au cours de cet article, les conteneurs ne s’exécutent pas directement sur macOS). Les développeurs qui travaillent sur Windows peuvent créer des images pour les conteneurs Linux ou Windows.

Pour héberger des conteneurs dans un environnement de développement et fournir des outils de développement supplémentaires, Docker fournit [Docker Community Edition (CE)](https://www.docker.com/community-edition) pour Windows ou macOS. Ces produits installent la machine virtuelle nécessaire (l’hôte Docker) pour héberger les conteneurs. Docker fournit aussi [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition), conçu pour le développement en entreprise et utilisé par les équipes informatiques qui génèrent, livrent et exécutent des applications métier stratégiques volumineuses en production.

Les [conteneurs Windows](/virtualization/windowscontainers/about/) fonctionnent avec deux types de runtime :

- Les **conteneurs Windows Server** assurent l’isolation d’applications grâce à la technologie d’isolation des processus et des espaces de noms. Un conteneur Windows Server partage un noyau avec l’hôte conteneur et avec tous les conteneurs exécutés sur l’hôte.

- Les **conteneurs Hyper-V** étendent l’isolation fournie par les conteneurs Windows Server en exécutant chaque conteneur sur une machine virtuelle hautement optimisée. Dans cette configuration, le noyau de l’hôte conteneur n’est pas partagé avec les conteneurs Hyper-V, ce qui garantit une meilleure isolation.

Les images de ces deux types de conteneurs sont créées et fonctionnent exactement de la même façon. La différence réside dans la façon dont le conteneur est créé à partir de l’image, l’exécution d’un conteneur Hyper-V nécessitant un paramètre supplémentaire. Pour plus d’informations, consultez [Conteneurs Hyper-V](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/hyperv-container).

## <a name="comparing-docker-containers-with-virtual-machines"></a>Comparaison entre les conteneurs Docker et les machines virtuelles

La figure 1-3 compare les machines virtuelles et les conteneurs Docker.

![Diagramme montrant une comparaison entre les environnements de machines virtuelles et de conteneurs.](./media/what-is-docker/comparison-vms-docker-conatiners.png)

**Figure 1-3**. Comparaison entre les machines virtuelles traditionnelles et les conteneurs Docker

Comme indiqué dans le diagramme ci-dessus, pour les machines virtuelles, il existe trois couches de base dans le serveur hôte. Du bas vers le haut : infrastructure, système d’exploitation hôte et hyperviseur. Au-dessus de tout cela, chaque machine virtuelle possède son propre système d’exploitation et toutes les bibliothèques nécessaires. D’un autre côté, pour l’arrimeur, le serveur hôte dispose uniquement de l’infrastructure et du système d’exploitation. En plus de cela, le moteur de conteneur conserve les conteneurs isolés, mais les autorise à partager les services d’un système d’exploitation de base unique.

Du fait que les conteneurs nécessitent beaucoup moins de ressources (par exemple, ils n’ont pas besoin d’un système d’exploitation complet), leur démarrage est rapide et leur déploiement est simple. Cela vous permet d’avoir une densité plus élevée, ce qui vous permet d’exécuter plus de services sur la même unité matérielle, limitant les coûts.

Comme effet secondaire de l’exécution sur le même noyau, vous obtenez une moins bonne isolation que sur les machines virtuelles.

L’objectif principal d’une image est de garantir un environnement (dépendances) identique entre les différents déploiements. Vous pouvez ainsi la déboguer sur votre machine et la déployer sur une autre machine, en étant sûr de garder le même environnement.

Une image conteneur est un moyen d’empaqueter une application ou un service et de les déployer ensuite d’une façon fiable et reproductible. Plus qu’une simple technologie, Docker peut également être vu comme une philosophie et un processus.

Les développeurs qui utilisent Docker ne disent pas : « Cela fonctionne sur ma machine, alors pourquoi pas en production ? » Ils peuvent simplement dire « Cela s’exécute sur Docker », car l’application Docker empaquetée peut être exécutée sur n’importe quel environnement Docker pris en charge et elle s’exécute comme prévu sur toutes les cibles de déploiement (par exemple développement, assurance qualité, préproduction et production).

## <a name="a-simple-analogy"></a>Une simple analogie

Peut-être qu’une analogie simple peut aider à bien comprendre le concept de base de Docker.

Retournons dans les années 1950 un instant. Le traitement de texte n’existait pas et les photocopieurs étaient utilisés partout (enfin, presque).

Imaginez que vous devez émettre rapidement des lots de lettres à envoyer aux clients, avec du papier et des enveloppes, et les livrer physiquement à l’adresse de chaque client (l’e-mail n’existait pas à l’époque).

À un moment donné, vous réalisez que les lettres sont simplement une composition d’un grand ensemble de paragraphes, qui sont organisés en fonction des besoins, selon l’objectif de la lettre, donc vous concevez un système permettant d’émettre des lettres rapidement, afin d’obtenir une augmentation importante.

Le système est simple :

1. Vous commencez avec un jeu de feuilles transparentes contenant chacune un paragraphe.

2. Pour émettre un ensemble de lettres, vous choisissez les feuilles avec les paragraphes dont vous avez besoin, vous les empilez et les alignez afin de pouvoir les lire correctement.

3. Enfin, vous placez l’ensemble dans la photocopieuse, puis appuyez sur démarrer pour produire autant de lettres que nécessaire.

C’est cela l’idée fondamentale de Docker (en version simplifiée).

Dans Docker, chaque couche est un jeu de résultats des modifications qui se produisent dans le système de fichiers après l’exécution d’une commande, par exemple, l’installation d’un programme.

Par conséquent, lorsque vous « regardez » le système de fichiers une fois que la couche a été copiée, vous voyez tous les fichiers, y compris la couche lorsque le programme a été installé.

Vous pouvez considérer une image en tant que disque dur en lecture seule auxiliaire prêt à être installé dans un « ordinateur » où le système d’exploitation est déjà installé.

De même, vous pouvez considérer un conteneur comme « l’ordinateur » avec le disque dur de l’image installé. Le conteneur, tout comme un ordinateur, peut être activé ou désactivé.

>[!div class="step-by-step"]
>[Précédent](introduction-to-containers-and-docker.md) 
> [Suivant](docker-terminology.md)
