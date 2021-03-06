---
title: -libpath
ms.date: 03/10/2018
helpviewer_keywords:
- libpath compiler option [Visual Basic]
- /libpath compiler option [Visual Basic]
- -libpath compiler option [Visual Basic]
ms.assetid: 5f1c26c9-3455-4e89-bdf3-b12d6c2e655b
ms.openlocfilehash: dff7e0c3eb696b9b18f4c4e59240a26c1cb9782c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408547"
---
# <a name="-libpath"></a>-libpath
Spécifie l’emplacement des assemblys référencés.  
  
## <a name="syntax"></a>Syntaxe  
  
```console  
-libpath:dirList  
```  
  
## <a name="arguments"></a>Arguments  
  
|Terme|Définition|  
|---|---|  
|`dirList`|Obligatoire. Liste de répertoires délimités par des points-virgules pour le compilateur à examiner si un assembly référencé est introuvable dans le répertoire de travail actuel (le répertoire à partir duquel vous appelez le compilateur) ou le répertoire système de l’common language runtime. Si le nom du répertoire contient un espace, mettez-le entre guillemets ("").|  
  
## <a name="remarks"></a>Notes  
 L' `-libpath` option spécifie l’emplacement des assemblys référencés par l’option [-Reference](reference.md) .  
  
 Le compilateur recherche les références d’assembly qui ne sont pas complètes dans l’ordre suivant :  
  
1. Répertoire de travail actuel. Il s’agit du répertoire à partir duquel le compilateur est appelé.  
  
2. Répertoire système du common language runtime.  
  
3. Répertoires spécifiés par `-libpath` .  
  
4. Répertoires spécifiés par la variable d’environnement LIB.  
  
 L' `-libpath` option est additive ; si vous la spécifiez plusieurs fois, elle est ajoutée à toutes les valeurs précédentes.  
  
 Utilisez `-reference` pour spécifier une référence d’assembly.  
  
|Pour définir-LIBPATH dans l’environnement de développement intégré Visual Studio|  
|---|  
|1. Sélectionnez un projet dans **Explorateur de solutions**. Dans le menu **Projet** , cliquez sur **Propriétés**. <br />2. cliquez sur l’onglet **références** .<br />3. cliquez sur le bouton **chemins d’accès des références..** ..<br />4. dans la boîte de dialogue **chemins d’accès des références** , entrez le nom du répertoire dans la zone **dossier :** .<br />5. cliquez sur **Ajouter un dossier**.|  
  
## <a name="example"></a>Exemple  
 Le code suivant compile `T2.vb` pour créer un fichier. exe. Le compilateur recherche les références d’assembly dans le répertoire de travail, dans le répertoire racine du lecteur C : et dans le nouveau répertoire des assemblys du lecteur C :.  
  
```console  
vbc -libpath:c:\;"c:\New Assemblies" -reference:t2.dll t2.vb  
```  
  
## <a name="see-also"></a>Voir aussi

- [Assemblys dans .NET](../../../standard/assembly/index.md)
- [Compilateur de ligne de commande de Visual Basic](index.md)
- [Exemples de lignes de commande de compilation](sample-compilation-command-lines.md)
