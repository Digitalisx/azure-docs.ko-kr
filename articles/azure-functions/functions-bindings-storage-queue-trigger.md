---
title: Azure 함수에 대한 Azure 큐 저장소 트리거
description: Azure 큐 저장소 데이터가 변경됨에 따라 Azure 함수를 실행하는 방법을 알아봅니다.
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2020
ms.author: cshoe
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 74ca984232bef979062221a451d0ee10a6965bc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277376"
---
# <a name="azure-queue-storage-trigger-for-azure-functions"></a>Azure 함수에 대한 Azure 큐 저장소 트리거

큐 저장소 트리거는 메시지가 Azure Queue 저장소에 추가될 때 함수를 실행합니다.

## <a name="encoding"></a>Encoding

함수에 *base64*로 인코딩된 문자열이 필요합니다. 인코딩 형식에 대한 조정(데이터를 *base64*로 인코딩된 문자열로 준비하기 위해)은 호출 서비스에 구현되어야 합니다.

## <a name="example"></a>예제

새 항목이 큐에 수신될 때 큐 트리거를 사용하여 함수를 시작합니다. 큐 메시지는 함수에 입력으로 제공됩니다.

# <a name="c"></a>[C #](#tab/csharp)

다음 예제에서는 `myqueue-items` 큐를 폴링하고 큐 항목이 처리될 때마다 로그를 쓰는 [C# 함수](functions-dotnet-class-library.md)를 보여줍니다.

```csharp
public static class QueueFunctions
{
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
}
```

# <a name="c-script"></a>[C# 스크립트](#tab/csharp-script)

다음 예제에서는 바인딩을 사용하는 *function.json* 파일 및 [C# 스크립트(.csx)](functions-reference-csharp.md) 코드에서 큐 트리거 바인딩을 보여 줍니다. 함수는 `myqueue-items` 큐를 폴링하고 큐 항목이 처리될 때마다 로그를 기록합니다.

*function.json* 파일은 다음과 같습니다.

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

[구성](#configuration) 섹션에서는 이러한 속성을 설명합니다.

C# 스크립트 코드는 다음과 같습니다.

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

[사용](#usage) 섹션은 function.json에서 `name` 속성에 의해 명명된 `myQueueItem`을 설명합니다.  [메시지 메타데이터 섹션](#message-metadata)에서는 표시된 다른 모든 변수를 설명합니다.

# <a name="javascript"></a>[자바 스크립트](#tab/javascript)

다음 예제는 *function.json* 파일의 큐 트리거 바인딩과 바인딩을 사용하는 [JavaScript 함수](functions-reference-node.md)를 보여 줍니다. 함수는 `myqueue-items` 큐를 폴링하고 큐 항목이 처리될 때마다 로그를 기록합니다.

*function.json* 파일은 다음과 같습니다.

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

[구성](#configuration) 섹션에서는 이러한 속성을 설명합니다.

> [!NOTE]
> name 매개 변수는 JavaScript에서 큐 항목 페이로드를 포함하는 `context.bindings.<name>`을 반영합니다. 이 페이로드는 함수에도 두 번째 매개 변수로 전달됩니다.

JavaScript 코드는 다음과 같습니다.

```javascript
module.exports = async function (context, message) {
    context.log('Node.js queue trigger function processed work item', message);
    // OR access using context.bindings.<name>
    // context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id =', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

[사용](#usage) 섹션은 function.json에서 `name` 속성에 의해 명명된 `myQueueItem`을 설명합니다.  [메시지 메타데이터 섹션](#message-metadata)에서는 표시된 다른 모든 변수를 설명합니다.

# <a name="python"></a>[Python](#tab/python)

다음 예제에서는 트리거를 통해 함수에 전달된 큐 메시지를 읽는 방법을 보여 줍니다.

저장소 큐 트리거는 *type이* 로 설정된 `queueTrigger` *function.json에* 정의됩니다.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "msg",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "messages",
      "connection": "AzureStorageQueuesConnectionString"
    }
  ]
}
```

코드 `func.ServiceBusMessage`init *\_.py는 함수에서 큐 메시지를 읽을 수 있는 매개 변수를 선언합니다. _ \__*

```python
import logging
import json

import azure.functions as func

def main(msg: func.QueueMessage):
    logging.info('Python queue trigger function processed a queue item.')

    result = json.dumps({
        'id': msg.id,
        'body': msg.get_body().decode('utf-8'),
        'expiration_time': (msg.expiration_time.isoformat()
                            if msg.expiration_time else None),
        'insertion_time': (msg.insertion_time.isoformat()
                           if msg.insertion_time else None),
        'time_next_visible': (msg.time_next_visible.isoformat()
                              if msg.time_next_visible else None),
        'pop_receipt': msg.pop_receipt,
        'dequeue_count': msg.dequeue_count
    })

    logging.info(result)
```

# <a name="java"></a>[Java](#tab/java)

다음 Java 예제에서는 큐에 배치된 트리거된 메시지를 기록하는 저장소 `myqueuename`큐 트리거 함수를 보여 주며, 이 함수는 큐에 삽입됩니다.

 ```java
 @FunctionName("queueprocessor")
 public void run(
    @QueueTrigger(name = "msg",
                   queueName = "myqueuename",
                   connection = "myconnvarname") String message,
     final ExecutionContext context
 ) {
     context.getLogger().info(message);
 }
 ```

 ---

## <a name="attributes-and-annotations"></a>특성 및 주석

# <a name="c"></a>[C #](#tab/csharp)

[C# 클래스 라이브러리](functions-dotnet-class-library.md)에서는 다음 특성을 사용하여 큐 트리거를 구성합니다.

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Queues/QueueTriggerAttribute.cs)

  특성의 생성자는 다음 예제와 같이 모니터링할 큐의 이름을 사용합니다.

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      ILogger log)
  {
      ...
  }
  ```

  다음 예제와 `Connection` 같이 사용할 저장소 계정 연결 문자열이 포함된 앱 설정을 지정하도록 속성을 설정할 수 있습니다.

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      ILogger log)
  {
      ....
  }
  ```

  전체 예제를 보려면 [예제를](#example)참조하십시오.

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  사용할 스토리지 계정을 지정하는 다른 방법을 제공합니다. 생성자는 스토리지 연결 문자열을 포함하는 앱 설정의 이름을 사용합니다. 매개 변수, 메서드 또는 클래스 수준에서 특성을 적용할 수 있습니다. 다음 예제에서는 클래스 수준 및 메서드 수준을 보여줍니다.

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("QueueTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

사용할 스토리지 계정은 다음과 같은 순서로 결정됩니다.

* `QueueTrigger` 특성의 `Connection` 속성
* `QueueTrigger` 특성과 동일한 매개 변수에 적용된 `StorageAccount` 특성
* 함수에 적용된 `StorageAccount` 특성
* 클래스에 적용된 `StorageAccount` 특성
* "AzureWebJobsStorage" 앱 설정

# <a name="c-script"></a>[C# 스크립트](#tab/csharp-script)

특성은 C# 스크립트에서 지원되지 않습니다.

# <a name="javascript"></a>[자바 스크립트](#tab/javascript)

속성은 자바 스크립트에서 지원되지 않습니다.

# <a name="python"></a>[Python](#tab/python)

특성은 파이썬에서 지원되지 않습니다.

# <a name="java"></a>[Java](#tab/java)

`QueueTrigger` 이 추가 는 함수를 트리거하는 큐에 대한 액세스를 제공합니다. 다음 예제는 매개 변수를 통해 함수에서 큐 메시지를 사용할 수 있도록 합니다. `message`

```java
package com.function;
import com.microsoft.azure.functions.annotation.*;
import java.util.Queue;
import com.microsoft.azure.functions.*;

public class QueueTriggerDemo {
    @FunctionName("QueueTriggerDemo")
    public void run(
        @QueueTrigger(name = "message", queueName = "messages", connection = "MyStorageConnectionAppSetting") String message,
        final ExecutionContext context
    ) {
        context.getLogger().info("Queue message: " + message);
    }
}
```

| 속성    | 설명 |
|-------------|-----------------------------|
|`name`       | 함수 시그니처에서 매개 변수 이름을 선언합니다. 함수가 트리거되면 이 매개 변수의 값에는 큐 메시지의 내용이 있습니다. |
|`queueName`  | 저장소 계정에서 큐 이름을 선언합니다. |
|`connection` | 저장소 계정 연결 문자열을 가리킵니다. |

---

## <a name="configuration"></a>Configuration

다음 표에서는 *function.json* 파일및 특성에서 설정한 바인딩 `QueueTrigger` 구성 속성에 대해 설명합니다.

|function.json 속성 | 특성 속성 |설명|
|---------|---------|----------------------|
|**종류** | 해당 없음| `queueTrigger`로 설정해야 합니다. 이 속성은 사용자가 Azure Portal에서 트리거를 만들 때 자동으로 설정됩니다.|
|**direction**| 해당 없음 | *function.json* 파일에서만 적용됩니다. `in`로 설정해야 합니다. 이 속성은 사용자가 Azure Portal에서 트리거를 만들 때 자동으로 설정됩니다. |
|**(이름)** | 해당 없음 |함수 코드에서 큐 항목 페이로드를 포함하는 변수 이름입니다.  |
|**큐 이름** | **큐 이름**| 폴링할 큐의 이름입니다. |
|**연결** | **연결** |이 바인딩에 사용할 스토리지 연결 문자열을 포함하는 앱 설정의 이름입니다. 앱 설정 이름이 "AzureWebJobs"로 시작하는 경우 여기에서 이름의 나머지만을 지정할 수 있습니다. 예를 들어 "MyStorage"로 설정하면 `connection` 함수 런타임에서 "MyStorage"라는 앱 설정을 찾습니다. `connection`을 비워 두면 함수 런타임 기능은 `AzureWebJobsStorage`라는 앱 설정에서 기본 스토리지 연결 문자열을 사용합니다.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>사용

# <a name="c"></a>[C #](#tab/csharp)

`string paramName`와 같은 메서드 매개 변수를 사용하여 메시지 데이터에 액세스합니다. 다음 중 원하는 형식으로 바인딩할 수 있습니다.

* 개체 - Functions 런타임은 JSON 페이로드를 코드에 정의된 임의 클래스 인스턴스로 역직렬화합니다. 
* `string`
* `byte[]`
* [클라우드큐메시지]

`CloudQueueMessage`에 바인딩하려고 하면 오류 메시지가 표시되는 경우 [올바른 Storage SDK 버전](functions-bindings-storage-queue.md#azure-storage-sdk-version-in-functions-1x)에 대한 참조가 있는지 확인합니다.

# <a name="c-script"></a>[C# 스크립트](#tab/csharp-script)

`string paramName`와 같은 메서드 매개 변수를 사용하여 메시지 데이터에 액세스합니다. 는 `paramName` `name` *function.json의*속성에 지정된 값입니다. 다음 중 원하는 형식으로 바인딩할 수 있습니다.

* 개체 - Functions 런타임은 JSON 페이로드를 코드에 정의된 임의 클래스 인스턴스로 역직렬화합니다. 
* `string`
* `byte[]`
* [클라우드큐메시지]

`CloudQueueMessage`에 바인딩하려고 하면 오류 메시지가 표시되는 경우 [올바른 Storage SDK 버전](functions-bindings-storage-queue.md#azure-storage-sdk-version-in-functions-1x)에 대한 참조가 있는지 확인합니다.

# <a name="javascript"></a>[자바 스크립트](#tab/javascript)

큐 항목 페이로드는 `context.bindings.<NAME>` `<NAME>` *function.json*에 정의된 이름과 일치하는 위치를 통해 사용할 수 있습니다. 페이로드가 JSON이면 값이 개체로 역직렬화됩니다.

# <a name="python"></a>[Python](#tab/python)

[QueueMessage로](https://docs.microsoft.com/python/api/azure-functions/azure.functions.queuemessage?view=azure-python)입력된 매개 변수를 통해 큐 메시지에 액세스합니다.

# <a name="java"></a>[Java](#tab/java)

[QueueTrigger](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.queuetrigger?view=azure-java-stable) 별추가 함수를 트리거한 큐 메시지에 대한 액세스를 제공합니다.

---

## <a name="message-metadata"></a>메시지 메타데이터

큐 트리거는 몇 가지 [메타데이터 속성](./functions-bindings-expressions-patterns.md#trigger-metadata)을 제공합니다. 이러한 속성을 다른 바인딩에서 바인딩 식의 일부로 사용하거나 코드에서 매개 변수로 사용할 수 있습니다. 속성은 [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage) 클래스의 멤버입니다.

|속성|Type|Description|
|--------|----|-----------|
|`QueueTrigger`|`string`|큐 페이로드(유효한 문자열인 경우) 큐 메시지 페이로드가 문자열인 `QueueTrigger` 경우 *function.json*에서 `name` 속성에 의해 명명된 변수와 동일한 값을 가짐|
|`DequeueCount`|`int`|이 메시지가 큐에서 제거된 횟수입니다.|
|`ExpirationTime`|`DateTimeOffset`|메시지가 만료되는 시간입니다.|
|`Id`|`string`|큐 메시지 ID입니다.|
|`InsertionTime`|`DateTimeOffset`|메시지가 큐에 추가된 시간입니다.|
|`NextVisibleTime`|`DateTimeOffset`|다음에 메시지가 표시되는 시간입니다.|
|`PopReceipt`|`string`|메시지의 PopReceipt입니다.|

## <a name="poison-messages"></a>포이즌 메시지

큐 트리거 함수가 실패하는 경우 Azure Functions는 해당 함수를 지정된 큐 메시지에 대해 최대 5번(첫 번째 시도 포함) 다시 시도합니다. 다섯 번의 시도가 모두 실패하면 함수 런타임은 * &lt;원래 큐이름>-poison라는*큐에 메시지를 추가합니다. 메시지를 기록하거나 수동 작업이 필요하다는 알림을 보내 포이즌 큐의 메시지를 처리하는 함수를 작성할 수 있습니다.

포이즌 메시지를 수동으로 처리하려면 큐 메시지의 [dequeueCount](#message-metadata)를 확인합니다.

## <a name="polling-algorithm"></a>폴링 알고리즘

큐 트리거는 무작위 지수 백오프 알고리즘을 구현하여 유휴 큐 폴링이 스토리지 트랜잭션 비용에 미치는 영향을 줄입니다.

알고리즘은 다음 논리를 사용합니다.

- 메시지가 발견되면 런타임은 2초 동안 기다린 다음 다른 메시지를 확인합니다.
- 메시지가 발견되지 않은 경우 다시 시도하기 전에 약 4초 정도 기다립니다.
- 후속 시도로 큐 메시지를 가져오지 못하면 최대 대기 시간(기본값 1분)에 도달할 때까지 대기 시간이 계속 증가합니다.
- 최대 대기 시간은 [host.json 파일](functions-host-json.md#queues)의 `maxPollingInterval` 속성을 통해 구성할 수 있습니다.

로컬 개발의 경우 최대 폴링 간격이 기본값으로 2초입니다.

청구와 관련하여 런타임에 소요된 폴링 시간은 '무료'이며 계정에 계산되지 않습니다.

## <a name="concurrency"></a>동시성

대기 중인 큐 메시지가 여러 개 있을 때, 큐 트리거는 일괄 처리 메시지를 검색하고 동시에 함수 인스턴스를 호출하여 처리합니다. 기본적으로 일괄 처리 크기는 16입니다. 처리되는 개수가 8로 감소하면 런타임은 다른 일괄 처리를 가져와 해당 메시지의 처리를 시작합니다. 따라서 VM(가상 머신) 1개에서 함수당 처리되는 최대 동시 메시지 수는 24입니다. 이 제한은 각 VM의 큐 트리거 함수에 개별적으로 적용됩니다. 함수 앱이 여러 VM으로 스케일 아웃되면 각 VM은 트리거를 기다리고 함수 실행을 시도합니다. 예를 들어, 함수 앱이 3개의 VM으로 스케일 아웃될 경우 큐 트리거 함수 1개에 대한 최대 동시 인스턴스 수의 기본값은 72입니다.

일괄 처리 크기 및 새 일괄 처리를 가져오기 위한 임계값은 [host.json 파일](functions-host-json.md#queues)에서 구성 가능합니다. 함수 앱에서 큐 트리거 함수의 병렬 실행을 최소화하려는 경우 일괄 처리 크기를 1로 설정할 수 있습니다. 이 설정은 함수 앱이 단일 VM(가상 머신)에서 실행되는 동안에만 동시성을 제거합니다. 

큐 트리거는 함수가 큐 메시지를 여러 번 처리하는 것을 방지합니다. 함수를 idempotent로 작성할 필요가 없습니다.

## <a name="hostjson-properties"></a>host.json 속성

[host.json](functions-host-json.md#queues) 파일에는 큐 트리거 동작을 제어하는 설정이 포함됩니다. 사용 가능한 설정에 대한 자세한 내용은 [host.json 설정](functions-bindings-storage-queue-output.md#hostjson-settings) 섹션을 참조하십시오.

## <a name="next-steps"></a>다음 단계

- [큐 저장소 메시지 쓰기(출력 바인딩)](./functions-bindings-storage-blob-output.md)

<!-- LINKS -->

[클라우드큐메시지]: /dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage
