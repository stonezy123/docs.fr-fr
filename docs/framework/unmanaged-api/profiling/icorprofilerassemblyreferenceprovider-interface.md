---
title: ICorProfilerAssemblyReferenceProvider, interface
ms.date: 03/30/2017
api_name:
- ICorProfilerAssemblyReferenceProvider
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: 17205116-66e1-4acc-8f01-532fb3867028
topic_type:
- apiref
ms.openlocfilehash: 17fb3de830d1aed2214631bbe8254a7f58030025
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500518"
---
# <a name="icorprofilerassemblyreferenceprovider-interface"></a>ICorProfilerAssemblyReferenceProvider, interface
[Pris en charge dans .NET Framework 4.5.2 et ultérieur]  
  
 Permet au profileur d’informer le common language runtime (CLR) des références d’assembly que le profileur ajoutera dans le rappel de [ICorProfilerCallback :: ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) .  
  
## <a name="methods"></a>Méthodes  
  
|Méthode|Description|  
|------------|-----------------|  
|[AddAssemblyReference, méthode](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md)|Informe le CLR d’une référence d’assembly que le profileur prévoit d’ajouter dans le rappel [ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) .|  
  
## <a name="remarks"></a>Remarques  
 Le CLR passe le profileur `ICorProfilerAssemblyReferenceProvider` à un objet d’interface dans le rappel [ICorProfilerCallback6 :: GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) . Cela permet au profileur d’informer le CLR des références d’assembly que le profileur prévoit d’ajouter ultérieurement dans [ICorProfilerCallback :: ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md). rappel. Ceci améliore la précision de l'analyseur de fermeture de référence d'assembly du CLR et de ses algorithmes pour déterminer si les assemblys peuvent être partagés.  
  
 Cette interface peut être utilisée uniquement dans le rappel [ICorProfilerCallback6 :: GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) qui passe cet objet d’interface au profileur.  
  
## <a name="requirements"></a>Configuration requise  
 **Plateformes :** Consultez [Configuration requise](../../get-started/system-requirements.md).  
  
 **En-tête :** CorProf.idl, CorProf.h  
  
 **Versions de .NET Framework :**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>Voir aussi

- [Interfaces de profilage](profiling-interfaces.md)
