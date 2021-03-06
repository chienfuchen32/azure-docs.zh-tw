---
title: 教學課程：Azure Active Directory 與 Carbonite Endpoint Backup 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Carbonite Endpoint Backup 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/06/2019
ms.author: jeedes
ms.openlocfilehash: ff19275270e5b6572fb7d637b88c4736a3aa6ea0
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2020
ms.locfileid: "92456478"
---
# <a name="tutorial-integrate-carbonite-endpoint-backup-with-azure-active-directory"></a>教學課程：整合 Carbonite Endpoint Backup 與 Azure Active Directory

在本教學課程中，您會了解如何整合 Carbonite Endpoint Backup 與 Azure Active Directory (Azure AD)。 在整合 Carbonite Endpoint Backup 與 Azure AD 後，您可以︰

* 在 Azure AD 中控制可存取 Carbonite Endpoint Backup 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 Carbonite Endpoint Backup。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 Carbonite Endpoint Backup 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* Carbonite Endpoint Backup 支援由 **SP 和 IDP** 起始的 SSO

## <a name="adding-carbonite-endpoint-backup-from-the-gallery"></a>從資源庫新增 Carbonite Endpoint Backup

若要設定以將 Carbonite Endpoint Backup 整合到 Azure AD 中，您需要從資源庫將 Carbonite Endpoint Backup 新增到受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中輸入 **Carbonite Endpoint Backup** 。
1. 從結果面板選取 [Carbonite Endpoint Backup]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 Carbonite Endpoint Backup 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 Carbonite Endpoint Backup 中相關使用者之間的連結關聯性。

若要設定及測試與 Carbonite Endpoint Backup 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
2. **[設定 Carbonite Endpoint Backup SSO](#configure-carbonite-endpoint-backup-sso)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Carbonite Endpoint Backup 測試使用者](#create-carbonite-endpoint-backup-test-user)** - 在 Carbonite Endpoint Backup 中建立一個與 Azure AD 中代表 B.Simon 之使用者連結的 Britta Simon 對應項目。
6. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

### <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Carbonite Endpoint Backup]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [選取單一登入方法]  頁面上，選取 [SAML]  。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 設定]  區段上，如果您想要以 **IDP** 起始模式設定應用程式，請輸入下列欄位的值：

    a. 在 [識別碼]  文字方塊中，輸入下列其中一個 URL：

    ```http
    https://red-us.mysecuredatavault.com
    https://red-apac.mysecuredatavault.com
    https://red-fr.mysecuredatavault.com
    https://red-emea.mysecuredatavault.com
    https://kamino.mysecuredatavault.com
    ```

    b. 在 [回覆 URL]  文字方塊中，輸入下列其中一個 URL：

    ```http
    https://red-us.mysecuredatavault.com/AssertionConsumerService.aspx
    https://red-apac.mysecuredatavault.com/AssertionConsumerService.aspx
    https://red-fr.mysecuredatavault.com/AssertionConsumerService.aspx
    https://red-emea.mysecuredatavault.com/AssertionConsumerService.aspx
    ```

1. 如果您想要以 **SP** 起始模式設定應用程式，請按一下 [設定其他 URL]  ，然後執行下列步驟：

    在 [登入 URL]  文字方塊中，輸入下列其中一個 URL：

    ```http
    https://red-us.mysecuredatavault.com/
    https://red-apac.mysecuredatavault.com/
    https://red-fr.mysecuredatavault.com/
    https://red-emea.mysecuredatavault.com/
    ```

1. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，尋找 [憑證 (Base64)]  並選取 [下載]  ，以下載憑證並將其儲存在電腦上。

    ![憑證下載連結](common/certificatebase64.png)

1. 在 [設定 Carbonite Endpoint Backup]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

### <a name="configure-carbonite-endpoint-backup-sso"></a>設定 Carbonite Endpoint Backup SSO

1. 若要自動執行 Carbonite Endpoint Backup 內的設定，您必須按一下 [安裝擴充功能]  ，以安裝「我的應用程式安全登入瀏覽器擴充功能」  。

    ![我的應用程式擴充功能](common/install-myappssecure-extension.png)

2. 將擴充功能新增至瀏覽器之後，按一下 [設定 Carbonite Endpoint Backup]  便會將您導向至 Carbonite Endpoint Backup 應用程式。 請從該處提供用以登入 Carbonite Endpoint Backup 的管理員認證。 瀏覽器延伸模組將會自動為您設定應用程式，並自動執行步驟 3 到 7。

    ![設定組態](common/setup-sso.png)

3. 如果您想要手動設定 Carbonite Endpoint Backup，請開啟新的網頁瀏覽器視窗，並以系統管理員身分登入 Carbonite Endpoint Backup 公司網站，然後執行下列步驟：

4. 按一下左窗格上的 [公司]  。

    ![顯示 Carbonite 端點的螢幕擷取畫面，其中已選取 [公司]。](media/carbonite-endpoint-backup-tutorial/configure1.png)

5. 按一下 [單一登入]  。

    ![顯示已選取 [單一登入] 的公司螢幕擷取畫面。](media/carbonite-endpoint-backup-tutorial/configure2.png)

6. 按一下 [啟用]  ，然後按一下 [編輯設定]  以進行設定。

    ![顯示 [單一登入] 索引標籤的螢幕擷取畫面，其中已呼叫 [啟用] 和 [編輯] 設定。](media/carbonite-endpoint-backup-tutorial/configure3.png)

7. 在 [單一登入設定]  頁面執行下列步驟：

    ![顯示 [單一登入] 索引標籤的螢幕擷取畫面，您會在其中輸入此步驟中所述的資訊。](media/carbonite-endpoint-backup-tutorial/configure4.png)

    1. 在 [識別提供者名稱]  文字方塊中，貼上您從 Azure 入口網站複製的 [Azure AD 識別碼]  值。

    1. 在 [識別提供者 URL]  文字方塊中，貼上您從 Azure 入口網站複製的 [登入 URL]  值。

    1. 按一下 [選擇檔案]  以上傳從 Azure 入口網站下載的 **憑證 (Base64)** 檔案。

    1. 按一下 [檔案]  。

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

在本節中，您會將 Carbonite Endpoint Backup 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [Carbonite Endpoint Backup]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="create-carbonite-endpoint-backup-test-user"></a>建立 Carbonite Endpoint Backup 測試使用者

1. 在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 Carbonite Endpoint Backup 公司網站。

1. 從左窗格中按一下 [使用者]  ，然後按一下 [新增使用者]  。

    ![顯示 [Carbonite 端點] 頁面的螢幕擷取畫面，其中已選取 [使用者] 和 [新增使用者]。](media/carbonite-endpoint-backup-tutorial/adduser1.png)

1. 在 [新增使用者]  頁面上，執行下列步驟：

    ![顯示 [新增使用者] 頁面的螢幕擷取畫面，您可以在其中執行這裡所述的步驟。](media/carbonite-endpoint-backup-tutorial/adduser2.png)

    1. 輸入使用者的 [電子郵件]  、[名字]  、[姓氏]  ，並根據組織需求提供必要的權限給使用者。

    1. 按一下 [新增使用者]  。

### <a name="test-sso"></a>測試 SSO

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在「存取面板」中按一下 [Carbonite Endpoint Backup] 圖格時，應該會自動登入您已設定 SSO 的 Carbonite Endpoint Backup。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)