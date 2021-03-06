---
title: 디자이너를 사용 하 여 ML 모델 빌드
titleSuffix: Azure Machine Learning
description: Azure Machine Learning용 디자이너를 구성하는 용어, 개념 및 워크플로를 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: peterlu
author: peterclu
ms.date: 06/28/2020
ms.custom: designer
ms.openlocfilehash: c92351131a60b571a102b85f7d11b79ab29d939e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90883111"
---
# <a name="what-is-azure-machine-learning-designer"></a>Azure Machine Learning 디자이너 란? 


Azure Machine Learning 디자이너를 통해 대화형 캔버스에서 [데이터 세트](#datasets) 및 [모듈](#module)을 시각적으로 연결하여 기계 학습 모델을 만들 수 있습니다. 디자이너를 시작하는 방법을 알아보려면 [자습서: 디자이너를 사용하여 자동차 가격 예측](tutorial-designer-automobile-price-train-score.md)을 참조하세요.

![Azure Machine Learning 디자이너 예](./media/concept-designer/designer-drag-and-drop.gif)

디자이너는 Azure Machine Learning [작업 영역](concept-workspace.md)을 사용하여 다음과 같은 공유 리소스를 구성합니다.

+ [파이프라인](#pipeline)
+ [데이터 세트](#datasets)
+ [컴퓨팅 리소스](#compute)
+ [등록된 모델](concept-azure-machine-learning-architecture.md#models)
+ [게시된 파이프라인](#publish)
+ [실시간 엔드포인트](#deploy)

## <a name="model-training-and-deployment"></a>모델 학습 및 배포

디자이너는 기계 학습 모델을 빌드, 테스트 및 배포하기 위한 시각적 캔버스를 제공합니다. 디자이너를 사용하여 다음 작업을 수행할 수 있습니다.

+ [데이터 세트](#datasets) 및 [모듈](#module)을 캔버스에 끌어서 놓습니다.
+ 모듈을 연결 하 여 [파이프라인 초안](#pipeline-draft)을 만듭니다.
+ Azure Machine Learning 작업 영역에서 컴퓨팅 리소스를 사용하여 [파이프라인 실행](#pipeline-run)을 제출합니다.
+ **학습 파이프라인**을 **유추 파이프라인**으로 변환합니다.
+ 다른 매개 변수 및 데이터 집합을 사용 하 여 실행 되는 새 파이프라인을 전송 하려면 REST **파이프라인 끝점** 에 파이프라인을 [게시](#publish) 합니다.
    + **학습 파이프라인**을 게시하여 매개 변수 및 데이터 세트를 변경하는 동안 여러 모델을 학습하는 데 단일 파이프라인을 다시 사용합니다.
    + **일괄 처리 유추 파이프라인**을 게시하여 이전에 학습된 모델을 통해 새 데이터에 대한 예측을 수행합니다.
+ 실시간 **유추 파이프라인** 을 실시간 끝점에 [배포](#deploy) 하 여 새 데이터에 대 한 예측을 실시간으로 만듭니다.

![디자이너의 학습, 일괄 처리 유추 및 실시간 유추를 위한 워크플로 다이어그램](./media/concept-designer/designer-workflow-diagram.png)

## <a name="pipeline"></a>파이프라인

[파이프라인](concept-azure-machine-learning-architecture.md#ml-pipelines) 은 사용자가 연결 하는 데이터 집합 및 분석 모듈로 구성 됩니다. 파이프라인은 다양한 용도로 사용됩니다. 단일 모델을 학습하는 파이프라인이나 여러 모델을 학습하는 파이프라인을 만들 수 있습니다. 실시간 또는 일괄 처리로 예측을 수행 하는 파이프라인을 만들거나 데이터를 정리 하는 파이프라인을 만들 수 있습니다. 파이프라인을 사용하여 작업을 다시 사용하고 프로젝트를 구성할 수 있습니다.

### <a name="pipeline-draft"></a>파이프라인 초안

디자이너에서 파이프라인을 편집하면 진행 상황이 **파이프라인 초안**으로 저장됩니다. 모듈을 추가하거나 제거하고, 컴퓨팅 대상을 구성하고, 매개 변수를 만드는 등의 작업을 통해 언제든지 파이프라인 초안을 편집할 수 있습니다.

유효한 파이프라인에는 다음과 같은 특징이 있습니다.

* 데이터 세트는 모듈에만 연결될 수 있습니다.
* 모듈은 데이터 세트 또는 기타 모듈에만 연결될 수 있습니다.
* 모듈의 모든 입력 포트에는 데이터 흐름에 대한 연결이 포함되어야 합니다.
* 각 모듈의 모든 필수 매개 변수를 설정해야 합니다.

파이프라인 초안을 실행할 준비가 되면 파이프라인 실행을 제출합니다.

### <a name="pipeline-run"></a>파이프라인 실행

파이프라인을 실행할 때마다 파이프라인의 구성과 해당 결과가 작업 영역에 **파이프라인 실행**으로 저장됩니다. 파이프라인 실행으로 돌아가서 문제 해결 또는 감사 목적으로 파이프라인 실행을 검사할 수 있습니다. 파이프라인 실행 **복제**하여 편집할 새 파이프라인 초안을 만듭니다.

파이프라인 실행은 [실험](concept-azure-machine-learning-architecture.md#experiments)으로 그룹화되어 실행 기록을 구성합니다. 모든 파이프라인 실행의 실험을 설정할 수 있습니다. 

## <a name="datasets"></a>데이터 세트

기계 학습 데이터 세트를 사용하여 손쉽게 데이터에 액세스하고 데이터를 사용할 수 있습니다. 실험을 위해 디자이너에 몇 가지 샘플 데이터 집합이 포함 되어 있습니다. 필요할 때 더 많은 데이터 세트를 [등록](how-to-create-register-datasets.md)할 수 있습니다.

## <a name="module"></a>모듈

모듈은 데이터에 대해 수행할 수 있는 알고리즘입니다. 디자이너에는 데이터 수신 함수에서 학습, 점수 매기기 및 유효성 검사 프로세스에 이르는 여러 모듈이 있습니다.

모듈에는 모듈 내부 알고리즘을 구성하는 데 사용할 수 있는 매개 변수 집합이 포함될 수 있습니다. 캔버스에서 모듈을 선택할 때 모듈 매개 변수가 캔버스 오른쪽의 속성 창에 표시됩니다. 해당 창에서 매개 변수를 수정하여 모델을 튜닝할 수 있습니다. 디자이너에서 개별 모듈의 컴퓨팅 리소스를 설정할 수 있습니다. 

:::image type="content" source="./media/concept-designer/properties.png"alt-text="모듈 속성":::


사용할 수 있는 기계 학습 알고리즘의 라이브러리를 탐색하는 데 도움이 필요한 경우 [알고리즘 및 모듈 참조 개요](algorithm-module-reference/module-reference.md)를 참조하세요. 알고리즘 선택에 도움이 필요한 경우 [Azure Machine Learning 알고리즘 치트 시트](algorithm-cheat-sheet.md)를 참조하세요.

## <a name="compute-resources"></a><a name="compute"></a> 컴퓨팅 리소스

작업 영역의 컴퓨팅 리소스를 사용하여 파이프라인을 실행하고 배포된 모델을 실시간 엔드포인트 또는 파이프라인 엔드포인트(일괄 처리 유추의 경우)로 호스트합니다. 지원되는 컴퓨팅 대상은 다음과 같습니다.

| 컴퓨팅 대상 | 학습 | 배포 |
| ---- |:----:|:----:|
| Azure Machine Learning 컴퓨팅 | ✓ | |
| Azure Machine Learning 컴퓨팅 인스턴스 | ✓ | |
| Azure Kubernetes Service | | ✓ |

컴퓨팅 대상은 [Azure Machine Learning 작업 영역](concept-workspace.md)에 연결됩니다. [Azure Machine Learning Studio](https://ml.azure.com)의 작업 영역에서 컴퓨팅 대상을 관리합니다.

## <a name="deploy"></a>배포

실시간 유추를 수행하려면 파이프라인을 **실시간 엔드포인트**로 배포해야 합니다. 실시간 엔드포인트는 외부 애플리케이션과 점수 매기기 모델 간 인터페이스를 만듭니다. 실시간 끝점에 대 한 호출은 응용 프로그램에 대 한 예측 결과를 실시간으로 반환 합니다. 실시간 엔드포인트를 호출하려면 엔드포인트를 배포할 때 생성된 API 키를 전달합니다. 엔드포인트는 웹 프로그래밍 프로젝트에 일반적으로 사용되는 아키텍처인 REST를 기반으로 합니다.

실시간 엔드포인트는 Azure Kubernetes Service 클러스터에 배포해야 합니다.

모델을 배포하는 방법을 알아보려면 [자습서: 디자이너를 사용하여 기계 학습 모델 배포](tutorial-designer-automobile-price-deploy.md)를 참조하세요.

## <a name="publish"></a>게시

파이프라인을 **파이프라인 엔드포인트**에 게시할 수도 있습니다. 실시간 엔드포인트와 마찬가지로, 파이프라인 엔드포인트를 사용하여 REST 호출을 통해 외부 애플리케이션에서 새 파이프라인 실행을 제출할 수 있습니다. 그러나 파이프라인 엔드포인트를 사용하여 실시간으로 데이터를 보내거나 받을 수 없습니다.

게시된 파이프라인은 유연하며, 모델을 학습 또는 재학습시키고, [일괄 처리 유추를 수행](how-to-run-batch-predictions-designer.md)하고, 새 데이터를 처리하는 등의 작업을 수행하는 데 사용할 수 있습니다. 여러 파이프라인을 단일 파이프라인 엔드포인트에 게시하고 실행할 파이프라인 버전을 지정할 수 있습니다.

게시된 파이프라인은 각 모듈의 파이프라인 초안에서 정의한 컴퓨팅 리소스에서 실행됩니다.

디자이너는 SDK와 동일한 [PublishedPipeline](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.publishedpipeline?view=azure-ml-py&preserve-view=true) 개체를 만듭니다.

## <a name="next-steps"></a>다음 단계

* [자습서: 디자이너를 사용하여 자동차 가격 예측](tutorial-designer-automobile-price-train-score.md)을 통해 예측 분석 및 기계 학습의 기본 사항을 알아봅니다.
* 기존 [디자이너 샘플](samples-designer.md)을 수정하여 요구에 맞게 조정하는 방법을 알아봅니다.
