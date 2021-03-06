---
title: 'VNet에 여러 VPN gateway 사이트 간 연결 추가: Azure Portal'
description: 기존 연결이 있는 VPN Gateway에 다중 사이트 S2S 연결 추가
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/27/2020
ms.author: cherylmc
ms.openlocfilehash: 92d39b0d39511571701fd092f641cb8ca3ae42c7
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2020
ms.locfileid: "92890171"
---
# <a name="add-additional-s2s-connections-to-a-vnet-azure-portal"></a>VNet에 S2S 연결 추가: Azure Portal

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell(클래식)](vpn-gateway-multi-site.md)
>

이 문서는 기존 연결이 있는 VPN 게이트웨이에 다른 S2S (사이트 간) 연결을 추가 하는 데 도움이 됩니다. 이 아키텍처를 종종 "다중 사이트" 구성 이라고 합니다. S2S 연결, 사이트 간 연결 또는 VNet 간 연결이 이미 있는 VNet에 S2S 연결을 추가할 수 있습니다. 연결을 추가하는 데 몇 가지 제한이 있습니다. 구성을 시작 하기 전에이 문서의 [필수 구성 요소](#before) 섹션을 확인 하 여 확인 합니다.

이 문서는 RouteBased VPN 게이트웨이가 있는 리소스 관리자 VNet에 적용됩니다. 이러한 단계는 새로운 Express 경로/사이트 간 공존 연결 구성에는 적용 되지 않습니다. 그러나 이미 존재 하는 기존 구성에 새 VPN 연결을 추가 하는 경우에는 다음 단계를 사용할 수 있습니다. 공존 연결에 대한 자세한 내용은 [xpress 경로/S2S 공존 연결](../expressroute/expressroute-howto-coexist-resource-manager.md)을 참조하세요.

## <a name="prerequisites"></a><a name="before"></a>필수 조건

다음 사항을 확인합니다.

* 새 공존 Express 경로 및 VPN Gateway 구성을 구성 하지 않습니다.
* 기존 연결로 Resource Manager 배포 모델을 사용하여 만든 가상 네트워크가 있습니다.
* VNet용 가상 네트워크 게이트웨이는 RouteBased입니다. PolicyBased VPN 게이트웨이가 있다면 가상 네트워크 게이트웨이를 삭제하고 새 VPN 게이트웨이를 RouteBased로 만들어야 합니다.
* 어떠한 주소 범위도 이 Vnet이 연결하고 있는 Vnet에 대해 겹치지 않습니다.
* 호환되는 VPN 디바이스 및 그것을 구성할 수 있는 사람이 있습니다. [VPN 디바이스 정보](vpn-gateway-about-vpn-devices.md)를 참조하세요. VPN 디바이스를 구성하는 방법과 온-프레미스 네트워크 구성에 있는 IP 주소 범위에 익숙하지 않은 경우 세부 정보를 제공할 수 있는 다른 사람의 도움을 받아야 합니다.
* VPN 디바이스에 대한 외부 연결 공용 IP 주소가 있습니다.

## <a name="configure-a-connection"></a><a name="configure"></a>연결 구성

1. 브라우저에서 [Azure Portal](https://portal.azure.com) 로 이동 하 고 필요한 경우 Azure 계정으로 로그인 합니다.
1. **모든 리소스** 를 선택 하 고 리소스 목록에서 **가상 네트워크 게이트웨이** 를 찾아 선택 합니다.
1. **가상 네트워크 게이트웨이** 페이지에서 **연결** 을 선택 합니다.

   :::image type="content" source="./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connections.png" alt-text="VPN gateway 연결":::
1. **연결** 페이지에서 **+ 추가** 를 선택 합니다.
1. 그러면 **연결 추가** 페이지가 열립니다.

   :::image type="content" source="./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/add-connection.png" alt-text="VPN gateway 연결":::
1. **연결 추가** 페이지에서 다음 필드에 정보를 입력합니다.

   * **이름:** 연결을 만들고자 하는 사이트에 부여하고자 하는 이름입니다.
   * **연결 형식:** **사이트 간 (IPsec)을** 선택 합니다.

## <a name="add-a-local-network-gateway"></a><a name="local"></a>로컬 네트워크 게이트웨이 추가

1. **로컬 네트워크 게이트웨이** 필드에서 **_로컬 네트워크 게이트웨이 선택_ _을 선택 합니다 *. 그러면 _* 로컬 네트워크 게이트웨이 선택 페이지가 열립니다** .
1. **+ 새로 만들기** 를 선택 하 여 **로컬 네트워크 게이트웨이 만들기** 페이지를 엽니다.

   :::image type="content" source="./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/create-local-network-gateway.png" alt-text="VPN gateway 연결":::
1. **로컬 네트워크 게이트웨이 만들기** 페이지에서 다음 필드에 정보를 입력합니다.

   * **이름:** 로컬 네트워크 게이트웨이 리소스에 부여하고자 하는 이름입니다.
   * **끝점:** 연결 하려는 사이트의 VPN 장치에 대 한 공용 IP 주소 또는 끝점의 FQDN입니다.
   * **주소 공간:** 새 로컬 네트워크 사이트로 라우팅하려는 주소 공간입니다.
1. **로컬 네트워크 게이트웨이 만들기** 페이지에서 **확인** 을 선택 하 여 변경 내용을 저장 합니다.

## <a name="add-the-shared-key"></a><a name="part3"></a>공유 키 추가

1. 로컬 네트워크 게이트웨이를 만든 후에 **연결 추가** 페이지로 돌아갑니다.
1. 나머지 필드를 완료 합니다. **공유 키 (PSK)** 의 경우 vpn 장치에서 공유 키를 가져오거나 여기에서 공유 키를 만든 다음 동일한 공유 키를 사용 하도록 vpn 장치를 구성할 수 있습니다. 중요한 것은 키가 정확히 동일하다는 것입니다.

## <a name="create-the-connection"></a><a name="create"></a>연결 만들기

1. 페이지 맨 아래에서 **확인** 을 선택 하 여 연결을 만듭니다. 연결이 즉시 생성 되기 시작 합니다.
1. 연결이 완료 되 면 확인 하 고 확인할 수 있습니다.

## <a name="view-and-verify-the-vpn-connection"></a><a name="verify"></a>VPN 연결 보기 및 확인

[!INCLUDE [Verify the connection](../../includes/vpn-gateway-verify-connection-portal-include.md)]

## <a name="next-steps"></a>다음 단계

연결이 완료되면 가상 네트워크에 가상 머신을 추가할 수 있습니다. 자세한 내용은 [가상 머신 학습 경로](/learn/paths/deploy-a-website-with-azure-virtual-machines/)를 참조 하세요.
