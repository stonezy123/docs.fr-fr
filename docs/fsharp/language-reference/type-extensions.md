---
title: Extensions de type
description: 'Découvrez comment les extensions de type F # vous permettent d’ajouter de nouveaux membres à un type d’objet précédemment défini.'
ms.date: 02/05/2020
ms.openlocfilehash: 8fdb2d5e527643b23d24a6118e8cef6b11f1a546
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559126"
---
# <a name="type-extensions"></a>Extensions de type

Les extensions de type (également appelées « _augmentations_») sont une famille de fonctionnalités qui vous permettent d’ajouter de nouveaux membres à un type d’objet précédemment défini. Les trois fonctionnalités sont les suivantes :

- Extensions de type intrinsèques
- Extensions de type facultatives
- Méthodes d’extension

Chaque peut être utilisé dans différents scénarios et présente des compromis différents.

## <a name="syntax"></a>Syntaxe

```fsharp
// Intrinsic and optional extensions
type typename with
    member self-identifier.member-name =
        body
    ...

// Extension methods
open System.Runtime.CompilerServices

[<Extension>]
type Extensions() =
    [<Extension>]
    static member self-identifier.extension-name (ty: typename, [args]) =
        body
    ...
```

## <a name="intrinsic-type-extensions"></a>Extensions de type intrinsèques

Une extension de type intrinsèque est une extension de type qui étend un type défini par l’utilisateur.

