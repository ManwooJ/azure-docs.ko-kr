---
title: 감사 로그에 액세스-Azure CLI-Azure Database for MariaDB
description: 이 문서에서는 Azure CLI에서 Azure Database for MariaDB의 감사 로그를 구성 하 고 액세스 하는 방법을 설명 합니다.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 6/24/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 0aba88c10304cf7d87277ad851ae38eae8eb5bf3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87497123"
---
# <a name="configure-and-access-azure-database-for-maria-db-audit-logs-in-the-azure-cli"></a>Azure CLI에서 민 DB 감사 로그에 대해 Azure Database 구성 및 액세스

Azure CLI에서 [Azure Database for MariaDB 감사 로그](concepts-audit-logs.md) 를 구성할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법 가이드를 단계별로 실행하려면 다음이 필요합니다.

- [Azure Database for MariaDB 서버](quickstart-create-mariadb-server-database-using-azure-portal.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> 이 방법 가이드에서는 Azure CLI 버전 2.0 이상을 사용해야 합니다. 버전을 확인하려면 Azure CLI 명령 프롬프트에서 `az --version`을 입력합니다. 설치하거나 업그레이드하려면 [Azure CLI 설치]( /cli/azure/install-azure-cli)를 참조하세요.

## <a name="configure-audit-logging"></a>감사 로깅 구성

>[!IMPORTANT]
> 서버 성능이 크게 영향을 받지 않도록 감사 목적에 필요한 이벤트 유형과 사용자를 기록 하는 것이 좋습니다.

다음 단계를 사용 하 여 감사 로깅을 설정 하 고 구성 합니다. 

1. **Audit_logs_enabled** 매개 변수를 "설정"으로 설정 하 여 감사 로그를 설정 합니다. 
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_enabled --resource-group myresourcegroup --server mydemoserver --value ON
    ```

1. **Audit_log_events** 매개 변수를 업데이트 하 여 로깅할 [이벤트 유형을](concepts-audit-logs.md#configure-audit-logging) 선택 합니다.
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_events --resource-group myresourcegroup --server mydemoserver --value "ADMIN,CONNECTION"
    ```

1. **Audit_log_exclude_users** 매개 변수를 업데이트 하 여 로깅에서 제외할 모든 다른 모든 사용자를 추가 합니다. 해당 사용자 이름을 입력 하 여 사용자를 지정 합니다.
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_exclude_users --resource-group myresourcegroup --server mydemoserver --value "azure_superuser"
    ```

1. **Audit_log_include_users** 매개 변수를 업데이트 하 여 로깅에 포함할 특정 Aadb 사용자를 추가 합니다. 해당 사용자 이름을 입력 하 여 사용자를 지정 합니다.
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_include_users --resource-group myresourcegroup --server mydemoserver --value "sampleuser"
    ```

## <a name="next-steps"></a>다음 단계

- 에서 [감사 로그](concepts-audit-logs.md) 에 대해 자세히 알아보세요 Azure Database for MariaDB
- [Azure Portal](howto-configure-audit-logs-portal.md) 에서 감사 로그를 구성 하는 방법을 알아봅니다.
