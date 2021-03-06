---
title: Privileged Identity Management에서 관리할 수 없는 역할 Azure Active Directory | Microsoft Docs
description: Azure AD PIM(Privileged Identity Management)에서 관리할 수 없는 역할을 설명합니다.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 05/11/2020
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: c72587c5486ed61215fd20c215a1dd194f4b7bc4
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92372415"
---
# <a name="roles-you-cant-manage-in-privileged-identity-management"></a>Privileged Identity Management에서 관리할 수 없는 역할

Azure Active Directory (Azure AD) Privileged Identity Management (PIM)를 사용 하 여 모든 [AZURE ad 역할](../roles/permissions-reference.md) 및 모든 [azure 역할](../../role-based-access-control/built-in-roles.md)을 관리할 수 있습니다. 또한 Azure 역할에는 관리 그룹, 구독, 리소스 그룹 및 리소스에 연결 된 사용자 지정 역할이 포함 될 수 있습니다. 그러나 관리할 수 없는 몇 가지 역할이 있습니다. 이 문서에서는 Privileged Identity Management에서 관리할 수 없는 역할에 대해 설명 합니다.

## <a name="classic-subscription-administrator-roles"></a>클래식 구독 관리자 역할

Privileged Identity Management에서 다음과 같은 클래식 구독 관리자 역할을 관리할 수 없습니다.

- 계정 관리자
- 서비스 관리자
- 공동 관리자

클래식 구독 관리자 역할에 대 한 자세한 내용은 [클래식 구독 관리자 역할, azure 역할 및 AZURE AD 관리자 역할](../../role-based-access-control/rbac-and-directory-admin-roles.md)을 참조 하세요.

## <a name="what-about-microsoft-365-admin-roles"></a>Microsoft 365 관리 역할은 무엇 인가요?

Exchange 관리자 및 SharePoint 관리자와 같은 Azure AD 역할 및 관리자 포털 환경에서 모든 Microsoft 365 역할을 지원 하지만 Exchange RBAC 또는 SharePoint RBAC 내의 특정 역할은 지원 하지 않습니다. 이러한 Microsoft 365 서비스에 대 한 자세한 내용은 [Microsoft 365 관리자 역할](/office365/admin/add-users/about-admin-roles)을 참조 하세요.

> [!NOTE]
> SharePoint 관리자 역할, 장치 관리자 역할 및 Microsoft 보안 및 규정 준수 센터에 액세스 하려는 모든 역할에 적합 한 사용자는 해당 역할을 활성화 한 후 최대 몇 시간이 지연 될 수 있습니다. 문제를 해결 하기 위해 이러한 팀과 함께 작업 하 고 있습니다.

## <a name="next-steps"></a>다음 단계

- [Privileged Identity Management에서 Azure AD 역할 할당](pim-how-to-add-role-to-user.md)
- [Privileged Identity Management에서 Azure 리소스 역할 할당](pim-resource-roles-assign-roles.md)