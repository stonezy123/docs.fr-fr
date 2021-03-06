---
title: ISymUnmanagedENCUpdate, interface
ms.date: 03/30/2017
api_name:
- ISymUnmanagedENCUpdate
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedENCUpdate
helpviewer_keywords:
- ISymUnmanagedENCUpdate interface [.NET Framework debugging]
ms.assetid: 63a9ef45-01a6-46da-b958-5c6dc2dc232c
topic_type:
- apiref
ms.openlocfilehash: 1986d5f91a3dcfa31a43f729ee1f50129e083f5f
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501740"
---
# <a name="isymunmanagedencupdate-interface"></a>ISymUnmanagedENCUpdate, interface
Fournit des fonctions pour la fonctionnalité modifier et continuer.  
  
## <a name="methods"></a>Méthodes  
  
|Méthode|Description|  
|------------|-----------------|  
|[GetLocalVariableCount, méthode](isymunmanagedencupdate-getlocalvariablecount-method.md)|Obtient le nombre de variables locales.|  
|[GetLocalVariables, méthode](isymunmanagedencupdate-getlocalvariables-method.md)|Obtient les variables locales.|  
|[InitializeForEnc, méthode](isymunmanagedencupdate-initializeforenc-method.md)|Autorise le calcul des limites de méthode avant le premier appel à la méthode [ISymUnmanagedENCUpdate :: UpdateSymbolStore2,](isymunmanagedencupdate-updatesymbolstore2-method.md) .|  
|[UpdateMethodLines, méthode](isymunmanagedencupdate-updatemethodlines-method.md)|Autorise la mise à jour des informations de ligne pour une méthode qui n’a pas été recompilée, mais dont les lignes ont été déplacées indépendamment. Un Delta est autorisé pour chaque instruction.|  
|[UpdateSymbolStore2, méthode](isymunmanagedencupdate-updatesymbolstore2-method.md)|Permet à un compilateur d’omettre les fonctions qui n’ont pas été modifiées à partir du flux de base de données du programme (PDB), à condition que les informations de ligne remplissent les conditions requises. Les informations de ligne correctes peuvent être déterminées par les anciennes informations de ligne PDB et un Delta pour toutes les lignes de la fonction.|  
  
## <a name="requirements"></a>Configuration requise  
 **En-tête :** CorSym. idl, CorSym. h  
  
## <a name="see-also"></a>Voir aussi

- [Interfaces du magasin de symboles de diagnostics](diagnostics-symbol-store-interfaces.md)
