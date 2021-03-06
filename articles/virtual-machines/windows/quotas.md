---
title: Azure 的 vCPU 配额 | Microsoft Docs
description: 了解 Azure 的 vCPU 配额。
keywords: ''
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: cynthn
ms.openlocfilehash: 50f28fcb2d7945949134d89f6bcc4238cf625bd1
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2018
---
# <a name="virtual-machine-vcpu-quotas"></a>虚拟机 vCPU 配额

对于每个区域中的每个订阅，虚拟机和虚拟机规模集的 vCPU 配额被安排在两个层中。 第一层是区域的 vCPU 总数，第二层是各种 VM 大小系列核心（如标准 D 系列 vCPU）。 每当部署新 VM 时，新部署的 VM 的 vCPU 数不能超过特定 VM 大小系列的 vCPU 配额或区域 vCPU 配额总数。 如果超过了上述任一配额，将不允许部署 VM。 此外，区域中还有虚拟机总数的配额。 有关上述每个配额的详细信息，可以在 [Azure 门户](https://portal.azure.com)的“订阅”页的“使用情况 + 配额”部分中查看，也可以使用 PowerShell 查询各值。

 
## <a name="check-usage"></a>查看使用情况

可以使用 [Get-AzureRmVMUsage](/powershell/module/azurerm.compute/get-azurermvmusage) cmdlet 来检查配额的使用情况。

```azurepowershell-interactive
Get-AzureRmVMUsage -Location "East US"
```

输出与此类似：

```
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional vCPUs                         4   260 Count
Virtual Machines                             4 10000 Count
Virtual Machine Scale Sets                   1  2000 Count
Standard B Family vCPUs                      1    10 Count
Standard DSv2 Family vCPUs                   1   100 Count
Standard Dv2 Family vCPUs                    2   100 Count
Basic A Family vCPUs                         0   100 Count
Standard A0-A7 Family vCPUs                  0   250 Count
Standard A8-A11 Family vCPUs                 0   100 Count
Standard D Family vCPUs                      0   100 Count
Standard G Family vCPUs                      0   100 Count
Standard DS Family vCPUs                     0   100 Count
Standard GS Family vCPUs                     0   100 Count
Standard F Family vCPUs                      0   100 Count
Standard FS Family vCPUs                     0   100 Count
Standard NV Family vCPUs                     0    24 Count
Standard NC Family vCPUs                     0    48 Count
Standard H Family vCPUs                      0     8 Count
Standard Av2 Family vCPUs                    0   100 Count
Standard LS Family vCPUs                     0   100 Count
Standard Dv2 Promo Family vCPUs              0   100 Count
Standard DSv2 Promo Family vCPUs             0   100 Count
Standard MS Family vCPUs                     0     0 Count
Standard Dv3 Family vCPUs                    0   100 Count
Standard DSv3 Family vCPUs                   0   100 Count
Standard Ev3 Family vCPUs                    0   100 Count
Standard ESv3 Family vCPUs                   0   100 Count
Standard FSv2 Family vCPUs                   0   100 Count
Standard ND Family vCPUs                     0     0 Count
Standard NCv2 Family vCPUs                   0     0 Count
Standard NCv3 Family vCPUs                   0     0 Count
Standard LSv2 Family vCPUs                   0     0 Count
Standard Storage Managed Disks               2 10000 Count
Premium Storage Managed Disks                1 10000 Count

```


## <a name="reserved-vm-instances"></a>保留 VM 实例
保留 VM 实例（其范围限定为单个订阅）将向 vCPU 配额添加一个新方面。 这些值描述一定能够部署在订阅中的所述大小的实例数。 它们在配额系统中作为占位符使用以确保保留配额，从而确保保留实例可在订阅中部署。 例如，如果特定订阅有 10 个 Standard_D1 保留实例，则 Standard_D1 保留实例的使用量限制将为 10。 这会导致 Azure 确保总区域 vCPU 配额中始终至少有 10 个 vCPU 可用于 Standard_D1 实例，并且标准 D 系列 vCPU 配额中始终至少有 10 个 vCPU 可用于 Standard_D1 实例。

如果需要增加配额来购买单个订阅 RI，则可以在订阅上[请求增加配额](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request)。

## <a name="next-steps"></a>后续步骤

有关计费和配额的详细信息，请参阅 [Azure 订阅和服务限制、配额和约束](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json)。
