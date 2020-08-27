---
ms.openlocfilehash: 87f9cc03f334233ef286abd11e6f5ff82d7988c2
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88811343"
---
### <a name="ca1831-use-asspan-or-asmemory-instead-of-range-based-indexer"></a><span data-ttu-id="5cbdb-101">CA1831 utiliser AsSpan ou AsMemory à la place de l’indexeur basé sur une plage</span><span class="sxs-lookup"><span data-stu-id="5cbdb-101">CA1831 Use AsSpan or AsMemory instead of Range-based indexer</span></span>

<span data-ttu-id="5cbdb-102">La règle de l’analyseur de code .NET [CA1831](/visualstudio/code-quality/ca1831) est activée, par défaut, à partir de .net 5,0.</span><span class="sxs-lookup"><span data-stu-id="5cbdb-102">.NET code analyzer rule [CA1831](/visualstudio/code-quality/ca1831) is enabled, by default, starting in .NET 5.0.</span></span> <span data-ttu-id="5cbdb-103">Il génère un avertissement de génération pour tout code où un <xref:System.Range> indexeur de based est utilisé sur une chaîne, mais aucune copie n’était prévue.</span><span class="sxs-lookup"><span data-stu-id="5cbdb-103">It produces a build warning for any code where a <xref:System.Range>-based indexer is used on a string, but no copy was intended.</span></span>

#### <a name="change-description"></a><span data-ttu-id="5cbdb-104">Description de la modification</span><span class="sxs-lookup"><span data-stu-id="5cbdb-104">Change description</span></span>

<span data-ttu-id="5cbdb-105">À compter de .NET 5,0, le kit de développement logiciel (SDK) .NET comprend des [analyseurs de code source .net](../../../../docs/fundamentals/productivity/code-analysis.md).</span><span class="sxs-lookup"><span data-stu-id="5cbdb-105">Starting in .NET 5.0, the .NET SDK includes [.NET source code analyzers](../../../../docs/fundamentals/productivity/code-analysis.md).</span></span> <span data-ttu-id="5cbdb-106">Plusieurs de ces règles sont activées, par défaut, y compris [CA1831](/visualstudio/code-quality/ca1831).</span><span class="sxs-lookup"><span data-stu-id="5cbdb-106">Several of these rules are enabled, by default, including [CA1831](/visualstudio/code-quality/ca1831).</span></span> <span data-ttu-id="5cbdb-107">Si votre projet contient du code qui enfreint cette règle et est configuré pour traiter les avertissements comme des erreurs, cette modification risque de perturber votre génération.</span><span class="sxs-lookup"><span data-stu-id="5cbdb-107">If your project contains code that violates this rule and is configured to treat warnings as errors, this change could break your build.</span></span>

<span data-ttu-id="5cbdb-108">La règle CA1831 trouve des instances où un <xref:System.Range> indexeur de base est utilisé sur une chaîne, mais qu’aucune copie n’était prévue.</span><span class="sxs-lookup"><span data-stu-id="5cbdb-108">Rule CA1831 finds instances where a <xref:System.Range>-based indexer is used on a string, but no copy was intended.</span></span> <span data-ttu-id="5cbdb-109">Si l' <xref:System.Range> indexeur de base est utilisé directement sur une chaîne pour produire un cast implicite, une copie inutile de la partie demandée de la chaîne est créée.</span><span class="sxs-lookup"><span data-stu-id="5cbdb-109">If the <xref:System.Range>-based indexer is used directly on a string to produce an implicit cast, then an unnecessary copy of the requested portion of the string is created.</span></span> <span data-ttu-id="5cbdb-110">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5cbdb-110">For example:</span></span>

```csharp
ReadOnlySpan<char> slice = str[1..3];
```

<span data-ttu-id="5cbdb-111">CA1831 suggère d’utiliser à la <xref:System.Range> place l’indexeur basé sur une *étendue* de la chaîne.</span><span class="sxs-lookup"><span data-stu-id="5cbdb-111">CA1831 suggests using the <xref:System.Range>-based indexer on a *span* of the string, instead.</span></span> <span data-ttu-id="5cbdb-112">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5cbdb-112">For example:</span></span>

```csharp
ReadOnlySpan<char> slice = str.AsSpan()[1..3];
```

#### <a name="version-introduced"></a><span data-ttu-id="5cbdb-113">Version introduite</span><span class="sxs-lookup"><span data-stu-id="5cbdb-113">Version introduced</span></span>

<span data-ttu-id="5cbdb-114">5,0 Preview 8</span><span class="sxs-lookup"><span data-stu-id="5cbdb-114">5.0 Preview 8</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="5cbdb-115">Action recommandée</span><span class="sxs-lookup"><span data-stu-id="5cbdb-115">Recommended action</span></span>

- <span data-ttu-id="5cbdb-116">Pour corriger votre code et éviter les allocations inutiles, appelez <xref:System.MemoryExtensions.AsSpan(System.String)> ou <xref:System.MemoryExtensions.AsMemory(System.String)> avant d’utiliser l' <xref:System.Range> indexeur basé sur.</span><span class="sxs-lookup"><span data-stu-id="5cbdb-116">To correct your code and avoid unnecessary allocations, call <xref:System.MemoryExtensions.AsSpan(System.String)> or <xref:System.MemoryExtensions.AsMemory(System.String)> before using the <xref:System.Range>-based indexer.</span></span> <span data-ttu-id="5cbdb-117">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5cbdb-117">For example:</span></span>

  ```csharp
  ReadOnlySpan<char> slice = str.AsSpan()[1..3];
  ```

- <span data-ttu-id="5cbdb-118">Si vous ne souhaitez pas modifier votre code, vous pouvez désactiver la règle en affectant à sa gravité la valeur `suggestion` ou `none` .</span><span class="sxs-lookup"><span data-stu-id="5cbdb-118">If you don't want to change your code, you can disable the rule by setting its severity to `suggestion` or `none`.</span></span> <span data-ttu-id="5cbdb-119">Pour plus d’informations, consultez [configurer des règles d’analyse du code](../../../../docs/fundamentals/productivity/configure-code-analysis-rules.md).</span><span class="sxs-lookup"><span data-stu-id="5cbdb-119">For more information, see [Configure code analysis rules](../../../../docs/fundamentals/productivity/configure-code-analysis-rules.md).</span></span>

- <span data-ttu-id="5cbdb-120">Pour désactiver complètement l’analyse du code, affectez `EnableNETAnalyzers` à `false` la valeur dans votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="5cbdb-120">To disable code analysis completely, set `EnableNETAnalyzers` to `false` in your project file.</span></span> <span data-ttu-id="5cbdb-121">Pour plus d’informations, consultez [EnableNETAnalyzers](../../../../docs/core/project-sdk/msbuild-props.md#enablenetanalyzers).</span><span class="sxs-lookup"><span data-stu-id="5cbdb-121">For more information, see [EnableNETAnalyzers](../../../../docs/core/project-sdk/msbuild-props.md#enablenetanalyzers).</span></span>

#### <a name="category"></a><span data-ttu-id="5cbdb-122">Category</span><span class="sxs-lookup"><span data-stu-id="5cbdb-122">Category</span></span>

<span data-ttu-id="5cbdb-123">Analyse du code</span><span class="sxs-lookup"><span data-stu-id="5cbdb-123">Code analysis</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="5cbdb-124">API affectées</span><span class="sxs-lookup"><span data-stu-id="5cbdb-124">Affected APIs</span></span>

- <xref:System.Range?displayProperty=fullName>

<!--

#### Affected APIs

- `T:System.Range`

-->