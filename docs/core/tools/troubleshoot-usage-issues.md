---
title: Résoudre les problèmes d’utilisation de l’outil .NET Core
description: Découvrez les problèmes courants liés à l’exécution des outils .NET Core et des solutions possibles.
author: kdollard
ms.topic: troubleshooting
ms.date: 02/14/2020
ms.openlocfilehash: db88958e1605fef589c5dbcb12065a6318183705
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608315"
---
# <a name="troubleshoot-net-core-tool-usage-issues"></a>Résoudre les problèmes d’utilisation de l’outil .NET Core

Vous risquez de rencontrer des problèmes lors de la tentative d’installation ou d’exécution d’un outil .NET Core, qui peut être un outil Global ou un outil local. Cet article décrit les causes principales et les solutions possibles.

## <a name="installed-net-core-tool-fails-to-run"></a>L’exécution de l’outil .NET Core installé échoue

En cas d’échec de l’exécution d’un outil .NET Core, vous avez probablement rencontré l’un des problèmes suivants :

* Le fichier exécutable de l’outil est introuvable.
* La version correcte du Runtime .NET Core est introuvable.

### <a name="executable-file-not-found"></a>Fichier exécutable introuvable

Si le fichier exécutable est introuvable, un message semblable au suivant s’affiche :

```console
Could not execute because the specified command or file was not found.
Possible reasons for this include:
  * You misspelled a built-in dotnet command.
  * You intended to execute a .NET Core program, but dotnet-xyz does not exist.
  * You intended to run a global tool, but a dotnet-prefixed executable with this name could not be found on the PATH.
```

Le nom de l’exécutable détermine la façon dont vous appelez l’outil. Le tableau suivant décrit le format :

| Format du nom de l’exécutable  | Format d’appel   |
|-------------------------|---------------------|
| `dotnet-<toolName>.exe` | `dotnet <toolName>` |
| `<toolName>.exe`        | `<toolName>`        |

* Outils globaux

  Les outils globaux peuvent être installés dans le répertoire par défaut ou dans un emplacement spécifique. Les répertoires par défaut sont :

  | Système d''exploitation          | Chemin d’accès                          |
  |-------------|-------------------------------|
  | Linux/macOS | `$HOME/.dotnet/tools`         |
  | Windows     | `%USERPROFILE%\.dotnet\tools` |

  Si vous essayez d’exécuter un outil Global, vérifiez que la `PATH` variable d’environnement sur votre ordinateur contient le chemin d’accès où vous avez installé l’outil Global et que l’exécutable se trouve dans ce chemin d’accès.

  Le CLI .NET Core tente d’ajouter l’emplacement par défaut à la variable d’environnement PATH lors de sa première utilisation. Toutefois, il existe des scénarios où l’emplacement ne peut pas être ajouté automatiquement au chemin d’accès :

  * Si vous utilisez Linux et que vous avez installé le kit SDK .NET Core à l’aide de fichiers *. tar. gz* et non de apt ou RPM.
  * Si vous utilisez macOS 10,15 « Catalina » ou versions ultérieures.
  * Si vous utilisez macOS 10,14 « Mojave » ou des versions antérieures, et que vous avez installé le kit SDK .NET Core à l’aide des fichiers *. tar. gz* et non *. pkg*.
  * Si vous avez installé le kit de développement logiciel (SDK) .NET Core 3,0 et que vous avez défini la `DOTNET_ADD_GLOBAL_TOOLS_TO_PATH` variable d’environnement sur `false` .
  * Si vous avez installé le kit de développement logiciel (SDK) .NET Core 2,2 ou des versions antérieures, et que vous avez défini la `DOTNET_SKIP_FIRST_TIME_EXPERIENCE` variable d’environnement sur `true` .

  Dans ces scénarios ou si vous avez spécifié l' `--tool-path` option, la `PATH` variable d’environnement sur votre ordinateur ne contient pas automatiquement le chemin d’accès où vous avez installé l’outil Global. Dans ce cas, ajoutez l’emplacement de l’outil (par exemple, `$HOME/.dotnet/tools` ) à la `PATH` variable d’environnement en utilisant la méthode fournie par votre interpréteur de commandes pour mettre à jour les variables d’environnement. Pour plus d’informations, consultez [outils .net Core](global-tools.md).

