---
title: Azure IoT 중앙 응용 프로그램 내보내기 | 마이크로 소프트 문서
description: 솔루션 관리자로서 응용 프로그램 템플릿을 다시 사용할 수 있도록 내보내고 싶습니다.
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: f50c7e8dcb33fd2ed95829286aaf815926d9fb3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157632"
---
# <a name="export-your-application"></a>애플리케이션 내보내기



이 문서에서는 솔루션 관리자로서 IoT Central 응용 프로그램을 다시 사용할 수 있도록 내보내는 방법을 설명합니다.

다음 두 가지 옵션을 사용할 수 있습니다.

- 응용 프로그램의 복제본을 만들어야 하는 경우 응용 프로그램의 복사본을 만들 수 있습니다.
- 여러 복사본을 만들려는 경우 응용 프로그램에서 응용 프로그램 템플릿을 만들 수 있습니다.

## <a name="copy-your-application"></a>응용 프로그램 복사

모든 디바이스 인스턴스, 디바이스 데이터 기록 및 사용자 데이터를 제외한 모든 애플리케이션의 복사본을 만들 수 있습니다. 복사본에는 요금이 청구되는 표준 요금제가 사용됩니다. 응용 프로그램을 복사하여 무료 가격 책정 계획을 사용하는 응용 프로그램을 만들 수 없습니다.

**복사**를 선택합니다. 대화 상자에 새 응용 프로그램에 대한 세부 정보를 입력합니다. 그런 다음 **복사를** 선택하여 계속할지 확인합니다. 양식의 필드에 대해 자세히 알아보려면 응용 프로그램 빠른 시작 [만들기를](quick-deploy-iot-central.md) 참조하십시오.

![애플리케이션 설정 페이지](media/howto-use-app-templates/appcopy2.png)

앱 복사 작업이 성공하면 링크를 사용하여 새 응용 프로그램으로 이동할 수 있습니다.

![애플리케이션 설정 페이지](media/howto-use-app-templates/appcopy3a.png)

응용 프로그램을 복사할 때 규칙 및 전자 메일 작업의 정의도 복사됩니다. 흐름 및 논리 앱과 같은 일부 작업은 규칙 ID를 통해 특정 규칙에 연결됩니다. 규칙이 다른 응용 프로그램에 복사되면 자체 규칙 ID가 가져옵니다. 이 경우 사용자는 새 작업을 만든 다음 새 규칙을 연결해야 합니다. 일반적으로 새 앱에서 규칙과 작업을 확인하여 최신 상태인지 확인하는 것이 좋습니다.

> [!WARNING]
> 대시보드에 특정 장치에 대한 정보를 표시하는 타일이 포함된 경우 해당 타일에는 **요청된 리소스가** 새 응용 프로그램에서 찾을 수 없는 것으로 표시됩니다. 새 응용 프로그램의 장치에 대한 정보를 표시하려면 이러한 타일을 다시 구성해야 합니다.

## <a name="create-an-application-template"></a>애플리케이션 템플릿 만들기

Azure IoT Central 응용 프로그램을 만들 때 기본 제공 샘플 템플릿을 선택할 수 있습니다. 기존 IoT Central 응용 프로그램에서 고유한 응용 프로그램 템플릿을 만들 수도 있습니다. 그런 다음 새 응용 프로그램을 만들 때 고유한 응용 프로그램 템플릿을 사용할 수 있습니다.

응용 프로그램 템플릿을 만들 때 기존 응용 프로그램의 다음 항목이 포함됩니다.

- 대시보드 레이아웃 및 정의한 모든 타일을 포함한 기본 응용 프로그램 대시보드입니다.
- 측정, 설정, 속성, 명령 및 대시보드를 포함한 장치 템플릿입니다.
- 규칙. 모든 규칙 정의가 포함됩니다. 그러나 전자 메일 작업을 제외한 작업은 포함되지 않습니다.
- 조건 및 대시보드를 포함한 장치 집합입니다.

> [!WARNING]
> 대시보드에 특정 장치에 대한 정보를 표시하는 타일이 포함된 경우 해당 타일에는 **요청된 리소스가** 새 응용 프로그램에서 찾을 수 없는 것으로 표시됩니다. 새 응용 프로그램의 장치에 대한 정보를 표시하려면 이러한 타일을 다시 구성해야 합니다.

응용 프로그램 템플릿을 만들 때 다음 항목이 포함되지 않습니다.

- 디바이스
- 사용자
- 작업 정의
- 연속 데이터 내보내기 정의

이러한 항목을 응용 프로그램 템플릿에서 만든 모든 응용 프로그램에 수동으로 추가합니다.

기존 IoT Central 응용 프로그램에서 응용 프로그램 템플릿을 만들려면 다음을 수행합니다.

1. 응용 프로그램의 **관리** 섹션으로 이동합니다.
1. **응용 프로그램 템플릿 내보내기를 선택합니다.**
1. 응용 **프로그램 템플릿 내보내기** 페이지에서 템플릿의 이름과 설명을 입력합니다.
1. **내보내기** 단추를 선택하여 응용 프로그램 템플릿을 만듭니다. 이제 다른 사용자가 템플릿에서 새 응용 프로그램을 만들 수 있는 **공유 가능 링크를** 복사할 수 있습니다.

![애플리케이션 템플릿 만들기](media/howto-use-app-templates/create-template.png)

### <a name="use-an-application-template"></a>응용 프로그램 템플릿 사용

응용 프로그램 템플릿을 사용하여 새 IoT Central 응용 프로그램을 만들려면 이전에 만든 **공유 가능 링크가**필요합니다. **공유 가능 링크를** 브라우저의 주소 표시줄에 붙여넣습니다. **응용 프로그램 만들기** 페이지에는 사용자 지정 응용 프로그램 템플릿이 선택된 것으로 표시됩니다.

![템플릿에서 응용 프로그램 만들기](media/howto-use-app-templates/create-app.png)

가격 책정 계획을 선택하고 양식의 다른 필드를 작성합니다. 그런 다음 응용 프로그램 템플릿에서 새 IoT Central 응용 프로그램을 **만들려면 만들기를** 선택합니다.

### <a name="manage-application-templates"></a>응용 프로그램 템플릿 관리

응용 **프로그램 템플릿 내보내기** 페이지에서 응용 프로그램 템플릿을 삭제하거나 업데이트할 수 있습니다.

응용 프로그램 템플릿을 삭제하면 이전에 생성된 공유 가능한 링크를 더 이상 사용하여 새 응용 프로그램을 만들 수 없습니다.

응용 프로그램 템플릿을 업데이트하려면 응용 프로그램 **템플릿 내보내기** 페이지에서 템플릿 이름 또는 설명을 변경합니다. 그런 다음 **내보내기** 단추를 다시 선택합니다. 이 작업은 새 **공유 가능 링크를** 생성하고 이전 공유 링크 URL을 **무효화합니다.**

## <a name="next-steps"></a>다음 단계

이제 응용 프로그램 템플릿을 사용하는 방법을 배웠으니 다음 단계는 Azure [포털에서 IoT Central을 관리하는](howto-manage-iot-central-from-portal.md) 방법을 알아보는 것입니다.
