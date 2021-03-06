---
title: Azure 정문 - 백엔드 상태 모니터링 | 마이크로 소프트 문서
description: 이 문서에서는 Azure 정문이 백엔드의 상태를 모니터링하는 방법을 이해하는 데 도움이 됩니다.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: e2e656c395f1a31c1f5ebbd46d5a18a046f854f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471577"
---
# <a name="health-probes"></a>상태 프로브

지정된 Front Door 환경에서 각 백 엔드의 상태와 근접성을 확인하기 위해 각 Front Door 환경은 구성된 각 백엔드에 가상 HTTP/HTTPS 요청을 주기적으로 보냅니다. 그런 다음, Front Door는 이러한 프로브의 응답을 사용하여 실제 클라이언트 요청을 라우팅해야 하는 “최상”의 백 엔드를 확인합니다. 

> [!WARNING]
> Front Door는 전 세계적으로 많은 에지 환경을 가지고 있기 때문에, 상태 프로브 가량에 따라 분당 25개 요청에서 분당 최대 1200개의 요청에 이르기까지 백엔드에 대한 상태 프로브 요청 량이 상당히 높을 수 있습니다. 기본 프로브 빈도가 30초인 경우 백엔드의 프로브 볼륨은 분당 약 200개의 요청이어야 합니다.

## <a name="supported-protocols"></a>지원되는 프로토콜

Front Door는 HTTP 또는 HTTPS 프로토콜을 통한 프로브 전송을 지원합니다. 이러한 프로브는 라우팅 클라이언트 요청에 대해 구성된 동일한 TCP 포트를 통해 전송되며, 재정의할 수 없습니다.

## <a name="supported-http-methods-for-health-probes"></a>상태 프로브에 지원되는 HTTP 메서드

Front Door는 상태 프로브를 보내기 위한 다음과 같은 HTTP 메서드를 지원합니다.

1. **Get:** GET 메서드는 요청-URI에 의해 식별되는 모든 정보(엔터티 형식)를 검색하는 것을 의미합니다.
2. **머리:** HEAD 메서드는 서버가 응답에서 메시지 본문을 반환하지 않아야 한다는 점을 제외하면 GET과 동일합니다. 새 전면 도어 프로파일의 경우 기본적으로 프로브 메서드가 HEAD로 설정됩니다.

> [!NOTE]
> 백엔드의 부하와 비용을 낮추기 위해 Front Door는 상태 프로브에 대한 HEAD 요청을 사용하는 것이 좋습니다.

## <a name="health-probe-responses"></a>상태 프로브 응답

| 응답  | 설명 | 
| ------------- | ------------- |
| 상태 확인  |  200 정상 상태 코드는 백 엔드가 정상임을 나타냅니다. 그 외 코드는 모두 실패로 간주됩니다. 어떠한 이유로(네트워크 오류를 포함) 유효한 HTTP 응답이 프로브에 대해 수신되지 않으면 프로브가 실패로 계산됩니다.|
| 대기 시간 측정  | 대기 시간은 응답의 마지막 바이트를 수신하는 순간 프로브 요청을 보내기 직전에 측정된 벽시계 시간입니다. 각 요청에 대한 새 TCP 연결을 사용하므로 이 측정값은 기존 웜 연결을 사용하는 백 엔드에 편향되지 않습니다.  |

## <a name="how-front-door-determines-backend-health"></a>Front Door가 백 엔드 상태를 결정하는 방법

Azure Front Door는 모든 알고리즘에서 아래와 동일한 3단계 프로세스를 사용하여 상태를 확인합니다.

1. 비활성화된 백 엔드를 제외합니다.

2. 상태 프로브 오류가 있는 백 엔드를 제외합니다.
    * 이 선택은 마지막 _n_ 상태 프로브 응답을 확인하여 완료합니다. 최소한 _x_가 정상적인 경우 백 엔드는 정상 상태로 간주됩니다.

    * _n은_ 로드 밸런싱 설정에서 SampleSize 속성을 변경하여 구성됩니다.

    * _x는_ 로드 밸런싱 설정에서 성공샘플링필수 속성을 변경하여 구성됩니다.

3. 백 엔드 풀의 정상 백 엔드 외에, Front Door는 각 백 엔드에 대한 대기 시간(왕복 시간)을 추가로 측정하고 유지합니다.


## <a name="complete-health-probe-failure"></a>상태 프로브 실패 완료

상태 프로브가 백 엔드 풀의 모든 백 엔드에 대해 실패한 경우, Front Door는 모든 백 엔드를 정상으로 간주하고 모든 백 엔드에 걸친 라운드 로빈 배포에 트래픽을 라우팅합니다.

백 엔드가 정상 상태로 돌아오면 Front Door는 일반 로드 밸런싱 알고리즘을 다시 시작합니다.

## <a name="disabling-health-probes"></a>상태 프로브 비활성화

백 엔드 풀에 단일 백 엔드가 있는 경우 응용 프로그램 백 엔드의 부하를 줄이는 상태 프로브를 사용하지 않도록 선택할 수 있습니다. 백 엔드 풀에 여러 백엔드가 있지만 그 중 하나만 활성화된 상태인 경우에도 상태 프로브를 사용하지 않도록 설정할 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [Front Door를 만드는](quickstart-create-front-door.md) 방법을 알아봅니다.
- [Front Door의 작동 원리](front-door-routing-architecture.md)를 알아봅니다.
