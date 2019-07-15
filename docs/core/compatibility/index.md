---
title: Évaluer les changements cassants - .NET Core
description: Pour en savoir plus sur les façons dont .NET Core tente de maintenir la compatibilité pour les développeurs entre les versions de .NET.
author: rpetrusha
ms.author: ronpet
ms.date: 06/10/2019
ms.openlocfilehash: c68a19b8b98a98bb9c64f5b9fa60b378935e6e93
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67736560"
---
# <a name="evaluate-breaking-changes-in-net-core"></a><span data-ttu-id="992ab-103">Évaluer les changements cassants dans .NET Core</span><span class="sxs-lookup"><span data-stu-id="992ab-103">Evaluate breaking changes in .NET Core</span></span>

<span data-ttu-id="992ab-104">Tout au long de son histoire, .NET a tenté de maintenir un niveau élevé de compatibilité de version en version et entre les différentes déclinaisons de .NET.</span><span class="sxs-lookup"><span data-stu-id="992ab-104">Throughout its history, .NET has attempted to maintain a high level of compatibility from version to version and across flavors of .NET.</span></span> <span data-ttu-id="992ab-105">Cette philosophie s’applique toujours avec .NET Core.</span><span class="sxs-lookup"><span data-stu-id="992ab-105">This continues to be true for .NET Core.</span></span> <span data-ttu-id="992ab-106">Bien que .NET Core peut être considéré comme une nouvelle technologie indépendante du .NET Framework, deux facteurs majeurs limitent la capacité de .NET Core à diverger du .NET Framework :</span><span class="sxs-lookup"><span data-stu-id="992ab-106">Although .NET Core can be considered as a new technology that is independent of the .NET Framework, two major factors limit the ability of .NET Core to diverge from .NET Framework:</span></span>

- <span data-ttu-id="992ab-107">Un grand nombre de développeurs ont développé à la base ou continuent à développer des applications .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="992ab-107">A large number of developers either originally developed or continue to develop .NET Framework applications.</span></span> <span data-ttu-id="992ab-108">Ils s’attendent à un comportement cohérent entre les implémentations de .NET.</span><span class="sxs-lookup"><span data-stu-id="992ab-108">They expect consistent behavior across .NET implementations.</span></span>

- <span data-ttu-id="992ab-109">Les projets de bibliothèque .NET Standard permettent aux développeurs de créer des bibliothèques qui ciblent les API communes partagées par .NET Core et .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="992ab-109">.NET Standard library projects allow developers to create libraries that target common APIs shared by .NET Core and .NET Framework.</span></span> <span data-ttu-id="992ab-110">Les développeurs s’attendent à ce qu’une bibliothèque utilisée dans une application .NET Core se comporte comme la bibliothèque utilisée dans une application .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="992ab-110">Developers expect that a library used in a .NET Core application should behave identically to the same library used in a .NET Framework application.</span></span>

<span data-ttu-id="992ab-111">En plus de la compatibilité entre les implémentations de .NET, les développeurs s’attendent à un haut niveau de compatibilité entre les versions de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="992ab-111">Along with compatibility across .NET implementations, developers expect a high level of compatibility across .NET Core versions.</span></span> <span data-ttu-id="992ab-112">En particulier, le code écrit pour une version antérieure de .NET Core doit s’exécuter sans problème sur une version ultérieure de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="992ab-112">In particular, code written for an earlier version of .NET Core should run seamlessly on a later version of .NET Core.</span></span> <span data-ttu-id="992ab-113">En fait, de nombreux développeurs s’attendent à ce que les nouvelles API dans les dernières versions de .NET Core soient également compatibles avec les versions préliminaires dans lesquelles ces API ont été introduites.</span><span class="sxs-lookup"><span data-stu-id="992ab-113">In fact, many developers expect that the new APIs found in newly released versions of .NET Core should also be compatible with the pre-release versions in which those APIs were introduced.</span></span>

