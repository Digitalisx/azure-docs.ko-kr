---
title: Azure 파트너 및 고객 사용 기여 | Azure 마켓플레이스
description: Azure Marketplace 솔루션에 대한 고객 사용량을 추적하는 방법의 개요
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 9/23/2019
ms.author: dsindona
ms.openlocfilehash: 2895944dea6417949488076186135680523e19db
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80874962"
---
# <a name="azure-partner-customer-usage-attribution"></a>Azure 파트너 고객 사용량 특성

Azure 소프트웨어 파트너의 솔루션은 Azure 구성 요소가 필요하거나 Azure 인프라에 직접 배포해야 합니다. 파트너 솔루션을 배포하고 자체 Azure 리소스를 프로비전하는 고객은 배포 상태를 파악하기 어렵고, 이에 따라 Azure 성장에 미치는 영향에 광학을 사용할 수 있습니다. 높은 수준의 가시성을 추가하면 Microsoft 판매 팀과 제휴하여 Microsoft 파트너 프로그램에 대한 크레딧을 얻게 됩니다.

Microsoft는 이제 파트너가 Azure에 자사 소프트웨어를 배포한 고객의 Azure 사용량을 더 효율적으로 추적할 수 있는 유용한 방법을 제공합니다. 이 새 방법에서는 Azure Resource Manager를 사용하여 Azure 서비스의 배포를 오케스트레이션합니다.

Microsoft 파트너는 고객을 대신하여 프로비전하는 Azure 리소스와 Azure 사용량을 연결할 수 있습니다. Azure Marketplace, 빠른 시작 리포지토리, 프라이빗 GitHub 리포지토리 및 일 대 일 고객 참여를 통해 연결을 구성할 수 있습니다. 고객 사용 기여는 다음 세 가지 배포 옵션을 지원합니다.

- Azure 리소스 관리자 템플릿: 파트너는 리소스 관리자 템플릿을 사용하여 Azure 서비스를 배포하여 파트너의 소프트웨어를 실행할 수 있습니다. 파트너는 Azure 솔루션의 인프라와 구성을 정의하는 Resource Manager 템플릿을 만들 수 있습니다. Resource Manager 템플릿을 사용하면 개발자와 고객이 수명 주기 전반에 걸쳐 솔루션을 배포할 수 있습니다. 리소스가 일관된 상태로 배포된다는 것을 확신할 수 있습니다.
- Azure Resource Manager API: 파트너는 Resource Manager API를 직접 호출하여 Resource Manager 템플릿을 배포하거나 Azure 서비스를 직접 프로비전할 API 호출을 생성할 수 있습니다.
- Terraform: 파트너는 Terraform과 같은 클라우드 오케스트레이터를 사용하여 리소스 관리자 템플릿을 배포하거나 Azure 서비스를 직접 배포할 수 있습니다.

고객 사용 기여는 새 배포를 위한 것이며 이미 배포된 기존 리소스에 태그를 지정하는 것을 지원하지 않습니다.

[Azure Application](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/azure-applications/cpp-azure-app-offer): Azure Marketplace에 게시된 솔루션 템플릿 제품에 대한 고객 사용 기여가 필요합니다.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="use-resource-manager-templates"></a>Resource Manager 템플릿 사용
많은 파트너 솔루션은 리소스 관리자 템플릿을 사용하여 고객의 구독에 배포됩니다. Azure 마켓플레이스, GitHub 또는 빠른 시작으로 사용할 수 있는 리소스 관리자 템플릿이 있는 경우 고객 사용 기여가 활성화되도록 템플릿을 수정하는 프로세스는 간단해야 합니다.

솔루션 템플릿을 만들고 게시하는 방법에 대한 자세한 내용은 다음을 참조하세요.

