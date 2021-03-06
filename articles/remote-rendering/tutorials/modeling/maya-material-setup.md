---
title: Maya에서 물리적 기반 렌더링 재질 설정
description: Maya에서 물리적 기반 렌더링 재질을 설정하고 FBX 형식으로 내보내는 방법을 설명합니다.
author: FlorianBorn71
ms.author: flborn
ms.date: 06/16/2020
ms.topic: tutorial
ms.openlocfilehash: 56aa0d91372ac2d21a20f28b1044f0811c716b0c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91358035"
---
# <a name="tutorial-set-up-physically-based-rendering-materials-in-maya"></a>자습서: Maya에서 물리적 기반 렌더링 재질 설정

## <a name="overview"></a>개요
이 자습서에서 학습할 방법은 다음과 같습니다.

> [!div class="checklist"]
>
> * 장면에서 고급 조명이 있는 재질을 개체에 할당
> * 개체 및 재질의 인스턴싱 처리
> * 장면을 FBX 형식으로 내보내고, 중요한 옵션 선택

Maya에서 [PBR(물리적 기반 렌더링) 재질](../../overview/features/pbr-materials.md) 만들기는 비교적 간단합니다. 3DS Max 같은 다른 콘텐츠 생성 앱에서 PBR을 설정하는 방법과 여러모로 비슷합니다. 이 자습서는 Azure Remote Rendering 프로젝트의 기본 PBR 셰이더 설정 및 FBX 내보내기에 대한 지침입니다. 

이 자습서의 샘플 장면에는 여러 다각형 상자 개체가 포함되어 있습니다. 이러한 개체에는 목재, 금속, 페인트를 칠한 금속, 플라스틱, 고무 등의 여러 재질이 할당됩니다. 일반적으로 각 재질에는 다음 텍스처가 전부 또는 대부분 포함되어 있습니다.

* **Albedo** - 재질 색 맵이며 **Diffuse** 또는 **BaseColor**라고도 합니다.
* **Metalness** - 재질이 금속인지, 그리고 어느 부분이 금속인지 결정합니다. 
* **Roughness** - 표면의 거칠거나 매끄러운 정도를 결정하여 표면의 반사 및 하이라이트의 선명도 또는 흐릿함에 영향을 줍니다.
* **Normal** - 다각형을 더 추가하지 않고 표면에 세부 정보를 추가합니다. 세부 정보의 예로 금속 표면의 점식(pitting) 및 파임(dent) 또는 목재의 나무의 입자를 들 수 있습니다.
* **Ambient Occlusion** - 모델에 부드러운 음영 및 콘택트 섀도(contact shadow)를 추가하는 데 사용됩니다. 모델에서 전체 조명(흰색) 또는 전체 음영(검은색)을 받는 영역을 나타내는 회색조 맵입니다. 

## <a name="prerequisites"></a>필수 구성 요소
* Autodesk Maya 2017 이상

## <a name="set-up-materials-in-the-scene"></a>장면의 재질 설정
Maya에서 PBR 재질을 설정하는 방법은 다음과 같습니다.

샘플 장면에서 볼 수 있듯이, 여러 상자 개체를 이미 만들어 두었습니다. 각 개체는 서론 다른 유형의 재질을 나타냅니다. 이미지에 표시된 것처럼, 각 개체에 적절한 고유 이름이 지정되었습니다.

Azure Remote Rendering은 측정에 미터법을 사용하며 위쪽 방향은 Y축입니다. 자산 만들기를 시작하기 전에, Maya에서 장면 단위를 미터로 설정하는 것이 좋습니다. 내보내기의 경우 FBX 내보내기 설정에서 단위를 미터로 설정합니다.

> [!TIP]
> 관련 부품 또는 재질 유형에 따라 모델 자산의 이름을 적절하게 지정합니다. 의미 있는 이름을 사용하면 개체가 많이 포함된 장면에서 좀 더 쉽게 탐색할 수 있습니다.

![개체 이름](media/object-names.jpg)

텍스처를 만들거나 가져온 후에는 고유한 텍스처를 만들 수 있습니다. Quixel Suite, PhotoShop, Substance Suite 같은 텍스처링 앱을 사용할 수도 되고, 다른 원본에서 일반 바둑판식 텍스처를 가져올 수 있습니다.

모델에 텍스처를 적용하는 방법은 다음과 같습니다.

1. 장면 뷰포트에서 모델 또는 기하 도형을 선택하고 마우스 오른쪽 단추로 클릭합니다. 표시되는 메뉴에서 **새 재질 할당**을 선택합니다.
1. **새 재질 할당** 대화 상자에서 **Maya** > **Stingray PBS**로 이동합니다. 이 작업을 수행하면 PBR 재질이 모델에 할당됩니다. 

Maya 2020에서는 다양한 PBR 셰이더를 사용할 수 있습니다. 그 중에는 **Maya 표준 표면**, **Arnold 표준 표면** 및 **Stingray PBR**이 포함되어 있습니다. **Maya 표준 표면 셰이더**는 아직 FBX 2020 플러그 인을 통해 내보낼 수 없습니다. **Arnold 표준 표면 셰이더**는 FBX 파일을 통해 내보낼 수 있습니다. 그 외의 특징은 대부분 **Maya 표준 표면 셰이더**와 동일합니다. 3D Studio Max의 **물리적 재질**과 유사합니다.

**Stingray PBR 셰이더**는 다른 여러 애플리케이션과 호환되며, Azure Remote Rendering의 요구 사항과 가장 근접하게 일치합니다. Maya 2017부터 지원됩니다. 이러한 유형의 재질을 뷰포트에서 시각화할 때 표시되는 모습은 나중에 Azure Remote Rendering에서 시각화할 때와 비슷합니다.

