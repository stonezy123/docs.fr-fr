---
ms.openlocfilehash: 47f42305f4106f5e05e555a859f13c41bb950519
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88811262"
---
### <a name="http-httpclient-instances-created-by-ihttpclientfactory-log-integer-status-codes"></a>HTTP : instances HttpClient créées par IHttpClientFactory enregistrer les codes d’état des entiers

<xref:System.Net.Http.HttpClient> instances créées par <xref:System.Net.Http.IHttpClientFactory> les codes d’État http du journal sous forme d’entiers au lieu de noms de code d’État.

#### <a name="version-introduced"></a>Version introduite

5,0 Preview 1

#### <a name="old-behavior"></a>Ancien comportement

La journalisation utilise les descriptions textuelles des codes d’état HTTP. Prenez en compte les messages de journal suivants :

```output
Received HTTP response after 56.0044ms - OK
End processing HTTP request after 70.0862ms - OK
```

#### <a name="new-behavior"></a>Nouveau comportement

La journalisation utilise les valeurs entières des codes d’état HTTP. Prenez en compte les messages de journal suivants :

```output
Received HTTP response after 56.0044ms - 200
End processing HTTP request after 70.0862ms - 200
```

#### <a name="reason-for-change"></a>Motif de modification

Le comportement d’origine de cette journalisation est incohérent avec d’autres parties de ASP.NET Core qui ont toujours utilisé des valeurs entières. L’incohérence rend les journaux difficiles à interroger par le biais de systèmes de journalisation structurés tels que [Elasticsearch](https://www.elastic.co/elasticsearch/). Pour plus de contexte, consultez [dotnet/extensions # 1549](https://github.com/dotnet/extensions/issues/1549).

L’utilisation de valeurs entières est plus souple que le texte, car elle autorise des requêtes sur des plages de valeurs.

L’ajout d’une autre valeur de journal pour capturer le code d’État entier a été pris en compte. Malheureusement, cela introduirait une autre incohérence avec le reste de ASP.NET Core. La journalisation HttpClient et la journalisation du serveur/de l’hébergement HTTP utilisent déjà le même `StatusCode` nom de clé.

#### <a name="recommended-action"></a>Action recommandée

La meilleure option consiste à mettre à jour les requêtes de journalisation pour utiliser les valeurs entières des codes d’État. Cette option peut entraîner des difficultés à écrire des requêtes sur plusieurs versions de ASP.NET Core. Toutefois, l’utilisation d’entiers à cet effet est bien plus flexible pour l’interrogation des journaux.

Si vous devez forcer la compatibilité avec l’ancien comportement et utiliser des codes d’État textuels, remplacez la `IHttpClientFactory` journalisation par les vôtres :

1. Copiez les versions .NET Core 3,1 des classes suivantes dans votre projet :

    * [LoggingHttpMessageHandlerBuilderFilter](https://github.com/dotnet/extensions/blob/release/3.1/src/HttpClientFactory/Http/src/Logging/LoggingHttpMessageHandlerBuilderFilter.cs)
    * [LoggingHttpMessageHandler](https://github.com/dotnet/extensions/blob/release/3.1/src/HttpClientFactory/Http/src/Logging/LoggingHttpMessageHandler.cs)
    * [LoggingScopeHttpMessageHandler](https://github.com/dotnet/extensions/blob/release/3.1/src/HttpClientFactory/Http/src/Logging/LoggingScopeHttpMessageHandler.cs)
    * [HttpHeadersLogValue](https://github.com/dotnet/extensions/blob/release/3.1/src/HttpClientFactory/Http/src/Logging/HttpHeadersLogValue.cs)

1. Renommez les classes pour éviter les conflits avec les types publics dans le package NuGet [Microsoft. extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http) .

1. Remplacez l’implémentation intégrée de par le `LoggingHttpMessageHandlerBuilderFilter` vôtre dans la méthode du projet `Startup.ConfigureServices` . Par exemple :

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // Other service registrations go first. Code omitted for brevity.

        // Place the following after all AddHttpClient registrations.
        var descriptors = services.Where(
            s => s.ServiceType == typeof(IHttpMessageHandlerBuilderFilter));
        foreach (var descriptor in descriptors)
        {
            services.Remove(descriptor);
        }

        services.AddSingleton<IHttpMessageHandlerBuilderFilter,
                              MyLoggingHttpMessageHandlerBuilderFilter>();
    }
    ```

#### <a name="category"></a>Category

ASP.NET Core

#### <a name="affected-apis"></a>API affectées

<xref:System.Net.Http.HttpClient?displayProperty=nameWithType>

<!--

#### Affected APIs

`T:System.Net.Http.HttpClient`

-->
