---
title: Oturum açmak için bir Azure VM yönetilen hizmet kimliği kullanma
description: Adım adım yönergeler ve örnekler kullanarak bir Azure VM MSI hizmet sorumlusu betik istemci için oturum açın ve kaynak için erişim.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: ec8c9de6ecd81900c4104abf58ecbe032e43fad9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-sign-in"></a>Oturum açmak için bir Azure VM yönetilen hizmet kimliği (MSI) kullanma 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Bu makalede PowerShell ve CLI komut dosyası örnekleri için hata işleme gibi önemli konularda bir MSI hizmet sorumlusu ve Kılavuzu kullanarak oturum açın sağlar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Bu makalede Azure PowerShell veya Azure CLI örnekler kullanmayı planlıyorsanız, en son sürümünü yüklediğinizden emin olun [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) veya [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Bu makaledeki tüm örnek komut dosyası, komut satırı istemcisi bir MSI özellikli sanal makine üzerinde çalışan varsayar. VM "Bağlan" özelliği Azure portalında uzaktan VM'nize bağlanmak için kullanın. Bir VM'de MSI etkinleştirme hakkında daha fazla bilgi için bkz: [bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](qs-configure-portal-windows-vm.md), veya değişken makaleleri (PowerShell, CLI, şablon veya bir Azure SDK'sını kullanarak). 
> - Kaynak erişim sırasında hataları önlemek için VM'ın MSI en az verilmelidir "Okuyucu" erişim uygun kapsamda (VM veya üzeri) VM üzerinde Azure Resource Manager işlemler izin vermek için. Bkz: [bir yönetilen hizmet kimliği (MSI) erişim Azure Portalı'nı kullanarak bir kaynak atama](howto-assign-access-portal.md) Ayrıntılar için.

## <a name="overview"></a>Genel Bakış

Bir MSI sağlayan bir [hizmet sorumlusu nesnesi](../develop/active-directory-dev-glossary.md#service-principal-object) , olduğu [MSI etkinleştirme sırasında oluşturulan](overview.md#how-does-it-work) VM üzerinde. Hizmet sorumlusu Azure kaynaklarına erişim verilir ve bir kimlik olarak komut dosyası/komut-satırı istemcileri oturum açma için ve kaynak erişimini tarafından kullanılan. Geleneksel olarak, kendi kimlik altında güvenlikli kaynaklara erişmek için bir komut dosyası istemci gerekir:  

   - kayıtlı ve gizli/web istemci uygulaması olarak Azure AD ile rıza
   - (komut dosyasında olasılığı olan) uygulamanın kimlik bilgilerini kullanarak kendi hizmet sorumlusu altında oturum açın

Bu MSI hizmet sorumlusu altında oturum açarak gibi MSI ile komut dosyası istemci artık, aşağıdakilerden birini gerekir. 

## <a name="azure-cli"></a>Azure CLI

Aşağıdaki komut dosyası gösterilmektedir nasıl yapılır:

1. VM'ın MSI hizmet sorumlusu altında Azure AD oturum açın  
2. Azure Resource Manager'ı çağırmaz ve VM'nin hizmet asıl Kimlik Al CLI belirteç edinme/kullanımı sizin için otomatik olarak yönetme mvc'deki. İçin sanal makine adı değiştirdiğinizden emin olun `<VM-NAME>`.  

   ```azurecli
   az login --identity
   
   spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
   echo The MSI service principal ID is $spID
   ```

## <a name="azure-powershell"></a>Azure PowerShell

Aşağıdaki komut dosyası gösterilmektedir nasıl yapılır:

1. VM'ın MSI hizmet sorumlusu altında Azure AD oturum açın  
2. VM hakkında bilgi almak için bir Azure Resource Manager cmdlet'ini çağırın. PowerShell belirteç kullanımı sizin için otomatik olarak yönetme mvc'deki.  

   ```azurepowershell
   Add-AzureRmAccount -identity

   # Call Azure Resource Manager to get the service principal ID for the VM's MSI. 
   $vmInfoPs = Get-AzureRMVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>
   $spID = $vmInfoPs.Identity.PrincipalId
   echo "The MSI service principal ID is $spID"
   ```

## <a name="resource-ids-for-azure-services"></a>Azure Hizmetleri için kaynak kimlikleri

Bkz: [Azure Hizmetleri, destek Azure AD kimlik doğrulamasının](services-support-msi.md#azure-services-that-support-azure-ad-authentication) Azure AD destekleyen ve MSI ile test kaynakları ve bunların ilgili kaynak kimlikleri listesi.

## <a name="error-handling-guidance"></a>Hata işleme Kılavuzu 

Aşağıdaki gibi yanıtları VM'nin MSI değil doğru şekilde yapılandırıldığını gösterebilir:

- PowerShell: *Invoke-WebRequest: uzak sunucuya bağlanılamıyor*
- CLI: *MSI: uygulamasından bir belirteç alınamadı 'http://localhost:50342/oauth2/token' ile bir hata oluştu ' HTTPConnectionPool (ana bilgisayar = 'localhost', bağlantı noktası 50342 =)* 

Azure VM ile Bu hatalardan biri alırsanız, dönmek [Azure portal](https://portal.azure.com) ve:

- Git **yapılandırma** sayfasında ve "Yönetilen hizmet kimliği" "Evet" olarak ayarlandığından emin olun
- Git **uzantıları** sayfasında ve başarıyla dağıtılan MSI uzantısı emin olun.

Ya da yanlışsa, kaynakta MSI yeniden dağıtmanız veya dağıtım hatası sorun giderme gerekebilir. Bkz: [bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](qs-configure-portal-windows-vm.md) VM yapılandırması yardıma ihtiyacınız varsa.

## <a name="related-content"></a>İlgili içerik

- Azure VM'de MSI etkinleştirmek için bkz: [bir VM yönetilen hizmet kimliği (PowerShell kullanarak MSI) yapılandırma](qs-configure-powershell-windows-vm.md), veya [bir VM yönetilen hizmet kimliği (Azure CLI kullanarak MSI) yapılandırma](qs-configure-cli-windows-vm.md)

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.








