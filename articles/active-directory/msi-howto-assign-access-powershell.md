---
title: "PowerShell kullanarak bir Azure kaynağı için geçersiz bir MSI erişimi atama"
description: "Adım adım yönergeleri bir kaynakta bir MSI atamak için PowerShell kullanarak başka bir kaynağa erişim."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: bdc6268bd56598b917f51f39e604f858cfe81868
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2018
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-powershell"></a>PowerShell kullanarak bir kaynak için bir yönetilen hizmet kimliği (MSI) erişimi atayın

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Bir Azure kaynağı ile bir MSI yapılandırdıktan sonra herhangi bir güvenlik asıl gibi başka bir kaynak MSI erişim izni verebilirsiniz. Bu örnek PowerShell kullanarak bir Azure depolama hesabı için bir Azure sanal makinenin MSI erişmesini sağlamak nasıl gösterir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Ayrıca, yükleme [Azure PowerShell sürüm 4.3.1](https://www.powershellgallery.com/packages/AzureRM/4.3.1) yapmadıysanız.

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Başka bir kaynağa MSI erişim atamak için RBAC kullanın

Bir Azure kaynağı üzerinde MSI etkinleştirdikten sonra [bir Azure VM gibi](msi-qs-configure-powershell-windows-vm.md):

1. Oturum Azure kullanmaya `Login-AzureRmAccount` cmdlet'i. MSI altında yapılandırdığınız Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

   ```powershell
   Login-AzureRmAccount
   ```
2. Bu örnekte, biz bir depolama hesabı için bir Azure VM erişimi vermiş olursunuz. İlk kullanırız [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm) "biz MSI etkinleştirildiğinde oluşturulduğu myVM", adlı VM için hizmet sorumlusu alınamıyor. Ardından, kullanırız [New-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment) "Okuyucu" erişimi bir depolama hesabı VM vermek için "myStorageAcct" olarak adlandırılan:

    ```powershell
    $spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="troubleshooting"></a>Sorun giderme

Kaynak için MSI kullanılabilir kimlikleri listesinde görünmüyor, MSI doğru etkin olduğunu doğrulayın. Örneğimizde, biz Azure VM'yi dönebilirsiniz [Azure portal](https://portal.azure.com) ve:

- "Yapılandırma" sayfasına bakın ve etkin MSI olun = "Yes."
- "Uzantılarla" sayfasına bakın ve başarılı bir şekilde dağıtılan MSI uzantısı emin olun.

Ya da yanlışsa, kaynakta MSI yeniden dağıtmanız veya dağıtım hatası sorun giderme gerekebilir.

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Azure VM'de MSI etkinleştirmek için bkz: [bir Azure VM yönetilen hizmet kimliği (PowerShell kullanarak MSI) yapılandırma](msi-qs-configure-powershell-windows-vm.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.

