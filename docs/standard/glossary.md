---
title: Glossaire .NET
description: Découvrez la signification de certains termes utilisés dans la documentation .NET.
ms.date: 01/22/2019
ms.technology: dotnet-standard
ms.openlocfilehash: 59e338de99510759e3e7acfd782915ed6dc5d988
ms.sourcegitcommit: 60dc0a11ebdd77f969f41891d5cca06335cda6a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88957571"
---
# <a name="net-glossary"></a>Glossaire .NET

L’objectif principal de ce glossaire est de préciser la signification de certains termes et acronymes qui apparaissent fréquemment dans la documentation .NET sans y être définis.

## <a name="aot"></a>AOT

Compilateur Ahead Of Time.

Semblable au compilateur [JIT](#jit), ce compilateur convertit également le langage [IL](#il) en code machine. Contrairement à la compilation JIT, la compilation AOT se produit avant que l’application ne soit exécutée et est généralement effectuée sur une autre machine. Étant donné que les chaînes d’outils AOA ne sont pas compilées au moment de l’exécution, elles n’ont pas à réduire le temps passé à compiler. Ainsi, elles peuvent dédier plus de temps à l’optimisation. Étant donné que le contexte d’AOT est l’ensemble de l’application, le compilateur AOT effectue également une liaison entre modules et une analyse de la totalité du programme, ce qui signifie que toutes les références sont suivies et qu’un seul exécutable est produit.

Consultez [CoreRT](#corert) et [.NET Native](#net-native).

## <a name="aspnet"></a>ASP.NET

Implémentation ASP.NET d’origine fournie avec .NET Framework.

ASP.NET est parfois un terme général qui désigne les deux implémentations d’ASP.NET, y compris ASP.NET Core. C’est le contexte qui détermine la signification véhiculée par le terme. Reportez-vous à ASP.NET 4. x pour indiquer clairement que vous n’utilisez pas ASP.NET pour signifier les deux implémentations.

Voir [Documentation d’ASP.NET](/aspnet/#pivot=aspnet).

## <a name="aspnet-core"></a>ASP.NET Core

Une implémentation multiplateforme, hautes performances et open source de ASP.NET.

Voir [Documentation ASP.NET Core](/aspnet/#pivot=core).

## <a name="assembly"></a>assembly

Fichier *. dll* / *. exe* qui peut contenir une collection d’API pouvant être appelées par des applications ou d’autres assemblys.

Un assembly peut inclure des types comme des interfaces, des classes, des structures, des énumérations et des délégués. Les assemblys qui se trouvent dans le dossier *bin* d’un projet sont parfois appelés *binaires*. Voir aussi [bibliothèque](#library).

## <a name="bcl"></a>BCL

Bibliothèque de classes de base. Également appelées *bibliothèques d’infrastructure*.

Ensemble de bibliothèques qui composent le système. \* (et, dans une certaine mesure, Microsoft. \* ) espaces. La bibliothèque de classes de base est un framework de niveau inférieur à usage général sur lequel reposent les frameworks d’applications de niveau supérieur, tels qu’ASP.NET Core.

Le code source de la bibliothèque de classes de [base pour .net 5 (et .net Core) et les versions ultérieures](#net-5-and-later-versions) est contenu dans le [référentiel du Runtime .net](https://github.com/dotnet/runtime). La majorité des API BCL pour cette implémentation plus récente de .NET sont également disponibles dans .NET Framework, ce qui vous permet de considérer ce code source comme une fourche du code source BCL .NET Framework.

## <a name="clr"></a>CLR

Common Language Runtime.

La signification exacte dépend du contexte. Le Common Language Runtime fait généralement référence au runtime de [.NET Framework](#net-framework) ou au runtime de [.net 5 (et .net Core) et versions ultérieures](#net-5-and-later-versions).

Un CLR gère l’allocation et la gestion de la mémoire. Un CLR est également un ordinateur virtuel qui exécute non seulement des applications, mais génère et compile du code à la volée à l’aide d’un compilateur [JIT](#jit) .

L’implémentation du CLR pour .NET Framework est Windows uniquement.

L’implémentation du CLR pour .NET 5 et versions ultérieures (également appelée CLR principale) est construite à partir de la même base de code que le CLR .NET Framework. À l’origine, le CLR principal était le runtime de Silverlight et a été conçu pour s’exécuter sur plusieurs plateformes, en particulier Windows et OS X. Il s’agit toujours d’un Runtime [multiplateforme](#cross-platform) , incluant désormais la prise en charge de nombreuses distributions Linux.

Voir aussi [Runtime](#runtime).

## <a name="core-clr"></a>CLR principal

Le Common Language Runtime pour [.net 5 (et .net Core) et versions ultérieures](#net-5-and-later-versions).

Voir [CLR](#clr)

## <a name="corert"></a>CoreRT

Contrairement au [CLR](#clr), CoreRT n’est pas un ordinateur virtuel, ce qui signifie qu’il n’inclut pas les fonctionnalités permettant de générer et d’exécuter du code à la volée, car il n’inclut pas de [JIT](#jit). Toutefois, elle inclut le [catalogue global](#gc) et la capacité d’identification du type au moment de l’exécution (RTTI) et de la réflexion. Toutefois, son système de type est conçu pour que les métadonnées de réflexion ne soient pas nécessaires. Le fait de ne pas exiger de métadonnées permet d’avoir une chaîne d’outils [AOA](#aot) qui peut lier des métadonnées superflues et, plus important encore, identifier du code que l’application n’utilise pas. CoreRT est en cours de développement.

Consultez [Introduction à .net native et CoreRT](https://github.com/dotnet/corert/blob/master/Documentation/intro-to-corert.md).

## <a name="cross-platform"></a>interplateformes

Possibilité de développer et d’exécuter une application qui peut être utilisée sur plusieurs systèmes d’exploitation différents, tels que Linux, Windows et iOS, sans avoir à les réécrire spécifiquement pour chacune d’elles. Cela permet la réutilisation et la cohérence du code entre les applications sur différentes plateformes.

## <a name="ecosystem"></a>écosystème

Tous les logiciels d’exécution, outils de développement et ressources de communautés qui permettent de générer et d’exécuter des applications pour une technologie donnée.

Le terme « écosystème .NET » diffère des termes tels que « pile .NET » en ce sens qu’il inclut les bibliothèques et les applications tierces. Voici un exemple dans une phrase :

- « L’objectif de [.NET Standard](#net-standard) est d’établir une meilleure uniformité dans l’écosystème .NET. »

## <a name="framework"></a>framework

En général, ensemble complet d’API qui facilite le développement et le déploiement d’applications basées sur une technologie particulière. Selon ce sens général, ASP.NET Core et Windows Forms sont des exemples de frameworks d’application. Voir aussi [bibliothèque](#library).

Le terme « Framework » a une signification différente dans les termes suivants :

- [.NET Framework](#net-framework)
- [Framework cible](#target-framework)
- [TFM (moniker de la version cible de .Net Framework)](#tfm)
- [application dépendante du Framework](../core/deploying/index.md#publish-framework-dependent)

Dans la documentation .NET héritée, « Framework » se réfère parfois à une [implémentation de .net](#implementation-of-net). Par exemple, un article peut appeler .NET 5 a Framework.

## <a name="gc"></a>GC

Garbage collector.

Le garbage collector est une implémentation de la gestion automatique de la mémoire.  Le GC libère la mémoire occupée par les objets qui ne sont plus en cours d’utilisation.

Consultez [Nettoyage de la mémoire](garbage-collection/index.md).

## <a name="il"></a>IL

Langage intermédiaire.

Les langages .NET de haut niveau, tels que C#, se compilent en un jeu d’instructions indépendant du matériel, qui est appelé Langage intermédiaire (IL). IL est parfois appelé MSIL (Microsoft IL) ou CIL (Common IL).

## <a name="jit"></a>JIT

Compilateur juste-à-temps.

Semblable au compilateur [AOT](#aot), ce compilateur convertit le langage [IL](#il) en code machine que le processeur comprend. Contrairement à la compilation AOT, la compilation JIT se produit à la demande et est effectuée sur la machine sur laquelle le code doit s’exécuter. Étant donné que la compilation JIT se produit pendant l’exécution de l’application, la compilation fait partie du temps d’exécution. Ainsi, les compilateurs JIT doivent trouver un équilibre entre le temps consacré à l’optimisation du code et les économies pouvant découler du code résultant. Toutefois, un compilateur JIT connaît le matériel réel et peut éviter aux développeurs d’avoir à transmettre différentes implémentations.

## <a name="implementation-of-net"></a>implémentation de .NET

Une implémentation de .NET comprend les éléments suivants :

- Un ou plusieurs runtimes. Exemples : [CLR](#clr), [CoreRT](#corert).
- Une bibliothèque de classes qui implémente une version de .NET Standard et qui peut inclure des API supplémentaires. Exemples : [BCL](#bcl) pour [.NET Framework](#net-framework) et [.net 5 (et .net Core) et versions ultérieures](#net-5-and-later-versions).
- Le cas échéant, un ou plusieurs frameworks d’application. Exemples : [ASP.net](#aspnet), Windows Forms et WPF sont inclus dans les .NET Framework et .net 5.
- Le cas échéant, des outils de développement. Certains outils de développement sont partagés entre plusieurs implémentations.

Exemples d’implémentations de .NET :

- [.NET Framework](#net-framework)
- [.NET 5 et versions ultérieures (y compris .NET Core 2.1-3.1)](#net-5-and-later-versions)
- [Plateforme Windows universelle (UWP)](#uwp)
- [Mono](#mono)

## <a name="library"></a>bibliothèque

Ensemble d’API que peuvent appeler les applications ou d’autres bibliothèques. Une bibliothèque .NET est composée d’un ou plusieurs [assemblys](#assembly).

Les mots bibliothèque et [framework](#framework) sont souvent utilisés indifféremment.

## <a name="mono"></a>Mono

Mono est une implémentation de .NET open source [multiplateforme](#cross-platform) qui est principalement utilisée quand un runtime réduit est requis. C’est le runtime qui alimente les applications Xamarin sur Android, Mac, iOS, tvOS et Watchos, et se concentre principalement sur les applications qui requièrent un faible encombrement.

Il prend en charge toutes les versions de .NET Standard publiées.

Historiquement, Mono implémentait l’API plus volumineuse de .NET Framework et émulait certaines des fonctionnalités les plus populaires sur Unix. Il est parfois utilisé pour exécuter des applications .NET qui s’appuient sur ces fonctionnalités sous Unix.

Mono est généralement utilisé avec un [compilateur juste-à-temps](#jit), mais il comporte également un [compilateur statique complet (compilation à l’avance)](#aot) qui est utilisé sur des plateformes comme iOS.

Consultez la [documentation mono](https://www.mono-project.com/docs/).

## <a name="net"></a>.NET

Terme générique désignant [.NET Standard](#net-standard), ainsi que toutes les charges de travail et les [implémentations de .NET](#implementation-of-net). Toujours entièrement en majuscules, jamais « .net ».

Lorsque [.net 5](#net-5-and-later-versions) (actuellement en préversion) est publié, il s’agit de l’implémentation .NET recommandée pour tout nouveau développement .net, et donc, dans certains contextes, « .net » implique « .net 5 et versions ultérieures ».

Consultez le [Guide .net](index.yml)

## <a name="net-5-and-later-versions"></a>.NET 5 et versions ultérieures

Une implémentation multiplateforme, hautes performances et open source de .NET. Comprend un[CLR](#clr)(Common Language Runtime), un Runtime [AOA](#aot) ([CoreRT](#corert), en développement), une bibliothèque de classes de base ([BCL](#bcl)) et le [Kit de développement logiciel (SDK) .net](#net-sdk).

Les versions antérieures de cette implémentation .NET sont appelées .NET Core. .NET 5,0 est la prochaine version qui suit .NET Core 3,1. La version 4 a été ignorée pour éviter de confondre cette implémentation plus récente de .NET avec l’ancienne implémentation connue sous le nom de [.NET Framework](#net-framework). La version actuelle de .NET Framework est 4,8.

Consultez [.net](../core/index.yml).

## <a name="net-cli"></a>CLI .NET

Chaîne d’outils multiplateforme pour le développement d’applications et de bibliothèques pour [.net 5 (et .net Core) et versions ultérieures](#net-5-and-later-versions). Également connu sous le nom de CLI .NET Core.

Consultez la section [CLI .net](../core/tools/index.md).

## <a name="net-core"></a>.NET Core

Consultez [.net 5 et versions ultérieures](#net-5-and-later-versions).

## <a name="net-framework"></a>.NET Framework

Implémentation de .NET qui s’exécute uniquement sur Windows. Comprend le[CLR](#clr)(Common Language Runtime), la bibliothèque de classes de base ([BCL](#bcl)) et les bibliothèques de l’infrastructure d’application, telles que [ASP.net](#aspnet), Windows Forms et WPF.

Consultez [Guide du .NET Framework](../framework/index.yml).

## <a name="net-native"></a>.NET Native

Chaîne d’outils du compilateur qui produit du code natif à l’avance ([AOA](#aot)), par opposition à juste-à-temps ([JIT](#jit)).

La compilation se produit sur la machine du développeur, à l’image du fonctionnement d’un éditeur de liens et d’un compilateur C++. Elle supprime le code inutilisé et consacre plus de temps à l’optimisation du code. Elle extrait le code des bibliothèques et le fusionne dans le fichier exécutable. Le résultat est un module unique qui représente l’application entière.

UWP fut le premier framework d’application pris en charge par .NET Native. De nos jours, nous prenons en charge la génération d’applications console natives pour Windows, macOS et Linux.

Consultez [Présentation de .NET Native et CoreRT](https://github.com/dotnet/corert/blob/master/Documentation/intro-to-corert.md).

## <a name="net-sdk"></a>Kit de développement logiciel (SDK) .NET

Ensemble de bibliothèques et d’outils qui permettent aux développeurs de créer des applications et des bibliothèques .NET pour [.net 5 (et .net Core) et versions ultérieures](#net-5-and-later-versions). Également connu sous le nom de kit SDK .NET Core.

Comprend l' [interface CLI .net](#net-cli) pour la génération d’applications, les bibliothèques .net et le runtime pour la génération et l’exécution d’applications, ainsi que l’exécutable dotnet (*dotnet.exe*) qui exécute des commandes CLI et exécute des applications.

Consultez [vue d’ensemble du SDK .net](../core/sdk.md).

## <a name="net-standard"></a>.NET Standard

Spécification formelle des API .NET disponibles dans chaque implémentation de .NET.

La spécification .NET Standard est parfois appelée bibliothèque dans la documentation. Comme une bibliothèque inclut des implémentations d’API, outre des spécifications (interfaces), il est trompeur d’appeler .NET Standard une « bibliothèque ».

Consultez [.NET Standard](net-standard.md).

## <a name="ngen"></a>NGEN

Génération (d’images) native

Vous pouvez considérer cette technologie comme un compilateur [JIT](#jit) persistant. Elle compile généralement le code sur la machine où le code est exécuté, mais la compilation se produit en règle générale au moment de l’installation.

## <a name="package"></a>package

Un package NuGet &mdash; ou simplement un package &mdash; est un fichier *.zip* qui comporte un ou plusieurs assemblys portant le même nom, ainsi que des métadonnées supplémentaires, telles que le nom de l’auteur.

Le fichier *.zip* porte l’extension *.nupkg* et peut contenir des composants, tels que des fichiers *.dll* et des fichiers *.xml*, à utiliser avec plusieurs frameworks et versions cibles. Quand ils sont installés dans une application ou une bibliothèque, les composants appropriés sont sélectionnés en fonction du framework cible spécifié par l’application ou la bibliothèque. Les composants qui définissent l’interface se trouvent dans le dossier *ref*, tandis que les ressources qui définissent l’implémentation se trouvent dans le dossier *lib*.

## <a name="platform"></a>plateforme

Système d’exploitation et le matériel sur lequel il s’exécute, tel que Windows, macOS, Linux, iOS et Android.

Voici quelques exemples d’utilisation dans des phrases :

- « .NET Core est une implémentation multiplateforme de .NET ».
- « Les profils de bibliothèque de classes portable représentent les plateformes Microsoft, alors que .NET Standard est indépendant de la plateforme. »

La documentation .NET héritée utilise parfois « plateforme .NET » pour signifier une [implémentation de .net ou de](#implementation-of-net) la [pile](#stack) .net, y compris toutes les implémentations. Ces deux utilisations ont tendance à être confondues avec la signification principale (système d’exploitation/matériel). nous essayons donc d’éviter ces utilisations.

« Plateforme » a une signification différente dans l’expression « plateforme de développement », qui fait référence à un logiciel qui fournit des outils et des bibliothèques pour la création et l’exécution d’applications. .NET est une plateforme de développement multiplateforme et open source permettant de créer de nombreux types d’applications différents.

## <a name="runtime"></a>runtime

En général, l’environnement d’exécution d’un programme managé. Le système d’exploitation fait partie de l’environnement d’exécution, mais pas du runtime .NET. Voici quelques exemples de runtimes .NET dans ce sens :

- [CLR](#clr)(Common Language Runtime)
- .NET Native (pour la plateforme Windows universelle)
- Runtime Mono

Le mot « Runtime » a une signification différente dans les contextes suivants :

* La [page de téléchargement de .net](https://dotnet.microsoft.com/download).

  « Runtime » est ici le [CLR](#clr) avec le [BCL](#bcl) (bibliothèques d’infrastructure) que vous pouvez télécharger et installer sur un ordinateur afin de pouvoir exécuter des applications [dépendantes](../core/deploying/index.md#publish-framework-dependent) de l’infrastructure sur l’ordinateur.

* [Identificateur de Runtime (RID)](../core/rid-catalog.md) pour [.net 5 (et .net Core) et versions ultérieures](#net-5-and-later-versions).

  « Runtime » correspond à la plateforme du système d’exploitation et à l’architecture du processeur sur laquelle s’exécute une application .NET, par exemple : `linux-x64` .

La documentation .NET héritée utilise parfois « Runtime » dans le sens d’une [implémentation de .net](#implementation-of-net), comme dans les exemples suivants :

- « Les différents runtimes .NET implémentent des versions spécifiques de .NET Standard. »
- « Les bibliothèques destinées à s’exécuter sur plusieurs runtimes doivent cibler ce framework. » (s’applique à .NET Standard)
- « Les différents runtimes .NET implémentent des versions spécifiques de .NET Standard. … Chaque version du runtime .NET publie la version .NET Standard la plus élevée qu’elle prend en charge... »

## <a name="stack"></a>pile

Ensemble de technologies de programmation qui sont utilisées conjointement pour générer et exécuter des applications.

L’expression « la pile .NET » fait référence à .NET Standard et à toutes les implémentations de .NET. L’expression « une pile .NET » peut faire référence à une implémentation de .NET.

## <a name="target-framework"></a>version cible de .NET Framework

Ensemble d’API sur lequel repose une bibliothèque ou une application .NET.

Une application ou une bibliothèque peut cibler une version de .NET Standard (par exemple, .NET Standard 2,0), qui est une spécification pour un ensemble standardisé d’API dans toutes les implémentations .NET. Une application ou une bibliothèque peut également cibler une version d’une implémentation spécifique de .NET ; dans ce cas, elle a accès aux API spécifiques à l’implémentation. Par exemple, une application qui cible Xamarin.iOS accède aux wrappers d’API iOS fournis par Xamarin.

Pour certains frameworks cibles (par exemple, .NET Framework), les API disponibles sont définies par les assemblys qu’une implémentation de .NET installe sur un système. Les API peuvent inclure des API de framework d’application (par exemple, ASP.NET ou WinForms). Pour les frameworks cibles basés sur le package (par exemple, .NET Standard et .NET Core), les API de framework sont définies par les packages installés dans l’application ou la bibliothèque. Dans ce cas, le Framework cible spécifie implicitement un package qui fait référence à tous les packages qui composent le Framework.

Consultez [Versions cibles de .NET Framework](frameworks.md).

## <a name="tfm"></a>TFM

Moniker de la version cible de .Net Framework.

Format de jeton standardisé pour la spécification du framework cible d’une bibliothèque ou d’une application .NET. Les frameworks cibles sont en général référencés par un nom court, tel que `net462`. Les TFM de forme longue (par exemple, .NETFramework,Version=4.6.2) existent, mais ne sont généralement pas utilisés pour spécifier un framework cible.

Consultez [Versions cibles de .NET Framework](frameworks.md).

## <a name="uwp"></a>UWP

Plateforme Windows universelle.

Implémentation de .NET qui sert à générer des logiciels et des applications Windows tactiles modernes pour l’Internet des objets (IoT). Il est conçu pour unifier les différents types d’appareils que vous pouvez cibler, y compris les PC, les tablettes, les téléphones et même la Xbox. UWP fournit de nombreux services, comme un magasin d’applications centralisé, un environnement d’exécution (AppContainer) et un ensemble d’API Windows à utiliser à la place de Win32 (WinRT). Les applications peuvent être écrites en C++, C#, Visual Basic et JavaScript. Lorsque vous utilisez C# et Visual Basic, les API .NET sont fournies par .NET 5 (et .NET Core) et les versions ultérieures.

## <a name="see-also"></a>Voir aussi

- [Guide .NET](index.yml)
- [Guide de .NET Framework](../framework/index.yml)
- [.NET Core](../core/index.yml)
- [Présentation de ASP.NET](/aspnet/index#pivot=aspnet)
- [Présentation de ASP.NET Core](/aspnet/index#pivot=core)
