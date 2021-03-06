---
title: Azure Data Factory의 웹 활동
description: Data Factory에서 지원하는 제어 흐름 작업 중 하나인 웹 작업을 사용하여 파이프라인에서 REST 엔드포인트를 호출하는 방법을 알아봅니다.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/19/2018
ms.openlocfilehash: e74361d6fb3eb1f9708f39f198506d16c7c046c4
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92635102"
---
# <a name="web-activity-in-azure-data-factory"></a>Azure Data Factory에서 웹 작업
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]


웹 작업은 Data Factory 파이프라인에서 사용자 지정 REST 엔드포인트를 호출하는 데 사용할 수 있습니다. 작업에서 사용하고 액세스하도록 데이터 세트 및 연결된 서비스를 전달할 수 있습니다.

> [!NOTE]
> 웹 작업은 자체 호스팅 통합 런타임을 활용하여 프라이빗 가상 네트워크에서 호스트되는 URL을 호출하는 경우에도 지원됩니다. 통합 런타임에서 URL 엔드포인트를 직접 확인할 수 있어야 합니다. 

## <a name="syntax"></a>구문

```json
{
   "name":"MyWebActivity",
   "type":"WebActivity",
   "typeProperties":{
      "method":"Post",
      "url":"<URLEndpoint>",
      "connectVia": {
          "referenceName": "<integrationRuntimeName>",
          "type": "IntegrationRuntimeReference"
      }
      "headers":{
         "Content-Type":"application/json"
      },
      "authentication":{
         "type":"ClientCertificate",
         "pfx":"****",
         "password":"****"
      },
      "datasets":[
         {
            "referenceName":"<ConsumedDatasetName>",
            "type":"DatasetReference",
            "parameters":{
               ...
            }
         }
      ],
      "linkedServices":[
         {
            "referenceName":"<ConsumedLinkedServiceName>",
            "type":"LinkedServiceReference"
         }
      ]
   }
}

```

## <a name="type-properties"></a>형식 속성

