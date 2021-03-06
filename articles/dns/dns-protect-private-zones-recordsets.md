---
title: 개인 DNS 영역 및 레코드 보호 - Azure DNS
description: 이 학습 경로에서 Microsoft Azure DNS에서 개인 DNS 영역 및 레코드 집합 보호를 시작합니다.
services: dns
author: asudbring
ms.service: dns
ms.topic: article
ms.date: 02/18/2020
ms.author: allensu
ms.openlocfilehash: da94c9aa97483ab5792e917d6a8f60f846b0722e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77473065"
---
# <a name="how-to-protect-private-dns-zones-and-records"></a>개인 DNS 영역 및 레코드를 보호하는 방법

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

개인 DNS 영역 및 레코드는 중요한 리소스입니다. DNS 영역 또는 단일 DNS 레코드를 삭제하면 서비스 중단이 발생할 수 있습니다. DNS 영역 및 레코드는 무단 또는 우발적인 변경으로부터 보호되는 것이 중요합니다.

이 문서에서는 Azure DNS를 사용하여 이러한 변경 내용으로부터 개인 DNS 영역 및 레코드를 보호하는 방법을 설명합니다.  Azure 리소스 관리자에서 제공하는 두 가지 강력한 증권 기능인 [역할 기반 액세스 제어](../role-based-access-control/overview.md) 및 리소스 [잠금을](../azure-resource-manager/management/lock-resources.md)적용합니다.

## <a name="role-based-access-control"></a>역할 기반 액세스 제어

Azure RBAC(Role-Based Access Control)는 Azure 사용자, 그룹, 리소스에 대해 정확한 액세스 관리를 지원합니다. RBAC를 사용하면 사용자에게 필요한 액세스 수준을 부여할 수 있습니다. RBAC를 사용하여 액세스를 관리하는 방법에 대한 자세한 내용은 [역할 기반 Access Control이란](../role-based-access-control/overview.md)을 참조하세요.

### <a name="the-private-dns-zone-contributor-role"></a>개인 DNS 영역 기여자 역할

개인 DNS 영역 참여자 역할은 개인 DNS 리소스를 관리하기 위한 기본 제공 역할입니다. 사용자 또는 그룹에 적용되는 이 역할을 사용하면 개인 DNS 리소스를 관리할 수 있습니다.

리소스 그룹 *myPrivateDNS* Contoso Corporation에 대 한 5 개의 영역이 포함 되어 있습니다. DNS 관리자에게 해당 리소스 그룹에 개인 DNS 영역 기여자 권한을 부여하면 해당 DNS 영역을 완전히 제어할 수 있습니다. 불필요한 사용 권한을 부여하지 않습니다. DNS 관리자는 가상 컴퓨터를 만들거나 중지할 수 없습니다.

RBAC 권한을 할당하는 가장 간단한 방법은 [Azure Portal을 사용](../role-based-access-control/role-assignments-portal.md)하는 것입니다.  

리소스 그룹에 대한 **IAM(액세스 제어 열기)을** 선택한 다음 **추가**를 선택한 다음 **개인 DNS 영역 참여자** 역할을 선택합니다. 사용 권한을 부여할 필수 사용자 또는 그룹을 선택합니다.

![Azure Portal을 통한 리소스 그룹 수준 RBAC](./media/dns-protect-private-zones-recordsets/rbac1.png)

권한은 [Azure PowerShell을 사용하여 부여](../role-based-access-control/role-assignments-powershell.md)할 수도 있습니다.

```azurepowershell-interactive
# Grant 'Private DNS Zone Contributor' permissions to all zones in a resource group

$rsg = "<resource group name>"
$usr = "<user email address>"
$rol = "Private DNS Zone Contributor"

New-AzRoleAssignment -SignInName $usr -RoleDefinitionName $rol -ResourceGroupName $rsg
```

동일한 명령을 [Azure CLI를 통해 사용](../role-based-access-control/role-assignments-cli.md)할 수도 있습니다.

```azurecli-interactive
# Grant 'Private DNS Zone Contributor' permissions to all zones in a resource group

az role assignment create \
--assignee "<user email address>" \
--role "Private DNS Zone Contributor" \
--resource-group "<resource group name>"
```

### <a name="private-zone-level-rbac"></a>프라이빗 존 레벨 RBAC

구독, 리소스 그룹 또는 개별 리소스에 Azure RBAC 규칙을 적용할 수 있습니다. 해당 리소스는 개별 DNS 영역 또는 개별 레코드 집합일 수 있습니다.

