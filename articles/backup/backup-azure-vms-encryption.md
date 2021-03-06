---
title: 備份和還原已加密的 Azure Vm
description: 說明如何使用 Azure 備份服務來備份和還原已加密的 Azure Vm。
ms.topic: conceptual
ms.date: 08/18/2020
ms.openlocfilehash: ee7fedffd58ffb9e98f8c412833d151eb1a95530
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2020
ms.locfileid: "96547146"
---
# <a name="back-up-and-restore-encrypted-azure-virtual-machines"></a>備份和還原已加密的 Azure 虛擬機器

本文說明如何使用 [Azure 備份](backup-overview.md) 服務，來備份和還原 Windows 或 Linux Azure 虛擬機器 (vm) 具有加密磁片的 vm。 如需詳細資訊，請參閱 [AZURE VM 備份的加密](backup-azure-vms-introduction.md#encryption-of-azure-vm-backups)。

## <a name="encryption-using-platform-managed-keys"></a>使用平臺管理的金鑰進行加密

根據預設，Vm 中的所有磁片都會使用平臺管理的金鑰自動加密， (使用 [儲存體服務加密](../storage/common/storage-service-encryption.md)的 PMK) 。 您可以使用 Azure 備份來備份這些 Vm，而不需要任何特定的動作，以在您的結尾支援加密。 如需使用平臺管理的金鑰進行加密的詳細資訊， [請參閱這篇文章](../virtual-machines/disk-encryption.md#platform-managed-keys)。

![加密磁碟](./media/backup-encryption/encrypted-disks.png)

## <a name="encryption-using-customer-managed-keys"></a>使用客戶管理的金鑰進行加密

當您使用客戶管理的金鑰來加密磁片時 (CMK) ，用來加密磁片的金鑰會儲存在 Azure Key Vault 中，並由您管理。 儲存體服務加密使用 CMK (SSE) 不同于 Azure 磁碟加密 (ADE) 加密。 ADE 會使用作業系統的加密工具。 SSE 會加密儲存體服務中的資料，讓您可以使用 Vm 的任何 OS 或映射。

您不需要執行任何明確的動作來備份或還原使用客戶管理金鑰來加密其磁片的 Vm。 儲存在保存庫中這些 Vm 的備份資料，會使用與保存 [庫上所使用的加密](encryption-at-rest-with-cmk.md)相同的方法進行加密。

如需使用客戶管理的金鑰來加密受控磁片的詳細資訊，請參閱 [這篇文章](../virtual-machines/disk-encryption.md#customer-managed-keys)。

## <a name="encryption-support-using-ade"></a>使用 ADE 的加密支援

Azure 備份支援以 Azure 磁碟加密 (ADE) 加密其作業系統/資料磁片的 Azure Vm 備份。 ADE 使用 BitLocker 來加密 Windows Vm，並使用適用于 Linux Vm 的 dm crypt 功能。 ADE 會與 Azure Key Vault 整合，以管理磁片加密金鑰和秘密。 Key Vault 金鑰加密金鑰 (Kek) 可以用來新增額外的安全性層級，然後在將加密密碼寫入 Key Vault 之前，先將其加密。

Azure 備份可以使用具有和沒有 Azure AD 應用程式的 ADE 來備份和還原 Azure Vm，如下表所摘要。

**VM 磁碟類型** | **ADE (BEK/dm-crypt)** | **ADE 和 KEK**
--- | --- | ---
**非受控** | 是 | 是
**受控**  | 是 | 是

- 深入瞭解 [ADE](../security/fundamentals/azure-disk-encryption-vms-vmss.md)、 [Key Vault](../key-vault/general/overview.md)和 [kek](../virtual-machine-scale-sets/disk-encryption-key-vault.md#set-up-a-key-encryption-key-kek)。
- 閱讀 Azure VM 磁片加密的 [常見問題](../security/fundamentals/azure-disk-encryption-vms-vmss.md) 。

### <a name="limitations"></a>限制

- 您可以在相同的訂用帳戶和區域內備份和還原已加密的 Vm。
- Azure 備份支援使用獨立金鑰加密的 Vm。 目前不支援任何屬於用於加密 VM 之憑證的金鑰。
- 您可以在與復原服務備份保存庫相同的訂用帳戶和區域中，備份和還原已加密的 Vm。
- 加密的 VM 無法在檔案/資料夾層級復原。 您必須復原整個 VM，才能還原檔案和資料夾。
- 還原 VM 時，您無法使用已加密 Vm 的 [ [取代現有的 VM](backup-azure-arm-restore-vms.md#restore-options) ] 選項。 只有未加密的受控磁片才支援此選項。

## <a name="before-you-start"></a>在您開始使用 Intune 之前

開始之前，請執行下列作業：

1. 請確定您有一或多個已啟用 ADE 的 [Windows](../virtual-machines/linux/disk-encryption-overview.md) 或 [Linux](../virtual-machines/linux/disk-encryption-overview.md) vm。
2. 查看 Azure VM 備份[的支援矩陣](backup-support-matrix-iaas.md)
3. [建立](backup-create-rs-vault.md) 復原服務備份保存庫（如果您沒有的話）。
4. 如果您針對已啟用備份的 Vm 啟用加密，您只需要提供具有存取 Key Vault 的許可權，就能在不中斷的情況下繼續進行備份。 [深入瞭解](#provide-permissions) 如何指派這些許可權。

此外，在某些情況下，您可能還需要做幾件事：

- **在 VM 上安裝 VM 代理程式**：為機器上執行的 Azure VM 代理程式安裝擴充功能，以 Azure 備份來備份 Azure VM。 如果您的 VM 是從 Azure Marketplace 映像建立，則代理程式已安裝且正在執行。 如果您建立自訂 VM，或遷移內部部署機器，您可能需要[手動安裝代理程式](backup-azure-arm-vms-prepare.md#install-the-vm-agent)。

## <a name="configure-a-backup-policy"></a>設定備份原則

1. 如果您尚未建立復原服務備份保存庫，請遵循下列 [指示](backup-create-rs-vault.md)。
1. 在入口網站中開啟保存庫，然後選取 [**總覽**] 區段中的 [ **+ 備份**]。

    ![備份窗格](./media/backup-azure-vms-encryption/select-backup.png)

1. 在 [**備份目標**] 中  >  **，您的工作負載在何處執行？** 選取 [ **Azure**]。
1. 在 [ **您要備份什麼？** ] 中，選取 [ **虛擬機器**]。 然後選取 [ **備份**]。

      ![案例窗格](./media/backup-azure-vms-encryption/select-backup-goal-one.png)

1. 在 [**備份原則**] 中  >  ，**選擇 [備份原則**]，選取您要與保存庫相關聯的原則。 然後選取 [確定]。
    - 備份原則會指定執行備份的時間，以及它們的儲存時間長度。
    - 預設原則的詳細資料便會列在下拉式功能表之下。

    ![選擇備份原則](./media/backup-azure-vms-encryption/select-backup-goal-two.png)

1. 如果您不想要使用預設原則，請選取 [ **建立新** 的]，然後 [建立自訂原則](backup-azure-arm-vms-prepare.md#create-a-custom-policy)。

1. 在 [虛擬機器] 下，選取 [新增]。

    ![新增虛擬機器](./media/backup-azure-vms-encryption/add-virtual-machines.png)

1. 使用 [選取原則] 選擇您要備份的加密 Vm，然後選取 **[確定]**。

      ![選取加密的 VM](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)

1. 如果您使用 Azure Key Vault，在 [保存庫] 頁面上，您會看到一則訊息，指出 Azure 備份需要 Key Vault 中的金鑰和密碼的唯讀存取權。

    - 如果您收到此訊息，則不需要採取任何動作。

        ![存取正常](./media/backup-azure-vms-encryption/access-ok.png)

    - 如果您收到此訊息，則必須依照 [下列](#provide-permissions)程式中的說明設定許可權。

        ![存取警告](./media/backup-azure-vms-encryption/access-warning.png)

1. 選取 [ **啟用備份** ] 將備份原則部署到保存庫，並為選取的 vm 啟用備份。

## <a name="trigger-a-backup-job"></a>觸發備份作業

初始備份會根據排程執行，但也可以立即執行，如下所示：

1. 在保存庫功能表中，選取 [ **備份專案**]。
2. 在 [ **備份專案**] 中，選取 [ **Azure 虛擬機器**]。
3. 在 [ **備份專案** ] 清單中，選取省略號 ( ... ) 。
4. 選取 [ **立即備份**]。
5. 在 [立即備份] 中，使用行事曆控制項來選取復原點應該保留的最後一天。 然後選取 [確定]。
6. 監視入口網站通知。 您可以在保存庫儀表板中監視作業進度 > [備份作業] > [進行中]。 根據您的 VM 大小，建立初始備份可能需要花一點時間。

## <a name="provide-permissions"></a>提供許可權

Azure 備份需要唯讀存取權來備份金鑰和密碼，以及相關聯的 Vm。

- 您的 Key Vault 會與 Azure 訂用帳戶的 Azure AD 租使用者相關聯。 如果您是 **成員使用者**，Azure 備份可以取得 Key Vault 的存取權，而不需要採取進一步的動作。
- 如果您是 **來賓使用者**，您必須提供 Azure 備份存取金鑰保存庫的許可權。

若要設定許可權：

1. 在 [Azure 入口網站中，選取 [ **所有服務**]，然後搜尋 **金鑰保存庫**。
1. 選取與您要備份之加密 VM 相關聯的金鑰保存庫。
1. 選取 [**存取** 原則  >  **新增存取原則**]。

    ![新增存取原則](./media/backup-azure-vms-encryption/add-access-policy.png)

1. 在 **Add access policy**[  >  **從範本設定] (選用)** 的 [新增存取原則] 中，選取 [ **Azure 備份**]。
    - 該備份會在 [金鑰權限] 和 [祕密權限] 預先填入必要的權限。
    - 如果您的 VM 只使用 **BEK** 加密，請移除 **金鑰許可權** 的選項，因為您只需要秘密的許可權。

    ![Azure 備份選取範圍](./media/backup-azure-vms-encryption/select-backup-template.png)

1. 選取 [新增]  。 **備份管理服務** 已新增至 **存取原則**。

    ![存取原則](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

1. 選取 [ **儲存** ] 以提供具有許可權的 Azure 備份。

## <a name="restore-an-encrypted-vm"></a>還原已加密的 VM

您只能藉由還原 VM 磁片來還原已加密的 Vm，如下所述。 不支援 **取代現有** 和 **還原 VM** 。

還原已加密的 Vm，如下所示：

1. [還原 VM 磁碟](backup-azure-arm-restore-vms.md#restore-disks)。
2. 執行下列其中一項動作來重新建立虛擬機器實例：
    1. 使用還原作業期間所產生的範本來自訂 VM 設定，並觸發 VM 部署。 [深入了解](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm)。
    2. 使用 PowerShell 從還原的磁片建立新的 VM。 [深入了解](backup-azure-vms-automation.md#create-a-vm-from-restored-disks)。
3. 針對 Linux Vm，請重新安裝 ADE 延伸模組，以開啟並掛接資料磁片。

## <a name="next-steps"></a>後續步驟

如果您遇到任何問題，請參閱下列文章：

- 備份和還原已加密的 Azure Vm 時[常見的錯誤](backup-azure-vms-troubleshoot.md)。
- [AZURE VM 代理程式/備份擴充](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) 功能問題。