속성 | Description | 허용되는 값 | 필수
-------- | ----------- | -------------- | --------
name | 웹 작업의 이름입니다. | String | 예
type | **WebActivity** 로 설정해야 합니다. | String | 예
method | 대상 엔드포인트에 대한 Rest API 메서드입니다. | 문자열. <br/><br/>지원되는 형식: "GET", "POST", "PUT" | 예
url | 대상 엔드포인트 및 경로입니다. | 문자열(또는 resultType 문자열이 있는 식). 활동이 엔드포인트에서 응답을 수신하지 않는 경우 오류가 발생하여 1분에 시간이 초과됩니다. | 예
headers | 요청에 전송되는 헤더입니다. 예를 들어 요청에 언어 및 형식을 설정하려면 다음과 같이 합니다. `"headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 문자열(또는 resultType 문자열이 있는 식) | 예, Content-Type 헤더가 필요합니다. `"headers":{ "Content-Type":"application/json"}`
본문 | 엔드포인트에 전송된 페이로드를 나타냅니다.  | 문자열(또는 resultType 문자열이 있는 식). <br/><br/>[요청 페이로드 스키마](#request-payload-schema) 섹션에서 요청 페이로드의 스키마를 참조하세요. | POST/PUT 메서드에 필요합니다.
인증 | 엔드포인트를 호출하는 데 사용되는 인증 방법입니다. 지원되는 형식은 "Basic" 또는 "ClientCertificate"입니다. 자세한 내용은 [인증](#authentication) 섹션을 참조하세요. 인증이 필요 없는 경우 이 속성을 제외합니다. | 문자열(또는 resultType 문자열이 있는 식) | 아니요
datasets | 엔드포인트에 전달되는 데이터 세트의 목록입니다. | 데이터 세트 참조의 배열입니다. 빈 배열일 수 있습니다. | 예
linkedServices | 엔드포인트에 전달되는 연결된 서비스 목록입니다. | 연결된 서비스 참조의 배열입니다. 빈 배열일 수 있습니다. | 예
connectVia | 데이터 저장소에 연결하는 데 사용할 [통합 런타임](./concepts-integration-runtime.md)입니다. Azure integration runtime 또는 자체 호스팅 integration runtime (데이터 저장소가 개인 네트워크에 있는 경우)을 사용할 수 있습니다. 이 속성이 지정 되지 않은 경우 서비스는 기본 Azure integration runtime을 사용 합니다. | Integration runtime 참조입니다. | 아니요 

> [!NOTE]
> 웹 작업이 호출하는 REST 엔드포인트는 JSON 형식의 응답을 반환해야 합니다. 활동이 엔드포인트에서 응답을 수신하지 않는 경우 오류가 발생하여 1분에 시간이 초과됩니다.

다음 표에서는 JSON 콘텐츠에 대한 요구 사항을 보여 줍니다.

| 값 형식 | 요청 본문 | 응답 본문 |
|---|---|---|
|JSON 개체 | 지원됨 | 지원됨 |
|JSON 배열 | 지원됨 <br/>(현재 JSON 배열은 버그로 인해 작동하지 않습니다. 수정이 진행 중입니다.) | 지원되지 않음 |
| JSON 값 | 지원됨 | 지원되지 않음 |
| 비-JSON 형식 | 지원되지 않음 | 지원되지 않음 |
||||

## <a name="authentication"></a>인증

다음은 웹 작업에서 지원 되는 인증 유형입니다.

### <a name="none"></a>없음

인증이 필요 없는 경우 "authentication" 속성을 포함하지 않습니다.

### <a name="basic"></a>기본

기본 인증에 사용할 사용자 이름 및 암호를 지정합니다.

```json
"authentication":{
   "type":"Basic",
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>클라이언트 인증서

PFX 파일의 base64로 인코딩된 콘텐츠 및 암호를 지정합니다.

```json
"authentication":{
   "type":"ClientCertificate",
   "pfx":"****",
   "password":"****"
}
```

### <a name="managed-identity"></a>관리 ID

데이터 팩터리에 대한 관리 ID를 사용하여 요청되는 액세스 토큰에 대한 리소스 URI를 지정합니다. Azure Resource 관리 API를 호출하려면 `https://management.azure.com/`을 사용합니다. 관리 ID의 작동 방식에 대한 자세한 내용은 [Azure 리소스의 관리 ID 개요 페이지](../active-directory/managed-identities-azure-resources/overview.md)를 참조하세요.

```json
"authentication": {
    "type": "MSI",
    "resource": "https://management.azure.com/"
}
```

> [!NOTE]
> 데이터 팩터리가 git 리포지토리로 구성 된 경우 기본 또는 클라이언트 인증서 인증을 사용 하도록 Azure Key Vault에 자격 증명을 저장 해야 합니다. Azure Data Factory는 git에 암호를 저장 하지 않습니다.

## <a name="request-payload-schema"></a>요청 페이로드 스키마
POST/PUT 메서드를 사용하는 경우 body 속성은 엔드포인트에 전송되는 페이로드를 나타냅니다. 연결된 서비스 및 데이터 세트를 페이로드의 일부로 전달할 수 있습니다. 페이로드 스키마는 다음과 같습니다.

```json
{
    "body": {
        "myMessage": "Sample",
        "datasets": [{
            "name": "MyDataset1",
            "properties": {
                ...
            }
        }],
        "linkedServices": [{
            "name": "MyStorageLinkedService1",
            "properties": {
                ...
            }
        }]
    }
}
```

## <a name="example"></a>예제
이 예제에서 파이프라인의 웹 작업은 REST 끝점을 호출하고 Azure SQL 연결된 서비스 및 Azure SQL 데이터 세트를 엔드포인트에 전달합니다. REST 끝점은 Azure SQL 연결 문자열을 사용 하 여 논리 SQL server에 연결 하 고 SQL server 인스턴스의 이름을 반환 합니다.

### <a name="pipeline-definition"></a>파이프라인 정의

```json
{
    "name": "<MyWebActivityPipeline>",
    "properties": {
        "activities": [
            {
                "name": "<MyWebActivity>",
                "type": "WebActivity",
                "typeProperties": {
                    "method": "Post",
                    "url": "@pipeline().parameters.url",
                    "headers": {
                        "Content-Type": "application/json"
                    },
                    "authentication": {
                        "type": "ClientCertificate",
                        "pfx": "*****",
                        "password": "*****"
                    },
                    "datasets": [
                        {
                            "referenceName": "MySQLDataset",
                            "type": "DatasetReference",
                            "parameters": {
                                "SqlTableName": "@pipeline().parameters.sqlTableName"
                            }
                        }
                    ],
                    "linkedServices": [
                        {
                            "referenceName": "SqlLinkedService",
                            "type": "LinkedServiceReference"
                        }
                    ]
                }
            }
        ],
        "parameters": {
            "sqlTableName": {
                "type": "String"
            },
            "url": {
                "type": "String"
            }
        }
    }
}

```

### <a name="pipeline-parameter-values"></a>파이프라인 매개 변수 값

```json
{
    "sqlTableName": "department",
    "url": "https://adftes.azurewebsites.net/api/execute/running"
}

```

### <a name="web-service-endpoint-code"></a>웹 서비스 엔드포인트 코드

```csharp

[HttpPost]
public HttpResponseMessage Execute(JObject payload)
{
    Trace.TraceInformation("Start Execute");

    JObject result = new JObject();
    result.Add("status", "complete");

    JArray datasets = payload.GetValue("datasets") as JArray;
    result.Add("sinktable", datasets[0]["properties"]["typeProperties"]["tableName"].ToString());

    JArray linkedServices = payload.GetValue("linkedServices") as JArray;
    string connString = linkedServices[0]["properties"]["typeProperties"]["connectionString"].ToString();

    System.Data.SqlClient.SqlConnection sqlConn = new System.Data.SqlClient.SqlConnection(connString);

    result.Add("sinkServer", sqlConn.DataSource);

    Trace.TraceInformation("Stop Execute");

    return this.Request.CreateResponse(HttpStatusCode.OK, result);
}

```

## <a name="next-steps"></a>다음 단계
Data Factory에서 지원하는 다른 제어 흐름 작업을 참조하세요.

- [파이프라인 실행 작업](control-flow-execute-pipeline-activity.md)
- [ForEach 작업](control-flow-for-each-activity.md)
- [메타데이터 작업 가져오기](control-flow-get-metadata-activity.md)
- [조회 작업](control-flow-lookup-activity.md)