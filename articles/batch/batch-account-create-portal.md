---
title: 在 Azure 门户中创建批处理帐户 | Microsoft Docs
description: 了解如何在 Azure 门户中创建 Azure Batch 帐户，以便在云中运行大规模并行工作负荷
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 83d97d9ed9c51d59500115c4ee3896d471024999
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/20/2018
ms.locfileid: "34359751"
---
# <a name="create-a-batch-account-with-the-azure-portal"></a>使用 Azure 门户创建 Batch 帐户

> [!div class="op_single_selector"]
> * [Azure 门户](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
>
>

了解如何在 [Azure 门户][azure_portal]中创建 Azure Batch 帐户，以及如何选择适合计算方案的帐户属性。 了解在何处查找重要的帐户属性，例如访问密钥和帐户 URL。

有关批处理帐户和方案的背景，请参阅[功能概述](batch-api-basics.md)。

## <a name="create-a-batch-account"></a>创建批处理帐户

[!INCLUDE [batch-account-mode-include](../../includes/batch-account-mode-include.md)]

1. 登录到 [Azure 门户][azure_portal]。

2. 单击“新建” > “计算” > “批处理服务”。

    ![Marketplace 中的批处理][marketplace_portal]

3. 输入“新 Batch 帐户”设置。 查看以下详细信息。

    ![创建批处理帐户][account_portal]

    a. **帐户名称**：所选名称必须在创建帐户的 Azure 区域中唯一（参见下面的“位置”）。 帐户名只能包含小写字符或数字，且长度必须为 3-24 个字符。

    b. **订阅**：要在其中创建批处理帐户的订阅。 如果只有一个订阅，则默认选择此项。

    c. **资源组**：为新批处理帐户选择现有的资源组，或选择创建一个新组。

    d. **位置**：要在其中创建批处理帐户的 Azure 区域。 只有订阅和资源组支持的区域显示为选项。

    e. **存储帐户**（可选）：与 Batch 帐户关联的 Azure 存储帐户。 建议大多数批处理帐户采用此设置。 有关 Batch 中的存储帐户选项，请参阅 [Batch 功能概述](batch-api-basics.md#azure-storage-account)。 在门户中，选择一个现有存储帐户，或者选择创建一个新帐户。

      ![创建存储帐户][storage_account]

    f. **池分配模式**：对于大多数情况，请接受默认值“Batch 服务”。

4. 单击“创建”  以创建帐户。



## <a name="view-batch-account-properties"></a>查看 Batch 帐户属性
创建帐户后，单击该帐户即可访问其设置和属性。 可以使用左侧菜单访问所有帐户设置和属性。

![Azure 门户中的 Batch 帐户页][account_blade]

* **Batch 帐户名、URL 和密钥**：通过 [Batch API](batch-apis-tools.md#azure-accounts-for-batch-development) 开发应用程序时，需要帐户 URL 和密钥才能访问 Batch 资源。 （Batch 还支持 Azure Active Directory 身份验证。）

    若要查看 Batch 帐户访问信息，请单击“密钥”。

    ![Azure 门户中的 Batch 帐户密钥][account_keys]

* 若要查看与 Batch 帐户关联的存储帐户的名称和密钥，请单击“存储帐户”。

* 若要查看适用于 Batch 帐户的资源配额，请单击“配额”。 有关详细信息，请参阅 [Batch 服务配额和限制](batch-quota-limit.md)。


## <a name="additional-configuration-for-user-subscription-mode"></a>用户订阅模式的其他配置

如果选择在用户订阅模式下创建 Batch 帐户，请在创建帐户前执行以下附加步骤。

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>允许 Azure Batch 访问订阅（一次性操作）
在用户订阅模式下创建第一个 Batch 帐户时，需将订阅注册到 Batch 中。 （如果已执行过此操作，请跳至下一部分。）

1. 登录到 [Azure 门户][azure_portal]。

2. 单击“更多服务” > “订阅”，并单击要用于批处理帐户的订阅。

3. 在“订阅”页中，单击“访问控制(IAM)” > “添加”。

    ![订阅访问控制][subscription_access]

4. 在“添加权限”页上，选择“参与者”角色，然后搜索 Batch API。 搜索每一条字符串，直到找到此 API：
    1. MicrosoftAzureBatch。
    2. Microsoft Azure Batch。 较新的 Azure AD 租户可能使用此名称。
    3. ddbf3205-c6bd-46ae-8127-60eb93363864 是此 Batch API 的 ID。 

5. 找到此 Batch API 后，将其选中并单击“保存”。

    ![添加批处理权限][add_permission]

### <a name="create-a-key-vault"></a>创建密钥保管库
在“用户订阅”模式下，需要的 Azure 密钥保管库与要创建的批处理帐户属于同一资源组。 请确保资源组所在的区域是[提供](https://azure.microsoft.com/regions/services/)批处理的区域，也是订阅所支持的区域。

1. 在 [Azure 门户][azure_portal]中，单击“新建” > “安全性” > “密钥保管库”。

2. 在“创建密钥保管库”页中，输入密钥保管库的名称，并在区域中创建需要用于 Batch 帐户的资源组。 让其余设置保留默认值，并单击“创建”。

以用户订阅模式创建 Batch 帐户时，请使用密钥保管库的资源组，指定“用户订阅”作为池分配模式，然后选择密钥保管库。

## <a name="other-batch-account-management-options"></a>其他 Batch 帐户管理选项
除了使用 Azure 门户外，还可使用以下工具创建和管理 Batch 帐户：

* [批处理 PowerShell cmdlet](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>后续步骤
* 请参阅[批处理功能概述](batch-api-basics.md)，详细了解处理服务的概念和功能。 本文讨论主要 Batch 资源（例如池、计算节点、作业和任务），并提供适用于大规模计算工作负荷的服务功能概述。
* 了解使用[批处理 .NET 客户端库](batch-dotnet-get-started.md)或 [Python](batch-python-tutorial.md) 开发支持批处理的应用程序的基本概念。 这些简介文章介绍了使用批处理服务在多个计算节点上执行工作负荷的可行应用程序，并说明了如何使用 Azure 存储进行工作负荷文件暂存和检索。

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[marketplace_portal]: ./media/batch-account-create-portal/marketplace-batch.png
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch-account-portal.png
[account_keys]: ./media/batch-account-create-portal/batch-account-keys.png
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png

