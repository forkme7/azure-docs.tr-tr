---
title: İstenen durum Yapılandırması'nı kullanarak Azure için kimlik bilgilerini geçirmek | Microsoft Docs
description: PowerShell istenen durum yapılandırması (DSC) kullanarak Azure sanal makineleri için kimlik bilgilerini güvenli bir şekilde geçirmek öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: zjalexander
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: dsc
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/17/2018
ms.author: zachal,migreene
ms.openlocfilehash: f372685692c2f02984bf0e0b8deeae27ce94422b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="pass-credentials-to-the-azure-dscextension-handler"></a>Kimlik bilgileri Azure DSCExtension işleyicisine geçirin
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Bu makalede, Azure için istenen durum Yapılandırması'nı (DSC) uzantısı yer almaktadır. DSC uzantı işleyici genel bakış için bkz: [Azure istenen durum yapılandırması uzantısı işleyici giriş](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="pass-in-credentials"></a>Kimlik bilgilerinde geçirin

Yapılandırma işleminin bir parçası olarak gerektiğinde kullanıcı hesaplarını, ayarlamak için hizmetlere erişmek veya kullanıcı bağlamında bir program yüklemek. Bu işlemler yapmak için kimlik bilgilerini sağlamanız gerekir.

DSC parametreli yapılandırmalarını ayarlamak için kullanabilirsiniz. Parametreli bir yapılandırmada, kimlik bilgileri yapılandırmaya geçirilen ve güvenli .mof dosyalarında depolanır. Azure uzantısı işleyici sertifikaların otomatik yönetimi sağlayarak kimlik bilgileri yönetimi basitleştirir.

Şu DSC yapılandırma betiği belirtilen parola ile yerel bir kullanıcı hesabı oluşturur:

```powershell
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )
    Node localhost {
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        }
    }
}
```

Eklenmesi önemlidir **düğümü localhost** yapılandırmasının bir parçası olarak. Uzantı işleyici özellikle arar **düğümü localhost** deyimi. Bu bildirimi eksikse, aşağıdaki adımları çalışmıyor. Typecast eklemek önemlidir **[PsCredential]**. Bu belirli tür kimlik bilgilerini şifrelemek için uzantı tetikler.

Bu komut dosyası Azure Blob depolama alanına yayımlamak için:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Azure DSC uzantısı ayarlamak ve kimlik bilgilerini sağlamak için:

```powershell
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```

## <a name="how-a-credential-is-secured"></a>Bir kimlik bilgisi güvenliği nasıl sağlanır

Bu kodu çalıştırmak için kimlik bilgilerini ister. Kimlik bilgisi sağlandıktan sonra kısaca bellekte depolanır. Ne zaman kimlik bilgisi yayımlanır kullanarak **kümesi AzureVmDscExtension** cmdlet, kimlik bilgisi VM HTTPS üzerinden aktarılır. VM'nin Azure yerel VM sertifika kullanarak diskte şifrelenmiş kimlik bilgileri depolar. Kimlik bilgisi kısaca bellekte şifresi çözülür ve ardından DSC için geçirmek için yeniden şifrelenir.

Bu işlem farklıdır [uzantısı işleyici olmadan güvenli yapılandırmaları kullanarak](https://msdn.microsoft.com/powershell/dsc/securemof). Azure ortamı yapılandırma verilerinin sertifikalar yoluyla güvenli bir şekilde iletmek için bir yöntem sunar. DSC uzantı işleyicisi kullandığınızda, sağlamanız gerekmez **$CertificatePath** veya **$CertificateID**/ **$Thumbprint** girişi**ConfigurationData**.

## <a name="next-steps"></a>Sonraki adımlar

* Alma bir [Azure DSC uzantısı işleyici giriş](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* İncelemek [DSC uzantısı için Azure Resource Manager şablonu](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* PowerShell DSC hakkında daha fazla bilgi için Git [PowerShell Belge Merkezi](https://msdn.microsoft.com/powershell/dsc/overview).
* PowerShell DSC kullanarak yönetebilmeniz için daha fazla işlevsellik ve daha fazla DSC kaynakları Gözat [PowerShell Galerisi](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).
