---
author: paulbouwer
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 33c8e7938e3b142e1af932e550c16770355babb8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "77594101"
---
```powershell
kubectl describe pod -l "app=voting-analytics, version=1.0" -n voting | Select-String -Pattern "istio-proxy:|voting-analytics:" -Context 0,2
```

`istio-proxy`Istio 會自動插入容器，以管理流入和流出元件的網路流量，如下列範例輸出所示：

```console
>   voting-analytics:
      Container ID:   docker://35efa1f31d95ca737ff2e2229ab8fe7d9f2f8a39ac11366008f31287be4cea4d
      Image:          mcr.microsoft.com/aks/samples/voting/analytics:1.0
>   istio-proxy:
      Container ID:  docker://1fa4eb43e8d4f375058c23cc062084f91c0863015e58eb377276b20c809d43c6
      Image:         docker.io/istio/proxyv2:1.3.2
```