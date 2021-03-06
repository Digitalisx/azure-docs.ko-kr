---
title: IntelliJ Azure 툴킷(미리 보기)으로 스파크 작업 디버그 - HDInsight
description: IntelliJ에 대한 Azure 도구 키트에서 HDInsight 도구를 사용하여 응용 프로그램을 디버깅하는 지침
keywords: IntelliJ 원격으로 디버그, IntelliJ 원격 디버깅, SSH, IntelliJ, HDInsight, IntelliJ 디버그, 디버깅
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 07/12/2019
ms.openlocfilehash: 1a0a6cf5a26854539dc4bbb0ae0254bbf08dad1f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73494591"
---
# <a name="failure-spark-job-debugging-with-azure-toolkit-for-intellij-preview"></a>IntelliJ용 Azure 도구 키트를 사용하여 작업 디버깅을 스파크(미리 보기)

이 문서에서는 [IntelliJ에서](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij?view=azure-java-stable) **스파크 실패 디버그** 응용 프로그램을 실행하는 데 Azure 도구 키트에서 HDInsight 도구를 사용하는 방법에 대한 단계별 지침을 제공합니다.

## <a name="prerequisites"></a>사전 요구 사항

* [오라클 자바 개발 키트](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). 이 자습서에서는 Java 버전 8.0.202를 사용합니다.
  
