---
title: Azure DevTest Labs에서 랩 만들기 | Microsoft Docs
description: 이 문서에서는 Azure 포털 및 Azure DevTest 랩을 사용하여 랩을 만드는 프로세스를 안내합니다.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: 5cd675823b85e975dcb8dfe152c27b2d30463c1c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759740"
---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs에서 랩 만들기
Azure DevTest Labs에서 랩은 VM(Virtual Machines)과 같은 리소스 그룹을 포함하는 인프라로서, 이를 통해 한도 및 할당량을 지정하여 해당 리소스를 더 잘 관리할 수 있습니다. 이 문서는 Azure Portal을 사용하여 랩을 만드는 과정을 안내합니다.

## <a name="prerequisites"></a>사전 요구 사항
랩을 만들려면 다음이 필요합니다.

* Azure 구독 Azure 구입 옵션에 대해 알아보려면 [Azure 구입 방법](https://azure.microsoft.com/pricing/purchase-options/) 또는 [1개월 무료 평가판](https://azure.microsoft.com/pricing/free-trial/)을 참조하세요. 랩을 만들려면 구독 소유자여야 합니다.

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs에서 랩을 만드는 단계
다음 단계는 Azure Portal을 사용하여 Azure DevTest Labs에서 랩을 만드는 방법을 설명합니다. 

1. [Azure 포털에](https://go.microsoft.com/fwlink/p/?LinkID=525040)로그인합니다.
1. 왼쪽의 주 메뉴에서 **모든 서비스**(목록 맨 위)를 선택합니다. **DEVOPS** 섹션에서 **DevTest Labs** 옆에 있는 *(별표)를 선택합니다. 이 작업을 수행하면 다음에 쉽게 액세스할 수 있도록 **DevTest Labs**가 왼쪽 탐색 메뉴에 추가됩니다. 

    ![모든 서비스 - DevTest Labs 선택](./media/devtest-lab-create-lab/all-services-select.png)
2. 이제 왼쪽 탐색 메뉴에서 **DevTest Labs**를 선택합니다. 도구 모음에서 **추가**를 선택합니다. 
   
    ![랩 추가](./media/devtest-lab-create-lab/add-lab-button.png)
1. **DevTest Lab 만들기** 페이지에서 다음 작업을 수행합니다. 
    1. 랩에 대한 **이름**을 입력합니다.
    2. **구독** 을 선택하여 랩과 연결합니다.
    3. 랩의 **리소스 그룹에 대한 이름**을 입력합니다. 
    4. 랩을 저장할 **위치**를 선택합니다.
    4. **자동 종료** 를 선택하여 사용하려는지를 지정하고 랩의 VM을 모두 자동 종료하는 매개 변수를 정의할 수 있습니다. 자동 종료 기능은 주로 VM을 자동으로 종료하려는 경우를 지정할 수 있는 비용 절감 기능입니다. [Azure DevTest Labs의 랩에 대한 모든 정책 관리](./devtest-lab-set-lab-policy.md#set-auto-shutdown)문서에 설명된 단계를 수행하여 랩을 만든 후 자동 종료 설정을 변경할 수 있습니다.
    1. 랩에서 만들 모든 리소스에 추가되는 사용자 지정 태그를 만들려는 경우 **태그**에 대한 **이름** 및 **값** 정보를 입력합니다. 태그는 랩 리소스를 범주별으로 관리하고 구성하는 데 유용합니다. 랩에서 만든 후에 태그를 추가하는 방법을 포함하여 태그에 대한 자세한 내용은 [랩에 태그 추가](devtest-lab-add-tag.md)를 참조하세요.
    6. 구성 자동화를 위한 Azure Resource Manager 템플릿을 가져오려면 **Automation 옵션**을 선택합니다. 
    7. **만들기**를 선택합니다. **알림** 영역을 확인하여 랩 만들기 프로세스 상태를 모니터링할 수 있습니다. 
    
        ![DevTest Labs의 랩 만들기 섹션](./media/devtest-lab-create-lab/create-devtestlab-blade.png)
    8. 완료되면 알림에서 **리소스로 이동**을 선택합니다. 또는 **DevTest Labs** 페이지를 새로 고쳐 랩 목록에서 새로 만든 랩을 확인합니다.  목록에서 랩을 선택합니다. 랩에 대한 홈페이지가 표시됩니다. 

        ![랩에 대한 홈페이지](./media/devtest-lab-create-lab/lab-home-page.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>다음 단계
랩을 만들었으면 다음 단계를 고려 합니다.

* [랩에 대한 안전한 액세스](devtest-lab-add-devtest-user.md)
* [랩 정책 설정](devtest-lab-set-lab-policy.md)
* [랩 템플릿 만들기](devtest-lab-create-template.md)
* [VM용 사용자 지정 아티팩트 만들기](devtest-lab-artifact-author.md)
* [랩에 VM 추가](devtest-lab-add-vm.md)

