---
title: ICorDebugProcess3::SetEnableCustomNotification, méthode
ms.date: 03/30/2017
api_name:
- ICorDebugProcess3.SetEnableCustomNotification Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess3::SetEnableCustomNotification
helpviewer_keywords:
- ICorDebugProcess3::SetEnableCustomNotification method [.NET Framework debugging]
- SetEnableCustomNotification method [.NET Framework debugging]
ms.assetid: afd88ee9-2589-4461-a75a-9b6fe55a2525
topic_type:
- apiref
ms.openlocfilehash: 523d9665ffd2637a0e856d74d4d3b3838cb5e83c
ms.sourcegitcommit: 488aced39b5f374bc0a139a4993616a54d15baf0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83212124"
---
# <a name="icordebugprocess3setenablecustomnotification-method"></a>ICorDebugProcess3::SetEnableCustomNotification, méthode
Active et désactive les notifications personnalisées du débogueur du type spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
HRESULT SetEnableCustomNotification(ICorDebugClass * pClass,  
                                    BOOL fEnable);  
```  
  
## <a name="parameters"></a>Paramètres  
 `pClass`  
 dans Type qui spécifie les notifications personnalisées du débogueur.  
  
 `fEnable`  
 [in] `true` pour activer les notifications personnalisées du débogueur ; `false`pour désactiver les notifications. La valeur par défaut est `false`.  
  
## <a name="remarks"></a>Remarks  
 Lorsque `fEnable` a la valeur `true` , les appels à la <xref:System.Diagnostics.Debugger.NotifyOfCrossThreadDependency%2A?displayProperty=nameWithType> méthode déclenchent un rappel [ICorDebugManagedCallback3 :: CustomNotification,](icordebugmanagedcallback3-customnotification-method.md) . Les notifications sont désactivées par défaut ; par conséquent, le débogueur doit spécifier les types de notification qu’il connaît et souhaite gérer. Étant donné que la classe [ICorDebugClass](icordebug-interface.md) est limitée par le domaine d’application, le débogueur doit appeler `SetEnableCustomNotification` pour chaque domaine d’application dans le processus s’il souhaite recevoir la notification dans tout le processus.  
  
 À partir du .NET Framework 4, la seule notification prise en charge est une notification de dépendance inter-threads.  
  
## <a name="requirements"></a>Spécifications  
 **Plateformes :** Consultez [Configuration requise](../../get-started/system-requirements.md).  
  
 **En-tête :** CorDebug.idl, CorDebug.h  
  
 **Bibliothèque :** CorGuids.lib  
  
 **Versions de .NET Framework :**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Voir aussi

- [ICorDebugProcess3, interface](icordebugprocess3-interface.md)
- [Interfaces de débogage](debugging-interfaces.md)
- [Débogage](index.md)
