---
title: Azure 코스모스 DB 쿼리 언어의 COT
description: Azure Cosmos DB의 Cotangent(COT) SQL 시스템 함수가 지정된 숫자 식에서 지정된 각도의 삼각법 코탄젠트를 라디안에서 반환하는 방법에 대해 알아봅니다.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 25c907644f58ee40ea08e5636d68dc0e84564a28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299490"
---
# <a name="cot-azure-cosmos-db"></a>COT (Azure 코스모스 DB)
 지정된 숫자 식의 라디안에서 지정된 각도의 삼각 코탄젠트를 반환합니다.  
  
## <a name="syntax"></a>구문
  
```sql
COT(<numeric_expr>)  
```  
  
## <a name="arguments"></a>인수
  
*numeric_expr*  
   숫자 식입니다.  
  
## <a name="return-types"></a>반환 형식
  
  숫자 식을 반환합니다.  
  
## <a name="examples"></a>예
  
  다음 예제는 지정된 `COT` 각도를 계산합니다.  
  
```sql
SELECT COT(124.1332) AS cot  
```  
  
 결과 집합은 다음과 같습니다.  
  
```json
[{"cot": -0.040311998371148884}]  
```  

## <a name="remarks"></a>설명

이 시스템 함수는 인덱스를 사용하지 않습니다.

## <a name="next-steps"></a>다음 단계

- [수학적 함수 Azure 코스모스 DB](sql-query-mathematical-functions.md)
- [시스템 기능 Azure 코스모스 DB](sql-query-system-functions.md)
- [Azure 코스모스 DB 소개](introduction.md)
