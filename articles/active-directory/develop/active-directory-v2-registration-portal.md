---
title: 應用程式註冊入口網站參考 | Azure
titleSuffix: Microsoft identity platform
description: 描述 Microsoft 應用程式註冊入口網站中的功能。
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: ryanwi
ms.reviewer: lenalepa
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 6a33da602eaa9bee20f155eaa550e558e5dcbeca
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/25/2021
ms.locfileid: "98755555"
---
# <a name="app-registration-reference"></a>App 註冊參考

本文件提供可在 Azure 入口網站[應用程式註冊](https://aka.ms/appregistrations)體驗中找到各種功能的內容及描述。

## <a name="my-applications-or-converged-applications"></a>我的應用程式或交集的應用程式

這份清單包含所有已註冊以與 Microsoft 身分識別平臺搭配使用的應用程式。 這些應用程式能夠讓使用者使用個人的 Microsoft 帳戶登入，也可使用工作/學校帳戶從 Azure Active Directory 登入。 若要深入瞭解 Microsoft 身分識別平臺，請參閱 [2.0 版總覽](./v2-overview.md)。 這些應用程式也可以用來與 Microsoft 帳戶驗證端點 `https://login.live.com`整合。

## <a name="azure-ad-only-applications"></a>僅限 Azure AD 的應用程式

此清單包含所有已註冊且可與 Azure AD v1.0 端點搭配使用的應用程式。 這些應用程式只能使用 Azure Active Directory 中的工作/學校帳戶來登入使用者。 這份清單包含已在 [Azure 入口網站](https://portal.azure.com)中使用 **應用程式註冊** 體驗來註冊的應用程式。

## <a name="live-sdk-applications"></a>Live SDK 應用程式

此清單包含所有已註冊且只能與 Microsoft 帳戶搭配使用的應用程式。 它們都不會啟用來與 Azure Active Directory 搭配使用。 您可以在此處找到任何先前已於 MSA 開發人員入口網站`https://account.live.com/developers/applications`註冊的應用程式。 先前曾在 `https://account.live.com/developers/applications` 執行的所有功能現在可在[應用程式註冊](https://aka.ms/appregistrations)中執行。

## <a name="application-secrets"></a>應用程式密碼

應用程式秘密是可讓您的應用程式使用 Microsoft 身分識別平臺執行可靠 [用戶端驗證](https://tools.ietf.org/html/rfc6749#section-2.3) 的認證。 在 OAuth 和 OpenID Connect 中，應用程式密碼通常稱為 `client_secret`。 在 v2.0 通訊協定中，任何在 web 可定址位置收到安全性權杖的應用程式 (使用 `https` 配置) 必須使用應用程式密碼，以在兌換該安全性權杖時，向 Microsoft 身分識別平臺識別其身分。 此外，在裝置上收到權杖的任何原生用戶端都將禁止使用應用程式密碼來執行用戶端驗證。 這項限制可防止在不安全的環境中儲存機密資料。

每個應用程式最後都能在任何時間點包含兩個有效的應用程式密碼。 藉由維護兩個密碼，您就能夠在應用程式的整個環境中執行定期的金鑰變換。 一旦將應用程式的全部內容移轉至新的密碼之後，您可能會刪除舊的密碼，並佈建一個新密碼。

此時，App 註冊入口網站中只允許兩種類型的應用程式密碼。 選擇 [產生新密碼]  將會在各自的資料存放區中產生並儲存共用的密碼，讓您可以在應用程式中加以使用。 選擇 [ **產生新的金鑰** 組] 會建立新的公開/私密金鑰組，可供下載並用於 Microsoft 身分識別平臺的用戶端驗證。 選擇 [上傳公開金鑰] 可讓您使用自己的公開/私密金鑰組。
您必須上傳含有公開金鑰的憑證。

## <a name="profile"></a>設定檔

應用程式註冊入口網站的設定檔區段可以用來自訂您應用程式的登入頁面。 此時，您可以改變登入頁面的應用程式標誌、服務條款 URL 及隱私權聲明 URL。 標誌必須是 GIF、PNG 或 JPEG 檔案中透明的 48 x 48 或 50 x 50 像素影像，且大小等於或小於 15 KB。 請嘗試變更數值，然後檢視產生的登入頁面！

## <a name="live-sdk-support"></a>Live SDK 支援

當您啟用「Live SDK 支援」時，即會將您建立的任何應用程式密碼佈建到 Azure AD 與 Microsoft 帳戶資料存放區。 這讓您的應用程式能夠直接與 Microsoft 帳戶服務 (login.live.com) 整合。 如果想要使用 Microsoft 帳戶直接建置應用程式 (而不使用 v2.0 端點)，則應確定已啟用 Live SDK 支援。

停用 Live SDK 支援可確保系統只會將應用程式密碼寫入 Azure AD 資料存放區。 Azure AD 資料存放區包含企業級法規，使其符合特定的標準，例如 FISMA 相容性。 如果您啟用 Live SDK 支援，您的應用程式可能不會遵守這其中的部分標準。

如果只打算使用 v2.0 端點，您可安全地停用 Live SDK 支援。