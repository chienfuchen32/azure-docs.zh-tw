---
title: 適用於 Azure Cosmos DB 的 Azure PowerShell 範例 - Gremlin API
description: 取得 Azure PowerShell 範例，以在 Azure Cosmos DB Gremlin API 中執行各種常見工作
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 01/20/2021
ms.author: mjbrown
ms.openlocfilehash: 393b5988c65b0c3f0ade85f014b1bd9f7d38abdc
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2021
ms.locfileid: "98679335"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-gremlin-api"></a>適用於 Azure Cosmos DB 的 Azure PowerShell 範例 - Gremlin API
[!INCLUDE[appliesto-gremlin-api](includes/appliesto-gremlin-api.md)]

下表包含適用於 Azure Cosmos DB 的常用 Azure PowerShell 指令碼連結。 使用右側的連結瀏覽至 API 特定範例。 所有 API 的通用範例皆相同。 您可以在 [Azure PowerShell 參考](/powershell/module/az.cosmosdb)中取得所有 Azure Cosmos DB PowerShell Cmdlet 的參考頁面。 `Az.CosmosDB`模組現在是模組的一部分 `Az` 。 [下載並安裝](/powershell/azure/install-az-ps?preserve-view=true&view=azps-5.4.0) 最新版本的 Az 模組，以取得 Azure Cosmos DB Cmdlet。 您也可以從 [PowerShell 資源庫](https://www.powershellgallery.com/packages/Az/5.4.0)取得最新版本。 您也可以從 GitHub 存放庫 ([GitHub 上的 Cosmos DB PowerShell 範例](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb)) 的 Cosmos DB 派生這些 PowerShell 範例 。

## <a name="common-samples"></a>通用範例

|Task | 描述 |
|---|---|
|[更新帳戶](scripts/powershell/common/account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| 更新 Cosmos DB 帳戶的預設一致性層級。 |
|[更新帳戶的區域](scripts/powershell/common/update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| 更新 Cosmos DB 帳戶的區域。 |
|[變更容錯移轉優先順序或觸發容錯移轉](scripts/powershell/common/failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| 變更 Azure Cosmos 帳戶的區域容錯移轉優先順序或觸發手動容錯移轉。 |
|[帳戶金鑰或連接字串](scripts/powershell/common/keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| 取得主要和次要金鑰、連接字串，或重新產生 Azure Cosmos DB 帳戶的帳戶金鑰。 |
|[建立具有 IP 防火牆的 Cosmos 帳戶](scripts/powershell/common/firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| 建立已啟用 IP 防火牆的 Azure Cosmos DB 帳戶。 |
|||

## <a name="gremlin-api-samples"></a>Gremlin API 範例

|Task | 描述 |
|---|---|
|[建立帳戶、資料庫和圖表](scripts/powershell/gremlin/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| 建立 Azure Cosmos 帳戶、資料庫和圖表。 |
|[建立具有自動調整功能的帳戶、資料庫和圖表](scripts/powershell/gremlin/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| 建立具有自動調整功能的 Azure Cosmos 帳戶、資料庫和圖表。 |
|[列出或取得資料庫或圖表](scripts/powershell/gremlin/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| 列出或取得資料庫或圖表。 |
|[輸送量作業](scripts/powershell/gremlin/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| 資料庫或圖表的輸送量作業，包括自動調整和標準輸送量之間的取得、更新和遷移。 |
|[鎖定資源避免刪除](scripts/powershell/gremlin/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| 使用資源鎖定避免刪除資源。 |
|||
