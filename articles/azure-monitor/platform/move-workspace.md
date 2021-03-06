---
title: Azure 모니터에서 로그 분석 작업 영역 이동 | 마이크로 소프트 문서
description: Log Analytics 작업 영역을 다른 구독 또는 리소스 그룹으로 이동하는 방법을 알아봅니다.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/13/2019
ms.openlocfilehash: 9213ddf034e725f6e31c9280d47bd13e4703b3f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77659495"
---
# <a name="move-a-log-analytics-workspace-to-different-subscription-or-resource-group"></a>로그 분석 작업 영역을 다른 구독 또는 리소스 그룹으로 이동

이 문서에서는 Log Analytics 작업 영역을 동일한 리전의 다른 리소스 그룹 또는 구독으로 이동하는 단계를 배웁니다. Azure 포털, PowerShell, Azure CLI 또는 REST API를 통해 Azure 리소스를 이동하는 방법에 대해 자세히 알아볼 수 있습니다. [리소스를 새 리소스 그룹 또는 구독으로 이동합니다.](../../azure-resource-manager/management/move-resource-group-and-subscription.md) 

> [!IMPORTANT]
> 작업 영역을 다른 지역으로 이동할 수 없습니다.

## <a name="verify-active-directory-tenant"></a>활성 디렉터리 테넌트 확인
작업 영역 원본 및 대상 구독은 동일한 Azure Active Directory 테넌트 내에 있어야 합니다. Azure PowerShell을 사용하여 두 구독에 동일한 테넌트 ID가 있는지 확인합니다.

``` PowerShell
(Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
(Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
```

## <a name="workspace-move-considerations"></a>작업 영역 이동 고려 사항
작업 영역에 설치된 관리되는 솔루션은 Log Analytics 작업 영역 이동 작업과 함께 이동됩니다. 연결된 에이전트는 연결된 상태로 유지되며 이동 후 작업 영역으로 데이터를 계속 보냅니다. 이동 작업에는 작업 영역에서 자동화 계정에 대한 링크가 없어야 하므로 해당 링크를 기반으로 하는 솔루션을 제거해야 합니다.

자동화 계정의 연결을 해제하려면 먼저 제거해야 하는 솔루션:

- 업데이트 관리
- 변경 내용 추적
- 작업이 없는 동안 VM 시작/중지


### <a name="delete-in-azure-portal"></a>Azure Portal에서 삭제
다음 절차를 사용하여 Azure 포털을 사용하여 솔루션을 제거합니다.

1. 솔루션이 설치된 리소스 그룹에 대한 메뉴를 엽니다.
2. 제거할 솔루션을 선택합니다.
3. **리소스 삭제를** 클릭한 다음 제거할 **리소스를**확인합니다.

![솔루션 삭제](media/move-workspace/delete-solutions.png)

### <a name="delete-using-powershell"></a>파워쉘을 사용하여 삭제

PowerShell을 사용하여 솔루션을 제거하려면 다음 예제와 같이 [제거-AzResource](/powershell/module/az.resources/remove-azresource?view=azps-2.8.0) cmdlet을 사용합니다.

``` PowerShell
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking(<workspace-name>)" -ResourceGroupName <resource-group-name>
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates(<workspace-name>)" -ResourceGroupName <resource-group-name>
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM(<workspace-name>)" -ResourceGroupName <resource-group-name>
```

### <a name="remove-alert-rules"></a>경고 규칙 제거
**VM 시작/중지** 솔루션의 경우 솔루션에서 만든 경고 규칙을 제거해야 합니다. Azure 포털에서 다음 절차를 사용하여 이러한 규칙을 제거합니다.

1. **모니터** 메뉴를 연 다음 **경고를 선택합니다.**
2. **경고 규칙 관리를 클릭합니다.**
3. 다음 세 가지 경고 규칙을 선택한 다음 **삭제를**클릭합니다.

   - AutoStop_VM_Child
   - ScheduledStartStop_Parent
   - SequencedStartStop_Parent

    ![규칙 삭제](media/move-workspace/delete-rules.png)

## <a name="unlink-automation-account"></a>연결 해제 자동화 계정
다음 절차를 사용하여 Azure 포털을 사용하여 작업 영역에서 자동화 계정의 연결을 해제합니다.

1. 자동화 **계정** 메뉴를 연 다음 제거할 계정을 선택합니다.
2. 메뉴의 **관련 리소스** 섹션에서 연결된 작업 **영역을**선택합니다. 
3. **작업 영역연결 해제를** 클릭하여 자동화 계정에서 작업 영역의 연결을 해제합니다.

    ![작업 영역 연결 해제](media/move-workspace/unlink-workspace.png)

## <a name="move-your-workspace"></a>작업 영역 이동

### <a name="azure-portal"></a>Azure portal
다음 절차를 사용하여 Azure 포털을 사용하여 작업 영역을 이동합니다.

1. **로그 분석 작업 영역** 메뉴를 연 다음 작업 영역을 선택합니다.
2. **개요** 페이지에서 **리소스 그룹** 또는 **구독**바로 옆에 **있는 변경을** 클릭합니다.
3. 작업 영역과 관련된 리소스 목록으로 새 페이지가 열립니다. 작업 영역과 동일한 대상 구독 및 리소스 그룹으로 이동할 리소스를 선택합니다. 
4. 대상 **구독** 및 **리소스 그룹을**선택합니다. 작업 영역을 동일한 구독의 다른 리소스 그룹으로 이동하는 경우 **구독** 옵션이 표시되지 않습니다.
5. **확인을** 클릭하여 작업 영역 및 선택한 리소스를 이동합니다.

    ![포털](media/move-workspace/portal.png)

### <a name="powershell"></a>PowerShell
PowerShell을 사용하여 작업 영역을 이동하려면 다음 예제와 같이 [Move-AzResource를](/powershell/module/AzureRM.Resources/Move-AzureRmResource) 사용합니다.

``` PowerShell
Move-AzResource -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyResourceGroup01/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace" -DestinationSubscriptionId "00000000-0000-0000-0000-000000000000" -DestinationResourceGroupName "MyResourceGroup02"
```



> [!IMPORTANT]
> 이동 작업 후 제거된 솔루션 및 자동화 계정 링크를 다시 구성하여 작업 영역을 이전 상태로 되돌려야 합니다.


## <a name="next-steps"></a>다음 단계
- 이동을 지원하는 리소스 목록은 [리소스에 대한 작업 지원 이동을](../../azure-resource-manager/management/move-support-resources.md)참조하십시오.
