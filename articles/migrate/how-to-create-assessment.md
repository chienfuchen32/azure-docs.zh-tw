---
title: 使用 Azure Migrate 伺服器評量建立 Azure VM 評量 |Microsoft Docs
description: 說明如何使用 Azure Migrate Server 評定工具來建立 Azure VM 評量
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 07/15/2019
ms.openlocfilehash: 178bdca78c6f011c607de8e1f5d5eabcdbaab7d4
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2021
ms.locfileid: "98567688"
---
# <a name="create-an-azure-vm-assessment"></a>建立 Azure VM 評量

本文說明如何使用 Azure Migrate：伺服器評量來建立適用于內部部署 VMware Vm 或 Hyper-v Vm 的 Azure VM 評量。

[Azure Migrate](migrate-services-overview.md) 可協助您遷移至 Azure。 Azure Migrate 能提供集中式的中樞，以追蹤針對 Azure 的內部部署基礎結構、應用程式與資料的探索、評量及移轉。 該中樞能提供 Azure 工具以進行評量和移轉，也提供協力廠商獨立軟體廠商 (ISV) 供應項目。 

## <a name="before-you-start"></a>在您開始使用 Intune 之前

- 請確定您已 [建立](./create-manage-projects.md) Azure Migrate 專案。
- 如果您已建立專案，請確定您已 [新增](how-to-assess.md) Azure Migrate：伺服器評定工具。
- 若要建立評量，您必須設定適用于 [VMware](how-to-set-up-appliance-vmware.md) 或 [hyper-v](how-to-set-up-appliance-hyper-v.md)的 Azure Migrate 設備。 設備會探索內部部署機器，並將中繼資料和效能資料傳送至 Azure Migrate：伺服器評量。 [深入了解](migrate-appliance.md)。


## <a name="azure-vm-assessment-overview"></a>Azure VM 評量總覽
您可以使用兩種類型的大小調整準則來建立使用 Azure Migrate：伺服器評量的 Azure VM 評量。

**評量** | **詳細資料** | **Data**
--- | --- | ---
**以效能為基礎** | 以所收集效能資料為基礎的評估 | **建議的 VM 大小**：以 CPU 和記憶體使用量資料為基礎。<br/><br/> **建議的磁碟類型 (標準或進階受控磁碟**)：以內部部署磁碟的 IOPS 和輸送量為基礎。
**作為內部部署** | 以內部部署大小調整為基礎的評估。 | **建議的 VM 大小**：以內部部署 VM 大小為基礎<br/><br> **建議的磁碟類型**：以您為評估選取的儲存體類型設定為基礎。

[深入了解](concepts-assessment-calculation.md)評估。

## <a name="run-an-assessment"></a>執行評估

執行評估，如下所示：

1. 在 [伺服器] 頁面 > [Windows 和 Linux 伺服器] 上，按一下 [評估和遷移伺服器]。

   ![評估和遷移伺服器按鈕的位置](./media/tutorial-assess-vmware-azure-vm/assess.png)

2. 在 [**Azure Migrate：伺服器評量]** 中，按一下 [評估]。

    ![[評估] 按鈕的位置](./media/tutorial-assess-vmware-azure-vm/assess-servers.png)

3. 在 [評估伺服器]  >  [評量類型] 中，請選取 [Azure VM]。
4. 在 [探索來源] 中：

    - 如果您使用設備探索到機器，請選取 [從 Azure Migrate 設備探索到的機器]。
    - 如果您使用匯入的 CSV 檔案探索到機器，請選取 [匯入的機器]。 
    
1. 按一下 [ **編輯** ] 以檢查評量屬性。

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vm/assessment-name.png" alt-text="查看評量屬性的 [編輯] 按鈕位置":::

