---
title: Azure 備份的虛刪除
description: 瞭解如何使用 Azure 備份中的安全性功能，讓備份更安全。
ms.topic: conceptual
ms.date: 04/30/2020
ms.openlocfilehash: da473b1d886ec2fe95a7baae76b09aff38fb3cd7
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/22/2020
ms.locfileid: "95254018"
---
# <a name="soft-delete-for-azure-backup"></a>Azure 備份的虛刪除

現在越來越重視安全性問題，例如惡意程式碼、勒索軟體和入侵。 這些安全性問題在成本和資料方面付出的代價很高。 為了防範這類攻擊，Azure 備份現在提供安全性功能來協助保護備份資料，即使在刪除後也一樣。

其中一項功能是虛刪除。 使用虛刪除時，即使惡意執行者刪除備份 (或不小心刪除備份資料) ，備份資料仍會保留14天，以允許復原該備份專案而不會遺失資料。 在「虛刪除」狀態中，備份資料保留的額外14天不會產生任何費用。

這些服務可使用虛刪除保護：

- [Azure 虛擬機器的虛刪除](soft-delete-virtual-machines.md)
- [Azure VM 中 SQL server 的虛刪除，以及 Azure VM 工作負載中 SAP Hana 的虛刪除](soft-delete-sql-saphana-in-azure-vm.md)

此流程圖顯示啟用虛刪除時，備份專案的不同步驟和狀態：

![虛刪除的備份專案生命週期](./media/backup-azure-security-feature-cloud/lifecycle.png)

## <a name="enabling-and-disabling-soft-delete"></a>啟用和停用虛刪除

新建立的保存庫預設會啟用虛刪除，以防止備份資料遭到意外或惡意刪除。  不建議停用此功能。 如果您打算將受保護的專案移至新的保存庫，而且在刪除和重新保護 (（例如在測試環境中）之前，您應該考慮停用虛刪除，則唯一的情況是您應考慮停用虛刪除 ) 。只有保存庫擁有者可以停用這項功能。 如果您停用此功能，所有未來的受保護專案刪除都會立即移除，而不需要還原。 在停用此功能之前，以虛刪除狀態存在的備份資料，將會在14天的期間內保持為虛刪除狀態。 如果您想要立即永久刪除這些專案，您需要取消刪除，然後再次刪除它們，以永久刪除。

請務必記住，一旦停用虛刪除，就會針對所有類型的工作負載停用此功能。 例如，您不可能只針對 SQL server 或 SAP Hana Db 停用虛刪除，同時讓相同保存庫中的虛擬機器保持啟用狀態。 您可以建立不同的保存庫以進行細微的控制。

### <a name="disabling-soft-delete-using-azure-portal"></a>使用 Azure 入口網站停用虛刪除

若要停用虛刪除，請遵循下列步驟：

1. 在 Azure 入口網站中，移至您的保存庫，然後移至 [**設定**  ->  **屬性**]。
2. 在 [屬性] 窗格中，選取 [**安全性設定**  ->  **更新**]。  
3. 在 [安全性設定] 窗格的 [虛 **刪除**] 下，選取 [ **停** 用]。

![停用虛刪除](./media/backup-azure-security-feature-cloud/disable-soft-delete.png)

### <a name="disabling-soft-delete-using-azure-powershell"></a>使用 Azure PowerShell 停用虛刪除

> [!IMPORTANT]
> 使用 Azure PowerShell 的虛刪除所需的 Az. Az.recoveryservices 版本最少2.2.0。 使用 ```Install-Module -Name Az.RecoveryServices -Force``` 取得最新版本。

若要停用，請使用 [AzRecoveryServicesVaultBackupProperty](/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty) PowerShell Cmdlet。

```powershell
Set-AzRecoveryServicesVaultProperty -VaultId $myVaultID -SoftDeleteFeatureState Disable


StorageModelType       :
StorageType            :
StorageTypeState       :
EnhancedSecurityState  : Enabled
SoftDeleteFeatureState : Disabled
```

### <a name="disabling-soft-delete-using-rest-api"></a>使用 REST API 停用虛刪除

