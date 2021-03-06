---
title: '@ (Options du compilateur C#)'
ms.date: 07/20/2015
f1_keywords:
- '@'
helpviewer_keywords:
- response files, specifying for compilation [C#]
- '@ compiler option'
ms.assetid: dda4fa9f-a02c-400f-8b6a-d58834e13d7f
ms.openlocfilehash: d8e5c0ec148754c3e4cebfa32ad9f44a0bb0119e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/14/2020
ms.locfileid: "70202907"
---
# <a name="-c-compiler-options"></a>@ (Options du compilateur C#)
L’option @ vous permet de spécifier un fichier qui contient les options du compilateur et les fichiers de code source à compiler.  
  
## <a name="syntax"></a>Syntaxe  
  
```console  
@response_file  
```  
  
## <a name="arguments"></a>Arguments  
 `response_file`  
 Fichier qui répertorie des options du compilateur ou des fichiers de code source à compiler.  
  
## <a name="remarks"></a>Notes   
 Les options du compilateur et les fichiers de code source seront traités par le compilateur exactement comme s’ils avaient été spécifiés sur la ligne de commande.  
  
 Pour spécifier plusieurs fichiers réponse dans une compilation, spécifiez plusieurs options de fichier réponse. Par exemple :  
  
```console  
@file1.rsp @file2.rsp  
```  
  
 Dans un fichier réponse, plusieurs options du compilateur et fichiers de code source peuvent apparaître sur une même ligne. En revanche, si vous spécifiez une seule option du compilateur, celle-ci doit obligatoirement figurer sur une seule et même ligne. Les fichiers réponse peuvent contenir des commentaires, précédés du symbole #.  
  
 La spécification d’options du compilateur à partir d’un fichier réponse est identique à la spécification de ces commandes sur la ligne de commande. Pour plus d’informations, consultez [Génération à partir de la ligne de commande](./how-to-set-environment-variables-for-the-visual-studio-command-line.md).  
  
 Le compilateur traite les options de commande au fur et à mesure qu’il les rencontre. Par conséquent, des arguments de ligne de commande peuvent substituer des options précédemment affichées dans des fichiers réponse. Inversement, les options d’un fichier réponse substituent les options affichées précédemment dans la ligne de commande ou dans d’autres fichiers réponse.  
  
 C# fournit le fichier csc.rsp, qui se trouve dans le même répertoire que le fichier csc.exe. Voir [-noconfig](./noconfig-compiler-option.md) pour plus d’informations sur csc.rsp.  
  
 Cette option du compilateur ne peut pas être définie dans l’environnement de développement Visual Studio, ni être modifiée par programmation.  
  
## <a name="example"></a> Exemple  
 Voici quelques lignes extraites d’un exemple de fichier réponse :  
  
```console  
# build the first output file  
-target:exe -out:MyExe.exe source1.cs source2.cs  
```  
  
## <a name="see-also"></a>Voir aussi

- [Options de compilateur C](./index.md)
