---
title: 'Procédure : tester le proxy de découverte'
ms.date: 03/30/2017
ms.assetid: d96e3fa2-3c42-4e5d-8244-2694081bdc32
ms.openlocfilehash: 78921d0a26f1116c87c2931b1472a161d6fed145
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84592812"
---
# <a name="how-to-test-the-discovery-proxy"></a>Procédure : tester le proxy de découverte
Cette rubrique est la quatrième d'une série quatre qui expliquent comment implémenter un proxy de découverte. Dans la rubrique précédente, [Comment : implémenter une application cliente qui utilise le proxy de découverte pour rechercher un service](client-app-discovery-proxy-to-find-a-service.md), vous avez implémenté une application cliente WCF qui utilise le proxy de découverte pour rechercher un service, puis appelle le service. Cette rubrique explique comment vérifier si le proxy de découverte, le service et l'application cliente fonctionnent comme prévu.  
  
### <a name="run-the-discovery-proxy"></a>Exécuter le proxy de découverte  
  
1. Ouvrez une invite de commandes en tant qu’administrateur.  
  
2. Une boîte de dialogue s'affiche éventuellement, indiquant : le Pare-feu Windows a bloqué certaines fonctionnalités de ce programme. Si ce message s’affiche, cliquez sur le bouton **débloquer** .  
  
3. Dans l'invite de commandes, exécutez le proxy de découverte, DiscoveryProxy.exe.  
  
4. L'application doit afficher le texte suivant : `Proxy started. Hit Enter to exit`.  
  
### <a name="run-the-discoverable-service"></a>Exécuter le service détectable  
  
1. Ouvrez une invite de commandes en tant qu’administrateur.  
  
2. Dans l'invite de commandes, exécutez le service détectable service.exe.  
  
3. DiscoveryProxy. exe doit afficher le texte suivant : `******* Adding the following service: ** [Service Contract Name] ** [Service Endpoint Addr] 3.******* Done *******` .  
  
### <a name="run-the-client-application"></a>Exécuter l'application cliente  
  
1. Ouvrez une invite de commandes.  
  
2. Dans l'invite de commandes, exécutez l'application client.exe.  
  
3. Au bout de quelques secondes l'application cliente affiche le texte suivant : connexion au [point de terminaison de service].  
  
4. Le service.exe doit ensuite afficher le texte suivant : message de salutation reçu, je vais répondre.  
  
5. Le client.exe doit ensuite afficher le texte suivant : Bonjour client !  
  
### <a name="shut-down-the-applications"></a>Arrêter les applications  
  
1. Arrêtez l'application cliente.  
  
2. Arrêtez le service. Le proxy de découverte affiche le texte suivant : `******* Removing the following service: ** [Service Contract Name] ** [Service Endpoint Addr] 2.3.******* Done *******`.  
  
3. Arrêtez le proxy de découverte.  
  
## <a name="see-also"></a>Voir aussi

- [Vue d'ensemble de la découverte WCF](wcf-discovery-overview.md)
- [Procédure : implémenter un proxy de découverte](how-to-implement-a-discovery-proxy.md)
- [Procédure : implémenter un service détectable qui s'enregistre avec le proxy de découverte.](discoverable-service-that-registers-with-the-discovery-proxy.md)
- [Procédure : implémenter une application cliente qui utilise le proxy de découverte pour rechercher un service](client-app-discovery-proxy-to-find-a-service.md)
