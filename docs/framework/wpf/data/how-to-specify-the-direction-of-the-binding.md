---
title: 'Comment : spécifier le sens de la liaison'
ms.date: 03/30/2017
helpviewer_keywords:
- direction of binding [WPF]
- binding direction [WPF]
- data binding [WPF], direction of binding
ms.assetid: 37334478-028b-4514-86c9-1420709f4818
ms.openlocfilehash: 8f7d843382ee3409047d7cf9549267d582883984
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459091"
---
# <a name="how-to-specify-the-direction-of-the-binding"></a>Comment : spécifier le sens de la liaison

Cet exemple montre comment spécifier si la liaison met à jour uniquement la propriété de la cible de liaison (cible), uniquement la propriété de la source de liaison (source), ou bien à la fois la propriété cible et la propriété source.  
  
## <a name="example"></a>Exemple  
 Vous utilisez la propriété <xref:System.Windows.Data.Binding.Mode%2A?displayProperty=nameWithType> pour spécifier le sens de la liaison. Les options disponibles pour les mises à jour de liaison sont les suivantes :  
  
- <xref:System.Windows.Data.BindingMode.TwoWay?displayProperty=nameWithType> met à jour la propriété ou la propriété cible chaque fois que la propriété cible ou la propriété source change.  
  
- <xref:System.Windows.Data.BindingMode.OneWay?displayProperty=nameWithType> met à jour la propriété cible uniquement lorsque la propriété source change.  
  
- <xref:System.Windows.Data.BindingMode.OneTime?displayProperty=nameWithType> met à jour la propriété cible uniquement lorsque l’application démarre ou lorsque le <xref:System.Windows.FrameworkElement.DataContext%2A> subit une modification.  
  
- <xref:System.Windows.Data.BindingMode.OneWayToSource?displayProperty=nameWithType> met à jour la propriété source lorsque la propriété cible est modifiée.  
  
- <xref:System.Windows.Data.BindingMode.Default?displayProperty=nameWithType> entraîne l’utilisation de la valeur de <xref:System.Windows.Data.Binding.Mode%2A> par défaut de la propriété cible.  
  
 Pour plus d’informations, consultez l’énumération <xref:System.Windows.Data.BindingMode>.  
  
 L'exemple suivant indique comment définir la propriété <xref:System.Windows.Data.Binding.Mode%2A>.  
  
 [!code-xaml[DirectionalBinding#4](~/samples/snippets/csharp/VS_Snippets_Wpf/DirectionalBinding/CSharp/Page1.xaml#4)]  
  
 Pour détecter les modifications de la source (applicables aux liaisons <xref:System.Windows.Data.BindingMode.OneWay> et <xref:System.Windows.Data.BindingMode.TwoWay>), la source doit implémenter un mécanisme de notification de modification de propriété approprié, tel que <xref:System.ComponentModel.INotifyPropertyChanged>. Consultez [implémenter la notification de modification de propriété](how-to-implement-property-change-notification.md) pour obtenir un exemple d’implémentation de <xref:System.ComponentModel.INotifyPropertyChanged>.  
  
 Pour les liaisons <xref:System.Windows.Data.BindingMode.TwoWay> ou <xref:System.Windows.Data.BindingMode.OneWayToSource>, vous pouvez contrôler le minutage des mises à jour de la source en définissant la propriété <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A>. Pour plus d'informations, voir <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A>.  
  
## <a name="see-also"></a>Voir aussi

- <xref:System.Windows.Data.Binding>
- [Vue d’ensemble de la liaison de données](../../../desktop-wpf/data/data-binding-overview.md)
- [Rubriques de guide pratique](data-binding-how-to-topics.md)
