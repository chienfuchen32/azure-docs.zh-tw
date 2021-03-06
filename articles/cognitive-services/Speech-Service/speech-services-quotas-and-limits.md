---
title: 語音服務配額和限制
titleSuffix: Azure Cognitive Services
description: Azure 認知語音服務配額和限制的快速參考、詳細描述和最佳作法
services: cognitive-services
author: alexeyo26
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/07/2020
ms.author: alexeyo
ms.openlocfilehash: 9b923ba208dd2a5111a59f67401b1e4f080b7187
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2020
ms.locfileid: "96754159"
---
# <a name="speech-services-quotas-and-limits"></a>語音服務配額和限制

本文包含 Azure 認知語音服務配額的快速參考和 **詳細說明** ，以及所有 [定價層](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)的限制。 它也包含一些可避免要求節流的最佳做法。 

## <a name="quotas-and-limits-quick-reference"></a>配額和限制快速參考
跳至 [文字轉換語音的配額和限制](#text-to-speech-quotas-and-limits-per-speech-resource)
### <a name="speech-to-text-quotas-and-limits-per-speech-resource"></a>每個語音資源的語音轉換文字配額和限制
在下表中，沒有「可調」資料列的參數 **無法** 針對所有定價層進行調整。

#### <a name="online-transcription"></a>線上轉譯

| Quota | 免費 (F0) <sup>1</sup> | 標準 (S0) |
|--|--|--|
| **(基礎和自訂模型的並行要求限制)** | 1 | 20 (預設值)  |
| 調 | 否<sup>2</sup> | 是<sup>2</sup> |

#### <a name="batch-transcription"></a>批次轉譯
| Quota | 免費 (F0) <sup>1</sup> | 標準 (S0) |
|--|--|--|
| REST API 限制 | F0 無法使用批次轉譯 | 每分鐘300個要求 |
| 音訊輸入檔大小上限 | N/A | 1 GB |
| 輸入 blob 大小上限 (可以包含一個以上的檔案，例如 zip 封存中的檔案;請務必記下上面的檔案大小限制)  | N/A | 2.5 GB |
| Blob 容器大小上限 | N/A | 5 GB |
| 每個容器的 blob 數目上限 | N/A | 10000 |
| 使用多個內容 Url 做為輸入時，每個轉譯要求 (的檔案數目上限)  | N/A | 1000  |
| 同時執行的作業數目上限 | N/A | 2000  |

#### <a name="model-customization"></a>模型自訂
| Quota | 免費 (F0) <sup>1</sup> | 標準 (S0) |
|--|--|--|
| REST API 限制 | 每分鐘300個要求 | 每分鐘300個要求 |
| 語音資料集的最大數目 | 2 | 500 |
| 資料匯入的聲場資料集檔案大小上限 | 2 GB | 2 GB |
| 匯入資料的最大語言資料集檔案大小 | 200 MB | 1.5 GB |
| 匯入資料的最大發音資料集檔案大小 | 1 KB | 1 MB |
| `text`在[建立模型](https://westcentralus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/CreateModel/)API 要求中使用參數時的文字大小上限 | 200 KB | 500 KB |

<sup>1</sup> 代表 **免費 (F0)** 定價層另請參閱 [定價頁面](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)上的每月額度額度。<br/>
<sup>2</sup> 請參閱 [其他說明](#detailed-description-quota-adjustment-and-best-practices)、 [最佳作法](#general-best-practices-to-mitigate-throttling-during-autoscaling)和 [調整指示](#speech-to-text-increasing-online-transcription-concurrent-request-limit)。<br/> 

### <a name="text-to-speech-quotas-and-limits-per-speech-resource"></a>每個語音資源的文字轉換語音配額和限制
在下表中，沒有「可調」資料列的參數 **無法** 針對所有定價層進行調整。

| Quota | 免費 (F0) <sup>3</sup> | 標準 (S0) |
|--|--|--|
| **標準和類神經語音的每秒交易數上限 (TPS)** | 200<sup>4</sup> | 200<sup>4</sup> |  |
| **自訂語音的並行要求限制** |  |  |
| 預設值 | 10 | 10 |
| 調 | 否<sup>5</sup> | 是<sup>5</sup> |
| **HTTP 特定配額** |  |
| 每個要求產生的最大音訊長度 | 10 分鐘 | 10 分鐘 |
| SSML 中相異標記的最大數目 `<voice>` | 50 | 50 |
| **Websocket 特定配額** |  |  |
|每回合產生的最大音訊長度 | 10 分鐘 | 10 分鐘 |
|每回合的 SSML 訊息大小上限 |64 KB |64 KB |
| **REST API 限制** | 每分鐘20個要求 | 每5秒25個要求 |


<sup>3</sup> **免費 (F0)** 定價層另請參閱 [定價頁面](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)上的每月額度額度。<br/>
<sup>4</sup> 請參閱 [其他說明](#detailed-description-quota-adjustment-and-best-practices) 和 [最佳作法](#general-best-practices-to-mitigate-throttling-during-autoscaling)。<br/>
<sup>5</sup> 請參閱 [其他說明](#detailed-description-quota-adjustment-and-best-practices)、 [最佳作法](#general-best-practices-to-mitigate-throttling-during-autoscaling)和 [調整指示](#text-to-speech-increasing-transcription-concurrent-request-limit-for-custom-voice)。<br/> 

## <a name="detailed-description-quota-adjustment-and-best-practices"></a>詳細描述、配額調整和最佳作法
在要求配額增加 (的情況下，) 確定是必要的。 語音服務會使用自動調整技術，將所需的計算資源納入「隨選」模式，同時在不需維護大量硬體容量的情況下，讓客戶的成本維持在最低。 每次您的應用程式收到回應碼 429 ( 「太多要求」 ) ，而您的工作負載是在定義的限制內 (請參閱 [配額和限制快速參考](#quotas-and-limits-quick-reference)) 最可能的說明是服務正在相應增加為您的需求，而且尚未達到所需的規模，因此不會立即有足夠的資源來提供要求。 此狀態通常是暫時性的，而且不應該是最長的。

### <a name="general-best-practices-to-mitigate-throttling-during-autoscaling"></a>在自動調整期間降低節流的一般最佳作法
若要將節流 (回應碼) 429 的相關問題降至最低，我們建議使用下列技術：
- 在您的應用程式中實作重試邏輯
- 避免工作負載中的明顯變更。 逐漸增加工作負載 <br/>
*範例。* 您的應用程式使用文字轉換語音，而您目前的工作負載是每秒)  (筆交易。 接下來，您會將負載增加到20個 TP (四倍以上) 。 服務會立即開始相應增加以滿足新的負載，但可能不會在一秒內完成，因此某些要求會收到回應碼429。   
- 測試不同的負載增加模式
  - 請參閱 [語音轉換文字範例](#speech-to-text-example-of-a-workload-pattern-best-practice)
- 在相同或不同區域中建立額外的語音資源，並使用「迴圈配置資源」技術來散發工作負載。 這對 **文字轉換語音的 tp (每秒交易數** 特別重要) 參數，每個語音資源設定為200，無法調整  

下一節將說明調整配額的特定案例。<br/>
跳至 [文字轉換語音。增加自訂語音的轉譯並行要求限制](#text-to-speech-increasing-transcription-concurrent-request-limit-for-custom-voice)

### <a name="speech-to-text-increasing-online-transcription-concurrent-request-limit"></a>語音轉換文字：增加線上轉譯並行要求限制
根據預設，並行要求的數目限制為每個語音資源 20 (基礎模型) 或每個自訂端點 (自訂模型) 。 若為標準定價層，則可以增加此數量。 提交要求之前，請確定您已熟悉 [本節中的內容，](#detailed-description-quota-adjustment-and-best-practices) 並瞭解這些 [最佳做法](#general-best-practices-to-mitigate-throttling-during-autoscaling)。

增加並行要求 **限制不會直接影響** 您的成本。 語音服務會使用「使用時付費」模型。 此限制會定義服務在開始節流您的要求之前，可以調整的程度。

**基底** 和 **自訂** 模型的並行要求限制需要 **另外** 調整。

Azure 入口網站、Command-Line 工具或 API 要求都看 **不** 到並行要求限制參數的現有值。 若要確認現有的值，請建立 Azure 支援要求。

>[!NOTE]
>[語音容器](speech-container-howto.md) 不需要增加並行要求的限制，因為容器只受限於裝載的硬體 cpu。 不過，語音容器有自己的容量限制，應納入考慮。 請參閱問題 *：「您可以協助進行容量規劃，以及內部部署語音轉換文字容器的成本估計」。* 從 [語音容器的常見問題](speech-container-faq.md)。

#### <a name="have-the-required-information-ready"></a>備妥所需的資訊：
- 針對 **基底模型**：
  - 語音資源識別碼
  - 區域
- **自訂模型**： 
  - 區域
  - 自訂端點識別碼

- **如何 (基底模型) 取得資訊**：  
  - 移至 [Azure 入口網站](https://portal.azure.com/)
  - 選取您要增加並行要求限制的語音資源
  - 選取 [*資源管理* 群組] (的 [*屬性*])  
  - 複製並儲存下欄欄位的值：
    - **資源識別碼**
    - 端點區域 (**位置**) 

- **如何 (自訂模型) 取得資訊**：
  - 前往 [Speech Studio](https://speech.microsoft.com/) 入口網站
  - 必要時登入
  - 移至自訂語音
  - 選取您的專案
  - 移至 *部署*
  - 選取所需的端點
  - 複製並儲存下欄欄位的值：
    - 端點區域 (的 **服務區域**) 
    - **端點識別碼**
  
#### <a name="create-and-submit-support-request"></a>建立並提交支援要求
針對您的資源起始增加的並行要求限制，或在必要時提交支援要求以檢查今日的限制：

- 確定您有 [必要的資訊](#have-the-required-information-ready)
- 移至 [Azure 入口網站](https://portal.azure.com/)
- 選取您要增加 (的語音資源，或檢查) 並行要求限制
- 選取 [ *新增支援要求* (*支援 + 疑難排解* ] 群組)  
- 新的視窗隨即出現，其中包含 Azure 訂用帳戶和 Azure 資源的自動填入資訊
- 輸入 *摘要* (例如「增加 STT 並行要求限制」 ) 
- 在 [ *問題類型* ] 中，選取 [配額或訂用帳戶問題]
- 出現 *問題子類型* 時，請選取：
  - 「配額或並行要求增加」-表示增加要求
  - 「配額或使用量驗證」以檢查現有的限制
- 按 *[下一步：方案]*
- 繼續進行要求建立
- 在 [ *詳細資料* ] 索引標籤的 [ *描述* ] 欄位中輸入：
  - 請注意，要求是關於 **語音轉換文字** 配額
  - **基底** 或 **自訂** 模型
  - 您[之前收集](#have-the-required-information-ready)的 Azure 資源資訊 
  - 完成輸入必要資訊，然後按一下 [*檢查 + 建立*] 索引標籤中的 [*建立*] 按鈕
  - 請注意 Azure 入口網站通知中的支援要求號碼。 即將聯繫您以進行進一步的處理

### <a name="speech-to-text-example-of-a-workload-pattern-best-practice"></a>語音轉換文字：工作負載模式最佳作法的範例
此範例會提供我們建議的方法，以減少因進行自動調整 [所](#detailed-description-quota-adjustment-and-best-practices)造成的可能要求節流。 這並不是「精確的配方」，而只是我們邀請遵循的範本並視需要進行調整。

讓我們假設語音資源的並行要求限制設定為300。 從20個並行連接啟動工作負載，並每隔 1.5-2 分鐘增加20個並行連線的負載。 如果您收到太多回應碼429，請控制服務的回應，並執行回復 (降低負載) 的邏輯。 然後在1-2-4-4 分鐘模式中重試。  (在1分鐘內重試負載增加，如果仍然無法運作，則在2分鐘內，依此類推) 

一般而言，強烈建議您先測試工作負載和工作負載模式，再進入生產環境。

### <a name="text-to-speech-increasing-transcription-concurrent-request-limit-for-custom-voice"></a>文字轉換語音：增加自訂語音的轉譯並行要求限制
依預設，自訂語音端點的並行要求數目限制為10個。 若為標準定價層，則可以增加此數量。 提交要求之前，請確定您已熟悉 [本節中的內容，](#detailed-description-quota-adjustment-and-best-practices) 並瞭解這些 [最佳做法](#general-best-practices-to-mitigate-throttling-during-autoscaling)。

增加並行要求 **限制不會直接影響** 您的成本。 語音服務會使用「使用時付費」模型。 此限制會定義服務在開始節流您的要求之前，可以調整的程度。

Azure 入口網站、Command-Line 工具或 API 要求都看 **不** 到並行要求限制參數的現有值。 若要確認現有的值，請建立 Azure 支援要求。

>[!NOTE]
>[語音容器](speech-container-howto.md) 不需要增加並行要求的限制，因為容器只受限於裝載的硬體 cpu。

#### <a name="prepare-the-required-information"></a>準備所需的資訊：
若要建立增加要求，您必須提供部署區域和自訂端點識別碼。 若要取得它，請執行下列動作： 

- 前往 [Speech Studio](https://speech.microsoft.com/) 入口網站
- 必要時登入
- 移至 *自訂語音*
- 選取您的專案
- 移至 *部署*
- 選取所需的端點
- 複製並儲存下欄欄位的值：
    - 端點區域 (的 **服務區域**) 
    - **端點識別碼**
  
#### <a name="create-and-submit-support-request"></a>建立並提交支援要求
針對您的資源起始增加的並行要求限制，或在必要時提交支援要求以檢查今日的限制：

- 確定您有 [必要的資訊](#prepare-the-required-information)
- 移至 [Azure 入口網站](https://portal.azure.com/)
- 選取您要增加 (的語音資源，或檢查) 並行要求限制
- 選取 [ *新增支援要求* (*支援 + 疑難排解* ] 群組)  
- 新的視窗隨即出現，其中包含 Azure 訂用帳戶和 Azure 資源的自動填入資訊
- 輸入 *摘要* (例如「增加 TTS 自訂端點並行要求限制」 ) 
- 在 [ *問題類型* ] 中，選取 [配額或訂用帳戶問題]
- 出現 *問題子類型* 時，請選取：
  - 「配額或並行要求增加」-表示增加要求
  - 「配額或使用量驗證」以檢查現有的限制
- 按 *[下一步：方案]*
- 繼續進行要求建立
- 在 [ *詳細資料* ] 索引標籤的 [ *描述* ] 欄位中輸入：
  - 請注意，要求是關於 **文字轉換語音** 配額
  - 您[之前收集](#prepare-the-required-information)的 Azure 資源資訊 
  - 完成輸入必要資訊，然後按一下 [*檢查 + 建立*] 索引標籤中的 [*建立*] 按鈕
  - 請注意 Azure 入口網站通知中的支援要求號碼。 即將聯繫您以進行進一步的處理

