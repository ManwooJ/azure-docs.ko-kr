---
title: 지원 되는 버전-Azure Database for MySQL
description: Azure Database for MySQL 서비스에서 지원 되는 MySQL server 버전을 알아봅니다.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 6/3/2020
ms.openlocfilehash: 4d5b858e2384ffc7dd531444aaff17ca3739b408
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "84337702"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>지원되는 MySQL용 Azure 데이터베이스 서버 버전

Azure Database for MySQL은 [MySQL Community Edition](https://www.mysql.com/products/community/)에서 InnoDB 엔진을 사용하여 개발되었습니다.

MySQL은 X.Y.Z 명명 체계를 사용합니다. X는 주 버전, Y는 부 버전이며, Z는 버그 수정 릴리스입니다. 스키마에 대한 자세한 내용은 [MySQL 설명서](https://dev.mysql.com/doc/refman/5.7/en/which-version.html)를 참조하세요.

> [!NOTE]
> 서비스에서 게이트웨이를 사용하여 새 인스턴스로 연결을 리디렉션합니다. 연결이 설정되면 MySQL 클라이언트는 MySQL Server 인스턴스에서 실행 중인 실제 버전이 아닌 게이트웨이에서 설정된 MySQL 버전을 표시합니다. MySQL Server 인스턴스의 버전을 확인하려면 MySQL 프롬프트에서 `SELECT VERSION();` 명령을 사용합니다.

Azure Database for MySQL은 현재 다음 버전을 지원합니다.

## <a name="mysql-version-56"></a>MySQL 버전 5.6

버그 수정 릴리스: 5.6.47

이 버전의 향상 된 기능 및 수정 내용에 대 한 자세한 내용은 MySQL [릴리스 정보](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-47.html) 를 참조 하세요.

## <a name="mysql-version-57"></a>MySQL 버전 5.7

버그 수정 릴리스: 5.7.29

이 버전의 향상 된 기능 및 수정 내용에 대 한 자세한 내용은 MySQL [릴리스 정보](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-29.html) 를 참조 하세요.

## <a name="mysql-version-80"></a>MySQL 버전 8.0

버그 수정 릴리스: 8.0.15

이 버전의 향상 된 기능 및 수정 내용에 대 한 자세한 내용은 MySQL [릴리스 정보](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-15.html) 를 참조 하세요.

## <a name="managing-updates-and-upgrades"></a>업데이트 및 업그레이드 관리
서비스는 버그 수정 버전 업데이트에 대한 패치를 자동으로 관리합니다. 예: 5.7.20~5.7.21  

현재 주 및 부 버전 업그레이드는 지원되지 않습니다. 예를 들어 MySQL 5.6에서 MySQL 5.7로의 업그레이드는 지원되지 않습니다. 5.6에서 5.7로 업그레이드하려는 경우 새 엔진 버전을 사용하여 만든 서버에 부 버전을 [덤프 및 복원](./concepts-migrate-dump-restore.md)합니다.

## <a name="next-steps"></a>다음 단계

**서비스 계층**에 따른 특정 리소스 할당량 및 제한 사항에 대한 자세한 내용은 [서비스 계층](./concepts-pricing-tiers.md)을 참조하세요.
