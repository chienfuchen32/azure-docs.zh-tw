---
title: 修復分區對應問題的復原管理員
description: 使用 RecoveryManager 類別來解決分區對應的問題
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 01/03/2019
ms.openlocfilehash: 91bcd998849c619a328a198c97bb8c977b9d8232
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/28/2020
ms.locfileid: "92792220"
---
# <a name="using-the-recoverymanager-class-to-fix-shard-map-problems"></a>使用 RecoveryManager 類別來修正分區對應問題
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

[RecoveryManager](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager)類別提供 ADO.NET 的應用程式，讓您能夠輕鬆地偵測全域分區對應 (GSM) 和分區化資料庫環境中的本機分區對應 (LSM) 之間的任何不一致情況。

GSM 和 LSM 會追蹤分區化環境中每個資料庫的對應。 但偶爾 GSM 和 LSM 之間會發生中斷的情況。 此時，請使用 RecoveryManager 類別來偵測並修復中斷的問題。

RecoveryManager 類別是 [彈性資料庫用戶端程式庫](elastic-database-client-library.md)的一部分。

![分區對應][1]

關於詞彙定義，請參閱 [彈性資料庫工具字彙](elastic-scale-glossary.md)。 若要了解 **ShardMapManager** 如何用來管理分區化解決方案中的資料，請參閱 [分區對應管理](elastic-scale-shard-map-management.md)。

## <a name="why-use-the-recovery-manager"></a>為何使用復原管理員

在分區化資料庫環境中，每個資料庫有一個租用戶，而每個伺服器中有許多資料庫。 環境中也可能會有許多伺服器。 每個資料庫都會在分區對應中對應，以便呼叫可以路由至正確的伺服器和資料庫。 根據 **分區化索引鍵** 追蹤資料庫，而每個分區會被指派 **某個範圍的索引鍵值** 。 例如，分區化索引鍵可能代表客戶名稱從 "D" 到 "F"。 所有分區 (也稱為資料庫) 和其對應範圍的對應，都包含在 **全域分區對應 (GSM)** 中。 每個資料庫也包含分區上所包含之範圍的對應 (稱為 **本機分區對應 (LSM)** )。 當應用程式連接到分區時，會隨著應用程式快取對應以供快速擷取。 LSM 用來驗證快取的資料。

GSM 和 LSM 可能因為以下原因變成不同步：

1. 刪除其範圍被認為不再使用中的分區，或重新命名分區。 刪除分區會導致 **被遺棄的分區對應** 。 同樣地，重新命名的資料庫可能造成被遺棄的分區對應。 根據變更的目的而定，可能需要移除分區或更新分區位置。 若要復原已刪除的資料庫，請參閱[還原已刪除的資料庫](recovery-using-backups.md)。
2. 發生異地備援容錯移轉事件。 若要繼續，您必須在應用程式中更新分區對應管理員的伺服器名稱和資料庫名稱，然後更新分區對應中所有分區的分區對應詳細資料。 如果有異地容錯移轉，這類復原邏輯應該在容錯移轉工作流程內自動執行。 自動化修復動作可為異地備援的資料庫啟用順暢的管理能力，並避免人工的動作。 若要瞭解在資料中心中斷時復原資料庫的選項，請參閱 [商務持續性](business-continuity-high-availability-disaster-recover-hadr-overview.md) 和嚴重損壞 [修復](disaster-recovery-guidance.md)。
3. 分區或 ShardMapManager 資料庫會還原到較早的時間點。 若要了解如何使用備份進行時間點復原，請參閱[使用備份進行復原](recovery-using-backups.md)。

如需有關 Azure SQL Database 彈性資料庫工具、異地複寫及還原的詳細資訊，請參閱下列連結：