![Stingray 재질](media/stingray-material.jpg)

재질이 자산에 할당되고 적절한 이름이 지정된 후에는 다양한 텍스처를 할당할 수 있습니다. 다음 이미지는 PBR 재질에서 각 텍스처 유형의 적절한 위치를 보여줍니다. Stingray PBR 재질을 사용하면 활성화할 수 있는 특성을 선택할 수 있습니다. 텍스처 맵을 연결하려면 먼저 관련 특성을 활성화해야 합니다.

![재질 설정](media/material-setup.jpg)

재질의 용도 또는 유형을 고려하여 재질의 이름을 적절하게 지정합니다. 고유한 파트에 사용되는 재질은 해당 파트를 이름으로 지정할 수 있습니다. 광범위한 영역에 사용되는 재질은 해당 속성 또는 형식을 이름으로 지정할 수 있습니다.

이미지처럼 텍스처를 할당합니다.

![질감 설정](media/texture-setup.jpg)

PBR 재질이 만들어지고 설정되면 장면에서 [개체 인스턴싱](../../how-tos/conversion/configure-model-conversion.md#instancing)을 수행하는 것이 좋습니다. 장면에서 너트, 볼트, 나사, 와셔 등의 유사한 개체를 인스턴싱하면 파일 크기가 대폭 축소됩니다. 마스터 개체의 인스턴스는 자체적으로 스케일링, 회전 및 변환을 사용할 수 있으므로 장면에서 필요한 대로 배치할 수 있습니다. 

Maya에서 인스턴싱 프로세스는 간단합니다.

1. **편집** 메뉴에서 **Duplicate Special**로 이동하여 옵션을 엽니다.
1. **Duplicate Special Options** 대화 상자에서 **Geometry type**으로 **Instance** 옵션을 선택합니다. 
1. **Duplicate Special**을 선택합니다.

   ![스크린샷은 중복된 특수 옵션 대화 상자가 열려 있고 중복된 특수 옵션이 선택된 Maya 창을 보여줍니다.](media/instancing.jpg)

이 작업은 개체의 인스턴스를 만듭니다. 부모 및 부모의 다른 인스턴스와 독립적으로 이동, 회전 또는 스케일링할 수 있습니다. 

구성 요소 모드에서 인스턴스를 변경하면 모든 변경 내용이 개체의 모든 인스턴스에 전송됩니다. 예를 들어 꼭짓점이나 다각형 면처럼 인스턴싱된 개체의 구성 요소를 작업할 수 있습니다. 모든 변경 내용을 이러한 모든 인스턴스에 적용할 것인지 결정해야 합니다. 

샘플 장면에서 각 개별 상자 개체가 인스턴싱되었습니다. 이 작업은 장면을 FBX 형식으로 내보낼 때 관련이 있습니다.

![장면 개요](media/scene-overview.jpg)

> [!TIP]
> 진행할 때 장면에서 인스턴스를 만듭니다. 나중에 복사본을 인스턴스 개체로 바꾸기는 매우 어렵습니다. 

## <a name="fbx-export-process"></a>FBX 내보내기 프로세스

장면 또는 장면 자산의 FBX 내보내기에 대해 알아보겠습니다. 자산을 내보낼 때, 장면에서 내보내려는 개체 또는 자산만 선택해야 합니다. 예를 들어 장면에 개체가 100개 있다고 가정하겠습니다. 그 중 30개만 사용하려면 전체 장면을 내보낼 필요가 없습니다. 

원하는 개체를 선택하는 방법은 다음과 같습니다.

1. **File** > **Export Selection**을 선택하여 **Export Selection** 대화 상자를 엽니다.
1. **Files of type** 상자에서 **FBX export**를 선택하여 FBX 내보내기 설정을 표시합니다. 이미지에서 빨간색으로 강조 표시된 것이 FBX 내보내기의 기본 설정입니다.

   ![FBX 내보내기](media/FBX-exporting.jpg)

예를 들어 요구 사항에 따라 자산을 클라이언트에 보내려 한다고 가정하겠습니다. 자산과 함께 대량의 텍스처 파일을 보낼 필요는 없습니다. 텍스처를 내보낸 FBX 파일 내에 포함하도록 선택할 수 있습니다. 이 옵션은 패키징할 파일이 하나뿐이지만, 해당 FBX 자산의 크기가 대폭 증가할 수 있습니다. 아래와 같이 **Embed Media** 옵션을 선택하여 텍스처를 포함하는 옵션을 사용할 수 있습니다.

> [!TIP]
> 이 샘플에서는 이 상태를 반영하도록 파일 이름이 지정되었습니다. 이 명명 스타일은 자산을 추적하는 좋은 방법입니다. 

내보내기에 대한 구성을 설정한 후에는 오른쪽 아래에서 **Export Selection**을 선택합니다.

![미디어 포함](media/embedding-media.jpg)

## <a name="conclusion"></a>결론

일반적으로 이 유형의 재질은 실제 세계의 광물리학을 기반으로 하므로 더 사실적으로 표현됩니다. 이를 통해 장면이 실제 세계에 존재하는 것 같은 매우 몰입적인 효과가 생성됩니다.

## <a name="next-steps"></a>다음 단계

장면에서 개체에 고급 조명을 사용하여 재질을 설정하는 방법을 알아보았습니다. Azure Remote Rendering에서 지원하는 FBX 형식으로 개체를 내보내는 방법도 알아보았습니다. 다음 단계는 Azure Remote Rendering에서 FBX 파일을 변환하고 시각화하는 것입니다.

> [!div class="nextstepaction"]
> [빠른 시작: 렌더링을 위해 모델 변환](../../quickstarts\convert-model.md)