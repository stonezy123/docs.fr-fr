---
title: 'Procédure : retourner une valeur à partir d’une tâche'
description: Consultez Comment utiliser le type System. Threading. Tasks. Task <TResult> pour retourner une valeur à partir de la propriété Result dans .net.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, how to return a value
ms.assetid: c4bc0f44-eba2-4e96-9e03-1cc787461e61
ms.openlocfilehash: 051cef7cac654e4369ec1486884876004370ba0b
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84767973"
---
# <a name="how-to-return-a-value-from-a-task"></a>Procédure : retourner une valeur à partir d’une tâche
Cet exemple montre comment utiliser le type <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> pour retourner une valeur à partir de la propriété <xref:System.Threading.Tasks.Task%601.Result%2A>. Il nécessite que le répertoire C:\Users\Public\Pictures\Sample Pictures\ existe et qu'il contienne des fichiers.  
  
## <a name="example"></a>Exemple  
 [!code-csharp[TPL#10](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl/cs/returnavalue10.cs#10)]
 [!code-vb[TPL#10](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl/vb/10_returnavalue.vb#10)]  
  
 La propriété <xref:System.Threading.Tasks.Task%601.Result%2A> bloque le thread appelant jusqu'à ce que la tâche se termine.  
  
 Pour savoir comment passer le résultat d’une <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> à une tâche de continuation, consultez [Chaînage des tâches à l’aide de tâches de continuation](chaining-tasks-by-using-continuation-tasks.md).  
  
## <a name="see-also"></a>Voir aussi

- [Programmation asynchrone basée sur les tâches](task-based-asynchronous-programming.md)
- [Expressions lambda dans PLINQ et TPL](lambda-expressions-in-plinq-and-tpl.md)
