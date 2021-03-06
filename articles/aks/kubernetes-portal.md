---
title: Azure Portal에서 Kubernetes 리소스에 액세스 (미리 보기)
description: Kubernetes 리소스와 상호 작용 하 여 Azure Portal에서 AKS (Azure Kubernetes Service) 클러스터를 관리 하는 방법에 대해 알아봅니다.
services: container-service
ms.topic: article
ms.date: 09/21/2020
ms.openlocfilehash: ae617615a8ba83e311a416581fb41d3cb6ca1b05
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92635612"
---
# <a name="access-kubernetes-resources-from-the-azure-portal-preview"></a>Azure Portal에서 Kubernetes 리소스에 액세스 (미리 보기)

Azure Portal에는 AKS (Azure Kubernetes Service) 클러스터의 Kubernetes 리소스에 쉽게 액세스할 수 있는 Kubernetes 리소스 뷰어 (미리 보기)가 포함 되어 있습니다. Azure Portal에서 Kubernetes 리소스를 보면 Azure Portal와 명령줄 도구 간의 컨텍스트 전환이 줄어들어 `kubectl` Kubernetes 리소스를 보고 편집 하는 환경이 간소화 됩니다. 리소스 뷰어에는 현재 여러 리소스 유형 (예: 배포, pod 및 복제본 집합)이 포함 되어 있습니다.

Azure Portal의 Kubernetes 리소스 뷰가 사용 중단에 대해 설정 된 [AKS dashboard 추가 기능][kubernetes-dashboard]을 대체 합니다.

>[!NOTE]
>Capabilty는 현재 [개인 Azure Kubernetes Service 클러스터](./private-clusters.md)에서 지원 되지 않습니다.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="prerequisites"></a>사전 요구 사항

Azure Portal에서 Kubernetes 리소스를 보려면 AKS 클러스터가 필요 합니다. 모든 클러스터가 지원 되지만 Azure Active Directory (Azure AD) 통합을 사용 하는 경우 클러스터에서 [AKS로 관리 되는 AZURE ad 통합][aks-managed-aad]을 사용 해야 합니다. 클러스터에서 레거시 Azure AD를 사용 하는 경우 포털에서 또는 [Azure CLI][cli-aad-upgrade]를 사용 하 여 클러스터를 업그레이드할 수 있습니다.

## <a name="view-kubernetes-resources"></a>Kubernetes 리소스 보기

Kubernetes 리소스를 보려면 Azure Portal AKS 클러스터로 이동 합니다. 왼쪽의 탐색 창은 리소스에 액세스 하는 데 사용 됩니다. 리소스에는 다음이 포함됩니다.

- **네임 스페이스** 클러스터의 네임 스페이스를 표시 합니다. 네임 스페이스 목록의 맨 위에 있는 필터는 네임 스페이스 리소스를 신속 하 게 필터링 및 표시 하는 방법을 제공 합니다.
- **워크 로드** 에는 클러스터에 배포 된 배포, pod, 복제본 집합 및 데몬 집합에 대 한 정보가 표시 됩니다. 아래 스크린샷에서는 예제 AKS 클러스터의 기본 시스템 pod를 보여 줍니다.
- **서비스 및 조절기** 는 클러스터의 서비스와 수신 리소스를 모두 표시 합니다.

:::image type="content" source="media/kubernetes-portal/workloads.png" alt-text="Azure Portal에 표시 되는 Kubernetes pod 정보입니다." lightbox="media/kubernetes-portal/workloads.png":::

### <a name="deploy-an-application"></a>애플리케이션 배포

이 예제에서는 샘플 AKS 클러스터를 사용 하 여 [AKS 빠른][portal-quickstart]시작에서 Azure 투표 응용 프로그램을 배포 합니다.

1. 리소스 뷰 (네임 스페이스, 작업, 서비스 및 조절기)에서 **추가** 를 선택 합니다.
1. [AKS 빠른][portal-quickstart]시작에서 Azure 투표 응용 프로그램에 대 한 yaml을 붙여넣습니다.
1. YAML 편집기의 맨 아래에 있는 **추가** 를 선택 하 여 응용 프로그램을 배포 합니다. 

YAML 파일이 추가 되 면 리소스 뷰어에는 생성 된 Kubernetes 서비스가 모두 표시 됩니다. 내부 서비스 (azure-응답) 및 외부 서비스 (azure-투표)는 Azure 투표 응용 프로그램에 액세스 합니다. 외부 서비스는 브라우저에서 응용 프로그램을 쉽게 볼 수 있도록 연결 된 외부 IP 주소를 포함 합니다.

:::image type="content" source="media/kubernetes-portal/portal-services.png" alt-text="Azure Portal에 표시 되는 Kubernetes pod 정보입니다." lightbox="media/kubernetes-portal/portal-services.png":::

### <a name="monitor-deployment-insights"></a>Deployment insights 모니터링

[컨테이너에 대해 Azure Monitor][enable-monitor] 를 사용 하는 AKS 클러스터는 deployment insights를 빠르게 볼 수 있습니다. Kubernetes 리소스 보기에서 사용자는 CPU 및 메모리 사용량을 비롯 한 개별 배포의 라이브 상태와 Azure monitor로 전환 하 여 자세한 정보를 확인할 수 있습니다. 다음은 샘플 AKS 클러스터의 배포 정보에 대 한 예입니다.

