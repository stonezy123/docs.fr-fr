---
title: S’abonner à des événements
description: Architecture de microservices .NET pour les applications .NET conteneurisées | Comprendre les détails de la publication et de l’abonnement à des événements d’intégration.
ms.date: 01/30/2020
ms.openlocfilehash: 426dcebe175e9db9a02bcdb2f21ad039154a7bda
ms.sourcegitcommit: 2b3b2d684259463ddfc76ad680e5e09fdc1984d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80888213"
---
# <a name="subscribing-to-events"></a>S’abonner à des événements

Pour utiliser Service Bus, vous devez d’abord abonner les microservices aux événements souhaités. Cette étape doit être effectuée dans les microservices récepteurs.

L’exemple de code suivant montre ce que chaque microservice récepteur doit implémenter au démarrage du service (dans la classe `Startup`) pour s’abonner aux événements dont il a besoin. Dans ce cas, le microservice `basket-api` doit s’abonner à `ProductPriceChangedIntegrationEvent` et aux messages `OrderStartedIntegrationEvent`.

Par exemple, lors de l’abonnement à l’événement, `ProductPriceChangedIntegrationEvent` qui rend le microservice panier conscient de toute modification du prix du produit et lui permet d’avertir l’utilisateur au sujet de la modification si ce produit est dans le panier de l’utilisateur.

```csharp
var eventBus = app.ApplicationServices.GetRequiredService<IEventBus>();

eventBus.Subscribe<ProductPriceChangedIntegrationEvent,
                   ProductPriceChangedIntegrationEventHandler>();

eventBus.Subscribe<OrderStartedIntegrationEvent,
                   OrderStartedIntegrationEventHandler>();

```

Lorsque ce code est exécuté, le microservice abonné écoute via les canaux RabbitMQ. Quand un message de type ProductPriceChangedIntegrationEvent est reçu, le code appelle le gestionnaire d’événements qui lui est passé et traite l’événement.

## <a name="publishing-events-through-the-event-bus"></a>Publication d’événements via le bus d’événements

Enfin, l’expéditeur du message (le microservice d’origine) publie les événements d’intégration avec du code similaire à celui de l’exemple suivant. (Il s’agit d’un exemple simplifié qui ne tient pas compte de l’atome.) Vous implémentez un code similaire chaque fois qu’un événement doit être propagé dans plusieurs microservices, généralement juste après avoir commis des données ou des transactions à partir du microservice d’origine.

Tout d’abord, l’objet d’implémentation du bus d’événements (basé sur RabbitMQ ou sur un bus de services) est injecté dans le constructeur du contrôleur, comme dans le code suivant :

```csharp
[Route("api/v1/[controller]")]
public class CatalogController : ControllerBase
{
    private readonly CatalogContext _context;
    private readonly IOptionsSnapshot<Settings> _settings;
    private readonly IEventBus _eventBus;

    public CatalogController(CatalogContext context,
        IOptionsSnapshot<Settings> settings,
        IEventBus eventBus)
    {
        _context = context;
        _settings = settings;
        _eventBus = eventBus;
    }
    // ...
}
```

Ensuite, vous l’utilisez à partir des méthodes de votre contrôleur, comme dans la méthode UpdateProduct:

```csharp
[Route("items")]
[HttpPost]
public async Task<IActionResult> UpdateProduct([FromBody]CatalogItem product)
{
    var item = await _context.CatalogItems.SingleOrDefaultAsync(
        i => i.Id == product.Id);
    // ...
    if (item.Price != product.Price)
    {
        var oldPrice = item.Price;
        item.Price = product.Price;
        _context.CatalogItems.Update(item);
        var @event = new ProductPriceChangedIntegrationEvent(item.Id,
            item.Price,
            oldPrice);
        // Commit changes in original transaction
        await _context.SaveChangesAsync();
        // Publish integration event to the event bus
        // (RabbitMQ or a service bus underneath)
        _eventBus.Publish(@event);
        // ...
    }
    // ...
}
```

Dans ce cas, le microservice d’origine étant un microservice CRUD simple, le code est placé dans un contrôleur d’API web.

Dans les microservices plus avancés, comme avec l’approche CQRS, vous pouvez l’implémenter dans la classe `CommandHandler`, dans la méthode `Handle()`.

