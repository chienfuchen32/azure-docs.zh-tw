---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 10/15/2020
ms.author: alkohli
ms.openlocfilehash: ca060e75e50a3e2327fc0516c3cfc9550afbf90f
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2020
ms.locfileid: "96581802"
---
1. [連接到 PowerShell 介面](#connect-to-the-powershell-interface)。
2. 使用 `Get-HcsApplianceInfo` 取得裝置的資訊。

    下列範例顯示此 Cmdlet 的使用方式：

    ```
    [10.100.10.10]: PS>Get-HcsApplianceInfo
    
    Id                            : b2044bdb-56fd-4561-a90b-407b2a67bdfc
    FriendlyName                  : DBE-NBSVFQR94S6
    Name                          : DBE-NBSVFQR94S6
    SerialNumber                  : HCS-NBSVFQR94S6
    DeviceId                      : 40d7288d-cd28-481d-a1ea-87ba9e71ca6b
    Model                         : Virtual
    FriendlySoftwareVersion       : Data Box Gateway 1902
    HcsVersion                    : 1.4.771.324
    IsClustered                   : False
    IsVirtual                     : True
    LocalCapacityInMb             : 1964992
    SystemState                   : Initialized
    SystemStatus                  : Normal
    Type                          : DataBoxGateway
    CloudReadRateBytesPerSec      : 0
    CloudWriteRateBytesPerSec     : 0
    IsInitialPasswordSet          : True
    FriendlySoftwareVersionNumber : 1902
    UploadPolicy                  : All
    DataDiskResiliencySettingName : Simple
    ApplianceTypeFriendlyName     : Data Box Gateway
    IsRegistered                  : False
    ```

    以下是摘要一些重要裝置資訊的表格：

    | 參數 | 描述 |
    |-----------|-------------|
    | FriendlyName                   | 裝置在部署期間透過本機 web UI 設定的易記名稱。 預設的易記名稱是裝置序號。  |
    | SerialNumber                   | 裝置序號是在工廠指派的唯一編號。                                                                             |
    | 模型                          | 裝置的型號。 適用于資料箱閘道的模型是虛擬的。                   |
    | FriendlySoftwareVersion        | 對應至裝置軟體版本的易記字串。 針對執行預覽的系統，易記的軟體版本會是 Data Box Edge 1902。 |
    | HcsVersion                     | 裝置上執行的 HCS 軟體版本。 例如，對應至 Data Box Edge 1902 的 HCS 軟體版本為1.4.771.324。            |
    | LocalCapacityInMb              | 裝置的本機容量總計（以 Mb 為單位）。                                                                                                        |
    | IsRegistered                   | 此值指出您的裝置是否已啟用服務。                                                                                         |