若要使用 REST API 停用虛刪除功能，請參閱 [這裡](use-restapi-update-vault-properties.md#update-soft-delete-state-using-rest-api)所述的步驟。

## <a name="permanently-deleting-soft-deleted-backup-items"></a>永久刪除已虛刪除的備份專案

在停用此功能之前，以虛刪除狀態備份資料，將會維持在虛刪除狀態。 如果您想要立即永久刪除這些專案，請將其取消刪除並再次刪除，以永久刪除。

### <a name="using-azure-portal"></a>使用 Azure 入口網站

依照下列步驟執行：

1. 依照步驟來停用虛 [刪除](#enabling-and-disabling-soft-delete)。

2. 在 Azure 入口網站中，移至您的保存庫，移至 [ **備份專案**]，然後選擇 [虛刪除] 專案。

   ![選擇虛刪除的專案](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

3. 選取 [取消 **刪除**] 選項。

   ![選擇取消刪除](./media/backup-azure-security-feature-cloud/choose-undelete.png)

4. 視窗隨即出現。 選取 [取消 **刪除**]。

   ![選取取消刪除](./media/backup-azure-security-feature-cloud/undelete-vm.png)

5. 選擇 [ **刪除備份資料** ] 以永久刪除備份資料。

   ![選擇 [刪除備份資料]](./media/backup-azure-manage-vms/delete-backup-button.png)

6. 輸入備份專案的名稱，以確認您想要刪除復原點。

   ![輸入備份專案的名稱](./media/backup-azure-manage-vms/delete-backup-data.png)

7. 若要刪除專案的備份資料，請選取 [ **刪除**]。 通知訊息可讓您知道備份資料已刪除。

### <a name="using-azure-powershell"></a>使用 Azure PowerShell

如果在停用虛刪除之前刪除了專案，則這些專案將會處於虛刪除的狀態。 若要立即刪除它們，必須反轉刪除作業，然後再次執行。

識別處於虛刪除狀態的專案。

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

然後反轉啟用虛刪除時所執行的刪除作業。

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

由於虛刪除現在已停用，因此刪除作業會立即移除備份資料。

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

### <a name="using-rest-api"></a>使用 REST API

如果在停用虛刪除之前刪除了專案，則這些專案將會處於虛刪除的狀態。 若要立即刪除它們，必須反轉刪除作業，然後再次執行。

1. 首先，請使用 [此處](backup-azure-arm-userestapi-backupazurevms.md#undo-the-deletion)所述的步驟來復原刪除作業。
2. 然後使用 [此處](use-restapi-update-vault-properties.md#update-soft-delete-state-using-rest-api)所述的步驟，使用 REST API 停用虛刪除功能。
3. 然後使用 [此處](backup-azure-arm-userestapi-backupazurevms.md#stop-protection-and-delete-data)所述的 REST API 來刪除備份。

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="do-i-need-to-enable-the-soft-delete-feature-on-every-vault"></a>我是否需要在每個保存庫上啟用虛刪除功能？

否，其為所有復原服務保存庫預設為內建和啟用。

### <a name="can-i-configure-the-number-of-days-for-which-my-data-will-be-retained-in-soft-deleted-state-after-the-delete-operation-is-complete"></a>我可以設定在刪除作業完成後，我的資料將保留在虛刪除狀態的天數嗎？

否，在刪除作業之後，它會固定為額外保留14天。

### <a name="do-i-need-to-pay-the-cost-for-this-additional-14-day-retention"></a>我是否需要支付額外的14天保留成本？

否，這14天的額外保留期在虛刪除功能中是免費的。

### <a name="can-i-perform-a-restore-operation-when-my-data-is-in-soft-delete-state"></a>當我的資料處於「虛刪除」狀態時，是否可以執行還原作業？

否，您需要取消刪除已虛刪除的資源，才能進行還原。 取消刪除作業會將資源帶回「 **停止保護」，並保留資料狀態** ，讓您可以還原至任何時間點。 垃圾收集行程會在此狀態下保持暫停狀態。

### <a name="will-my-snapshots-follow-the-same-lifecycle-as-my-recovery-points-in-the-vault"></a>我的快照集會跟保存庫中的復原點是否遵循相同的生命週期？

是。

### <a name="how-can-i-trigger-the-scheduled-backups-again-for-a-soft-deleted-resource"></a>如何針對虛刪除的資源再次觸發排程備份？

之後再繼續操作將會再次保護資源。 Resume 作業會建立備份原則的關聯，以觸發已選取保留期限的排程備份。 此外，垃圾收集行程會在繼續操作完成時立即執行。 如果您想要從超過其到期日的復原點執行還原，建議您在觸發繼續操作之前執行此作業。

### <a name="can-i-delete-my-vault-if-there-are-soft-deleted-items-in-the-vault"></a>如果保存庫中有虛刪除的專案，我是否可以刪除保存庫？

如果保存庫中有處於虛刪除狀態的備份專案，則無法刪除復原服務保存庫。 虛刪除的專案會在刪除作業的14天內永久刪除。 如果您無法等候14天，請停用虛 [刪除](#enabling-and-disabling-soft-delete)、取消刪除已虛刪除的專案，然後重新刪除它們以永久刪除。 確定沒有受保護的專案且沒有虛刪除的專案之後，就可以刪除該保存庫。  

### <a name="can-i-delete-the-data-earlier-than-the-14-days-soft-delete-period-after-deletion"></a>是否可以在刪除後的14天內刪除超過14天的資料？

否。 您無法強制刪除虛刪除的專案。 它們會在14天后自動刪除。 啟用這項安全性功能，以保護備份資料免于遭到意外或惡意刪除。  您應等候14天，然後再對專案執行任何其他動作。  虛刪除的專案將不會收費。  如果您需要在新保存庫的14天內重新保護標示為虛刪除的專案，請洽詢 Microsoft 支援服務。

### <a name="can-soft-delete-operations-be-performed-in-powershell-or-cli"></a>是否可以在 PowerShell 或 CLI 中執行虛刪除作業？

您可以使用 PowerShell 來執行虛刪除作業。 目前不支援 CLI。

## <a name="next-steps"></a>後續步驟

- [Azure 備份中的安全性功能概觀](security-overview.md)