### <a name="designing-atomicity-and-resiliency-when-publishing-to-the-event-bus"></a>Conception de l’atomicité et de la résilience lors de la publication d’événements dans le bus d’événements

Quand vous publiez des événements d’intégration par le biais d’un système de messagerie distribuée comme votre bus d’événements, vous être confronté au problème de mettre à jour la base de données d’origine et de publier un événement de manière atomique (autrement dit, les deux opérations se terminent ou aucune des deux ne se termine). Par exemple, dans l’exemple simplifié précédent, le code valide les données dans la base de données lorsque le prix du produit est modifié, puis il publie un message ProductPriceChangedIntegrationEvent. Au début, il peut paraître essentiel que ces deux opérations soient effectuées de manière atomique. Toutefois, si vous utilisez une transaction distribuée impliquant la base de données et le répartiteur de messages, comme vous le feriez sur les anciens systèmes tels que [Microsoft Message Queuing (MSMQ)](https://msdn.microsoft.com/library/windows/desktop/ms711472(v=vs.85).aspx), cette méthode n’est pas recommandée pour les raisons énoncées par le [théorème CAP](https://www.quora.com/What-Is-CAP-Theorem-1).

Pour résumer, vous utilisez des microservices pour créer des systèmes évolutifs et hautement disponibles. Simplifiant quelque peu, le théorème de la PAC dit que vous ne pouvez pas construire une base de données (distribuée) (ou un microservice qui possède son modèle) qui est continuellement disponible, fortement cohérente *et* tolérante à toute partition. Vous ne pouvez avoir que deux de ces propriétés à la fois.

Dans les architectures basées sur les microservices, vous devez choisir la disponibilité et la tolérance, et vous devez désaccentuer une forte cohérence. Par conséquent, dans les applications de microservice les plus récentes, il n’est généralement pas souhaitable d’utiliser des transactions distribuées pour la messagerie, comme vous le feriez pour implémenter des [transactions distribuées](https://docs.microsoft.com/previous-versions/windows/desktop/ms681205(v=vs.85)) DTC avec [MSMQ](https://msdn.microsoft.com/library/windows/desktop/ms711472(v=vs.85).aspx).

Revenons à la question initiale et à son exemple. Si le service s’écrase après la mise à jour de `_context.SaveChangesAsync()`la base de données (dans ce cas, juste après la ligne de code avec), mais avant la publication de l’événement d’intégration, le système global pourrait devenir incohérent. Cela peut être critique pour l’entreprise, selon le type d’opération.

Comme déjà mentionné dans la section relative à l’architecture, plusieurs méthodes permettent de traiter ce problème :

- Utiliser la version complète du [modèle d’approvisionnement en événements](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)

- Utiliser [l’exploration des données du journal des transactions](https://www.scoop.it/t/sql-server-transaction-log-mining)

- Utiliser le [modèle de boîte d’envoi](https://www.kamilgrzybek.com/design/the-outbox-pattern/) Il s’agit d’une table transactionnelle permettant de stocker les événements d’intégration (en étendant la transaction locale).

Pour ce scénario, l’utilisation du modèle d’approvisionnement en événements est l’une des meilleures approches, sinon *la* meilleure. Toutefois, dans de nombreux scénarios d’application, vous ne pourrez pas implémenter un système d’approvisionnement en événements complet. Avec l’approvisionnement en événements, vous stockez uniquement des événements de domaine dans votre base de données transactionnelle, au lieu de stocker les données de l’état actuel. Le fait de stocker uniquement les événements de domaine comporte des avantages, tels que la disponibilité de l’historique de votre système et la capacité à déterminer l’état antérieur de votre système. Toutefois, l’implémentation d’un système d’approvisionnement en événements complet nécessite la modification d’une grande partie de l’architecture de votre système, et ajoute de nombreuses exigences et davantage de complexité. Par exemple, vous pouvez utiliser une base de données créée spécialement pour l’approvisionnement en événements, telle qu’un [magasin d’événements](https://eventstore.org/), ou une base de données orientée documents, telle qu’Azure Cosmos DB, MongoDB, Cassandra, CouchDB ou RavenDB. L’approvisionnement en événements est une excellente approche pour ce problème, mais ce n’est pas la plus simple, sauf si vous êtes déjà familiarisé avec l’approvisionnement en événements.

L’option d’utiliser l’extraction de journaux de transaction semble d’abord transparente. Toutefois, pour cette méthode, vous devez associer le microservice à votre journal des transactions SGBDR, comme par exemple le journal des transactions SQL Server. Toutefois, cela n’est pas souhaitable. Un autre inconvénient à cela est que les mises à jour de bas niveau enregistrées dans le journal des transactions peuvent ne pas être au même niveau que les événements d’intégration de haut niveau. Si c’est le cas, il peut être difficile d’effectuer une rétroconception sur les opérations du journal des transactions.

La meilleure approche consiste à combiner une table de base de données transactionnelle et un modèle simplifié d’approvisionnement en événements. Vous pouvez utiliser un état tel que « prêt à publier l’événement », que vous définissez dans l’événement original lorsque vous l’engagez à la table des événements d’intégration. Ensuite, vous essayez de publier l’événement dans le bus d’événements. Si l’action de publication-événement réussit, vous démarrez une autre transaction dans le service d’origine et déplacez l’état de « prêt à publier l’événement » à « événement déjà publié ».

Si l’action de publication dans l’autobus de l’événement échoue, les données ne seront toujours pas incompatibles dans le microservice d’origine — elles sont toujours marquées comme étant « prêtes à publier l’événement », et en ce qui concerne le reste des services, elles finiront par être cohérentes. Vous pouvez toujours avoir des tâches en arrière-plan qui vérifient l’état des transactions ou des événements d’intégration. Si le travail trouve un événement dans l’état « prêt à publier l’événement », il peut essayer de rééditer cet événement à l’autobus de l’événement.

Notez qu’avec cette approche, vous conservez uniquement les événements d’intégration de chaque microservice d’origine, et uniquement les événements que vous souhaitez communiquer aux autres microservices ou systèmes externes. En revanche, dans un système d’approvisionnement en événements complet, tous les événements de domaine sont également stockés.

Par conséquent, cette approche mixte n’est autre qu’un système simplifié d’approvisionnement en événements. Vous avez besoin d’une liste d’événements d’intégration avec leur état actuel ("prêt à publier" par opposition à "publié"). Toutefois, vous avez uniquement besoin d’implémenter ces états pour les événements d’intégration. En outre, avec cette approche, il n’est pas nécessaire de stocker toutes les données de votre domaine comme des événements dans la base de données transactionnelle, comme vous le feriez dans un système d’approvisionnement en événements complet.

Si vous utilisez déjà une base de données relationnelle, vous pouvez utiliser une table transactionnelle pour stocker les événements d’intégration. Pour obtenir l’atomicité de votre application, vous devez utiliser un processus en deux étapes basé sur les transactions locales. Pour résumer, une table IntegrationEvent doit se trouver dans la même base de données que vos entités de domaine. Cette table fonctionne comme une garantie d’obtention de l’atomicité, et vous permet d’inclure les événements d’intégration persistants dans les transactions qui valident vos données de domaine.

Voici, étape par étape, le déroulement de ce processus :

1. L’application commence une transaction de base de données locale.

2. Ensuite, elle met à jour l’état de vos entités de domaine et insère un événement dans la table d’événements d’intégration.

3. Enfin, elle valide la transaction pour vous permettre d’obtenir l’atomicité souhaitée, puis

4. Vous publiez l’événement d’une manière ou d’une autre (à venir).

Lorsque vous implémentez les étapes de publication des événements, vous avez les possibilités suivantes :

- Publiez l’événement d’intégration juste après la validation de la transaction, puis utilisez une autre transaction locale pour marquer les événements de la table comme étant publiés. Ensuite, utilisez la table comme un artefact pour effectuer le suivi des événements d’intégration en cas de problèmes au niveau des microservices distants, puis effectuez des actions compensatoires en fonction des événements d’intégration stockés.

- Utilisez la table comme un type de file d’attente. Un thread d’application ou un processus distinct interroge la table d’événements d’intégration, publie les événements dans le bus d’événements, puis utilise une transaction locale pour marquer les événements comme étant publiés.

La figure 6-22 montre l’architecture de la première de ces approches.

![Diagramme de l’atome lors de la publication sans microservice de travailleur.](./media/subscribe-events/atomicity-publish-event-bus.png)

**Figure 6-22.** Atomicité lors de la publication d’événements dans le bus d’événements

Il manque à l’approche illustrée à la figure 6-22 un microservice de worker supplémentaire chargé de vérifier et de confirmer la publication des événements d’intégration. En cas d’échec, le microservice de worker de vérification supplémentaire peut lire les événements de la table et les republier (autrement dit, répétez l’étape 2).

Pour la deuxième approche, vous devez utiliser la table EventLog comme une file d’attente et toujours utiliser un microservice de worker pour publier les messages. Dans ce cas, le processus est similaire à celui illustré dans la figure 6-23. On y voit un microservice supplémentaire, ainsi que la table qui est la seule source lors de la publication d’événements.

![Diagramme de l’atome lors de la publication avec un microservice de travailleur.](./media/subscribe-events/atomicity-publish-worker-microservice.png)

**Figure 6-23.** Atomicité lors de la publication d’événements dans le bus d’événements avec un microservice de worker

Par souci de simplicité, l’exemple eShopOnContainers utilise la première approche (sans processus ou microservices de vérification supplémentaires), ainsi que le bus d’événements. Toutefois, l’échantillon eShopOnContainers ne traite pas tous les cas d’échec possibles. Dans une application réelle déployée dans le cloud, vous devez accepter le fait que des problèmes surviendront un jour ou l’autre. Il est donc nécessaire d’implémenter cette logique de vérification et de renvoi. L’utilisation de la table comme une file d’attente peut se révéler plus efficace que la première approche si cette table est la seule source d’événements quand vous les publiez (avec le worker) à l’aide du bus d’événements.

### <a name="implementing-atomicity-when-publishing-integration-events-through-the-event-bus"></a>Implémentation de l’atomicité lors de la publication d’événements d’intégration via le bus d’événements

Le code suivant montre comment vous pouvez créer une transaction impliquant plusieurs objets DbContext : un contexte lié aux données d’origine mises à jour, et le deuxième contexte lié à la table IntegrationEventLog.

La transaction dans le code de l’exemple ci-dessous ne sera pas résiliente si les connexions à la base de données ont un problème au moment où le code est en cours d’exécution. Cela peut se produire dans les systèmes cloud comme Azure SQL DB, qui sont susceptibles de déplacer des bases de données d’un serveur à un autre. Pour implémenter des transactions résilientes dans plusieurs contextes, consultez la section [Implémentation de connexions SQL à Entity Framework Core résilientes](../implement-resilient-applications/implement-resilient-entity-framework-core-sql-connections.md) plus loin dans ce guide.

Pour plus de clarté, l’exemple suivant montre l’ensemble du processus dans un même bloc de code. Cependant, la mise en œuvre eShopOnContainers est refactored et divise cette logique en plusieurs classes de sorte qu’il est plus facile à maintenir.

```csharp
// Update Product from the Catalog microservice
//
public async Task<IActionResult> UpdateProduct([FromBody]CatalogItem productToUpdate)
{
  var catalogItem =
       await _catalogContext.CatalogItems.SingleOrDefaultAsync(i => i.Id ==
                                                               productToUpdate.Id);
  if (catalogItem == null) return NotFound();

  bool raiseProductPriceChangedEvent = false;
  IntegrationEvent priceChangedEvent = null;

  if (catalogItem.Price != productToUpdate.Price)
          raiseProductPriceChangedEvent = true;

  if (raiseProductPriceChangedEvent) // Create event if price has changed
  {
      var oldPrice = catalogItem.Price;
      priceChangedEvent = new ProductPriceChangedIntegrationEvent(catalogItem.Id,
                                                                  productToUpdate.Price,
                                                                  oldPrice);
  }
  // Update current product
  catalogItem = productToUpdate;

  // Just save the updated product if the Product's Price hasn't changed.
  if (!raiseProductPriceChangedEvent)
  {
      await _catalogContext.SaveChangesAsync();
  }
  else  // Publish to event bus only if product price changed
  {
        // Achieving atomicity between original DB and the IntegrationEventLog
        // with a local transaction
        using (var transaction = _catalogContext.Database.BeginTransaction())
        {
           _catalogContext.CatalogItems.Update(catalogItem);
           await _catalogContext.SaveChangesAsync();

           // Save to EventLog only if product price changed
           if(raiseProductPriceChangedEvent)
               await _integrationEventLogService.SaveEventAsync(priceChangedEvent);

           transaction.Commit();
        }

      // Publish the integration event through the event bus
      _eventBus.Publish(priceChangedEvent);

      _integrationEventLogService.MarkEventAsPublishedAsync(
                                                priceChangedEvent);
  }

  return Ok();
}

```

Une fois que l’événement d’intégration ProductPriceChangedIntegrationEvent a été créé, la transaction qui stocke l’opération de domaine d’origine (mise à jour de l’élément de catalogue) inclut également la persistance de l’événement dans la table EventLog. De cette façon, vous n’obtenez qu’une seule transaction, et vous pourrez toujours vérifier si les messages d’événement ont été envoyés.

La table du journal des événements est mise à jour atomiquement avec l’opération de base de données d’origine, à l’aide d’une transaction locale dans la même base de données. Si l’une des opérations échoue, une exception est levée et la transaction restaure toute opération terminée, de manière à maintenir la cohérence entre les opérations de domaine et les messages d’événement envoyés enregistrés dans la table.

### <a name="receiving-messages-from-subscriptions-event-handlers-in-receiver-microservices"></a>Réception de messages provenant d’abonnements : les gestionnaires d’événements dans les microservices récepteurs

En plus de la logique d’abonnement aux événements, vous devez implémenter le code interne pour les gestionnaires d’événements d’intégration (comme une méthode de rappel). C’est dans le gestionnaire d’événements que vous spécifiez où les messages d’événement d’un certain type doivent être reçus et traités.

Un gestionnaire d’événements reçoit d’abord une instance d’événement provenant du bus d’événements. Il localise ensuite le composant à traiter qui est lié à cet événement d’intégration, en propageant et en conservant l’événement en tant que changement d’état dans le récepteur de microservice. Par exemple, si un événement ProductPriceChanged provient du microservice de catalogue, il est géré dans le microservice de panier d’achat et change également d’état dans le microservice de panier récepteur, comme indiqué dans le code suivant.

```csharp
namespace Microsoft.eShopOnContainers.Services.Basket.API.IntegrationEvents.EventHandling
{
    public class ProductPriceChangedIntegrationEventHandler :
        IIntegrationEventHandler<ProductPriceChangedIntegrationEvent>
    {
        private readonly IBasketRepository _repository;

        public ProductPriceChangedIntegrationEventHandler(
            IBasketRepository repository)
        {
            _repository = repository;
        }

        public async Task Handle(ProductPriceChangedIntegrationEvent @event)
        {
            var userIds = await _repository.GetUsers();
            foreach (var id in userIds)
            {
                var basket = await _repository.GetBasket(id);
                await UpdatePriceInBasketItems(@event.ProductId, @event.NewPrice, basket);
            }
        }

        private async Task UpdatePriceInBasketItems(int productId, decimal newPrice,
            CustomerBasket basket)
        {
            var itemsToUpdate = basket?.Items?.Where(x => int.Parse(x.ProductId) ==
                productId).ToList();
            if (itemsToUpdate != null)
            {
                foreach (var item in itemsToUpdate)
                {
                    if(item.UnitPrice != newPrice)
                    {
                        var originalPrice = item.UnitPrice;
                        item.UnitPrice = newPrice;
                        item.OldUnitPrice = originalPrice;
                    }
                }
                await _repository.UpdateBasket(basket);
            }
        }
    }
}
```

Le gestionnaire d’événements doit vérifier si le produit se trouve dans une des instances du panier d’achat. Il met également à jour le prix de chaque article associé. Enfin, il crée une alerte pour l’utilisateur concernant le changement de prix, comme indiqué dans la figure 6-24.

![Capture d’écran d’un navigateur montrant la notification de changement de prix sur le panier d’utilisateur.](./media/subscribe-events/display-item-price-change.png)

**Figure 6-24.** Affichage du changement de prix d’un article du panier, communiqué par les événements d’intégration

## <a name="idempotency-in-update-message-events"></a>Idempotence des événements de mise à jour des messages

Un aspect important des événements de mise à jour des messages est qu’une défaillance se produisant à n’importe quel moment de la communication doit entraîner une nouvelle tentative de publication du message. Sinon, une tâche en arrière-plan pourrait tenter de publier un événement qui a déjà été publié, ce qui aurait pour effet de créer une condition de concurrence. Assurez-vous que les mises à jour sont soit idempotent ou qu’elles fournissent suffisamment d’informations pour vous assurer que vous pouvez détecter un double, le jeter et renvoyer une seule réponse.

Comme indiqué précédemment, l’idempotence signifie qu’une opération peut être effectuée plusieurs fois sans que le résultat soit modifié. Dans un environnement de messagerie, comme lorsque vous communiquez des événements, un événement est idempotent s’il peut être remis plusieurs fois sans modifier le résultat pour le microservice récepteur. Cela peut être nécessaire en raison de la nature de l’événement, ou en raison de la façon dont le système gère l’événement. L’idempotence des messages est importante dans les applications qui utilisent la messagerie, et pas seulement dans les applications qui implémentent le modèle de bus d’événements.

Un exemple d’opération idempotente est une instruction SQL qui insère des données dans une table uniquement si ces données ne s’y trouvent pas déjà. Quel que soit le nombre de fois que vous exécutez cette instruction SQL INSERT, le résultat sera le même et la table contiendra les mêmes données. Une telle idempotence peut également être nécessaire lorsque vous traitez des messages, si ces messages sont susceptibles d’être envoyés et donc traités plusieurs fois. Par exemple, si la logique de nouvelle tentative fait qu’un émetteur envoie exactement le même message plusieurs fois, vous devez vous assurer qu’il est idempotent.

Il est possible de concevoir des messages idempotents. Par exemple, vous pouvez créer un événement indiquant « fixer le prix du produit à 25 $ » au lieu de « ajouter 5 $ au prix du produit ». Vous pouvez traiter le premier message autant de fois que vous voulez, le résultat sera le même. Cependant, ce n’est pas vrai pour le deuxième message. Mais même dans le premier cas, il n’est pas forcément souhaitable de traiter le premier événement, car le système peut avoir envoyé un événement de changement de prix plus récent et vous écraseriez ainsi le nouveau prix.

Un autre exemple pourrait être un événement complété par une commande qui est propagé à plusieurs abonnés. L’application doit s’assurer que les informations de commande ne sont mises à jour dans d’autres systèmes qu’une seule fois, même s’il y a des événements de messages dupliqués pour le même événement complété par commande.

Il peut être utile d’attribuer une identité à chaque événement, pour que vous puissiez créer une logique selon laquelle un événement ne doit être traité qu’une seule fois pour chaque récepteur.

Le traitement des messages est fondamentalement idempotent. Par exemple, si un système génère des images miniatures, le nombre de traitements du message concernant la génération des miniatures importe peu. Le résultat est que les miniatures sont générées et qu’elles sont identiques à chaque fois. En revanche, les opérations telles que l’appel d’une passerelle de paiement pour prélever un montant sur une carte de crédit peuvent ne pas être idempotentes du tout. Dans ce cas, vous devez être sûr que le fait de traiter un message plusieurs fois aura l’effet que vous attendez.

### <a name="additional-resources"></a>Ressources supplémentaires

- **Honorer l’idempotence du message** \
  <https://docs.microsoft.com/previous-versions/msp-n-p/jj591565(v=pandp.10)#honoring-message-idempotency>

## <a name="deduplicating-integration-event-messages"></a>Déduplication des messages d’événements d’intégration

Vous pouvez vous assurer que les événements de message sont envoyés et traités qu’une seule fois par abonné à différents niveaux. L’une des méthodes possibles consiste à utiliser la fonctionnalité de déduplication proposée par l’infrastructure de messagerie que vous utilisez. Une autre consiste à implémenter une logique personnalisée dans votre microservice de destination. La meilleure solution est d’avoir des validations à la fois au niveau du transport et au niveau de l’application.

### <a name="deduplicating-message-events-at-the-eventhandler-level"></a>Déduplication d’événements de message au niveau d’EventHandler

Une façon de s’assurer qu’un événement n’est traité qu’une seule fois par un récepteur est de mettre en œuvre une certaine logique lors du traitement des événements de message dans les gestionnaires d’événements. Par exemple, c’est l’approche utilisée dans l’application eShopOnContainers, comme vous pouvez le voir dans le [code source de la classe UserCheckoutAcceptedIntegrationEventHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/IntegrationEvents/EventHandling/UserCheckoutAcceptedIntegrationEventHandler.cs) lorsqu’elle reçoit un événement d’intégration. `UserCheckoutAcceptedIntegrationEvent` (Dans ce cas, le `CreateOrderCommand` `IdentifiedCommand`est enveloppé `eventMsg.RequestId` d’un , en utilisant l’identifiant, avant de l’envoyer au gestionnaire de commande).

### <a name="deduplicating-messages-when-using-rabbitmq"></a>Déduplication de messages lors de l’utilisation de RabbitMQ

Lorsque des défaillances intermittentes du réseau se produisent, des messages peuvent être dupliqués et le récepteur des messages doit être prêt à gérer cette déduplication. Si possible, les récepteurs doivent gérer les messages de manière idempotente, ce qui est mieux que de les gérer explicitement avec la déduplication.

Selon la [documentation RabbitMQ](https://www.rabbitmq.com/reliability.html#consumer), « Si un message est livré à un consommateur, puis requeued (parce qu’il n’a pas été reconnu avant que la connexion du consommateur a chuté, par exemple), alors RabbitMQ mettra le drapeau redelivered sur elle quand il est livré à nouveau (que ce soit au même consommateur ou un autre).

Si le drapeau "redelivered" est défini, le récepteur doit en tenir compte, car le message a peut-être déjà été traité. Toutefois, cela n’est pas garanti. Le message peut ne jamais avoir atteint le récepteur après avoir quitté le répartiteur de messages, peut-être en raison de problèmes de réseau. D’autre part, si le drapeau "redelivered" n’est pas fixé, il est garanti que le message n’a pas été envoyé plus d’une fois. Par conséquent, le récepteur doit déduplicer des messages ou traiter des messages d’une manière idempotente que si le drapeau "redelivered" est placé dans le message.

### <a name="additional-resources"></a>Ressources supplémentaires

- **Fourche eShopOnContainers à l’aide de NServiceBus (Logiciel particulier)** \
    <https://go.particular.net/eShopOnContainers>

- **Messagerie axée sur l’événement** \
    <https://patterns.arcitura.com/soa-patterns/design_patterns/event_driven_messaging>

- **Jimmy Bogard. Refactoring Vers la résilience : évaluer le couplage** \
    <https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/>

- **Chaîne De publication-Abonnez-vous** \
    <https://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html>

- **Communiquer entre les contextes liés** \
    <https://docs.microsoft.com/previous-versions/msp-n-p/jj591572(v=pandp.10)>

- **Cohérence éventuelle** \
    <https://en.wikipedia.org/wiki/Eventual_consistency>

- **Philip Brown. Stratégies d’intégration des contextes délimités** \
    <https://www.culttt.com/2014/11/26/strategies-integrating-bounded-contexts/>

- **Chris Richardson. Développer des microservices transactionnels à l’aide d’agrégats, d’approvisionnement en événements et de CQRS - Partie 2** \
    <https://www.infoq.com/articles/microservices-aggregates-events-cqrs-part-2-richardson>

- **Chris Richardson. Modèle d’approvisionnement d’événements** \
    <https://microservices.io/patterns/data/event-sourcing.html>

- **Présentation de l’approvisionnement en événements** \
    <https://docs.microsoft.com/previous-versions/msp-n-p/jj591559(v=pandp.10)>

- **Base de données du magasin d’événements**. Site officiel. \
    <https://geteventstore.com/>

- **Patrick Nommensen. Gestion des données axée sur les événements pour les microservices** \
    <https://dzone.com/articles/event-driven-data-management-for-microservices-1>

- **Le théorème de la PAC** \
    <https://en.wikipedia.org/wiki/CAP_theorem>

- **What is CAP Theorem?** \
    <https://www.quora.com/What-Is-CAP-Theorem-1>

- **Apprêt de cohérence de données** \
    <https://docs.microsoft.com/previous-versions/msp-n-p/dn589800(v=pandp.10)>

- **Rick Saling. Le théorème de la PAC : pourquoi « tout est différent » avec le Cloud et Internet** \
    <https://docs.microsoft.com/archive/blogs/rickatmicrosoft/the-cap-theorem-why-everything-is-different-with-the-cloud-and-internet/>

- **Eric Brewer. CAP douze ans plus tard : comment les « règles » ont changé** \
    <https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed>

- **Bus de service Azure. Messagerie négociée : Détection en double**  \
    <https://code.msdn.microsoft.com/Brokered-Messaging-c0acea25>

- **Reliability Guide** (RabbitMQ documentation) \
    <https://www.rabbitmq.com/reliability.html#consumer>

> [!div class="step-by-step"]
> [Suivant précédent](rabbitmq-event-bus-development-test-environment.md)
> [Next](test-aspnet-core-services-web-apps.md)
