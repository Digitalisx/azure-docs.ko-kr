---
title: 인포박스 UI 엘리먼트
description: Azure 포털에 대한 Microsoft.Common.InfoBox UI 요소에 대해 설명합니다. 관리되는 응용 프로그램을 배포할 때 텍스트 또는 경고를 추가하는 데 사용합니다.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: tomfitz
ms.openlocfilehash: 6d1e4a84904ef7022d9ce85803bf10285bf0b8ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75652477"
---
# <a name="microsoftcommoninfobox-ui-element"></a>Microsoft.Common.InfoBox UI 요소

정보 상자를 추가하는 컨트롤입니다. 이 상자에는 사용자가 입력하는 값을 파악하는 데 도움이 되는 중요한 텍스트나 경고가 포함됩니다. 상자를 자세한 정보를 제공하는 URI에 연결할 수도 있습니다.

## <a name="ui-sample"></a>UI 샘플

![Microsoft.Common.InfoBox](./media/managed-application-elements/microsoft.common.infobox.png)


## <a name="schema"></a>스키마

```json
{
  "name": "text1",
  "type": "Microsoft.Common.InfoBox",
  "visible": true,
  "options": {
    "icon": "None",
    "text": "Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo.",
    "uri": "https://www.microsoft.com"
  }
}
```

## <a name="sample-output"></a>샘플 출력

```json
"Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo."
```

## <a name="remarks"></a>설명

* `icon`의 경우 **없음**, **정보**, **경고** 또는 **오류**를 사용합니다.
* `uri` 속성은 선택 사항입니다.

## <a name="next-steps"></a>다음 단계

* UI 정의 만들기에 대한 소개는 [CreateUiDefinition 시작](create-uidefinition-overview.md)을 참조하세요.
* UI 요소의 공용 속성에 대한 설명은 [CreateUiDefinition 요소](create-uidefinition-elements.md)를 참조하세요.
