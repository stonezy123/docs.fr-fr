---
title: Emplacement d’assembly
description: L’emplacement d’un assembly .NET détermine la façon dont le CLR le trouve et s’il peut être partagé avec d’autres assemblys.
ms.date: 08/20/2019
helpviewer_keywords:
- locating assemblies
- assemblies [.NET Framework], location
ms.assetid: 9f1f41a7-2954-49d3-a2c0-62b6ef4d40ab
ms.openlocfilehash: 7ab3804b14b586e1430d654f4da32a310bcb6cc9
ms.sourcegitcommit: d6bd7903d7d46698e9d89d3725f3bb4876891aa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/13/2020
ms.locfileid: "83379891"
---
# <a name="assembly-location"></a>Emplacement d’assembly
L’emplacement d’un assembly détermine si le common language runtime peut le localiser quand il est référencé, et il peut aussi déterminer si l’assembly peut être partagé avec d’autres assemblys. Vous pouvez déployer un assembly dans les emplacements suivants :

- Répertoire ou sous-répertoires de l’application.

     Il s’agit de l’emplacement le plus courant pour déployer un assembly. Les sous-répertoires du répertoire racine d’une application peuvent être basés sur la langue ou la culture. Si un assembly a des informations de l’attribut de culture, il doit se trouver dans un sous-répertoire sous le répertoire de l’application avec le nom de cette culture.

- Le Global Assembly Cache.

     Il s’agit d’un cache de code au niveau de la machine, qui est installé partout où le common language runtime est installé. Dans la plupart des cas, si vous prévoyez de partager un assembly entre plusieurs applications, vous devez le déployer dans le Global Assembly Cache.

- Sur un serveur HTTP.

     Un assembly déployé sur un serveur HTTP doit avoir un nom fort ; vous pointez vers l’assembly dans la section codebase du fichier de configuration de l’application.

## <a name="see-also"></a>Voir aussi

- [Créer des assemblys](create.md)
- [Global assembly cache](../../framework/app-domains/gac.md)
- [Comment le runtime localise les assemblys](../../framework/deployment/how-the-runtime-locates-assemblies.md)
