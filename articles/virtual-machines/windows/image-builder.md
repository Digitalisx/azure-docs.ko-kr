---
title: Azure 이미지 빌더를 사용하여 Windows VM 만들기(미리 보기)
description: Azure 이미지 빌더를 사용하여 Windows VM을 만듭니다.
author: cynthn
ms.author: cynthn
ms.date: 07/31/2019
ms.topic: article
ms.service: virtual-machines-windows
manager: gwallace
ms.openlocfilehash: e82d82dac833f7455e3d83d7e11c0c57c4eea816
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80238798"
---
# <a name="preview-create-a-windows-vm-with-azure-image-builder"></a>미리 보기: Azure 이미지 빌더를 사용하여 Windows VM 만들기

이 문서에서는 Azure VM 이미지 빌더를 사용하여 사용자 지정된 Windows 이미지를 만드는 방법을 보여 주시고 있습니다. 이 문서의 예제에서는 이미지를 사용자 지정하기 위해 [사용자 지정자를](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#properties-customize) 사용합니다.
- PowerShell (ScriptUri) - 다운로드 및 [PowerShell 스크립트를](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/testPsScript.ps1)실행합니다.
- Windows 다시 시작 - VM을 다시 시작합니다.
- PowerShell(인라인) - 특정 명령을 실행합니다. 이 예제에서는 을 사용하여 `mkdir c:\\buildActions`VM에서 디렉터리를 만듭니다.
- 파일 - GitHub에서 VM에 파일을 복사합니다. 이 예제는 VM에서 [index.md](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) 복사본입니다. `c:\buildArtifacts\index.html`

`buildTimeoutInMinutes`을 지정할 수도 있습니다. 기본값은 240분이며 빌드 시간을 늘려 빌드를 더 오래 실행할 수 있도록 할 수 있습니다.

샘플 .json 템플릿을 사용하여 이미지를 구성합니다. 우리가 사용하는 .json 파일은 여기 : [helloImageTemplateWin.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json). 


> [!IMPORTANT]
> Azure 이미지 빌더는 현재 공개 미리 보기 상태입니다.
> 이 미리 보기 버전은 서비스 수준 계약 없이 제공되며 프로덕션 워크로드에는 사용하지 않는 것이 좋습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.


## <a name="register-the-features"></a>피처 등록

미리 보기 중에 Azure 이미지 빌더를 사용하려면 새 기능을 등록해야 합니다.

```azurecli-interactive
az feature register --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview
```

피처 등록 상태를 확인합니다.

```azurecli-interactive
az feature show --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview | grep state
```

등록을 확인하십시오.

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState

az provider show -n Microsoft.Storage | grep registrationState
```

등록을 하지 않는 경우 다음을 실행합니다.

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages

az provider register -n Microsoft.Storage
```

## <a name="set-variables"></a>변수 설정

일부 정보를 반복적으로 사용하므로 해당 정보를 저장하는 몇 가지 변수를 만듭니다.


```azurecli-interactive
# Resource group name - we are using myImageBuilderRG in this example
imageResourceGroup=myWinImgBuilderRG
# Region location 
location=WestUS2
# Name for the image 
imageName=myWinBuilderImage
# Run output name
runOutputName=aibWindows
# name of the image to be created
imageName=aibWinImage
```

구독 ID에 대한 변수를 만듭니다. 을 사용하여 `az account show | grep id`이 것을 얻을 수 있습니다.

```azurecli-interactive
subscriptionID=<Your subscription ID>
```
## <a name="create-a-resource-group"></a>리소스 그룹 만들기
이 리소스 그룹은 이미지 구성 템플릿 아티팩트와 이미지를 저장하는 데 사용됩니다.


```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

## <a name="set-permissions-on-the-resource-group"></a>리소스 그룹에 대한 사용 권한 설정

이미지 작성기 '기여자' 권한을 부여하여 리소스 그룹에서 이미지를 만들 수 있습니다. 이렇게 하지 않으면 이미지 빌드가 실패합니다. 

값은 `--assignee` 이미지 빌더 서비스의 앱 등록 ID입니다. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```


## <a name="download-the-image-configuration-template-example"></a>이미지 구성 템플릿 예제 다운로드

시도할 매개 변수화된 이미지 구성 템플릿이 만들어졌습니다. 예제 .json 파일을 다운로드하고 이전에 설정한 변수로 구성합니다.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json -o helloImageTemplateWin.json

sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateWin.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateWin.json
sed -i -e "s/<region>/$location/g" helloImageTemplateWin.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateWin.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateWin.json

```

터미널에서 와 같은 `vi`텍스트 편집기에서 이 예제를 수정할 수 있습니다.

```azurecli-interactive
vi helloImageTemplateLinux.json
```

> [!NOTE]
> 원본 이미지의 경우 항상 [버전을 지정해야](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-version-failure)합니다. `latest`
> 이미지가 배포되는 리소스 그룹을 추가하거나 변경하는 경우 리소스 그룹에 [사용 권한을 설정해야](#set-permissions-on-the-resource-group) 합니다.
 
## <a name="create-the-image"></a>이미지 만들기

VM 이미지 빌더 서비스에 이미지 구성 제출

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateWin.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

완료되면 성공 메시지가 콘솔로 다시 반환되고 `Image Builder Configuration Template` `$imageResourceGroup`에서 를 만듭니다. '숨겨진 형식 표시'를 사용하도록 설정하면 Azure 포털의 리소스 그룹에서 이 리소스를 볼 수 있습니다.

백그라운드에서 이미지 빌더는 구독에서 준비 리소스 그룹을 만듭니다. 이 리소스 그룹은 이미지 빌드에 사용됩니다. 이 형식은 다음과 같은 형식입니다.`IT_<DestinationResourceGroup>_<TemplateName>`

> [!Note]
> 스테이징 리소스 그룹을 직접 삭제해서는 안 됩니다. 먼저 이미지 템플릿 아티팩트를 삭제하면 스테이징 리소스 그룹이 삭제됩니다.

서비스에서 이미지 구성 템플릿 제출 중에 오류가 보고되는 경우:
-  이러한 [문제 해결](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#template-submission-errors--troubleshooting) 단계를 검토합니다. 
- 제출을 다시 시도하기 전에 다음 코드 조각을 사용하여 템플릿을 삭제해야 합니다.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

## <a name="start-the-image-build"></a>이미지 빌드 시작
[az 리소스 호출-작업을](/cli/azure/resource#az-resource-invoke-action)사용하여 이미지 빌드 프로세스를 시작합니다.

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateWin01 \
     --action Run 
```

빌드가 완료될 때까지 기다립니다. 이 경우 약 15분이 소요될 수 있습니다.

오류가 발생하면 다음 [문제 해결](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-build-errors--troubleshooting) 단계를 검토하십시오.


## <a name="create-the-vm"></a>VM 만들기

작성한 이미지를 사용하여 VM을 만듭니다. * \<암호>* `aibuser` VM에 대한 사용자 고유의 암호로 바꿉습니다.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name aibImgWinVm00 \
  --admin-username aibuser \
  --admin-password <password> \
  --image $imageName \
  --location $location
```

## <a name="verify-the-customization"></a>사용자 지정 확인

VM을 만들 때 설정한 사용자 이름과 암호를 사용하여 VM에 대한 원격 데스크톱 연결을 만듭니다. VM 내부에서 cmd 프롬프트를 열고 다음을 입력합니다.

```console
dir c:\
```

이미지 사용자 지정 중에 만든 다음 두 디렉터리가 표시됩니다.
- 빌드 작업
- 빌드 아티팩트

## <a name="clean-up"></a>정리

작업이 완료되면 리소스를 삭제합니다.

### <a name="delete-the-image-builder-template"></a>이미지 빌더 템플릿 삭제

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

### <a name="delete-the-image-resource-group"></a>이미지 리소스 그룹 삭제

```azurecli-interactive
az group delete -n $imageResourceGroup
```


## <a name="next-steps"></a>다음 단계

이 문서에서 사용되는 .json 파일의 구성 요소에 대해 자세히 알아보려면 [이미지 빌더 템플릿 참조를](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)참조하십시오.
