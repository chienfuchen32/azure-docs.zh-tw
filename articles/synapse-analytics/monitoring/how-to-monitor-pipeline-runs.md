---
title: 使用 Synapse Studio 監視管線執行
description: 使用 Synapse Studio 來監視您的工作區管線執行。
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 11/30/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: dbd890117c78274392d5745e0563332371b404c5
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/01/2020
ms.locfileid: "96452571"
---
# <a name="use-synapse-studio-to-monitor-your-workspace-pipeline-runs"></a>使用 Synapse Studio 監視您的工作區管線執行

使用 Azure Synapse Analytics，您可以建立複雜的管線，以自動化和整合您解決方案內的資料移動、資料轉換和計算活動。 您可以使用 Synapse Studio 來撰寫和監視這些管線。

本文說明如何監視您的管線執行，這可讓您留意管線的最新狀態、問題和進度。

## <a name="access-pipeline-runs-list"></a>存取管線執行清單

若要查看工作區中的管線執行清單，請先 [開啟 Synapse Studio](https://web.azuresynapse.net/) ，然後選取您的工作區。

![登入工作區](./media/common/login-workspace.png)

開啟您的工作區之後，請選取左側的 [ **監視** ] 區段。

![選取監視中樞](./media/common/left-nav.png)

選取 [ **管線執行** ] 以查看管線執行的清單。

![選取管線執行](./media/how-to-monitor-pipeline-runs/monitor-hub-nav-pipelineruns.png)

## <a name="filter-your-pipeline-runs"></a>篩選您的管線執行

您可以將管線執行清單篩選至您感興趣的專案。 畫面頂端的篩選器可讓您指定想要篩選的欄位。

例如，您可以篩選視圖，只查看名為「假日」之管線的管線執行：

![範例篩選](./media/how-to-monitor-pipeline-runs/filter-example.png)

## <a name="view-details-about-a-specific-pipeline-run"></a>查看特定管線執行的詳細資料

若要查看管線執行的詳細資料，請選取管線執行。 然後，查看與管線執行相關聯的活動執行。 如果管線仍在執行中，您可以監視進度。 
  
## <a name="next-steps"></a>後續步驟

若要深入瞭解監視應用程式，請參閱 [Monitor Apache Spark applications](how-to-monitor-spark-applications.md) 文章。 
