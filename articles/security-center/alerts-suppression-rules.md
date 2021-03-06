---
title: 중복된 경고 제거 규칙을 사용하여 Azure Security Center에서 가양성 또는 기타 원치 않는 보안 경고 표시 안 함.
description: 이 문서에서는 Azure Security Center의 비 표시 규칙을 사용 하 여 원치 않는 Azure Defender 경고를 숨기는 방법을 설명 합니다.
author: memildin
manager: rkarlin
services: security-center
ms.author: memildin
ms.date: 09/10/2020
ms.service: security-center
ms.topic: how-to
ms.openlocfilehash: 50426e0b0920e89cf83dc5a81c515b06c06c09c5
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92342096"
---
# <a name="suppress-alerts-from-azure-defender"></a>Azure Defender의 경고 표시 안 함

이 페이지에서는 경고 비 표시 규칙을 사용 하 여 Azure Defender에서 가양성 또는 기타 원치 않는 보안 경고를 표시 하지 않는 방법을 설명 합니다.

## <a name="availability"></a>가용성

|양상|세부 정보|
|----|:----|
|릴리스 상태:|미리 보기|
|가격 책정:|Free<br>(대부분의 보안 경고는 Azure Defender 에서만 사용할 수 있습니다.)|
|필요한 역할 및 권한:|**보안 관리자** 및 **소유자** 는 규칙을 만들거나 삭제할 수 있습니다.<br>**보안 읽기 권한자** 및 **읽기 권한자**는 규칙을 볼 수 있습니다.|
|클라우드:|![예](./media/icons/yes-icon.png) 상용 클라우드<br>![예](./media/icons/yes-icon.png) 국가/소버린(미국 정부, 중국 정부, 기타 정부)|
|||


## <a name="what-are-suppression-rules"></a>비 표시 규칙 이란?

다양 한 Azure Defender 요금제는 사용자 환경의 모든 영역에서 위협을 감지 하 고 보안 경고를 생성 합니다.

관심 없거나 관련 없는 경고를 직접 해제할 수 있습니다. 또는 중복된 경고 제거 규칙 기능을 사용하여 나중에 유사한 경고를 자동으로 해제할 수 있습니다. 일반적으로 중복된 경고 제거 규칙은 다음과 같은 용도로 사용합니다.

- 거짓 긍정으로 식별 한 경고 표시 안 함

- 너무 자주 트리거되는 경고를 표시 하지 않습니다.

중복된 경고 제거 규칙은 경고를 자동으로 해제할 조건을 정의합니다.

> [!CAUTION]
> 보안 경고를 억제 하면 Azure Defender의 위협 방지 효과가 줄어듭니다. 중복된 경고 제거 규칙의 잠재적인 영향을 신중하게 확인하고 오랜 시간 동안 모니터링해야 합니다.

:::image type="content" source="./media/alerts-suppression-rules/create-suppression-rule.gif" alt-text="경고 표시 안 함 규칙 만들기":::

## <a name="create-a-suppression-rule"></a>제거 규칙 만들기

원치 않는 보안 경고를 표시하지 않는 규칙을 만드는 몇 가지 방법이 있습니다.

- 관리 그룹 수준에서 경고를 표시하지 않으려면 Azure Policy를 사용합니다.
- 구독 수준에서 경고를 표시하지 않으려면 아래에 설명된 대로 Azure Portal 또는 REST API를 사용할 수 있습니다.

중복된 경고 제거 규칙은 선택한 구독에서 이미 트리거된 경고만 해제할 수 있습니다.

Azure Portal에서 직접 규칙을 만들려면 다음을 수행합니다.

