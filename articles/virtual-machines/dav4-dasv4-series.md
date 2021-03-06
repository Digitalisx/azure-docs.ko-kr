---
title: Dav4 및 Dasv4 시리즈 - Azure 가상 컴퓨터
description: Dav4 및 Dasv4 시리즈 VM용 사양입니다.
services: virtual-machines
author: migerdes
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 56e86aa75b153b5cb005c96fca45373d30ffa8b4
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437103"
---
# <a name="dav4-and-dasv4-series"></a>Dav4 및 Dasv4 시리즈

Dav4 시리즈와 Dasv4 시리즈는 AMD의 2.35Ghz EPYC<sup>TM</sup> 7452 프로세서를 최대 256MB L3 캐시로 구성된 다중 스레드 구성의 새로운 크기로, 8GB의 L3 캐시를 모든 8코어에 바침전하여 범용 워크로드를 실행하기 위한 고객 옵션을 증가시고 있습니다. Dav4 시리즈와 Dasv4 시리즈는 D& Dsv3 시리즈와 동일한 메모리 및 디스크 구성을 갖습니다.

## <a name="dav4-series"></a>대브4 시리즈

ACU: 230-260

프리미엄 스토리지: 지원되지 않음

프리미엄 스토리지 캐싱: 지원되지 않음

라이브 마이그레이션: 지원됨

메모리 보존 업데이트: 지원

Dav4 시리즈 크기는 3.35GHz의 최대 주파수를 높일 수 있는 2.35Ghz AMD EPYC<sup>TM</sup> 7452 프로세서를 기반으로 합니다. Dav4 시리즈 크기는 대부분의 프로덕션 워크로드에 vCPU, 메모리 및 임시 스토리지의 조합을 제공합니다. 데이터 디스크 스토리지는 가상 머신과 별도로 비용이 청구됩니다. 프리미엄 SSD를 사용하려면 Dasv4 크기를 사용하십시오. Dasv4 크기의 가격 및 청구 미터는 Dav4 시리즈와 동일합니다.

| 크기 | vCPU | 메모리: GiB | 임시 스토리지(SSD) GiB | 최대 데이터 디스크 수 | 최대 임시 스토리지 처리량: IOPS/읽기 MBps/쓰기 MBps | 최대 NIC / 예상 네트워크 대역폭(MBps) |
|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2a_v4 |  2  | 8  | 50  | 4  | 3000 / 46 / 23   | 2 / 1000 |
| Standard_D4a_v4 |  4  | 16 | 100 | 8  | 6000 / 93 / 46   | 2 / 2000 |
| Standard_D8a_v4 |  8  | 32 | 200 | 16 | 12000 / 187 / 93 | 4 / 4000 |
| Standard_D16a_v4|  16 | 64 | 400 |32  | 24000 / 375 / 187 |8 / 8000 |
| Standard_D32a_v4|  32 | 128| 800 | 32 | 48000 / 750 / 375 |8 / 16000 |
| Standard_D48a_v4| 48 | 192| 1200 | 32 | 96000 / 1000 / 500 | 8 / 24000 |
| Standard_D64a_v4| 64 | 256 | 1600 | 32 | 96000 / 1000 / 500 | 8 / 30000 |
| Standard_D96a_v4| 96 | 384 | 2400 | 32 | 96000 / 1000 / 500 | 8 / 30000 |

## <a name="dasv4-series"></a>Dasv4 시리즈

ACU: 230-260

Premium Storage: 지원됨

프리미엄 스토리지 캐싱: 지원

라이브 마이그레이션: 지원됨

메모리 보존 업데이트: 지원

Dasv4 계열 크기는 2.35Ghz AMD EPYC<sup>TM</sup> 7452 프로세서를 기반으로 하며, 최대 주파수 3.35GHz를 달성하고 프리미엄 SSD를 사용할 수 있습니다. Dasv4 계열 크기는 대부분의 프로덕션 워크로드에 vCPU, 메모리 및 임시 스토리지의 조합을 제공합니다.

| 크기 | vCPU | 메모리: GiB | 임시 스토리지(SSD) GiB | 최대 데이터 디스크 수 | 최대 캐시된 임시 스토리지 처리량: IOPS/MBps(GiB 단위의 캐시 크기) | 최대 캐시되지 않은 디스크 처리량: IOPS/MBps | 최대 NIC / 예상 네트워크 대역폭(MBps) |
|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2as_v4|2|8|16|4|4000 / 32 (50)|3200 / 48|2 / 1000 |
| Standard_D4as_v4|4|16|32|8|8000 / 64 (100)|6400 / 96|2 / 2000 |
| Standard_D8as_v4|8|32|64|16|16000 / 128 (200)|12800 / 192|4 / 4000 |
| Standard_D16as_v4|16|64|128|32|32000 / 255 (400)|25600 / 384|8 / 8000 |
| Standard_D32as_v4|32|128|256|32|64000 / 510 (800)|51200 / 768|8 / 16000 |
| Standard_D48as_v4|48|192|384|32|96000 / 1020 (1200)|76800 / 1148|8 / 24000 |
| Standard_D64as_v4|64|256|512|32|128000 / 1020 (1600)|80000 / 1200|8 / 30000 | 
| Standard_D96as_v4|96|384|768|32|192000 / 1020 (2400)|80000 / 1200|8 / 30000 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>기타 크기

- [범용](sizes-general.md)
- [메모리 최적화](sizes-memory.md)
- [Storage에 최적화](sizes-storage.md)
- [GPU에 최적화](sizes-gpu.md)
- [고성능 컴퓨팅](sizes-hpc.md)
- [이전 세대](sizes-previous-gen.md)

## <a name="next-steps"></a>다음 단계

[ACU(Azure 컴퓨팅 단위)](acu.md)가 Azure SKU 간의 Compute 성능을 비교하는 데 어떻게 도움을 줄 수 있는지 알아봅니다.
