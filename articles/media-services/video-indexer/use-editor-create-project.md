---
title: 비디오 인덱서 편집기를 사용하여 프로젝트 만들기
titleSuffix: Azure Media Services
description: 이 항목에서는 Video Indexer 편집기를 사용하여 프로젝트를 만드는 방법을 보여 줍니다.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 04/02/2019
ms.author: juliako
ms.openlocfilehash: 9f16ab34dc9b37806f9c58b22a3f02afe839632e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73839159"
---
# <a name="use-the-video-indexer-editor-to-create-projects"></a>비디오 인덱서 편집기를 사용하여 프로젝트 만들기

Video Indexer 웹 사이트를 사용하면 동영상의 심층적인 통찰력을 사용하여 적합한 미디어 콘텐츠를 찾고, 관심 있는 부분을 찾고, 결과를 사용하여 완전히 새로운 프로젝트를 만들 수 있습니다. 일단 생성되면, 프로젝트는 비디오 인덱서에서 렌더링 및 다운로드하고 자신의 편집 응용 프로그램 또는 다운 스트림 워크플로우에 사용할 수 있습니다.

이 기능이 유용할 수 있는 몇 가지 시나리오는 다음과 같습니다. 

* 예고편에 대한 동영상 하이라이트 만들기.
* 뉴스 캐스트에서 비디오의 오래된 클립을 사용.
* 소셜 미디어를 위한 짧은 콘텐츠 만들기.

이 문서에서는 프로젝트를 처음부터 만드는 방법과 계정의 비디오에서 프로젝트를 만드는 방법을 보여 줍니다.

## <a name="create-new-project-and-manage-videos"></a>새 프로젝트 만들기 및 비디오 관리

