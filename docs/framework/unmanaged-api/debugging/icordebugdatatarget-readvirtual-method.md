---
title: ICorDebugDataTarget::ReadVirtual, méthode
ms.date: 03/30/2017
api_name:
- ICorDebugDataTarget.ReadVirtual Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugDataTarget::ReadVirtual
helpviewer_keywords:
- ICorDebugDataTarget::ReadVirtual method [.NET Framework debugging]
- ReadVirtual method, ICorDebugDataTarget interface [.NET Framework debugging]
ms.assetid: 55e57640-b3d2-413d-b4f4-fbc27fb8e37c
topic_type:
- apiref
ms.openlocfilehash: 36a18d92f05db55957bba55de84490c0da1a1f86
ms.sourcegitcommit: fff146ba3fd1762c8c432d95c8b877825ae536fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82976510"
---
# <a name="icordebugdatatargetreadvirtual-method"></a>ICorDebugDataTarget::ReadVirtual, méthode
Obtient un bloc de mémoire contiguë commençant à l’adresse spécifiée et le retourne dans la mémoire tampon fournie.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
HRESULT ReadVirtual(  
    [in] CORDB_ADDRESS   address,  
    [out, size_is(bytesRequested), length_is(*pBytesRead)]  
          BYTE *     pBuffer,  
    [in]  ULONG32    bytesRequested,  
    [out] ULONG32 *  pBytesRead);  
```  
  
## <a name="parameters"></a>Paramètres  
 `address`  
 dans Adresse de début de la mémoire demandée.  
  
 `pbuffer`  
 à Mémoire tampon dans laquelle la mémoire sera stockée.  
  
 `bytesRequested`  
 dans Nombre d’octets à récupérer à partir de l’adresse cible.  
  
 `pBytesRead`  
 à Nombre d’octets lus réellement à partir de l’adresse cible. Ce nombre peut être inférieur `bytesRequested`à.  
  
## <a name="remarks"></a>Notes   
 Si le premier octet (à l’adresse de début spécifiée) peut être lu, l’appel doit retourner Success (pour prendre en charge la lecture efficace des structures de données avec une longueur autodescriptive, comme les chaînes terminées par le caractère null).  
  
## <a name="requirements"></a>Spécifications  
 **Plateformes :** Consultez [Configuration requise](../../get-started/system-requirements.md).  
  
 **En-tête :** CorDebug.idl, CorDebug.h  
  
 **Bibliothèque :** CorGuids.lib  
  
 **Versions de .NET Framework :**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Voir aussi

- [ICorDebugDataTarget, interface](icordebugdatatarget-interface.md)
- [Interfaces de débogage](debugging-interfaces.md)
- [Débogage](index.md)
