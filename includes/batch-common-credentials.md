---
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 11/09/2018
ms.author: lahugh
ms.openlocfilehash: 0ca6e38a9c9b5b92041e7f5b0fe964de58ef8f55
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "70128048"
---
## <a name="get-account-credentials"></a>계정 자격 증명 가져오기

이 예제에서는 배치 계정과 스토리지 계정에 대한 자격 증명을 제공해야 합니다. 필요한 자격 증명을 가져오는 간단한 방법은 Azure Portal에 있습니다. (Azure API 또는 명령줄 도구를 사용하여 이러한 자격 증명을 가져올 수도 있습니다.)

1. **모든 서비스** > **배치 계정**을 차례로 선택한 다음, 배치 계정의 이름을 선택합니다.

2. Batch 자격 증명을 보려면 **키**를 선택합니다. **배치 계정**, **URL** 및 **기본 액세스 키**의 값을 텍스트 편집기에 복사합니다.

3. 스토리지 계정 이름 및 키를 보려면 **스토리지 계정**을 선택합니다. **스토리지 계정 이름** 및 **Key1**의 값을 텍스트 편집기에 복사합니다.