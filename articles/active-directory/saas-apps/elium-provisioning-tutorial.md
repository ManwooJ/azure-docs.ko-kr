---
title: '자습서: Azure Active Directory을 사용 하 여 자동 사용자 프로 비전을 위한 Elium 구성 | Microsoft Docs'
description: 사용자 계정을 Elium로 자동으로 프로 비전 및 프로 비전 해제 하도록 Azure Active Directory를 구성 하는 방법에 대해 알아봅니다.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 08/19/2019
ms.author: Zhchia
ms.openlocfilehash: d9bb21ab4b5610c8318fe4ee869f1140faf55367
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88544233"
---
# <a name="tutorial-configure-elium-for-automatic-user-provisioning"></a>자습서: 자동 사용자 프로 비전을 위한 Elium 구성

이 자습서에서는 사용자 또는 그룹을 Elium로 자동으로 프로 비전 및 프로 비전 해제 하도록 Elium 및 Azure Active Directory (Azure AD)를 구성 하는 방법을 보여 줍니다.

> [!NOTE]
> 이 자습서에서는 Azure AD 사용자 프로 비전 서비스 위에 구축 된 커넥터에 대해 설명 합니다. 이 서비스의 기능 및 작동 방식에 대 한 중요 한 정보 및 질문과 대답은 [Azure Active Directory를 사용 하 여 SaaS 응용 프로그램에 대 한 사용자 프로 비전 및 프로 비전](../app-provisioning/user-provisioning.md)해제를 참조 하세요.
>
> 이 커넥터는 현재 미리 보기로 제공되고 있습니다. 미리 보기의 Azure 기능에 대 한 일반적인 사용 약관은 [Microsoft Azure 미리 보기의 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조 하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 자습서에서는 사용자에 게 이미 다음과 같은 필수 구성 요소가 있다고 가정 합니다.

* Azure AD 테넌트
* [Elium 테 넌 트](https://www.elium.com/pricing/)
* 관리자 권한이 있는 Elium의 사용자 계정

## <a name="assigning-users-to-elium"></a>Elium에 사용자 할당

Azure AD는 *할당* 이라는 개념을 사용 하 여 선택한 앱에 대 한 액세스 권한을 받는 사용자를 결정 합니다. 자동 사용자 프로 비전의 컨텍스트에서는 Azure AD의 응용 프로그램에 할당 된 사용자 및 그룹만 동기화 됩니다.

자동 사용자 프로비저닝을 구성 하 고 사용 하도록 설정 하기 전에 Elium에 액세스 해야 하는 Azure AD의 사용자 및 그룹을 결정 합니다. 그런 다음 [엔터프라이즈 앱에 사용자 또는 그룹 할당](../manage-apps/assign-user-or-group-access-portal.md)의 단계에 따라 이러한 사용자 및 그룹을 Elium에 할당 합니다.

## <a name="important-tips-for-assigning-users-to-elium"></a>Elium에 사용자를 할당 하기 위한 주요 팁 

자동 사용자 프로 비전 구성을 테스트 하려면 단일 Azure AD 사용자를 Elium에 할당 하는 것이 좋습니다. 나중에 더 많은 사용자와 그룹을 할당할 수 있습니다.

사용자를 Elium에 할당할 때 할당 대화 상자에서 유효한 응용 프로그램별 역할 (있는 경우)을 선택 해야 합니다. **기본 액세스** 역할이 있는 사용자는 프로 비전에서 제외 됩니다.

## <a name="set-up-elium-for-provisioning"></a>프로 비전을 위한 Elium 설정

Azure AD를 사용 하 여 자동 사용자 프로 비전을 위한 Elium를 구성 하기 전에 Elium에서 SCIM (도메인 간 Id 관리) 프로 비전을 위해 시스템을 사용 하도록 설정 해야 합니다. 다음 단계를 수행합니다.

1. Elium에 로그인 하 고 **내 프로필**  >  **설정**으로 이동 합니다.

    ![Elium의 설정 메뉴 항목](media/Elium-provisioning-tutorial/setting.png)

1. 왼쪽 아래 모서리에 있는 **고급**에서 **보안**을 선택 합니다.

    ![Elium의 보안 링크](media/Elium-provisioning-tutorial/security.png)

1. **테 넌 트 URL** 및 **암호 토큰** 값을 복사 합니다. 이러한 값은 나중에 Azure Portal Elium 응용 프로그램의 **프로 비전** 탭에 있는 해당 필드에서 사용 합니다.

    ![Elium의 테 넌 트 URL 및 비밀 토큰 필드](media/Elium-provisioning-tutorial/token.png)

## <a name="add-elium-from-the-gallery"></a>갤러리에서 Elium 추가

Azure AD를 사용 하 여 자동 사용자 프로 비전을 위해 Elium를 구성 하려면 Azure AD 응용 프로그램 갤러리의 Elium를 관리 되는 SaaS (software as a service) 응용 프로그램 목록에 추가 해야 합니다. 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com)의 왼쪽 탐색 패널에서 **Azure Active Directory**를 선택 합니다.

    ![Azure Active Directory 메뉴 항목](common/select-azuread.png)

1. **엔터프라이즈 애플리케이션**으로 이동한 다음, **모든 애플리케이션**을 선택합니다.

     ![Azure AD 엔터프라이즈 응용 프로그램 블레이드](common/enterprise-applications.png)

1. 새 응용 프로그램을 추가 하려면 창 위쪽에서 **새 응용 프로그램** 을 선택 합니다.

    ![새 응용 프로그램 링크](common/add-new-app.png)

1. 검색 상자에 **Elium**를 입력 하 고 결과 목록에서 **Elium** 를 선택한 다음 **추가** 를 선택 하 여 응용 프로그램을 추가 합니다.

    ![갤러리 검색 상자](common/search-new-app.png)

## <a name="configure-automatic-user-provisioning-to-elium"></a>Elium에 자동 사용자 프로 비전 구성

이 섹션에서는 azure ad의 사용자 및 그룹 할당에 따라 Elium에서 사용자 및 그룹을 만들고, 업데이트 하 고, 비활성화 하도록 Azure AD 프로 비전 서비스를 구성 하는 단계를 안내 합니다.

> [!TIP]
> [Elium Single Sign-On 자습서](Elium-tutorial.md)의 지침에 따라 SECURITY ASSERTION MARKUP LANGUAGE (SAML)을 기반으로 Elium에 대 한 Single Sign-On를 사용 하도록 선택할 수도 있습니다. 자동 사용자 프로 비전과 독립적으로 Single Sign-On를 구성할 수 있습니다. 단, 두 가지 기능은 서로 보완적입니다.

Azure AD에서 Elium에 대 한 자동 사용자 프로 비전을 구성 하려면 다음 단계를 수행 합니다.

1. [Azure Portal](https://portal.azure.com)에 로그인 하 고 **엔터프라이즈 응용 프로그램**을 선택한 다음 **모든 응용 프로그램**을 선택 합니다.

    ![Azure AD 엔터프라이즈 응용 프로그램 블레이드](common/enterprise-applications.png)

1. 애플리케이션 목록에서 **Elium**을 선택합니다.

    ![엔터프라이즈 응용 프로그램 블레이드의 응용 프로그램 목록](common/all-applications.png)

1. **프로비전** 탭을 선택합니다.

    ![엔터프라이즈 응용 프로그램 블레이드의 프로 비전 탭](common/provisioning.png)

1. **프로비전 모드**를 **자동**으로 설정합니다.

    ![프로 비전 모드 자동 설정](common/provisioning-automatic.png)

1. **관리자 자격 증명** 섹션에 있는 **테 넌 트 URL** 필드에 ** \<tenantURL\> /scim/v2** 를 입력 합니다. **L** 는 Elium 관리 콘솔에서 이전에 검색 한 값입니다. 또한 **비밀 토큰** 필드에 Elium **secret token** 값을 입력 합니다. 마지막으로 **연결 테스트** 를 선택 하 여 Azure AD가 Elium에 연결할 수 있는지 확인 합니다. 연결에 실패 하면 Elium 계정에 관리자 권한이 있는지 확인 하 고 다시 시도 합니다.

    ![관리자 자격 증명의 테 넌 트 URL 및 암호 토큰 필드](common/provisioning-testconnection-tenanturltoken.png)

1. **알림 전자 메일** 필드에 프로 비전 오류 알림을 받을 개인 또는 그룹의 전자 메일 주소를 입력 합니다. 그런 다음 **오류가 발생 하면 전자 메일 알림 보내기** 확인란을 선택 합니다.

    ![알림 이메일](common/provisioning-notification-email.png)

1. **Save**을 클릭합니다.

1. **매핑** 섹션에서 **Azure Active Directory 사용자를 Elium에 동기화를**선택 합니다.

    ![Azure AD 사용자를 Elium에 매핑하기 위한 동기화 링크](media/Elium-provisioning-tutorial/usermapping.png)

1. **특성 매핑** 섹션에서 Azure AD에서 Elium로 동기화 되는 사용자 특성을 검토 합니다. **일치** 속성으로 선택한 특성은 업데이트 작업을 위해 Elium의 사용자 계정을 일치 시키는 데 사용 됩니다. 변경 내용을 커밋하려면 **저장**을 선택합니다.

    ![Azure AD와 Elium 간의 특성 매핑](media/Elium-provisioning-tutorial/userattribute.png)

1. 범위 지정 필터를 구성 하려면 [범위 지정 필터 자습서](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)의 지침을 따르세요.

1. Elium에 대 한 Azure AD 프로 비전 서비스를 사용 하도록 **설정 하려면 설정** 섹션에서 **프로 비전 상태** 를 **켜기** 로 변경 합니다.

    ![프로 비전 상태를 켜기로 설정](common/provisioning-toggle-on.png)

1. **설정** 섹션의 **범위** 드롭다운 목록 상자에서 원하는 값을 선택 하 여 Elium에 프로 비전 할 사용자 및 그룹을 정의 합니다.

    ![프로 비전 범위 목록 상자](common/provisioning-scope.png)

1. 프로비저닝할 준비가 되면 **저장**을 선택합니다.

    ![프로 비전 구성의 저장 단추](common/provisioning-configuration-save.png)

이 작업을 수행 하면 **설정** 섹션의 **범위** 에 정의 된 모든 사용자 및 그룹의 초기 동기화가 시작 됩니다. 이 초기 동기화 프로세스는 나중에 동기화 되는 것 보다 오래 걸립니다. 프로 비전에 필요한 시간에 대 한 자세한 내용은 [사용자를 프로 비전 하](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users)는 데 얼마나 걸립니까?를 참조 하세요.

**현재 상태** 섹션을 사용 하 여 진행률을 모니터링 하 고 프로 비전 활동 보고서에 대 한 링크를 따릅니다. 프로 비전 활동 보고서는 Elium의 Azure AD 프로 비전 서비스에서 수행 하는 모든 작업을 설명 합니다. 자세한 내용은 [사용자 프로비저닝 상태 확인](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md)을 참조하세요. Azure AD 프로 비전 로그를 읽으려면 [자동 사용자 계정 프로 비전에 대 한 보고](../app-provisioning/check-status-user-account-provisioning.md)를 참조 하세요.

## <a name="additional-resources"></a>추가 리소스

* [엔터프라이즈 앱에 대 한 사용자 계정 프로 비전 관리](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>다음 단계

* [프로비저닝 작업에 대한 로그를 검토하고 보고서를 받아보는 방법을 알아봅니다](../app-provisioning/check-status-user-account-provisioning.md).