* IntelliJ IDEA. 이 문서는 [IntelliJ IDEA 커뮤니티 를 사용합니다.](https://www.jetbrains.com/idea/download/#section=windows)
  
* Azure Toolkit for IntelliJ. [Azure Toolkit for IntelliJ 설치](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation?view=azure-java-stable)를 참조하세요.

* HDInsight 클러스터에 연결합니다. [HDInsight 클러스터에 연결 을](apache-spark-intellij-tool-plugin.md)참조하십시오.

* 마이크로소프트 Azure 스토리지 탐색기. [마이크로소프트 Azure 저장소 탐색기 다운로드를](https://azure.microsoft.com/features/storage-explorer/)참조하십시오.

## <a name="create-a-project-with-debugging-template"></a>디버깅 템플릿으로 프로젝트 만들기

spark2.3.2 프로젝트를 만들어 실패 디버그를 계속하고 이 문서에서 샘플 파일을 디버깅하는 실패 작업 수행합니다.

1. IntelliJ IDEA를 엽니다. 새 프로젝트 창을 **엽니다.**

   a. 왼쪽 창에서 **Azure Spark/HDInsight**를 선택합니다.

   b. 기본 창에서 **장애 작업 디버깅 샘플(미리 보기)(스칼라)이 있는 스파크 프로젝트를** 선택합니다.

     ![Intellij 디버그 프로젝트 만들기](./media/apache-spark-intellij-tool-failure-debug/hdinsight-create-projectfor-failure-debug.png)

   다. **다음**을 선택합니다.

2. **새 프로젝트** 창에서 다음 단계를 수행합니다.

   ![Intellij 새로운 프로젝트 선택 스파크 버전](./media/apache-spark-intellij-tool-failure-debug/hdinsight-create-new-project.png)

   a. 프로젝트 이름과 프로젝트 위치를 입력합니다.

   b. 프로젝트 **SDK** 드롭다운 목록에서 **Spark 2.3.2** 클러스터에 대해 **Java 1.8을** 선택합니다.

   다. **스파크 버전** 드롭다운 목록에서 **스파크 2.3.2(스칼라 2.11.8)를**선택합니다.

   d. **마침**을 선택합니다.

3. **src** > **주** > **스칼라를** 선택하여 프로젝트에서 코드를 엽니다. 이 예제에서는 **AgeMean_Div()** 스크립트를 사용합니다.

## <a name="run-a-spark-scalajava-application-on-an-hdinsight-cluster"></a>HDInsight 클러스터에서 스파크 스칼라/자바 애플리케이션 실행

스파크 스칼라/Java 응용 프로그램을 만든 다음 다음 단계를 수행하여 Spark 클러스터에서 응용 프로그램을 실행합니다.

1. **실행/디버그 구성** 창을 열려면 구성 **추가를** 클릭합니다.

   ![HDI Intellij 추가 구성](./media/apache-spark-intellij-tool-failure-debug/hdinsight-add-new-configuration.png)

2. **실행/디버깅 구성** 대화 상자에서 더하기 기호(**+**)를 선택합니다. 그런 다음 **HDInsight 옵션에서 아파치 스파크를** 선택합니다.

   ![Intellij 새 구성 추가](./media/apache-spark-intellij-tool-failure-debug/hdinsight-create-new-configuraion-01.png)

3. 클러스터 **탭에서 원격으로 실행으로** 전환합니다. **이름,** **스파크 클러스터**및 **주 클래스 이름에**대한 정보를 입력합니다. 이 도구는 **실행기**를 사용하여 디버그를 지원합니다. **numExectors**, 기본값은 5이며 3보다 높게 설정하지 않는 것이 좋습니다. 런타임을 줄이려면 **spark.yarn.maxApp시도를** 작업 **구성에** 추가하고 값을 1로 설정할 수 있습니다. **확인** 버튼을 클릭하여 구성을 저장합니다.

   ![Intellij Run 디버그 구성 새](./media/apache-spark-intellij-tool-failure-debug/hdinsight-create-new-configuraion-002.png)

4. 이제 사용자가 입력한 이름으로 구성이 저장됩니다. 구성 세부 정보를 보려면 구성 이름을 선택합니다. 변경하려면 **구성 편집**을 선택합니다.

5. 구성 설정을 완료한 후 원격 클러스터에 대해 프로젝트를 실행할 수 있습니다.

   ![Intellij 디버그 원격 스파크 작업 원격 실행 버튼](./media/apache-spark-intellij-tool-failure-debug/hdinsight-local-run-configuration.png)

6. 출력 창에서 응용 프로그램 ID를 확인할 수 있습니다.

   ![Intellij 디버그 원격 스파크 작업 원격 실행 결과](./media/apache-spark-intellij-tool-failure-debug/hdinsight-remotely-run-result.png)

## <a name="download-failed-job-profile"></a>실패한 작업 프로필 다운로드

작업 제출에 실패하면 추가 디버깅을 위해 실패한 작업 프로필을 로컬 컴퓨터에 다운로드할 수 있습니다.

1. **열려 마이크로 소프트 Azure 스토리지 탐색기,** 실패 한 작업에 대 한 클러스터의 HDInsight 계정을 찾아 해당 위치에서 실패 한 작업 리소스를 다운로드: **\\\hdp\spark2-이벤트 .spark 실패\\\<응용 프로그램 ID>** 로컬 폴더에. **활동** 창에는 다운로드 진행률을 표시합니다.

   ![Azure 저장소 탐색기 다운로드 실패](./media/apache-spark-intellij-tool-failure-debug/hdinsight-find-spark-file-001.png)

   ![Azure 스토리지 탐색기 다운로드 성공](./media/apache-spark-intellij-tool-failure-debug/spark-on-cosmos-doenload-file-2.png)

## <a name="configure-local-debugging-environment-and-debug-on-failure"></a>로컬 디버깅 환경 구성 및 오류 시 디버그

1. 원래 프로젝트를 열거나 새 프로젝트를 만들고 원래 소스 코드와 연결합니다. 현재 실패 디버깅에는 spark2.3.2 버전만 지원됩니다.

1. IntelliJ IDEA에서 **스파크 실패 디버그** 구성 파일을 만들고 **스파크 작업 실패 컨텍스트 위치** 필드에 대해 이전에 다운로드한 실패한 작업 리소스에서 FTD 파일을 선택합니다.

   ![크레타 실패 구성](./media/apache-spark-intellij-tool-failure-debug/hdinsight-create-failure-configuration-01.png)

1. 도구 모음에서 로컬 실행 버튼을 클릭하면 실행 창에 오류가 표시됩니다.

   ![실행 실패 구성1](./media/apache-spark-intellij-tool-failure-debug/local-run-failure-configuraion-01.png)

   ![실행 실패 구성2](./media/apache-spark-intellij-tool-failure-debug/local-run-failure-configuration.png)

1. 로그가 나타내는 대로 중단점을 설정한 다음 로컬 디버그 버튼을 클릭하여 IntelliJ의 일반 스칼라 / Java 프로젝트와 마찬가지로 로컬 디버깅을 수행합니다.

1. 디버깅 후 프로젝트가 성공적으로 완료되면 HDInsight 클러스터의 스파크에 실패한 작업을 다시 제출할 수 있습니다.

## <a name="next-steps"></a><a name="seealso"></a>다음 단계

* [개요: 아파치 스파크 응용 프로그램 디버그](apache-spark-intellij-tool-debug-remotely-through-ssh.md)

### <a name="demo"></a>데모

* Scala 프로젝트 만들기(비디오): [Apache Spark Scala 애플리케이션 만들기](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* 원격 디버그(비디오): [Azure Toolkit for IntelliJ를 사용하여 HDInsight 클러스터에서 원격으로 Apache Spark 애플리케이션 디버그](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>시나리오

* [BI와 아파치 스파크: BI 도구와 HDInsight에서 스파크를 사용하여 대화 형 데이터 분석을 수행](apache-spark-use-bi-tools.md)
* [Machine Learning과 Apache Spark: HVAC 데이터를 사용하여 건물 온도를 분석하는 데 HDInsight의 Spark 사용](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning과 Apache Spark: HDInsight의 Spark를 사용하여 식품 검사 결과 예측](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight의 Apache Spark를 사용한 웹 사이트 로그 분석](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>애플리케이션 만들기 및 실행

* [Scala를 사용하여 독립 실행형 애플리케이션 만들기](../hdinsight-apache-spark-create-standalone-application.md)
* [Apache Livy를 사용하여 Apache Spark 클러스터에서 원격으로 작업 실행](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>도구 및 확장

* [Azure Toolkit for IntelliJ를 사용하여 HDInsight 클러스터용 Apache Spark 애플리케이션 만들기](apache-spark-intellij-tool-plugin.md)
* [Azure Toolkit for IntelliJ를 사용하여 VPN을 통해 원격으로 Apache Spark 애플리케이션 디버그](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Hortonworks 샌드박스에서 IntelliJ용 HDInsight Tools 사용](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Azure Toolkit for Eclipse의 HDInsight 도구를 사용하여 Apache Spark 애플리케이션 만들기](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [HDInsight에서 Apache Spark 클러스터와 함께 Apache Zeppelin Notebook 사용](apache-spark-zeppelin-notebook.md)
* [HDInsight의 Apache Spark 클러스터에서 Jupyter Notebook에 사용할 수 있는 커널](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter 노트북에서 외부 패키지 사용](apache-spark-jupyter-notebook-use-external-packages.md)
* [컴퓨터에 Jupyter를 설치하고 HDInsight Spark 클러스터에 연결](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>리소스 관리

* [Azure HDInsight에서 Apache Spark 클러스터에 대한 리소스 관리](apache-spark-resource-manager.md)
* [HDInsight의 Apache Spark 클러스터에서 실행되는 작업 추적 및 디버그](apache-spark-job-debugging.md)
