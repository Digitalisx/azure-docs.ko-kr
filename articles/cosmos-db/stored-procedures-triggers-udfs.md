---
title: Azure Cosmos DB에서 저장된 프로시저, 트리거 및 UdF로 작업
description: 이 문서에서는 Azure Cosmos DB의 저장 프로시저, 트리거 및 사용자 정의 함수와 같은 개념을 소개합니다.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 23a14e7590eca6f63c92acdf6336ffaef8b54381
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80065904"
---
# <a name="stored-procedures-triggers-and-user-defined-functions"></a>저장 프로시저, 트리거 및 사용자 정의 함수

Azure Cosmos DB는 JavaScript의 언어 통합형, 트랜잭션 실행을 제공합니다. Azure Cosmos DB에서 SQL API를 사용하는 경우 JavaScript 언어로 **저장 프로시저**, **트리거** 및 **UDF(사용자 정의 함수)** 를 작성할 수 있습니다. 데이터베이스 엔진 내에서 실행되는 JavaScript로 논리를 작성할 수 있습니다. [Azure Portal](https://portal.azure.com/), [Azure Cosmos DB의 JavaScript LINQ(Language-Integrated Query)](javascript-query-api.md) 또는 [Cosmos DB SQL API 클라이언트 SDK](how-to-use-stored-procedures-triggers-udfs.md)를 사용하여 트리거, 저장 프로시저 및 UDF를 만들고 실행할 수 있습니다.

## <a name="benefits-of-using-server-side-programming"></a>서버 쪽 프로그래밍 사용의 이점

JavaScript에서 저장 프로시저, 트리거 및 UDF(사용자 정의 함수)를 작성하면 서식 있는 애플리케이션을 빌드하고 다음과 같은 이점을 얻을 수 있습니다.

* **절차 논리:** JavaScript는 비즈니스 논리를 표현하기 위해 풍부하고 친숙한 인터페이스를 제공하는 고급 프로그래밍 언어로 사용됩니다. 데이터에 대해 복잡한 작업 시퀀스를 수행할 수 있습니다.

* **원자성 트랜잭션:** Azure Cosmos DB는 단일 저장 프로시저 또는 트리거 내에서 수행되는 데이터베이스 작업이 원자성임을 보장합니다. 이 원자성 기능을 사용하면 애플리케이션이 관련 작업을 단일 배치로 결합하여 모든 작업이 성공하거나 모두 실패하도록 할 수 있습니다.

* **성능:** JSON 데이터는 본질적으로 자바 스크립트 언어 유형 시스템에 매핑됩니다. 이 매핑을 사용하면 버퍼 풀에 있는 JSON 문서의 지연 구체화, 실행 중인 코드에서 요청 시 사용 가능과 같은 다양한 최적화가 허용됩니다. 데이터베이스에 비즈니스 논리를 전달할 경우 다음과 같은 기타 성능상의 이점이 있습니다.

   * *일괄 처리:* 삽입과 같은 작업을 그룹화하고 일괄제출할 수 있습니다. 개별 트랜잭션을 만들기 위한 네트워크 트래픽 지연 비용 및 저장소 오버헤드가 크게 줄어듭니다.

   * *사전 편집:* 저장 프로시저, 트리거 및 UdF는 각 스크립트 호출 시 컴파일 비용을 피하기 위해 byte 코드 형식으로 암시적으로 미리 컴파일됩니다. 사전 컴파일로 인해 저장 프로시저 호출이 빨라지고 사용 공간이 줄어듭니다.

   * *시퀀싱:* 경우에 따라 작업에 하나 또는 추가 업데이트데이터를 수행할 수 있는 트리거링 메커니즘이 필요합니다. 서버 쪽에서 실행할 경우 원자성 이외의 성능상 이점도 얻을 수 있습니다.

* **캡슐화:** 저장 프로시저는 논리를 한 곳에서 그룹화하는 데 사용할 수 있습니다. 캡슐화는 데이터 위에 추상 계층을 추가하므로 데이터와 독립적으로 애플리케이션을 개발할 수 있습니다. 이 추상화 계층은 데이터에 스키마가 없으며 애플리케이션에 직접 논리를 추가할 필요가 없는 경우에 유용합니다. 추상화를 사용하면 스크립트에서 액세스를 간소화하여 데이터를 안전하게 유지할 수 있습니다.

> [!TIP]
> 저장 프로시저는 쓰기가 많고 파티션 키 값에 대한 트랜잭션이 필요한 작업에 가장 적합합니다. 저장 프로시저를 사용할지 여부를 결정할 때는 가능한 최대 쓰기 양을 캡슐화하는 데 최적화합니다. 일반적으로 저장 프로시저는 많은 수의 읽기 또는 쿼리 작업을 수행하는 가장 효율적인 수단이 아니므로 저장 프로시저를 사용하여 많은 수의 읽기를 일괄 처리하여 클라이언트로 반환하는 것이 원하는 이점을 얻지 못할 수 있습니다. 최상의 성능을 위해 이러한 읽기 무거운 작업은 Cosmos SDK를 사용하여 클라이언트 측에서 수행해야 합니다. 

## <a name="transactions"></a>의

일반적인 데이터베이스의 트랜잭션은 하나의 논리적 작업 단위로 수행되는 작업 시퀀스로 정의할 수 있습니다. 각 트랜잭션에서는 **ACID 속성 보장**을 제공합니다. ACID는 잘 알려진 약어입니다: **A**토미성, **C**아포스텐시, **I**solation, 및 **D**운뇨성. 

* 원자성은 트랜잭션 내부에서 수행된 모든 작업이 하나의 단위로 처리되어 모두 커밋되거나 커밋되지 않도록 합니다. 

* 일관성은 데이터가 트랜잭션 간에 항상 올바른 상태로 유지되도록 합니다. 

* 격리는 두 트랜잭션이 서로를 방해하지 않도록 합니다. 많은 상용 시스템은 애플리케이션 요구에 따라 사용할 수 있는 여러 격리 수준을 제공합니다. 

* 내구성은 데이터베이스에서 커밋된 변경 내용이 항상 유지되도록 합니다.

Azure Cosmos DB에서 JavaScript 런타임의 호스트는 데이터베이스 엔진입니다. 따라서 저장 프로시저 및 트리거 내에서 수행된 요청이 동일한 데이터베이스 세션 범위에서 실행됩니다. 이 기능을 통해 Azure Cosmos DB에서 저장 프로시저 또는 트리거에 속하는 모든 작업에 대해 ACID 속성을 보장할 수 있습니다. 예제를 보려면 [트랜잭션 구현 방법](how-to-write-stored-procedures-triggers-udfs.md#transactions) 문서를 참조하세요.

### <a name="scope-of-a-transaction"></a>트랜잭션의 범위

저장 프로시저는 Azure Cosmos 컨테이너와 연결되고 저장 프로시저 실행은 논리 파티션 키로 범위가 조정됩니다. 저장 프로시저는 트랜잭션 범위에 대한 논리 파티션을 정의하는 실행 중에 논리 파티션 키 값을 포함해야 합니다. 자세한 내용은 [Azure Cosmos DB 분할](partition-data.md) 문서를 참조하세요.

### <a name="commit-and-rollback"></a>커밋 및 롤백

트랜잭션은 기본적으로 Azure Cosmos DB의 JavaScript 프로그래밍 모델에 통합됩니다. JavaScript 함수 내부에서 모든 작업은 자동으로 단일 트랜잭션 아래에 래핑됩니다. 저장 프로시저의 JavaScript 논리가 예외 없이 완료되면 트랜잭션 내의 모든 작업이 데이터베이스로 커밋됩니다. `BEGIN TRANSACTION` 및 `COMMIT TRANSACTION`(관계형 데이터베이스에 잘 알려짐)과 같은 문은 Azure Cosmos DB에서 암시적입니다. 스크립트에서 발생한 예외가 있을 경우 Azure Cosmos DB의 JavaScript 런타임이 전체 트랜잭션을 롤백합니다. 따라서 예외를 발생시키는 것은 결과적으로 Azure Cosmos DB의 `ROLLBACK TRANSACTION`과 동일합니다.

### <a name="data-consistency"></a>데이터 일관성

저장 프로시저와 트리거는 항상 Azure Cosmos 컨테이너의 주 복제본에서 실행됩니다. 이 기능은 저장 프로시저의 읽기에서 [강력한 일관성](consistency-levels-tradeoffs.md)을 제공합니다. 사용자 정의 함수를 사용하는 쿼리는 주 또는 보조 복제본에서 실행할 수 있습니다. 저장 프로시저 및 트리거는 트랜잭션 쓰기를 지원하기 위한 것이지만, [Azure Cosmos DB SQL API SDK](sql-api-dotnet-samples.md)를 사용하는 애플리케이션 쪽 논리 및 쿼리가 데이터베이스 처리량의 범위를 제한하는 데 도움이 되므로 읽기 전용 논리가 가장 잘 구현됩니다. 

## <a name="bounded-execution"></a>제한된 예외

모든 Azure Cosmos DB 작업은 지정된 시간 제한 기간 내에 완료되어야 합니다. 이 제약 조건은 JavaScript 함수인 저장 프로시저, 트리거 및 사용자 정의 함수에도 적용됩니다. 작업이 시간 제한 내에 완료되지 않으면 트랜잭션이 롤백됩니다.

따라서 JavaScript 함수가 시간 제한 내에 완료되거나 실행을 일괄 처리/다시 시작하는 연속 기반 모델을 구현될 것으로 보장할 수 있습니다. 시간 제한을 처리하는 저장 프로시저 및 트리거 개발을 간소화하기 위해 Azure Cosmos 컨테이너 아래의 모든 함수(예: 항목 만들기, 읽기, 업데이트 및 삭제)는 해당 작업이 완료되는지 여부를 나타내는 부울 값을 반환합니다. 이 값이 false이면 스크립트가 구성된 값보다 더 많은 시간 및 프로비전된 처리량을 사용하므로 프로시저가 실행을 래핑해야 한다는 것을 나타냅니다. 수락되지 않은 첫 번째 저장소 작업 전에 대기된 작업은 저장 프로시저가 제시간에 완료되고 더 이상 요청을 대기열에 추가하지 않을 경우 완료됩니다. 따라서 JavaScript의 콜백 규칙을 사용하여 한 번에 하나의 작업을 큐에 대기함으로써 스크립트의 제어 흐름을 관리해야 합니다. 스크립트는 서버 쪽 환경에서 실행되므로 엄격하게 관리됩니다. 실행 경계를 반복적으로 위반하는 스크립트는 비활성 상태로 표시되어 실행할 수 없으며, 실행 경계를 준수하도록 다시 만들어야 합니다.

JavaScript 함수는 [프로비저된 처리 용량](request-units.md)에도 적용됩니다. JavaScript 함수는 짧은 시간 내에 많은 요청 단위를 사용할 수 있으며, 프로비전된 처리 용량 한계에 도달하면 속도가 제한될 수 있습니다. 이러한 데이터베이스 작업은 클라이언트에서 동일한 작업을 실행하는 것보다는 약간 더 저렴하지만, 데이터베이스 작업 실행에 소요되는 처리량 외에 스크립트에도 추가 처리량이 사용된다는 점에 유의해야 합니다.

## <a name="triggers"></a>트리거

Azure Cosmos DB는 두 가지 유형의 트리거를 지원합니다.

### <a name="pre-triggers"></a>사전 트리거

Azure Cosmos DB는 Azure Cosmos 항목에 대해 작업을 수행하여 호출할 수 있는 트리거를 제공합니다. 예를 들어, 항목을 작성할 때 사전 트리거를 지정할 수 있습니다. 이 경우 항목을 만들기 전에 사전 트리거가 실행됩니다. 사전 트리거는 입력 매개 변수를 사용할 수 없습니다. 필요한 경우 요청 개체를 사용하여 원래 요청에서 문서 본문을 업데이트할 수 있습니다. 사용자는 트리거를 등록할 때 트리거 실행에 사용되는 작업을 지정할 수 있습니다. 트리거를 `TriggerOperation.Create`로 만든 경우에는 바꾸기 작업에서 해당 트리거를 사용하는 것이 허용되지 않습니다. 예제를 보려면 [트리거 작성 방법](how-to-write-stored-procedures-triggers-udfs.md#triggers) 문서를 참조하세요.

### <a name="post-triggers"></a>사후 트리거

사전 트리거와 마찬가지로 사후 트리거도 Azure Cosmos 항목의 작업과 연결되며 입력 매개 변수가 필요하지 않습니다. 작업이 완료된 *후* 실행되며 클라이언트에 전송되는 응답 메시지에 액세스할 수 있습니다. 예제를 보려면 [트리거 작성 방법](how-to-write-stored-procedures-triggers-udfs.md#triggers) 문서를 참조하세요.

> [!NOTE]
> 등록된 트리거는 해당 작업(생성/삭제/바꾸기/업데이트 생성/삭제/업데이트)이 발생할 때 자동으로 실행되지 않습니다. 이러한 작업을 실행할 때 등록된 트리거를 명시적으로 호출해야 합니다. 자세한 내용은 [트리거 를 실행하는 방법을](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) 참조하세요.

## <a name="user-defined-functions"></a><a id="udfs"></a>사용자 정의 기능

UDF(사용자 정의 함수)는 SQL API 쿼리 언어 구문을 확장하고 사용자 지정 비즈니스 논리를 쉽게 구현하는 데 사용됩니다. 이 함수는 쿼리 내에서만 호출할 수 있습니다. UDF는 컨텍스트 개체에 액세스할 수 없으며 컴퓨팅 전용 JavaScript로 사용되어야 합니다. 따라서 UDF는 보조 복제본에서 실행할 수 있습니다. 예제를 보려면 [사용자 정의 함수 작성 방법](how-to-write-stored-procedures-triggers-udfs.md#udfs) 문서를 참조하세요.

## <a name="javascript-language-integrated-query-api"></a><a id="jsqueryapi"></a>JavaScript LINQ(Language-Integrated Query) API

SQL API 쿼리 구문을 사용하여 쿼리를 실행하는 것외에, [서버 쪽 SDK](https://azure.github.io/azure-cosmosdb-js-server)를 사용하면 SQL에 대한 지식 없이도 JavaScript 인터페이스를 사용하여 쿼리를 수행할 수 있습니다. JavaScript 쿼리 API를 사용하면 조건자 함수를 함수 호출 시퀀스로 전달하여 프로그래밍 방식으로 쿼리를 빌드할 수 있습니다. 쿼리는 JavaScript 런타임에서 구문 분석되며 Azure Cosmos DB 내에서 효율적으로 실행됩니다. JavaScript 쿼리 API 지원에 대해 자세히 알아보려면 [ API 작업](javascript-query-api.md) 문서를 참조하세요. 예제를 보려면 [Javascript 쿼리 API를 사용하여 저장 프로시저 및 트리거를 작성하는 방법](how-to-write-javascript-query-api.md) 문서를 참조하세요.

## <a name="next-steps"></a>다음 단계

다음 문서를 사용하여 Azure Cosmos DB에서 저장 프로시저, 트리거 및 사용자 정의 함수를 작성하고 사용하는 방법을 알아봅니다.

* [저장 프로시저, 트리거 및 사용자 정의 함수를 작성하는 방법](how-to-write-stored-procedures-triggers-udfs.md)

* [저장 프로시저, 트리거 및 사용자 정의 함수를 사용하는 방법](how-to-use-stored-procedures-triggers-udfs.md)

* [JavaScript LINQ(Language-Integrated Query) 작업](javascript-query-api.md)