예를 들어 리소스 그룹 *myPrivateDNS에는* *영역 private.contoso.com* 및 하위 영역 *customers.private.contoso.com*포함됩니다. CNAME 레코드는 각 고객 계정에 대해 만들어집니다. CNAME 레코드를 관리하는 데 사용되는 관리자 계정에는 *customers.private.contoso.com* 영역에서 레코드를 만들 수 있는 권한이 할당됩니다. 계정은 *customers.private.contoso.com* 만 관리할 수 있습니다.

영역 수준 RBAC 권한은 Azure Portal을 통해 부여할 수 있습니다.  영역에 대한 **IAM(액세스 제어 열기)을** 선택한 다음 **추가를**선택한 다음 **개인 DNS 영역 참여자** 역할을 선택합니다. 사용 권한을 부여할 필수 사용자 또는 그룹을 선택합니다.

![Azure Portal을 통한 DNS 영역 수준 RBAC](./media/dns-protect-private-zones-recordsets/rbac2.png)

권한은 [Azure PowerShell을 사용하여 부여](../role-based-access-control/role-assignments-powershell.md)할 수도 있습니다.

```azurepowershell-interactive
# Grant 'Private DNS Zone Contributor' permissions to a specific zone

$rsg = "<resource group name>"
$usr = "<user email address>"
$zon = "<zone name>"
$rol = "Private DNS Zone Contributor"
$rsc = "Microsoft.Network/privateDnsZones"

New-AzRoleAssignment -SignInName $usr -RoleDefinitionName $rol -ResourceGroupName $rsg -ResourceName $zon -ResourceType $rsc
```

동일한 명령을 [Azure CLI를 통해 사용](../role-based-access-control/role-assignments-cli.md)할 수도 있습니다.

```azurecli-interactive
# Grant 'Private DNS Zone Contributor' permissions to a specific zone

az role assignment create \
--assignee <user email address> \
--role "Private DNS Zone Contributor" \
--scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/privateDnsZones/<zone name>/"
```

### <a name="record-set-level-rbac"></a>레코드 집합 수준 RBAC

사용 권한은 레코드 집합 수준에서 적용됩니다.  사용자에게 필요한 항목에 대한 제어권한이 부여되며 다른 변경은 할 수 없습니다.

레코드 집합 수준 RBAC 권한은 레코드 집합 페이지의 **IAM(액세스 제어)** 단추를 사용하여 Azure 포털을 통해 구성할 수 있습니다.

![Azure Portal을 통한 레코드 집합 수준 RBAC](./media/dns-protect-private-zones-recordsets/rbac3.png)

![Azure Portal을 통한 레코드 집합 수준 RBAC](./media/dns-protect-private-zones-recordsets/rbac4.png)

레코드 집합 수준 RBAC 권한은 [Azure PowerShell을 사용하여 부여](../role-based-access-control/role-assignments-powershell.md)할 수도 있습니다.

```azurepowershell-interactive
# Grant permissions to a specific record set

$usr = "<user email address>"
$rol = "Private DNS Zone Contributor"
$sco = 
"/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/privateDnsZones/<zone name>/<record type>/<record name>"

New-AzRoleAssignment -SignInName $usr -RoleDefinitionName $rol -Scope $sco
```

동일한 명령을 [Azure CLI를 통해 사용](../role-based-access-control/role-assignments-cli.md)할 수도 있습니다.

