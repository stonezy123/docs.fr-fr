---
title: "Comment : animer une propriété à l'aide d'un storyboard"
description: Animez votre interface utilisateur avec des animations et des storyboards pour les propriétés dans Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- animation [WPF], Storyboards
- Storyboards [WPF], animation
ms.assetid: f4a314e9-1da2-4367-85fc-1232487efa7a
ms.openlocfilehash: f21b606751b845905a7bde6d3a7658b214369cc6
ms.sourcegitcommit: b6a1869f97a37f11a68c90afde1a520a6887dcbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853751"
---
# <a name="how-to-animate-a-property-by-using-a-storyboard"></a>Comment : animer une propriété à l'aide d'un storyboard
Cet exemple montre comment utiliser <xref:System.Windows.Media.Animation.Storyboard> pour animer des propriétés. Pour animer une propriété à l’aide d’un <xref:System.Windows.Media.Animation.Storyboard> , créez une animation pour chaque propriété que vous souhaitez animer et créez également un <xref:System.Windows.Media.Animation.Storyboard> pour contenir les animations.  
  
 Le type de propriété détermine le type d’animation à utiliser. Par exemple, pour animer une propriété qui accepte <xref:System.Double> des valeurs, utilisez <xref:System.Windows.Media.Animation.DoubleAnimation> . Les <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> <xref:System.Windows.Media.Animation.Storyboard.TargetProperty> propriétés jointes et spécifient l’objet et la propriété auxquels l’animation est appliquée.  
  
 Pour démarrer une table de montage séquentiel dans [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] , utilisez une <xref:System.Windows.Media.Animation.BeginStoryboard> action et un <xref:System.Windows.EventTrigger> . Le <xref:System.Windows.EventTrigger> commence l' <xref:System.Windows.Media.Animation.BeginStoryboard> action lorsque l’événement spécifié par sa <xref:System.Windows.EventTrigger.RoutedEvent%2A> propriété se produit. L' <xref:System.Windows.Media.Animation.BeginStoryboard> action démarre <xref:System.Windows.Media.Animation.Storyboard> .  
  
 L’exemple suivant utilise des <xref:System.Windows.Media.Animation.Storyboard> objets pour animer deux <xref:System.Windows.Controls.Button> contrôles. Pour que la taille du premier bouton change, son <xref:System.Windows.FrameworkElement.Width%2A> est animé. Pour que le deuxième bouton change de couleur, la <xref:System.Windows.Media.SolidColorBrush.Color%2A> propriété du <xref:System.Windows.Media.SolidColorBrush> est utilisée pour définir le <xref:System.Windows.Controls.Control.Background%2A> du bouton animé.  
  
## <a name="example"></a>Exemple  
 [!code-xaml[AnimatePropertyStoryboards#1](~/samples/snippets/xaml/VS_Snippets_Wpf/AnimatePropertyStoryboards/XAML/StoryboardExample.xaml#1)]  
  
> [!NOTE]
> Bien que les animations puissent cibler à la fois un <xref:System.Windows.FrameworkElement> objet, tel qu’un objet <xref:System.Windows.Controls.Control> ou, <xref:System.Windows.Controls.Panel> et un <xref:System.Windows.Freezable> objet, tel que <xref:System.Windows.Media.Brush> ou <xref:System.Windows.Media.Transform> , seuls les éléments de l’infrastructure ont une <xref:System.Windows.FrameworkElement.Name%2A> propriété. Pour attribuer un nom à un objet Freezable afin qu’il puisse être ciblé par une animation, utilisez la [Directive x:Name](../../../desktop-wpf/xaml-services/xname-directive.md), comme illustré dans l’exemple précédent.  
  
 Si vous utilisez du code, vous devez créer un <xref:System.Windows.NameScope> pour un <xref:System.Windows.FrameworkElement> et inscrire les noms des objets à animer avec celui-ci <xref:System.Windows.FrameworkElement> . Pour démarrer les animations dans le code, utilisez une <xref:System.Windows.Media.Animation.BeginStoryboard> action avec un <xref:System.Windows.EventTrigger> . Si vous le souhaitez, vous pouvez utiliser un gestionnaire d’événements et la <xref:System.Windows.Media.Animation.Storyboard.Begin%2A> méthode de <xref:System.Windows.Media.Animation.Storyboard> . L'exemple suivant illustre l'utilisation de la méthode <xref:System.Windows.Media.Animation.Storyboard.Begin%2A>.  
  
 [!code-csharp[AnimatePropertyStoryboards#11](~/samples/snippets/csharp/VS_Snippets_Wpf/AnimatePropertyStoryboards/CSharp/StoryboardExample.cs#11)]
 [!code-vb[AnimatePropertyStoryboards#11](~/samples/snippets/visualbasic/VS_Snippets_Wpf/AnimatePropertyStoryboards/VisualBasic/StoryboardExample.vb#11)]  
  
 Pour plus d’informations sur l’animation et les tables de montage séquentiel, consultez [Vue d’ensemble de l’animation](animation-overview.md).  
  
 Si vous utilisez du code, vous n’êtes pas limité à l’utilisation d' <xref:System.Windows.Media.Animation.Storyboard> objets pour animer des propriétés. Pour obtenir d’autres informations et exemples, consultez [Comment : animer une propriété sans utiliser de storyboard](how-to-animate-a-property-without-using-a-storyboard.md) et [Comment : animer une propriété à l’aide d’un AnimationClock](how-to-animate-a-property-by-using-an-animationclock.md).
