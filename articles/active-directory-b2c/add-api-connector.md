---
title: '將 API 連接器新增至使用者流程 (預覽版) '
description: 設定要在使用者流程中使用的 API 連接器。
services: active-directory-b2c
ms.service: active-directory
ms.subservice: B2C
ms.topic: how-to
ms.date: 10/15/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.openlocfilehash: 9117474c3cbf5087a5b63512fcc17c4771bf7aa6
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2020
ms.locfileid: "96343870"
---
# <a name="add-an-api-connector-to-a-sign-up-user-flow-preview"></a>將 API 連接器新增至註冊使用者流程 (預覽版) 

> [!IMPORTANT]
> 適用于註冊的 API 連接器是 Azure AD B2C 的公開預覽功能。 如需有關預覽版的詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

若要使用 [api 連接器](api-connectors-overview.md)，請先建立 api 連接器，然後在使用者流程中啟用它。

## <a name="create-an-api-connector"></a>建立 API 連接器

1. 登入 [Azure 入口網站](https://portal.azure.com/)。
2. 在 [ **Azure 服務**] 下，選取 [ **Azure AD B2C**]。
4. 選取 [ **api 連接器 (預覽])**，然後選取 [ **新增 api 連接器**]。

   ![新增 API 連接器](./media/add-api-connector/api-connector-new.png)

5. 提供通話的顯示名稱。 例如， **驗證使用者資訊**。
6. 提供 API 呼叫的 **端點 URL** 。
7. 提供 API 的驗證資訊。

   - 目前僅支援基本驗證。 如果您想要在開發用途不使用基本驗證的情況下使用 API，只要輸入您的 API 可以忽略的「虛擬」使用者 **名稱** 和 **密碼** 即可。 若要搭配使用 Azure 函式與 API 金鑰，您可以在 **端點 URL** 中包含程式碼做為查詢參數 (例如 HTTPs： []() //contoso.azurewebsites.net/api/endpoint <b>？ code = 0123456789</b>) 。

   ![設定新的 API 連接器](./media/add-api-connector/api-connector-config.png)
8. 選取 [儲存]。

## <a name="the-request-sent-to-your-api"></a>傳送給 API 的要求
API 連接器具體化為 **HTTP POST** 要求，會將使用者屬性 ( ' 宣告 ' ) 作為 JSON 主體中的機碼值組。 屬性的序列化方式類似于 [Microsoft Graph](/graph/api/resources/user#properties) 使用者屬性。 

**範例要求**
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "surname":"Smith",
 "jobTitle":"Supplier",
 "streetAddress":"1000 Microsoft Way",
 "city":"Seattle",
 "postalCode": "12345",
 "state":"Washington",
 "country":"United States",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```

只有 **Azure AD B2C** 使用者屬性體驗中列出的使用者屬性和自訂屬性才  >  **User attributes** 可在要求中傳送。

自訂屬性存在於目錄中 **extension_ \<extensions-app-id> _CustomAttribute**  格式。 您的 API 預期會以相同的序列化格式接收宣告。 如需自訂屬性的詳細資訊，請參閱 [在 Azure Active Directory B2C 中定義自訂屬性](user-flow-custom-attributes.md)。

此外，預設會在所有要求中傳送 **( ' ui_locales ' ) 宣告的 UI 地區** 設定。 它會提供使用者的地區設定 (s) 如其裝置上所設定，可供 API 用來傳回國際化回應。

> [!IMPORTANT]
> 如果宣告在呼叫 API 端點時沒有值，則不會將宣告傳送至 API。 您的 API 應設計成明確檢查並處理要求中不會有宣告的情況。

> [!TIP] 
> 身分識別 ( 「身分識別」 [**)**](/graph/api/resources/objectidentity)和 **電子郵件地址 ( 「電子郵件」 )** 宣告可供您的 API 用來識別使用者，然後他們在您的租使用者中具有帳戶。 

## <a name="enable-the-api-connector-in-a-user-flow"></a>在使用者流程中啟用 API 連接器

請遵循下列步驟，將 API 連接器新增至註冊使用者流程。

1. 登入 [Azure 入口網站](https://portal.azure.com/)。
2. 在 [ **Azure 服務**] 下，選取 [ **Azure AD B2C**]。
4. 選取 [ **使用者流程**]，然後選取您想要新增 API 連接器的使用者流程。
5. 選取 [ **api 連接器**]，然後選取您想要在使用者流程的下列步驟中叫用的 api 端點：

   - **使用身分識別提供者登入之後**
   - **建立使用者之前**

   ![將 Api 新增至使用者流程](./media/add-api-connector/api-connectors-user-flow-select.png)

6. 選取 [儲存]。

## <a name="after-signing-in-with-an-identity-provider"></a>使用身分識別提供者登入之後

在使用者向身分識別 (提供者（例如 Google、Facebook、& Azure AD) ）進行驗證之後，會立即叫用註冊程式中這個步驟的 API 連接器。 此步驟位於 **_屬性集合頁面_* _ 上方，這是向使用者顯示的表單，以收集使用者屬性。 如果使用者正在使用本機帳戶註冊，就不會叫用這個步驟。

### <a name="example-request-sent-to-the-api-at-this-step"></a>範例要求會在此步驟傳送至 API
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ 
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "lastName":"Smith",
 "ui_locales":"en-US"
}
```

傳送至 API 的確切宣告取決於身分識別提供者所提供的資訊。 一律會傳送「電子郵件」。

### <a name="expected-response-types-from-the-web-api-at-this-step"></a>此步驟中 web API 預期的回應類型

當 web API 在使用者流程中從 Azure AD 接收 HTTP 要求時，它可能會傳回下列回應：

- 接續回應
- 封鎖回應

#### <a name="continuation-response"></a>接續回應

接續回應表示使用者流程應該繼續進行下一個步驟： [屬性集合] 頁面。

在接續回應中，API 可以傳回宣告。 如果 API 傳回宣告，宣告會執行下列動作：

- 預先填入 [屬性集合] 頁面中的輸入欄位。

請參閱 [接續回應](#example-of-a-continuation-response)的範例。

#### <a name="blocking-response"></a>封鎖回應

封鎖回應會離開使用者流程。 您可以刻意發出 API，藉由向使用者顯示封鎖頁面，以停止使用者流程的接續。 [封鎖] 頁面會顯示 `userMessage` API 提供的。

請參閱 [封鎖回應](#example-of-a-blocking-response)的範例。

## <a name="before-creating-the-user"></a>建立使用者之前

註冊程式中這個步驟的 API 連接器會在屬性集合頁面（如果包含的話）之後叫用。 在建立使用者帳戶之前，一律會叫用此步驟。

<!-- The following are examples of scenarios you might enable at this point during sign-up: -->
<!-- 
- Validate user input data and ask a user to resubmit data.
- Block a user sign-up based on data entered by the user.
- Perform identity verification.
- Query external systems for existing data about the user and overwrite the user-provided value. -->

### <a name="example-request-sent-to-the-api-at-this-step"></a>範例要求會在此步驟傳送至 API

```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "surname":"Smith",
 "jobTitle":"Supplier",
 "streetAddress":"1000 Microsoft Way",
 "city":"Seattle",
 "postalCode": "12345",
 "state":"Washington",
 "country":"United States",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```
傳送至 API 的確切宣告取決於從使用者收集的資訊，或是由身分識別提供者所提供的資訊。

### <a name="expected-response-types-from-the-web-api-at-this-step"></a>此步驟中 web API 預期的回應類型

當 web API 在使用者流程中從 Azure AD 接收 HTTP 要求時，它可能會傳回下列回應：

- 接續回應
- 封鎖回應
- 驗證回應

#### <a name="continuation-response"></a>接續回應

接續回應表示使用者流程應該繼續進行下一個步驟：在目錄中建立使用者。

在接續回應中，API 可以傳回宣告。 如果 API 傳回宣告，宣告會執行下列動作：

- 覆寫任何已從 [屬性集合] 頁面指派給宣告的值。

請參閱 [接續回應](#example-of-a-continuation-response)的範例。

#### <a name="blocking-response"></a>封鎖回應
封鎖回應會離開使用者流程。 您可以刻意發出 API，藉由向使用者顯示封鎖頁面，以停止使用者流程的接續。 [封鎖] 頁面會顯示 `userMessage` API 提供的。

請參閱 [封鎖回應](#example-of-a-blocking-response)的範例。

### <a name="validation-error-response"></a>驗證-錯誤回應
 當 API 回應驗證錯誤回應時，使用者流程會保留在 [屬性集合] 頁面上，並 `userMessage` 向使用者顯示。 然後，使用者可以編輯並重新提交表單。 這種類型的回應可用於輸入驗證。

請參閱 [驗證錯誤回應](#example-of-a-validation-error-response)的範例。

## <a name="example-responses"></a>範例回應

### <a name="example-of-a-continuation-response"></a>接續回應的範例

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "Continue",
    "postalCode": "12349", // return claim
    "extension_<extensions-app-id>_CustomAttribute": "value" // return claim
}
```

| 參數                                          | 類型              | 必要 | 描述                                                                                                                                                                                                                                                                            |
| -------------------------------------------------- | ----------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| version                                            | String            | 是      | API 的版本。                                                                                                                                                                                                                                                                |
| 動作                                             | String            | 是      | 值必須為 `Continue`。                                                                                                                                                                                                                                                              |
| \<builtInUserAttribute>                            | \<attribute-type> | 否       | 傳回的值會覆寫從使用者收集的值。 如果選取為 _ * 應用程式宣告 * *，也可以在權杖中傳回。                                              |
| \<extension\_{extensions-app-id}\_CustomAttribute> | \<attribute-type> | 否       | 宣告不需要包含 `_<extensions-app-id>_` 。 傳回的值會覆寫從使用者收集的值。 如果選取做為 **應用程式** 宣告，也可以在權杖中傳回。  |

### <a name="example-of-a-blocking-response"></a>封鎖回應的範例

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "There was a problem with your request. You are not able to sign up at this time.",
}

```

| 參數   | 類型   | 必要 | 描述                                                                |
| ----------- | ------ | -------- | -------------------------------------------------------------------------- |
| version     | String | 是      | API 的版本。                                                    |
| 動作      | String | 是      | 值必須是 `ShowBlockPage`                                              |
| userMessage | String | 是      | 要向使用者顯示的訊息。                                            |

**封鎖回應的終端使用者體驗**

![區塊頁面範例](./media/add-api-connector/blocking-page-response.png)

### <a name="example-of-a-validation-error-response"></a>驗證錯誤回應的範例



```http
HTTP/1.1 400 Bad Request
Content-type: application/json

{
    "version": "1.0.0",
    "status": 400,
    "action": "ValidationError",
    "userMessage": "Please enter a valid Postal Code."
}
```

| 參數   | 類型    | 必要 | 描述                                                                |
| ----------- | ------- | -------- | -------------------------------------------------------------------------- |
| version     | String  | 是      | API 的版本。                                                    |
| 動作      | String  | 是      | 值必須為 `ValidationError`。                                           |
| status      | 整數 | 是      | 必須是 `400` ValidationError 回應的值。                        |
| userMessage | String  | 是      | 要向使用者顯示的訊息。                                            |

> [!NOTE]
> 除了回應主體中的「狀態」值之外，HTTP 狀態碼必須是 "400"。

**使用驗證錯誤回應的使用者體驗**

![範例驗證頁面](./media/add-api-connector/validation-error-postal-code.png)


## <a name="best-practices-and-how-to-troubleshoot"></a>最佳做法及如何進行疑難排解

### <a name="using-serverless-cloud-functions"></a>使用無伺服器雲端函數
無伺服器函式（例如 Azure Functions 中的 HTTP 觸發程式）提供簡單的方式來建立 API 端點，以搭配 API 連接器使用。 [例如](code-samples.md#api-connectors)，您可以使用無伺服器雲端函式來執行驗證邏輯，並限制特定電子郵件網域的註冊。 無伺服器雲端函式也可以針對更複雜的案例，呼叫並叫用其他 web Api、使用者存放區及其他雲端服務。

### <a name="best-practices"></a>最佳作法
請確定：
* 您的 API 正遵循 API 要求和回應合約，如上所述。 
* API 連接器的 **端點 URL** 會指向正確的 api 端點。
* 您的 API 會明確檢查收到的宣告是否有 null 值。
* 您的 API 會儘快回應，以確保流暢的使用者體驗。
    * 如果使用無伺服器函式或可擴充的 web 服務，請使用可保持 API 「喚醒」或「暖」的主控方案。 在生產環境中。 針對 Azure Functions，建議使用 [Premium 方案](../azure-functions/functions-scale.md)


### <a name="use-logging"></a>使用記錄
一般而言，使用您的 web API 服務（例如 [Application insights](../azure-functions/functions-monitoring.md)）所啟用的記錄工具，來監視您的 API 是否有未預期的錯誤碼、例外狀況和效能不佳的情況，會很有説明。
* 監視非 HTTP 200 或400的 HTTP 狀態碼。
* 401或 403 HTTP 狀態碼通常表示您的驗證有問題。 再次檢查 API 的驗證層，以及 API 連接器中的對應設定。
* 如有需要，請使用更積極的記錄層級 (例如「追蹤」或「debug」 ) 開發。
* 監視您的 API 是否有很長的回應時間。

## <a name="next-steps"></a>後續步驟
<!-- - Learn how to [add a custom approval workflow to sign-up](add-approvals.md) -->
- 開始使用我們的 [範例](code-samples.md#api-connectors)。