1. 在 [評量屬性] > [目標屬性] 中：
    - 在 [目標位置] 中，指定您要將機器遷移到其中的 Azure 區域。
        - 大小和成本建議是根據您指定的位置。 從預設值變更目標位置之後，系統會提示您指定 **保留實例** 和 **VM 系列**。
        - 在 Azure Government 中，您可以將評量的目標設定為[這些區域](migrate-support-matrix.md#supported-geographies-azure-government)
    - 在 [儲存體類型] 中，
        - 如果您想要在評量中使用以效能為基礎的資料，請針對 Azure Migrate 選取 [自動]，以根據磁碟 IOPS 和輸送量來建議儲存體類型。
        - 或者，選取您想要在遷移 VM 時使用的儲存體類型。
    - 在 [保留執行個體] 中，指定是否要在遷移 VM 時使用保留執行個體。
        - 如果您選取使用保留執行個體，則無法指定 [折扣 (%)] 或 [VM 執行時間]。 
        - [深入了解](https://aka.ms/azurereservedinstances)。
 1. 在 [VM 大小] 中：
     - 在 [調整大小準則] 中，選取是否要根據電腦設定資料/中繼資料或以效能為基礎的資料進行評量。 如果您使用效能資料：
        - 在 [效能歷程記錄] 中，指出您想要作為評量基礎的資料持續時間
        - 在 [百分位數使用率] 中，指定您想要用於效能範例的百分位數值。 
    - 在 [VM 系列] 中，指定您想要考量的 Azure VM 系列。
        - 如果您使用的是以效能為基礎的評量，Azure Migrate 會為您建議值。
        - 視需要調整設定。 例如，如果您沒有實際執行環境需要 Azure 中的 A 系列 VM，則可以從系列清單中排除 A 系列。
    - 在 [緩和因數] 中，指出您想要在評量期間使用的緩衝區。 這會考量各個問題，例如季節性使用量、簡短的效能歷程記錄，以及未來可能增加的使用量。 例如，如果您使用兩個緩和因數：
    
        **元件** | **有效使用率** | **新增緩和因數 (2.0)**
        --- | --- | ---
        核心 | 2  | 4
        記憶體 | 8 GB | 16 GB
   
1. 在 [價格] 中：
    - 如果您已註冊，請在 [供應項目] 中指定 [Azure 供應項目](https://azure.microsoft.com/support/legal/offer-details/)。 伺服器評量會評估該供應項目的成本。
    - 在 [貨幣] 中，選取您帳戶的帳單貨幣。
    - 在 [折扣 (%)] 中，在 Azure 供應項目上新增您獲得的任何訂用帳戶特定折扣。 預設設定為 0%。
    - 在 [VM 執行時間] 中，指定 VM 將會執行的持續時間 (每月天數/每天時數)。
        - 這適用於不會連續執行的 Azure VM。
        - 成本預估是根據指定的持續時間。
        - 預設值是每月 31 天/每天 24 小時。
    - 在 [EA 訂用帳戶] 中，指定是否要將 Enterprise 合約 (EA) 訂用帳戶折扣納入考量，以預估成本。 
    - 在 [Azure Hybrid Benefit] 中，指定您是否已經有 Windows Server 授權。 如果您有授權，且授權涵蓋 Windows Server 訂用帳戶的作用中軟體保證，您可以在將授權帶入 Azure 時，針對 [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/) 套用。

1. 如果您進行變更，請按一下 [儲存]。

    ![評量屬性](./media/tutorial-assess-vmware-azure-vm/assessment-properties.png)

1. 在 [ **評估伺服器** ] > 按 **[下一步]**。

1. 在 [**選取要評估** 評量的電腦]  >   > 指定評量的名稱。 

1. 在 [ **選取或建立群組** ] > 選取 [ **建立新** 的]，並指定組名。 

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vm/assess-group.png" alt-text="將 VM 新增至群組":::

1. 選取設備，然後選取您想要新增至群組的 VM。 然後按一下 [下一步]  。


1. 在 [檢閱 + 建立評量] 中，檢閱評量詳細資料，然後按一下 [建立評量] 以建立群組並執行評量。

1. 評量建立好之後，可在 [伺服器] >  **[Azure Migrate：伺服器評量]**  > [評量] 中加以檢視。

1. 按一下 [匯出評估]，將其下載為 Excel 檔案。
    > [!NOTE]
    > 針對以效能為基礎的評量，建議您在建立評量之前，先等待至少一天後再開始探索。 這可提供更高的信賴度來收集效能資料。 在理想的情況下，在您開始探索之後，請等候您指定的效能持續時間 (天/週/月)，以取得高信賴度。


## <a name="review-an-azure-vm-assessment"></a>檢閱 Azure VM 評量

Azure VM 評量內容會說明：

- **Azure 移轉整備程度**：VM 是否適合移轉至 Azure。
- **每月成本預估**：在 Azure 中執行 VM 的預估每月計算和儲存體成本。
- **每月儲存體成本預估**：移轉之後的磁碟儲存體預估成本。

### <a name="view-an-azure-vm-assessment"></a>查看 Azure VM 評量

1. 在 [移轉目標] >  [伺服器] 中，按一下 [Azure Migrate：伺服器評量] 中的 [評量]。
2. 在 [評量] 中，按一下評量來加以開啟。

    ![評量摘要](./media/how-to-create-assessment/assessment-summary.png)

### <a name="review-azure-readiness"></a>檢閱 Azure 移轉整備程度

1. 在 [Azure 移轉整備程度] 中，確認 VM 是否已準備好移轉至 Azure。
2. 檢查 VM 狀態：
    - **可供 Azure 使用**：Azure Migrate 會針對評估中的 VM 建議 VM大小和成本估計值。
    - **有條件的備妥**：顯示問題和建議的補救措施。
    - **未備妥供 Azure 使用**：顯示問題和建議的補救措施。
    - **移轉整備程度未知**：當 Azure Migrate 因資料可用性問題而無法評估整備程度時，便會使用此狀態。

3. 按一下 [Azure 移轉整備程度] 狀態。 您可以檢視 VM 的整備程度詳細資料，並向下切入以查看 VM 詳細資料，包括計算、儲存體和網路設定。



### <a name="review-cost-details"></a>檢閱成本詳細資料

此檢視會顯示在 Azure 中執行 VM 的計算和儲存體預估成本。

1. 檢閱每月的計算和儲存體成本。 系統會針對評估群組中的所有 VM 匯總成本。

    - 系統會根據機器的大小建議和其磁碟與屬性來預估成本。
    - 系統會顯示計算和儲存體的每月預估成本。
    - 以 IaaS VM 的形式來執行內部部署 VM 時，才能估計成本。 Azure Migrate 伺服器評估不會考慮 PaaS 或 SaaS 的成本。

2. 您可以檢閱每月的儲存體成本估計值。 此檢視會顯示評估群組的彙總儲存體成本，並分割為不同類型的儲存體磁碟。
3. 您可以向下切入以查看特定 VM 的詳細資料。


### <a name="review-confidence-rating"></a>檢閱信賴評等

當您執行以效能為基礎的評估時，系統會對評估指派信賴評等。

![信賴評等](./media/how-to-create-assessment/confidence-rating.png)

- 會給予 1 星 (最低) 到 5 星 (最高) 的評等。
- 信賴評等可協助您預估評估作業所提供的大小建議是否可靠。
- 信賴評等會以計算評估所需的資料點可用性為基礎。

評估的信賴評等如下所示。

**資料點可用性** | **信賴評等**
--- | ---
0%-20% | 1 顆星
21%-40% | 2 顆星
41%-60% | 3 顆星
61%-80% | 4 顆星
81%-100% | 5 顆星




## <a name="next-steps"></a>後續步驟

- 瞭解如何使用相依性 [對應](how-to-create-group-machine-dependencies.md) 來建立高信賴度群組。
- [深入了解](concepts-assessment-calculation.md)評定的計算方式。