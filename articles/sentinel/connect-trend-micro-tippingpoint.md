---
title: 將 Trend 微 TippingPoint 連接到 Azure Sentinel |Microsoft Docs
description: 瞭解如何使用 Trend 微 TippingPoint 資料連線器，將 TippingPoint SMS 登入提取 Azure Sentinel。 查看活頁簿中的 TippingPoint 資料、建立警示及改進調查。
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2021
ms.author: yelevin
ms.openlocfilehash: 5c7491a0e0ba2a3bf604988c613e1fd8937f277d
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/25/2021
ms.locfileid: "98752177"
---
# <a name="connect-your-trend-micro-tippingpoint-solution-to-azure-sentinel"></a>將您的趨勢微 TippingPoint 解決方案連線至 Azure Sentinel

> [!IMPORTANT]
> Trend 微 TippingPoint 連接器目前為 **預覽** 狀態。 請參閱 [Microsoft Azure 預覽的補充使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) ，以取得適用于 Azure 功能（Beta、預覽或尚未發行正式運作）的其他法律條款。

本文說明如何將 Trend 微 TippingPoint 威脅防護系統解決方案連線至 Azure Sentinel。 Trend 微 TippingPoint data connector 可讓您輕鬆地將 TippingPoint 安全性管理系統與 Azure Sentinel 連接 (SMS) 記錄檔，讓您可以查看活頁簿中的資料、使用它來建立自訂警示，並加以合併以改善調查。

> [!NOTE]
> 資料會儲存在您執行 Azure Sentinel 之工作區的地理位置。

## <a name="prerequisites"></a>必要條件

- 您必須擁有 Azure Sentinel 工作區的讀取和寫入權限。

- 您必須具有工作區共用金鑰的讀取權限。

## <a name="send-trend-micro-tippingpoint-logs-to-azure-sentinel"></a>將 Trend 微 TippingPoint 記錄傳送至 Azure Sentinel

若要將其登入 Azure Sentinel，請將 TippingPoint TPS 解決方案設定為以 CEF 格式將 Syslog 訊息傳送至以 Linux 為基礎的記錄轉送伺服器 (執行 rsyslog 或 Syslog) 。 此伺服器將會安裝 Log Analytics 代理程式，而代理程式會將記錄轉送到您的 Azure Sentinel 工作區。 連接器會使用剖析器函式，將其接收的資料轉換成正規化架構。 

1. 在 Azure Sentinel 導覽功能表中，選取 [ **資料連線器**]。

1. 從 **資料連線器** 資源庫中，選取 [ **Trend 微 TippingPoint (Preview])**，然後 **開啟 [連接器] 頁面**。

1. 遵循 [**說明**] 索引標籤的 [**設定：**

    1. 小於 **1。Linux Syslog 代理程式** 設定-如果您還沒有執行中的記錄轉寄站，或您需要另一個記錄轉寄站，請執行此步驟。 如需詳細的指示和說明，請參閱 Azure Sentinel 檔中的 [步驟1：部署記錄](connect-cef-agent.md) 轉寄站。

    1. 在 **2。將 Trend 微 TippingPoint SMS 記錄轉送到 Syslog 代理** 程式-此設定應包含下列元素：
        - 記錄目的地–記錄轉送伺服器的主機名稱和/或 IP 位址
        - 通訊協定和埠– **TCP 514** (如果另有建議，請務必在記錄轉送伺服器上的 syslog daemon 進行平行變更) 
        - 記錄格式– **ARCSIGHT CEF 格式 4.2**
        - 記錄檔類型–全部可用

    1. 在 **3。驗證連接** ：在 [連接器] 頁面上複製命令並在記錄轉寄站上執行命令，以驗證資料內嵌。 如需詳細的指示和說明，請參閱 Azure Sentinel 檔中的 [步驟3：驗證連接](connect-cef-verify.md) 。

        最多可能需要20分鐘的時間，記錄才會出現在 Log Analytics 中。

## <a name="find-your-data"></a>尋找您的資料

建立成功的連接之後，資料就會出現在 [ **記錄**] 的 [ **Azure Sentinel** ] 區段的 [ *CommonSecurityLog* ] 資料表中。

若要在 Log Analytics 中取得 Trend 微 TippingPoint 資料，您將查詢剖析器函式，而不是資料表。 將下列內容複寫到查詢視窗中，並在您選擇的情況下套用其他篩選準則：

```kusto
TrendMicroTippingPoint
| sort by TimeGenerated
```

如需更多查詢範例，請參閱連接器頁面中的 [ **下一個步驟** ] 索引標籤。

## <a name="next-steps"></a>後續步驟

在本檔中，您已瞭解如何將 Trend 微 TippingPoint 連接至 Azure Sentinel。 若要深入了解 Azure Sentinel，請參閱下列文章：

- 深入了解如何[取得資料的可見度以及潛在威脅](quickstart-get-visibility.md)。
- 開始[使用 Azure Sentinel 偵測威脅](tutorial-detect-threats-built-in.md)。
- [使用活頁簿](tutorial-monitor-your-data.md)監視資料。
