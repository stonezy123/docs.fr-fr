---
title: Nouveautés de F# 4,6- F# Guide
description: Bénéficiez d’une vue d’ensemble des nouvelles F# fonctionnalités disponibles dans 4,6.
ms.date: 11/27/2019
ms.openlocfilehash: 81d3e988d044cb16f8ec079118fd0ede2dabc587
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74644081"
---
# <a name="whats-new-in-f-46"></a><span data-ttu-id="dafc8-103">Nouveautés de F# 4,6</span><span class="sxs-lookup"><span data-stu-id="dafc8-103">What's new in F# 4.6</span></span>

<span data-ttu-id="dafc8-104">F#4,6 ajoute plusieurs améliorations au F# langage.</span><span class="sxs-lookup"><span data-stu-id="dafc8-104">F# 4.6 adds multiple improvements to the F# language.</span></span>

## <a name="get-started"></a><span data-ttu-id="dafc8-105">Prise en main</span><span class="sxs-lookup"><span data-stu-id="dafc8-105">Get started</span></span>

<span data-ttu-id="dafc8-106">F#4,6 est disponible dans toutes les distributions .NET Core et les outils Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dafc8-106">F# 4.6 is available in all .NET Core distributions and Visual Studio tooling.</span></span> <span data-ttu-id="dafc8-107">[Commencez avec F# ](../get-started/index.md) pour en savoir plus.</span><span class="sxs-lookup"><span data-stu-id="dafc8-107">[Get started with F#](../get-started/index.md) to learn more.</span></span>

## <a name="anonymous-records"></a><span data-ttu-id="dafc8-108">Enregistrements anonymes</span><span class="sxs-lookup"><span data-stu-id="dafc8-108">Anonymous records</span></span>

<span data-ttu-id="dafc8-109">Les [enregistrements anonymes](../language-reference/anonymous-records.md) sont un nouveau F# genre de type F# introduit dans 4,6.</span><span class="sxs-lookup"><span data-stu-id="dafc8-109">[Anonymous records](../language-reference/anonymous-records.md) are a new kind of F# type introduced in F# 4.6.</span></span> <span data-ttu-id="dafc8-110">Il s’agit d’agrégats simples de valeurs nommées qui n’ont pas besoin d’être déclarées avant d’être utilisées.</span><span class="sxs-lookup"><span data-stu-id="dafc8-110">They are simple aggregates of named values that don't need to be declared before use.</span></span> <span data-ttu-id="dafc8-111">Vous pouvez les déclarer comme des structs ou des types référence.</span><span class="sxs-lookup"><span data-stu-id="dafc8-111">You can declare them as either structs or reference types.</span></span> <span data-ttu-id="dafc8-112">Ils sont des types référence par défaut.</span><span class="sxs-lookup"><span data-stu-id="dafc8-112">They're reference types by default.</span></span>

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    {| Diameter = d; Area = a; Circumference = c |}

let r = 2.0
let stats = getCircleStats r
printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
    r stats.Diameter stats.Area stats.Circumference
```

<span data-ttu-id="dafc8-113">Elles peuvent également être déclarées comme structs pour lorsque vous souhaitez regrouper des types valeur et fonctionner dans des scénarios de performances :</span><span class="sxs-lookup"><span data-stu-id="dafc8-113">They can also be declared as structs for when you want to group value types and are operating in performance-sensitive scenarios:</span></span>

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    struct {| Diameter = d; Area = a; Circumference = c |}

let r = 2.0
let stats = getCircleStats r
printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
    r stats.Diameter stats.Area stats.Circumference
```

<span data-ttu-id="dafc8-114">Elles sont très puissantes et peuvent être utilisées dans de nombreux scénarios.</span><span class="sxs-lookup"><span data-stu-id="dafc8-114">They're quite powerful and can be used in numerous scenarios.</span></span> <span data-ttu-id="dafc8-115">En savoir plus sur les [enregistrements anonymes](../language-reference/anonymous-records.md).</span><span class="sxs-lookup"><span data-stu-id="dafc8-115">Learn more at [Anonymous records](../language-reference/anonymous-records.md).</span></span>

## <a name="valueoption-functions"></a><span data-ttu-id="dafc8-116">ValueOption, fonctions</span><span class="sxs-lookup"><span data-stu-id="dafc8-116">ValueOption functions</span></span>

<span data-ttu-id="dafc8-117">Le type ValueOption ajouté dans F# 4,5 a désormais « parité de fonction liée au module » avec le type d’option.</span><span class="sxs-lookup"><span data-stu-id="dafc8-117">The ValueOption type added in F# 4.5 now has "module-bound function parity" with the Option type.</span></span> <span data-ttu-id="dafc8-118">Voici quelques exemples les plus couramment utilisés :</span><span class="sxs-lookup"><span data-stu-id="dafc8-118">Some of the more commonly-used examples are as follows:</span></span>

```fsharp
// Multiply a value option by 2 if it has  value
let xOpt = ValueSome 99
let result = xOpt |> ValueOption.map (fun v -> v * 2)

// Reverse a string if it exists
let strOpt = ValueSome "Mirror image"
let reverse (str: string) =
    match str with
    | null
    | "" -> None
    | s ->
        str.ToCharArray()
        |> Array.rev
        |> string
        |> Some

let reversedString = strOpt |> Option.bind reverse
```

<span data-ttu-id="dafc8-119">Cela permet l’utilisation de ValueOption comme option dans les scénarios où un type valeur améliore les performances.</span><span class="sxs-lookup"><span data-stu-id="dafc8-119">This allows for ValueOption to be used just like Option in scenarios where having a value type improves performance.</span></span>