---
title: 'Comment : utiliser la classe ChannelFactory'
description: Découvrez comment créer une fabrique de canaux pour créer plusieurs canaux pour accéder aux services à l’aide d’un client WCF.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d48f01b5-582b-4c8b-b547-8adddae7e371
ms.openlocfilehash: 7c87026ca4cf7c739f4615da9bc7f0272a382392
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246660"
---
# <a name="how-to-use-the-channelfactory"></a>Comment : utiliser la classe ChannelFactory
La classe <xref:System.ServiceModel.ChannelFactory%601> générique est utilisée dans les scénarios avancés qui requièrent la création d'une fabrication de canal pouvant être utilisée pour créer plusieurs canaux.  
  
### <a name="to-create-and-use-the-channelfactory-class"></a>Pour créer et utiliser la classe ChannelFactory  
  
1. Générez et exécutez un service Windows Communication Foundation (WCF). Pour plus d’informations, consultez [conception et implémentation de services](../designing-and-implementing-services.md), [configuration de services](../configuring-services.md)et Hébergement de [services](../hosting-services.md).  
  
2. Utilisez l' [outil ServiceModel Metadata Utility Tool (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) pour générer le contrat (interface) du client.  
  
3. Dans le code client, utilisez la classe <xref:System.ServiceModel.ChannelFactory%601> pour créer plusieurs écouteurs de point de terminaison.  
  
## <a name="example"></a>Exemple  
 [!code-csharp[c_HowToUseChannelFactory#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtousechannelfactory/cs/source.cs#1)]
 [!code-vb[c_HowToUseChannelFactory#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtousechannelfactory/vb/source.vb#1)]