1. Security Center의 보안 경고 페이지에서 다음을 수행합니다.

    - 더 이상 표시하지 않을 특정 경고를 찾고, 경고의 줄임표 메뉴(...)에서 **중복된 경고 제거 규칙 만들기**를 선택합니다.

        [![**중복된 경고 제거 규칙 만들기** 옵션](media/alerts-suppression-rules/auto-dismiss-future-option.png)](media/alerts-suppression-rules/auto-dismiss-future-option.png#lightbox)

    - 또는 페이지 맨 위에 있는 **중복된 경고 제거 규칙** 링크를 선택하고, 중복된 경고 제거 규칙 페이지에서 **새 중복된 경고 제거 규칙 만들기**를 선택합니다.

        ![새 중복된 경고 제거 규칙 만들기** 단추](media/alerts-suppression-rules/create-new-suppression-rule.png)

1. 새 중복된 경고 제거 규칙 창에서 새 규칙의 세부 정보를 입력합니다.
    - 규칙은 **모든 리소스**에 대한 경고를 해제할 수 있으므로 나중에 이와 같은 경고가 표시되지 않습니다.     
    - 규칙은 특정 IP 주소, 프로세스 이름, 사용자 계정, Azure 리소스 또는 위치와 관련된 **특정 조건**에서 경고를 해제할 수 있습니다.

    > [!TIP]
    > 특정 경고에서 새 규칙 페이지를 열면 새 규칙에 경고 및 구독이 자동으로 구성됩니다. **새 중복된 경고 규칙 만들기** 링크를 사용하면 선택한 구독이 포털의 현재 필터와 일치하게 됩니다.

    [![중복된 경고 제거 규칙 만들기 창](media/alerts-suppression-rules/new-suppression-rule-pane.png)](media/alerts-suppression-rules/new-suppression-rule-pane.png#lightbox)
1. 규칙의 세부 정보를 입력합니다.
    - **이름** - 규칙의 이름입니다. 규칙 이름은 문자 또는 숫자로 시작하고 2-50자 사이여야 하며 대시(-) 또는 밑줄(_) 이외의 기호를 포함하지 않아야 합니다. 
    - **상태** - '사용' 또는 '사용 안 함'입니다.
    - **이유** - 기본 제공 이유 중 하나를 선택하고, 해당하는 이유가 없을 경우 '기타'를 선택합니다.
    - **만료 날짜** - 규칙의 종료 날짜 및 시간입니다. 최대 6개월 동안 규칙을 실행할 수 있습니다.
1. 필요에 따라 **시뮬레이트** 단추를 사용하여 규칙을 테스트하고 이 규칙을 활성화하면 해제되는 경고 수를 확인합니다.
1. 규칙을 저장합니다. 


## <a name="edit-a-suppression-rules"></a>비 표시 규칙 편집

만든 규칙을 편집 하려면 비 표시 규칙 페이지를 사용 합니다.

1. Security Center의 보안 경고 페이지에서 페이지 맨 위에 있는 **중복된 경고 제거 규칙** 링크를 선택합니다.
1. 선택한 구독에 대 한 모든 규칙을 포함 하는 비 표시 규칙 페이지가 열립니다.

    [![중복된 경고 제거 규칙 목록](media/alerts-suppression-rules/suppression-rules-page.png)](media/alerts-suppression-rules/suppression-rules-page.png#lightbox)

1. 단일 규칙을 편집하려면 규칙에 대한 줄임표 메뉴(...)를 열고 **편집**을 선택합니다.
1. 필요한 부분을 변경하고 **적용**을 선택합니다. 

## <a name="delete-a-suppression-rule"></a>비 표시 규칙 삭제

만든 규칙 중 하나 이상을 삭제하려면 중복된 경고 제거 규칙 페이지를 사용합니다.

1. Security Center의 보안 경고 페이지에서 페이지 맨 위에 있는 **중복된 경고 제거 규칙** 링크를 선택합니다.
1. 선택한 구독에 대 한 모든 규칙을 포함 하는 비 표시 규칙 페이지가 열립니다.
1. 단일 규칙을 삭제하려면 규칙에 대한 줄임표 메뉴(...)를 열고 **삭제**를 선택합니다.
1. 여러 규칙을 삭제하려면 삭제할 규칙의 확인란을 선택하고 **삭제**를 선택합니다.
    ![하나 이상의 중복된 경고 제거 규칙 삭제](media/alerts-suppression-rules/delete-multiple-alerts.png)

## <a name="view-suppressed-alerts"></a>표시 되지 않은 경고 보기

활성화된 중복된 경고 제거 규칙과 일치하는 경고는 계속 생성되지만 해당 상태는 **해제됨**으로 설정됩니다. Azure Portal에서 상태를 보거나 Security Center 보안 경고에 액세스할 수 있습니다. 

> [!TIP]
> [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/)은 제거된 경고에 대한 인시던트를 만들지 않습니다. 다른 SIEM의 경우 경고의 상태('해제됨')를 사용하여 제거된 경고를 필터링할 수 있습니다.

Security Center의 필터를 사용하여 규칙에 의해 해제된 경고를 볼 수 있습니다.

* Security Center의 보안 경고 페이지에서 필터 옵션을 열고 **해제됨**을 선택합니다.  

   [![해제된 경고 보기](media/alerts-suppression-rules/view-dismissed-alerts.png)](media/alerts-suppression-rules/view-dismissed-alerts.png#lightbox)


## <a name="create-and-manage-suppression-rules-with-the-api"></a>API를 사용 하 여 비 표시 규칙 만들기 및 관리

Security Center의 REST API를 통해 중복된 경고 제거 규칙을 만들거나, 보거나, 삭제할 수 있습니다. 

REST API에서 중복된 경고 제거 규칙의 관련 HTTP 메서드는 다음과 같습니다.

- **PUT**: 지정된 구독에서 중복된 경고 제거 규칙을 만들거나 업데이트합니다.

- **GET**:

    - 지정된 구독에 대해 구성된 모든 규칙을 나열합니다. 이 메서드는 해당하는 규칙의 배열을 반환합니다.

    - 지정된 구독에 대한 특정 규칙의 세부 정보를 가져옵니다. 이 메서드는 중복된 경고 제거 규칙 하나를 반환합니다.

    - 설계 단계에서 중복된 경고 제거 규칙의 영향을 시뮬레이션합니다. 이 호출은 규칙이 활성화되면 해제되는 기존 경고를 식별합니다.

- **DELETE**: 기존 규칙을 삭제하되 이 규칙에 의해 이미 해제된 경고의 상태는 변경하지 않습니다.

전체 세부 정보 및 사용 예제는 [API 설명서](/rest/api/securitycenter/)를 참조하세요. 


## <a name="next-steps"></a>다음 단계

이 문서에서는 원치 않는 경고를 자동으로 해제하는 Azure Security Center의 중복된 경고 제거 규칙에 대해 설명했습니다.

Azure Defender 보안 경고에 대 한 자세한 내용은 다음 페이지를 참조 하세요.

- [보안 경고 및 의도 kill 체인](alerts-reference.md) -Azure Defender에서 얻을 수 있는 보안 경고에 대 한 참조 가이드입니다.