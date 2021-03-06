---
title: HC 시리즈 VM 크기 성능
description: Azure에서 HC 시리즈 VM 크기에 대 한 성능 테스트 결과에 대해 알아봅니다.
author: vermagit
ms.service: virtual-machines
ms.topic: article
ms.date: 09/10/2020
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: 34d47e6c10692cc212b6e178e3f9658069b96020
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91995110"
---
# <a name="hc-series-virtual-machine-sizes"></a>HC 시리즈 가상 머신 크기

여러 성능 테스트가 HC 시리즈 크기에서 실행 되었습니다. 다음은 이러한 성능 테스트의 결과 중 일부입니다.

| 작업                                        | HB                    |
|-------------------------------------------------|-----------------------|
| 스트림 조로 묶어                                    | 190 g b/초 (Intel MLC AVX-512)  |
| High-Performance Linpack (HPL)                  | 3520 GigaFLOPS (Rpeak), 2970 GigaFLOPS (Rpeak) |
| RDMA 대기 시간 & 대역폭                        | 1.05 마이크로초, 96.8 g b/초   |
| 로컬 NVMe SSD의 FIO                           | 1.3 m b/초 읽기, 900 m b/초 쓰기 |  
| IOR on 4 Azure 프리미엄 SSD (P30 Managed Disks, RAID0) * *  | 780 m b/초 읽기, 780 m b/쓰기 |

## <a name="mpi-latency"></a>MPI 대기 시간

OSU 마이크로 벤치 마크 제품군의 MPI 대기 시간 테스트를 실행 합니다. 샘플 스크립트는 [GitHub](https://github.com/Azure/azhpc-images/blob/04ddb645314a6b2b02e9edb1ea52f079241f1297/tests/run-tests.sh) 에 있습니다.

```bash
./bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./osu_latency 
```

:::image type="content" source="./media/latency-hc.png" alt-text="Azure HC의 MPI 대기 시간입니다.":::

## <a name="mpi-bandwidth"></a>MPI 대역폭

OSU 마이크로 벤치 마크 제품군의 MPI 대역폭 테스트가 실행 됩니다. 샘플 스크립트는 [GitHub](https://github.com/Azure/azhpc-images/blob/04ddb645314a6b2b02e9edb1ea52f079241f1297/tests/run-tests.sh) 에 있습니다.

```bash
./mvapich2-2.3.install/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./mvapich2-2.3/osu_benchmarks/mpi/pt2pt/osu_bw
```

:::image type="content" source="./media/bandwidth-hc.png" alt-text="Azure HC의 MPI 대기 시간입니다.":::


## <a name="mellanox-perftest"></a>Mellanox Perftest

[Mellanox Perftest 패키지](https://community.mellanox.com/s/article/perftest-package) 에는 대기 시간 (ib_send_lat) 및 대역폭 (ib_send_bw)과 같은 많은 InfiniBand 테스트가 있습니다. 예제 명령은 다음과 같습니다.

```console
numactl --physcpubind=[INSERT CORE #]  ib_send_lat -a
```

## <a name="next-steps"></a>다음 단계

- [Azure Compute 기술 커뮤니티 블로그](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute)에서 최신 발표 및 일부 HPC (고성능 컴퓨팅) 예제 및 결과에 대해 읽어 보세요.
- 실행 중인 HPC 워크 로드에 대 한 높은 수준의 아키텍처 보기는 [Azure의 hpc (고성능 컴퓨팅)](/azure/architecture/topics/high-performance-computing/)를 참조 하세요.