Les extensions de type intrinsèques doivent être définies dans le même fichier **et** dans le même espace de noms ou module que le type qu’elles étendent. Toutes les autres définitions ont pour conséquence des [extensions de type facultatives](type-extensions.md#optional-type-extensions).

Les extensions de type intrinsèques sont parfois un moyen plus clair de séparer les fonctionnalités de la déclaration de type. L’exemple suivant montre comment définir une extension de type intrinsèque :

```fsharp
namespace Example

type Variant =
    | Num of int
    | Str of string
  
module Variant =
    let print v =
        match v with
        | Num n -> printf "Num %d" n
        | Str s -> printf "Str %s" s

// Add a member to Variant as an extension
type Variant with
    member x.Print() = Variant.print x
```

L’utilisation d’une extension de type vous permet de séparer chacun des éléments suivants :

- Déclaration d’un `Variant` type
- Fonctionnalité pour imprimer la `Variant` classe en fonction de sa « forme »
- Un moyen d’accéder à la fonctionnalité d’impression avec la notation de style objet `.`

Il s’agit d’une alternative à la définition de tout en tant que membre sur `Variant` . Bien qu’il ne s’agisse pas d’une approche fondamentalement meilleure, il peut s’agir d’une représentation plus claire des fonctionnalités dans certaines situations.

Les extensions de type intrinsèque sont compilées en tant que membres du type qu’elles augmentent et apparaissent sur le type quand le type est examiné par réflexion.

## <a name="optional-type-extensions"></a>Extensions de type facultatives

Une extension de type facultative est une extension qui apparaît à l’extérieur du module, de l’espace de noms ou de l’assembly d’origine du type en cours d’extension.

Les extensions de type facultatives sont utiles pour étendre un type que vous n’avez pas défini vous-même. Par exemple :

```fsharp
module Extensions

type IEnumerable<'T> with
    /// Repeat each element of the sequence n times
    member xs.RepeatElements(n: int) =
        seq {
            for x in xs do
                for _ in 1 .. n -> x
        }
```

Vous pouvez maintenant accéder `RepeatElements` comme s’il s’agissait d’un membre de <xref:System.Collections.Generic.IEnumerable%601> tant que le `Extensions` module est ouvert dans l’étendue dans laquelle vous travaillez.

Les extensions facultatives n’apparaissent pas sur le type étendu lorsqu’elles sont examinées par réflexion. Les extensions facultatives doivent se trouver dans des modules, et elles sont uniquement dans la portée lorsque le module qui contient l’extension est ouvert ou est dans la portée.

Les membres d’extension facultatifs sont compilés en membres statiques pour lesquels l’instance de l’objet est passée implicitement en tant que premier paramètre. Toutefois, ils agissent comme s’il s’agissait de membres d’instance ou de membres statiques en fonction de la façon dont ils sont déclarés.

Les membres d’extension facultatifs ne sont pas non plus visibles pour les consommateurs C# ou Visual Basic. Ils peuvent uniquement être consommés dans un autre code F #.

## <a name="generic-limitation-of-intrinsic-and-optional-type-extensions"></a>Limitation générique des extensions de type intrinsèques et facultatives

Il est possible de déclarer une extension de type sur un type générique où la variable de type est contractionnelle. La spécification est que la contrainte de la déclaration d’extension correspond à la contrainte du type déclaré.

Toutefois, même lorsque les contraintes sont mises en correspondance entre un type déclaré et une extension de type, il est possible qu’une contrainte soit déduite par le corps d’un membre étendu qui impose une exigence différente sur le paramètre de type que le type déclaré. Par exemple :

```fsharp
open System.Collections.Generic

// NOT POSSIBLE AND FAILS TO COMPILE!
//
// The member 'Sum' has a different requirement on 'T than the type IEnumerable<'T>
type IEnumerable<'T> with
    member this.Sum() = Seq.sum this
```

Il n’existe aucun moyen de faire en sorte que ce code fonctionne avec une extension de type facultative :

- Comme c’est le cas, le `Sum` membre a une contrainte différente sur `'T` ( `static member get_Zero` et `static member (+)` ) que celle définie par l’extension de type.
- La modification de l’extension de type pour avoir la même contrainte que `Sum` ne correspondra plus à la contrainte définie sur `IEnumerable<'T>` .
- La modification `member this.Sum` de en `member inline this.Sum` génère une erreur indiquant que les contraintes de type ne correspondent pas.

Les méthodes statiques qui sont souhaitées sont des méthodes statiques qui « flottent dans l’espace » et peuvent être présentées comme si elles étendaient un type. C’est là que les méthodes d’extension deviennent nécessaires.

## <a name="extension-methods"></a>Méthodes d’extension

Enfin, les méthodes d’extension (parfois appelées « membres d’extension de style C# ») peuvent être déclarées en F # en tant que méthode de membre statique sur une classe.

Les méthodes d’extension sont utiles lorsque vous souhaitez définir des extensions sur un type générique qui contraignent la variable de type. Par exemple :

```fsharp
namespace Extensions

open System.Collections.Generic
open System.Runtime.CompilerServices

[<Extension>]
type IEnumerableExtensions =
    [<Extension>]
    static member inline Sum(xs: IEnumerable<'T>) = Seq.sum xs
```

Lorsqu’il est utilisé, ce code s’affiche comme si `Sum` est défini sur <xref:System.Collections.Generic.IEnumerable%601> , tant que `Extensions` a été ouvert ou est dans l’étendue.

Pour que l’extension soit disponible pour le code VB.NET, une valeur supplémentaire `ExtensionAttribute` est requise au niveau de l’assembly :

```fsharp
module AssemblyInfo
open System.Runtime.CompilerServices
[<assembly:Extension>]
do ()
```

## <a name="other-remarks"></a>Autres remarques

Les extensions de type ont également les attributs suivants :

- Tout type accessible peut être étendu.
- Les extensions de type intrinsèques et facultatives peuvent définir _n’importe quel type de_ membre, pas seulement des méthodes. Ainsi, les propriétés d’extension sont également possibles, par exemple.
- Le `self-identifier` jeton dans la [syntaxe](type-extensions.md#syntax) représente l’instance du type appelé, tout comme les membres ordinaires.
- Les membres étendus peuvent être des membres statiques ou d’instance.
- Les variables de type sur une extension de type doivent correspondre aux contraintes du type déclaré.

Les limitations suivantes existent également pour les extensions de type :

- Les extensions de type ne prennent pas en charge les méthodes virtuelles ou abstraites.
- Les extensions de type ne prennent pas en charge les méthodes override comme augmentations.
- Les extensions de type ne prennent pas en charge les [paramètres de type résolus statiquement](./generics/statically-resolved-type-parameters.md).
- Les extensions de type facultatives ne prennent pas en charge les constructeurs comme augmentations.
- Les extensions de type ne peuvent pas être définies sur les [abréviations de type](type-abbreviations.md).
- Les extensions de type ne sont pas valides pour `byref<'T>` (bien qu’elles puissent être déclarées).
- Les extensions de type ne sont pas valides pour les attributs (bien qu’elles puissent être déclarées).
- Vous pouvez définir des extensions qui surchargent d’autres méthodes du même nom, mais le compilateur F # donne la préférence aux méthodes qui ne sont pas des extensions si un appel est ambigu.

Enfin, s’il existe plusieurs extensions de type intrinsèques pour un type, tous les membres doivent être uniques. Pour les extensions de type facultatives, les membres de différentes extensions de type du même type peuvent avoir le même nom. Les erreurs d’ambiguïté se produisent uniquement si le code client ouvre deux portées différentes qui définissent les mêmes noms de membres.

## <a name="see-also"></a>Voir aussi

- [Informations de référence sur le langage F #](index.md)
- [Members](./members/index.md) (Membres)
