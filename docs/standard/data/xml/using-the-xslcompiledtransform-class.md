---
title: Utilisation de la classe XslCompiledTransform
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: f9b074f6-d6f4-49dd-a093-df510bf0cf7b
ms.openlocfilehash: 8705b4c6324ce20a1f37d3ee864ab494adcdd6a5
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84281766"
---
# <a name="using-the-xslcompiledtransform-class"></a>Utilisation de la classe XslCompiledTransform
La classe <xref:System.Xml.Xsl.XslCompiledTransform> est le processeur XSLT dans Microsoft .NET Framework. Cette classe est utilisée pour compiler des feuilles de style et exécuter des transformations XSLT.  
  
> [!NOTE]
> Bien que les performances globales de la classe <xref:System.Xml.Xsl.XslCompiledTransform> soient meilleures que celles de la classe <xref:System.Xml.Xsl.XslTransform>, la méthode <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> de la classe <xref:System.Xml.Xsl.XslCompiledTransform> peut s'exécuter plus lentement que la méthode <xref:System.Xml.Xsl.XslTransform.Load%2A> de la classe <xref:System.Xml.Xsl.XslTransform> la première fois qu'elle est appelée pour une transformation. C'est parce que le fichier XSLT doit être compilé avant d'être chargé. Pour plus d'informations, consultez le billet de blog suivant : [XslCompiledTransform plus lent que XslTransform ?](https://docs.microsoft.com/archive/blogs/antosha/xslcompiledtransform-slower-than-xsltransform).  
  
## <a name="in-this-section"></a>Dans cette section  
 [Entrées dans la classe XslCompiledTransform](inputs-to-the-xslcompiledtransform-class.md)  
 Décrit les options d'entrée XSLT disponibles.  
  
 [Options de sortie de la classe XslCompiledTransform](output-options-on-the-xslcompiledtransform-class.md)  
 Décrit les options de sortie XSLT disponibles.  
  
 [Résolution de ressources externes lors du traitement XSLT](resolving-external-resources-during-xslt-processing.md)  
 Présente l'utilisation de la classe <xref:System.Xml.XmlResolver> pour résoudre des ressources externes.  
  
 [Extension de feuilles de style XSLT](extending-xslt-style-sheets.md)  
 Présente la prise en charge des extensions XSLT.  
  
|||  
|-|-|  
|[Erreurs XSLT récupérables](recoverable-xslt-errors.md)|Répertorie chaque comportement discrétionnaire autorisé par la recommandation du World Wide Web Consortium (W3C) sur XSLT 1.0 et décrit la façon dont ces comportements sont gérés par la classe <xref:System.Xml.Xsl.XslCompiledTransform>.|  
|[Procédure : transformer un fragment de nœud](how-to-transform-a-node-fragment.md)|Décrit comment transformer un fragment de nœud.|  
  
## <a name="related-sections"></a>Sections connexes  
 [Migration depuis la classe XslTransform](migrating-from-the-xsltransform-class.md)  
 Explique comment migrer du code à partir de la classe <xref:System.Xml.Xsl.XslTransform>  
  
## <a name="see-also"></a>Voir aussi

- <xref:System.Xml.Xsl.XsltSettings>
- <xref:System.Xml.Xsl.XsltMessageEncounteredEventArgs>
