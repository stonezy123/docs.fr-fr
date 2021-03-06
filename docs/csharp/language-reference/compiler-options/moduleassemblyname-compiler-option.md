---
title: -moduleassemblyname (Options du compilateur C#)
ms.date: 07/20/2015
f1_keywords:
- /moduleassemblyname
helpviewer_keywords:
- moduleassemblyname compiler option [C#]
- /moduleassemblyname compiler option [C#]
- .moduleassemblyname compiler option [C#]
ms.assetid: d464d9b9-f18d-423b-95e9-66c7878fd53a
ms.openlocfilehash: 1477eeb0f2e16e18cb86009739bc8e7d9dee2ac0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173716"
---
# <a name="-moduleassemblyname-c-compiler-option"></a>-moduleassemblyname (Options du compilateur C#)
Spécifie un assembly dont les types non publics sont accessibles par un .netmodule.  
  
## <a name="syntax"></a>Syntaxe  
  
```console  
-moduleassemblyname:assembly_name  
```  
  
## <a name="arguments"></a>Arguments  
 `assembly_name`  
 Nom de l’assembly dont les types non publics sont accessibles au fichier .netmodule.  
  
## <a name="remarks"></a>Notes   
 **-moduleassemblyname** doit être utilisée au moment de créer un fichier .netmodule et quand les conditions suivantes sont réunies :  
  
- Le fichier .netmodule doit accéder à des types non publics dans un assembly existant.  
  
- Vous connaissez le nom de l’assembly dans lequel le fichier .netmodule sera généré.  
  
- L’assembly existant a accordé un accès d’assembly friend à l’assembly dans lequel le fichier .netmodule sera généré.  
  
 Pour plus d’informations sur la création d’un fichier .netmodule, consultez [-target:module (Options du compilateur C#)](./target-module-compiler-option.md).  
  
 Pour plus d’informations sur les assemblys friend, consultez [Assemblys friend](../../../standard/assembly/friend.md).  
  
 Cette option n’est pas disponible dans l’environnement de développement ; elle l’est uniquement au moment de compiler à partir de la ligne de commande.  
  
 Cette option de compilateur n’est pas disponible dans Visual Studio et ne peut pas être changée par programmation.  
  
## <a name="example"></a> Exemple  
 Cet exemple génère un assembly avec un type privé, ce qui octroie un accès d’assembly friend à un assembly appelé csman_an_assembly.  
  
```csharp  
// moduleassemblyname_1.cs  
// compile with: -target:library  
using System;  
using System.Runtime.CompilerServices;  
  
[assembly:InternalsVisibleTo ("csman_an_assembly")]  
  
class An_Internal_Class
{  
    public void Test()
    {
        Console.WriteLine("An_Internal_Class.Test called");
    }  
}  
```  
  
## <a name="example"></a> Exemple  
 Cet exemple génère un fichier .netmodule qui accède à un type non public dans l’assembly moduleassemblyname_1.dll. En sachant que cette .netmodule sera intégrée dans une assemblée appelée csman_an_assembly, nous pouvons spécifier **-moduleassemblyname**, permettant à la .netmodule d’accéder aux types non publics dans une assemblée qui a accordé l’accès à l’assemblage d’amis à csman_an_assembly.  
  
```csharp  
// moduleassemblyname_2.cs  
// compile with: -moduleassemblyname:csman_an_assembly -target:module -reference:moduleassemblyname_1.dll  
class B {  
    public void Test() {  
        An_Internal_Class x = new An_Internal_Class();  
        x.Test();  
    }  
}  
```  
  
## <a name="example"></a> Exemple  
 Cet exemple de code génère l’assembly csman_an_assembly en faisant référence à l’assembly et au fichier .netmodule générés précédemment.  
  
```csharp  
// csman_an_assembly.cs  
// compile with: -addmodule:moduleassemblyname_2.netmodule -reference:moduleassemblyname_1.dll  
class A {  
    public static void Main() {  
        B bb = new B();  
        bb.Test();  
    }  
}  
```  
  
**An_Internal_Class.Test called**

## <a name="see-also"></a>Voir aussi

- [Options de compilateur C](./index.md)
- [Gestion des propriétés des projets et des solutions](/visualstudio/ide/managing-project-and-solution-properties)
