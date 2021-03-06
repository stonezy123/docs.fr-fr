---
title: Collections de nœuds dans NamedNodeMap et NodeList
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 025954b8-7aa8-47c5-a1c1-f81064fb4d65
ms.openlocfilehash: 3d721e9a6cff87adbf8896b1b37c0e2dfa55fa71
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288769"
---
# <a name="node-collections-in-namednodemaps-and-nodelists"></a>Collections de nœuds dans NamedNodeMap et NodeList
Il est possible d'extraire une collection de nœuds et de les placer dans une collection triée ou non. Si vous regroupez une collection de nœuds dans une collection non triée, cet ensemble est appelé NamedNodeMap par le World Wide Web Consortium (W3C). Ce type de collection vous permet d’extraire des données au moyen de leur nom ou de l’index. En revanche, si vous placez une collection de nœuds dans une collection triée, celle-ci est appelée NodeList par le W3C, et ses données peuvent être extraites au moyen d’un index de base zéro. NamedNodeMap et NodeList sont décrits par le W3C. Leurs implémentations dans Microsoft .NET Framework sont **XmlNamedNodeMap** pour NamedNodeMap et **XmlNodeList** pour NodeList.  
  
 Pour obtenir des informations sur la collection non triée, consultez [Extraction de nœuds non triés par leur nom ou par l’index](unordered-node-retrieval-by-name-or-index.md). Pour obtenir des informations sur la collection triée, consultez [Extraction de nœuds triés par l’index](ordered-node-retrieval-by-index.md).  
  
## <a name="see-also"></a>Voir aussi

- [DOM (Document Object Model) XML](xml-document-object-model-dom.md)
