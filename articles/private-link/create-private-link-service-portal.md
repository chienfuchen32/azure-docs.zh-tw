---
title: 快速入門 - 使用 Azure 入口網站建立 Private Link 服務
titlesuffix: Azure Private Link
description: 在本快速入門中了解如何使用 Azure 入口網站建立 Private Link 服務
services: private-link
author: asudbring
ms.service: private-link
ms.topic: quickstart
ms.date: 01/18/2021
ms.author: allensu
ms.openlocfilehash: d394a475c5121607f70c03437382e104a5d0cbee
ms.sourcegitcommit: 4d48a54d0a3f772c01171719a9b80ee9c41c0c5d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2021
ms.locfileid: "98746402"
---
# <a name="quickstart-create-a-private-link-service-by-using-the-azure-portal"></a>快速入門：使用 Azure 入口網站建立 Private Link 服務

開始建立參考您服務的 Private Link 服務。  針對在 Azure Standard Load Balancer 後方部署的服務或資源，授與 Private Link 存取權限。  您服務的使用者可以從其虛擬網路進行私人存取。

## <a name="prerequisites"></a>Prerequisites

* 具有有效訂用帳戶的 Azure 帳戶。 [免費建立帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="sign-in-to-the-azure-portal"></a>登入 Azure 入口網站

在 https://portal.azure.com 登入 Azure 入口網站。

## <a name="create-an-internal-load-balancer"></a>建立內部負載平衡器

在本節中，您會建立虛擬網路和內部 Azure Load Balancer。

### <a name="virtual-network"></a>虛擬網路

在本節中，您會建立虛擬網路和子網路，以裝載可存取 Private Link 服務的負載平衡器。

1. 在畫面的左上方，選取 [建立資源] > [網路] > [虛擬網路]  ，或在搜尋方塊中搜尋 [虛擬網路]  。

2. 在 [建立虛擬網路] 中，在 [基本] 索引標籤中輸入或選取這項資訊：

    | **設定**          | **值**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **專案詳細資料**  |                                                                 |
    | 訂用帳戶     | 選取您的 Azure 訂用帳戶                                  |
    | 資源群組   | 選取 **CreatePrivLinkService-rg** |
    | **執行個體詳細資料** |                                                                 |
    | 名稱             | 輸入 **myVNet**                                    |
    | 區域           | 選取 [美國東部 2]。 |

3. 選取 [IP 位址] 索引標籤，或選取頁面底部的 [下一步：IP 位置] 按鈕。

4. 在 [IP 位址] 索引標籤中，輸入這項資訊：

    | 設定            | 值                      |
    |--------------------|----------------------------|
    | IPv4 位址空間 | 輸入 **10.1.0.0/16** |

5. 在 [子網路名稱] 下，選取 [預設] 字組。

6. 在 [編輯子網路] 中，輸入這項資訊：

    | 設定            | 值                      |
    |--------------------|----------------------------|
    | 子網路名稱 | 輸入 mySubnet |
    | 子網路位址範圍 | 輸入 **10.1.0.0/24** |

7. 選取 [儲存]。

8. 選取 [檢閱 + 建立]  索引標籤，或選取 [檢閱 + 建立]  按鈕。

9. 選取 [建立]。

### <a name="create-a-standard-load-balancer"></a>建立標準負載平衡器

使用入口網站建立標準內部負載平衡器。 

1. 在畫面的左上方，選取 [建立資源] > [網路] > [負載平衡器]。

2. 在 [建立負載平衡器] 頁面的 [基本] 索引標籤中，輸入或選取下列資訊： 

    | 設定                 | 值                                              |
    | ---                     | ---                                                |
    | 訂用帳戶               | 選取您的訂用帳戶。    |    
    | 資源群組         | 選取在上一個步驟中建立的 **CreatePrivLinkService-rg**。|
    | 名稱                   | 輸入 **myLoadBalancer**                                   |
    | 區域         | 選取 [美國東部 2]。                                        |
    | 類型          | 選取 [內部]。                                        |
    | SKU           | 選取 [標準] |
    | 虛擬網路 | 選取您在上一個步驟中建立的 **myVNet**。 |
    | 子網路  | 選取在上一個步驟中建立的 **mySubnet**。 |
    | IP 位址指派 | 選取 [動態]。 |
    | 可用性區域 | 選取 [區域備援] |

3. 接受其餘設定的預設值，然後選取 [檢閱 + 建立]。

4. 在 [檢閱 + 建立] 索引標籤中，選取 [建立]。   

## <a name="create-load-balancer-resources"></a>建立負載平衡器資源

在本節中，您將會設定：

* 後端位址集區的負載平衡器設定。
* 健康情況探查。
* 負載平衡器規則。

### <a name="create-a-backend-pool"></a>建立後端集區

後端位址集區會包含已連線至負載平衡器之虛擬 NIC 的 IP 位址。 

建立後端位址集區 **myBackendPool**，以包含用於平衡網際網路流量負載的虛擬機器。

1. 選取左側功能表中的 [所有服務]、選取 [所有資源]，然後從資源清單中選取 **myLoadBalancer**。

2. 在 [設定] 底下選取 [後端集區]，然後選取 [新增]。

3. 在 [新增後端集區] 頁面上，針對名稱輸入 **myBackEndPool** 作為後端集區的名稱，然後選取 [新增]。

### <a name="create-a-health-probe"></a>建立健康狀態探查

負載平衡器會透過健康情況探查監視應用程式的狀態。 

健康狀態探查會根據 VM 對健康情況檢查的回應，從負載平衡器輪替中新增或移除 VM。 

建立名為 myHealthProbe 的健康狀態探查，以監視 VM 的健康狀態。

1. 選取左側功能表中的 [所有服務]、選取 [所有資源]，然後從資源清單中選取 **myLoadBalancer**。

2. 在 [設定] 底下選取 [健康狀態探查]，然後選取 [新增]。
    
    | 設定 | 值 |
    | ------- | ----- |
    | 名稱 | 輸入 **myHealthProbe**。 |
    | 通訊協定 | 選取 [TCP]。 |
    | 連接埠 | 輸入 **80**。|
    | 間隔 | 輸入 **15** 作為探查嘗試之間的 [間隔]  秒數。 |
    | 狀況不良臨界值 | 選取 [2] 作為 [狀況不良閾值] 的數值，或將 VM 視為狀況不良之前，必須達到的連續探查失敗次數。|
    | | |

3. 保留其餘的預設值，然後選取 [確定]。

### <a name="create-a-load-balancer-rule"></a>建立負載平衡器規則

負載平衡器規則用來定義如何將流量分散至 VM。 您可定義連入流量的前端 IP 組態及後端 IP 集區來接收流量。 來源和目的地連接埠會在規則中定義。 

在本節中，您將建立負載平衡器規則：

* 已命名為 **myHTTPRule**。
* 在名為 **LoadBalancerFrontEnd** 的前端中。
* 接聽 **連接埠 80**。
* 將負載平衡的流量導向至 **連接埠 80** 上名為 **myBackendPool** 的後端。

1. 選取左側功能表中的 [所有服務]、選取 [所有資源]，然後從資源清單中選取 **myLoadBalancer**。

2. 在 [設定] 下選取 [負載平衡規則]，然後選取 [新增]。

3. 使用下列值來設定負載平衡規則：
    
    | 設定 | 值 |
    | ------- | ----- |
    | 名稱 | 輸入 **myHTTPRule**。 |
    | IP 版本 | 選取 **IPv4** |
    | 前端 IP 位址 | 選取 **LoadBalancerFrontEnd** |
    | 通訊協定 | 選取 [TCP]。 |
    | 連接埠 | 輸入 **80**。|
    | 後端連接埠 | 輸入 **80**。 |
    | 後端集區 | 選取 [myBackendPool]。|
    | 健全狀況探查 | 選取 [myHealthProbe]。 |
    | 閒置逾時 (分鐘) | 將滑桿移至 **15 分鐘**。 |
    | TCP 重設 | 選取 [啟用]  。 |

4. 保留其餘的預設值，然後選取 [確定]。

## <a name="create-a-private-link-service"></a>建立私人連結服務

在本節中，您將建立標準負載平衡器後方的 Private Link 服務。

1. 在 Azure 入口網站頁面的左上角，選取 [建立資源]。

2. 在 [搜尋 Marketplace] 方塊中搜尋 **Private Link**。

3. 選取 [建立]。

4. 在 [Private Link 中心] 下的 [概觀] 中，選取藍色 [建立私人連結服務] 按鈕。

5. 在 [建立私人連結服務] 下的 [基本] 索引標籤中，輸入或選取下列資訊：

    | 設定 | 值 |
    | ------- | ----- |
    | **專案詳細資料** |  |
    | 訂用帳戶 | 選取您的訂用帳戶。 |
    | 資源群組 | 選取 **CreatePrivLinkService-rg**。 |
    | **執行個體詳細資料** |  |
    | 名稱 | 輸入 **myPrivateLinkService**。 |
    | 區域 | 選取 [美國東部 2]。 |

6. 選取 [輸出設定] 索引標籤，或在頁面底部選取 [下一步：輸出設定]。

7. 在 [輸出設定] 索引標籤中，輸入或選取以下資訊：

    | 設定 | 值 |
    | ------- | ----- |
    | 負載平衡器 | 選取 [myLoadBalancer]。 |
    | 負載平衡器前端 IP 位址 | 選取 **LoadBalancerFrontEnd (10.1.0.4)** 。 |
    | 來源 NAT 子網路 | 選取 **mySubnet (10.1.0.0/24)** 。 |
    | 啟用 TCP Proxy V2 | 保留預設值 [否]。 </br> 如果您的應用程式需要 TCP Proxy v2 標頭，選取 [是]。 |
    | **私人 IP 位址設定** |  |
    | 保留預設設定 |  |

8. 選取 [存取安全性] 索引標籤，或在頁面底部選取 [下一步：存取安全性] 按鈕。

9. 在 [存取安全性] 索引標籤中，保留 [僅角色型存取控制] 預設值。

10. 選取 [標記] 索引標籤，或在頁面底部選取 [下一步：標記]。

11. 選取 [檢閱 + 建立] 索引標籤，或在頁面底部選取 [下一步：檢閱 + 建立]。

12. 在 [檢閱 + 建立] 索引標籤中，選取 [建立]。

系統會建立您的私人連結服務，並可接收流量。 如果您想要查看流量流程，請在您的標準負載平衡器後方設定您的應用程式。


## <a name="create-private-endpoint"></a>建立私人端點

在本節中，您會將 private link 服務對應至私人端點。 虛擬網路包含私用連結服務的私人端點。 此虛擬網路包含可存取私用連結服務的資源。

### <a name="create-private-endpoint-virtual-network"></a>建立私人端點虛擬網路

1. 在畫面的左上方，選取 [建立資源] > [網路] > [虛擬網路]  ，或在搜尋方塊中搜尋 [虛擬網路]  。

2. 在 [建立虛擬網路] 中，在 [基本] 索引標籤中輸入或選取這項資訊：

    | **設定**          | **值**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **專案詳細資料**  |                                                                 |
    | 訂用帳戶     | 選取您的 Azure 訂用帳戶                                  |
    | 資源群組   | 選取 **CreatePrivLinkService-rg** |
    | **執行個體詳細資料** |                                                                 |
    | 名稱             | 輸入 **myVNetPE**                                    |
    | 區域           | 選取 [美國東部 2]。 |

3. 選取 [IP 位址] 索引標籤，或選取頁面底部的 [下一步：IP 位置] 按鈕。

4. 在 [IP 位址] 索引標籤中，輸入這項資訊：

    | 設定            | 值                      |
    |--------------------|----------------------------|
    | IPv4 位址空間 | 輸入 **11.1.0.0/16** |

5. 在 [子網路名稱] 下，選取 [預設] 字組。

6. 在 [編輯子網路] 中，輸入這項資訊：

    | 設定            | 值                      |
    |--------------------|----------------------------|
    | 子網路名稱 | 輸入 **mySubnetPE** |
    | 子網路位址範圍 | 輸入 **11.1.0.0/24** |

7. 選取 [儲存]。

8. 選取 [檢閱 + 建立]  索引標籤，或選取 [檢閱 + 建立]  按鈕。

9. 選取 [建立]  。

### <a name="create-private-endpoint"></a>建立私人端點

1. 在入口網站的畫面左上角，選取 [建立資源] > [網路] > [Private Link]，或在搜尋方塊中輸入 **Private Link**。

2. 選取 [建立]。

3. 在 **Private Link 中心** 內，選取左側功能表中的 [私人端點]。

4. 在 [私人端點] 中，選取 [+ 新增]。

5. 在 [建立私人端點] 的 [基本] 索引標籤中，輸入或選取這項資訊：

    | 設定 | 值 |
    | ------- | ----- |
    | **專案詳細資料** | |
    | 訂用帳戶 | 選取您的訂用帳戶。 |
    | 資源群組 | 選取 **CreatePrivLinkService-rg**。 您已在上一節中建立此資源群組。|
    | **執行個體詳細資料** |  |
    | 名稱  | 輸入 myPrivateEndpoint。 |
    | 區域 | 選取 [美國東部 2]。 |

6. 選取 [資源] 索引標籤或 [下一步：資源] 按鈕 (位於頁面底部)。
    
7. 在 [資源] 中，輸入或選取這項資訊：

    | 設定 | 值 |
    | ------- | ----- |
    | 連線方法 | 選取 [連線到我目錄中的 Azure 資源]。 |
    | 訂用帳戶 | 選取您的訂用帳戶。 |
    | 資源類型 | 選取 [ **Microsoft. Network/privateLinkServices**]。 |
    | 資源 | 選取 [ **myPrivateLinkService**]。 |

8. 選取 [設定] 索引標籤或 [下一步：設定] 按鈕 (位於畫面底部)。

9. 在 [設定] 中，輸入或選取這項資訊：

    | 設定 | 值 |
    | ------- | ----- |
    | **網路功能** |  |
    | 虛擬網路 | 選取 [ **myVNetPE**]。 |
    | 子網路 | 選取 [ **mySubnetPE**]。 |

10. 選取 [ **審核 + 建立** ] 索引標籤，或畫面底部的 [ **審核 + 建立** ] 按鈕。

11. 選取 [建立]  。

### <a name="ip-address-of-private-endpoint"></a>私人端點的 IP 位址

在本節中，您會找到與負載平衡器和私用連結服務對應的私人端點 IP 位址。

1. 在 Azure 入口網站的左側資料行中，選取 [ **資源群組**]。

2. 選取 **CreatePrivLinkService-rg** 資源群組。

3. 在 **CreatePrivLinkService-rg** 資源群組中，選取 [ **myPrivateEndpoint**]。

4. 在 **myPrivateEndpoint** 的 [**總覽**] 頁面中，選取與私人端點相關聯的網路介面名稱。  網路介面名稱的開頭為 **myPrivateEndpoint nic**。

5. 在私人端點 nic 的 [ **總覽** ] 頁面中，端點的 ip 位址會顯示在 [ **私人 IP 位址**] 中。
    

## <a name="clean-up-resources"></a>清除資源

當您使用 private link 服務完成時，請刪除資源群組以清除本快速入門中使用的資源。

1. 在入口網站頂端的 [搜尋] 方塊中，輸入 [CreatePrivLinkService-rg]，然後從搜尋結果中選取 [CreatePrivLinkService-rg]。
1. 選取 [刪除資源群組]。
1. 在 [輸入資源群組名稱] 中，輸入 [CreatePrivLinkService-rg]。
1. 選取 [刪除]。

## <a name="next-steps"></a>後續步驟

在本快速入門中，您已：

* 建立虛擬網路和內部 Azure Load Balancer。
* 已建立 private link 服務。
* 建立私人連結服務的虛擬網路和私人端點。

若要深入了解 Azure 私人端點，請繼續：
> [!div class="nextstepaction"]
> [快速入門：使用 Azure 入口網站建立私人端點](create-private-endpoint-portal.md)