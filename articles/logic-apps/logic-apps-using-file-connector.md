---
title: 온-프레미스 파일 시스템에 연결
description: Azure Logic Apps에서 온-프레미스 데이터 게이트웨이를 통해 파일 시스템 커넥터를 사용하여 온-프레미스 파일 시스템에 연결하는 작업 및 워크플로 자동화
services: logic-apps
ms.suite: integration
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan, logicappspm
ms.topic: article
ms.date: 01/13/2019
ms.openlocfilehash: 2a00405a2100c3e565ca4f8ea4149540a5199b43
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651409"
---
# <a name="connect-to-on-premises-file-systems-with-azure-logic-apps"></a>Azure Logic Apps를 사용하여 온-프레미스 파일 시스템에 연결

Azure Logic Apps 및 파일 시스템 커넥터를 사용하면 온-프레미스 파일 공유에서 파일을 만들고 관리하는 자동화된 작업 및 워크플로를 만들 수 있습니다.

- 파일 만들기, 가져오기, 추가, 업데이트 및 삭제를 수행합니다.
- 폴더 또는 루트 폴더의 파일을 나열합니다.
- 파일 콘텐츠 및 메타데이터를 가져옵니다.

이 문서에서는 [Dropbox에 업로드된 파일을 파일 공유에 복사한 후 이메일 보내기] 예제 시나리오에서 설명한 것처럼, 온-프레미스 파일 시스템에 연결하는 방법을 보여줍니다. 온-프레미스 시스템에 안전하게 연결하고 액세스할 수 있도록 논리 앱은 [온-프레미스 데이터 게이트웨이](../logic-apps/logic-apps-gateway-connection.md)를 사용합니다. 논리 앱을 새로 접하는 경우 [Azure 논리 앱이란 무엇입니까?](../logic-apps/logic-apps-overview.md) 커넥터 관련 기술 정보는 파일 [시스템 커넥터 참조](/connectors/filesystem/)를 참조하십시오.

## <a name="prerequisites"></a>사전 요구 사항