1. [Video Indexer](https://www.videoindexer.ai/) 웹 사이트로 이동하고 로그인합니다.
1. **프로젝트** 탭을 선택합니다. 이전에 프로젝트를 만든 경우 다른 모든 프로젝트가 여기에 표시됩니다.
1. **새 프로젝트 만들기를 클릭합니다.**  

    ![새 프로젝트](./media/video-indexer-view-edit/new-project.png)
1. 연필 아이콘을 클릭하여 프로젝트 이름을 지정합니다. "제목 없는 프로젝트"라는 텍스트를 프로젝트 이름으로 바꾸고 검사를 클릭합니다.

    ![새 프로젝트](./media/video-indexer-view-edit/new-project3.png)
    
### <a name="add-videos-to-the-project"></a>프로젝트에 비디오 추가

> [!NOTE]
> 현재 프로젝트에는 동일한 언어로 인덱싱된 동영상만 포함될 수 있습니다. 한 언어로 동영상을 선택하면 계정에 다른 언어로 된 동영상을 추가할 수 없습니다.

1. 이 프로젝트에서 작업할 동영상을 추가하여 비디오 **추가를**선택합니다.

    계정에 있는 모든 동영상과 '텍스트, 키워드 또는 시각적 콘텐츠 검색'이라는 검색 상자가 표시됩니다. 성적 증명서 및 OCR에서 특정 사람, 레이블, 브랜드, 키워드 또는 발생이 있는 동영상을 검색합니다.
    
    예를 들어 아래 이미지에서 "GitHub"를 언급하는 동영상을 찾고 있습니다.
    
    ![GitHub](./media/video-indexer-view-edit/github.png)

    결과를 필터링하여 결과를 추가로 **필터링할**수 있습니다. 필터링하여 특정 사람이 있는 동영상을 표시하거나 특정 언어로 된 동영상 결과만 보거나 특정 소유자가 있는 동영상 결과만 보도록 지정할 수 있습니다. <br/> 쿼리 의 범위를 지정할 수도 있습니다. 예를 들어 OCR에서 "GitHub"를 검색하려면 **시각적 텍스트를**선택합니다.

    ![Assert](./media/video-indexer-view-edit/visual-text.png)

    쿼리에 여러 필터를 계층화할 수 있습니다. **+** 사용하여 필터를 추가/제거합니다. / **-** **지우기 필터를** 사용하여 모든 필터를 제거합니다.
1. 비디오를 추가하려면 비디오를 선택한 다음 **추가를**선택합니다.
1. 이제 선택한 모든 동영상을 볼 수 있습니다. 다음은 프로젝트의 클립을 선택하려는 비디오입니다.

    끌어서 놓기하거나 목록 메뉴 단추를 선택하고 아래로 이동 또는 **위로 이동을**선택하여 비디오 순서를 다시 정렬할 수 있습니다. **Move down** 목록 메뉴에서 이 프로젝트에서 비디오를 제거할 수도 있습니다. 

    ![재정렬](./media/video-indexer-view-edit/rearrange.png)
    
    **비디오 추가를**선택하여 언제든지 이 프로젝트에 더 많은 비디오를 추가할 수 있습니다. 프로젝트에 동일한 비디오의 여러 발생을 추가할 수도 있습니다. 한 비디오의 클립을 표시한 다음 다른 비디오의 클립을 표시한 다음 첫 번째 비디오의 다른 클립을 표시하려는 경우 이 작업을 수행할 수 있습니다. 

### <a name="select-clips-to-use-in-your-project"></a>프로젝트에서 사용할 클립 선택

각 동영상의 오른쪽에 있는 아래쪽 화살표를 클릭하면 타임스탬프(비디오 클립)를 기반으로 동영상의 인사이트를 엽니다. 

1. **인사이트 보기를** 선택하여 보고 싶은 인사이트와 보고 싶지 않은 인사이트를 사용자 지정합니다. 

    ![인사이트 보기](./media/video-indexer-view-edit/insights.png)
1. 특정 클립에 대한 쿼리를 만들려면 "성적 증명서, 시각적 텍스트, 사람 및 레이블에서 검색"이라는 검색 상자를 사용합니다.
1. 필터옵션을 선택하여 원하는 장면에 대한 세부 정보를 추가하기 **위해 필터를**추가합니다.

    ![필터 옵션](./media/video-indexer-view-edit/filter-options.png)

    예를 들어 도노반 브라운이 화면에 있는 동안 GitHub가 언급된 클립을 볼 수 있습니다. 이를 위해 인사이트 유형으로 "사람"이 있는 "포함" 필터를 추가해야 합니다. 그런 다음 필터검색란에 "도노반 브라운"을 입력해야 합니다.
    
    ![포함](./media/video-indexer-view-edit/include.png)
    
    Donovan Brown이 화면에 없는 동안 GitHub가 _언급된_ 클립을 원하는 경우 드롭다운을 사용하여 "포함" 필터를 "제외" 필터로 변경하기만 하면 됩니다. 

1. 추가할 세그먼트를 선택하여 프로젝트에 클립을 추가합니다. 세그먼트를 다시 클릭하여 이 클립을 선택 취소할 수 있습니다.
    
    비디오 옆에 있는 목록 메뉴 옵션을 클릭하고 모든 세그먼트 선택을 선택하여 비디오의 모든 **세그먼트를 추가합니다.** 

    ![모두 추가](./media/video-indexer-view-edit/add-all.png)

    선택 지우기 선택을 선택하여 모든 선택 항목을 지울 수 있습니다.

> [!TIP]
> 클립을 선택하고 주문할 때 페이지 오른쪽에 있는 플레이어에서 비디오를 미리 볼 수 있습니다. 

![미리 보기](./media/video-indexer-view-edit/preview.png)

프로젝트 **저장을**선택하여 변경할 때 프로젝트를 저장해야 합니다. 

### <a name="render-and-download-the-project"></a>프로젝트 렌더링 및 다운로드

> [!NOTE]
> 비디오 인덱서 유료 계정의 경우 프로젝트를 렌더링하면 인코딩 비용이 듭니다. 비디오 인덱서 평가판 계정은 렌더링 5시간으로 제한됩니다.

1. 작업이 완료되면 프로젝트가 저장되었는지 확인합니다. 이제 이 프로젝트를 렌더링할 수 있습니다. **렌더링 및 다운로드를**선택합니다. 

    ![저장](./media/video-indexer-view-edit/save.png)

    Video 인덱서가 파일을 렌더링하고 다운로드 링크가 이메일로 전송된다는 팝업이 나타납니다. 계속을 선택합니다. 
    
    또한 프로젝트가 페이지 상단에 렌더링되고 있다는 알림도 표시됩니다. 렌더링이 완료되면 프로젝트가 성공적으로 렌더링되었다는 새 알림이 표시됩니다. 알림을 클릭하여 프로젝트를 다운로드합니다. 그것은 mp4 형식으로 프로젝트를 다운로드합니다.

    ![렌더링 완료](./media/video-indexer-view-edit/rendering-done.png)

1. **프로젝트** 탭에서 저장된 프로젝트에 액세스할 수 있습니다. 

    이 프로젝트를 선택하면 이 프로젝트의 모든 인사이트와 타임라인이 표시됩니다. **비디오 편집기를**선택하면 이 프로젝트를 계속 편집할 수 있습니다. 편집에는 비디오 및 클립 추가 또는 제거 또는 프로젝트 이름 바꾸기가 포함됩니다.

    ![비디오 편집기](./media/video-indexer-view-edit/video-editor.png)
     
## <a name="create-a-project-from-your-video"></a>비디오에서 프로젝트 만들기

계정의 동영상에서 직접 새 프로젝트를 만들 수 있습니다. 

1. 비디오 인덱서 웹 사이트의 **라이브러리** 탭으로 이동합니다.
1. 프로젝트를 만드는 데 사용할 비디오를 엽니다. 인사이트 및 타임라인 페이지에서 **비디오 편집기** 단추를 선택합니다.

    이렇게 하면 새 프로젝트를 만드는 데 사용한 것과 동일한 페이지로 이동합니다. 새 프로젝트와 달리 이전에 편집을 시작했던 비디오의 타임스탬프가 찍힌 인사이트 세그먼트가 표시됩니다.

## <a name="see-also"></a>참조

[Video Indexer 개요](video-indexer-overview.md)

