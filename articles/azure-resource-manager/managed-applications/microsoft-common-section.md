---
title: 區段 UI 元素
description: 描述 Azure 入口網站的 Microsoft.Common.Section UI 元素。 用來群組入口網站中的元素，以部署受控應用程式。
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 924aff8f2ba3d796b65f52494845f3b10018065c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "87063973"
---
# <a name="microsoftcommonsection-ui-element"></a>Microsoft.Common.Section UI 元素

將標題下方一個或多個元素進行群組的控制項。

## <a name="ui-sample"></a>UI 範例

![Microsoft.Common.Section](./media/managed-application-elements/microsoft-common-section.png)

## <a name="schema"></a>結構描述

```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Example section",
  "elements": [
    {
      "name": "text1",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 1"
    },
    {
      "name": "text2",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>備註

- `elements` 必須包含至少一個元素，且可以包含 `Microsoft.Common.Section` 以外的所有元素類型。
- 此元素不支援 `toolTip` 屬性。

## <a name="sample-output"></a>範例輸出
若要存取 `elements` 中的元素輸出值，可使用 [basics()](create-ui-definition-referencing-functions.md#basics) 或 [steps()](create-ui-definition-referencing-functions.md#steps) 函式和點標記法︰

```json
steps('configuration').section1.text1
```

`Microsoft.Common.Section` 類型的元素本身沒有任何輸出值。

## <a name="next-steps"></a>接下來的步驟

* 如需建立 UI 定義的簡介，請參閱[開始使用 CreateUiDefinition](create-uidefinition-overview.md)。
* 如需 UI 元素中通用屬性的說明，請參閱 [CreateUiDefinition 元素](create-uidefinition-elements.md)。
