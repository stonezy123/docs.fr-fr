---
title: Falsification
ms.date: 03/30/2017
ms.assetid: 3bad93be-60bb-4f89-96ab-a1c3dc7c0fad
ms.openlocfilehash: e618ab369a46d403aa8db26c4b472e2be3634785
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600709"
---
# <a name="tampering"></a>Falsification
La *falsification* est l’action consistant à modifier un message ou à remettre un message, et à utiliser le message modifié à des fins autres que celles prévues.  
  
## <a name="do-not-disable-ws-addressing"></a>Ne pas désactiver WS-Addressing  
 La spécification WS-Addressing fournit des en-têtes d'adresse sur chaque message, ce qui permet au destinataire d'un message de vérifier l'expéditeur de celui-ci. Vous pouvez désactiver cette fonctionnalité en affectant <xref:System.ServiceModel.Channels.MessageVersion.Addressing%2A> à la propriété <xref:System.ServiceModel.Channels.AddressingVersion.None%2A>.  
  
 Si le mode de sécurité a la valeur Message et que WS-Addressing est désactivé, un intrus peut prendre la demande d'un client et l'envoyer à un autre service, le deuxième service n'ayant aucun moyen de détecter que le message est provenu du client d'origine. En effet, le premier service peut prétendre qu'il est un client lorsqu'il parle au deuxième service.  
  
 Pour atténuer ce risque, n'affectez jamais <xref:System.ServiceModel.Channels.MessageVersion.Addressing%2A> à la propriété <xref:System.ServiceModel.Channels.AddressingVersion.None%2A> et évitez d'utiliser <xref:System.ServiceModel.Channels.MessageVersion>, tel que la propriété statique <xref:System.ServiceModel.Channels.MessageVersion.Soap12%2A>, qui affecte <xref:System.ServiceModel.Channels.MessageVersion.Addressing%2A> à la propriété <xref:System.ServiceModel.Channels.AddressingVersion.None%2A>.  
  
## <a name="see-also"></a>Voir aussi

- [Security Considerations](security-considerations-in-wcf.md)
- [Divulgation d’informations](information-disclosure.md)
- [Élévation de privilège](elevation-of-privilege.md)
- [Déni de service](denial-of-service.md)
- [Scénarios non pris en charge](unsupported-scenarios.md)
- [Attaques par relecture](replay-attacks.md)
