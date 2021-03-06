---
title: Azure 블록체인 서비스 컨소시엄
description: Azure 블록 체인 서비스가 컨소시엄 블록 체인 네트워크를 구현하는 방법에 대한 개요.
ms.date: 11/21/2019
ms.topic: conceptual
ms.reviewer: zeyadr
ms.openlocfilehash: 7b8885ba08d35db20d1eb7e75141cb173913b386
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247619"
---
# <a name="azure-blockchain-service-consortium"></a>Azure 블록체인 서비스 컨소시엄

Azure 블록 체인 서비스를 사용하면 각 블록 체인 네트워크가 네트워크의 특정 참가자로 제한 될 수있는 개인 컨소시엄 블록 체인 네트워크를 만들 수 있습니다. 개인 컨소시엄 블록 체인 네트워크의 참가자만 블록 체인을보고 상호 작용할 수 있습니다. Azure 블록 체인 서비스의 컨소시엄 네트워크에는 두 가지 유형의 구성원 참가자 역할이 포함될 수 있습니다.

* **관리자** - 컨소시엄 관리 작업을 수행하고 블록 체인 거래에 참여할 수있는 권한이있는 참가자.

* **사용자** - 컨소시엄 관리 조치를 취할 수는 없지만 블록 체인 거래에 참여할 수있는 참가자.

컨소시엄 네트워크는 참가자 역할을 혼합할 수 있으며 각 역할 유형의 임의 수를 가질 수 있습니다. 관리자가 하나 이상 있어야 합니다.

다음 다이어그램은 여러 참가자가 있는 컨소시엄 네트워크를 보여 주며,

![개인 컨소시엄 네트워크 다이어그램](./media/consortium/network-diagram.png)

Azure 블록 체인 서비스의 컨소시엄 관리를 통해 컨소시엄 네트워크에서 참가자를 관리할 수 있습니다. 컨소시엄의 관리는 네트워크의 합의 모델을 기반으로합니다. 현재 미리 보기 릴리스에서 Azure 블록 체인 서비스는 컨소시엄 관리를위한 중앙 집중식 합의 모델을 제공합니다. 관리 역할이 있는 권한 있는 참가자는 네트워크에서 참가자를 추가하거나 제거하는 등의 컨소시엄 관리 작업을 수행할 수 있습니다.

## <a name="roles"></a>역할

컨소시엄의 참가자는 개인 또는 조직일 수 있으며 사용자 역할 또는 관리자 역할을 할당할 수 있습니다. 다음 표에는 두 역할 간의 높은 수준의 차이점이 나열되어 있습니다.

| 작업 | 사용자 역할 | 관리자 역할
|--------|:----:|:------------:|
| 새 멤버 만들기 | yes | yes |
| 신입회원 초대 | 예 | yes |
| 멤버 참여자 역할 설정 또는 변경 | 예 | yes |
| 멤버 표시 이름 변경 | 자신의 회원에 대해서만 | 자신의 회원에 대해서만 |
| 구성원 제거 | 자신의 회원에 대해서만 | yes |
| 블록체인 거래 참여 | yes | yes |

### <a name="user-role"></a>사용자 역할

사용자는 관리자 기능이 없는 컨소시엄 참가자입니다. 컨소시엄과 관련된 구성원 관리에 참여할 수 없습니다. 사용자는 구성원 표시 이름을 변경할 수 있으며 컨소시엄에서 자신을 제거할 수 있습니다.

### <a name="administrator"></a>관리자

관리자는 컨소시엄 내의 구성원을 관리할 수 있습니다. 관리자는 컨소시엄 내에서 구성원을 초대하거나, 구성원을 제거하거나, 구성원 역할을 업데이트할 수 있습니다.
컨소시엄 내에 항상 관리자가 하나 이상 있어야 합니다. 마지막 관리자는 컨소시엄을 떠나기 전에 다른 참가자를 관리자 역할로 지정해야 합니다.

## <a name="managing-members"></a>구성원 관리

관리자만 다른 참가자를 컨소시엄에 초대할 수 있습니다. 관리자는 Azure 구독 ID를 사용하여 참가자를 초대합니다.

초대를 받으면 참가자는 Azure 블록 체인 서비스에 새 멤버를 배포하여 블록 체인 컨소시엄에 가입 할 수 있습니다. 초대된 컨소시엄을 보고 가입하려면 네트워크 관리자가 초대에 사용된 것과 동일한 Azure 구독 ID를 지정해야 합니다.

관리자는 다른 관리자를 포함하여 컨소시엄에서 모든 참가자를 제거할 수 있습니다. 회원은 컨소시엄에서만 자신을 제거할 수 있습니다.

## <a name="consortium-management-smart-contract"></a>컨소시엄 관리 스마트 계약

Azure 블록 체인 서비스의 컨소시엄 관리는 컨소시엄 관리 스마트 계약을 통해 수행됩니다. 스마트 계약은 새 블록 체인 멤버를 배포 할 때 노드에 자동으로 배포됩니다.

루트 컨소시엄 관리 스마트 계약의 주소는 Azure 포털에서 볼 수 있습니다. **RootContract 주소는** 블록 체인 회원의 개요 섹션에 있습니다.

![루트 계약 주소](./media/consortium/rootcontract-address.png)

컨소시엄 관리 [PowerShell 모듈,](manage-consortium-powershell.md)Azure 포털을 사용하거나 Azure 블록 체인 서비스 생성 이더리움 계정을 사용하여 스마트 계약을 통해 직접 컨소시엄 관리 스마트 계약과 상호 작용할 수 있습니다.

## <a name="ethereum-account"></a>이더리움 계정

멤버가 생성되면 이더리움 계정 키가 만들어집니다. Azure 블록 체인 서비스는 컨소시엄 관리와 관련된 트랜잭션을 만들기 위해 키를 사용합니다. 이더리움 계정 키는 Azure 블록체인 서비스에서 자동으로 관리됩니다.

멤버 계정은 Azure 포털에서 볼 수 있습니다. 회원 계정은 블록체인 회원의 개요 섹션에 있습니다.

![회원 계정](./media/consortium/member-account.png)

회원 계정을 클릭하고 새 암호를 입력하여 이더리움 계정을 재설정할 수 있습니다. 이더리움 계정 주소와 비밀번호가 모두 재설정됩니다.  

## <a name="next-steps"></a>다음 단계

컨소시엄 관리 작업은 PowerShell을 통해 액세스할 수 있습니다. 자세한 내용은 [PowerShell을 사용하여 Azure 블록 체인 서비스에서 컨소시엄 구성원 관리를](manage-consortium-powershell.md)참조하십시오.
