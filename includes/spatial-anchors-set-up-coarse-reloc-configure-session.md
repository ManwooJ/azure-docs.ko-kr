---
author: bucurb
ms.author: bobuc
ms.date: 09/18/2019
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: 574003a150ef294aa6a2ebc035fe48cf877dec66
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "76545217"
---
## <a name="configure-the-cloud-spatial-anchor-session"></a>클라우드 공간 앵커 세션 구성

다음으로 클라우드 공간 앵커 세션을 구성합니다. 첫 번째 줄에서는 세션에서 센서 공급자를 설정합니다. 지금부터 세션 중에 만든 모든 앵커는 센서 판독값 세트와 연결됩니다. 다음으로, 주변 디바이스 찾기 기준을 인스턴스화하고 애플리케이션 요구 사항에 맞게 초기화합니다. 마지막으로, 주변 디바이스 기준에서 감시자를 만들어 앵커를 찾을 때 센서 데이터를 사용하도록 세션에 지시합니다.