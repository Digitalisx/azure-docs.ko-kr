---
title: Azure Security Center에서 사용자 데이터 관리 | Microsoft Docs
description: Azure 보안 센터에서 사용자 데이터를 관리하는 방법을 알아봅니다. 사용자 데이터 관리에는 데이터에 액세스하거나, 데이터를 삭제하거나, 내보내는 기능이 포함됩니다.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: memildin
ms.openlocfilehash: bf715d872fab421de30ebcb146a1981a7d008738
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80585984"
---
# <a name="manage-user-data-in-azure-security-center"></a>Azure Security Center에서 사용자 데이터 관리
이 아티클에서는 Azure Security Center에서 사용자 데이터를 관리하는 방법에 대한 정보를 제공합니다. 사용자 데이터 관리에는 데이터에 액세스하거나, 데이터를 삭제하거나, 내보내는 기능이 포함됩니다.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

읽기 권한자, 소유자, 기여자 또는 계정 관리자 역할이 할당된 Security Center 사용자는 도구 내에서 고객 데이터에 액세스할 수 있습니다. 계정 관리자 역할에 대해 자세히 알아보려면 [Azure 역할 기반 액세스 제어에 대한 기본 제공 역할을](../role-based-access-control/built-in-roles.md) 참조하여 판독기, 소유자 및 기여자 역할에 대해 자세히 알아봅니다. [Azure 구독 관리자를](../cost-management-billing/manage/add-change-subscription-administrator.md)참조하십시오.

## <a name="searching-for-and-identifying-personal-data"></a>개인 데이터 검색 및 식별
Security Center 사용자는 Azure Portal을 통해 해당 개인 데이터를 볼 수 있습니다. Security Center는 이메일 주소, 전화 번호와 같은 보안 연락처 세부 정보만을 저장합니다. 자세한 내용은 [Azure 보안 센터의 보안 연락처 세부 정보 제공을](security-center-provide-security-contact-details.md)참조하십시오.

Azure 포털에서 사용자는 보안 센터의 적시 VM 액세스 기능을 사용하여 허용된 IP 구성을 볼 수 있습니다. 자세한 내용은 [적시에 가상 시스템 액세스 관리를](security-center-just-in-time.md)참조하십시오.

Azure Portal에서 사용자는 IP 주소 및 공격자 세부 정보를 비롯한 Security Center에서 제공하는 보안 경고를 볼 수 있습니다. 자세한 내용은 [Azure 보안 센터의 보안 경고 관리 및 응답을](security-center-managing-and-responding-alerts.md)참조하십시오.

## <a name="classifying-personal-data"></a>개인 데이터 분류
보안 센터의 보안 연락처 기능에 있는 개인 데이터를 분류할 필요가 없습니다. 저장된 데이터는 이메일 주소(또는 여러 이메일 주소)와 전화 번호입니다. Security Center에서 [연락처 데이터](security-center-provide-security-contact-details.md)의 유효성을 검사합니다.

보안 센터의 [적시](security-center-just-in-time.md) 기능에 의해 저장된 IP 주소와 포트 번호를 분류할 필요가 없습니다.

관리자 역할이 할당된 사용자는 Security Center에서 [경고를 보고](security-center-managing-and-responding-alerts.md) 개인 데이터를 분류할 수 있습니다.

## <a name="securing-and-controlling-access-to-personal-data"></a>개인 데이터에 대한 액세스 보안 및 제어
읽기 권한자, 소유자, 기여자 또는 계정 관리자 역할이 할당된 Security Center 사용자는 [보안 연락처 데이터](security-center-provide-security-contact-details.md)에 액세스할 수 있습니다.

리더, 소유자, 기여자 또는 계정 관리자의 역할을 할당한 보안 센터 사용자는 [적시에](security-center-just-in-time.md) 정책에 액세스할 수 있습니다.

읽기 권한자, 소유자, 기여자 또는 계정 관리자 역할이 할당된 Security Center 사용자는 해당 [경고](security-center-managing-and-responding-alerts.md)에 액세스할 수 있습니다.

## <a name="updating-personal-data"></a>개인 데이터 업데이트
소유자, 기여자 또는 계정 관리자 역할이 할당된 Security Center 사용자는 Azure Portal을 통해 [보안 연락처 데이터](security-center-provide-security-contact-details.md)를 업데이트할 수 있습니다.

소유자, 기여자 또는 계정 관리자 역할을 할당한 보안 센터 사용자는 [적시에 정책을](security-center-just-in-time.md)업데이트할 수 있습니다.

계정 관리자는 경고 인시던트를 편집할 수 없습니다. [경고 인시던트](security-center-managing-and-responding-alerts.md)는 보안 데이터로 간주되며 읽기 전용입니다.

## <a name="deleting-personal-data"></a>개인 데이터 삭제
소유자, 기여자 또는 계정 관리자 역할이 할당된 Security Center 사용자는 Azure Portal을 통해 [보안 연락처 데이터](security-center-provide-security-contact-details.md)를 삭제할 수 있습니다.

소유자, 기여자 또는 계정 관리자역할을 할당한 보안 센터 사용자는 Azure 포털을 통해 [적시 정책을](security-center-just-in-time.md) 삭제할 수 있습니다.

보안 센터 사용자는 경고 인시던트를 삭제할 수 없습니다. 보안상의 이유로 [경고 인시던트는](security-center-managing-and-responding-alerts.md) 읽기 전용 데이터로 간주됩니다.

## <a name="exporting-personal-data"></a>개인 데이터 내보내기
읽기 권한자, 소유자, 기여자 또는 계정 관리자 역할이 할당된 Security Center 사용자는 다음을 수행하여 [보안 연락처 데이터](security-center-provide-security-contact-details.md)를 내보낼 수 있습니다.

- Azure 포털에서 복사
- Azure REST API 호출, GET HTTP 실행:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/providers/Microsoft.Security/securityContacts?api-version={api-version}
  ```

계정 관리자 역할을 할당한 보안 센터 사용자는 다음을 통해 IP 주소가 포함된 [적시 정책을](security-center-just-in-time.md) 내보낼 수 있습니다.

- Azure 포털에서 복사
- Azure REST API 호출, GET HTTP 실행:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Security/locations/{location}/jitNetworkAccessPolicies/default?api-version={api-version}
  ```

계정 관리자는 다음을 수행하여 경고 세부 정보를 내보낼 수 있습니다.

- Azure 포털에서 복사
- Azure REST API 호출, GET HTTP 실행:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/providers/microsoft.Security/alerts?api-version={api-version}
  ```

자세한 내용은 [보안 경고 받기(GET 컬렉션)를](https://msdn.microsoft.com/library/mt704050.aspx)참조하십시오.

## <a name="restricting-the-use-of-personal-data-for-profiling-or-marketing-without-consent"></a>동의 없이 프로파일링 또는 마케팅을 위해 개인 데이터 사용 제한
Security Center 사용자는 해당 [보안 연락처 데이터](security-center-provide-security-contact-details.md)를 삭제하여 옵트아웃하도록 선택할 수 있습니다.

[적시 데이터는](security-center-just-in-time.md) 식별할 수 없는 데이터로 간주되며 30일 동안 유지됩니다.

[경고 데이터](security-center-managing-and-responding-alerts.md)는 보안 데이터로 간주되고 2년 동안 유지됩니다.

## <a name="auditing-and-reporting"></a>감사 및 보고
보안 연락처, 적시 및 경고 업데이트의 감사 로그는 [Azure 활동 로그에서](../azure-monitor/platform/platform-logs-overview.md)유지됩니다.