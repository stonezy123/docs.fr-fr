---
title: Matérialisation intermédiaire (C#)
description: Cet exemple C# illustre la matérialisation intermédiaire, où une requête amène AppendString à énumérer la totalité de sa source avant de générer le premier élément.
ms.date: 07/20/2015
ms.assetid: 7922d38f-5044-41cf-8e17-7173d6553a5e
ms.openlocfilehash: 30951aaeea261efbd414205bcc54b63106324344
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165715"
---
# <a name="intermediate-materialization-c"></a>Matérialisation intermédiaire (C#)
Si vous n’y prenez pas garde, dans certaines situations vous risquez de modifier de manière significative le profil de mémoire et de performances de votre application en provoquant la matérialisation prématurée de collections dans vos requêtes. Certains opérateurs de requête standard provoquent la matérialisation de leur collection source avant de générer un seul élément. Par exemple, <xref:System.Linq.Enumerable.OrderBy%2A?displayProperty=nameWithType> itère au sein de l'ensemble de sa collection source, trie tous les éléments, puis génère le premier élément. Cela signifie qu'il est coûteux d'obtenir le premier élément d'une collection ordonnée ; chaque élément ultérieur a un faible coût. Tout cela est logique : il serait impossible pour cet opérateur de requête de faire autrement.  
  
## <a name="example"></a>Exemple  
 Cet exemple modifie l’exemple précédent. La méthode `AppendString` appelle <xref:System.Linq.Enumerable.ToList%2A> avant d'itérer la source. Cela provoque la matérialisation.  
  
```csharp  
public static class LocalExtensions  
{  
    public static IEnumerable<string>  
      ConvertCollectionToUpperCase(this IEnumerable<string> source)  
    {  
        foreach (string str in source)  
        {  
            Console.WriteLine("ToUpper: source >{0}<", str);  
            yield return str.ToUpper();  
        }  
    }  
  
    public static IEnumerable<string>  
      AppendString(this IEnumerable<string> source, string stringToAppend)  
    {  
        // the following statement materializes the source collection in a List<T>  
        // before iterating through it  
        foreach (string str in source.ToList())  
        {  
            Console.WriteLine("AppendString: source >{0}<", str);  
            yield return str + stringToAppend;  
        }  
    }  
}  
  
class Program  
{  
    static void Main(string[] args)  
    {  
        string[] stringArray = { "abc", "def", "ghi" };  
  
        IEnumerable<string> q1 =  
            from s in stringArray.ConvertCollectionToUpperCase()  
            select s;  
  
        IEnumerable<string> q2 =  
            from s in q1.AppendString("!!!")  
            select s;  
  
        foreach (string str in q2)  
        {  
            Console.WriteLine("Main: str >{0}<", str);  
            Console.WriteLine();  
        }  
    }  
}  
```  
  
 Cet exemple produit la sortie suivante :  
  
```output  
ToUpper: source >abc<  
ToUpper: source >def<  
ToUpper: source >ghi<  
AppendString: source >ABC<  
Main: str >ABC!!!<  
  
AppendString: source >DEF<  
Main: str >DEF!!!<  
  
AppendString: source >GHI<  
Main: str >GHI!!!<  
```  
  
 Dans cet exemple, on observe que l'appel de <xref:System.Linq.Enumerable.ToList%2A> force `AppendString` à parcourir l'ensemble de sa source avant de générer le premier élément. Si la source est un grand tableau, cela modifie considérablement le profil de mémoire de l'application.  
  
 Les opérateurs de requête standard peuvent également être chaînés ensemble. La dernière rubrique de ce didacticiel illustre cela.  
  
- [Chaînage d’opérateurs de requête standard (C#)](./chaining-standard-query-operators-together.md)  
  
## <a name="see-also"></a>Voir aussi

- [Didacticiel : chaînage de requêtes (C#)](./deferred-execution-and-lazy-evaluation-in-linq-to-xml.md)
