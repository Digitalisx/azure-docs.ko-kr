---
title: 그룹에 민감도 레이블 할당 - Azure AD | 마이크로 소프트 문서
description: 그룹을 자동으로 채우는 멤버 자격 규칙과 규칙 참조를 만드는 방법입니다.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 02/24/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 667fb39aabfec14cff01221b82a45ba8ad1d68d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78329735"
---
# <a name="assign-sensitivity-labels-to-office-365-groups-in-azure-active-directory-preview"></a>Azure Active Directory에서 Office 365 그룹에 민감도 레이블 할당(미리 보기)

Azure Active Directory(Azure AD)는 [Microsoft 365 준수 센터에서](https://sip.protection.office.com/homepage) 게시한 민감도 레이블을 Office 365 그룹에 적용하는 것을 지원합니다. 민감도 레이블은 Outlook, Microsoft 팀 및 SharePoint와 같은 서비스 전체에서 그룹에 적용됩니다. 이 기능은 현재 공개 미리 보기로 제공됩니다. Office 365 앱 지원에 대한 자세한 내용은 [민감도 레이블에 대한 Office 365 지원을](https://docs.microsoft.com/microsoft-365/compliance/sensitivity-labels-teams-groups-sites#support-for-the-sensitivity-labels)참조하십시오.

> [!IMPORTANT]
> 이 기능을 구성하려면 Azure AD 조직에 하나 이상의 활성 Azure Active Directory Premium P1 라이선스가 있어야 합니다.

## <a name="enable-sensitivity-label-support-in-powershell"></a>PowerShell에서 감도 레이블 지원 활성화

게시된 레이블을 그룹에 적용하려면 먼저 피쳐를 사용하도록 설정해야 합니다. 이러한 단계는 Azure AD에서 기능을 사용하도록 설정합니다.

1. 컴퓨터에서 Windows PowerShell 창을 엽니다. 높은 권한이 없으면 이 창이 열리지 않습니다.
1. 다음 명령을 실행하여 cmdlet 실행을 준비합니다.

    ```PowerShell
    Import-Module AzureADPreview
    Connect-AzureAD
    ```

    계정에 **로그인** 페이지에서 관리자 계정과 암호를 입력하여 서비스에 연결하고 **로그인을**선택합니다.
1. Azure AD 조직의 현재 그룹 설정을 가져옵니다.

    ```PowerShell
    $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
    ```

    > [!NOTE]
    > 이 Azure AD 조직에 대해 그룹 설정이 만들어지지 않은 경우 먼저 설정을 만들어야 합니다. 그룹 설정을 구성하여 이 Azure AD 조직에 대한 그룹 설정을 만들기 [위한 Azure Active Directory cmdlet의](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-settings-cmdlets) 단계를 따릅니다.

1. 다음으로 현재 그룹 설정을 표시합니다.

    ```PowerShell
    $Setting.Values
    ```

1. 그런 다음 다음 기능을 사용하도록 설정합니다.

    ```PowerShell
    $Setting["EnableMIPLabels"] = "True"
    ```

1. 그런 다음 변경 내용을 저장하고 설정을 적용합니다.

    ```PowerShell
    Set-AzureADDirectorySetting -Id $Setting.Id -DirectorySetting $Setting
    ```

이것으로 끝입니다. 이 기능을 사용하도록 설정하고 게시된 레이블을 그룹에 적용할 수 있습니다.

## <a name="assign-a-label-to-a-new-group-in-azure-portal"></a>Azure 포털의 새 그룹에 레이블 할당

1. [Azure AD 관리자 센터에](https://aad.portal.azure.com)로그인합니다.
1. **그룹을**선택한 다음 **새 그룹을**선택합니다.
1. 새 **그룹** 페이지에서 **Office 365를**선택한 다음 새 그룹에 필요한 정보를 입력하고 목록에서 민감도 레이블을 선택합니다.

   ![새 그룹 페이지에 민감도 레이블 할당](./media/groups-assign-sensitivity-labels/new-group-page.png)

1. 변경 내용을 저장하고 **을 선택합니다.**

그룹이 만들어지고 선택한 레이블과 연결된 사이트 및 그룹 설정이 자동으로 적용됩니다.

## <a name="assign-a-label-to-an-existing-group-in-azure-portal"></a>Azure 포털의 기존 그룹에 레이블 할당

1. 그룹 관리자 계정또는 그룹 소유자로 [Azure AD 관리자 센터에](https://aad.portal.azure.com) 로그인합니다.
1. **그룹**선택 .
1. 모든 **그룹** 페이지에서 레이블을 지정할 그룹을 선택합니다.
1. 선택한 그룹의 페이지에서 **속성을** 선택하고 목록에서 민감도 레이블을 선택합니다.

   ![그룹에 대한 개요 페이지에 민감도 레이블 할당](./media/groups-assign-sensitivity-labels/assign-to-existing.png)

1. **저장**을 선택하여 변경 내용을 저장합니다.

## <a name="remove-a-label-from-an-existing-group-in-azure-portal"></a>Azure 포털의 기존 그룹에서 레이블 제거

1. 글로벌 관리자 또는 그룹 관리자 계정으로 [Azure AD 관리자 센터에](https://aad.portal.azure.com) 로그인하거나 그룹 소유자로 로그인합니다.
1. **그룹**선택 .
1. 모든 **그룹** 페이지에서 레이블을 제거할 그룹을 선택합니다.
1. **그룹** 페이지에서 **속성**을 선택합니다.
1. **제거**를 선택합니다.
1. **저장**을 선택하여 변경 내용을 적용합니다.

## <a name="using-classic-azure-ad-classifications"></a>클래식 Azure AD 분류 사용

이 기능을 활성화하면 그룹에 대한 "클래식" 분류가 기존 그룹 및 사이트만 표시되며 민감도 레이블을 지원하지 않는 앱에서 그룹을 만드는 경우에만 새 그룹에 사용해야 합니다. 관리자는 나중에 필요한 경우 민감도 레이블로 변환할 수 있습니다. 클래식 분류는 Azure AD PowerShell에서 설정에 `ClassificationList` 대한 값을 정의하여 설정한 이전 분류입니다. 이 기능을 사용하도록 설정하면 해당 분류는 그룹에 적용되지 않습니다.

## <a name="troubleshooting-issues"></a>문제 해결

### <a name="sensitivity-labels-are-not-available-for-assignment-on-a-group"></a>민감도 레이블은 그룹에서 할당할 수 없습니다.

민감도 레이블 옵션은 다음 모든 조건이 충족될 때만 그룹에 대해 표시됩니다.

1. 레이블은 이 테넌트에 대한 Microsoft 365 규정 준수 센터에 게시됩니다.
1. 이 기능이 활성화되고, 인에이블MIPLabels는 파워쉘에서 True로 설정됩니다.
1. 이 그룹은 Office 365 그룹입니다.
1. 테넌트에는 활성 Azure Active Directory Premium P1 라이선스가 있습니다.
1. 현재 로그인한 사용자에게레이블을 할당할 수 있는 충분한 권한이 있습니다. 사용자는 글로벌 관리자, 그룹 관리자 또는 그룹 소유자여야 합니다.

그룹에 레이블을 할당하려면 모든 조건이 충족되는지 확인하십시오.

### <a name="the-label-i-want-to-assign-is-not-in-the-list"></a>할당하려는 레이블이 목록에 없습니다.

찾고 있는 레이블이 목록에 없는 경우 다음과 같은 이유 중 하나에 대 한 경우가 될 수 있습니다.

- 레이블이 Microsoft 365 규정 준수 센터에 게시되지 않을 수 있습니다. 이는 더 이상 게시되지 않은 레이블에도 적용될 수 있습니다. 자세한 내용은 관리자에게 문의하십시오.
- 레이블은 게시될 수 있지만 로그인한 사용자는 레이블을 사용할 수 없습니다. 라벨에 액세스하는 방법에 대한 자세한 내용은 관리자에게 문의하십시오.

### <a name="how-to-change-the-label-on-a-group"></a>그룹의 레이블을 변경하는 방법

레이블은 다음과 같이 기존 그룹에 레이블을 할당하는 것과 동일한 단계를 사용하여 언제든지 레이블을 교환할 수 있습니다.

1. 글로벌 또는 그룹 관리자 계정 또는 그룹 소유자로 [Azure AD 관리자 센터에](https://aad.portal.azure.com) 로그인합니다.
1. **그룹**선택 .
1. 모든 **그룹** 페이지에서 레이블을 지정할 그룹을 선택합니다.
1. 선택한 그룹의 페이지에서 **속성을** 선택하고 목록에서 새 민감도 레이블을 선택합니다.
1. **저장**을 선택합니다.

### <a name="group-setting-changes-to-published-labels-are-not-updated-on-the-groups"></a>게시된 레이블에 대한 그룹 설정 변경 내용이 그룹에서 업데이트되지 않습니다.

가장 좋은 방법은 레이블이 그룹에 적용된 후에레이블에 대한 그룹 설정을 변경하지 않는 것이 좋습니다. [Microsoft 365 준수 센터에서](https://sip.protection.office.com/homepage)게시된 레이블과 연결된 그룹 설정을 변경하면 해당 정책 변경 내용이 영향을 받는 그룹에 자동으로 적용되지 않습니다.

변경해야 하는 경우 Azure [AD PowerShell 스크립트를](https://github.com/microsoftgraph/powershell-aad-samples/blob/master/ReassignSensitivityLabelToO365Groups.ps1) 사용하여 영향을 받는 그룹에 업데이트를 수동으로 적용합니다. 이 방법을 사용하면 모든 기존 그룹이 새 설정을 적용합니다.

## <a name="next-steps"></a>다음 단계

- [Microsoft 팀, Office 365 그룹 및 SharePoint 사이트에서 민감도 레이블 사용](https://docs.microsoft.com/microsoft-365/compliance/sensitivity-labels-teams-groups-sites)
- [Azure AD PowerShell 스크립트를 사용 하 여 레이블 정책 변경 후 그룹 업데이트](https://github.com/microsoftgraph/powershell-aad-samples/blob/master/ReassignSensitivityLabelToO365Groups.ps1)
- [그룹 설정 편집](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-settings-azure-portal)
- [PowerShell 명령을 사용하여 그룹 관리](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-settings-v2-cmdlets)
