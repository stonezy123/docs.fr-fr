---
title: Indications relatives à l'utilisation
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- class library design guidelines [.NET Framework], usage guidelines
ms.assetid: 42215ffa-a099-4a26-b14e-fb2bdb6f95b7
ms.openlocfilehash: 03eaba3e52cb25619f65637efb4f414c22770440
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291342"
---
# <a name="usage-guidelines"></a>Indications relatives à l'utilisation

Cette section contient des instructions pour l’utilisation des types courants dans les API accessibles publiquement. Il traite de l’utilisation directe des types d’infrastructure intégrés (par exemple, les attributs de sérialisation) et de la surcharge des opérateurs courants.
  
L' <xref:System.IDisposable?displayProperty=nameWithType> interface n’est pas traitée dans cette section, mais elle est décrite dans la section [modèle de suppression](../garbage-collection/implementing-dispose.md) .

> [!NOTE]
> Pour obtenir des instructions et des informations supplémentaires sur d’autres types de .NET Framework intégrés communs, consultez les rubriques de référence pour les éléments suivants : <xref:System.DateTime?displayProperty=nameWithType> , <xref:System.DateTimeOffset?displayProperty=nameWithType> , <xref:System.ICloneable?displayProperty=nameWithType> , <xref:System.IComparable%601?displayProperty=nameWithType> , <xref:System.IEquatable%601?displayProperty=nameWithType> , <xref:System.Nullable%601?displayProperty=nameWithType> , <xref:System.Object?displayProperty=nameWithType> , <xref:System.Uri?displayProperty=nameWithType> .

## <a name="in-this-section"></a>Dans cette section

[Tableaux](arrays.md)  
[Attributs](attributes.md)  
[Regroupements](guidelines-for-collections.md)  
[Sérialisation](serialization.md)  
[Utilisation de System. Xml](system-xml-usage.md)  
[Opérateurs d’égalité](equality-operators.md)  

*Parties © 2005, 2009 Microsoft Corporation. Tous droits réservés.*

*Réimprimé avec l’autorisation de Pearson Education, Inc. et extrait de [Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) par Krzysztof Cwalina et Brad Abrams, publié le 22 octobre 2008 par Addison-Wesley Professional dans le cadre de la série sur le développement Microsoft Windows.*
  
## <a name="see-also"></a>Voir aussi

- [Directives de conception d’infrastructure](index.md)
