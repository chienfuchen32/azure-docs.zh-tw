---
title: Azure CLI 指令碼 - 調整和監視適用於 PostgreSQL 的 Azure 資料庫
description: Azure CLI 指令碼範例 - 在查詢計量之後，將「適用於 PostgreSQL 的 Azure 資料庫」伺服器調整為不同的效能等級。
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.devlang: azurecli
ms.custom: mvc, devx-track-azurecli
ms.topic: sample
ms.date: 08/07/2019
ms.openlocfilehash: 6bbf5f3a0a7d32425f80687de10444ee0819b9df
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94660452"
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>使用 Azure CLI 監視和調整單一 PostgreSQL 伺服器
此範例 CLI 指令碼會在查詢計量之後，對單一「適用於 PostgreSQL 的 Azure 資料庫」伺服器調整計算和儲存。 計算可以擴大或縮小。 儲存體只能擴大。 

> [!IMPORTANT] 
> 儲存體只能擴大，而不能縮小。

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- 本文需要 2.0 版或更新版本的 Azure CLI。 如果您是使用 Azure Cloud Shell，就已安裝最新版本。

## <a name="sample-script"></a>範例指令碼
使用您的訂用帳戶識別碼來更新指令碼。
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>清除部署
執行指令碼之後，請使用下列命令來移除資源群組及其相關聯的所有資源。 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>指令碼說明
此指令碼會使用下表中簡述的命令：

| **命令** | **注意事項** |
|---|---|
| [az group create](/cli/azure/group) | 建立用來存放所有資源的資源群組。 |
| [az postgres server create](/cli/azure/postgres/server#az-postgres-server-create) | 建立主控資料庫的 PostgreSQL 伺服器。 |
| [az postgres server update](/cli/azure/postgres/server#az-postgres-server-update) | 更新 PostgreSQL 伺服器的屬性。 |
| [az monitor metrics list](/cli/azure/monitor/metrics) | 列出資源的度量值。 |
| [az group delete](/cli/azure/group) | 刪除資源群組，包括所有的巢狀資源。 |

## <a name="next-steps"></a>後續步驟
- 深入了解[適用於 PostgreSQL 的 Azure 資料庫的計算和儲存](../concepts-pricing-tiers.md)
- 嘗試額外的指令碼：[Azure Database for PostgreSQL 的 Azuer CLI 範例](../sample-scripts-azure-cli.md)
- 深入了解 [Azure CLI](/cli/azure)