<span data-ttu-id="992ab-114">Cet article décrit les catégories de modifications de compatibilité (ou changements cassants) et la manière dont l’équipe .NET évalue les modifications dans chacune de ces catégories.</span><span class="sxs-lookup"><span data-stu-id="992ab-114">This article outlines the categories of compatibility changes (or breaking changes) and the way in which the .NET team evaluates changes in each of these categories.</span></span> <span data-ttu-id="992ab-115">Comprendre comment l’équipe .NET approche les changements cassants potentiels est particulièrement utile pour les développeurs qui ouvrent des demandes de tirage dans le référentiel GitHub [dotnet/corefx](https://github.com/dotnet/corefx) qui modifient le comportement des API existantes.</span><span class="sxs-lookup"><span data-stu-id="992ab-115">Understanding how the .NET team approaches possible breaking changes is particularly helpful for developers who are opening pull requests in the [dotnet/corefx](https://github.com/dotnet/corefx) GitHub repository that modify the behavior of existing APIs.</span></span>

> [!NOTE]
> <span data-ttu-id="992ab-116">Pour une définition des catégories de compatibilité, comme la compatibilité binaire et la compatibilité descendante, consultez [Catégories de changements cassants](categories.md).</span><span class="sxs-lookup"><span data-stu-id="992ab-116">For a definition of compatibility categories, such as binary compatibility and backward compatibility, see [Breaking change categories](categories.md).</span></span>

<span data-ttu-id="992ab-117">Les sections suivantes décrivent les catégories de modifications apportées aux API .NET Core et leur impact sur la compatibilité des applications.</span><span class="sxs-lookup"><span data-stu-id="992ab-117">The following sections describes the categories of changes made to .NET Core APIs and their impact on application compatibility.</span></span> <span data-ttu-id="992ab-118">L’icône ✔️ indique qu’un type particulier de modification est autorisé, ❌ indique qu’il n’est pas autorisé et ❓ indique que la modification peut être autorisée ou non autorisée.</span><span class="sxs-lookup"><span data-stu-id="992ab-118">The ✔️ icon indicates that a particular kind of change is allowed, ❌ indicates that it is disallowed, and  ❓ indicates a change that may or may not be allowed.</span></span> <span data-ttu-id="992ab-119">Les modifications de cette dernière catégorie nécessitent un jugement et une évaluation de la prévisibilité et de la cohérence du comportement.</span><span class="sxs-lookup"><span data-stu-id="992ab-119">Changes in this last category require judgment and an evaluation of how predictable, obvious, and consistent the previous behavior was.</span></span>

> [!NOTE]
> <span data-ttu-id="992ab-120">En plus de servir de guide pour l’évaluation des modifications apportées aux bibliothèques .NET Core, les développeurs de bibliothèques peuvent également utiliser ces critères pour évaluer les modifications apportées à leurs bibliothèques qui ciblent plusieurs implémentations et versions de .NET.</span><span class="sxs-lookup"><span data-stu-id="992ab-120">In addition to serving as a guide to how changes to .NET Core libraries are evaluated, library developers can also use these criteria to evaluate changes to their libraries that target multiple .NET implementations and versions.</span></span>

## <a name="modifications-to-the-public-contract"></a><span data-ttu-id="992ab-121">Modifications au contrat public</span><span class="sxs-lookup"><span data-stu-id="992ab-121">Modifications to the public contract</span></span>

<span data-ttu-id="992ab-122">Les modifications de cette catégorie *modifient* la surface d’exposition publique d’un type.</span><span class="sxs-lookup"><span data-stu-id="992ab-122">Changes in this category *modify* the public surface area of a type.</span></span> <span data-ttu-id="992ab-123">La plupart des modifications de cette catégorie ne sont pas autorisées dans la mesure où elles enfreignent une *compatibilité descendante* (la possibilité pour une application qui a été développée avec une version précédente d’une API de s’exécuter sans recompilation sur une version ultérieure).</span><span class="sxs-lookup"><span data-stu-id="992ab-123">Most of the changes in this category are disallowed since they violate *backwards compatibility* (the ability of an application that was developed with a previous version of an API to execute without recompilation on a later version).</span></span>

### <a name="types"></a><span data-ttu-id="992ab-124">Types</span><span class="sxs-lookup"><span data-stu-id="992ab-124">Types</span></span>

- <span data-ttu-id="992ab-125">**✔️ La suppression d’une implémentation d’interface d’un type de l’interface est déjà implémentée par un type de base️**</span><span class="sxs-lookup"><span data-stu-id="992ab-125">**✔️ Removing an interface implementation from a type when the interface is already implemented by a base type**</span></span>

- <span data-ttu-id="992ab-126">**❓ Ajout d’une nouvelle implémentation d’interface à un type**</span><span class="sxs-lookup"><span data-stu-id="992ab-126">**❓ Adding a new interface implementation to a type**</span></span>

  <span data-ttu-id="992ab-127">Il s’agit d’une modification acceptable, car elle n’affectera pas les clients existants.</span><span class="sxs-lookup"><span data-stu-id="992ab-127">This is an acceptable change because it does not adversely affect existing clients.</span></span> <span data-ttu-id="992ab-128">Toutes les modifications au type doivent fonctionner dans les limites des modifications acceptables définies ici pour la nouvelle implémentation afin de rester acceptables.</span><span class="sxs-lookup"><span data-stu-id="992ab-128">Any changes to the type must work within the boundaries of acceptable changes defined here for the new implementation to remain acceptable.</span></span> <span data-ttu-id="992ab-129">Une prudence extrême est nécessaire lors de l’ajout d’interfaces qui affectent directement la capacité d’un concepteur ou sérialiseur à générer du code ou des données qui ne peuvent pas être consommées à un bas niveau.</span><span class="sxs-lookup"><span data-stu-id="992ab-129">Extreme caution is necessary when adding interfaces that directly affect the ability of a designer or serializer to generate code or data that cannot be consumed down-level.</span></span> <span data-ttu-id="992ab-130">Par exemple, l’interface <xref:System.Runtime.Serialization.ISerializable>.</span><span class="sxs-lookup"><span data-stu-id="992ab-130">An example is the <xref:System.Runtime.Serialization.ISerializable> interface.</span></span>

- <span data-ttu-id="992ab-131">**❓ Ajout d’une classe de base**</span><span class="sxs-lookup"><span data-stu-id="992ab-131">**❓ Introducing a new base class**</span></span>

  <span data-ttu-id="992ab-132">Un type peut être introduit dans une hiérarchie entre deux types existants si elle n’introduit pas de nouveaux membres [abstraits](../../csharp/language-reference/keywords/abstract.md) et ne modifie pas la sémantique ou le comportement des types existants.</span><span class="sxs-lookup"><span data-stu-id="992ab-132">A type can be introduced into an hierarchy between two existing types if it doesn't introduce any new [abstract](../../csharp/language-reference/keywords/abstract.md) members or change the semantics or behavior of existing types.</span></span> <span data-ttu-id="992ab-133">Par exemple, dans .NET Framework 2.0, la classe <xref:System.Data.Common.DbConnection> est devenue une classe de base pour <xref:System.Data.SqlClient.SqlConnection>, qui était précédemment dérivée directement de <xref:System.ComponentModel.Component>.</span><span class="sxs-lookup"><span data-stu-id="992ab-133">For example, in .NET Framework 2.0, the <xref:System.Data.Common.DbConnection> class became a new base class for <xref:System.Data.SqlClient.SqlConnection>, which had previously derived directly from <xref:System.ComponentModel.Component>.</span></span>

- <span data-ttu-id="992ab-134">**✔️ Déplacement d’un type d’un assembly à un autre**</span><span class="sxs-lookup"><span data-stu-id="992ab-134">**✔️ Moving a type from one assembly to another**</span></span>

  <span data-ttu-id="992ab-135">Notez que *l’ancien* assembly doit être marqué avec le <xref:System.Runtime.CompilerServices.TypeForwardedToAttribute> qui pointe vers le nouvel assembly.</span><span class="sxs-lookup"><span data-stu-id="992ab-135">Note that the *old* assembly must be marked with the <xref:System.Runtime.CompilerServices.TypeForwardedToAttribute> that points to the new assembly.</span></span>

- <span data-ttu-id="992ab-136">**✔️ Changement d’un type [struct](../../csharp/language-reference/keywords/struct.md) en type `readonly struct`**</span><span class="sxs-lookup"><span data-stu-id="992ab-136">**✔️ Changing a [struct](../../csharp/language-reference/keywords/struct.md) type to a `readonly struct` type**</span></span>

  <span data-ttu-id="992ab-137">Notez que le changement d’un type `readonly struct` en type `struct` n’est pas autorisé.</span><span class="sxs-lookup"><span data-stu-id="992ab-137">Note that changing a `readonly struct` type to a `struct` type is not allowed.</span></span>
  
- <span data-ttu-id="992ab-138">**✔️ L’ajout du mot clé [sealed](../../csharp/language-reference/keywords/sealed.md) ou [abstraite](../../csharp/language-reference/keywords/abstract.md) à un type lorsqu’il y a aucun constructeur *accessible* (public ou protégé)**</span><span class="sxs-lookup"><span data-stu-id="992ab-138">**✔️ Adding the [sealed](../../csharp/language-reference/keywords/sealed.md) or [abstract](../../csharp/language-reference/keywords/abstract.md) keyword to a type when there are no *accessible* (public or protected) constructors**</span></span>

- <span data-ttu-id="992ab-139">**✔️ Extension de la visibilité d’un type**</span><span class="sxs-lookup"><span data-stu-id="992ab-139">**✔️ Expanding the visibility of a type**</span></span>

- <span data-ttu-id="992ab-140">**❌ Changement de l’espace de noms ou du nom d’un type**</span><span class="sxs-lookup"><span data-stu-id="992ab-140">**❌ Changing the namespace or name of a type**</span></span>

- <span data-ttu-id="992ab-141">**❌ Changement du nom ou suppression d’un type public**</span><span class="sxs-lookup"><span data-stu-id="992ab-141">**❌ Renaming or removing a public type**</span></span>

   <span data-ttu-id="992ab-142">Cela empêche tout code qui utilise le type renommé ou supprimé de fonctionner.</span><span class="sxs-lookup"><span data-stu-id="992ab-142">This breaks all code that uses the renamed or removed type.</span></span>

- <span data-ttu-id="992ab-143">**❌ Modification du type sous-jacent d’une énumération**</span><span class="sxs-lookup"><span data-stu-id="992ab-143">**❌ Changing the underlying type of an enumeration**</span></span>

   <span data-ttu-id="992ab-144">Il s’agit d’un changement cassant au niveau de la compilation et du comportement ainsi qu’un changement cassant binaire qui peut rendre les arguments d’attribut non analysables.</span><span class="sxs-lookup"><span data-stu-id="992ab-144">This is a compile-time and behavioral breaking change as well as a binary breaking change that can make attribute arguments unparsable.</span></span>

- <span data-ttu-id="992ab-145">**❌ Scellement d’un type qui était précédemment non scellé**</span><span class="sxs-lookup"><span data-stu-id="992ab-145">**❌ Sealing a type that was previously unsealed**</span></span>

- <span data-ttu-id="992ab-146">**❌ Ajout d’une interface à l’ensemble des types de base d’une interface**</span><span class="sxs-lookup"><span data-stu-id="992ab-146">**❌ Adding an interface to the set of base types of an interface**</span></span>

   <span data-ttu-id="992ab-147">Si une interface implémente une interface qu’elle n’implémentait pas précédemment, tous les types implémentant la version d’origine de l’interface cessent de fonctionner.</span><span class="sxs-lookup"><span data-stu-id="992ab-147">If an interface implements an interface that it previously did not implement, all types that implemented the original version of the interface are broken.</span></span>

- <span data-ttu-id="992ab-148">**❓ Suppression d’une classe d’un ensemble des classes de base ou d’une interface à partir de l’ensemble des interfaces implémentées**</span><span class="sxs-lookup"><span data-stu-id="992ab-148">**❓ Removing a class from the set of base classes or an interface from the set of implemented interfaces**</span></span>

  <span data-ttu-id="992ab-149">Il existe une exception à la règle pour la suppression de l’interface : vous pouvez ajouter l’implémentation d’une interface qui dérive de l’interface supprimée.</span><span class="sxs-lookup"><span data-stu-id="992ab-149">There is one exception to the rule for interface removal: you can add the implementation of an interface that derives from the removed interface.</span></span> <span data-ttu-id="992ab-150">Par exemple, vous pouvez supprimer <xref:System.IDisposable> si le type ou l’interface implémente désormais <xref:System.ComponentModel.IComponent>, qui implémente <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="992ab-150">For example, you can remove <xref:System.IDisposable> if the type or interface now implements <xref:System.ComponentModel.IComponent>, which implements <xref:System.IDisposable>.</span></span>

- <span data-ttu-id="992ab-151">**❌️ Changement d’un type `readonly struct` en type [struct](../../csharp/language-reference/keywords/struct.md)**</span><span class="sxs-lookup"><span data-stu-id="992ab-151">**❌ Changing a `readonly struct` type to a [struct](../../csharp/language-reference/keywords/struct.md) type**</span></span>

  <span data-ttu-id="992ab-152">Notez que le changement d’un type `struct` en type `readonly struct` est autorisé.</span><span class="sxs-lookup"><span data-stu-id="992ab-152">Note that the change of a `struct` type to a `readonly struct` type is allowed.</span></span>

- <span data-ttu-id="992ab-153">**❌ Changement d’un type [struct](../../csharp/language-reference/keywords/struct.md) en type `ref struct` et vice versa**</span><span class="sxs-lookup"><span data-stu-id="992ab-153">**❌ Changing a [struct](../../csharp/language-reference/keywords/struct.md) type to a `ref struct` type, and vice versa**</span></span>

- <span data-ttu-id="992ab-154">**❌ Réduction de la visibilité d’un type**</span><span class="sxs-lookup"><span data-stu-id="992ab-154">**❌ Reducing the visibility of a type**</span></span>

   <span data-ttu-id="992ab-155">Toutefois, l’augmentation de la visibilité d’un type est autorisée.</span><span class="sxs-lookup"><span data-stu-id="992ab-155">However, increasing the visibility of a type is allowed.</span></span>

### <a name="members"></a><span data-ttu-id="992ab-156">Membres</span><span class="sxs-lookup"><span data-stu-id="992ab-156">Members</span></span>

- <span data-ttu-id="992ab-157">**✔️ Développement de la visibilité d’un membre qui n’est pas [virtual](../../csharp/language-reference/keywords/sealed.md)**</span><span class="sxs-lookup"><span data-stu-id="992ab-157">**✔️ Expanding the visibility of a member that is not [virtual](../../csharp/language-reference/keywords/sealed.md)**</span></span>

- <span data-ttu-id="992ab-158">**✔ Ajout d’un membre abstrait à un type public qui n’a pas de constructeur️ *accessible* (public ou protégé) ou dont le type est [sealed](../../csharp/language-reference/keywords/sealed.md)**</span><span class="sxs-lookup"><span data-stu-id="992ab-158">**✔️ Adding an abstract member to a public type that has no *accessible* (public or protected) constructors, or the type is [sealed](../../csharp/language-reference/keywords/sealed.md)**</span></span>

  <span data-ttu-id="992ab-159">Toutefois, l’ajout à un membre abstrait à un type qui a des constructeurs accessibles (publics ou protégés) et n’est pas `sealed` n’est pas autorisé.</span><span class="sxs-lookup"><span data-stu-id="992ab-159">However, adding an abstract member to a type that has accessible (public or protected) constructors and is not `sealed` is not allowed.</span></span>

- <span data-ttu-id="992ab-160">**✔️ Restriction de la visibilité d’un membre [protected](../../csharp/language-reference/keywords/protected.md) chaque fois que le type n’a pas de constructeur accessible (public ou protégé) ou que le type est [sealed](../../csharp/language-reference/keywords/sealed.md)**</span><span class="sxs-lookup"><span data-stu-id="992ab-160">**✔️ Restricting the visibility of a [protected](../../csharp/language-reference/keywords/protected.md) member when the type has no accessible (public or protected) constructors, or the type is [sealed](../../csharp/language-reference/keywords/sealed.md)**</span></span>

- <span data-ttu-id="992ab-161">**✔️ Déplacement d’un membre vers une classe supérieure au type à partir duquel il a été supprimé dans la hiérarchie**</span><span class="sxs-lookup"><span data-stu-id="992ab-161">**✔️ Moving a member into a class higher in the hierarchy than the type from which it was removed**</span></span>

- <span data-ttu-id="992ab-162">**✔️ Ajout ou suppression d’une substitution**</span><span class="sxs-lookup"><span data-stu-id="992ab-162">**✔️ Adding or removing an override**</span></span>

  <span data-ttu-id="992ab-163">Notez qu’en cas d’ajout d’une substitution, les consommateurs précédents pourraient ignorer la substitution lors de l’appel de [base](../../csharp/language-reference/keywords/base.md).</span><span class="sxs-lookup"><span data-stu-id="992ab-163">Note that introducing an override might cause previous consumers to skip over the override when calling [base](../../csharp/language-reference/keywords/base.md).</span></span>

- <span data-ttu-id="992ab-164">**✔ Ajout d’un constructeur à une classe, ainsi que d’un constructeur par défaut (sans paramètre) si la classe n’avait auparavant aucun constructeur️**</span><span class="sxs-lookup"><span data-stu-id="992ab-164">**✔️ Adding a constructor to a class, along with a default (parameterless) constructor if the class previously had no constructors**</span></span>

   <span data-ttu-id="992ab-165">Cependant, ajouter un constructeur à une classe qui n’avait auparavant aucun constructeur *sans* ajouter le constructeur sans paramètre n’est pas autorisé.</span><span class="sxs-lookup"><span data-stu-id="992ab-165">However, adding a constructor to a class that previously had no constructors *without* adding the parameterless constructor is not allowed.</span></span>

- <span data-ttu-id="992ab-166">**✔ Changement d’un membre de️ [abstract](../../csharp/language-reference/keywords/abstract.md) à [virtual](../../csharp/language-reference/keywords/virtual.md)**</span><span class="sxs-lookup"><span data-stu-id="992ab-166">**✔️ Changing a member from [abstract](../../csharp/language-reference/keywords/abstract.md) to [virtual](../../csharp/language-reference/keywords/virtual.md)**</span></span>

- <span data-ttu-id="992ab-167">**✔️ Changement d’une valeur renvoyée `ref readonly` à `ref` (à l’exception des méthodes virtuelles et des interfaces)**</span><span class="sxs-lookup"><span data-stu-id="992ab-167">**✔️ Changing from a `ref readonly` to a `ref` return value (except for virtual methods or interfaces)**</span></span>

- <span data-ttu-id="992ab-168">**✔️ Suppression d’un [readonly](../../csharp/language-reference/keywords/readonly.md) d’un champ, sauf si le type statique du champ est un type valeur mutable**</span><span class="sxs-lookup"><span data-stu-id="992ab-168">**✔️ Removing [readonly](../../csharp/language-reference/keywords/readonly.md) from a field, unless the static type of the field is a mutable value type**</span></span>

- <span data-ttu-id="992ab-169">**✔️ Appel d’un nouvel événement qui n’a pas été défini précédemment**</span><span class="sxs-lookup"><span data-stu-id="992ab-169">**✔️ Calling a new event that wasn't previously defined**</span></span>

- <span data-ttu-id="992ab-170">**❓ Ajout d’un nouveau champ d’instance à un type**</span><span class="sxs-lookup"><span data-stu-id="992ab-170">**❓ Adding a new instance field to a type**</span></span>

   <span data-ttu-id="992ab-171">Cette modification a un impact sur la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="992ab-171">This change impacts serialization.</span></span>

- <span data-ttu-id="992ab-172">**❌ Changement de nom ou suppression d’un paramètre ou membre public**</span><span class="sxs-lookup"><span data-stu-id="992ab-172">**❌ Renaming or removing a public member or parameter**</span></span>

   <span data-ttu-id="992ab-173">Cela empêche tout code qui utilise le membre ou paramètre renommé ou supprimé de fonctionner.</span><span class="sxs-lookup"><span data-stu-id="992ab-173">This breaks all code that uses the renamed or removed member, or parameter.</span></span>

   <span data-ttu-id="992ab-174">Notez que cela inclut la suppression et le changement de nom d’un accesseur Get ou Set à partir d’une propriété, ou de membres d’une énumération.</span><span class="sxs-lookup"><span data-stu-id="992ab-174">Note that this includes removing or renaming a getter or setter from a property, as well as renaming or removing enumeration members.</span></span>

- <span data-ttu-id="992ab-175">**❌ Ajout d’un membre à une interface**</span><span class="sxs-lookup"><span data-stu-id="992ab-175">**❌ Adding a member to an interface**</span></span>

- <span data-ttu-id="992ab-176">**❌ Modification de la valeur d’un membre d’énumération ou d’une constante publique**</span><span class="sxs-lookup"><span data-stu-id="992ab-176">**❌ Changing the value of a public constant or enumeration member**</span></span>

- <span data-ttu-id="992ab-177">**❌ Modification du type d’une propriété, un champ, d’un paramètre ou d’une valeur renvoyée**</span><span class="sxs-lookup"><span data-stu-id="992ab-177">**❌ Changing the type of a property, field, parameter, or return value**</span></span>

- <span data-ttu-id="992ab-178">**❌ Ajout, suppression ou modification de l’ordre des paramètres**</span><span class="sxs-lookup"><span data-stu-id="992ab-178">**❌ Adding, removing, or changing the order of parameters**</span></span>

- <span data-ttu-id="992ab-179">**❌ Ajout ou suppression des mots clés [in](../../csharp/language-reference/keywords/in.md), [out](../../csharp/language-reference/keywords/out.md) ou [ref](../../csharp/language-reference/keywords/ref.md) d’un paramètre**</span><span class="sxs-lookup"><span data-stu-id="992ab-179">**❌ Adding or removing the [in](../../csharp/language-reference/keywords/in.md), [out](../../csharp/language-reference/keywords/out.md) , or [ref](../../csharp/language-reference/keywords/ref.md) keyword from a parameter**</span></span>

- <span data-ttu-id="992ab-180">**❌ Changement de nom d’un paramètre (y compris changement de casse)**</span><span class="sxs-lookup"><span data-stu-id="992ab-180">**❌ Renaming a parameter (including changing its case)**</span></span>

  <span data-ttu-id="992ab-181">Cela est considéré comme un changement cassant pour deux raisons :</span><span class="sxs-lookup"><span data-stu-id="992ab-181">This is considered breaking for two reasons:</span></span>
  
  - <span data-ttu-id="992ab-182">Cela empêche les scénarios à liaison tardive comme la fonctionnalité de liaison tardive de Visual Basic et [dynamic](../../csharp/language-reference/keywords/dynamic.md) de C#.</span><span class="sxs-lookup"><span data-stu-id="992ab-182">It breaks late-bound scenarios such as the late binding feature in Visual Basic and [dynamic](../../csharp/language-reference/keywords/dynamic.md) in C#.</span></span>
  
  - <span data-ttu-id="992ab-183">Cela annule la [compatibilité de la source](categories.md#source-compatibility) lorsque les développeurs utilisent des [arguments nommés](../../csharp/programming-guide/classes-and-structs/named-and-optional-arguments.md#named-arguments).</span><span class="sxs-lookup"><span data-stu-id="992ab-183">It breaks [source compatibility](categories.md#source-compatibility) when developers use [named arguments](../../csharp/programming-guide/classes-and-structs/named-and-optional-arguments.md#named-arguments).</span></span>

- <span data-ttu-id="992ab-184">**❌ Changement d’une valeur renvoyée `ref` à `ref readonly`**</span><span class="sxs-lookup"><span data-stu-id="992ab-184">**❌ Changing from a `ref` return value to a `ref readonly` return value**</span></span>

- <span data-ttu-id="992ab-185">**❌️ Changement d’une valeur renvoyée `ref readonly` à `ref` sur une méthode virtuelle ou une interface**</span><span class="sxs-lookup"><span data-stu-id="992ab-185">**❌️ Changing from a `ref readonly` to a `ref` return value on a virtual method or interface**</span></span>

- <span data-ttu-id="992ab-186">**❌ Ajout ou suppression de [abstract](../../csharp/language-reference/keywords/abstract.md) sur un membre**</span><span class="sxs-lookup"><span data-stu-id="992ab-186">**❌ Adding or removing [abstract](../../csharp/language-reference/keywords/abstract.md) from a member**</span></span>

- <span data-ttu-id="992ab-187">**❌ Suppression du mot clé [virtual](../../csharp/language-reference/keywords/virtual.md) d’un membre**</span><span class="sxs-lookup"><span data-stu-id="992ab-187">**❌ Removing the [virtual](../../csharp/language-reference/keywords/virtual.md) keyword from a member**</span></span>

  <span data-ttu-id="992ab-188">Bien que cela n’est souvent pas un changement cassant, car le compilateur C# a tendance à émettre des instructions [callvirt](<xref:System.Reflection.Emit.OpCodes.Callvirt>) en langage intermédiaire pour appeler des méthodes non virtuelles (`callvirt` effectue une vérification de valeur null, alors qu’un appel normal ne le fait pas), ce comportement n’est pas invariable pour plusieurs raisons :</span><span class="sxs-lookup"><span data-stu-id="992ab-188">While this often is not a breaking change because the C# compiler tends to emit [callvirt](<xref:System.Reflection.Emit.OpCodes.Callvirt>) Intermediate Language (IL) instructions to call non-virtual methods (`callvirt` performs a null check, while a normal call doesn't), this behavior is not invariable for several reasons:</span></span>
  - <span data-ttu-id="992ab-189">C# n’est pas le seul langage que .NET cible.</span><span class="sxs-lookup"><span data-stu-id="992ab-189">C# is not the only language that .NET targets.</span></span>
  
  - <span data-ttu-id="992ab-190">Le compilateur C# essaie progressivement d’optimiser `callvirt` en un appel normal chaque fois que la méthode cible est non virtuelle et n’a probablement pas la valeur null (par exemple une méthode accessible via [l’opérateur de propagation de NULL ?](../../csharp/language-reference/operators/member-access-operators.md#null-conditional-operators--and-)).</span><span class="sxs-lookup"><span data-stu-id="992ab-190">The C# compiler increasingly tries to optimize `callvirt` to a normal call whenever the target method is non-virtual and is probably not null (such as a method accessed through the [?. null propagation operator](../../csharp/language-reference/operators/member-access-operators.md#null-conditional-operators--and-)).</span></span>
  
  <span data-ttu-id="992ab-191">Rendre une méthode virtuelle signifie que le code consommateur finirait souvent par l’appeler de façon non virtuelle.</span><span class="sxs-lookup"><span data-stu-id="992ab-191">Making a method virtual means that the consumer code would often end up calling it non-virtually.</span></span>

- <span data-ttu-id="992ab-192">**❌ Ajout du mot clé [virtual](../../csharp/language-reference/keywords/virtual.md) à un membre**</span><span class="sxs-lookup"><span data-stu-id="992ab-192">**❌ Adding the [virtual](../../csharp/language-reference/keywords/virtual.md) keyword to a member**</span></span>

- <span data-ttu-id="992ab-193">**❌ Changement d’un membre virtuel en abstrait**</span><span class="sxs-lookup"><span data-stu-id="992ab-193">**❌ Making a virtual member abstract**</span></span>

  <span data-ttu-id="992ab-194">Un [membre virtuel](../../csharp/language-reference/keywords/virtual.md) fournit une implémentation de méthode qui *peut être* substituée par une classe dérivée.</span><span class="sxs-lookup"><span data-stu-id="992ab-194">A [virtual member](../../csharp/language-reference/keywords/virtual.md) provides a method implementation that *can be* overridden by a derived class.</span></span> <span data-ttu-id="992ab-195">Un [membre abstrait](../../csharp/language-reference/keywords/abstract.md) ne fournit aucune implémentation et *doit être* substitué.</span><span class="sxs-lookup"><span data-stu-id="992ab-195">An [abstract member](../../csharp/language-reference/keywords/abstract.md) provides no implementation and *must be* overridden.</span></span>

- <span data-ttu-id="992ab-196">**❌ Ajout d’un membre abstrait à un type public qui a des constructeurs (publics ou protégés) accessibles et qui n’est pas [sealed](../../csharp/language-reference/keywords/sealed.md)**</span><span class="sxs-lookup"><span data-stu-id="992ab-196">**❌ Adding an abstract member to a public type that has accessible (public or protected) constructors and that is not [sealed](../../csharp/language-reference/keywords/sealed.md)**</span></span>

- <span data-ttu-id="992ab-197">**❌ Ajout ou suppression du mot clé [static](../../csharp/language-reference/keywords/static.md) d’un membre**</span><span class="sxs-lookup"><span data-stu-id="992ab-197">**❌ Adding or removing the [static](../../csharp/language-reference/keywords/static.md) keyword from a member**</span></span>

- <span data-ttu-id="992ab-198">**❌ Ajout d’une surcharge qui exclut une surcharge existante et définit un comportement différent**</span><span class="sxs-lookup"><span data-stu-id="992ab-198">**❌ Adding an overload that precludes an existing overload and defines a different behavior**</span></span>

  <span data-ttu-id="992ab-199">Cela empêche de fonctionner les clients existants qui étaient liés à la surcharge précédente.</span><span class="sxs-lookup"><span data-stu-id="992ab-199">This breaks existing clients that were bound to the previous overload.</span></span> <span data-ttu-id="992ab-200">Par exemple, si une classe a une seule version d’une méthode qui accepte un <xref:System.UInt32>, un consommateur existant est correctement lié à cette surcharge lors du passage d’une valeur <xref:System.Int32>.</span><span class="sxs-lookup"><span data-stu-id="992ab-200">For example, if a class has a single version of a method that accepts a <xref:System.UInt32>, an existing consumer will successfully bind to that overload when passing a <xref:System.Int32> value.</span></span> <span data-ttu-id="992ab-201">Toutefois, si vous ajoutez une surcharge qui accepte un <xref:System.Int32> lors de la recompilation ou à l’aide d’une liaison tardive, le compilateur est maintenant lié à la nouvelle surcharge.</span><span class="sxs-lookup"><span data-stu-id="992ab-201">However, if you add an overload that accepts an <xref:System.Int32>, when recompiling or using late-binding, the compiler now binds to the new overload.</span></span> <span data-ttu-id="992ab-202">Si un comportement différent se produit, il s’agit d’un changement cassant.</span><span class="sxs-lookup"><span data-stu-id="992ab-202">If different behavior results, this is a breaking change.</span></span>

- <span data-ttu-id="992ab-203">**❌ Ajout d’un constructeur à une classe qui n’avait auparavant pas de constructeur sans ajout du constructeur sans paramètre**</span><span class="sxs-lookup"><span data-stu-id="992ab-203">**❌ Adding a constructor to a class that previously had no constructor without adding the parameterless constructor**</span></span>

- <span data-ttu-id="992ab-204">**❌️ Ajout de [readonly](../../csharp/language-reference/keywords/readonly.md) à un champ**</span><span class="sxs-lookup"><span data-stu-id="992ab-204">**❌️ Adding [readonly](../../csharp/language-reference/keywords/readonly.md) to a field**</span></span>

- <span data-ttu-id="992ab-205">**❌ Réduction de la visibilité d’un membre**</span><span class="sxs-lookup"><span data-stu-id="992ab-205">**❌ Reducing the visibility of a member**</span></span>

   <span data-ttu-id="992ab-206">Cela inclut la réduction de la visibilité d’un membre [protected](../../csharp/language-reference/keywords/protected.md) lorsqu’il y a des constructeurs *accessibles* (publics ou protégés) et que le type n’est *pas* [sealed](../../csharp/language-reference/keywords/sealed.md).</span><span class="sxs-lookup"><span data-stu-id="992ab-206">This includes reducing the visibility of a [protected](../../csharp/language-reference/keywords/protected.md) member when there are *accessible* (public or protected) constructors and the type is *not* [sealed](../../csharp/language-reference/keywords/sealed.md).</span></span> <span data-ttu-id="992ab-207">Si ce n’est pas le cas, la réduction de la visibilité d’un membre protégé est autorisée.</span><span class="sxs-lookup"><span data-stu-id="992ab-207">If this is not the case, reducing the visibility of a protected member is allowed.</span></span>

   <span data-ttu-id="992ab-208">Notez que l’augmentation de la visibilité d’un membre est autorisée.</span><span class="sxs-lookup"><span data-stu-id="992ab-208">Note that increasing the visibility of a member is allowed.</span></span>

- <span data-ttu-id="992ab-209">**❌ Modification du type d’un membre**</span><span class="sxs-lookup"><span data-stu-id="992ab-209">**❌ Changing the type of a member**</span></span>

   <span data-ttu-id="992ab-210">La valeur renvoyée d’une méthode ou le type d’une propriété ou d’un champ ne peut pas être modifiée.</span><span class="sxs-lookup"><span data-stu-id="992ab-210">The return value of a method or the type of a property or field cannot be modified.</span></span> <span data-ttu-id="992ab-211">Par exemple, la signature d’une méthode qui retourne un <xref:System.Object> ne peut pas être modifiée afin de retourner un <xref:System.String>, ou vice versa.</span><span class="sxs-lookup"><span data-stu-id="992ab-211">For example, the signature of a method that returns an <xref:System.Object> cannot be changed to return a <xref:System.String>, or vice versa.</span></span>

- <span data-ttu-id="992ab-212">**❌ Ajout d’un champ à une un struct qui n’avait précédemment aucun état**</span><span class="sxs-lookup"><span data-stu-id="992ab-212">**❌ Adding a field to a struct that previously had no state**</span></span>

  <span data-ttu-id="992ab-213">Les règles d’affectation autorisent l’utilisation de variables non initialisées tant que le type de variable est un struct sans état.</span><span class="sxs-lookup"><span data-stu-id="992ab-213">Definite assignment rules allow the use of uninitialized variables so long as the variable type is a stateless struct.</span></span> <span data-ttu-id="992ab-214">Si le struct est avec état, le code pourrait se retrouver avec des données non initialisées.</span><span class="sxs-lookup"><span data-stu-id="992ab-214">If the struct is made stateful, code could end up with uninitialized data.</span></span> <span data-ttu-id="992ab-215">Il s’agit potentiellement d’un changement cassant binaire et d’un changement cassant source.</span><span class="sxs-lookup"><span data-stu-id="992ab-215">This is both potentially a source breaking and a binary breaking change.</span></span>

- <span data-ttu-id="992ab-216">**❌ Déclenchement d’un événement existant alors qu’il n’a jamais été déclenché avant**</span><span class="sxs-lookup"><span data-stu-id="992ab-216">**❌ Firing an existing event when it was never fired before**</span></span>

## <a name="behavioral-changes"></a><span data-ttu-id="992ab-217">Changements de comportement</span><span class="sxs-lookup"><span data-stu-id="992ab-217">Behavioral changes</span></span>

### <a name="assemblies"></a><span data-ttu-id="992ab-218">Assemblys</span><span class="sxs-lookup"><span data-stu-id="992ab-218">Assemblies</span></span>

- <span data-ttu-id="992ab-219">**✔️ Rendre un assembly portable lorsque les mêmes plateformes sont toujours prises en charge**</span><span class="sxs-lookup"><span data-stu-id="992ab-219">**✔️ Making an assembly portable when the same platforms are still supported**</span></span>

- <span data-ttu-id="992ab-220">**❌ Modification du nom d’un assembly**</span><span class="sxs-lookup"><span data-stu-id="992ab-220">**❌ Changing the name of an assembly**</span></span>
- <span data-ttu-id="992ab-221">**❌ Modification de la clé publique d’un assembly**</span><span class="sxs-lookup"><span data-stu-id="992ab-221">**❌ Changing the public key of an assembly**</span></span>

### <a name="properties-fields-parameters-and-return-values"></a><span data-ttu-id="992ab-222">Propriétés, champs, paramètres et valeurs renvoyées</span><span class="sxs-lookup"><span data-stu-id="992ab-222">Properties, fields, parameters, and return values</span></span>

- <span data-ttu-id="992ab-223">**✔️ Modification de la valeur d’une propriété, d’un champ, d’une valeur renvoyée, ou d’un paramètre [out](../../csharp/language-reference/keywords/out-parameter-modifier.md) en un type plus dérivé**</span><span class="sxs-lookup"><span data-stu-id="992ab-223">**✔️ Changing the value of a property, field, return value, or [out](../../csharp/language-reference/keywords/out-parameter-modifier.md) parameter to a more derived type**</span></span>

  <span data-ttu-id="992ab-224">Par exemple, une méthode qui retourne un type <xref:System.Object> peut retourner une instance de <xref:System.String>.</span><span class="sxs-lookup"><span data-stu-id="992ab-224">For example, a method that returns a type of <xref:System.Object> can return a <xref:System.String> instance.</span></span> <span data-ttu-id="992ab-225">(Toutefois, la signature de méthode ne peut pas être modifiée).</span><span class="sxs-lookup"><span data-stu-id="992ab-225">(However, the method signature cannot change.)</span></span>

- <span data-ttu-id="992ab-226">**✔️ Augmentation de l’intervalle accepté de valeurs pour une propriété ou un paramètre si le membre n’est pas [virtual](../../csharp/language-reference/keywords/virtual.md)**</span><span class="sxs-lookup"><span data-stu-id="992ab-226">**✔️ Increasing the range of accepted values for a property or parameter if the member is not [virtual](../../csharp/language-reference/keywords/virtual.md)**</span></span>

  <span data-ttu-id="992ab-227">Notez que bien que la plage de valeurs qui peuvent être passées à la méthode ou sont retournées par le membre peut être étendue, le type de membre ou paramètre ne peut pas l’être.</span><span class="sxs-lookup"><span data-stu-id="992ab-227">Note that while the range of values that can be passed to the method or are returned by the member can expand, the parameter or member type cannot.</span></span> <span data-ttu-id="992ab-228">Par exemple, tandis que les valeurs passées à une méthode peuvent s’étendre de 0-124 à 0-255, le type de paramètre ne peut pas être changé de <xref:System.Byte> à <xref:System.Int32>.</span><span class="sxs-lookup"><span data-stu-id="992ab-228">For example, while the values passed to a method can expand from 0-124 to 0-255, the parameter type cannot change from <xref:System.Byte> to <xref:System.Int32>.</span></span>

- <span data-ttu-id="992ab-229">**❌ Augmentation de l’intervalle accepté de valeurs pour une propriété ou un paramètre si le membre est [virtual](../../csharp/language-reference/keywords/virtual.md)**</span><span class="sxs-lookup"><span data-stu-id="992ab-229">**❌ Increasing the range of accepted values for a property or parameter if the member is [virtual](../../csharp/language-reference/keywords/virtual.md)**</span></span>

   <span data-ttu-id="992ab-230">Ce changement empêche le fonctionnement des membres substitués existants, qui ne fonctionneront pas correctement pour la plage de valeurs étendue.</span><span class="sxs-lookup"><span data-stu-id="992ab-230">This change breaks existing overridden members, which will not function correctly for the extended range of values.</span></span>

- <span data-ttu-id="992ab-231">**❌ Diminution de la plage des valeurs acceptées pour une propriété ou un paramètre**</span><span class="sxs-lookup"><span data-stu-id="992ab-231">**❌ Decreasing the range of accepted values for a property or parameter**</span></span>

- <span data-ttu-id="992ab-232">**❌ Augmentation de l’intervalle de valeurs renvoyées pour une propriété, un champ, une valeur de retour ou un paramètre [out](../../csharp/language-reference/keywords/out-parameter-modifier.md)**</span><span class="sxs-lookup"><span data-stu-id="992ab-232">**❌ Increasing the range of returned values for a property, field, return value, or [out](../../csharp/language-reference/keywords/out-parameter-modifier.md) parameter**</span></span>

- <span data-ttu-id="992ab-233">**❌ Modification des valeurs renvoyées pour une propriété, un champ, une valeur de retour de méthode ou un paramètre [out](../../csharp/language-reference/keywords/out-parameter-modifier.md)**</span><span class="sxs-lookup"><span data-stu-id="992ab-233">**❌ Changing the returned values for a property, field, method return value, or [out](../../csharp/language-reference/keywords/out-parameter-modifier.md) parameter**</span></span>

- <span data-ttu-id="992ab-234">**❌ Modification de la valeur par défaut d’une propriété, d’un champ ou d’un paramètre**</span><span class="sxs-lookup"><span data-stu-id="992ab-234">**❌ Changing the default value of a property, field, or parameter**</span></span>

- <span data-ttu-id="992ab-235">**Modification de la précision d’une valeur de retournée numérique de ❌**</span><span class="sxs-lookup"><span data-stu-id="992ab-235">**❌ Changing the precision of a numeric return value**</span></span>

- <span data-ttu-id="992ab-236">**❓ Changement dans l’analyse de l’entrée et levée de nouvelles exceptions (même si le comportement d’analyse n’est pas spécifié dans la documentation)**</span><span class="sxs-lookup"><span data-stu-id="992ab-236">**❓ A change in the parsing of input and throwing new exceptions (even if parsing behavior is not specified in the documentation**</span></span>

### <a name="exceptions"></a><span data-ttu-id="992ab-237">Exceptions</span><span class="sxs-lookup"><span data-stu-id="992ab-237">Exceptions</span></span>

- <span data-ttu-id="992ab-238">**✔️ Levée d’une exception plus dérivée qu’une exception existante**</span><span class="sxs-lookup"><span data-stu-id="992ab-238">**✔️ Throwing a more derived exception than an existing exception**</span></span>

  <span data-ttu-id="992ab-239">Étant donné que la nouvelle exception est une sous-classe d’une exception existante, le code de gestion des exceptions précédent continue à gérer l’exception.</span><span class="sxs-lookup"><span data-stu-id="992ab-239">Because the new exception is a subclass of an existing exception, previous exception handling code continues to handle the exception.</span></span> <span data-ttu-id="992ab-240">Par exemple, dans .NET Framework 4, les méthodes de création et de récupération de culture ont commencé à lever un <xref:System.Globalization.CultureNotFoundException> au lieu d’un <xref:System.ArgumentException> si la culture est introuvable.</span><span class="sxs-lookup"><span data-stu-id="992ab-240">For example, in .NET Framework 4, culture creation and retrieval methods began to throw a <xref:System.Globalization.CultureNotFoundException> instead of an <xref:System.ArgumentException> if the culture could not be found.</span></span> <span data-ttu-id="992ab-241">Étant donné que <xref:System.Globalization.CultureNotFoundException> dérive de <xref:System.ArgumentException>, il s’agit d’une modification acceptable.</span><span class="sxs-lookup"><span data-stu-id="992ab-241">Because <xref:System.Globalization.CultureNotFoundException> derives from <xref:System.ArgumentException>, this is an acceptable change.</span></span>

- <span data-ttu-id="992ab-242">**✔️ Levée d’une exception plus spécifique que <xref:System.NotSupportedException>, <xref:System.NotImplementedException>, <xref:System.NullReferenceException>**</span><span class="sxs-lookup"><span data-stu-id="992ab-242">**✔️ Throwing a more specific exception than <xref:System.NotSupportedException>, <xref:System.NotImplementedException>, <xref:System.NullReferenceException>**</span></span>

- <span data-ttu-id="992ab-243">**✔️ Levée d’une exception qui est considérée comme irrécupérable**</span><span class="sxs-lookup"><span data-stu-id="992ab-243">**✔️ Throwing an exception that is considered unrecoverable**</span></span>

  <span data-ttu-id="992ab-244">Les exceptions irrécupérables ne doivent pas être interceptées, mais plutôt être gérées par un gestionnaire fourre-tout de haut niveau.</span><span class="sxs-lookup"><span data-stu-id="992ab-244">Unrecoverable exceptions should not be caught but instead should be handled by a high-level catch-all handler.</span></span> <span data-ttu-id="992ab-245">Par conséquent, les utilisateurs ne sont pas censés avoir du code qui intercepte les exceptions explicites.</span><span class="sxs-lookup"><span data-stu-id="992ab-245">Therefore, users are not expected to have code that catches these explicit exceptions.</span></span> <span data-ttu-id="992ab-246">Les exceptions irrécupérables sont :</span><span class="sxs-lookup"><span data-stu-id="992ab-246">The unrecoverable exceptions are:</span></span>

  - <xref:System.AccessViolationException>
  - <xref:System.ExecutionEngineException>
  - <xref:System.Runtime.InteropServices.SEHException>
  - <xref:System.StackOverflowException>

- <span data-ttu-id="992ab-247">**✔️ Levée une exception dans un nouveau chemin de code**</span><span class="sxs-lookup"><span data-stu-id="992ab-247">**✔️ Throwing a new exception in a new code path**</span></span>

  <span data-ttu-id="992ab-248">L’exception doit s’appliquer uniquement à un nouveau chemin de code qui est exécuté avec les nouvelles valeurs de paramètre ou le nouvel état, et qui ne peut pas être exécuté par le code existant qui cible la version précédente.</span><span class="sxs-lookup"><span data-stu-id="992ab-248">The exception must apply only to a new code-path which is executed with new parameter values or state, and that can't be executed by existing code that targets the previous version.</span></span>

- <span data-ttu-id="992ab-249">**✔ Suppression d’une exception pour permettre un comportement plus robuste ou de nouveaux scénarios️**</span><span class="sxs-lookup"><span data-stu-id="992ab-249">**✔️ Removing an exception to enable more robust behavior or new scenarios**</span></span>

  <span data-ttu-id="992ab-250">Par exemple, une méthode `Divide` qui pouvait auparavant uniquement gérer des valeurs positives et levait un <xref:System.ArgumentOutOfRangeException> peut être modifiée pour prendre en charge les valeurs positives et négatives sans lever d’exception.</span><span class="sxs-lookup"><span data-stu-id="992ab-250">For example, a `Divide` method that previously only handled positive values and threw an <xref:System.ArgumentOutOfRangeException> otherwise can be changed to support both negative and positive values without throwing an exception.</span></span>

- <span data-ttu-id="992ab-251">**✔️ Modification du texte d’un message d’erreur**</span><span class="sxs-lookup"><span data-stu-id="992ab-251">**✔️ Changing the text of an error message**</span></span>

  <span data-ttu-id="992ab-252">Les développeurs ne doivent pas compter sur le texte des messages d’erreur, qui changent également selon la culture de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="992ab-252">Developers should not rely on the text of error messages, which also change based on the user's culture.</span></span>

- <span data-ttu-id="992ab-253">**❌ Levée d’une exception dans tous les autres cas non répertoriés ci-dessus**</span><span class="sxs-lookup"><span data-stu-id="992ab-253">**❌ Throwing an exception in any other case not listed above**</span></span>

- <span data-ttu-id="992ab-254">**❌ Suppression d’une exception dans tous les autres cas non répertoriés ci-dessus**</span><span class="sxs-lookup"><span data-stu-id="992ab-254">**❌ Removing an exception in any other case not listed above**</span></span>

### <a name="attributes"></a><span data-ttu-id="992ab-255">Attributs</span><span class="sxs-lookup"><span data-stu-id="992ab-255">Attributes</span></span>

- <span data-ttu-id="992ab-256">**✔️ Modification de la valeur d’un attribut qui n’est *pas* observable**</span><span class="sxs-lookup"><span data-stu-id="992ab-256">**✔️ Changing the value of an attribute that is *not* observable**</span></span>

- <span data-ttu-id="992ab-257">**❌ Modification de la valeur d’un attribut qui *est* observable**</span><span class="sxs-lookup"><span data-stu-id="992ab-257">**❌ Changing the value of an attribute that *is* observable**</span></span>

- <span data-ttu-id="992ab-258">**❓ Suppression d’un attribut**</span><span class="sxs-lookup"><span data-stu-id="992ab-258">**❓ Removing an attribute**</span></span>

  <span data-ttu-id="992ab-259">Dans la plupart des cas, la suppression d’un attribut (comme <xref:System.NonSerializedAttribute>) est un changement cassant.</span><span class="sxs-lookup"><span data-stu-id="992ab-259">In most cases, removing an attribute (such as <xref:System.NonSerializedAttribute>) is a breaking change.</span></span>

## <a name="platform-support"></a><span data-ttu-id="992ab-260">Plateforme prise en charge</span><span class="sxs-lookup"><span data-stu-id="992ab-260">Platform support</span></span>

- <span data-ttu-id="992ab-261">**✔️ Prise en charge d’une opération sur une plateforme qui n’était précédemment pas prise en charge**</span><span class="sxs-lookup"><span data-stu-id="992ab-261">**✔️ Supporting an operation on a platform that was previously not supported**</span></span>

- <span data-ttu-id="992ab-262">**❌ Cesser la prise en charge ou commencer à requérir un service pack spécifique pour une opération qui était précédemment prise en charge sur une plateforme**</span><span class="sxs-lookup"><span data-stu-id="992ab-262">**❌ Not supporting or now requiring a specific service pack for an operation that was previously supported on a platform**</span></span>

## <a name="internal-implementation-changes"></a><span data-ttu-id="992ab-263">Modifications de l’implémentation interne</span><span class="sxs-lookup"><span data-stu-id="992ab-263">Internal implementation changes</span></span>

- <span data-ttu-id="992ab-264">**❓ Modification de la surface d’exposition d’un type interne**</span><span class="sxs-lookup"><span data-stu-id="992ab-264">**❓ Changing the surface area of an internal type**</span></span>

   <span data-ttu-id="992ab-265">Ces modifications sont généralement autorisées, même si elles rompent la réflexion privée.</span><span class="sxs-lookup"><span data-stu-id="992ab-265">Such changes are generally allowed, although they break private reflection.</span></span> <span data-ttu-id="992ab-266">Dans certains cas, où les bibliothèques tierces les plus courantes ou un grand nombre de développeurs dépendent d’API internes, ces modifications ne peuvent pas être autorisées.</span><span class="sxs-lookup"><span data-stu-id="992ab-266">In some cases, where popular third-party libraries or a large number of developers depend on the internal APIs, such changes may not be allowed.</span></span>

- <span data-ttu-id="992ab-267">**❓ Modification de l’implémentation interne d’un membre**</span><span class="sxs-lookup"><span data-stu-id="992ab-267">**❓ Changing the internal implementation of a member**</span></span>

  <span data-ttu-id="992ab-268">Ces modifications sont généralement autorisées, même si elles rompent la réflexion privée.</span><span class="sxs-lookup"><span data-stu-id="992ab-268">These changes are generally allowed, although they break private reflection.</span></span> <span data-ttu-id="992ab-269">Dans certains cas où le code du client dépend souvent de la réflexion privée ou où la modification introduit des effets secondaires involontaires, ces modifications peuvent ne pas être autorisées.</span><span class="sxs-lookup"><span data-stu-id="992ab-269">In some cases, where customer code frequently depends on private reflection or where the change introduces unintended side effects, these changes may not be allowed.</span></span>

- <span data-ttu-id="992ab-270">**✔ Amélioration des performances d’une opération**</span><span class="sxs-lookup"><span data-stu-id="992ab-270">**✔️ Improving the performance of an operation**</span></span>

   <span data-ttu-id="992ab-271">La possibilité de modifier les performances d’une opération est essentielle, mais ces modifications peuvent endommager le code repose sur la vitesse actuelle d’une opération.</span><span class="sxs-lookup"><span data-stu-id="992ab-271">The ability to modify the performance of an operation is essential, but such changes can break code that relies upon the current speed of an operation.</span></span> <span data-ttu-id="992ab-272">Cela est particulièrement vrai pour du code qui dépend de la chronologie des opérations asynchrones.</span><span class="sxs-lookup"><span data-stu-id="992ab-272">This is particularly true of code that depends on the timing of asynchronous operations.</span></span> <span data-ttu-id="992ab-273">Notez que la modification de performances ne doit avoir aucun effet sur les autres comportements de l’API en question ; sinon, la modification sera un changement cassant.</span><span class="sxs-lookup"><span data-stu-id="992ab-273">Note that the performance change should have no effect on other behavior of the API in question; otherwise, the change will be breaking.</span></span>

- <span data-ttu-id="992ab-274">**✔️ Changement indirect (et souvent avec un impact négatif) des performances d’une opération**</span><span class="sxs-lookup"><span data-stu-id="992ab-274">**✔️ Indirectly (and often adversely) changing the performance of an operation**</span></span>

  <span data-ttu-id="992ab-275">Si la modification en question n’est pas considérée comme un changement cassant pour une raison quelconque, cela est acceptable.</span><span class="sxs-lookup"><span data-stu-id="992ab-275">If the change in question is not categorized as breaking for some other reason, this is acceptable.</span></span> <span data-ttu-id="992ab-276">Souvent, des mesures doivent être prises, ce qui peut inclure des opérations supplémentaires ou l’ajout de nouvelles fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="992ab-276">Often, actions need to be taken that may include extra operations or that add new functionality.</span></span> <span data-ttu-id="992ab-277">Cela affectera presque toujours les performances, mais peut s’avérer essentiel pour que l’API en question fonctionne comme prévu.</span><span class="sxs-lookup"><span data-stu-id="992ab-277">This will almost always affect performance but may be essential to make the API in question function as expected.</span></span>

- <span data-ttu-id="992ab-278">**❌ Modification d’une API synchrone en asynchrone (et vice versa)**</span><span class="sxs-lookup"><span data-stu-id="992ab-278">**❌ Changing a synchronous API to asynchronous (and vice versa)**</span></span>

## <a name="code-changes"></a><span data-ttu-id="992ab-279">Modifications du code</span><span class="sxs-lookup"><span data-stu-id="992ab-279">Code changes</span></span>

- <span data-ttu-id="992ab-280">**✔️ Ajout de [params](../../csharp/language-reference/keywords/params.md) à un paramètre**</span><span class="sxs-lookup"><span data-stu-id="992ab-280">**✔️ Adding [params](../../csharp/language-reference/keywords/params.md) to a parameter**</span></span>

- <span data-ttu-id="992ab-281">**❌ Changement d’un [struct](../../csharp/language-reference/keywords/struct.md) en [classe](../../csharp/language-reference/keywords/class.md) et vice versa**</span><span class="sxs-lookup"><span data-stu-id="992ab-281">**❌ Changing a [struct](../../csharp/language-reference/keywords/struct.md) to a [class](../../csharp/language-reference/keywords/class.md) and vice versa**</span></span>

- <span data-ttu-id="992ab-282">**❌ Ajout du mot clé [checked](../../csharp/language-reference/keywords/virtual.md) à un bloc de code**</span><span class="sxs-lookup"><span data-stu-id="992ab-282">**❌ Adding the [checked](../../csharp/language-reference/keywords/virtual.md) keyword to a code block**</span></span>

   <span data-ttu-id="992ab-283">Cette modification peut pousser le code précédemment exécuté à lever <xref:System.OverflowException>, ce qui n’est pas acceptable.</span><span class="sxs-lookup"><span data-stu-id="992ab-283">This change may cause code that previously executed to throw an <xref:System.OverflowException> and is unacceptable.</span></span>

- <span data-ttu-id="992ab-284">**❌ Suppression de [params](../../csharp/language-reference/keywords/params.md) à partir d’un paramètre**</span><span class="sxs-lookup"><span data-stu-id="992ab-284">**❌ Removing [params](../../csharp/language-reference/keywords/params.md) from a parameter**</span></span>

- <span data-ttu-id="992ab-285">**❌ Modification de l’ordre dans lequel les événements sont déclenchés**</span><span class="sxs-lookup"><span data-stu-id="992ab-285">**❌ Changing the order in which events are fired**</span></span>

  <span data-ttu-id="992ab-286">Les développeurs peuvent raisonnablement s’attendre à ce que les événements se déclenchent dans le même ordre, et le code développeur dépend souvent de l’ordre dans lequel les événements sont déclenchés.</span><span class="sxs-lookup"><span data-stu-id="992ab-286">Developers can reasonably expect events to fire in the same order, and developer code frequently depends on the order in which events are fired.</span></span>

- <span data-ttu-id="992ab-287">**❌ Suppression du déclenchement d’un événement sur une action donnée**</span><span class="sxs-lookup"><span data-stu-id="992ab-287">**❌ Removing the raising of an event on a given action**</span></span>

- <span data-ttu-id="992ab-288">**❌ Modification du nombre de fois que des événements donnés sont appelés**</span><span class="sxs-lookup"><span data-stu-id="992ab-288">**❌ Changing the number of times given events are called**</span></span>

- <span data-ttu-id="992ab-289">**❌ Ajout de <xref:System.FlagsAttribute> à un type d’énumération**</span><span class="sxs-lookup"><span data-stu-id="992ab-289">**❌ Adding the <xref:System.FlagsAttribute> to an enumeration type**</span></span>