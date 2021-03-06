---
title: Striim 快速入門
description: 快速開始使用 Striim 和 Azure Synapse Analytics。
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: d6b5fa1a42201479c5d426a5bbb9188d0cb8e03d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "89485969"
---
# <a name="striim-azure-synapse-analytics-marketplace-offering-install-guide"></a>Striim Azure Synapse Analytics Marketplace 供應專案安裝指南

本快速入門假設您已經有 Azure Synapse Analytics 的預先存在實例。

搜尋 Azure Marketplace 中的 [Striim]，然後選取 [Striim]，將 [資料整合] Azure Synapse Analytics (暫存) 選項 

![安裝 Striim][install]

使用指定的屬性設定 Striim VM，並記下 Striim 叢集名稱、密碼和管理員密碼

![設定 Striim][configure]

在部署之後，請在 Azure 入口網站中按一下 \<VM Name>-masternode，按一下 [連線]，並使用 VM 本機帳戶複製登入資料 

![將 Striim 連接到 Azure Synapse Analytics][connect]

將 sqljdbc42.jar 從 <https://www.microsoft.com/en-us/download/details.aspx?id=54671> 下載到您的本機電腦。 

開啟命令列視窗，並切換至您下載 JDBC jar 的目錄。 透過 SCP 將 jar 檔案移至您的 Striim VM，並從 Azure 入口網站中取得位址和密碼

![將 jar 檔案複製到您的 VM][copy-jar]

開啟另一個命令列視窗，或使用 ssh 公用程式透過 ssh 連線至 Striim 叢集

![透過 SSH 連線至叢集][ssh]

執行下列命令將 JDBC jar 檔案移入 Striim 的 lib 目錄，然後啟動和停止伺服器。

   1. sudo su
   2. cd /tmp
   3. mv sqljdbc42.jar /opt/striim/lib
   4. systemctl stop striim-node
   5. systemctl stop striim-dbms
   6. systemctl start striim-dbms
   7. systemctl start striim-node

![Start the Striim cluster][start-striim]

現在，開啟您慣用的瀏覽器並瀏覽至 \<DNS Name>:9080

![瀏覽至登入畫面][navigate]

使用您在 Azure 入口網站中設定的使用者名稱和密碼登入，然後選取您慣用的精靈以開始使用，或移至 [應用程式] 頁面以開始使用拖放 UI

![使用伺服器認證登入][login]



[install]: ./media/striim-quickstart/install.png
[configure]: ./media/striim-quickstart/configure.png
[connect]:./media/striim-quickstart/connect.png
[copy-jar]:./media/striim-quickstart/copy-jar.png
[ssh]:./media/striim-quickstart/ssh.png
[start-striim]:./media/striim-quickstart/start-striim.png
[navigate]:./media/striim-quickstart/navigate.png
[login]:./media/striim-quickstart/login.png
