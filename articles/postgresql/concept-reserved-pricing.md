---
title: 保留計算定價-適用於 PostgreSQL 的 Azure 資料庫-單一伺服器
description: 預付具有保留容量的適用於 PostgreSQL 的 Azure 資料庫計算資源
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/16/2020
ms.openlocfilehash: 9b8dafa4a69358b3f6f09551ac426b908750e2f4
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98735467"
---
# <a name="prepay-for-azure-database-for-postgresql---single-server-compute-resources-with-reserved-capacity"></a>預付適用於 PostgreSQL 的 Azure 資料庫-具有保留容量的單一伺服器計算資源

相較于隨用隨付價格，適用於 PostgreSQL 的 Azure 資料庫現在可協助您節省預付的計算資源成本。 使用適用於 PostgreSQL 的 Azure 資料庫保留容量時，您可以在於 postgresql 伺服器上預先承諾一或三年期，以取得計算成本的大量折扣。 若要購買適用於 PostgreSQL 的 Azure 資料庫保留容量，您需要指定 Azure 區域、部署類型、效能層級和期限。 </br>

您不需要將保留指派給特定的適用於 PostgreSQL 的 Azure 資料庫伺服器。 已在執行的適用於 PostgreSQL 的 Azure 資料庫 (或剛部署的) 將會自動獲得保留定價的優點。 購買保留之後，您就可以預先支付一年或三年期的計算成本。 當您購買保留專案時，與保留屬性相符的 Azure database for 于 postgresql 計算費用就不會再以隨用隨付費率計費。 保留並未涵蓋與于 postgresql 資料庫伺服器相關聯的軟體、網路或儲存體費用。 在保留期限結束時，帳單權益會過期，而適用於 PostgreSQL 的 Azure 資料庫會以隨用隨付價格計費。 保留不會自動更新。 如需定價資訊，請參閱 [適用於 PostgreSQL 的 Azure 資料庫保留容量](https://azure.microsoft.com/pricing/details/postgresql/)供應專案。 </br>

> [!IMPORTANT]
> 您可以在 [單一伺服器](./overview.md#azure-database-for-postgresql---single-server) 和 [超大規模 Citus](./overview.md#azure-database-for-postgresql--hyperscale-citus) 部署選項中取得適用於 PostgreSQL 的 Azure 資料庫的保留容量定價。 如需超大規模 (Citus) RI 定價的相關資訊，請參閱 [此頁面](concepts-hyperscale-reserved-pricing.md)。

您可以在 [Azure 入口網站](https://portal.azure.com/)中購買適用於 PostgreSQL 的 Azure 資料庫的保留容量。 保留的付款方式可為[預先付款或每月付款](../cost-management-billing/reservations/prepare-buy-reservation.md)。 若要購買保留容量：

* 您必須是至少一個具有隨用隨付費率的企業或個別訂用帳戶的擁有者角色。
* 針對企業訂用帳戶，必須在 [EA 入口網站](https://ea.azure.com/)中啟用 **新增保留執行個體**。 或者，如果該設定已停用，則您必須是訂用帳戶上的 EA 系統管理員。
* 針對雲端解決方案提供者 (CSP) 方案，只有系統管理員專員或銷售專員可以購買適用於 PostgreSQL 的 Azure 資料庫保留容量。 </br>

如需企業客戶和隨用隨付客戶的計費方式的詳細資訊，請參閱針對您的 [enterprise 註冊瞭解 azure 保留使用量](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md) ，並 [瞭解您的隨用隨付訂用帳戶的 azure 保留使用量](../cost-management-billing/reservations/understand-reserved-instance-usage.md)。


## <a name="determine-the-right-server-size-before-purchase"></a>在購買前判斷正確的伺服器大小

保留大小應該以特定區域內現有或即將部署的伺服器所使用的總計算量為基礎，並且使用相同的效能層級和硬體世代。</br>

例如，假設您執行的是一個一般用途第5代– 32 vCore 于 postgresql 資料庫，以及兩個記憶體優化第5代-16 vCore 于 postgresql 資料庫。 此外，假設您打算在下個月內部署額外的一般目的第5代– 8 vCore 資料庫伺服器，以及一個記憶體優化的第5代– 32 vCore 資料庫伺服器。 假設您知道您將需要這些資源至少一年。 在此情況下，您應該購買 40 (32 + 8) 虛擬核心，一年保留單一資料庫一般目的-第5代和 64 (2x16 + 32) vCore 一年保留單一資料庫記憶體優化-第5代


## <a name="buy-azure-database-for-postgresql-reserved-capacity"></a>購買適用於 PostgreSQL 的 Azure 資料庫保留容量

1. 登入 [Azure 入口網站](https://portal.azure.com/)。
2. 選取 [所有服務] > [保留]。
3. 選取 [ **新增** ]，然後在 [購買保留] 窗格中，選取 [ **適用於 PostgreSQL 的 Azure 資料庫** 購買于 postgresql 資料庫的新保留。
4. 填寫必要欄位。 符合所選屬性的現有或新資料庫符合取得保留容量折扣的資格。 取得折扣的適用於 PostgreSQL 的 Azure 資料庫伺服器實際數目取決於選取的範圍和數量。


:::image type="content" source="media/concepts-reserved-pricing/postgresql-reserved-price.png" alt-text="保留定價總覽":::


下表說明必要的欄位。

| 欄位 | 描述 |
| :------------ | :------- |
| 訂用帳戶   | 用來支付適用於 PostgreSQL 的 Azure 資料庫保留容量保留的訂用帳戶。 訂用帳戶上的付款方法會收取適用於 PostgreSQL 的 Azure 資料庫保留容量保留的預付成本。 訂用帳戶類型必須是 enterprise 合約 (供應專案號碼： MS-AZR-0003P->ms-azr-0017p 或 MS-AZR-0003P-Ms-azr-0148p) 或具有隨用隨付定價的個別合約 (供應專案號碼： MS-MS-AZR-0003P-Ms-azr-0003p 或 MS-MS-AZR-0003P-Ms-azr-0023p) 。 若為企業訂用帳戶，費用會從註冊的 Azure 預付款扣除 (先前稱為預付金) 餘額或收取超額部分的費用。 針對具有隨用隨付定價的個別訂用帳戶，費用會以訂用帳戶的信用卡或發票付款方法計費。
| 影響範圍 | 虛擬核心保留容量範圍可以涵蓋一個訂用帳戶或多個訂用帳戶 (共用範圍)。 如果您選取： </br></br> **共用**，vCore 保留折扣會套用至計費內容內任何訂用帳戶中執行的適用於 PostgreSQL 的 Azure 資料庫伺服器。 針對企業客戶，共用範圍是註冊，並包含註冊中的所有訂用帳戶。 針對隨用隨付客戶，共用範圍是帳戶系統管理員所建立的所有隨用隨付訂用帳戶。</br></br> **單一訂** 用帳戶，vCore 保留折扣會套用到此訂用帳戶中適用於 PostgreSQL 的 Azure 資料庫的伺服器。 </br></br> **單一資源群組**，保留折扣會套用至所選訂用帳戶中的適用於 PostgreSQL 的 Azure 資料庫伺服器，以及該訂用帳戶內選取的資源群組。
| 區域 | 適用於 PostgreSQL 的 Azure 資料庫保留容量保留所涵蓋的 Azure 區域。
| 部署類型 | 您要為其購買保留的適用於 PostgreSQL 的 Azure 資料庫資源類型。
| 效能層級 | 適用於 PostgreSQL 的 Azure 資料庫伺服器的服務層級。
| 詞彙 | 一年
| 數量 | 適用於 PostgreSQL 的 Azure 資料庫保留容量保留內所購買的計算資源數量。 數量是所選 Azure 區域和效能層級中所保留的虛擬核心數，將會取得帳單折扣。 例如，如果您正在執行或計畫在美國東部區域中執行第5代16虛擬核心的總計算容量的適用於 PostgreSQL 的 Azure 資料庫伺服器，則您會將 quantity 指定為16，以將所有伺服器的效益最大化。

## <a name="cancel-exchange-or-refund-reservations"></a>取消、交換保留或進行退費

您可以取消、交換保留或進行退費，但有某些限制。 如需詳細資訊，請參閱 [Azure 保留的自助式交換和退費](../cost-management-billing/reservations/exchange-and-refund-azure-reservations.md)。

## <a name="vcore-size-flexibility"></a>vCore 大小彈性

vCore 大小彈性可協助您在效能層級和區域內相應增加或相應減少，而不會失去保留容量優勢。 如果您的虛擬核心規模比您的保留容量更高，則會以隨用隨付定價向您收取超過虛擬核心的費用。


## <a name="need-help-contact-us"></a>需要協助嗎？ 與我們連絡

如果您有問題或需要協助，請[建立支援要求](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)。

## <a name="next-steps"></a>後續步驟

VCore 保留折扣會自動套用至符合適用於 PostgreSQL 的 Azure 資料庫保留容量保留範圍和屬性的適用於 PostgreSQL 的 Azure 資料庫伺服器數目。 您可以透過 Azure 入口網站、PowerShell、CLI 或 API 來更新 Azure 資料庫的範圍，以進行于 postgresql 保留容量保留。

若要深入了解 Azure 保留項目，請參閱下列文章：

* [什麼是 Azure 保留](../cost-management-billing/reservations/save-compute-costs-reservations.md)？
* [管理 Azure 保留項目](../cost-management-billing/reservations/manage-reserved-vm-instance.md)
* [了解 Azure 保留折扣](../cost-management-billing/reservations/understand-reservation-charges.md)
* [了解隨用隨付訂用帳戶的保留使用量](../cost-management-billing/reservations/understand-reservation-charges-postgresql.md)
* [了解 Enterprise 註冊的保留項目使用量](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md)
* [合作夥伴中心雲端解決方案提供者 (CSP) 計畫中的 Azure 保留項目](/partner-center/azure-reservations)