```azurecli-interactive
# Grant permissions to a specific record set

az role assignment create \
--assignee "<user email address>" \
--role "Private DNS Zone Contributor" \
--scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/privateDnsZones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>사용자 지정 역할

기본 제공 개인 DNS 영역 기여자 역할을 사용하면 DNS 리소스를 완전히 제어할 수 있습니다. 사용자 지정 Azure 역할을 빌드하여 보다 세분화된 컨트롤을 제공할 수 있습니다.

CNAM을 관리하는 데 사용되는 계정에는 CNAME 레코드만 관리할 수 있는 권한이 부여됩니다. 계정은 다른 형식의 레코드를 수정할 수 없습니다. 계정은 영역 삭제와 같은 영역 수준 작업을 수행할 수 없습니다.

다음 예는 CNAME 레코드 관리에 대한 사용자 지정 역할 정의를 보여줍니다.

```json
{
    "Name": "Private DNS CNAME Contributor",
    "Id": "",
    "IsCustom": true,
    "Description": "Can manage DNS CNAME records only.",
    "Actions": [
        "Microsoft.Network/privateDnsZones/CNAME/*",
        "Microsoft.Network/privateDNSZones/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
    ],
    "NotActions": [
    ],
    "AssignableScopes": [
        "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
}
```

작업 속성은 다음과 같은 DNS별 권한을 정의합니다.

* `Microsoft.Network/privateDnsZones/CNAME/*`은 CNAME 레코드에 대한 모든 권한을 부여합니다.
* `Microsoft.Network/privateDNSZones/read`을 사용하므로 DNS 개인 영역을 읽을 수 있지만 수정할 수 없습니다.

> [!NOTE]
> 레코드 집합 삭제를 방지하기 위해 사용자 지정 RBAC 역할을 사용하면서 업데이트를 허용하는 것은 효과적인 관리가 아닙니다. 이 방식에서는 레코드 집합을 삭제할 수 없지만 수정은 방지할 수 없습니다.  허용되는 수정 작업에는 레코드 집합을 비우기 위해 레코드를 모두 제거하는 작업을 포함하여 레코드 집합에서 레코드를 추가 및 제거하는 작업이 포함됩니다. 이 경우 DNS 확인 관점에서 레코드 집합을 삭제하는 것과 같은 효과를 가집니다.

사용자 지정 역할 정의는 현재 Azure 포털을 통해 정의할 수 없습니다. 이 역할 정의를 기준으로 하는 사용자 지정 역할은 Azure PowerShell을 사용하여 만들 수 있습니다.

```azurepowershell-interactive
# Create new role definition based on input file

New-AzRoleDefinition -InputFile <file path>
```

Azure CLI를 통해 만들 수도 있습니다.

```azurecli-interactive
# Create new role definition based on input file

az role create -inputfile <file path>
```

그런 다음 이 문서의 앞부분에서 설명한 대로 역할을 기본 역할과 동일한 방식으로 할당할 수 있습니다.

사용자 지정 역할을 생성, 관리 및 할당하는 방법에 대한 자세한 내용은 [Azure RBAC의 사용자 지정 역할을](../role-based-access-control/custom-roles.md)참조하세요.

## <a name="resource-locks"></a>리소스 잠금

Azure Resource Manager는 리소스를 잠그는 다른 유형의 보안 제어를 지원합니다. 리소스 잠금은 리소스에 적용되며 모든 사용자 및 역할에 효과적입니다. 자세한 내용은 [Azure 리소스 관리자를 사용 하 여 리소스 잠금](../azure-resource-manager/management/lock-resources.md)을 참조 합니다.

리소스 잠금에는 **CanNotDelete** 및 **ReadOnly**의 두 가지 유형이 있습니다. 이러한 잠금 유형은 개인 DNS 영역 또는 개별 레코드 집합에 적용할 수 있습니다.  다음 섹션에서는 몇 가지 일반적인 시나리오와 리소스 잠금을 사용하여 지원하는 방법에 대해 설명합니다.

### <a name="protecting-against-all-changes"></a>모든 변경으로부터 보호

변경 이 이루어지지 않도록 하려면 영역에 ReadOnly 잠금을 적용합니다. 이 잠금을 사용하면 새 레코드 집합이 만들어지지 않고 기존 레코드 집합이 수정 되거나 삭제됩니다.

Azure Portal을 통해 영역 수준의 리소스 잠금을 만들 수 있습니다.  DNS 영역 페이지에서 **잠금**을 선택한 다음, **+추가**를 선택합니다.

![Azure Portal을 통한 영역 수준 리소스 잠금](./media/dns-protect-private-zones-recordsets/locks1.png)

영역 수준 리소스 잠금Azure [PowerShell:](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcelock?view=latest)

```azurepowershell-interactive
# Lock a DNS zone

$lvl = "<lock level>"
$lnm = "<lock name>"
$rsc = "<zone name>"
$rty = "Microsoft.Network/privateDnsZones"
$rsg = "<resource group name>"

New-AzResourceLock -LockLevel $lvl -LockName $lnm -ResourceName $rsc -ResourceType $rty -ResourceGroupName $rsg
```

동일한 명령을 [Azure CLI를 통해 사용](https://docs.microsoft.com/cli/azure/lock?view=azure-cli-latest#az-lock-create)할 수도 있습니다.

```azurecli-interactive
# Lock a DNS zone

az lock create \
--lock-type "<lock level>" \
--name "<lock name>" \
--resource-name "<zone name>" \
--namespace "Microsoft.Network" \
--resource-type "privateDnsZones" \
--resource-group "<resource group name>"
```
### <a name="protecting-individual-records"></a>개별 레코드 보호

기존 DNS 레코드 집합이 수정되지 않도록 하려면 레코드 집합에 ReadOnly 잠금을 적용합니다.

> [!NOTE]
> 레코드 집합에 CanNotDelete 잠금을 적용하는 것은 효과적인 관리 방법이 아닙니다. 이 방식에서는 레코드 집합을 삭제할 수 없지만 수정은 방지할 수 없습니다.  허용되는 수정 작업에는 레코드 집합을 비우기 위해 레코드를 모두 제거하는 작업을 포함하여 레코드 집합에서 레코드를 추가 및 제거하는 작업이 포함됩니다. 이 경우 DNS 확인 관점에서 레코드 집합을 삭제하는 것과 같은 효과를 가집니다.

레코드 집합 수준 리소스 잠금은 현재 Azure PowerShell을 사용해야 구성할 수 있으며  Azure 포털 또는 Azure CLI에서는 지원되지 않습니다.

Azure PowerShell

```azurepowershell-interactive
# Lock a DNS record set

$lvl = "<lock level>"
$lnm = "<lock name>"
$rnm = "<zone name>/<record set name>"
$rty = "Microsoft.Network/privateDnsZones"
$rsg = "<resource group name>"

New-AzResourceLock -LockLevel $lvl -LockName $lnm -ResourceName $rnm -ResourceType $rty -ResourceGroupName $rsg
```
### <a name="protecting-against-zone-deletion"></a>영역 삭제로부터 보호

Azure DNS에서 영역이 삭제되면 영역의 모든 레코드 집합이 삭제됩니다.  이 작업은 취소할 수 없습니다. 중요 영역을 실수로 삭제할 경우 비즈니스에 심각한 영향을 미칩니다.  우발적인 영역 삭제로부터 보호하는 것이 중요합니다.

영역에 CanNotDelete 잠금을 적용할 경우 영역이 삭제되지 않습니다. 잠금은 자식 리소스에 의해 상속됩니다. 잠금을 사용하면 영역의 레코드 집합이 삭제되지 않습니다. 위의 메모에 설명된 대로 레코드를 기존 레코드 집합에서 여전히 제거할 수 있으므로 비효율적입니다.

또는 CanNotDelete 잠금을 SOA 레코드 세트와 같은 영역의 레코드 집합에 적용합니다. 레코드 집합도 삭제하지 않고 영역이 삭제되지 않습니다. 이 잠금은 영역 삭제로부터 보호하면서 영역 내의 레코드 세트를 자유롭게 수정할 수 있도록 합니다. 영역을 삭제하려고 시도하는 경우 Azure 리소스 관리자는 이 제거를 검색합니다. 제거는 SOA 레코드 집합도 삭제하고, Azure 리소스 관리자는 SOA가 잠겨 있기 때문에 호출을 차단합니다.  삭제되는 레코드 집합이 없습니다.

다음 PowerShell 명령은 지정된 영역의 SOA 레코드에 대해 CanNotDelete 잠금을 만듭니다.

```azurepowershell-interactive
# Protect against zone delete with CanNotDelete lock on the record set

$lvl = "CanNotDelete"
$lnm = "<lock name>"
$rnm = "<zone name>/@"
$rty = "Microsoft.Network/privateDnsZones/SOA"
$rsg = "<resource group name>"

New-AzResourceLock -LockLevel $lvl -LockName $lnm -ResourceName $rnm -ResourceType $rty -ResourceGroupName $rsg
```
실수로 영역삭제를 방지하는 또 다른 옵션은 사용자 지정 역할을 사용하는 것입니다. 이 역할은 영역을 관리하는 데 사용되는 계정에 영역 삭제 권한이 없는지 확인합니다. 

영역을 삭제해야 하는 경우 다음 2단계 삭제를 적용할 수 있습니다.

 - 첫째, 권한 부여 영역 삭제 권한
 - 둘째, 영역을 삭제할 수 있는 권한을 부여합니다.

사용자 지정 역할은 해당 계정에서 액세스하는 모든 영역에 대해 작동합니다. 구독 소유자와 같은 영역 삭제 권한이 있는 계정은 실수로 영역을 삭제할 수 있습니다.

리소스 잠금 과 사용자 지정 역할이라는 두 가지 방법을 동시에 DNS 영역 보호에 대한 심층 방어 접근 방식으로 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

* RBAC를 사용하는 방법은 [Azure Portal에서 액세스 관리 시작](../role-based-access-control/overview.md)을 참조하세요.
* 리소스 잠금에 대한 자세한 내용은[ Azure Resource Manager를 사용하여 리소스 잠그기](../azure-resource-manager/management/lock-resources.md)를 참조하세요.
