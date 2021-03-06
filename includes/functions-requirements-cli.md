---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 7ddceb2808874e531cfda81e5a9ce74be3a4fab4
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92517085"
---
## <a name="configure-your-local-environment"></a>로컬 환경 구성

시작하기 전에 다음이 있어야 합니다.

+ 활성 구독이 있는 Azure 계정. [체험 계정을 만듭니다](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-java"  
+ [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md#v2) 버전 2.7.1846 이상 2.x 버전
::: zone-end  
::: zone pivot="programming-language-python"
+ 설치된 Python 버전에 해당하는 Azure Functions Core Tools 버전:

   | Python 버전 | Core Tools 버전 |
   | -------------- | ------------------ |
   | Python 3.8     | [버전 3.x](../articles/azure-functions/functions-run-local.md#v2) |
   | Python 3.6<br/>Python 3.7 | [버전 2.7.1846 이상](../articles/azure-functions/functions-run-local.md#v2) |
  
::: zone-end

+ [Azure CLI](/cli/azure/install-azure-cli) 버전 2.4 이상. 
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ [Node.js](https://nodejs.org/), 활성 LTS 및 유지 관리 LTS 버전(8.11.1 및 10.14.1 권장).
::: zone-end

::: zone pivot="programming-language-python"
+ Azure Functions에서 지원되는 [Python 3.8(64비트)](https://www.python.org/downloads/release/python-382/), [Python 3.7(64비트)](https://www.python.org/downloads/release/python-375/), [Python 3.6(64비트)](https://www.python.org/downloads/release/python-368/). 
::: zone-end
::: zone pivot="programming-language-powershell"
+ [.NET Core SDK 3.1](https://www.microsoft.com/net/download)
::: zone-end
::: zone pivot="programming-language-java"  
+ [Java Developer Kit](https://aka.ms/azure-jdks) 버전 8 또는 11. 

+ [Apache Maven](https://maven.apache.org) 버전 3.0 이상

::: zone-end
