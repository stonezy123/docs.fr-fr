---
title: Extensions d'
ms.date: 12/13/2019
description: Découvrez comment charger des extensions SQLite.
ms.openlocfilehash: a85b1227be4274dd20156d2475d6d2d68e250f99
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901297"
---
# <a name="extensions"></a><span data-ttu-id="82cba-103">Extensions d'</span><span class="sxs-lookup"><span data-stu-id="82cba-103">Extensions</span></span>

<span data-ttu-id="82cba-104">SQLite prend en charge le chargement des extensions au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="82cba-104">SQLite supports loading extensions at run time.</span></span> <span data-ttu-id="82cba-105">Les extensions incluent des fonctions telles que les fonctions SQL supplémentaires, les classements, les tables virtuelles et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="82cba-105">Extensions include things like additional SQL functions, collations, virtual tables, and more.</span></span>

<span data-ttu-id="82cba-106">.NET Core comprend une logique supplémentaire pour localiser les bibliothèques natives dans d’autres emplacements, tels que les packages NuGet référencés.</span><span class="sxs-lookup"><span data-stu-id="82cba-106">.NET Core includes additional logic for locating native libraries in additional places like referenced NuGet packages.</span></span> <span data-ttu-id="82cba-107">Malheureusement, SQLite ne peut pas tirer parti de cette logique. Il appelle l’API de la plateforme directement pour charger des bibliothèques.</span><span class="sxs-lookup"><span data-stu-id="82cba-107">Unfortunately, SQLite can't leverage this logic; it calls the platform API directly to load libraries.</span></span> <span data-ttu-id="82cba-108">Pour cette raison, vous devrez peut-être modifier les variables d’environnement PATH, LD_LIBRARY_PATH ou DYLD_LIBRARY_PATH avant de charger les extensions SQLite.</span><span class="sxs-lookup"><span data-stu-id="82cba-108">Because of this, you may need to modify the PATH, LD_LIBRARY_PATH, or DYLD_LIBRARY_PATH environment variables before loading SQLite extensions.</span></span> <span data-ttu-id="82cba-109">[Un exemple](https://github.com/dotnet/samples/blob/master/snippets/standard/data/sqlite/ExtensionsSample/Program.cs) sur GitHub illustre la recherche de fichiers binaires pour le runtime actuel à l’intérieur d’un package NuGet référencé.</span><span class="sxs-lookup"><span data-stu-id="82cba-109">There's [a sample](https://github.com/dotnet/samples/blob/master/snippets/standard/data/sqlite/ExtensionsSample/Program.cs) on GitHub that demonstrates finding binaries for the current runtime inside a referenced NuGet package.</span></span>

<span data-ttu-id="82cba-110">Pour charger une extension, appelez la méthode <xref:Microsoft.Data.Sqlite.SqliteConnection.LoadExtension%2A>.</span><span class="sxs-lookup"><span data-stu-id="82cba-110">To load an extension, call the <xref:Microsoft.Data.Sqlite.SqliteConnection.LoadExtension%2A> method.</span></span> <span data-ttu-id="82cba-111">Microsoft. Data. sqlite garantit que l’extension reste chargée même si la connexion est fermée et rouverte.</span><span class="sxs-lookup"><span data-stu-id="82cba-111">Microsoft.Data.Sqlite will ensure that the extension remains loaded even if the connection is closed and reopened.</span></span>

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/ExtensionsSample/Program.cs?name=snippet_LoadExtension)]

## <a name="see-also"></a><span data-ttu-id="82cba-112">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="82cba-112">See also</span></span>

* [<span data-ttu-id="82cba-113">Extensions chargeable au moment de l’exécution</span><span class="sxs-lookup"><span data-stu-id="82cba-113">Run-Time Loadable Extensions</span></span>](https://www.sqlite.org/loadext.html)