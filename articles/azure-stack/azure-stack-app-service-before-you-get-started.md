---
title: "Azure yığın uygulama hizmeti dağıtmadan önce | Microsoft Docs"
description: "Azure yığın uygulama hizmeti dağıtmadan önce tamamlamak için adımlar"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2018
ms.author: anwestg
ms.openlocfilehash: 5323fe505adfd9b3495dd85ce41d6f141125184b
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Azure yığın uygulama hizmeti ile çalışmaya başlamadan önce

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

> [!IMPORTANT]
> Azure tümleşik yığını sisteminizi 1802 güncelleştirmesini veya Azure uygulama hizmeti dağıtmadan önce en son Azure yığın Geliştirme Seti dağıtın.
>
>

Azure uygulama hizmeti Azure yığında dağıtmadan önce bu makaledeki önkoşulları tamamlamanız gerekir.

## <a name="download-the-installer-and-helper-scripts"></a>Yükleyici ve yardımcı komut dosyalarını indirme

1. Karşıdan [Azure yığın dağıtım Yardımcısı betikleri uygulama hizmeti](https://aka.ms/appsvconmashelpers).
2. Karşıdan [yükleyici Azure yığın uygulama hizmeti](https://aka.ms/appsvconmasinstaller).
3. Yardımcı betikleri .zip dosyasından dosyaları ayıklayın. Aşağıdaki dosyalar ve klasör yapısı görünür:
   - Common.ps1
   - Create-AADIdentityApp.ps1
   - Create-ADFSIdentityApp.ps1
   - Create-AppServiceCerts.ps1
   - Get-AzureStackRootCert.ps1
   - Remove-AppService.ps1
   - Modüller
     - GraphAPI.psm1

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Hata etki alanları için destek eklendi, Azure yığın 1802 sürümü nedeniyle Azure uygulama hizmeti Azure yığında yeni dağıtımı hata etki alanlarında dağıtılmış ve hataya dayanıklılık sağlamak.  1802 sürümünden önce dağıtılan Azure yığında Azure App Service'in mevcut dağıtımlar için güncelleştirme, lütfen bakın [belgelerine](azure-stack-app-service-fault-domain-update.md) dağıtımı yeniden dengelemeniz nasıl.

Ayrıca Azure App Service, yüksek kullanılabilirlik için Azure yığında gerekli dosya sunucusu ve SQL Server örneği yüksek oranda kullanılabilir bir yapılandırmada dağıtın. 

## <a name="get-certificates"></a>Sertifikaları alma

### <a name="azure-resource-manager-root-certificate-for-azure-stack"></a>Azure yığını için Azure Resource Manager kök sertifikası

Azure yığın tümleşik sistem veya Azure yığın Geliştirme Seti ana bilgisayarda ayrıcalıklı endpoint ulaşabileceği bir makinede azurestack\CloudAdmin çalışan bir PowerShell oturumunda, Get-AzureStackRootCert.ps1 betiği ayıkladığınız klasöründen çalıştırın. yardımcı betikler. Betik uygulama hizmeti sertifikaları oluşturmak için gereken komut dosyası ile aynı klasörde bir kök sertifikası oluşturun.

| Get-AzureStackRootCert.ps1 parameter | Gerekli veya isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| PrivilegedEndpoint | Gerekli | AzS-ERCS01 | Ayrıcalıklı uç noktası |
| CloudAdminCredential | Gerekli | AzureStack\CloudAdmin | Etki alanı hesabı kimlik bilgileri Azure yığın bulut yöneticileri |

### <a name="certificates-required-for-the-azure-stack-development-kit"></a>Azure yığın Geliştirme Seti için gerekli sertifikalar

İlk komut, uygulama hizmeti gereken dört sertifikalar oluşturmak üzere Azure yığın sertifika yetkilisi ile çalışır:

| Dosya adı | Kullanım |
| --- | --- |
| _.appservice.local.azurestack.external.pfx | Uygulama hizmeti varsayılan SSL sertifikası |
| api.appservice.local.azurestack.external.pfx | App Service API SSL sertifikası |
| ftp.appservice.local.azurestack.external.pfx | Uygulama hizmeti yayımcı SSL sertifikası |
| sso.appservice.local.azurestack.external.pfx | Uygulama hizmeti kimliği uygulama sertifika |

Azure yığın Geliştirme Seti konakta komut dosyasını çalıştırın ve PowerShell azurestack\CloudAdmin çalıştırdığınızdan emin olun:

1. Azurestack\AzureStackAdmin çalışan bir PowerShell oturumunda oluşturma AppServiceCerts.ps1 komut dosyası Yardımcısı betikleri ayıkladığınız klasöründen çalıştırın. Komut dosyası, uygulama hizmeti sertifikaları oluşturmak için gereken komut dosyası ile aynı klasörde dört sertifikaları oluşturur.
2. .Pfx dosyaları korumak için bir parola girin ve bunu not edin. Uygulama hizmeti Azure yığın yükleyici girmelisiniz.

#### <a name="create-appservicecertsps1-parameters"></a>Create-AppServiceCerts.ps1 parameters

| Parametre | Gerekli veya isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| pfxPassword | Gerekli | Null | Parola sertifika özel anahtarı korumaya yardımcı olur |
| domainName | Gerekli | local.azurestack.external | Azure yığın bölge ve etki alanı soneki |

### <a name="certificates-required-for-a-production-deployment-of-azure-app-service-on-azure-stack"></a>Azure uygulama hizmeti Azure yığında Üretim dağıtımı için gerekli sertifikalar

Üretim kaynak sağlayıcısındaki çalıştırmak için aşağıdaki dört sertifikaları belirtmeniz gerekir:

#### <a name="default-domain-certificate"></a>Varsayılan etki alanı sertifikası

Varsayılan etki alanı sertifikası ön uç rolüne yerleştirilir. Azure App Service'e kullanıcı uygulamaları joker veya varsayılan etki alanı istekleri için bu sertifikayı kullanın. Sertifika Ayrıca kaynak denetimi işlemleri (Kudu) için kullanılır.

Sertifika .pfx biçiminde olmalıdır ve üç konulu bir joker sertifika olmalıdır. Bu, hem varsayılan etki alanı hem de kaynak denetimi işlemleri için SCM uç noktasının karşılamak üzere bir sertifika sağlar.

| Biçim | Örnek |
| --- | --- |
| \*.appservice. \<bölge\>.\< DomainName\>.\< uzantısı\> | \*.appservice.redmond.azurestack.external |
| \*.scm.appservice.<region>.<DomainName>.<extension> | \*.scm.appservice.redmond.azurestack.external |
| \*.sso.appservice.<region>.<DomainName>.<extension> | \*.sso.appservice.redmond.azurestack.external |

#### <a name="api-certificate"></a>API sertifika

API sertifika yönetim rolünü yerleştirilir. Kaynak sağlayıcısı güvenli API çağrıları yardımcı olmak için kullanır. Yayımlama sertifikası API DNS girişi ile eşleşen bir konu içermelidir.

| Biçim | Örnek |
| --- | --- |
| api.appservice. \<bölge\>.\< DomainName\>.\< uzantısı\> | api.appservice.redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Yayımlama sertifikası

İçerik yüklediklerinde yayımcı rolünün sertifikası için uygulama sahipleri FTPS trafiğinin güvenliğini sağlar. Yayımlama sertifikası FTPS DNS girişi ile eşleşen bir konu içermelidir.

| Biçim | Örnek |
| --- | --- |
| FTP.appservice. \<bölge\>.\< DomainName\>.\< uzantısı\> | ftp.appservice.redmond.azurestack.external |

#### <a name="identity-certificate"></a>Kimlik sertifikası

Kimlik uygulama için sertifika sağlar:

- Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) dizin, Azure yığını ve uygulama hizmeti işlem kaynak sağlayıcısı ile tümleştirmeyi desteklemek arasında tümleştirme.
- Çoklu oturum açma senaryoları için Azure yığında Azure App Service içinde Gelişmiş geliştirici araçları.

Sertifika kimliği şu biçimde eşleşen bir konu içermelidir:

| Biçim | Örnek |
| --- | --- |
| SSO.appservice. \<bölge\>.\< DomainName\>.\< uzantısı\> | sso.appservice.redmond.azurestack.external |

## <a name="virtual-network"></a>Sanal Ağ

Azure uygulama hizmeti Azure yığında sağlar kaynak sağlayıcısı ya dağıtmak için var olan bir sanal ağ veya uygulama hizmeti bir dağıtımının parçası olarak oluşturur.  Varolan bir sanal ağı kullanarak dosya sunucusu ve Azure App Service Azure yığında gerektirdiği SQL Server'a bağlanmak için iç IP'leri kullanımını etkinleştirir.  Sanal ağ aşağıdaki adres aralığı ve alt ağları ile Azure uygulama hizmeti Azure yığında yüklemeden önce yapılandırılması gerekir:

Sanal ağ - /16

Alt ağlar

* ControllersSubnet /24
* ManagementServersSubnet /24
* FrontEndsSubnet /24
* PublishersSubnet /24
* WorkersSubnet /21

## <a name="prepare-the-file-server"></a>Dosya sunucusu hazırlayın

Azure uygulama hizmeti bir dosya sunucusu kullanılmasını gerektirir. Üretim dağıtımları için dosya sunucusu yüksek oranda kullanılabilir ve hataları işleme yeteneğine sahip olacak şekilde yapılandırılması gerekir.

Yalnızca Azure yığın Geliştirme Seti dağıtımları için kullandığınız [örnek Azure Resource Manager dağıtım şablonu](https://aka.ms/appsvconmasdkfstemplate) yapılandırılmış tek düğümlü dosya sunucusu dağıtmak için. Tek düğümlü dosya sunucusu bir çalışma grubunda olacaktır.

>[!IMPORTANT]
> App Service içinde varolan bir sanal ağı dağıtmak seçerseniz, dosya sunucusu ayrı bir alt uygulama hizmetinden içine dağıtılmalıdır.
>

### <a name="provision-groups-and-accounts-in-active-directory"></a>Gruplar ve hesaplar Active Directory'de sağlama

1. Aşağıdaki Active Directory genel güvenlik gruplarını oluşturun:
   - FileShareOwners
   - FileShareUsers
2. Hizmet hesapları olarak aşağıdaki Active Directory hesaplarını oluşturun:
   - FileShareOwner
   - FileShareUser

   Güvenlik en iyi uygulama, bu hesaplar için (ve tüm web rollerinin) kullanıcıları birbirinden farklı ve güçlü kullanıcı adları ve parolalar olması gerekir. Parolaları aşağıdaki koşullarla ayarlayın:
   - Etkinleştirme **parola her zaman geçerli olsun**.
   - Etkinleştirme **kullanıcı parolayı değiştiremez**.
   - Devre dışı **kullanıcı bir sonraki oturumda parola değiştirmeli**.
3. Hesapları grup üyeliklerine aşağıdaki gibi ekleyin:
   - Ekleme **FileShareOwner** için **FileShareOwners** grubu.
   - Ekleme **FileShareUser** için **FileShareUsers** grubu.

### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Gruplar ve hesaplar bir çalışma grubunda sağlama

>[!NOTE]
> Bir dosya sunucusunu yapılandırırken bir yönetici komut istemi penceresinde aşağıdaki komutları çalıştırın. *PowerShell kullanmayın.*

Azure Resource Manager şablonunu kullandığınızda, kullanıcılar zaten oluşturulur.

1. FileShareOwner ve FileShareUser hesapları oluşturmak için aşağıdaki komutları çalıştırın. Değiştir `<password>` kendi değerlere sahip.
    ``` DOS
    net user FileShareOwner <password> /add /expires:never /passwordchg:no
    net user FileShareUser <password> /add /expires:never /passwordchg:no
    ```
2. Aşağıdaki WMIC aşağıdaki komutlarını çalıştırarak dolmayacak hesaplar için parolaları ayarlayın:
    ``` DOS
    WMIC USERACCOUNT WHERE "Name='FileShareOwner'" SET PasswordExpires=FALSE
    WMIC USERACCOUNT WHERE "Name='FileShareUser'" SET PasswordExpires=FALSE
    ```
3. FileShareUsers ve FileShareOwners yerel gruplarını oluşturun ve hesapları ilk adımda bunlara ekleyin:
    ``` DOS
    net localgroup FileShareUsers /add
    net localgroup FileShareUsers FileShareUser /add
    net localgroup FileShareOwners /add
    net localgroup FileShareOwners FileShareOwner /add
    ```

### <a name="provision-the-content-share"></a>İçerik paylaşımı sağlama

İçerik paylaşımı Kiracı Web sitesi içeriğini içerir. Tek dosya sunucusunda içerik paylaşımı sağlama yordamı, Active Directory ve çalışma grubu ortamları için aynıdır. Ancak, Active Directory'deki bir yük devretme kümesi için farklıdır.

#### <a name="provision-the-content-share-on-a-single-file-server-active-directory-or-workgroup"></a>(Active Directory veya çalışma grubu) tek dosya sunucusunda içerik paylaşımı sağlama

Tek dosya sunucusunda, yükseltilmiş bir komut isteminde aşağıdaki komutları çalıştırın. Değeri Değiştir `C:\WebSites` ortamınızdaki karşılık gelen yollarla.

```DOS
set WEBSITES_SHARE=WebSites
set WEBSITES_FOLDER=C:\WebSites
md %WEBSITES_FOLDER%
net share %WEBSITES_SHARE% /delete
net share %WEBSITES_SHARE%=%WEBSITES_FOLDER% /grant:Everyone,full
```

### <a name="add-the-fileshareowners-group-to-the-local-administrators-group"></a>FileShareOwners grubunu yerel Administrators grubuna ekleyin

Windows düzgün çalışması için uzaktan yönetimi için yerel Administrators grubuna FileShareOwners grubunu eklemeniz gerekir.

#### <a name="active-directory"></a>Active Directory

Dosya sunucusu veya bir yük devretme kümesi düğümü olarak davranan her dosya sunucusunda aşağıdaki komutları yükseltilmiş bir komut isteminde çalıştırın. Değeri Değiştir `<DOMAIN>` kullanmak istediğiniz etki alanı adına sahip.

```DOS
set DOMAIN=<DOMAIN>
net localgroup Administrators %DOMAIN%\FileShareOwners /add
```

#### <a name="workgroup"></a>Çalışma grubu

Aşağıdaki komutu yükseltilmiş bir komut isteminde dosya sunucusunda çalıştırın:

```DOS
net localgroup Administrators FileShareOwners /add
```

### <a name="configure-access-control-to-the-shares"></a>Paylaşımlara erişim denetimini yapılandırın

Aşağıdaki komutlar, dosya sunucusunda veya geçerli küme kaynak sahibi yük devretme kümesi düğümünde yükseltilmiş bir komut isteminde çalıştırın. İtalik değerleri ortamınıza özgü değerlerle değiştirin.

#### <a name="active-directory"></a>Active Directory

```DOS
set DOMAIN=<DOMAIN>
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

#### <a name="workgroup"></a>Çalışma grubu

```DOS
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

## <a name="prepare-the-sql-server-instance"></a>SQL Server örneği hazırlamak

Azure uygulama hizmeti Azure yığın barındırma ve ölçüm veritabanları, uygulama hizmeti veritabanlarını tutmak için bir SQL Server örneği hazırlamanız gerekir.

Azure yığın Geliştirme Seti dağıtımları için SQL Server Express 2014 SP2 kullanabilirsiniz veya sonraki bir sürümü.

Üretim ve yüksek kullanılabilirlik amaçları için SQL Server 2014 SP2 tam bir sürümünü kullanmanız veya daha sonra karma mod kimlik doğrulamasını etkinleştirmek ve gerekir dağıtmanız bir [yüksek oranda kullanılabilir yapılandırma](https://docs.microsoft.com/sql/sql-server/failover-clusters/high-availability-solutions-sql-server).

SQL Server örneği Azure yığında Azure App Service için tüm uygulama hizmeti rollerden erişilebilir olması gerekir. SQL Server'ın Azure yığınında varsayılan sağlayıcı abonelik içinde dağıtabilirsiniz. Veya yapabilirsiniz, kuruluşunuzdaki mevcut altyapısının (var olduğu sürece Azure yığın bağlantı) kullanın. Bir Azure Market görüntüsü kullanıyorsanız, güvenlik duvarı uygun şekilde yapılandırmak unutmayın.

SQL Server rollerini birini için varsayılan bir örnek veya adlandırılmış bir örnek kullanabilirsiniz. Adlandırılmış bir örnek kullanırsanız, el ile SQL Server Browser hizmetini başlatmak ve bağlantı noktası 1434 açmak emin olun.

>[!IMPORTANT]
> App Service içinde varolan bir sanal ağı dağıtmak isterseniz SQL Server uygulama hizmeti ve dosya sunucusu ayrı bir alt ağ içinde dağıtılmalıdır.
>

## <a name="create-an-azure-active-directory-application"></a>Azure Active Directory Uygulama oluşturma

Aşağıdakileri desteklemek için bir Azure AD hizmet sorumlusu yapılandırın:

- Sanal makine ölçek tümleştirme üzerinde çalışan katmanları ayarlayın.
- SSO için Azure işlevleri Geliştirici Portalı ve Gelişmiş araçlar.

Bu adımlar, yalnızca Azure AD güvenlikli Azure yığın ortamlar için geçerlidir.

Yöneticiler için SSO yapılandırmanız gerekir:

- Uygulama Hizmeti (Kudu) içinde Gelişmiş geliştirici araçları sağlar.
- Azure işlevleri portal deneyimi kullanımını etkinleştirin.

Şu adımları uygulayın:

1. Bir PowerShell örneği azurestack\AzureStackAdmin açın.
2. Yüklediğiniz ve içinde ayıklanan komut dosyalarının konumuna gidin [önkoşul adım](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [PowerShell için Azure Yığın Yükle](azure-stack-powershell-install.md).
4. Çalıştırma **oluşturma AADIdentityApp.ps1** komut dosyası. İstendiğinde, Azure yığın dağıtımınız için kullanmakta olduğunuz Azure AD Kiracı kimliği girin. Örneğin **myazurestack.onmicrosoft.com**.
5. İçinde **kimlik bilgisi** penceresinde, Azure AD hizmet yönetici hesabı ve parolasını girin. **Tamam**’ı seçin.
6. Sertifika dosyası yolu ve sertifika parolasını girin [daha önce oluşturduğunuz sertifika](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Varsayılan olarak bu adım için oluşturulan sertifika **sso.appservice.local.azurestack.external.pfx**.
7. Betik Kiracı Azure AD örneğinde yeni bir uygulama oluşturur. PowerShell çıkışı döndürülen uygulama Kimliğini not edin. Yükleme sırasında bu bilgileri gerekir.
8. Yeni bir tarayıcı penceresi açın ve oturum [Azure portal](https://portal.azure.com) Azure Active Directory Hizmet Yöneticisi olarak
9. Azure AD kaynak sağlayıcısı açın.
10. Seçin **uygulama kayıtlar**.
11. Adım 7 bir parçası olarak döndürülen uygulama kimliği arayın. Bir uygulama hizmeti uygulaması listelenir.
12. Seçin **uygulama** listesinde.
13. Tıklatın **ayarları**.
14. Seçin **gerekli izinleri** > **izinleri verin** > **Evet**.

| Create-AADIdentityApp.ps1  parameter | Gerekli veya isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| DirectoryTenantName | Gerekli | Null | Azure AD Kiracı kimliği. GUID veya dize sağlayın. Myazureaaddirectory.onmicrosoft.com örneğidir. |
| AdminArmEndpoint | Gerekli | Null | Yönetici Azure Resource Manager uç noktası. Adminmanagement.local.azurestack.external örneğidir. |
| TenantARMEndpoint | Gerekli | Null | Kiracı Azure Resource Manager uç noktası. Management.local.azurestack.external örneğidir. |
| AzureStackAdminCredential | Gerekli | Null | Azure AD hizmet yönetici kimlik bilgileri. |
| CertificateFilePath | Gerekli | Null | Daha önce oluşturulan kimlik uygulama sertifika dosyasının yolu. |
| CertificatePassword | Gerekli | Null | Parola sertifika özel anahtarı korumaya yardımcı olur. |

## <a name="create-an-active-directory-federation-services-application"></a>Active Directory Federasyon Hizmetleri uygulama oluşturma

AD FS tarafından güvenliği sağlanan Azure yığın ortamlar için aşağıdakileri desteklemek için bir AD FS hizmet sorumlusu yapılandırmanız gerekir:

- Sanal makine ölçek tümleştirme üzerinde çalışan katmanları ayarlayın.
- SSO için Azure işlevleri Geliştirici Portalı ve Gelişmiş araçlar.

Yöneticiler için SSO yapılandırmanız gerekir:

- Sanal makine ölçek kümesi tümleştirmesi için bir hizmet sorumlusu üzerinde çalışan katmanları yapılandırın.
- Uygulama Hizmeti (Kudu) içinde Gelişmiş geliştirici araçları sağlar.
- Azure işlevleri portal deneyimi kullanımını etkinleştirin.

Şu adımları uygulayın:

1. Bir PowerShell örneği azurestack\AzureStackAdmin açın.
2. Yüklediğiniz ve içinde ayıklanan komut dosyalarının konumuna gidin [önkoşul adım](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [PowerShell için Azure Yığın Yükle](azure-stack-powershell-install.md).
4. Çalıştırma **oluşturma ADFSIdentityApp.ps1** komut dosyası.
5. İçinde **kimlik bilgisi** penceresinde, AD FS bulut yönetici hesabı ve parolasını girin. **Tamam**’ı seçin.
6. Sertifika dosyası yolu ve sertifika parolasını sağlamak [daha önce oluşturduğunuz sertifika](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Varsayılan olarak bu adım için oluşturulan sertifika **sso.appservice.local.azurestack.external.pfx**.

| Oluşturma ADFSIdentityApp.ps1 parametresi | Gerekli veya isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| AdminArmEndpoint | Gerekli | Null | Yönetici Azure Resource Manager uç noktası. Adminmanagement.local.azurestack.external örneğidir. |
| PrivilegedEndpoint | Gerekli | Null | Ayrıcalıklı uç noktası. AzS ERCS01 örneğidir. |
| CloudAdminCredential | Gerekli | Null | Etki alanı hesabı Azure yığın bulut admins kimlik bilgileri. Azurestack\CloudAdmin örneğidir. |
| CertificateFilePath | Gerekli | Null | Kimlik uygulamanın Sertifika PFX dosyasının yolu. |
| CertificatePassword | Gerekli | Null | Parola sertifika özel anahtarı korumaya yardımcı olur. |

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama hizmeti kaynak Sağlayıcısı'nı yüklemek](azure-stack-app-service-deploy.md)
