---
title: 음성 장치 SDK 문제 해결 - 음성 서비스
titleSuffix: Azure Cognitive Services
description: 이 문서에서는 음성 장치 SDK를 사용할 때 발생할 수 있는 문제를 해결하는 데 도움이 되는 정보를 제공합니다.
services: cognitive-services
author: mswellsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: wellsi
ms.openlocfilehash: c590e0972de289a36890a75b220eddbded701ea8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74815569"
---
# <a name="troubleshoot-the-speech-devices-sdk"></a>Speech Devices SDK 문제 해결

이 문서에서는 음성 장치 SDK를 사용할 때 발생할 수 있는 문제를 해결하는 데 도움이 되는 정보를 제공합니다.

## <a name="certificate-failures"></a>인증서 오류

Speech 서비스를 사용할 때 인증서 오류가 발생하는 경우 장치에 정확한 날짜와 시간이 있는지 확인합니다.

1. **설정**으로 이동합니다. **시스템** 아래에서 **날짜 및 시간**을 선택합니다.

    ![[설정] 아래에서 [날짜 및 시간] 선택](media/speech-devices-sdk/qsg-12.png)

1. **자동 날짜 및 시간** 옵션을 선택한 채로 둡니다. **표준 시간대 선택** 아래에서 현재 표준 시간대를 선택합니다.

    ![날짜 및 표준 시간대 선택](media/speech-devices-sdk/qsg-13.png)

    개발 키트의 시간이 PC의 시간과 일치하면 개발 키트가 인터넷에 연결됩니다.

## <a name="next-steps"></a>다음 단계

* [릴리스 정보 검토](devices-sdk-release-notes.md)
