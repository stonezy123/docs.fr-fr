---
title: ICorDebugThread4::GetBlockingObjects, méthode
ms.date: 03/30/2017
api_name:
- ICorDebugThread4.GetBlockingObjects Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread4::GetBlockingObjects
helpviewer_keywords:
- GetBlockingObjects method [.NET Framework debugging]
- ICorDebugThread4::GetBlockingObjects method [.NET Framework debugging]
ms.assetid: a7e6c54e-7be9-4e52-bbb4-95f52458e8e4
topic_type:
- apiref
ms.openlocfilehash: 366b5124cc66a4e9a1c3bd4e77f604f15ba8d8a8
ms.sourcegitcommit: d6bd7903d7d46698e9d89d3725f3bb4876891aa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/13/2020
ms.locfileid: "83379674"
---
# <a name="icordebugthread4getblockingobjects-method"></a>ICorDebugThread4::GetBlockingObjects, méthode
Fournit une énumération ordonnée des structures [CorDebugBlockingObject](cordebugblockingobject-structure.md) qui fournissent des informations sur le blocage des threads.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
HRESULT GetBlockingObjects (  
    [out] ICorDebugBlockingObjectEnum **ppBlockingObjectEnum  
```  
  
## <a name="parameters"></a>Paramètres  
 `ppBlockingObjectEnum`  
 à Pointeur vers une énumération ordonnée de structures [CorDebugBlockingObject](cordebugblockingobject-structure.md) .  
  
## <a name="remarks"></a>Remarks  
 Le premier élément de l’énumération retournée correspond à la première structure qui bloque le thread. Le deuxième élément correspond à un élément bloquant qui est rencontré lors de l’exécution d’un appel de procédure asynchrone (APC) lorsqu’il est bloqué sur le premier, et ainsi de suite.  
  
 L’énumération est valide uniquement pour la durée de l’état synchronisé actuel.  
  
 Cette méthode doit être appelée lorsque l’élément débogué est dans un état synchronisé.  
  
 Si `ppBlockingObjectEnum` n’est pas un pointeur valide, le résultat n’est pas défini.  
  
 Si un thread est bloqué et que l’erreur ne peut pas être déterminée, la méthode retourne un HRESULT qui indique un échec ; Sinon, elle retourne S_OK.  
  
## <a name="requirements"></a>Spécifications  
 **Plateformes :** Consultez [Configuration requise](../../get-started/system-requirements.md).  
  
 **En-tête :** CorDebug.idl, CorDebug.h  
  
 **Bibliothèque :** CorGuids.lib  
  
 **Versions de .NET Framework :**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Voir aussi

- [ICorDebugThread4, interface](icordebugthread4-interface.md)
- [Interfaces de débogage](debugging-interfaces.md)
- [Débogage](index.md)
