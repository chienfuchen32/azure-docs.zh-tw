---
title: DTU 資源限制彈性集區
description: 此頁面描述 Azure SQL Database 中彈性集區的一些常見 DTU 資源限制。
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: seo-lt-2019 sqldbrb=1 references_regions
ms.devlang: ''
ms.topic: reference
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: sstein
ms.date: 07/28/2020
ms.openlocfilehash: d87c5d162b96209c0ce3d3276dc518f42373590f
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/28/2020
ms.locfileid: "92780807"
---
# <a name="resources-limits-for-elastic-pools-using-the-dtu-purchasing-model"></a>使用 DTU 購買模型的彈性集區資源限制
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

本文針對使用 DTU 購買模型的彈性集區內 Azure SQL Database 中的資料庫，提供詳細的資源限制。

* 如需 Azure SQL Database 的 DTU 購買模型資源限制，請參閱 [dtu 資源限制-Azure SQL Database](resource-limits-dtu-single-databases.md)。
* 如需 vCore 資源限制，請參閱 [vCore 資源限制-Azure SQL Database](resource-limits-vcore-single-databases.md) 和 [vCore 資源限制-彈性](resource-limits-vcore-elastic-pools.md)集區。

## <a name="elastic-pool-storage-sizes-and-compute-sizes"></a>彈性集區：儲存體大小與計算大小

針對 Azure SQL Database 彈性集區，下表顯示每個服務層級和計算大小的可用資源。 您可以使用下列內容來設定服務層、計算大小和儲存體數量：

