---
title: 파이썬을 사용하여 DB를 & Azure 데이터 탐색기 클러스터 만들기
description: Python을 사용하여 Azure Data Explorer 클러스터 및 데이터베이스를 만드는 방법을 알아봅니다.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 8425058c9f6ac5b90c37a99f749a810672b406fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77560510"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-python"></a>Python을 사용하여 Azure Data Explorer 클러스터 및 데이터베이스 만들기

> [!div class="op_single_selector"]
> * [포털](create-cluster-database-portal.md)
> * [Cli](create-cluster-database-cli.md)
> * [Powershell](create-cluster-database-powershell.md)
> * [C #](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [팔 템플릿](create-cluster-database-resource-manager.md)

이 문서에서는 Python을 사용하여 Azure 데이터 탐색기 클러스터 및 데이터베이스를 만듭니다. Azure Data Explorer는 애플리케이션, 웹 사이트, IoT 디바이스 등으로부터 대량의 데이터 스트리밍에 대한 실시간 분석을 제공하는 빠른 속도의 완전 관리형 데이터 분석 서비스입니다. Azure 데이터 탐색기를 사용하려면 먼저 클러스터를 만들고 해당 클러스터에서 하나 이상의 데이터베이스를 만듭니다. 그런 다음 데이터를 수집하거나 데이터베이스에 로드하여 쿼리를 실행할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

* 활성 구독이 있는 Azure 계정. [체험 계정 만들기](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

* [Python 3.4 이상](https://www.python.org/downloads/).

* [리소스에 액세스할 수 있는 Azure AD 응용 프로그램 및 서비스 주체입니다.](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) 에 `Directory (tenant) ID` `Application ID`대한 값을 `Client Secret`가져옵니다.

## <a name="install-python-package"></a>Python 패키지 설치

Azure Data Explorer(Kusto)용 Python 패키지를 설치하려면 경로에 Python이 있는 명령 프롬프트를 엽니다. 다음 명령을 실행합니다.

```
pip install azure-common
pip install azure-mgmt-kusto
```
## <a name="authentication"></a>인증
이 문서의 예제를 실행하려면 리소스에 액세스할 수 있는 Azure AD 응용 프로그램 및 서비스 주체가 필요합니다. [Azure AD 응용 프로그램을 만들어](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) 무료 Azure AD 응용 프로그램을 만들고 구독 범위에 역할 할당을 추가합니다. 또한 `Directory (tenant) ID`을 `Application ID`얻는 방법도 표시됩니다. `Client Secret`

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Data Explorer 클러스터 만들기

1. 다음 명령을 사용하여 클러스터를 만듭니다.

    ```Python
    from azure.mgmt.kusto import KustoManagementClient
    from azure.mgmt.kusto.models import Cluster, AzureSku
    from azure.common.credentials import ServicePrincipalCredentials

    #Directory (tenant) ID
    tenant_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
    #Application ID
    client_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
    #Client Secret
    client_secret = "xxxxxxxxxxxxxx"
    subscription_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
    credentials = ServicePrincipalCredentials(
        client_id=client_id,
        secret=client_secret,
        tenant=tenant_id
    )

    location = 'Central US'
    sku_name = 'Standard_D13_v2'
    capacity = 5
    tier = "Standard"
    resource_group_name = 'testrg'
    cluster_name = 'mykustocluster'
    cluster = Cluster(location=location, sku=AzureSku(name=sku_name, capacity=capacity, tier=tier))
    
    kustoManagementClient = KustoManagementClient(credentials, subscription_id)
    
    cluster_operations = kustoManagementClient.clusters
    
    poller = cluster_operations.create_or_update(resource_group_name, cluster_name, cluster)
    ```

   |**설정** | **제안된 값** | **필드 설명**|
   |---|---|---|
   | cluster_name | *mykustocluster* | 원하는 클러스터 이름입니다.|
   | sku_name | *Standard_D13_v2* | 클러스터에 사용될 SKU입니다. |
   | 계층 | *Standard* | SKU 계층입니다. |
   | 용량 | *수* | 클러스터의 인스턴스 수입니다. |
   | resource_group_name | *테서그 (것)에 대한* | 클러스터가 만들어질 리소스 그룹 이름입니다. |

    > [!NOTE]
    > **클러스터 만들기는** 장기 실행 작업입니다. 메서드 **create_or_update** LROPoller의 인스턴스를 반환, 자세한 정보를 얻으려면 [LROPoller 클래스를](/python/api/msrest/msrest.polling.lropoller?view=azure-python) 참조 하십시오.

1. 다음 명령을 실행하여 클러스터가 성공적으로 만들어졌는지 확인합니다.

    ```Python
    cluster_operations.get(resource_group_name = resource_group_name, cluster_name= clusterName, custom_headers=None, raw=False)
    ```

결과에 값이 `Succeeded`인 `provisioningState`가 있으면 클러스터가 성공적으로 만들어진 것입니다.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Data Explorer 클러스터에 데이터베이스 만들기

1. 다음 명령을 사용하여 데이터베이스를 만듭니다.

    ```Python
    from azure.mgmt.kusto.models import Database
    from datetime import timedelta
    
    softDeletePeriod = timedelta(days=3650)
    hotCachePeriod = timedelta(days=3650)
    databaseName="mykustodatabase"
    
    database_operations = kusto_management_client.databases 
    _database = ReadWriteDatabase(location=location,
                        soft_delete_period=softDeletePeriod,
                        hot_cache_period=hotCachePeriod)
    
    #Returns an instance of LROPoller, see https://docs.microsoft.com/python/api/msrest/msrest.polling.lropoller?view=azure-python
    poller =database_operations.create_or_update(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName, parameters = _database)
    ```

        [!NOTE]
        If you are using Python version 0.4.0 or below, use Database instead of ReadWriteDatabase.

   |**설정** | **제안된 값** | **필드 설명**|
   |---|---|---|
   | cluster_name | *mykustocluster* | 데이터베이스가 만들어지는 클러스터의 이름입니다.|
   | database_name | *mykustodatabase* | 데이터베이스의 이름입니다.|
   | resource_group_name | *테서그 (것)에 대한* | 클러스터가 만들어질 리소스 그룹 이름입니다. |
   | soft_delete_period | *3650 days, 0:00:00* | 데이터를 쿼리할 수 있도록 유지되는 시간입니다. |
   | hot_cache_period | *3650 days, 0:00:00* | 데이터가 캐시에 유지되는 시간입니다. |

1. 다음 명령을 실행하여 직접 만든 데이터베이스를 살펴봅니다.

    ```Python
    database_operations.get(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName)
    ```

이제 클러스터와 데이터베이스가 있습니다.

## <a name="clean-up-resources"></a>리소스 정리

* 다른 기사를 팔로우하려면 만든 리소스를 보관하십시오.
* 리소스를 정리하려면 클러스터를 삭제합니다. 클러스터를 삭제하면 해당 클러스터에 있는 모든 데이터베이스도 함께 삭제됩니다. 다음 명령을 사용하여 클러스터를 삭제합니다.

    ```Python
    cluster_operations.delete(resource_group_name = resource_group_name, cluster_name = clusterName)
    ```

## <a name="next-steps"></a>다음 단계

* [Azure 데이터 탐색기 파이썬 라이브러리를 사용하여 데이터 수집](python-ingest-data.md)
