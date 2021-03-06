---
title: Azure 서비스 패브릭 CLI-sfctl 설정 원격 분석
description: Azure 서비스 패브릭 명령줄 인터페이스인 sfctl에 대해 알아봅니다. sfctl 원격 분석을 구성하기 위한 명령 목록을 포함합니다.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 6af5fa944ef399756f9e890ddd77a7f5f32e2bfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76903020"
---
# <a name="sfctl-settings-telemetry"></a>sfctl settings telemetry
이 sfctl 인스턴스에 로컬인 원격 분석 설정을 구성합니다.

sfctl telemetry는 제공된 매개 변수 없이 명령 이름이나 해당 값, sfctl 버전, OS 유형, Python 버전, 명령의 성공 또는 실패, 반환된 오류 메시지를 수집합니다.

## <a name="commands"></a>명령

|명령|설명|
| --- | --- |
| set-telemetry | 원격 분석을 켜거나 끕니다. |

## <a name="sfctl-settings-telemetry-set-telemetry"></a>sfctl settings telemetry set-telemetry
원격 분석을 켜거나 끕니다.

### <a name="arguments"></a>인수

|인수|설명|
| --- | --- |
| --off | 원격 분석을 끕니다. |
| --on | 원격 분석을 켭니다. 기본값입니다. |

### <a name="global-arguments"></a>전역 인수

|인수|설명|
| --- | --- |
| --debug | 로깅의 자세한 정도를 늘려 모든 디버그 로그를 표시합니다. |
| --help -h | 이 도움말 메시지를 표시하고 종료합니다. |
| --output -o | 출력 형식입니다.  허용되는 값\: json, jsonc, table, tsv.  기본값\: json. |
| --query | JMESPath 쿼리 문자열입니다. 자세한 내용 및 예제는 http\://jmespath.org/를 참조하세요. |
| --verbose | 로깅의 자세한 정도를 늘립니다. 전체 디버그 로그를 표시하려면 --debug를 사용합니다. |

### <a name="examples"></a>예

원격 분석을 끕니다.

```
sfctl settings telemetry set_telemetry --off
```

원격 분석을 켭니다.

```
sfctl settings telemetry set_telemetry --on
```


## <a name="next-steps"></a>다음 단계
- 서비스 패브릭 [CLI를 설정합니다.](service-fabric-cli.md)
- [샘플 스크립트](/azure/service-fabric/scripts/sfctl-upgrade-application)를 사용하여 Microsoft Azure Service Fabric CLI를 사용하는 방법에 대해 알아봅니다.