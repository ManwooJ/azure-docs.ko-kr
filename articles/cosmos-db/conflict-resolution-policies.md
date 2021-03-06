---
title: Azure Cosmos DB의 충돌 해결 유형 및 해결 정책
description: 이 문서에서는 Azure Cosmos DB의 충돌 범주 및 충돌 해결 정책에 대해 설명합니다.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: bfe4bd8483485ffc3b09e77e7683d2d0ec38ba75
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93090191"
---
# <a name="conflict-types-and-resolution-policies-when-using-multiple-write-regions"></a>여러 쓰기 영역을 사용 하는 경우 충돌 형식 및 해결 정책
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

충돌 및 충돌 해결 정책은 Azure Cosmos DB 계정이 여러 쓰기 지역으로 구성된 경우 적용될 수 있습니다.

여러 쓰기 지역으로 구성 된 Azure Cosmos 계정의 경우 작성자가 여러 지역에서 동일한 항목을 동시에 업데이트 하는 경우 업데이트 충돌이 발생할 수 있습니다. 업데이트 충돌은 다음과 같은 세 가지 유형 중 하나일 수 있습니다.

* **삽입 충돌** : 응용 프로그램이 두 개 이상의 지역에서 동일한 고유 인덱스를 사용 하 여 둘 이상의 항목을 동시에 삽입 하는 경우 이러한 충돌이 발생할 수 있습니다. 예를 들어이 충돌은 ID 속성을 사용 하 여 발생할 수 있습니다.

* **충돌 바꾸기** : 응용 프로그램이 둘 이상의 영역에서 동시에 동일한 항목을 업데이트할 때 이러한 충돌이 발생할 수 있습니다.

* **삭제 충돌** : 응용 프로그램이 한 지역에서 항목을 동시에 삭제 하 고 다른 지역에서 업데이트 하는 경우 이러한 충돌이 발생할 수 있습니다.

## <a name="conflict-resolution-policies"></a>충돌 해결 정책

Azure Cosmos DB는 쓰기 충돌을 해결 하는 유연한 정책 기반 메커니즘을 제공 합니다. Azure Cosmos 컨테이너에 대 한 두 가지 충돌 해결 정책 중에서 선택할 수 있습니다.

* **마지막 쓰기 Wins (LWW)** :이 해결 정책은 기본적으로 시스템 정의 타임 스탬프 속성을 사용 합니다. 시간 동기화 클록 프로토콜을 기반으로 합니다. SQL API를 사용 하는 경우 충돌 해결에 사용할 다른 사용자 지정 숫자 속성 (예: 타임 스탬프의 고유한 개념)을 지정할 수 있습니다. 사용자 지정 숫자 속성을 *충돌 해결 경로* 라고도 합니다. 

  두 개 이상 항목이 삽입 또는 대체 작업에서 충돌하는 경우 충돌 해결 경로에 대한 가장 높은 값이 포함된 항목이 선정됩니다. 여러 항목에 충돌 해결 경로에 대한 동일한 숫자 값이 있는 경우 시스템에서 선정 항목이 결정됩니다. 모든 지역은 단일 승자로 수렴 하 고 동일한 버전의 커밋된 항목으로 종료 됩니다. 삭제 충돌이 포함되어 있으면 삭제된 버전이 항상 삽입 또는 대체 충돌에 비해 우선적으로 선정됩니다. 이 결과는 충돌 해결 경로의 값에 관계 없이 발생 합니다.

  > [!NOTE]
  > 마지막 쓰기 Wins는 기본 충돌 해결 정책 이며, `_ts` SQL, MongoDB, Cassandra, Gremlin 및 Table api에 대 한 타임 스탬프를 사용 합니다. 사용자 지정 숫자 속성은 SQL API에 대해서만 사용할 수 있습니다.

  자세한 내용은 [LWW 충돌 해결 정책을 사용하는 예제](how-to-manage-conflicts.md)를 참조하세요.

* **사용자 지정** :이 해결 정책은 충돌 조정을 위해 응용 프로그램에 정의 된 의미 체계를 위해 설계 되었습니다. Azure Cosmos 컨테이너에서이 정책을 설정 하면 *병합 저장 프로시저* 도 등록 해야 합니다. 이 프로시저는 서버의 데이터베이스 트랜잭션 아래에서 충돌이 감지 되 면 자동으로 호출 됩니다. 시스템은 커밋 프로토콜의 일부로 병합 프로시저의 실행에 대해 정확히 한 번의 보장을 제공합니다.  

  사용자 지정 해결 옵션을 사용 하 여 컨테이너를 구성 하는 경우 컨테이너에 병합 프로시저를 등록 하지 못하거나 병합 프로시저에서 런타임에 예외를 throw 하는 경우 충돌은 *충돌 피드에* 기록 됩니다. 그러면, 애플리케이션은 충돌 피드에서 충돌을 수동으로 해결해야 합니다. 자세한 내용은 [사용자 지정 해결 정책을 사용하는 방법 및 충돌 피드를 사용하는 방법의 예제](how-to-manage-conflicts.md)를 참조하세요.

  > [!NOTE]
  > 사용자 지정 충돌 해결 정책은 SQL API 계정에서만 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

충돌 해결 정책을 구성 하는 방법에 대해 알아봅니다.

* [응용 프로그램에 대 한 여러 쓰기 영역을 구성 하는 방법](how-to-multi-master.md)
* [충돌 해결 정책을 관리 하는 방법](how-to-manage-conflicts.md)
* [충돌 피드를 읽는 방법](how-to-manage-conflicts.md#read-from-conflict-feed)
