---
title: Exemple de configuration de copie en bloc
ms.date: 03/30/2017
ms.assetid: d4dde6ac-b8b6-4593-965a-635c8fb2dadb
ms.openlocfilehash: 80350d112da03c00e422432ce271ca5ea3ac58ab
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79148840"
---
# <a name="bulk-copy-example-setup"></a>Exemple de configuration de copie en bloc
La classe <xref:System.Data.SqlClient.SqlBulkCopy> peut être utilisée pour écrire des données uniquement sur les tables SQL Server. Les exemples de code présentés dans cette rubrique utilisent l’exemple de base de données SQL Server **AdventureWorks**. Pour éviter de modifier les exemples de code des tables existantes, écrivez les données dans des tables que vous aurez préalablement créées.  
  
 Les tables **BulkCopyDemoMatchingColumns** et **BulkCopyDemoDifferentColumns** sont basées sur la table **AdventureWorks** ** Production.Products**. Dans les exemples de code qui utilisent ces tables, les données sont ajoutées à partir de la table **Production.Products** à l’un de ces exemples de tables. La table **BulkCopyDemoDifferentColumns** est utilisée quand l’exemple illustre comment mapper des colonnes de données source à la table de destination. **BulkCopyDemoMatchingColumns** est utilisé pour la plupart des autres exemples.  
  
 Certains exemples de code montrent comment utiliser une classe <xref:System.Data.SqlClient.SqlBulkCopy> pour écrire dans plusieurs tables. Pour ces échantillons, les tables **BulkCopyDemoOrderHeader** et **BulkCopyDemoOrderDetail** sont utilisées comme tables de destination. Ces tables sont basées sur les tables **Sales.SalesOrderHeader** et **Sales.SalesOrderDetail** dans **AdventureWorks**.  
  
> [!NOTE]
> Les exemples de code **SqlBulkCopy** sont fournis uniquement pour illustrer la syntaxe de l’utilisation de **SqlBulkCopy**. Si les tables source et de destination se trouvent dans la même instance SQL Server, il est plus facile et plus rapide d’utiliser une instruction Transact-SQL `INSERT … SELECT` pour copier les données.  
  
## <a name="table-setup"></a>Configuration de table  
 Pour créer les tables nécessaires à la bonne exécution des exemples de code, vous devez exécuter les instructions Transact-SQL suivantes dans une base de données SQL Server.  
  
```sql
USE AdventureWorks  
  
IF EXISTS (SELECT * FROM dbo.sysobjects
 WHERE id = object_id(N'[dbo].[BulkCopyDemoMatchingColumns]')  
 AND OBJECTPROPERTY(id, N'IsUserTable') = 1)  
    DROP TABLE [dbo].[BulkCopyDemoMatchingColumns]  
  
CREATE TABLE [dbo].[BulkCopyDemoMatchingColumns]([ProductID] [int] IDENTITY(1,1) NOT NULL,  
    [Name] [nvarchar](50) NOT NULL,  
    [ProductNumber] [nvarchar](25) NOT NULL,  
 CONSTRAINT [PK_ProductID] PRIMARY KEY CLUSTERED  
(  
    [ProductID] ASC  
) ON [PRIMARY]) ON [PRIMARY]  
  
IF EXISTS (SELECT * FROM dbo.sysobjects
 WHERE id = object_id(N'[dbo].[BulkCopyDemoDifferentColumns]')  
 AND OBJECTPROPERTY(id, N'IsUserTable') = 1)  
    DROP TABLE [dbo].[BulkCopyDemoDifferentColumns]  
  
CREATE TABLE [dbo].[BulkCopyDemoDifferentColumns]([ProdID] [int] IDENTITY(1,1) NOT NULL,  
    [ProdNum] [nvarchar](25) NOT NULL,  
    [ProdName] [nvarchar](50) NOT NULL,  
 CONSTRAINT [PK_ProdID] PRIMARY KEY CLUSTERED  
(  
    [ProdID] ASC  
) ON [PRIMARY]) ON [PRIMARY]  
  
IF EXISTS (SELECT * FROM dbo.sysobjects
 WHERE id = object_id(N'[dbo].[BulkCopyDemoOrderHeader]')  
 AND OBJECTPROPERTY(id, N'IsUserTable') = 1)  
    DROP TABLE [dbo].[BulkCopyDemoOrderHeader]  
  
CREATE TABLE [dbo].[BulkCopyDemoOrderHeader]([SalesOrderID] [int] IDENTITY(1,1) NOT NULL,  
    [OrderDate] [datetime] NOT NULL,  
    [AccountNumber] [nvarchar](15) NULL,  
 CONSTRAINT [PK_SalesOrderID] PRIMARY KEY CLUSTERED  
(  
    [SalesOrderID] ASC  
) ON [PRIMARY]) ON [PRIMARY]  
  
IF EXISTS (SELECT * FROM dbo.sysobjects
 WHERE id = object_id(N'[dbo].[BulkCopyDemoOrderDetail]')  
 AND OBJECTPROPERTY(id, N'IsUserTable') = 1)  
    DROP TABLE [dbo].[BulkCopyDemoOrderDetail]  
  
CREATE TABLE [dbo].[BulkCopyDemoOrderDetail]([SalesOrderID] [int] NOT NULL,  
    [SalesOrderDetailID] [int] NOT NULL,  
    [OrderQty] [smallint] NOT NULL,  
    [ProductID] [int] NOT NULL,  
    [UnitPrice] [money] NOT NULL,  
 CONSTRAINT [PK_LineNumber] PRIMARY KEY CLUSTERED  
(  
    [SalesOrderID] ASC,  
    [SalesOrderDetailID] ASC  
) ON [PRIMARY]) ON [PRIMARY]  
```  
  
## <a name="see-also"></a>Voir aussi

- [Opérations de copie en vrac dans SQL Server](bulk-copy-operations-in-sql-server.md)
- [Vue d'ensemble d’ADO.NET](../ado-net-overview.md)
