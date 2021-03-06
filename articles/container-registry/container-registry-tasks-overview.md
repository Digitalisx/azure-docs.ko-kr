---
title: ACR 작업 개요
description: 클라우드에서 안전하고 자동화된 컨테이너 이미지 빌드, 관리 및 패치를 제공하는 Azure 컨테이너 레지스트리의 기능 모음인 ACR 작업에 대한 소개입니다.
ms.topic: article
ms.date: 01/22/2020
ms.openlocfilehash: 4fda57c1d7c866f2e6f72b04d75e53f91e995baf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79087280"
---
# <a name="automate-container-image-builds-and-maintenance-with-acr-tasks"></a>ACR 작업을 통해 컨테이너 이미지 빌드 및 유지 관리 자동화

컨테이너는 새 수준의 가상화를 제공하여 인프라 및 운영 요구 사항에서 애플리케이션 및 개발자 종속성을 격리합니다. 그러나 남은 것은 이 응용 프로그램 가상화를 컨테이너 수명 주기 동안 관리하고 패치하는 방법을 해결해야 한다는 것입니다.

## <a name="what-is-acr-tasks"></a>ACR 작업이란?

**ACR 작업**은 Azure Container Registry 내의 기능 모음입니다. Linux, Windows 및 ARM을 비롯한 [플랫폼을](#image-platforms) 위한 클라우드 기반 컨테이너 이미지 빌드를 제공하며 Docker 컨테이너에 대한 [OS 및 프레임워크 패치를](#automate-os-and-framework-patching) 자동화할 수 있습니다. ACR 작업은 주문형 컨테이너 이미지 빌드를 사용하여 "내부 루프" 개발 주기를 클라우드로 확장할 뿐만 아니라 소스 코드 업데이트, 컨테이너의 기본 이미지 업데이트 또는 타이머에 의해 트리거되는 자동화된 빌드를 사용할 수 있습니다. 예를 들어 기본 이미지 업데이트 트리거를 사용하면 OS 및 응용 프로그램 프레임워크 패치 워크플로우를 자동화하여 변경할 수 없는 컨테이너의 원칙을 준수하면서 안전한 환경을 유지할 수 있습니다.

## <a name="task-scenarios"></a>작업 시나리오

ACR 작업은 컨테이너 이미지 및 기타 아티팩트를 빌드하고 유지 관리하는 몇 가지 시나리오를 지원합니다. 자세한 내용은 이 문서의 다음 섹션을 참조하십시오.

* **[빠른 작업](#quick-task)** - 로컬 Docker Engine 설치없이 Azure에서 주문형 컨테이너 레지스트리에 단일 컨테이너 이미지를 빌드하고 푸시합니다. 클라우드의 `docker build`, `docker push`를 생각하면 됩니다.
* **자동으로 트리거된 작업** - 하나 이상의 *트리거를* 사용하여 이미지를 빌드할 수 있습니다.
  * **[소스 코드 업데이트시 트리거](#trigger-task-on-source-code-update)** 
  * **[기본 이미지 업데이트시 트리거](#automate-os-and-framework-patching)** 
  * **[일정에 따라 트리거](#schedule-a-task)** 
* **[다중 단계 작업](#multi-step-tasks)** - 다중 단계, 다중 컨테이너 기반 워크플로우를 통해 ACR 작업의 단일 이미지 빌드 및 푸시 기능을 확장합니다. 

각 ACR 작업에는 컨테이너 이미지 또는 기타 아티팩트를 빌드하는 데 사용되는 소스 파일 집합의 위치와 같은 연결된 [소스 코드 컨텍스트가](#context-locations) 있습니다. 예제 컨텍스트에는 Git 리포지토리 또는 로컬 파일 시스템이 포함됩니다.

작업도 [실행 변수를](container-registry-tasks-reference-yaml.md#run-variables)활용할 수 있으므로 작업 정의를 재사용하고 이미지 및 아티팩트에 대한 태그를 표준화할 수 있습니다.

## <a name="quick-task"></a>빠른 작업

내부 루프 개발 주기는 원본 제어에 커밋하기 전에 애플리케이션 코드 작성, 빌드 및 테스트를 반복하는 프로세스로, 실제로 컨테이너 수명 주기 관리의 시작입니다.

첫 번째 코드 줄을 커밋하기 전에, ACR 작업의 빠른 [작업](container-registry-tutorial-quick-task.md) 기능은 컨테이너 이미지 빌드를 Azure에 오프로드하여 통합 개발 환경을 제공할 수 있습니다. 빠른 작업을 사용하면 코드를 커밋하기 전에 자동화된 빌드 정의를 확인하고 잠재적인 문제점을 발견할 수 있습니다.

친숙한 `docker build` 형식을 사용하여 Azure CLI의 [az acr build][az-acr-build] 명령에서 [컨텍스트](#context-locations)(빌드할 파일 집합)를 사용하고, 이를 ACR 작업으로 보내고, 기본적으로 완료되면 빌드된 이미지를 해당 레지스트리에 푸시합니다.

소개는 Azure 컨테이너 레지스트리에서 [컨테이너 이미지를 빌드하고 실행하는](container-registry-quickstart-task-cli.md) 빠른 시작을 참조하십시오.  

ACR 작업은 기본 컨테이너 수명 주기로 설계되었습니다. 예를 들어, ACR 작업을 CI/CD 솔루션에 통합합니다. CI/CD 솔루션은 [서비스 주체][az-login-service-principal]로 [az login][az-login]을 실행한 다음, [az acr build][az-acr-build] 명령으로 이미지 빌드를 시작할 수 있습니다.

첫 번째 ACR 작업 자습서인 [Azure Container Registry 작업을 사용하여 클라우드에서 컨테이너 이미지 빌드](container-registry-tutorial-quick-task.md)에서 빠른 작업을 사용하는 방법을 알아보세요.

> [!TIP]
> Dockerfile 없이 소스 코드에서 직접 이미지를 빌드하고 푸시하려는 경우 Azure 컨테이너 레지스트리는 [az acr pack 빌드][az-acr-pack-build] 명령(미리 보기)을 제공합니다. 이 도구는 [Cloud 네이티브 빌드팩을](https://buildpacks.io/)사용하여 응용 프로그램 소스 코드에서 이미지를 빌드하고 푸시합니다.

## <a name="trigger-task-on-source-code-update"></a>소스 코드 업데이트에 대한 트리거 작업

코드가 커밋되거나 끌어오기 요청이 GitHub 또는 Azure DevOps의 공용 또는 개인 Git 리포지토리로 수행되거나 업데이트될 때 컨테이너 이미지 빌드 또는 다단계 작업을 트리거합니다. 예를 들어 Git 리포지토리와 선택적으로 분기 및 Dockerfile을 지정하여 Azure CLI 명령 [az acr 태스크를][az-acr-task-create] 사용하여 빌드 작업을 구성합니다. 팀이 리포지토리에서 코드를 업데이트하면 ACR 작업에서 만든 웹후크가 리포지토리에 정의된 컨테이너 이미지빌드를 트리거합니다. 

ACR 작업은 Git 리포지토리를 작업의 컨텍스트로 설정할 때 다음 트리거를 지원합니다.

| 트리거 | 기본적으로 사용하도록 설정됨 |
| ------- | ------------------ |
| Commit | yes |
| 끌어오기 요청 | 예 |

소스 코드 업데이트 트리거를 구성하려면 공용 또는 개인 GitHub 또는 Azure DevOps 리포지토리에서 웹후크를 설정하기 위해 PAT(개인 액세스 토큰)을 작업에 제공해야 합니다.

> [!NOTE]
> 현재 ACR 작업은 GitHub Enterprise 리포지토리에서 커밋 또는 끌어오기 요청 트리거를 지원하지 않습니다.

두 번째 ACR 작업 자습서인 [Azure Container Registry 작업을 사용하여 컨테이너 이미지 빌드 자동화](container-registry-tutorial-build-task.md)에서 소스 코드 커밋 시 빌드를 트리거하는 방법을 알아보세요.

## <a name="automate-os-and-framework-patching"></a>OS 및 프레임워크 패치 자동화

컨테이너 빌드 워크플로를 진정으로 향상시키는 ACR 작업의 힘은 *기본 이미지에*대한 업데이트를 검색하는 기능에서 비롯됩니다. 대부분의 컨테이너 이미지의 특징인 기본 이미지는 하나 이상의 응용 프로그램 이미지의 기반이 되는 상위 이미지입니다. 기본 이미지에는 일반적으로 운영 체제및 응용 프로그램 프레임워크가 포함됩니다. 

응용 프로그램 이미지를 빌드할 때 기본 이미지에 대한 종속성을 추적하기 위해 ACR 작업을 설정할 수 있습니다. 업데이트된 기본 이미지가 레지스트리로 푸시되거나 Docker Hub와 같은 공용 리포지토리에서 기본 이미지가 업데이트되면 ACR 작업은 이를 기반으로 모든 응용 프로그램 이미지를 자동으로 빌드할 수 있습니다.
이 자동 검색 및 다시 빌드를 통해 ACR 작업에서 업데이트된 기본 이미지를 참조하는 각 애플리케이션 이미지를 빠짐없이 수동으로 추적하고 업데이트하는 데 필요한 시간과 노력을 절약할 수 있습니다.

ACR 작업에 대한 [기본 이미지 업데이트 트리거에](container-registry-tasks-base-images.md) 대해 자세히 알아보세요. 기본 이미지가 Azure 컨테이너 레지스트리에서 기본 이미지가 업데이트될 때 컨테이너 [이미지 빌드를 자동화하는](container-registry-tutorial-base-image-update.md) 자습서의 컨테이너 레지스트리로 푸시될 때 이미지 빌드를 트리거하는 방법을 알아봅니다.

## <a name="schedule-a-task"></a>작업 예약

선택적으로 작업을 만들거나 업데이트할 때 하나 이상의 타이머 트리거를 설정하여 작업을 *예약합니다.* 작업 예약은 정의된 일정에 따라 컨테이너 워크로드를 실행하거나 레지스트리에 정기적으로 푸시된 이미지에 대한 유지 관리 작업 또는 테스트를 실행하는 데 유용합니다. 자세한 내용은 [정의된 일정에 따라 ACR 작업 실행을](container-registry-tasks-scheduled.md)참조하십시오.

## <a name="multi-step-tasks"></a>다단계 작업

다중 단계 작업은 클라우드에서 컨테이너 이미지를 빌드, 테스트 및 패치하기 위한 단계 기반 작업 정의 및 실행을 지원합니다. [YAML 파일에](container-registry-tasks-reference-yaml.md) 정의된 작업 단계는 컨테이너 이미지 또는 기타 아티팩트에 대한 개별 빌드 및 푸시 작업을 지정합니다. 해당 실행 환경으로 컨테이너를 사용하여 각 단계로 하나 이상의 컨테이너 실행을 정의할 수도 있습니다.

예를 들어, 다음을 자동화하는 다단계 작업을 만들 수 있습니다.

1. 웹 애플리케이션 이미지 빌드
1. 웹 애플리케이션 컨테이너 실행
1. 웹 애플리케이션 테스트 이미지 빌드
1. 실행 중인 응용 프로그램 컨테이너에 대한 테스트를 수행하는 웹 응용 프로그램 테스트 컨테이너를 실행합니다.
1. 테스트에 통과하면 Helm 차트 보관 패키지 빌드
1. 새 Helm 차트 보관 패키지를 사용하여 `helm upgrade` 수행

다단계 작업을 사용하면 단계 간 종속성 지원을 통해 이미지 빌드, 실행 및 테스트를 보다 복잡한 단계로 분할할 수 있습니다. ACR 작업의 다단계 작업을 통해 이미지 빌드, 테스트, OS 및 프레임워크 패치 워크플로를 보다 세부적으로 제어할 수 있습니다.

[ACR 작업에서 다단계 빌드, 테스트 및 패치 작업 실행](container-registry-tasks-multi-step.md)에서 다단계 작업에 대해 알아보세요.

## <a name="context-locations"></a>컨텍스트 위치

다음 표에서는 ACR 작업에 지원되는 컨텍스트 위치의 몇 가지 예를 보여 줍니다.

| 컨텍스트 위치 | Description | 예제 |
| ---------------- | ----------- | ------- |
| 로컬 파일 시스템 | 로컬 파일 시스템의 디렉터리 내에 있는 파일. | `/home/user/projects/myapp` |
| GitHub 마스터 분기 | 공용 또는 개인 GitHub 리포지토리의 마스터(또는 기타 기본) 분기 내의 파일입니다.  | `https://github.com/gituser/myapp-repo.git` |
| GitHub 분기 | 공용 또는 개인 GitHub 리포지토리의 특정 분기입니다.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| GitHub 하위 폴더 | 공용 또는 개인 GitHub 리포지토리의 하위 폴더 내의 파일입니다. 분기 및 하위 폴더 사양의 조합을 보여 주면 됩니다. | `https://github.com/gituser/myapp-repo.git#mybranch:myfolder` |
| GitHub 커밋 | 공용 또는 개인 GitHub 리포지토리의 특정 커밋입니다. SHA(커밋 해시) 및 하위 폴더 사양의 조합을 예로 들 수 있습니다. | `https://github.com/gituser/myapp-repo.git#git-commit-hash:myfolder` |
| Azure DevOps 하위 폴더 | 공용 또는 개인 Azure 리포지토리의 하위 폴더 내의 파일입니다. 예를 들어 분기 및 하위 폴더 사양의 조합을 보여 주다. | `https://dev.azure.com/user/myproject/_git/myapp-repo#mybranch:myfolder` |
| 원격 Tarball | 원격 웹 서버의 압축된 아카이브에 있는 파일. | `http://remoteserver/myapp.tar.gz` |

> [!NOTE]
> 개인 Git 리포지토리를 작업에 대한 컨텍스트로 사용하는 경우 PAT(개인 액세스 토큰)를 제공해야 합니다.

## <a name="image-platforms"></a>이미지 플랫폼

기본적으로 ACR 작업은 Linux OS 및 amd64 아키텍처에 대한 이미지를 빌드합니다. 태그를 `--platform` 지정하여 다른 아키텍처에 대한 Windows 이미지 또는 Linux 이미지를 빌드합니다. OS 및 선택적으로 지원되는 아키텍처를 OS/아키텍처 형식(예: `--platform Linux/arm`)로 지정합니다. ARM 아키텍처의 경우 선택적으로 OS/아키텍처/변형 형식(예: `--platform Linux/arm64/v8`:)의 변형을 지정합니다.

| OS | Architecture|
| --- | ------- | 
| Linux | amd64<br/>arm<br/>암64<br/>386 |
| Windows | amd64 |

## <a name="view-task-output"></a>태스크 출력 보기

각 작업 실행은 작업 단계가 성공적으로 실행되었는지 여부를 확인하기 위해 검사할 수 있는 로그 출력을 생성합니다. 작업을 수동으로 트리거하면 작업 실행에 대한 로그 출력이 콘솔로 스트리밍되고 나중에 검색할 수 있도록 저장됩니다. 소스 코드 커밋 또는 기본 이미지 업데이트와 같은 작업이 자동으로 트리거되면 작업 로그만 저장됩니다. Azure 포털에서 실행 로그를 보거나 [az acr 작업 로그](/cli/azure/acr/task#az-acr-task-logs) 명령을 사용합니다.

[작업 로그 보기 및 관리에](container-registry-tasks-logs.md)대해 자세히 알아보십시오.

## <a name="next-steps"></a>다음 단계

클라우드에서 컨테이너 이미지 빌드 및 유지 관리를 자동화할 준비가 되면 [ACR 작업 자습서 시리즈를](container-registry-tutorial-quick-task.md)확인하십시오.

선택적으로 Azure 컨테이너 레지스트리와 작동할 [Visual Studio Code용 Docker 확장](https://code.visualstudio.com/docs/azure/docker)과 [Azure 계정](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) 확장을 설정합니다. Visual Studio Code 내에서 Azure 컨테이너 레지스트리에 이미지를 밀어넣고 끌어오거나, ACR Tasks를 실행합니다.

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-pack-build]: /cli/azure/acr/pack#az-acr-pack-build
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
