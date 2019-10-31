---
ms.openlocfilehash: 7d40324e6b0bc4afab9dd39b236f0909f360cc9b
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72394274"
---
### <a name="caching-compactonmemorypressure-property-removed"></a><span data-ttu-id="ea340-101">Caching : propriété CompactOnMemoryPressure supprimée</span><span class="sxs-lookup"><span data-stu-id="ea340-101">Caching: CompactOnMemoryPressure property removed</span></span>

<span data-ttu-id="ea340-102">La version 3,0 de ASP.NET Core a supprimé les [API obsolètes de MemoryCacheOptions](https://github.com/aspnet/Extensions/blob/dc5c593da7b72c82e6fe85abb91d03818f9b700c/src/Caching/Memory/src/MemoryCacheOptions.cs#L17-L18).</span><span class="sxs-lookup"><span data-stu-id="ea340-102">The ASP.NET Core 3.0 release removed the [obsolete MemoryCacheOptions APIs](https://github.com/aspnet/Extensions/blob/dc5c593da7b72c82e6fe85abb91d03818f9b700c/src/Caching/Memory/src/MemoryCacheOptions.cs#L17-L18).</span></span>

#### <a name="change-description"></a><span data-ttu-id="ea340-103">Modifier la description</span><span class="sxs-lookup"><span data-stu-id="ea340-103">Change description</span></span>

<span data-ttu-id="ea340-104">Cette modification est un suivi de [ASPNET/Caching # 221](https://github.com/aspnet/Caching/issues/221).</span><span class="sxs-lookup"><span data-stu-id="ea340-104">This change is a follow-up to [aspnet/Caching#221](https://github.com/aspnet/Caching/issues/221).</span></span> <span data-ttu-id="ea340-105">Pour plus d’informations, consultez [ASPNET/extensions # 1062](https://github.com/aspnet/Extensions/issues/1062).</span><span class="sxs-lookup"><span data-stu-id="ea340-105">For discussion, see [aspnet/Extensions#1062](https://github.com/aspnet/Extensions/issues/1062).</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="ea340-106">Version introduite</span><span class="sxs-lookup"><span data-stu-id="ea340-106">Version introduced</span></span>

<span data-ttu-id="ea340-107">3,0</span><span class="sxs-lookup"><span data-stu-id="ea340-107">3.0</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="ea340-108">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="ea340-108">Old behavior</span></span>

<span data-ttu-id="ea340-109">la propriété `MemoryCacheOptions.CompactOnMemoryPressure` était disponible.</span><span class="sxs-lookup"><span data-stu-id="ea340-109">`MemoryCacheOptions.CompactOnMemoryPressure` property was available.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="ea340-110">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="ea340-110">New behavior</span></span>

<span data-ttu-id="ea340-111">La propriété `MemoryCacheOptions.CompactOnMemoryPressure` a été supprimée.</span><span class="sxs-lookup"><span data-stu-id="ea340-111">The `MemoryCacheOptions.CompactOnMemoryPressure` property has been removed.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="ea340-112">Motif de modification</span><span class="sxs-lookup"><span data-stu-id="ea340-112">Reason for change</span></span>

<span data-ttu-id="ea340-113">Le compactage automatique du cache a provoqué des problèmes.</span><span class="sxs-lookup"><span data-stu-id="ea340-113">Automatically compacting the cache caused problems.</span></span> <span data-ttu-id="ea340-114">Pour éviter un comportement inattendu, le cache ne doit être compacté que si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="ea340-114">To avoid unexpected behavior, the cache should only be compacted when needed.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="ea340-115">Action recommandée</span><span class="sxs-lookup"><span data-stu-id="ea340-115">Recommended action</span></span>

<span data-ttu-id="ea340-116">Pour compacter le cache, vous devez effectuer un cast aval sur `MemoryCache` et appeler `Compact` en cas de besoin.</span><span class="sxs-lookup"><span data-stu-id="ea340-116">To compact the cache, downcast to `MemoryCache` and call `Compact` when needed.</span></span>

#### <a name="category"></a><span data-ttu-id="ea340-117">Category</span><span class="sxs-lookup"><span data-stu-id="ea340-117">Category</span></span>

<span data-ttu-id="ea340-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea340-118">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="ea340-119">API affectées</span><span class="sxs-lookup"><span data-stu-id="ea340-119">Affected APIs</span></span>

<xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.CompactOnMemoryPressure%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

`Overload:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.CompactOnMemoryPressure`

-->