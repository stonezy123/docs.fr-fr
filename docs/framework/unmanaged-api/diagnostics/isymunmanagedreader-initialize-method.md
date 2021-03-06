---
title: ISymUnmanagedReader::Initialize, méthode
ms.date: 03/30/2017
api_name:
- ISymUnmanagedReader.Initialize
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedReader::Initialize
helpviewer_keywords:
- ISymUnmanagedReader::Initialize method [.NET Framework debugging]
- Initialize method, ISymUnmanagedReader interface [.NET Framework debugging]
ms.assetid: 8f0dd2fe-7df7-464e-91f4-5518c586bb5f
topic_type:
- apiref
ms.openlocfilehash: 07d2de5d12fd769cb5cce243d9e721bb6fc185a7
ms.sourcegitcommit: 27db07ffb26f76912feefba7b884313547410db5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83615473"
---
# <a name="isymunmanagedreaderinitialize-method"></a>ISymUnmanagedReader::Initialize, méthode
Initialise le lecteur de symboles avec l’interface de l’importateur de métadonnées à laquelle ce lecteur sera associé, ainsi que le nom de fichier du module.  
  
> [!NOTE]
> Cette méthode ne peut être appelée qu’une seule fois et doit être appelée avant toute autre méthode de lecteur.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
HRESULT Initialize (  
    [in]  IUnknown     *importer,  
    [in]  const WCHAR  *filename,  
    [in]  const WCHAR  *searchPath,  
    [in]  IStream      *pIStream);  
```  
  
## <a name="parameters"></a>Paramètres  
 `importer`  
 dans Interface de l’importateur de métadonnées à laquelle ce lecteur sera associé.  
  
 `filename`  
 dans Nom de fichier du module. Vous pouvez utiliser le `pIStream` paramètre à la place.  
  
 `searchPath`  
 dans Chemin d’accès à rechercher. Ce paramètre est facultatif.  
  
 `pIStream`  
 dans Le flux de fichier, utilisé comme alternative au paramètre filename.  
  
## <a name="return-value"></a>Valeur de retour  
 S_OK si la méthode est réussie ; Sinon, E_FAIL ou un autre code d’erreur.  
  
## <a name="remarks"></a>Notes  
 Vous devez spécifier un seul des `filename` paramètres ou, mais `pIStream` pas les deux. Le paramètre `searchPath` est facultatif.  
  
## <a name="requirements"></a>Conditions requises  
 **En-tête :** CorSym. idl, CorSym. h  
  
## <a name="see-also"></a>Voir aussi

- [ISymUnmanagedReader, interface](isymunmanagedreader-interface.md)
