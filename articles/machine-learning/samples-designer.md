---
title: 예제 디자이너 파이프라인
titleSuffix: Azure Machine Learning
description: Azure Machine Learning 디자이너에서 샘플을 사용하여 기계 학습 파이프라인을 신속하게 시작합니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 03/29/2020
ms.openlocfilehash: f9a8b0a4c51024d91e517db2f6ae10a4dba62384
ms.sourcegitcommit: 0553a8b2f255184d544ab231b231f45caf7bbbb0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80389344"
---
# <a name="designer-sample-pipelines"></a>디자이너 샘플 파이프라인

Azure Machine Learning 디자이너에서 기본 제공되는 예제를 사용하여 고유한 기계 학습 파이프라인 빌드를 신속하게 시작합니다. Azure Machine Learning 디자이너 [GitHub 리포지토리](https://github.com/Azure/MachineLearningDesigner)는 몇 가지 일반적인 기계 학습 시나리오를 이해하는 데 도움이 되는 자세한 설명서를 포함합니다.

## <a name="prerequisites"></a>사전 요구 사항

* Azure 구독 Azure 구독이 아직 없는 경우 [체험 계정](https://aka.ms/AMLFree)을 만듭니다.
* Enterprise SKU를 사용하는 Azure Machine Learning 작업 영역


## <a name="how-to-use-sample-pipelines"></a>샘플 파이프라인을 사용하는 방법

디자이너는 샘플 파이프라인의 복사본을 스튜디오 작업 영역에 저장합니다. 파이프라인을 편집하여 사용자의 요구에 맞게 조정하고 사용자의 파이프라인으로 저장할 수 있습니다. 이를 시작점으로 사용하여 프로젝트를 신속하게 시작합니다.

### <a name="open-a-sample-pipeline"></a>샘플 파이프라인 열기

1. <a href="https://ml.azure.com?tabs=jre" target="_blank">ml.azure.com</a>에 로그인하고, 사용하려는 작업 영역을 선택합니다.

1. **디자이너**를 선택합니다.

1. **새 파이프라인** 섹션에서 샘플 파이프라인을 선택합니다.

    샘플의 전체 목록을 보려면 **더 많은 샘플 표시**를 선택합니다.

### <a name="submit-a-pipeline-run"></a>파이프라인 실행 제출

파이프라인을 실행하려면 먼저 파이프라인을 실행할 기본 컴퓨팅 대상을 설정해야 합니다.

1. 캔버스 오른쪽에 있는 **설정** 창에서 **컴퓨팅 대상 선택**을 선택합니다.

1. 표시되는 대화 상자에서 기존 컴퓨팅 대상을 선택하거나 새로 만듭니다. **저장**을 선택합니다.

1. 캔버스 맨 위에 있는 **제출**을 선택하여 파이프라인 실행을 제출합니다.

샘플 파이프라인 및 컴퓨팅 설정에 따라 실행을 완료하는 데 다소 시간이 걸릴 수 있습니다. 기본 컴퓨팅 설정의 최소 노드 크기는 0입니다. 즉, 디자이너가 유휴 상태가 된 후에 리소스를 할당해야 합니다. 컴퓨팅 리소스가 이미 할당되었기 때문에 반복되는 파이프라인 실행은 시간이 덜 걸립니다. 또한 디자이너는 각 모듈에 대해 캐시된 결과를 사용하여 효율성을 더욱 향상시킵니다.


### <a name="review-the-results"></a>결과 검토

파이프라인 실행이 완료되면 파이프라인을 검토하고 각 모듈의 출력을 확인하여 자세히 알아볼 수 있습니다.

다음 단계를 사용하여 모듈 출력을 봅니다.

1. 캔버스에서 모듈을 선택합니다.

1. 캔버스 오른쪽에 있는 모듈 세부 정보 창에서 **출력 + 로그**를 선택합니다. 그래프 아이콘 ![시각화 아이콘](./media/tutorial-designer-automobile-price-train-score/visualize-icon.png)을 선택하여 각 모듈의 결과를 봅니다. 

가장 일반적인 기계 학습 시나리오 중 일부에 대한 샘플을 시작점으로 사용합니다.

## <a name="regression-samples"></a>회귀 샘플

기본 제공 회귀 샘플에 대해 자세히 알아보세요.

| 샘플 제목 | Description | 
| --- | --- |
| [샘플 1: 회귀 - 자동차 가격 예측(기본)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-basic.md) | 선형 회귀를 사용하여 자동차 가격을 예측합니다. |
| [샘플 2: 회귀 - 자동차 가격 예측(고급)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-compare-algorithms.md) | 의사 결정 포리스트와 향상된 의사 결정 트리 회귀 변수를 사용하여 자동차 가격을 예측합니다. 모델을 비교하여 가장 적합한 알고리즘을 찾습니다.

## <a name="classification-samples"></a>분류 샘플

기본 제공 분류 샘플에 대해 자세히 알아보세요. 샘플을 열고 모듈 주석을 대신 확인하여 설명서 링크 없이 샘플에 대해 자세히 알아볼 수 있습니다.

| 샘플 제목 | Description | 
| --- | --- |
| [샘플 3: 기능 선택이 포함된 이진 분류 - 수입 예측](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-predict-income.md) | 2클래스 향상된 의사 결정 트리를 사용하여 수입을 높음 또는 낮음으로 예측합니다. 피어슨 상관 관계를 사용하여 기능을 선택합니다.
| [샘플 4: 사용자 지정 Python 스크립트를 사용하는 이진 분류 - 신용 위험 예측](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-credit-risk-cost-sensitive.md) | 신용 애플리케이션을 위험 수준 높음 또는 위험 수준 낮음으로 분류합니다. Python 스크립트 실행 모듈을 사용하여 데이터에 가중치를 부여합니다.
| [샘플 5: 이진 분류 - 고객 관계 예측](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-churn.md) | 2클래스 향상된 의사 결정 트리를 사용하여 고객 이탈을 예측합니다. SMOTE를 사용하여 편향 데이터를 샘플링합니다.
| [샘플 7: 텍스트 분류 - Wikipedia SP 500 데이터 세트](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-text-classification.md) | 다중 클래스 로지스틱 회귀를 사용하여 Wikipedia 문서의 회사 유형을 분류합니다. |
| 샘플 12: 다중 클래스 분류 - 문자 인식 | 작성된 문자를 분류하는 이진 분류자의 앙상블을 만듭니다. |

## <a name="recommender-samples"></a>추천 샘플

기본 제공 추천 샘플에 대해 자세히 알아보세요. 샘플을 열고 모듈 주석을 대신 확인하여 설명서 링크 없이 샘플에 대해 자세히 알아볼 수 있습니다.

| 샘플 제목 | Description | 
| --- | --- |
| 샘플 10: 권장 사항 - 영화 등급 트윗 | 영화 제목 및 등급에서 영화 추천 엔진을 빌드합니다. |

## <a name="utility-samples"></a>유틸리티 샘플

기계 학습 유틸리티 및 기능을 보여 주는 샘플에 대해 자세히 알아보세요. 샘플을 열고 모듈 주석을 대신 확인하여 설명서 링크 없이 샘플에 대해 자세히 알아볼 수 있습니다.

| 샘플 제목 | Description | 
| --- | --- |
| [샘플 6: 사용자 지정 R 스크립트 사용 - 비행 지연 예측](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-flight-delay.md) |
| 샘플 8: 이진 분류자 교차 유효성 검사 - 성인 수입 예측 | 교차 유효성 검사를 사용하여 성인 수입에 대한 이진 분류자를 빌드합니다.
| 샘플 9: 순열 기능 중요도 | 순열 기능 중요도를 사용하여 테스트 데이터 세트의 중요도 점수를 계산합니다. 
| 샘플 11: 이진 분류자 매개 변수 튜닝 - 성인 수입 예측 | 모델 하이퍼 매개 변수 튜닝을 사용하여 이진 분류자를 빌드하기 위한 최적의 하이퍼 매개 변수를 찾습니다. |

## <a name="clean-up-resources"></a>리소스 정리

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>다음 단계

다음을 사용하여 기계 학습 모델을 빌드하고 배포하는 방법을 알아봅니다. [자습서: 디자이너를 사용하여 자동차 가격 예측](tutorial-designer-automobile-price-train-score.md)을 참조하세요.