* Outils locaux

  Si vous essayez d’exécuter un outil local, vérifiez qu’il existe un fichier manifeste nommé *dotnet-tools.js* dans le répertoire actif ou dans l’un de ses répertoires parents. Ce fichier peut également résider sous un dossier nommé *. config* n’importe où dans l’arborescence des dossiers du projet, et non dans le dossier racine. Si *dotnet-tools.js* existe, ouvrez-le et recherchez l’outil que vous essayez d’exécuter. Si le fichier ne contient pas d’entrée pour `"isRoot": true` , activez également la case à cocher plus haut dans la hiérarchie des fichiers pour les fichiers de manifeste d’outils supplémentaires.

  Si vous essayez d’exécuter un outil .NET Core qui a été installé avec un chemin d’accès spécifié, vous devez inclure ce chemin d’accès lors de l’utilisation de l’outil. Voici un exemple d’utilisation d’un outil d’installation de chemin d’accès d’outil :

  ```console
  ..\<toolDirectory>\dotnet-<toolName>
  ```

### <a name="runtime-not-found"></a>Runtime introuvable

Les outils .NET Core sont des [applications dépendantes du Framework](../deploying/index.md#publish-framework-dependent), ce qui signifie qu’elles s’appuient sur un Runtime .net Core installé sur votre ordinateur. Si le runtime attendu est introuvable, ils suivent les règles de restauration par progression normales du Runtime .NET Core, telles que :

* Une application restaure par progression le correctif le plus élevé de la version principale et secondaire spécifiée.
* S’il n’existe aucun Runtime correspondant avec un numéro de version principale et secondaire correspondant, la version mineure supérieure suivante est utilisée.
* Une restauration par progression ne se produit pas entre des préversions du runtime ou entre des préversions et des versions finales. Ainsi, les outils .NET Core créés à l’aide des versions préliminaires doivent être reconstruits et republiés par l’auteur et réinstallés.

La restauration par progression ne se produit pas par défaut dans deux scénarios courants :

* Seules les versions inférieures du runtime sont disponibles. La restauration par progression sélectionne uniquement les versions ultérieures du Runtime.
* Seules les versions majeures les plus importantes du runtime sont disponibles. La restauration par progression n’entre pas dans les limites de version principales.

Si une application ne parvient pas à trouver un Runtime approprié, elle ne parvient pas à s’exécuter et signale une erreur.

Vous pouvez déterminer quels runtimes .NET Core sont installés sur votre ordinateur à l’aide de l’une des commandes suivantes :

```dotnetcli
dotnet --list-runtimes
dotnet --info
```

Si vous pensez que l’outil doit prendre en charge la version du runtime que vous avez installée actuellement, vous pouvez contacter l’auteur de l’outil et voir s’il peut mettre à jour le numéro de version ou le multi-cible. Une fois qu’ils ont recompilé et republié leur package d’outils dans NuGet avec un numéro de version mis à jour, vous pouvez mettre à jour votre copie. Bien que cela ne se produise pas, la solution la plus rapide consiste à installer une version du runtime qui fonctionnerait avec l’outil que vous essayez d’exécuter. Pour télécharger une version spécifique du Runtime .NET Core, visitez la [page de téléchargement de .net Core](https://dotnet.microsoft.com/download/dotnet-core).

Si vous installez le kit SDK .NET Core à un emplacement autre que celui par défaut, vous devez définir la variable `DOTNET_ROOT` d’environnement sur le répertoire qui contient le `dotnet` fichier exécutable.

## <a name="net-core-tool-installation-fails"></a>Échec de l’installation de l’outil .NET Core

Il existe plusieurs raisons pour lesquelles l’installation d’un outil Global ou local .NET Core peut échouer. Lorsque l’installation de l’outil échoue, un message semblable à celui-ci s’affiche :

```console
Tool '{0}' failed to install. This failure may have been caused by:

* You are attempting to install a preview release and did not use the --version option to specify the version.
* A package by this name was found, but it was not a .NET Core tool.
* The required NuGet feed cannot be accessed, perhaps because of an Internet connection problem.
* You mistyped the name of the tool.

For more reasons, including package naming enforcement, visit https://aka.ms/failure-installing-tool
```

Pour faciliter le diagnostic de ces échecs, les messages NuGet s’affichent directement à l’utilisateur, ainsi que le message précédent. Le message NuGet peut vous aider à identifier le problème.

### <a name="package-naming-enforcement"></a>Mise en application des noms de packages

Microsoft a modifié ses conseils sur l’ID de package pour les outils, ce qui a entraîné un certain nombre d’outils introuvables avec le nom prédit. La nouvelle recommandation est que tous les outils Microsoft sont préfixés avec « Microsoft ». Ce préfixe est réservé et ne peut être utilisé que pour les packages signés avec un certificat autorisé Microsoft.

Pendant la transition, certains outils Microsoft auront l’ancienne forme de l’ID de package, tandis que d’autres auront le nouveau formulaire :

```dotnetcli
dotnet tool install -g Microsoft.<toolName>
dotnet tool install -g <toolName>
```

À mesure que les ID de package sont mis à jour, vous devez passer au nouvel ID de package pour récupérer les dernières mises à jour. Les packages avec le nom de l’outil simplifié seront déconseillés.

### <a name="preview-releases"></a>Versions préliminaires

* Vous tentez d’installer une version préliminaire et vous n’avez pas utilisé l' `--version` option pour spécifier la version.

Les outils .NET Core qui sont en version préliminaire doivent être spécifiés avec une partie du nom pour indiquer qu’ils sont en préversion. Vous n’avez pas besoin d’inclure la version préliminaire entière. En supposant que les numéros de version sont au format attendu, vous pouvez utiliser une commande semblable à l’exemple suivant :

```dotnetcli
dotnet tool install -g --version 1.1.0-pre <toolName>
```

### <a name="package-isnt-a-net-core-tool"></a>Le package n’est pas un outil .NET Core

* Un package NuGet portant ce nom a été trouvé, mais il ne s’agissait pas d’un outil .NET Core.

Si vous essayez d’installer un package NuGet qui est un package NuGet standard et non un outil .NET Core, une erreur semblable à la suivante s’affiche :

> NU1212 : combinaison projet-package non valide pour `<ToolName>` . Le style de projet DotnetToolReference peut uniquement contenir des références du type DotnetTool.

### <a name="nuget-feed-cant-be-accessed"></a>Impossible d’accéder au flux NuGet

* Le flux NuGet requis n’est pas accessible, peut-être en raison d’un problème de connexion à Internet.

L’installation de l’outil nécessite l’accès au flux NuGet qui contient le package d’outils. Elle échoue si le flux n’est pas disponible. Vous pouvez modifier les flux avec `nuget.config` , demander un `nuget.config` fichier spécifique ou spécifier des flux supplémentaires avec le `--add-source` commutateur. Par défaut, NuGet génère une erreur pour tout flux qui ne peut pas se connecter. L’indicateur `--ignore-failed-sources` peut ignorer ces sources qui ne sont pas accessibles.

### <a name="package-id-incorrect"></a>ID de package incorrect

* Vous avez tapé le nom de l’outil de façon invoulue.

L’échec est généralement dû au fait que le nom de l’outil n’est pas correct. Cela peut se produire en raison d’une saisie incorrecte ou parce que l’outil a été déplacé ou déconseillé. Pour les outils sur NuGet.org, l’une des façons de s’assurer que le nom est correct consiste à Rechercher l’outil sur NuGet.org et à copier la commande d’installation.

## <a name="see-also"></a>Voir aussi

* [Outils .NET Core](global-tools.md)
