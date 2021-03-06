---
title: AKS(Azure Kubernetes Service)에서 OpenFaaS 사용
description: Azure Kubernetes 서비스(AKS) 클러스터에서 OpenFaaS를 배포하고 사용하여 컨테이너가 있는 서버없는 기능을 빌드하는 방법을 알아봅니다.
author: justindavies
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: juda
ms.custom: mvc
ms.openlocfilehash: 95039573c607f516755f08f1ebad8b968416ec8b
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631465"
---
# <a name="using-openfaas-on-aks"></a>AKS에서 OpenFaaS 사용

[OpenFaaS는][open-faas] 컨테이너를 사용하여 서버리스 기능을 구축하기 위한 프레임워크입니다. 오픈 소스 프로젝트로써 커뮤니티 내에서 대규모로 채택되었습니다. 이 문서에서는 AKS(Azure Kubernetes Service) 클러스터에서 OpenFaas를 설치하고 사용하는 방법을 자세히 설명합니다.

## <a name="prerequisites"></a>사전 요구 사항

이 아티클 내의 단계를 완료하기 위해 다음 항목이 필요합니다.

* Kubernetes에 대한 기본적인 이해
* AKS(Azure Kubernetes Service) 클러스터 및 개발 시스템에 구성된 AKS 자격 증명
* 개발 시스템에 설치된 Azure CLI.
* 시스템에 설치된 Git 명령줄 도구

## <a name="add-the-openfaas-helm-chart-repo"></a>OpenFaaS 투구 차트 리포지토리 추가

