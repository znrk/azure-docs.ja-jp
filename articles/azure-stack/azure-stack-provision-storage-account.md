---
title: Storage accounts in Azure Stack | Microsoft Docs
description: Learn how to create an Azure Stack storage account.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: e1152110-b756-4c1a-9fa2-73fe3ab0ad8e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/26/2016
ms.author: erikje
translationtype: Human Translation
ms.sourcegitcommit: 0aee1c31876060777c32c768e0767a39de86cb2a
ms.openlocfilehash: 4fee57fbf1c23b68f9984c9ca8c8f506d137bcca


---
# <a name="storage-accounts-in-azure-stack"></a>Storage accounts in Azure Stack
Storage accounts include Blob and Table services, and the unique namespace for your storage data objects. By default, the data in your account is available only to you, the storage account owner.

1. On the Azure Stack POC computer, log in to `https://portal.azurestack.local` as [an admin](azure-stack-connect-azure-stack.md), and then click **New** > **Data + Storage** > **Storage account**.

   ![](media/azure-stack-provision-storage-account/image01.png)
2. In the **Create storage account** blade, type a name for your storage account. Create a new **Resource Group**, or select an existing one, then click **Create** to create the storage account.

   ![](media/azure-stack-provision-storage-account/image02.png)
3. To see your new storage account, click **All resources**, then search for the storage account and click its name.

    ![](media/azure-stack-provision-storage-account/image03.png)

## <a name="next-steps"></a>Next steps
[Use Azure Resource Manager templates](azure-stack-arm-templates.md)

[Learn about Azure storage accounts](../storage/storage-create-storage-account.md)

[Download the Azure Stack Azure-consistent Storage Validation Guide](http://aka.ms/azurestacktp1doc)



<!--HONumber=Nov16_HO2-->


