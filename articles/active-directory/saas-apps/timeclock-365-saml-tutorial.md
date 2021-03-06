---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 Timeclock 365 SAML 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Timeclock 365 SAML 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/16/2020
ms.author: jeedes
ms.openlocfilehash: 329e0fbfdea43f3933333744f89ed1c03ef0ed13
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98729275"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-timeclock-365-saml"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 Timeclock 365 SAML 整合

在此教學課程中，您將了解如何將 Timeclock 365 SAML 與 Azure Active Directory (Azure AD) 整合。 在將 Timeclock 365 SAML 與 Azure AD 整合時，您可以︰

* 在 Azure AD 中控制可存取 Timeclock 365 SAML 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 Timeclock 365 SAML。
* 在 Azure 入口網站集中管理您的帳戶。

## <a name="prerequisites"></a>必要條件

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 Timeclock 365 SAML 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* Timeclock 365 SAML 支援由 **SP** 起始的 SSO

## <a name="adding-timeclock-365-saml-from-the-gallery"></a>從資源庫新增 Timeclock 365 SAML

若要設定 Timeclock 365 SAML 與 Azure AD 整合，您需要從資源庫將 Timeclock 365 SAML 新增到受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 Azure 入口網站。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory] 服務。
1. 巡覽至 [企業應用程式]，然後選取 [所有應用程式]。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增] 區段的搜尋方塊中，輸入 **Timeclock 365 SAML**。
1. 從結果面板選取 [Timeclock 365 SAML]，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。


## <a name="configure-and-test-azure-ad-sso-for-timeclock-365-saml"></a>設定和測試 Timeclock 365 SAML 的 Azure AD SSO

以名為 **B.Simon** 的測試使用者，設定及測試與 Timeclock 365 SAML 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 Timeclock 365 SAML 中相關使用者之間的連結關聯性。

若要設定及測試與 Timeclock 365 SAML 搭配運作的 Azure AD SSO，請執行下列步驟：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    1. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    1. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 Timeclock 365 SAML SSO](#configure-timeclock-365-saml-sso)** - 在應用程式端設定單一登入設定。
    1. **[建立 Timeclock 365 SAML 測試使用者](#create-timeclock-365-saml-test-user)** - 使 Timeclock 365 SAML 中對應的 B.Simon 連結到該使用者在 Azure AD 中的代表項目。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 Azure 入口網站的 **Timeclock 365 SAML** 應用程式整合頁面上，尋找 **管理** 區段並選取 [單一登入]。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 組態]  區段上，輸入下列欄位的值：

    在 [登入 URL] 文字方塊中，輸入 URL：`https://live.timeclock365.com/login`


1. 在 [以 SAML 設定單一登入] 頁面的 [SAML 簽署憑證] 區段中，按一下 [複製] 按鈕以複製 [應用程式同盟中繼資料 URL]，並將資料儲存在您的電腦上。

    ![憑證下載連結](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

在本節中，您將在 Azure 入口網站中建立名為 B.Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。
1. 在畫面頂端選取 [新增使用者]  。
1. 在 [使用者]  屬性中，執行下列步驟：
   1. 在 [名稱]  欄位中，輸入 `B.Simon`。  
   1. 在 [使用者名稱]  欄位中，輸入 username@companydomain.extension。 例如： `B.Simon@contoso.com` 。
   1. 選取 [顯示密碼]  核取方塊，然後記下 [密碼]  方塊中顯示的值。
   1. 按一下頁面底部的 [新增]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 Timeclock 365 SAML 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [Timeclock 365 SAML]。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。
1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。
1. 在 [使用者和群組] 對話方塊的 [使用者] 清單中選取 [B.Simon]，然後按一下畫面底部的 [選取] 按鈕。
1. 如果您需要將角色指派給使用者，您可以從 [選取角色] 下拉式清單中選取。 如果未設定此應用程式的角色，您會看到已選取 [預設存取] 角色。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

## <a name="configure-timeclock-365-saml-sso"></a>設定 Timeclock 365 SAML SSO

1. 在瀏覽器中開啟新索引標籤，以系統管理員的身分登入您的 Timeclock 365 SAML 公司網站。

1. 執行以下所述的步驟。

    ![Timeclock 組態](./media/timeclock-365-saml-tutorial/saml-configuration.png)

    a. 移至 **設定 > 公司設定檔 > 設定** 索引標籤。

    b. 在 [IDP 中繼資料路徑] 中，貼上您從 Azure 入口網站複製的 [應用程式同盟中繼資料 URL]。

    c. 接著按一下 [更新]。

### <a name="create-timeclock-365-saml-test-user"></a>建立 Timeclock 365 SAML 測試使用者

1. 在瀏覽器中開啟新索引標籤，以系統管理員的身分登入您的 Timeclock 365 SAML 公司網站。

1. 移至 **使用者 > 新增使用者**。

    ![建立測試使用者 1 ](./media/timeclock-365-saml-tutorial/add-user-1.png)

1. 在 **使用者資訊** 頁面中提供所有必要的資訊，然後按一下 [儲存]。

    ![建立測試使用者 2 ](./media/timeclock-365-saml-tutorial/add-user-2.png)

1. 按一下 [建立] 按鈕以建立測試使用者。

## <a name="test-sso"></a>測試 SSO 

在本節中，您會使用下列選項來測試您的 Azure AD 單一登入組態。 

* 在 Azure 入口網站中按一下 [測試此應用程式]。 這會將您重新導向至可以在其中起始登入流程的 Timeclock 365 SAML 登入 URL。 

* 直接移至 Timeclock 365 SAML 登入 URL，然後從該處起始登入流程。

* 您可以使用 Microsoft 的「我的應用程式」。 當您按一下「我的應用程式」中的 [Timeclock 365 SAML] 圖格時，將會重新導向至 Timeclock 365 SAML 登入 URL。 如需「我的應用程式」的詳細資訊，請參閱[我的應用程式簡介](../user-help/my-apps-portal-end-user-access.md)。


## <a name="next-steps"></a>後續步驟

設定 Timeclock 365 SAML 後，您可以強制執行工作階段控制項，以即時防止組織的敏感資料遭到外洩和滲透。 工作階段控制項會從條件式存取延伸。 [了解如何使用 Microsoft Cloud App Security 來強制執行工作階段控制項](/cloud-app-security/proxy-deployment-any-app)。