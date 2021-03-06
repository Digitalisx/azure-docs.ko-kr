---
title: 컨테이너 인스턴스 로그 & 이벤트 받기
description: Azure 컨테이너 인스턴스에서 컨테이너 로그 및 이벤트를 검색하여 컨테이너 문제를 해결하는 방법을 알아봅니다.
ms.topic: article
ms.date: 12/30/2019
ms.custom: mvc
ms.openlocfilehash: 0991b9cb1f99606910dbdf2c87b111f67da6da7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78249991"
---
# <a name="retrieve-container-logs-and-events-in-azure-container-instances"></a>Azure Container Instances에서 컨테이너 로그 및 이벤트 검색

Azure 컨테이너 인스턴스에서 잘못 행동하는 컨테이너가 있는 경우 az [컨테이너 로그를][az-container-logs]사용하는 로그를 보고 az [컨테이너 연결로][az-container-attach]표준 아웃 및 표준 오류를 스트리밍합니다. 또한 Azure 포털에서 컨테이너 인스턴스에 대한 로그 및 이벤트를 보거나 컨테이너 그룹에 대한 로그 및 이벤트 데이터를 [Azure Monitor 로그에](container-instances-log-analytics.md)보낼 수도 있습니다.

## <a name="view-logs"></a>로그 보기

컨테이너 내에서 애플리케이션 코드에서 로그를 보려면 [az 컨테이너 로그][az-container-logs] 명령을 사용할 수 있습니다.

다음은 명령줄 재정의를 사용하여 잘못된 URL을 제공한 후 [컨테이너 인스턴스에서 명령줄 설정의](container-instances-start-command.md#azure-cli-example)예제 작업 기반 컨테이너의 로그 출력입니다.

```azurecli
az container logs --resource-group myResourceGroup --name mycontainer
```

```output
Traceback (most recent call last):
  File "wordcount.py", line 11, in <module>
    urllib.request.urlretrieve (sys.argv[1], "foo.txt")
  File "/usr/local/lib/python3.6/urllib/request.py", line 248, in urlretrieve
    with contextlib.closing(urlopen(url, data)) as fp:
  File "/usr/local/lib/python3.6/urllib/request.py", line 223, in urlopen
    return opener.open(url, data, timeout)
  File "/usr/local/lib/python3.6/urllib/request.py", line 532, in open
    response = meth(req, response)
  File "/usr/local/lib/python3.6/urllib/request.py", line 642, in http_response
    'http', request, response, code, msg, hdrs)
  File "/usr/local/lib/python3.6/urllib/request.py", line 570, in error
    return self._call_chain(*args)
  File "/usr/local/lib/python3.6/urllib/request.py", line 504, in _call_chain
    result = func(*args)
  File "/usr/local/lib/python3.6/urllib/request.py", line 650, in http_error_default
    raise HTTPError(req.full_url, code, msg, hdrs, fp)
urllib.error.HTTPError: HTTP Error 404: Not Found
```

## <a name="attach-output-streams"></a>출력 스트림 연결

[az container attach][az-container-attach] 명령은 컨테이너를 시작하는 동안 진단 정보를 제공합니다. 컨테이너가 시작되면 로컬 콘솔에 STDOUT 및 STDERR을 스트리밍합니다.

예를 들어 처리할 큰 텍스트 파일의 유효한 URL을 제공한 후 [컨테이너 인스턴스에서 명령줄 설정에서](container-instances-start-command.md#azure-cli-example)작업 기반 컨테이너에서 출력됩니다.

```azurecli
az container attach --resource-group myResourceGroup --name mycontainer
```

```output
Container 'mycontainer' is in state 'Unknown'...
Container 'mycontainer' is in state 'Waiting'...
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2019-03-21 19:42:39+00:00) pulling image "mcr.microsoft.com/azuredocs/aci-wordcount:latest"
Container 'mycontainer1' is in state 'Running'...
(count: 1) (last timestamp: 2019-03-21 19:42:39+00:00) pulling image "mcr.microsoft.com/azuredocs/aci-wordcount:latest"
(count: 1) (last timestamp: 2019-03-21 19:42:52+00:00) Successfully pulled image "mcr.microsoft.com/azuredocs/aci-wordcount:latest"
(count: 1) (last timestamp: 2019-03-21 19:42:55+00:00) Created container
(count: 1) (last timestamp: 2019-03-21 19:42:55+00:00) Started container

Start streaming logs:
[('the', 22979),
 ('I', 20003),
 ('and', 18373),
 ('to', 15651),
 ('of', 15558),
 ('a', 12500),
 ('you', 11818),
 ('my', 10651),
 ('in', 9707),
 ('is', 8195)]
```

## <a name="get-diagnostic-events"></a>진단 이벤트 가져오기

컨테이너가 성공적으로 배포되지 않으면 Azure 컨테이너 인스턴스 리소스 공급자가 제공한 진단 정보를 검토합니다. 컨테이너에 대한 이벤트를 보려면 [az container show][az-container-show] 명령을 실행합니다.

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

출력에는 배포 이벤트와 함께 컨테이너의 핵심 속성이 포함되어 있습니다(여기에서 잘려서 표시됨).

```JSON
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "mcr.microsoft.com/azuredocs/aci-helloworld",
      ...
        "events": [
          {
            "count": 1,
            "firstTimestamp": "2019-03-21T19:46:22+00:00",
            "lastTimestamp": "2019-03-21T19:46:22+00:00",
            "message": "pulling image \"mcr.microsoft.com/azuredocs/aci-helloworld\"",
            "name": "Pulling",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2019-03-21T19:46:28+00:00",
            "lastTimestamp": "2019-03-21T19:46:28+00:00",
            "message": "Successfully pulled image \"mcr.microsoft.com/azuredocs/aci-helloworld\"",
            "name": "Pulled",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2019-03-21T19:46:31+00:00",
            "lastTimestamp": "2019-03-21T19:46:31+00:00",
            "message": "Created container",
            "name": "Created",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2019-03-21T19:46:31+00:00",
            "lastTimestamp": "2019-03-21T19:46:31+00:00",
            "message": "Started container",
            "name": "Started",
            "type": "Normal"
          }
        ],
        "previousState": null,
        "restartCount": 0
      },
      "name": "mycontainer",
      "ports": [
        {
          "port": 80,
          "protocol": null
        }
      ],
      ...
    }
  ],
  ...
}
```
## <a name="next-steps"></a>다음 단계
Azure Container Instances의 [컨테이너 및 배포 문제를 해결](container-instances-troubleshooting.md)하는 방법을 알아봅니다.

컨테이너 그룹에 대한 로그 및 이벤트 데이터를 [Azure Monitor 로그에](container-instances-log-analytics.md)보내는 방법에 대해 알아봅니다.

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show