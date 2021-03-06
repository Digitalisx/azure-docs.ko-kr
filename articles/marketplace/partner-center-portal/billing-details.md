---
title: 상업용 마켓플레이스 청구 | Azure 마켓플레이스
description: 이 문서에서는 상업 시장에 대한 상거래 관련 주제를 다룹니다.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.topic: guide
ms.date: 12/12/2019
ms.openlocfilehash: a0912e1dca63fabfe6bc546305824a1c582b9a2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80279745"
---
# <a name="commercial-marketplace-billing"></a>상업용 마켓플레이스 청구

이 문서에서는 상업 시장에 대한 상거래 관련 주제를 다룹니다.

- [Marketplace 게시 옵션](#marketplace-publishing-options)
- [거래 일반 개요](#transact-general-overview)
- [거래 청구 모델](#transact-billing-models)

## <a name="marketplace-publishing-options"></a>Marketplace 게시 옵션

상업용 마켓플레이스는 게시자를 위한 몇 가지 게시 옵션을 제공합니다.

### <a name="list-and-trial-publishing-options"></a>목록 및 평가판 게시 옵션

게시자는 홍보 및 사용자 확보를 위해 목록, 평가판 및 BYOL(라이선스) 게시 옵션을 가져올 수 있습니다. 이러한 옵션을 사용하면 Microsoft는 게시자의 소프트웨어 라이센스 트랜잭션에 직접 참여하지 않으며 관련 트랜잭션 수수료가 없습니다. 게시자는 주문, 이행, 계량, 청구, 송장, 지불 및 수금을 포함하되 이에 제한되지 않는 소프트웨어 라이선스 트랜잭션의 모든 측면을 지원할 책임이 있습니다. 목록 및 평가판 게시 옵션을 사용할 때 게시자는 고객으로부터 수금된 게시자 소프트웨어 라이선스 요금을 100% 보유합니다.

### <a name="transact-publishing-option"></a>거래 게시 옵션

목록 및 평가판 게시 옵션 외에도 게시자가 거래 게시 옵션을 사용할 수 있습니다. 이 옵션은 Microsoft에서 전 세계적으로 사용 가능한 상거래 기능을 활용하고 Microsoft가 게시자를 대신하여 클라우드 마켓플레이스 트랜잭션을 호스팅할 수 있도록 합니다.

## <a name="transact-general-overview"></a>거래 일반 개요

거래 게시 옵션을 사용하는 경우 Microsoft는 타사 소프트웨어를 판매하고 일부 제품 유형을 고객의 Azure 구독에 배포할 수 있습니다. 게시자는 청구 모델 및 제안 유형을 선택할 때 인프라 요금 청구 및 게시자의 자체 소프트웨어 라이선스 비용을 고려해야 합니다.

현재 가상 컴퓨터, Azure 응용 프로그램 및 SaaS 응용 프로그램 과 같은 제안 유형에 대해 Transact 게시 옵션이 지원됩니다.

![Azure 마켓플레이스에서 거래](./media/transact-amp.png)

### <a name="billing-infrastructure-costs"></a>인프라 비용 청구

#### <a name="for-virtual-machines-and-azure-applications"></a>가상 컴퓨터 및 Azure 응용 프로그램의 경우

가상 시스템 및 Azure 응용 프로그램의 경우 Azure 인프라 사용 요금은 고객의 Azure 구독에 청구됩니다. 인프라 사용 요금은 가격이 책정되며 고객의 송장에 있는 소프트웨어 공급자의 라이선스 비용과 별도로 제공됩니다.

#### <a name="for-saas-apps"></a>SaaS 앱의 경우

SaaS 앱의 경우 게시자는 Azure 인프라 사용 요금 및 소프트웨어 라이선스 요금을 단일 비용 항목으로 처리해야 합니다. 그것은 고객에게 정액 요금으로 표시됩니다. Azure 인프라 사용량은 관리되며 파트너에게 직접 청구됩니다. 실제 인프라 사용 요금은 고객에게 표시되지 않습니다. 일반적으로 게시자는 Azure 인프라 사용 요금을 소프트웨어 라이선스 가격 책정에 추가하는 것을 선택합니다. 소프트웨어 라이선스 요금은 측정되거나 사용되지 않습니다.

## <a name="transact-billing-models"></a>거래 청구 모델

사용된 거래 옵션에 따라 게시자의 소프트웨어 라이센스 요금은 다음과 같이 표시될 수 있습니다.

- *무료*: 소프트웨어 라이센스에 대한 무료입니다.
- *BYOL(사용자 고유의 라이선스 가져오기)*: 소프트웨어 라이선스에 적용되는 모든 요금은 게시자와 고객 간에 직접 관리됩니다. Microsoft는 Azure 인프라 사용 요금을 통해서만 전달합니다. (Virtual Machines 및 Azure 애플리케이션의 경우만 해당)
- *종량제*: 소프트웨어 라이선스 요금은 사용된 Azure 인프라에 따라 시간당 코어별(vCPU) 가격 책정 요금으로 표시됩니다. 이 모델은 가상 컴퓨터 및 Azure 응용 프로그램에만 적용됩니다.
- *구독 가격*: 소프트웨어 라이센스 요금은 정액 요금 또는 시트당 청구되는 월별 또는 연간 반복 요금으로 제공됩니다. 이 모델은 SaaS 앱 및 Azure 응용 프로그램- 관리되는 앱에만 적용됩니다.
- *무료 소프트웨어 평가판*: 30일 또는 90일 동안 소프트웨어 라이센스에 대한 무료 요금이 부과되지 않습니다.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>무료 및 BYOL(사용자 라이선스 필요) 가격 책정

무료 또는 사용자 라이선스 필요 트랜잭션 제품을 게시하는 경우 Microsoft는 해당 소프트웨어 라이선스 요금에 대한 판매 트랜잭션을 촉진하는 역할을 담당하지 않습니다. 목록 및 평가판 게시 옵션과 같이 게시자는 소프트웨어 라이선스 요금을 100% 보유합니다.

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>종량제 및 구독(사이트 기준) 가격 책정

종량제 또는 구독 트랜잭션 오퍼를 게시할 때 Microsoft는 소프트웨어 라이선스 구매, 반품 및 청구 를 처리하는 기술과 서비스를 제공합니다. 이 시나리오에서 게시자는 Microsoft가 이러한 목적을 위한 에이전트 역할을 하도록 권한을 부여합니다. 게시자는 Microsoft가 판매자, 공급 기업, 배포자 및 라이선스 허가자로서의 역할을 유지하는 동시에 소프트웨어 라이선스 트랜잭션을 촉진하도록 허용합니다.

Microsoft를 사용하면 고객이 Microsoft의 상용 마켓플레이스와 게시자의 최종 사용자 라이선스 계약의 이용 약관에 따라 게시자 소프트웨어를 주문, 라이선스 및 사용할 수 있습니다. 게시자는 최종 사용자 라이선스 계약을 제공하거나 오퍼를 만들 때 [표준 계약을](https://docs.microsoft.com/azure/marketplace/standard-contract) 선택해야 합니다.

### <a name="free-software-trials"></a>소프트웨어 평가판

거래 게시 시나리오의 경우 게시자는 30일 또는 90일 동안 소프트웨어 라이선스를 무료로 사용할 수 있습니다. 이 할인 기능은 파트너 솔루션 사용으로 인한 Azure 인프라 사용 비용은 포함하지 않습니다.

### <a name="private-offers"></a>프라이빗 제품

퍼블리셔는 쿠폰 유형 및 결제 모델을 사용하여 쿠폰으로 수익을 창출할 뿐만 아니라 협상 및 거래별 가격 또는 맞춤 구성을 갖춘 비공개 쿠폰을 거래할 수 있습니다. 프라이빗 제품은 3가지 거래 게시 옵션 모두에서 지원됩니다.

이 옵션을 사용하면 공개적으로 제공되는 제품보다 더 높거나 낮은 가격을 사용할 수 있습니다. 프라이빗 오퍼는 할인또는 프리미엄을 추가하는 데 사용할 수 있습니다. 프라이빗 제품은 제품 수준에서 Azure 구독을 허용 목록에 추가하면 둘 이상의 고객이 사용할 수 있습니다.

### <a name="examples"></a>예

#### <a name="pay-as-you-go"></a>종량제

* 종량제 옵션을 사용하면 다음과 같은 비용 구조가 나타납니다.

|라이선스 비용  | 시간당 $1.00  |
|---------|---------|
|Azure 사용량 비용(D1/1개 코어)    |   시간당 $0.14     |
|*Microsoft에서 고객에게 청구하는 요금*    |  *시간당 $1.14*       |

* 이 시나리오에서 Microsoft는 게시된 VM 이미지 사용에 대해 시간당 $1.14를 청구합니다.

|Microsoft 청구  | 시간당 $1.14  |
|---------|---------|
|Microsoft는 라이선스 비용의 80%를 지불합니다.|   시간당 $0.80     |
|Microsoft는 라이선스 비용의 20%를 유지합니다.  |  시간당 $0.20       |
|Microsoft는 Azure 사용 비용의 100%를 유지합니다. | 시간당 $0.14 |

### <a name="bring-your-own-license-byol"></a>자신의 라이센스 가져오기(BYOL)

* BYOL 옵션을 사용하면 다음과 같은 비용 구조가 나타납니다.

|라이선스 비용  | 사용자가 라이선스 요금을 협상하고 청구합니다.  |
|---------|---------|
|Azure 사용량 비용(D1/1개 코어)    |   시간당 $0.14     |
|*Microsoft에서 고객에게 청구하는 요금*    |  *시간당 $0.14*       |

* 이 시나리오에서 Microsoft는 게시된 VM 이미지 사용에 대해 시간당 $0.14를 청구합니다.

|Microsoft 청구  | 시간당 $0.14  |
|---------|---------|
|Microsoft는 Azure 사용량 비용을 유지합니다.    |   시간당 $0.14     |
|Microsoft은 라이선스 비용의 0%를 유지합니다.   |  시간당 $0.00       |

### <a name="saas-app-subscription"></a>SaaS 앱 구독

이 옵션은 Microsoft를 통해 판매하도록 구성되어야 하며 고정 요금으로 가격을 책정하거나 월별 또는 연간 기준으로 사용자당 가격을 책정할 수 있습니다.

• SaaS 오퍼에 대해 Microsoft를 통한 판매 옵션을 사용하도록 설정하면 다음과 같은 비용 구조가 있습니다.

|라이선스 비용       | 매월 $100.00  |
|--------------|---------|
|Azure 사용량 비용(D1/1개 코어)    | 고객이 아닌 게시자에게 직접 청구됩니다. |
|*Microsoft에서 고객에게 청구하는 요금*    |  *월간 $100.00(참고: 게시자는 발생하거나 통과된 인프라 비용을 라이선스 요금으로 계산해야 함)*  |

* 이 시나리오에서 Microsoft는 소프트웨어 라이선스에 대해 $100.00를 청구하고, 게시자에게 $80.00를 지급합니다.
* 할인된 마켓플레이스 서비스 요금을 받을 자격이 있는 파트너는 2019년 5월부터 2020년 6월까지 SaaS 혜택에 대한 거래 수수료가 감소합니다. 이 시나리오에서 Microsoft는 소프트웨어 라이센스에 대해 $100.00를 청구하고 게시자에게 $90.00를 지불합니다.

|Microsoft 청구  | 매월 $100.00  |
|---------|---------|
|Microsoft는 라이선스 비용의 80%를 지불합니다. <br> \*Microsoft는 자격을 갖춘 SaaS 앱에 대해 라이선스 비용의 90%를 지불합니다.   |   매월 $80.00 <br> \*월 $90.00    |
|Microsoft는 라이선스 비용의 20%를 유지합니다. <br> \*Microsoft는 자격을 갖춘 SaaS 앱에 대해 라이선스 비용의 10%를 유지합니다.  |  매월 $20.00 <br> \*$10.00     |

**마켓플레이스 서비스 요금 인하:** 상용 마켓플레이스에 게시하는 특정 SaaS 제품의 경우 Microsoft는 마켓플레이스 서비스 요금을 20%(Microsoft 게시자 계약에 설명된 대로)에서 10%로 줄입니다.  제품이 자격을 갖추려면 Microsoft에서 제품 중 하나 이상을 IP 공동 판매 준비 또는 IP 공동 판매 우선 순위로 지정해야 합니다. 이 할인된 마켓플레이스 서비스 요금을 받으려면 이전 달이 끝나기 최소 5영업일 전에 자격이 충족되어야 합니다. 할인된 마켓플레이스 서비스 요금은 VM, 관리형 앱 또는 상용 마켓플레이스를 통해 제공되는 기타 제품에는 적용되지 않습니다.  이 할인된 마켓플레이스 서비스 요금은 2019년 5월 1일부터 2020년 6월 30일까지 Microsoft에서 징수한 라이선스 요금과 함께 자격을 갖춘 오퍼에 사용할 수 있습니다.  이 시간이 지나면 마켓플레이스 서비스 요금이 정상 금액으로 반환됩니다.