브라우저에서 [https://shell.azure.com](https://shell.azure.com) Azure 클라우드 셸을 열려면 이동합니다.

OpenFaaS는 모든 최신 변경 사항을 최신 상태로 유지하기 위해 자체 헬멧 차트를 유지합니다.

```console
helm repo add openfaas https://openfaas.github.io/faas-netes/
helm repo update
```

## <a name="deploy-openfaas"></a>OpenFaaS 배포

좋은 방법은 OpenFaaS 및 OpenFaaS 함수를 고유한 Kubernetes 네임스페이스에 저장하는 것입니다.

OpenFaaS 시스템 및 함수에 대한 네임스페이스 만들기:

```console
kubectl apply -f https://raw.githubusercontent.com/openfaas/faas-netes/master/namespaces.yml
```

OpenFaaS UI 포털 및 REST API에 대한 암호를 생성합니다.

```console
# generate a random password
PASSWORD=$(head -c 12 /dev/urandom | shasum| cut -d' ' -f1)

kubectl -n openfaas create secret generic basic-auth \
--from-literal=basic-auth-user=admin \
--from-literal=basic-auth-password="$PASSWORD"
```

을 통해 비밀의 값을 `echo $PASSWORD`얻을 수 있습니다.

여기서 생성되는 암호는 클라우드 로드밸슬러를 통해 인터넷에 노출되는 OpenFaaS 게이트웨이에서 기본 인증을 활성화하기 위해 헬름 차트에서 사용됩니다.

OpenFaaS에 대한 Helm 차트는 복제된 리포지토리에 포함됩니다. 이 차트를 사용하여 OpenFaaS를 AKS 클러스터에 배포합니다.

```console
helm upgrade openfaas --install openfaas/openfaas \
    --namespace openfaas  \
    --set basic_auth=true \
    --set functionNamespace=openfaas-fn \
    --set serviceType=LoadBalancer
```

출력:

```output
NAME:   openfaas
LAST DEPLOYED: Wed Feb 28 08:26:11 2018
NAMESPACE: openfaas
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                 DATA  AGE
prometheus-config    2     20s
alertmanager-config  1     20s

{snip}

NOTES:
To verify that openfaas has started, run:

  kubectl --namespace=openfaas get deployments -l "release=openfaas, app=openfaas"
```

OpenFaaS 게이트웨이에 액세스하기 위해 공용 IP 주소를 생성합니다. 이 IP 주소를 검색하려면 [kubectl get service][kubectl-get] 명령을 사용합니다. 서비스에 IP 주소가 할당될 때까지 잠깐 시간이 걸릴 수 있습니다.

```console
kubectl get service -l component=gateway --namespace openfaas
```

출력

```output
NAME               TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)          AGE
gateway            ClusterIP      10.0.156.194   <none>         8080/TCP         7m
gateway-external   LoadBalancer   10.0.28.18     52.186.64.52   8080:30800/TCP   7m
```

OpenFaaS 시스템을 테스트하려면 포트 8080의 외부 IP 주소로 이동합니다(이 예제에서 `http://52.186.64.52:8080`). 로그인하라는 메시지가 표시됩니다. 암호를 가져오려면 을 `echo $PASSWORD`입력합니다.

![OpenFaaS UI](media/container-service-serverless/openfaas.png)

마지막으로 OpenFaaS CLI를 설치합니다. 이 예제에서는 brew를 사용합니다, 추가 옵션은 [OpenFaaS CLI 설명서][open-faas-cli]를 참조하세요.

```console
brew install faas-cli
```

위에 `$OPENFAAS_URL` 있는 공용 IP로 설정합니다.

Azure CLI로 로그인:

```console
export OPENFAAS_URL=http://52.186.64.52:8080
echo -n $PASSWORD | ./faas-cli login -g $OPENFAAS_URL -u admin --password-stdin
```

## <a name="create-first-function"></a>첫 번째 함수 만들기

이제 OpenFaaS가 작동하므로 OpenFaas 포털을 사용하여 함수를 만듭니다.

**새 함수 배포**를 클릭하고 **Figlet**을 검색합니다. Figlet 함수를 선택하고 **배포**를 클릭합니다.

![Figlet](media/container-service-serverless/figlet.png)

Curl을 사용하여 함수를 호출합니다. 다음 예제의 IP 주소를 OpenFaas 게이트웨이의 IP 주소로 바꿉니다.

```console
curl -X POST http://52.186.64.52:8080/function/figlet -d "Hello Azure"
```

출력:

```output
 _   _      _ _            _
| | | | ___| | | ___      / \    _____   _ _ __ ___
| |_| |/ _ \ | |/ _ \    / _ \  |_  / | | | '__/ _ \
|  _  |  __/ | | (_) |  / ___ \  / /| |_| | | |  __/
|_| |_|\___|_|_|\___/  /_/   \_\/___|\__,_|_|  \___|

```

## <a name="create-second-function"></a>두 번째 함수 만들기

이제 두 번째 함수를 만듭니다. 이 예제는 OpenFaaS CLI를 사용하여 배포되고 사용자 지정 컨테이너 이미지 및 Cosmos DB의 검색 데이터를 포함합니다. 함수를 만들기 전에 몇 가지 항목을 구성해야 합니다.

먼저 Cosmos DB에 대한 새 리소스 그룹을 만듭니다.

```azurecli-interactive
az group create --name serverless-backing --location eastus
```

`MongoDB` 종류의 CosmosDB 인스턴스를 배포합니다. 인스턴스는 고유한 이름을 입력하고, `openfaas-cosmos`를 환경에 고유한 이름으로 업데이트합니다.

```azurecli-interactive
az cosmosdb create --resource-group serverless-backing --name openfaas-cosmos --kind MongoDB
```

Cosmos 데이터베이스 연결 문자열을 가져오고 변수에 저장합니다.

`--resource-group` 인수의 값을 리소스 그룹의 이름으로 업데이트하고 `--name` 인수를 Cosmos DB 이름으로 업데이트합니다.

```azurecli-interactive
COSMOS=$(az cosmosdb list-connection-strings \
  --resource-group serverless-backing \
  --name openfaas-cosmos \
  --query connectionStrings[0].connectionString \
  --output tsv)
```

이제 테스트 데이터로 Cosmos DB를 채웁니다. 파일 `plans.json`을 만들고 다음 JSON에 복사합니다.

```json
{
    "name" : "two_person",
    "friendlyName" : "Two Person Plan",
    "portionSize" : "1-2 Person",
    "mealsPerWeek" : "3 Unique meals per week",
    "price" : 72,
    "description" : "Our basic plan, delivering 3 meals per week, which will feed 1-2 people.",
    "__v" : 0
}
```

*mongoimport* 도구를 사용하여 데이터가 포함된 CosmosDB 인스턴스를 로드합니다.

필요한 경우 MongoDB 도구를 설치합니다. 다음 예제에서는 brew를 사용하여 이러한 도구를 설치합니다. 기타 옵션은 [MongoDB 설명서][install-mongo]를 참조하세요.

```console
brew install mongodb
```

데이터베이스에 데이터를 로드합니다.

```console
mongoimport --uri=$COSMOS -c plans < plans.json
```

출력:

```output
2018-02-19T14:42:14.313+0000    connected to: localhost
2018-02-19T14:42:14.918+0000    imported 1 document
```

다음 명령을 실행하여 함수를 만듭니다. `-g` 인수 값을 OpenFaaS 게이트웨이 주소로 업데이트합니다.

```console
faas-cli deploy -g http://52.186.64.52:8080 --image=shanepeckham/openfaascosmos --name=cosmos-query --env=NODE_ENV=$COSMOS
```

해당 기능이 배포되면 함수에 새로 만든 OpenFaaS 엔드포인트가 표시됩니다.

```output
Deployed. 202 Accepted.
URL: http://52.186.64.52:8080/function/cosmos-query
```

curl을 사용하여 함수를 테스트합니다. OpenFaaS 게이트웨이 주소로 IP 주소를 업데이트합니다.

```console
curl -s http://52.186.64.52:8080/function/cosmos-query
```

출력:

```json
[{"ID":"","Name":"two_person","FriendlyName":"","PortionSize":"","MealsPerWeek":"","Price":72,"Description":"Our basic plan, delivering 3 meals per week, which will feed 1-2 people."}]
```

OpenFaaS UI 내에서 함수를 테스트할 수도 있습니다.

![대체 텍스트](media/container-service-serverless/OpenFaaSUI.png)

## <a name="next-steps"></a>다음 단계

OpenFaaS 워크샵에서는 자체 GitHub 봇 생성 방법, 비밀 소비, 메트릭 보기 및 자동 크기 조정과 같은 주제를 다루는 실습 랩을 통해 계속 학습할 수 있습니다.

<!-- LINKS - external -->
[install-mongo]: https://docs.mongodb.com/manual/installation/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[open-faas]: https://www.openfaas.com/
[open-faas-cli]: https://github.com/openfaas/faas-cli
[openfaas-workshop]: https://github.com/openfaas/workshop
