---
title: COR_SEGMENT, structure
ms.date: 03/30/2017
api_name:
- COR_SEGMENT
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- COR_SEGMENT
helpviewer_keywords:
- COR_SEGMENT structure [.NET Framework debugging]
ms.assetid: 93aeecb9-7fef-4545-8daf-f566dfc47084
topic_type:
- apiref
ms.openlocfilehash: a5c743064b8ca645cf45d02b8800c88187bf4c6c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79179282"
---
# <a name="cor_segment-structure"></a>COR_SEGMENT, structure
Contient des informations sur une région de la mémoire dans le tas managé.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
typedef struct _COR_SEGMENT {  
    CORDB_ADDRESS start;
    CORDB_ADDRESS end;
    CorDebugGenerationTypes gen;
    ULONG heap;
} COR_SEGMENT;  
```  
  
## <a name="members"></a>Membres  
  
|Membre|Description|  
|------------|-----------------|  
|`start`|Adresse de départ de la zone de mémoire.|  
|`end`|Adresse de fin de la zone de mémoire.|  
|`gen`|Membre d’énumération [CorDebugGenerationTypes](cordebuggenerationtypes-enumeration.md) qui indique la génération de la zone de mémoire.|  
|`heap`|Numéro du tas dans lequel réside la zone de mémoire. Pour plus d'informations, consultez la section Remarques.|  
  
## <a name="remarks"></a>Notes   
 La structure `COR_SEGMENTS` représente une zone de mémoire dans le tas managé.  Les objets `COR_SEGMENTS` sont des membres de l’objet de collection [ICorDebugHeapRegionEnum](icordebugheapsegmentenum-interface.md), qui est rempli en appelant la méthode [ICorDebugProcess5::EnumerateHeapRegions](icordebugprocess5-enumerateheapregions-method.md).  
  
 Le champ `heap` est le numéro de processeur, qui correspond au tas signalé. Pour les récupérateurs de mémoire de station de travail, sa valeur est toujours égale à zéro, car les stations de travail n’ont qu’un seul tas de garbage collection. Pour les récupérateurs de mémoire de serveur, sa valeur correspond au processeur auquel le tas est attaché. Notez qu’il peut y avoir plus ou moins de tas de garbage collection que de processeurs, en raison des détails d’implémentation du récupérateur de mémoire.  
  
## <a name="requirements"></a>Spécifications  
 **Plateformes :** Consultez [Configuration requise](../../get-started/system-requirements.md).  
  
 **En-tête :** CorDebug.idl, CorDebug.h  
  
 **Bibliothèque :** CorGuids.lib  
  
 **.NET Versions-cadre:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Voir aussi

- [Structures de débogage](debugging-structures.md)
- [Débogage](index.md)