* [첫 번째 Resource Manager 템플릿 만들기 및 배포](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)
* [Azure 응용 프로그램 제공](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/azure-applications/cpp-azure-app-offer).
* 비디오: [Azure 마켓플레이스에 대한 솔루션 템플릿 및 관리되는 응용 프로그램 빌드.](https://channel9.msdn.com/Events/Build/2018/BRK3603)


## <a name="add-a-guid-to-your-template"></a>템플릿에 GUID 추가

GUID(Globally Unique Identifier)를 추가하려면 주 템플릿 파일을 한 번만 수정하면 됩니다.

1. 제안된 방법을 사용하여 [GUID를 만들고](#create-guids)[GUID를 등록](#register-guids-and-offers)합니다.

1. Resource Manager 템플릿을 엽니다.

1. 기본 템플릿 파일에 새 리소스를 추가합니다. 리소스는 **mainTemplate.json** 또는 **azuredeploy.json** 파일에만 있어야 하며, 중첩되거나 연결된 템플릿에 있으면 안됩니다.

1. **pid-** 접두사 뒤에 GUID 값을 입력합니다(예: pid-eb7927c8-dd66-43e1-b0cf-c346a422063).

1. 템플릿에서 오류를 확인합니다.

1. 적절한 리포지토리에 템플릿을 다시 게시합니다.

1. [템플릿 배포에서 GUID 성공을 확인](#verify-the-guid-deployment)합니다.

### <a name="sample-resource-manager-template-code"></a>샘플 Resource Manager 템플릿 코드

템플릿에 대한 추적 리소스를 사용하도록 설정하려면 리소스 섹션 아래에 다음 추가 리소스를 추가해야 합니다. 아래 샘플 코드를 기본 템플릿 파일에 추가할 때 사용자 고유의 입력으로 수정해야 합니다.
리소스는 **mainTemplate.json** 또는 **azuredeploy.json** 파일에만 추가해야 하며, 중첩되거나 연결된 템플릿에 있으면 안 됩니다.

```
// Make sure to modify this sample code with your own inputs where applicable

{ // add this resource to the resources section in the mainTemplate.json (do not add the entire file)
    "apiVersion": "2018-02-01",
    "name": "pid-XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX", // use your generated GUID here
    "type": "Microsoft.Resources/deployments",
    "properties": {
        "mode": "Incremental",
        "template": {
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "resources": []
        }
    }
} // remove all comments from the file when complete
```

## <a name="use-the-resource-manager-apis"></a>Resource Manager API 사용

파트너가 Resource Manager REST API를 직접 호출하여 Azure 서비스를 배포하는 방법을 선호하는 경우도 있습니다. Azure는 이러한 호출을 사용하도록 설정하는 [여러 SDK를 지원](https://docs.microsoft.com/azure/?pivot=sdkstools)합니다. SDK 중 하나를 사용하거나, REST API를 직접 호출하여 리소스를 배포할 수 있습니다.

Resource Manager 템플릿을 사용하는 경우 앞에서 설명한 지침에 따라 솔루션에 태그를 지정해야 합니다. Resource Manager 템플릿을 사용하지 않고 API를 직접 호출하는 경우에도 Azure 리소스 사용량에 연결하도록 배포에 태그를 지정할 수 있습니다.

### <a name="tag-a-deployment-with-the-resource-manager-apis"></a>Resource Manager API를 사용하여 배포에 태그 지정

고객 사용 기여기능을 활성화하려면 API 호출을 디자인할 때 요청에 사용자 에이전트 헤더에 GUID를 포함합니다. 각 제품 또는 SKU에 대한 GUID를 추가합니다. **pid-** 접두사를 사용하여 문자열의 형식을 지정하고 파트너가 생성한 GUID를 포함시킵니다. 사용자 에이전트에 삽입하기 위한 GUID 형식의 예제는 다음과 같습니다.

![GUID 형식 예제](media/marketplace-publishers-guide/tracking-sample-guid-for-lu-2.PNG)

> [!Note]
> 문자열의 형식이 중요합니다. **pid-** 접두사가 포함되지 않으면 데이터를 쿼리할 수 없습니다. SDK마다 다른 방법으로 추적합니다. 이 방법을 구현하려면 기본 설정 Azure SDK에 대한 지원 및 추적 방법을 검토하세요.

#### <a name="example-the-python-sdk"></a>예제: Python SDK

Python의 경우 **config** 특성을 사용합니다. 이 특성은 UserAgent에만 추가할 수 있습니다. 예를 들면 다음과 같습니다.

![사용자 에이전트에 특성 추가](media/marketplace-publishers-guide/python-for-lu.PNG)

> [!Note]
> 각 클라이언트에 대한 특성을 추가합니다. 글로벌 정적 구성이 없습니다. 모든 클라이언트에서 추적할 수 있도록 클라이언트 팩터리에 태그를 지정할 수 있습니다. 자세한 내용은 [GitHub의 클라이언트 팩터리 샘플](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79)을 참조하세요.

#### <a name="tag-a-deployment-by-using-the-azure-powershell"></a>Azure PowerShell을 사용하여 배포에 태그 지정

Azure PowerShell을 통해 리소스를 배포하는 경우 다음 방법을 사용하여 GUID를 추가합니다.

```powershell
[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")
```

#### <a name="tag-a-deployment-by-using-the-azure-cli"></a>Azure CLI를 사용하여 배포에 태그 지정

Azure CLI를 사용하여 GUID를 추가하는 경우 **AZURE_HTTP_USER_AGENT** 환경 변수를 설정합니다. 이 변수는 스크립트의 범위 내에서 설정할 수 있습니다. 셸 범위에 대한 글로벌 변수를 설정할 수도 있습니다.

```
export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'
```
자세한 내용은 [이동에 대 한 Azure SDK를](https://docs.microsoft.com/azure/go/)참조 하십시오.

## <a name="use-terraform"></a>테라폼 사용

테라폼에 대한 지원은 Azure 공급자의 1.21.0 [https://github.com/terraform-providers/terraform-provider-azurerm/blob/master/CHANGELOG.md#1210-january-11-2019](https://github.com/terraform-providers/terraform-provider-azurerm/blob/master/CHANGELOG.md#1210-january-11-2019)릴리스를 통해 사용할 수 있습니다.  이 지원은 Terraform을 통해 솔루션을 배포하는 모든 파트너와 Azure 공급자가 배포하고 계량하는 모든 리소스(버전 1.21.0 이상)에 적용됩니다.

Terraform용 Azure 공급자는 솔루션에 사용하는 추적 GUID를 지정하는 [*partner_id라는*](https://www.terraform.io/docs/providers/azurerm/#partner_id) 새 선택적 필드를 추가했습니다. 이 필드의 값은 *ARM_PARTNER_ID* 환경 변수에서 원본화할 수도 있습니다.

```
provider "azurerm" {
          subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          client_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          ……
          # new stuff for ISV attribution
          partner_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"}
```
고객 사용 기여로 추적된 Terraform을 통해 배포를 받으려는 파트너는 다음을 수행해야 합니다.

* GUID 만들기(각 오퍼 또는 SKU에 대해 GUID를 추가해야 합니다).
* azure 공급자를 업데이트하여 *partner_id* 값을 GUID로 설정합니다("pid-"로 GUID를 미리 수정하지 말고 실제 GUID로 설정).

## <a name="create-guids"></a>GUID 만들기

GUID는 32자리의 16진수가 있는 고유 참조 번호입니다. 추적을 위한 GUID를 만들려면 GUID 생성기를 사용해야 합니다. Azure Storage 팀은 [GUID 생성기 양식](https://aka.ms/StoragePartners)을 만든 후 이 양식을 통해 올바른 형식의 GUID를 이메일로 전달합니다. 이 GUID를 다른 추적 시스템에서 다시 사용할 수 있습니다.

> [!Note]
> [Azure Storage의 GUID 생성기 양식을](https://aka.ms/StoragePartners) 사용하여 GUID를 만드는 것이 좋습니다. 자세한 내용은 [FAQ](#faq)를 참조하세요.

각 제품에 대한 모든 제안 및 배포 채널에 대해 고유한 GUID를 만드는 것이 좋습니다. 보고를 분할하지 않으려면 제품의 여러 배포 채널에 대해 단일 GUID를 사용하도록 선택할 수 있습니다.

템플릿을 사용하여 제품을 배포하고 Azure Marketplace 및 GitHub에서 모두 사용할 수 있는 경우, 2개의 고유 GUID를 만들고 등록할 수 있습니다.

*   Azure Marketplace의 제품 A
*   GitHub의 제품 A

보고는 파트너 값(Microsoft 파트너 ID) 및 GUID를 통해 수행됩니다.

SKU는 제품의 변형인 SKU와 같이 더 세부적인 수준에서 GUID를 추적할 수도 있습니다.

## <a name="register-guids-and-offers"></a>GUID 및 제품 등록

고객 사용 기여가 가능하도록 GUID를 등록해야 합니다.

템플릿 GUID에 대한 모든 등록은 파트너 센터 내에서 수행됩니다.

템플릿 또는 사용자 에이전트에 GUID를 추가하고 파트너 센터에 GUID를 등록하면 모든 배포가 추적됩니다.

1. [상업용 마켓플레이스 퍼블리셔로](https://aka.ms/JoinMarketplace)등록하십시오.

   * 파트너는 [파트너 센터에 프로필이 있어야](https://docs.microsoft.com/azure/marketplace/become-publisher)합니다. Azure Marketplace 또는 AppSource에 제품을 나열하는 것이 좋습니다.
   * 파트너는 여러 GUID를 등록할 수 있습니다.
   * 파트너는 Marketplace 이외의 솔루션 템플릿 및 제품에 대한 GUID를 등록할 수 있습니다.

1. [파트너 센터에 로그인합니다.](https://partner.microsoft.com/dashboard)

1. 오른쪽 위쪽 모서리에서 설정 기어 아이콘을 선택한 다음 **개발자 설정을**선택합니다.

1. 계정 **설정 페이지에서** **추적 GUID 추가를 선택합니다.**

1. **GUID** 상자에 추적 GUID를 입력합니다. **pid-** 접두사 없이 GUID만 입력합니다. **설명** 상자에 쿠폰 이름 또는 설명을 입력합니다.

1. 여러 GUID를 등록하려면 **추적 GUID 추가**를 다시 선택합니다. 추가 상자가 페이지에 표시됩니다.

1. **저장**을 선택합니다.


## <a name="verify-the-guid-deployment"></a>GUID 배포 확인

템플릿이 수정되고 테스트 배포가 실행되면 다음 PowerShell 스크립트를 사용하여 배포하고 태그를 지정한 리소스를 검색합니다.

이 스크립트를 사용하여 GUID가 Resource Manager 템플릿에 성공적으로 추가되었는지 확인할 수 있습니다. 스크립트는 리소스 관리자 API 또는 Terraform 배포에 적용되지 않습니다.

Azure에 로그인합니다. 스크립트를 실행하기 전에 확인하려는 배포가 있는 구독을 선택합니다. 배포의 구독 컨텍스트 내에서 스크립트를 실행합니다.

배포의 **GUID** 및 **resourceGroup** 이름은 필수 매개 변수입니다.

GitHub에서 [원래의 스크립트](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1)를 가져올 수 있습니다.

```powershell
Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName
)

# Get the correlationId of the pid deployment

$correlationId = (Get-AzResourceGroupDeployment -ResourceGroupName
$resourceGroupName -Name "pid-$guid").correlationId

# Find all deployments with that correlationId

$deployments = Get-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

# Find all deploymentOperations in a deployment by name
# PowerShell doesn't surface outputResources on the deployment
# or correlationId on the deploymentOperation

foreach ($deployment in $deployments){

# Get deploymentOperations by deploymentName
# then the resourceId for any create operation

($deployment | Get-AzResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}
```

## <a name="report"></a>보고서

파트너 센터 대시보드()에서[https://partner.microsoft.com/dashboard/mpn/analytics/CPP/MicrosoftAzure](https://partner.microsoft.com/dashboard/mpn/analytics/CPP/MicrosoftAzure)고객 사용 기여에 대한 보고서를 찾을 수 있습니다. 보고서를 보려면 파트너 센터 자격 증명을 사용하여 로그인해야 합니다. 보고서 또는 로그인에 문제가 있는 경우 지원 받기 섹션의 지침에 따라 지원 요청을 만듭니다.

파트너 연결 유형의 드롭다운 목록에서 추적된 템플릿을 선택하여 보고서를 확인합니다.

![고객 사용 기여에 대한 보고서](media/marketplace-publishers-guide/customer-usage-attribution-report.png)

## <a name="notify-your-customers"></a>고객에게 알림

파트너는 고객 사용 기여를 사용하는 배포에 대해 고객에게 알려야 합니다. Microsoft는 이러한 배포와 연결된 Azure 사용량을 파트너에게 보고합니다. 다음 예제에는 이러한 배포에 대해 고객에게 알리는 데 사용할 수 있는 콘텐츠가 포함되어 있습니다. 이 예제에서 \<PARTNER>는 회사 이름으로 바꿉니다. 파트너는 추적에서 고객을 제외할 수 있는 옵션을 포함하여 알림이 해당 데이터 개인 정보 및 수집 정책과 일치하는지 확인해야 합니다.

### <a name="notification-for-resource-manager-template-deployments"></a>Resource Manager 템플릿 배포에 대한 알림

이 템플릿을 배포하면 Microsoft에서 배포된 Azure 리소스를 사용하여 \<PARTNER> 소프트웨어의 설치를 식별할 수 있습니다. Microsoft는 소프트웨어를 지원하는 데 사용되는 Azure 리소스를 상호 연결할 수 있습니다. Microsoft는 제품에 최상의 환경을 제공하고 비즈니스를 운영하기 위해 이 정보를 수집합니다. 데이터는 Microsoft 개인 정보 보호 정책에 따라 수집되고 관리되며, https://www.microsoft.com/trustcenter에 있습니다.

### <a name="notification-for-sdk-or-api-deployments"></a>SDK 또는 API 배포 알림

\<PARTNER> 소프트웨어를 배포하면 Microsoft에서 배포된 Azure 리소스를 사용하여 \<PARTNER> 소프트웨어의 설치를 식별할 수 있습니다. Microsoft는 소프트웨어를 지원하는 데 사용되는 Azure 리소스를 상호 연결할 수 있습니다. Microsoft는 제품에 최상의 환경을 제공하고 비즈니스를 운영하기 위해 이 정보를 수집합니다. 데이터는 Microsoft 개인 정보 보호 정책에 따라 수집되고 관리되며, https://www.microsoft.com/trustcenter에 있습니다.

## <a name="get-support"></a>지원 받기

현재 직면하고 있는 문제에 따라 두 가지 지원 채널이 있습니다.

고객 사용 기여 보고서 보기 또는 로그인과 같은 파트너 센터에서 문제가 발생하면 여기에서 파트너 센터 지원 팀에 지원 요청을 작성하십시오.[https://partner.microsoft.com/support](https://partner.microsoft.com/support)

![](./media/marketplace-publishers-guide/partner-center-log-in-support.png)

고객 사용 기여 를 설정하는 방법과 같이 일반적으로 마켓플레이스 온보딩 및/또는 고객 사용 기여에 대한 지원이 필요한 경우 다음 단계를 따르세요.

1. [지원 페이지](https://go.microsoft.com/fwlink/?linkid=844975)로 이동합니다.

1. **문제 유형** 아래에서 **Marketplace 온보딩**을 선택합니다.

1. 다음과 같이 문제에 대한 **범주**를 선택합니다.

   - 사용량 관련 문제의 경우 **기타**를 선택합니다.
   - Azure 마켓플레이스의 액세스 문제의 경우 **액세스 문제를**선택합니다.

     ![문제 범주 선택](media/marketplace-publishers-guide/lu-article-incident.png)

1. **요청 시작을**선택합니다.

1. 다음 페이지에서 필요한 값을 입력합니다. **계속**을 선택합니다.

1. 다음 페이지에서 필요한 값을 입력합니다.

   > [!Important]
   > **인시던트 제목** 상자에서 **ISV 사용량 추적**을 입력합니다. 문제를 자세히 설명하세요.

   ![인시던트 제목에 대한 ISV 사용량 추적 입력](media/marketplace-publishers-guide/guid-dev-center-help-hd%201.png)

1. 양식을 완성한 다음, **제출**을 선택합니다.

또한 Microsoft 파트너 기술 컨설턴트로부터 기술 사전 판매, 배포 및 앱 개발 시나리오를 통해 고객 사용 기여도를 이해하고 통합할 수 있는 기술 지침을 받을 수 있습니다.

### <a name="how-to-submit-a-technical-consultation-request"></a>기술 상담 요청을 제출하는 방법

1. [https://aka.ms/TechnicalJourney](https://aka.ms/TechnicalJourney)로 이동합니다.
1. 클라우드 인프라 및 관리를 선택하면 기술 여정을 볼 수 있는 새 페이지가 열립니다.
1. 배포 서비스에서 요청 제출 단추를 클릭합니다.
1. MSA(MPN 계정) 또는 AAD(파트너 대시보드 계정)를 사용하여 로그인합니다. 로그인 자격 증명에 따라 온라인 요청 양식이 열립니다.
    * 연락처 정보를 완료/검토합니다.
    * 상담 세부 사항은 미리 채우거나 드롭다운에서 선택할 수 있습니다.
    * 제목과 문제에 대한 설명을 입력합니다(가능한 한 많은 세부 정보 제공).
1. 제출을 클릭합니다.

에서 스크린샷이 포함된 단계별 [https://aka.ms/TechConsultInstructions](https://aka.ms/TechConsultInstructions)지침을 봅니다.

### <a name="whats-next"></a>다음 단계

Microsoft 파트너 기술 컨설턴트가 사용자의 요구 범위를 넓우기 위한 호출을 설정하라는 연락을 받게 됩니다.

## <a name="faq"></a>FAQ

**템플릿에 GUID를 추가하면 어떤 이점이 있나요?**

Microsoft는 파트너에게 솔루션의 고객 배포에 대한 뷰와 영향을 받는 사용에 대한 통찰력을 제공합니다. Microsoft와 파트너는 모두 이 정보를 사용하여 영업 팀 간에 긴밀한 관계를 구축할 수 있습니다. Microsoft와 파트너는 이 정보를 사용하여 개별 파트너가 Azure 성장에 미치는 영향에 대한 일관된 보기를 얻을 수 있습니다.

**GUID는 추가된 후에 변경할 수 있나요?**

예, 고객 또는 구현 파트너는 템플릿을 사용자 지정할 수 있으며 GUID를 변경하거나 제거할 수 있습니다. 파트너는 GUID를 제거하거나 편집하지 못하도록 고객 및 파트너에게 리소스 및 GUID의 역할을 사전에 설명하는 것이 좋습니다. GUID가 변경되면 기존이 아닌 새 배포 및 리소스에만 영향을 줍니다.

**GitHub 등의 타사 리포지토리에서 배포된 템플릿을 추적할 수 있나요?**

예, 템플릿을 배포할 때 GUID가 있는 한 사용량이 추적됩니다. 파트너는 Azure 마켓플레이스 외부의 배포에 사용되는 GUID를 등록하려면 파트너 센터의 상용 마켓플레이스 등록에 프로필이 있어야 합니다.

**고객도 보고를 받나요?**

고객은 Azure Portal 내에서 개별 리소스 또는 고객 정의 리소스 그룹의 사용량을 추적할 수 있습니다.

**이 방법론은 디지털 레코드 파트너(DPOR)와 유사합니까?**

배포 및 사용량을 파트너 솔루션에 연결하는 이 새로운 방법은 파트너 솔루션을 Azure 사용량에 연결하는 메커니즘을 제공합니다. DPOR는 컨설팅(시스템 통합자) 또는 관리(관리 서비스 공급자) 파트너를 고객의 Azure 구독과 연결하기 위한 것입니다.

**Azure Storage의 GUID 생성기 양식을 사용하면 어떤 이점이 있나요?**

Azure Storage의 GUID 생성기 양식은 필요한 형식의 GUID를 생성하도록 보장합니다. 또한, Azure Storage의 데이터 평면 추적 방법을 사용하는 경우 Marketplace 제어 평면 추적에 동일한 GUID를 활용할 수 있습니다. 이렇게 하면 별도의 GUIDS를 유지하지 않고도 파트너 특성으로 단일 통합 GUID를 활용할 수 있습니다.

**Azure 마켓플레이스에서 솔루션 템플릿 오퍼에 개인 맞춤형 VHD를 사용할 수 있습니까?**

아니요, 할 수 없습니다. 가상 시스템 이미지는 Azure 마켓플레이스에서 제공되어야 합니다. [https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines](https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines)

사용자 지정 VHD를 사용하여 마켓플레이스에서 VM 오퍼를 만들고 비공개로 표시하여 아무도 볼 수 없도록 할 수 있습니다. 그런 다음 솔루션 템플릿에서 이 VM을 참조합니다.

**기본 템플릿에 대한 *contentVersion* 속성을 업데이트하지 못했습니까?**

어떤 이유로 이전 contentVersion을 기대하는 다른 템플릿의 TemplateLink를 사용하여 템플릿을 배포할 때 버그가 발생할 수 있습니다. 해결 방법은 메타데이터 속성을 사용하는 것입니다.

```
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "metadata": {
        "contentVersion": "1.0.1.0"
    },
    "parameters": {
```