:::image type="content" source="media/kubernetes-portal/deployment-insights.png" alt-text="Azure Portal에 표시 되는 Kubernetes pod 정보입니다." lightbox="media/kubernetes-portal/deployment-insights.png":::

## <a name="edit-yaml"></a>YAML 편집

Kubernetes 리소스 뷰에는 YAML 편집기도 포함 되어 있습니다. 기본 제공 되는 YAML 편집기는 포털 내에서 서비스 및 배포를 업데이트 하거나 만들고 변경 내용을 즉시 적용할 수 있음을 의미 합니다.

:::image type="content" source="media/kubernetes-portal/service-editor.png" alt-text="Azure Portal에 표시 되는 Kubernetes pod 정보입니다.":::

YAML을 편집한 후에는 **검토 + 저장** 을 선택 하 고 변경 내용을 확인 한 후 다시 저장 하 여 변경 내용을 적용 합니다.

>[!WARNING]
> UI 나 CLI를 통해 직접 프로덕션 변경을 수행 하지 않는 것이 좋습니다. [CI (지속적인 통합) 및 CD (지속적인 배포) 모범 사례](kubernetes-action.md)를 활용 하는 것이 좋습니다. Azure Portal Kubernetes 관리 기능 및 YAML 편집기는 개발 및 테스트 설정에서 새 배포를 학습 하 고이를 위한 새 배포를 위해 작성 되었습니다.

## <a name="troubleshooting"></a>문제 해결

이 섹션에서는 일반적인 문제 및 문제 해결 단계를 다룹니다.

### <a name="unauthorized-access"></a>무단 액세스

Kubernetes 리소스에 액세스 하려면 AKS 클러스터, Kubernetes API 및 Kubernetes 개체에 대 한 액세스 권한이 있어야 합니다. 클러스터 관리자 또는 AKS 클러스터에 액세스할 수 있는 적절 한 권한이 있는 사용자 인지 확인 합니다. 클러스터 보안에 대 한 자세한 내용은 [AKS의 Access and identity options][concepts-identity]를 참조 하세요.

>[!NOTE]
> Azure Portal의 kubernetes 리소스 보기는 관리 되는 [aad 사용 클러스터](managed-aad.md) 또는 aad를 사용 하지 않는 클러스터 에서만 지원 됩니다. 관리 되는 AAD를 사용 하는 클러스터를 사용 하는 경우 [사용자 `kubeconfig` ](control-kubeconfig-access.md)를 끌어올 수 있는 권한 외에도 aad 사용자 또는 id에 해당 역할/역할 바인딩을 사용 하 여 kubernetes API에 액세스 해야 합니다.

### <a name="enable-resource-view"></a>리소스 뷰 사용

기존 클러스터의 경우 Kubernetes 리소스 뷰를 사용 하도록 설정 해야 할 수 있습니다. 리소스 보기를 사용 하도록 설정 하려면 포털에서 클러스터에 대 한 메시지를 따릅니다.

:::image type="content" source="media/kubernetes-portal/enable-resource-view.png" alt-text="Azure Portal에 표시 되는 Kubernetes pod 정보입니다." lightbox="media/kubernetes-portal/enable-resource-view.png":::

> [!TIP]
> Api [**server 권한이 부여 된 IP 범위**](api-server-authorized-ip-ranges.md) 에 대 한 AKS 기능을 추가 하 여 api 서버 액세스를 방화벽의 공용 끝점 으로만 제한할 수 있습니다. 이러한 클러스터에 대 한 또 다른 옵션은 `--api-server-authorized-ip-ranges` 로컬 클라이언트 컴퓨터 또는 IP 주소 범위 (포털을 검색 하는)에 대 한 액세스를 포함 하도록 업데이트 하는 것입니다. 이 액세스를 허용하려면 컴퓨터의 공용 IPv4 주소가 필요합니다. 아래 명령을 사용 하거나 인터넷 브라우저에서 "내 IP 주소는 무엇입니까"를 검색 하 여이 주소를 찾을 수 있습니다.
```bash
# Retrieve your IP address
CURRENT_IP=$(dig @resolver1.opendns.com ANY myip.opendns.com +short)

# Add to AKS approved list
az aks update -g $RG -n $AKSNAME --api-server-authorized-ip-ranges $CURRENT_IP/32

```

## <a name="next-steps"></a>다음 단계

이 문서에서는 AKS 클러스터에 대 한 Kubernetes 리소스에 액세스 하는 방법을 살펴보았습니다. Kubernetes 리소스 뷰어를 사용 하 여 액세스 하는 클러스터 리소스 및 YAML 파일에 대 한 자세한 내용은 [배포 및 yaml 매니페스트][deployments] 를 참조 하세요.

<!-- LINKS - internal -->
[kubernetes-dashboard]: kubernetes-dashboard.md
[concepts-identity]: concepts-identity.md
[portal-quickstart]: kubernetes-walkthrough-portal.md#run-the-application
[deployments]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[aks-managed-aad]: managed-aad.md
[cli-aad-upgrade]: managed-aad.md#upgrading-to-aks-managed-azure-ad-integration
[enable-monitor]: ../azure-monitor/insights/container-insights-enable-existing-clusters.md