* Azure 구독 Azure 구독이 없는 경우 [체험 Azure 계정에 등록](https://azure.microsoft.com/free/)합니다.

* 논리 앱을 파일 시스템 서버 같은 온-프레미스 시스템에 연결하려면 먼저 [온-프레미스 데이터 게이트웨이를 설치 및 설정](../logic-apps/logic-apps-gateway-install.md)해야 합니다. 이런 방식으로 논리 앱에서 파일 시스템 연결을 만들 때 게이트웨이 설치를 사용하도록 지정할 수 있습니다.

* 무료로 가입할 수 있는 [Dropbox 계정.](https://www.dropbox.com/) 로직 앱과 Dropbox 계정 간의 연결을 만드는 데 계정 자격 증명이 필요합니다.

* 사용하려는 파일 시스템이 있는 컴퓨터에 액세스합니다. 예를 들어 파일 시스템과 동일한 컴퓨터에 데이터 게이트웨이를 설치하는 경우 해당 컴퓨터에 대한 계정 자격 증명이 필요합니다.

* Office 365 Outlook, Outlook.com, Gmail 등 Logic Apps에서 지원되는 공급자의 이메일 계정. 다른 공급자에 대한 내용은 [여기서 커넥터 목록을 검토하세요](https://docs.microsoft.com/connectors/). 이 논리 앱은 Office 365 Outlook 계정을 사용합니다. 다른 이메일 계정을 사용하는 경우 전체 단계는 동일하지만 UI가 약간 다를 수 있습니다.

* [논리 앱 만드는 방법](../logic-apps/quickstart-create-first-logic-app-workflow.md)에 관한 기본 지식 이 예에서는 빈 논리 앱이 필요합니다.

## <a name="add-trigger"></a>트리거 추가

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure Portal](https://portal.azure.com)에 로그인하고, 아직 열리지 않은 경우 Logic App Designer에서 논리 앱을 엽니다.

1. 검색 상자에 필터로 "dropbox"를 입력합니다. 트리거 목록에서 **파일을 만들 때** 트리거를 선택합니다.

   ![Dropbox 트리거 선택](media/logic-apps-using-file-connector/select-dropbox-trigger.png)

1. Dropbox 계정 자격 증명을 사용하여 로그인하고, Dropbox 데이터에 대한 액세스 권한을 Azure Logic Apps에 부여합니다.

1. 트리거에 필요한 정보를 입력합니다.

   ![Dropbox 트리거](media/logic-apps-using-file-connector/dropbox-trigger.png)

## <a name="add-actions"></a>작업 추가

1. 트리거 아래에서 **다음 단계**를 선택합니다. 검색 상자에 "파일 시스템"을 필터로 입력합니다. 작업 목록에서 이 작업을 **선택합니다.**

   ![파일 시스템 커넥터 찾기](media/logic-apps-using-file-connector/find-file-system-action.png)

1. 파일 시스템에 아직 연결하지 않은 경우 연결을 생성하라는 메시지가 표시됩니다.

   ![연결 만들기](media/logic-apps-using-file-connector/file-system-connection.png)

   | 속성 | 필수 | 값 | 설명 |
   | -------- | -------- | ----- | ----------- |
   | **연결 이름** | yes | <*연결 이름*> | 연결에 사용하려는 이름 |
   | **루트 폴더** | yes | <*루트 폴더 이름*> | 온-프레미스 데이터 게이트웨이가 설치된 컴퓨터의 로컬 폴더나 컴퓨터가 액세스할 수 있는 네트워크 공유용 폴더 등의 위치에 온-프레미스 데이터 게이트웨이를 설치한 경우 파일 시스템용 루트 폴더입니다. <p>예: `\\PublicShare\\DropboxFiles` <p>루트 폴더는 모든 파일 관련 작업의 상대 경로에 사용되는 기본 상위 폴더입니다. |
   | **인증 유형** | 예 | <*인증 유형*> | 파일 시스템에 사용되는 인증 유형(예: **Windows**) |
   | **사용자** | yes | <*domain*>도메인\\사용자*이름* <> | 파일 시스템이 있는 컴퓨터의 사용자 이름 |
   | **암호** | yes | <*암호*> | 파일 시스템이 있는 컴퓨터의 암호 |
   | **게이트웨이** | yes | <*설치된 게이트웨이 이름*> | 이전에 설치된 게이트웨이의 이름 |
   |||||

1. 작업을 완료하면 **만들기**를 선택합니다.

   Logic Apps는 연결을 구성하고 테스트하여 제대로 작동되는지 확인합니다. 연결이 제대로 설정되면 이전에 선택한 작업에 대한 옵션이 표시됩니다.

1. **파일 만들기** 작업에서, Dropbox에서 온-프레미스 파일 공유의 루트 폴더로 파일을 복사하기 위한 세부 정보를 입력합니다. 이전 단계의 출력을 추가하려면 상자 내부를 클릭하고, 동적 콘텐츠 목록이 나타나면 사용 가능한 필드에서 선택합니다.

   ![파일 작업 만들기](media/logic-apps-using-file-connector/create-file-filled.png)

1. 이제 적절한 사용자가 새 파일에 대해 알 수 있도록 이메일을 보내는 Outlook 작업을 추가합니다. 전자 메일의 받는 사람, 제목 및 본문을 입력합니다. 자신의 이메일 주소를 사용하여 테스트할 수 있습니다.

   ![전자 메일 보내기 작업](media/logic-apps-using-file-connector/send-email.png)

1. 논리 앱을 저장합니다. Dropbox에 파일을 업로드하여 앱을 테스트합니다.

   논리 앱은 온-프레미스 파일 공유로 파일을 복사하고, 받는 사람에게 복사된 파일에 대한 이메일을 보냅니다.

## <a name="connector-reference"></a>커넥터 참조

커넥터의 Swagger 파일에 설명된 트리거, 작업 및 제한과 같은 이 커넥터에 대한 자세한 내용은 [커넥터의 참조 페이지를](https://docs.microsoft.com/connectors/fileconnector/)참조하십시오.

> [!NOTE]
> [통합 서비스 환경(ISE)의](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)논리 앱의 경우 이 커넥터의 ISE 레이블이 지정된 버전은 [ISE 메시지 제한을](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) 대신 사용합니다.

## <a name="next-steps"></a>다음 단계

* [온-프레미스 데이터에 연결](../logic-apps/logic-apps-gateway-connection.md)하는 방법을 알아봅니다. 
* 다른 [Logic Apps 커넥터](../connectors/apis-list.md)에 대해 알아봅니다.
