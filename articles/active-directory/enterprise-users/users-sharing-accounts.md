---
title: 共用帳戶和認證 - Azure Active Directory | Microsoft Docs
description: 描述 Azure Active Directory 如何讓組織安全地共用內部部署應用程式和取用者雲端服務的帳戶。
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.topic: how-to
ms.date: 12/03/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: fb088d56879ebdf5d439c913ac47a701db5c4a60
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2020
ms.locfileid: "96576242"
---
# <a name="sharing-accounts-with-azure-ad"></a>使用 Azure AD 共用帳戶

## <a name="overview"></a>概觀

有時候組織需要讓多人使用同一個使用者名稱和密碼，這通常發生在兩種情況下：

* 每個使用者都必須使用唯一的登入名稱和密碼來存取應用程式時 (無論是內部部署的應用程式或取用者雲端服務，例如公司的社交媒體帳戶)。
* 建立多個使用者環境時。 您可能會有一個具備較高權限的本機帳戶，而且該帳戶用於執行核心設定、管理和復原活動。 例如，Microsoft 365 的本機「全域管理員」帳戶或 Salesforce 中的根帳號。

傳統上，這些帳戶的共用方式是透過將認證 (使用者名稱和密碼) 散發給適當的人員，或是將認證儲存在多個受信任的代理人可以存取的共用位置。

傳統共用模型有幾個缺點：

* 您必須將認證散發給需要存取新應用程式的所有人，他們才能進行存取。
* 每個共用的應用程式可能都需要唯一的一組共用認證，使用者必須記住許多組認證。 當使用者必須記住許多認證時，他們會依靠有風險的做法，風險因此會增加。 (例如，寫下密碼。)
* 您不知道誰有權存取應用程式。
* 您不知道誰 *存取* 了應用程式。
* 當您想移除某個應用程式的存取權時，您必須更新認證，並將認證重新散發給需要存取該應用程式的所有人。

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory 帳戶共用

Azure AD 提供使用共用帳戶的新方法，可以消除這些缺點。

透過使用「存取面板」並選擇最適合該應用程式的單一登入類型，Azure AD 系統管理員可以設定使用者可以存取的應用程式。 其中的「密碼單一登入」 類型在該應用程式的登入程序期間，可讓 Azure AD 做為一種「代理程式」。

使用者會以組織帳戶登入一次。 此帳戶與他們平常用來存取桌面或電子郵件的帳戶相同。 他們只能探索和存取指派給他們的那些應用程式。 使用共用帳戶，這份應用程式清單可以包含任何數目的共用認證。 使用者不需記住或寫下多個可能使用的帳戶。

共用帳戶不只增加監督的方便性和改善可用性，也可增強安全性。 具有認證使用權限的使用者看不到共用密碼，而是會在協調的驗證流程當中取得密碼的使用權限。 此外，某些密碼 SSO 應用程式可讓您選擇使用 Azure AD 以定期換用 (更新) 密碼。 系統會使用大型、複雜的密碼，以提高帳戶安全性。 管理員可以輕易地授與或撤銷應用程式的存取權，知道誰有權存取帳戶以及誰曾經存取帳戶。

對於任何 Enterprise Mobility Suite (EMS) 或 Azure AD Premium 授權方案，Azure AD 支援所有類型的密碼單一登入應用程式使用共用帳戶。 您可以共用應用程式庫中數千個預先整合的應用程式的帳戶，並可加入含有 [自訂 SSO 應用程式](../manage-apps/what-is-single-sign-on.md)的密碼驗證應用程式。

啟用帳戶共用的 Azure AD 功能包括：

* [密碼單一登入](../manage-apps/sso-options.md#password-based-sso)
* 密碼單一登入代理程式
* [群組指派](groups-self-service-management.md)
* 自訂密碼應用程式
* [應用程式使用方式儀表板/報告](../authentication/howto-sspr-reporting.md)
* 使用者存取入口網站
* [應用程式 proxy](../manage-apps/application-proxy.md)
* [Active Directory 市集](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActiveDirectory)

## <a name="sharing-an-account"></a>共用帳戶

若要使用 Azure AD 來共用帳戶，您必須：

* 新增應用程式[應用程式庫](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActiveDirectory)或[自訂應用程式](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-now-in-preview/)
* 設定應用程式使用密碼單一登入 (SSO)
* 使用[以群組為基礎的指派](groups-saasapps.md)，並選取輸入共用認證的選項

使用 Azure AD，可以透過 Multi-Factor Authentication (MFA) 讓您的共用帳戶更安全 (深入了解[使用 Azure AD 保護應用程式](../authentication/concept-mfa-howitworks.md))，並可使用 [Azure AD 自助式](groups-self-service-management.md)群組管理來委派管理誰有權存取應用程式。

## <a name="next-steps"></a>後續步驟

* [Azure Active Directory 中的應用程式管理](../manage-apps/what-is-application-management.md)
* [使用條件式存取來保護應用程式](../../active-directory-b2c/overview.md)
* [自助式群組管理/SSAA](groups-self-service-management.md)