* [Azure 入口網站](elastic-pool-manage.md#azure-portal)
* [PowerShell](elastic-pool-manage.md#powershell)
* [Azure CLI](elastic-pool-manage.md#azure-cli)
* [REST API](elastic-pool-manage.md#rest-api)。

> [!IMPORTANT]
> 如需調整指導方針和考慮，請參閱[調整彈性集](elastic-pool-scale.md)區

根據 DTU 和服務層級，彈性集區中個別資料庫的資源限制通常與集區外部之單一資料庫的資源限制相同。 例如，S2 資料庫的並行背景工作數上限是 120 個背景工作。 因此，如果集區中每個資料庫的最大 DTU 是 50 DTU (這相當於 S2)，標準集區中的資料庫最大並行背景工作數上限也會是 120 個背景工作。
 
針對相同數量的 Dtu，提供給彈性集區的資源可能會超過提供給彈性集區外部之單一資料庫的資源。 這表示，彈性集區的 eDTU 使用率可能小於集區內的 DTU 使用率總和，視工作負載模式而定。 例如，在極端情況下，在資料庫 DTU 使用率為100% 的彈性集區中，只有一個資料庫，可能會有特定工作負載模式的集區 eDTU 使用率為50%。 即使每個資料庫的最大 DTU 都維持在指定集區大小的最大支援值，也可能會發生這種情況。

> [!NOTE]
> 下表中每個集區資源限制的儲存體不包含 tempdb 和記錄儲存體。

### <a name="basic-elastic-pool-limits"></a>基本彈性集區限制

| 每集區 eDTU | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| 每個集區內含的儲存體 (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| 每集區最大儲存體 (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| 每個集區的記憶體內部 OLTP 儲存體上限 (GB) | N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A |
| 每個集區的資料庫數目上限 <sup>1</sup> | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| 每個集區<sup>2</sup>的並行背景工作 (要求數上限)  | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| 每集區<sup>2</sup>的並行會話數上限 | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| 每個資料庫選項的最小 DTU | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| 每個資料庫選項的最大 DTU | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| 每個資料庫的儲存體上限 (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
||||||||

<sup>1</sup> 如需其他考慮，請參閱 [密集彈性集區中的資源管理](elastic-pool-resource-management.md) 。

<sup>2</sup> 對於任何個別資料庫，最大並行背景工作 (要求) ，請參閱 [單一資料庫資源限制](resource-limits-vcore-single-databases.md)。 例如，如果彈性集區使用第5代，且每個資料庫的最大 vCore 設定為2，則最大並行背景工作角色值為200。  如果 [每個資料庫的 vCore 上限] 設定為0.5，則 [最大並行背景工作角色] 值為50，因為在第5代上，每個 vCore 最多可有100個並行背景工作角色。 對於少於 1 個 V 核心的每個資料庫 V 核心最大數量的其他設定，並行背景工作角色的最大數目也是同樣地重新調整。

### <a name="standard-elastic-pool-limits"></a>標準彈性集區限制

| 每集區 eDTU | **50** | **100** | **200** | **300** | **400** | **800**|
|:---|---:|---:|---:| ---: | ---: | ---: |
| 每個集區的內含儲存體 (GB) <sup>1</sup> | 50 | 100 | 200 | 300 | 400 | 800 |
| 每集區最大儲存體 (GB) | 500 | 750 | 1024 | 1280 | 1536 | 2048 |
| 每個集區的記憶體內部 OLTP 儲存體上限 (GB) | N/A | N/A | N/A | N/A | N/A | N/A |
| 每個集區<sup>2</sup>的資料庫數目上限 | 100 | 200 | 500 | 500 | 500 | 500 |
| 每個集區的最大並行背景工作 (要求) <sup>3</sup> | 100 | 200 | 400 | 600 | 800 | 1600 |
| 每個集區的並行會話數目上限 <sup>3</sup> | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| 每個資料庫選項的最小 DTU | 0, 10, 20, 50 | 0、10、20、50、100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| 每個資料庫選項的最大 DTU | 10, 20, 50 | 10、20、50、100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 |
| 每個資料庫的儲存體上限 (GB) | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

<sup>1</sup> 如需因布建任何額外的儲存空間而產生的額外成本詳細資訊，請參閱 [SQL Database 定價選項](https://azure.microsoft.com/pricing/details/sql-database/elastic/) 。

<sup>2</sup> 如需其他考慮，請參閱 [密集彈性集區中的資源管理](elastic-pool-resource-management.md) 。

<sup>3</sup> 針對任何個別資料庫的並行背景工作數上限 (要求) ，請參閱 [單一資料庫資源限制](resource-limits-vcore-single-databases.md)。 例如，如果彈性集區使用第5代，且每個資料庫的最大 vCore 設定為2，則最大並行背景工作角色值為200。  如果 [每個資料庫的 vCore 上限] 設定為0.5，則 [最大並行背景工作角色] 值為50，因為在第5代上，每個 vCore 最多可有100個並行背景工作角色。 對於少於 1 個 V 核心的每個資料庫 V 核心最大數量的其他設定，並行背景工作角色的最大數目也是同樣地重新調整。

### <a name="standard-elastic-pool-limits-continued"></a>標準彈性集區限制 (續)

| 每集區 eDTU | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| 每個集區的內含儲存體 (GB) <sup>1</sup> | 1200 | 1600 | 2000 | 2500 | 3000 |
| 每集區最大儲存體 (GB) | 2560 | 3072 | 3584 | 4096 | 4096 |
| 每個集區的記憶體內部 OLTP 儲存體上限 (GB) | N/A | N/A | N/A | N/A | N/A |
| 每個集區<sup>2</sup>的資料庫數目上限 | 500 | 500 | 500 | 500 | 500 |
| 每個集區的最大並行背景工作 (要求) <sup>3</sup> | 2400 | 3200 | 4000 | 5000 | 6000 |
| 每個集區的並行會話數目上限 <sup>3</sup> | 30000 | 30000 | 30000 | 30000 | 30000 |
| 每個資料庫選項的最小 DTU | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| 每個資料庫選項的最大 DTU | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| 每個資料庫的儲存體上限 (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> 如需因布建任何額外的儲存空間而產生的額外成本詳細資訊，請參閱 [SQL Database 定價選項](https://azure.microsoft.com/pricing/details/sql-database/elastic/) 。

<sup>2</sup> 如需其他考慮，請參閱 [密集彈性集區中的資源管理](elastic-pool-resource-management.md) 。

<sup>3</sup> 針對任何個別資料庫的並行背景工作數上限 (要求) ，請參閱 [單一資料庫資源限制](resource-limits-vcore-single-databases.md)。 例如，如果彈性集區使用第5代，且每個資料庫的最大 vCore 設定為2，則最大並行背景工作角色值為200。  如果 [每個資料庫的 vCore 上限] 設定為0.5，則 [最大並行背景工作角色] 值為50，因為在第5代上，每個 vCore 最多可有100個並行背景工作角色。 對於少於 1 個 V 核心的每個資料庫 V 核心最大數量的其他設定，並行背景工作角色的最大數目也是同樣地重新調整。

### <a name="premium-elastic-pool-limits"></a>高階彈性集區限制

| 每集區 eDTU | **125** | **250** | **500** | **1000** | **1500**|
|:---|---:|---:|---:| ---: | ---: |
| 每個集區的內含儲存體 (GB) <sup>1</sup> | 250 | 500 | 750 | 1024 | 1536 |
| 每集區最大儲存體 (GB) | 1024 | 1024 | 1024 | 1024 | 1536 |
| 每個集區的記憶體內部 OLTP 儲存體上限 (GB) | 1 | 2 | 4 | 10 | 12 |
| 每個集區<sup>2</sup>的資料庫數目上限 | 50 | 100 | 100 | 100 | 100 |
| 每個集區的並行背景工作數上限 (要求) <sup>3</sup> | 200 | 400 | 800 | 1600 | 2400 |
| 每個集區的並行會話數目上限 <sup>3</sup> | 30000 | 30000 | 30000 | 30000 | 30000 |
| 每資料庫的 eDTU 下限 | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000|
| 每資料庫的 eDTU 上限 | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000|
| 每個資料庫的儲存體上限 (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> 如需因布建任何額外的儲存空間而產生的額外成本詳細資訊，請參閱 [SQL Database 定價選項](https://azure.microsoft.com/pricing/details/sql-database/elastic/) 。

<sup>2</sup> 如需其他考慮，請參閱 [密集彈性集區中的資源管理](elastic-pool-resource-management.md) 。

<sup>3</sup> 針對任何個別資料庫的並行背景工作數上限 (要求) ，請參閱 [單一資料庫資源限制](resource-limits-vcore-single-databases.md)。 例如，如果彈性集區使用第5代，且每個資料庫的最大 vCore 設定為2，則最大並行背景工作角色值為200。  如果 [每個資料庫的 vCore 上限] 設定為0.5，則 [最大並行背景工作角色] 值為50，因為在第5代上，每個 vCore 最多可有100個並行背景工作角色。 對於少於 1 個 V 核心的每個資料庫 V 核心最大數量的其他設定，並行背景工作角色的最大數目也是同樣地重新調整。

### <a name="premium-elastic-pool-limits-continued"></a>高階彈性集區限制 (續)

| 每集區 eDTU | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: |
| 每個集區的內含儲存體 (GB) <sup>1</sup> | 2048 | 2560 | 3072 | 3548 | 4096 |
| 每集區最大儲存體 (GB) | 2048 | 2560 | 3072 | 3548 | 4096|
| 每個集區的記憶體內部 OLTP 儲存體上限 (GB) | 16 | 20 | 24 | 28 | 32 |
| 每個集區<sup>2</sup>的資料庫數目上限 | 100 | 100 | 100 | 100 | 100 |
| 每個集區的最大並行背景工作 (要求) <sup>3</sup> | 3200 | 4000 | 4800 | 5600 | 6400 |
| 每個集區的並行會話數目上限 <sup>3</sup> | 30000 | 30000 | 30000 | 30000 | 30000 |
| 每個資料庫選項的最小 DTU | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 |
| 每個資料庫選項的最大 DTU | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 |
| 每個資料庫的儲存體上限 (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> 如需因布建任何額外的儲存空間而產生的額外成本詳細資訊，請參閱 [SQL Database 定價選項](https://azure.microsoft.com/pricing/details/sql-database/elastic/) 。

<sup>2</sup> 如需其他考慮，請參閱 [密集彈性集區中的資源管理](elastic-pool-resource-management.md) 。

<sup>3</sup> 針對任何個別資料庫的並行背景工作數上限 (要求) ，請參閱 [單一資料庫資源限制](resource-limits-vcore-single-databases.md)。 例如，如果彈性集區使用第5代，且每個資料庫的最大 vCore 設定為2，則最大並行背景工作角色值為200。  如果 [每個資料庫的 vCore 上限] 設定為0.5，則 [最大並行背景工作角色] 值為50，因為在第5代上，每個 vCore 最多可有100個並行背景工作角色。 對於少於 1 個 V 核心的每個資料庫 V 核心最大數量的其他設定，並行背景工作角色的最大數目也是同樣地重新調整。

> [!IMPORTANT]
> 所有區域目前均可使用進階層中超過 1 TB 的儲存體，但：中國東部、中國北部、德國中部和德國東北部除外。 在這些區域中，進階層中的儲存空間上限為 1 TB。  如需詳細資訊，請參閱 [P11-P15 目前的限制](single-database-scale.md#p11-and-p15-constraints-when-max-size-greater-than-1-tb)。

如果彈性集區的所有 DTU 均已使用，則集區中的每個資料庫會收到等量的資源以處理查詢。 SQL Database 服務藉由確保運算時間的均等配量，提供資料庫之間的資源共用公平性。 彈性集區資源共用公平性不包括任何資源數量，否則當每個資料庫的最小 DTU 數設為非零的值時，便會對每個資料庫保證資源數量。

> [!NOTE]
> 如需 `tempdb` 限制，請參閱 [tempdb 限制](/sql/relational-databases/databases/tempdb-database?view=sql-server-2017#tempdb-database-in-sql-database)。

### <a name="database-properties-for-pooled-databases"></a>集區資料庫的資料庫屬性

下表描述集區資料庫的屬性。

| 屬性 | 描述 |
|:--- |:--- |
| 每資料庫的 eDTU 上限 |集區中任何資料庫可以使用的 eDTU 數目上限，是否可用則是根據集區中其他資料庫的使用量而定。 每個資料庫的 eDTU 數目上限不等於資料庫的資源保證。 這個設定是全域設定，會套用至集區中的所有資料庫。 將每個資料庫的 eDTU 設定為最上限，以處理資料庫使用率的尖峰。 某種程度的過量使用是可預期的情況，因為集區通常會假設資料庫的熱門和冷門使用模式；在這些模式中，所有資料庫不會同時處於尖峰期。 例如，假設每個資料庫的尖峰使用量是 20 個 DTU，且集區中的 100 個資料庫只有 20% 會同時暴增到尖峰。 如果每一資料庫的 eDTU 上限設為 20 個 eDTU，則以 5 倍的量過量使用集區，並將每集區 eDTU 設為 400 個是合理的作法。 |
| 每資料庫的 eDTU 下限 |集區中單一資料庫能夠保證的最小 eDTU 數。 這個設定是全域設定，會套用至集區中的所有資料庫。 每個資料庫最小 eDTU 建議設定為 0，同時也是預設值。 此屬性會設為 0 到每一資料庫的 eDTU 使用量平均值之間的任意數。 集區中資料庫數目和每個資料庫 eDTU 數目下限的乘積不能超過每個集區的 eDTU。 例如，如果集區有 20 個資料庫，且每個資料庫的最小 eDTU 設定為 10 eDTU，則每個集區 eDTU 必須至少為 200 個 eDTU。 |
| 每個資料庫的儲存體上限 |使用者所設定集區資料庫的資料庫大小上限。 不過，集區資料庫會共用配置的集區儲存體。 即使 *每個資料庫* 的最大儲存空間總計設定為大於集區的可用儲存 *空間* 總計，但所有資料庫實際使用的總空間將無法超過可用的集區限制。 資料庫大小上限是指資料檔案的大小上限，並不包含記錄檔所使用的空間。 |
|||

## <a name="next-steps"></a>後續步驟

* 如需單一資料庫的 vCore 資源限制，請參閱 [使用 vCore 購買模型的單一資料庫資源限制](resource-limits-vcore-single-databases.md)
* 如需單一資料庫的 DTU 資源限制，請參閱 [使用 DTU 購買模型的單一資料庫資源限制](resource-limits-dtu-single-databases.md)
* 如需彈性集區的 vCore 資源限制，請參閱 [使用 vCore 購買模型的彈性集區資源限制](resource-limits-vcore-elastic-pools.md)
* 如需 Azure SQL 受控執行個體中受控實例的資源限制，請參閱 [SQL 受控執行個體資源限制](../managed-instance/resource-limits.md)。
* 如需一般 Azure 限制的相關資訊，請參閱 [Azure 訂用帳戶和服務限制、配額及條件約束](../../azure-resource-manager/management/azure-subscription-service-limits.md)。
* 如需邏輯 SQL server 上資源限制的相關資訊，請參閱 [SQL server 上的資源限制總覽](resource-limits-logical-server.md) ，以取得有關伺服器和訂用帳戶層級之限制的資訊。