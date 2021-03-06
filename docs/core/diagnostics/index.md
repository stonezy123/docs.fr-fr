---
title: Vue d’ensemble des outils de diagnostics - .NET Core
description: Une vue d’ensemble des outils et techniques disponibles pour diagnostiquer les applications .NET Core.
ms.date: 07/16/2020
ms.topic: overview
ms.openlocfilehash: ae3b9a1961f331c9cdea786bd5fe06b7bfa10927
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558112"
---
# <a name="what-diagnostic-tools-are-available-in-net-core"></a>Quels sont les outils de diagnostic disponibles dans .NET Core ?

Les logiciels ne se comportent pas toujours comme vous l'attendez, mais .NET Core dispose d’outils et d’API qui vous aideront à diagnostiquer ces problèmes rapidement et efficacement.

Cet article vous aide à trouver les différents outils dont vous avez besoin.

## <a name="managed-debuggers"></a>Débogueurs gérés

Les [débogueurs managés](managed-debuggers.md) vous permettent d’interagir avec votre programme. La pause, l'exécution incrémentale, l'examen et la reprise vous offrent un aperçu du comportement de votre code. Un débogueur est le premier choix pour diagnostiquer les problèmes fonctionnels qui peuvent être facilement reproduits.

## <a name="logging-and-tracing"></a>Journalisation et suivi

La [journalisation et le suivi](logging-tracing.md) sont des techniques associées. Elles se réfèrent au code d'instrumentation permettant de créer des fichiers journaux. Les fichiers consignent le détail des tâches exécutées par un programme. Ces informations peuvent être utilisées pour diagnostiquer les problèmes les plus complexes. Combinées à l'horodatage, ces techniques sont également très utiles dans les analyses de performances.

## <a name="unit-testing"></a>Test des unités

Le [test unitaire](../testing/index.md) est un composant clé de l’intégration et du déploiement continus de logiciels de haute qualité. Les tests unitaires sont conçus pour vous prévenir d’un problème survenu.

## <a name="net-core-dotnet-diagnostic-global-tools"></a>Outils globaux .NET Core dotnet diagnostic

### <a name="dotnet-counters"></a>dotnet-counters

[dotnet-Counters](dotnet-counters.md) est un outil d’analyse des performances pour l’analyse de l’intégrité et des performances de premier niveau. Il observe les valeurs de compteur de performance publiées par le biais de l' <xref:System.Diagnostics.Tracing.EventCounter> API. Par exemple, vous pouvez rapidement surveiller des éléments tels que l’utilisation du processeur ou le taux d’exceptions levées dans votre application .NET Core.

### <a name="dotnet-dump"></a>dotnet-dump

L’outil [dotnet-dump](dotnet-dump.md) est un moyen de collecter et d’analyser les vidages noyau Windows et Linux sans débogueur natif.

### <a name="dotnet-gcdump"></a>dotnet-gcdump

L’outil [dotnet-gcdump](dotnet-gcdump.md) est un moyen de collecter des vidages de mémoire GC (garbage collector) des processus .net en direct.

### <a name="dotnet-trace"></a>dotnet-trace

.NET Core comprend ce qui est appelé `EventPipe` par le biais duquel les données de diagnostic sont exposées. L’outil [dotnet-trace](dotnet-trace.md) vous permet de consommer des données de profilage intéressantes à partir de votre application, ce qui peut aider dans les scénarios où vous devez provoquer une exécution lente des applications.

## <a name="net-core-diagnostics-tutorials"></a>Tutoriels de diagnostics .NET Core

### <a name="debug-a-memory-leak"></a>Déboguer une fuite de mémoire

[Didacticiel : déboguer une fuite de mémoire](debug-memory-leak.md) vous guide tout au long de la détection d’une fuite de mémoire. L’outil [dotnet-Counters](dotnet-counters.md) est utilisé pour confirmer la fuite et l’outil [dotnet-dump](dotnet-dump.md) est utilisé pour diagnostiquer la fuite.

### <a name="debug-high-cpu-usage"></a>Déboguer une utilisation élevée du processeur

[Didacticiel : déboguer l’utilisation intensive du processeur](debug-highcpu.md) vous guide tout au long de l’examen de l’utilisation intensive du processeur. Elle utilise l’outil [dotnet-Counters](dotnet-counters.md) pour vérifier l’utilisation intensive du processeur. Il vous guide ensuite dans l’utilisation [de trace for Performance Analysis Utility ( `dotnet-trace` )](dotnet-trace.md) ou Linux `perf` pour collecter et afficher le profil d’utilisation de l’UC.

### <a name="debug-deadlock"></a>Déboguer un interblocage

[Didacticiel : déboguer le blocage](debug-deadlock.md) vous montre comment utiliser l’outil [dotnet-dump](dotnet-dump.md) pour examiner les threads et les verrous.
