---
title: ICorDebugILFrame::GetIP, méthode
ms.date: 03/30/2017
api_name:
- ICorDebugILFrame.GetIP
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugILFrame::GetIP
helpviewer_keywords:
- GetIP method, ICorDebugILFrame interface [.NET Framework debugging]
- ICorDebugILFrame::GetIP method [.NET Framework debugging]
ms.assetid: 18217ba1-1776-4297-a3b9-f77e64b0fead
topic_type:
- apiref
ms.openlocfilehash: 3890cb4236f113bc6efc23bfb606d19a525ec234
ms.sourcegitcommit: 488aced39b5f374bc0a139a4993616a54d15baf0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83210265"
---
# <a name="icordebugilframegetip-method"></a>ICorDebugILFrame::GetIP, méthode
Obtient la valeur du pointeur d’instruction et une valeur de combinaison au niveau du bit qui décrit comment la valeur du pointeur d’instruction a été obtenue.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
HRESULT GetIP (  
    [out] ULONG32               *pnOffset,
    [out] CorDebugMappingResult *pMappingResult  
);  
```  
  
## <a name="parameters"></a>Paramètres  
 `pnOffset`  
 à Valeur du pointeur d’instruction.  
  
 `pMappingResult`  
 à Pointeur vers une combinaison d’opérations de bits des valeurs d’énumération CorDebugMappingResult, qui décrivent comment la valeur du pointeur d’instruction a été obtenue.  
  
## <a name="remarks"></a>Remarks  
 La valeur du pointeur d’instruction est l’offset du frame de pile dans le code MSIL (Microsoft Intermediate Language) de la fonction. Si le frame de pile est actif, cette adresse est l’instruction suivante à exécuter. Si le frame de pile n’est pas actif, cette adresse est la prochaine instruction à exécuter lorsque le frame de pile est réactivé.  
  
 Si ce frame est un frame compilé juste-à-temps (JIT), la valeur du pointeur d’instruction sera déterminée par le mappage vers l’arrière à partir du pointeur d’instruction Native réel, donc la valeur peut être uniquement approximative.  
  
## <a name="requirements"></a>Spécifications  
 **Plateformes :** Consultez [Configuration requise](../../get-started/system-requirements.md).  
  
 **En-tête :** CorDebug.idl, CorDebug.h  
  
 **Bibliothèque :** CorGuids.lib  
  
 **Versions de .NET Framework :**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