* [總覽：利用 SQL Database 的雲端商務持續性和資料庫嚴重損壞修復](business-continuity-high-availability-disaster-recover-hadr-overview.md)
* [開始使用彈性資料庫工具](elastic-scale-get-started.md)  
* [ShardMap 管理](elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>從 ShardMapManager 擷取 RecoveryManager

第一個步驟是建立 RecoveryManager 執行個體。 [GetRecoveryManager 方法](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager)會傳回目前的 [ShardMapManager](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager) 執行個體的復原管理員。 若要解決分區對應中的任何不一致，您必須先擷取特定分區對應的 RecoveryManager。

   ```java
    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager();
   ```

在此範例中，RecoveryManager 是從 ShardMapManager 初始化。 ShardMapManager 包含也已經初始化的 ShardMap。

由於此應用程式程式碼會操作分區對應本身，因此在 Factory 方法中使用的認證 (在上述範例中為 smmConnectionString) 應該是在連接字串所參考的 GSM 資料庫上具備讀寫權限的認證。 這些認證通常不同於用來對資料獨立路由開啟連接的認證。 如需詳細資訊，請參閱 [在彈性資料庫用戶端中使用認證](elastic-scale-manage-credentials.md)。

## <a name="removing-a-shard-from-the-shardmap-after-a-shard-is-deleted"></a>刪除分區之後從 ShardMap 移除分區

[DetachShard 方法](/previous-versions/azure/dn842083(v=azure.100)) 會從給定的分區卸離分區對應，並刪除與分區相關聯的對應。  

* location 參數是分區位置，特別是要卸離的分區的伺服器名稱和資料庫名稱。
* shardMapName 參數是分區對應名稱。 只有在多個分區對應是由相同的分區對應管理員管理時才為必要。 選擇性。

> [!IMPORTANT]
> 只有在您確定更新對應的範圍是空白時，才可使用這項技術。 上述方法並不會檢查要移動的資料範圍，因此您最好在程式碼中納入檢查。

這個範例會從分區對應中移除分區。

   ```java
   rm.DetachShard(s.Location, customerMap);
   ```

此分區對應會反映刪除分區之前，分區在 GSM 中的位置。 因為已刪除分區，會假設這是特意的，而且分區化索引鍵範圍已不再使用中。 如果情況並非如此，您可以執行時間點還原， 從較早的時間點復原分區。  (在這種情況下，請參閱下一節來偵測分區不一致的情況。 ) 若要復原，請參閱 [時間點復原](recovery-using-backups.md)。

由於假設刪除資料庫是在預期中，最終的系統管理清除動作是刪除分區對應管理員中分區的項目。 這可避免應用程式不小心將資訊寫入至未預期的範圍。

## <a name="to-detect-mapping-differences"></a>偵測對應的差異

[DetectMappingDifferences 方法](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences) 可選取並傳回其中一個分區對應 (本機或全域) 做為真實來源，並調解兩個分區對應 (GSM 和 LSM) 上的對應。

   ```java
   rm.DetectMappingDifferences(location, shardMapName);
   ```

* *location* 指定伺服器名稱和資料庫名稱。
* *ShardMapName* 參數是分區對應名稱。 只有在多個分區對應是由相同的分區對應管理員管理時才為必要。 選擇性。

## <a name="to-resolve-mapping-differences"></a>解決對應的差異

[ResolveMappingDifferences 方法](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences) 可選取其中一個分區對應 (本機或全域) 做為真實來源，並調解兩個分區對應 (GSM 和 LSM) 上的對應。

   ```java
   ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   ```

* *RecoveryToken* 參數會列舉特定分區的 GSM 與 LSM 之間對應的差異。
* [MappingDifferenceResolution 列舉](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution) 用來指出用於解析分區對應之間差異的方法。
* 當 LSM 包含正確的對應因而應該使用分區中的對應時，建議使用 **MappingDifferenceResolution.KeepShardMapping** 。 當發生容錯移轉時，通常會是這種情況：分區現在位於新的伺服器上。 由於必須先從 GSM 中移除分區 (使用 RecoveryManager.DetachShard 方法)，GSM 上不再存在對應。 因此，必須使用 LSM 來重新建立分區對應。

## <a name="attach-a-shard-to-the-shardmap-after-a-shard-is-restored"></a>在還原分區之後將分區附加至 ShardMap

[AttachShard 方法](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard) 會將給定的分區附加至分區對應。 然後它會偵測任何分區對應不一致，並更新對應以符合分區還原時間點的分區。 系統會假設資料庫也重新命名來反映原始資料庫名稱 (還原分區之前)，因為時間點還原預設是採用新資料庫再加上時間戳記。

   ```java
   rm.AttachShard(location, shardMapName)
   ```

* *location* 參數是要附加的分區的伺服器名稱和資料庫名稱。
* *ShardMapName* 參數是分區對應名稱。 只有在多個分區對應是由相同的分區對應管理員管理時才為必要。 選擇性。

此範例會將分區新增到最近從較早時間點還原的分區對應。 因為已還原分區 (也就是 LSM 中的分區對應)，該分區可能會與 GSM 中的分區項目不一致。 在這個範例程式碼之外，分區已還原並重新命名為資料庫的原始名稱。 因為它已還原，就會假設 LSM 中的對應為受信任的對應。

   ```java
   rm.AttachShard(s.Location, customerMap);
   var gs = rm.DetectMappingDifferences(s.Location);
      foreach (RecoveryToken g in gs)
       {
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping);
       }
   ```

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-the-shards"></a>在分區的異地複寫容錯移轉 (還原) 之後更新分區位置

如果發生異地容錯移轉，次要資料庫會變成可供寫入存取，並成為新的主要資料庫。 伺服器的名稱和可能的資料庫 (根據您的設定而定)，可能會將原始主要複本的不同。 因此，必須修正 GSM 和 LSM 分區的對應項目。 同樣地，如果資料庫還原至不同的名稱或位置，或到較早的時間點，這可能會在分區對應中造成不一致。 分區對應管理員會處理開啟連接到正確資料庫的散發。 分配時，會根據分區對應中的資料和作為應用程式要求目標之分區化金鑰的值，進行分配。 異地複寫容錯移轉之後，必須以正確的伺服器名稱、資料庫名稱和修復資料庫的分區對應更新這項資訊。

## <a name="best-practices"></a>最佳作法

地理容錯移轉和復原通常是由應用程式的雲端系統管理員刻意管理的作業，刻意利用 Azure SQL Database 的商務持續性功能。 商務持續性計劃需要處理程序、程序和措施以確保商務運作能持續而不會中斷。 應該在此工作流程中使用隨著 RecoveryManager 類別提供的方法，以確保根據採取的修復動作，GSM 和 LSM 都處於最新狀態。 在 5 個基本步驟可正確確保 GSM 和 LSM 在容錯移轉事件之後反映正確的資訊。 執行這些步驟的應用程式程式碼可以整合至現有的工具和工作流程。

1. 從 ShardMapManager 擷取 RecoveryManager。
2. 從分區對應卸離舊分區。
3. 將新的分區附加至分區對應，包括新的分區位置。
4. 在 GSM 和 LSM 之間的對應中偵測到不一致。
5. 解決 GSM 和 LSM 之間的差異，信任 LSM。

此範例會執行下列步驟：

1. 從「分區對應」中移除反映容錯移轉事件之前分區位置的分區。
2. 將分區附加至分區對應會反映新分區位置 (參數 "Configuration.SecondaryServer" 是是新伺服器名稱，但是相同的資料庫名稱)。
3. 透過偵測每個分區的 GSM 與 LSM 之間的對應差異來擷取復原權杖。
4. 透過信任來自每個分區 LSM 的對應，即可解決不一致情形。

   ```java
   var shards = smm.GetShards();
   foreach (shard s in shards)
   {
     if (s.Location.Server == Configuration.PrimaryServer)
         {
          ShardLocation slNew = new ShardLocation(Configuration.SecondaryServer, s.Location.Database);
          rm.DetachShard(s.Location);
          rm.AttachShard(slNew);
          var gs = rm.DetectMappingDifferences(slNew);
          foreach (RecoveryToken g in gs)
            {
               rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping);
            }
        }
    }
   ```

[!INCLUDE [elastic-scale-include](../../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/elastic-database-recovery-manager/recovery-manager.png