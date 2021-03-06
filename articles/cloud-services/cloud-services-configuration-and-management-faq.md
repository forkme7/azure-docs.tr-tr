---
title: Yapılandırması ve yönetimi sorunları için Microsoft Azure bulut Hizmetleri ile ilgili SSS | Microsoft Docs
description: Bu makalede, yapılandırması ve yönetimi için Microsoft Azure bulut hizmetleri hakkında sık sorulan sorular listelenmektedir.
services: cloud-services
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: genli
ms.openlocfilehash: cc43d1a0e07eac78a47e6f183c2fd066a489f4f4
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure bulut Hizmetleri için yapılandırma ve yönetme sorununu: sık sorulan sorular (SSS)

Bu makale için yapılandırması ve yönetimi sorunları hakkında sık sorulan sorular içerir [Microsoft Azure bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services). Ayrıca başvurabilirsiniz [bulut Hizmetleri VM boyutu sayfa](cloud-services-sizes-specs.md) boyutu bilgi.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

**Sertifikalar**

- [Neden my bulut hizmeti SSL sertifikası sertifika zinciri eksik mi?](#why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete)
- ["Windows Azure Araçları şifreleme sertifikası için uzantıları" amacı nedir?](#what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions)
- [Nasıl t bir sertifika imzalama isteği (CSR) "RDP-lık" olmadan içinde örneğine oluşturabilir miyim?](#how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance)
- [Bulut Hizmet Yönetim sertifikası'My süresi doluyor. Yenilemek nasıl?](#my-cloud-service-management-certificate-is-expiring-how-to-renew-it)
- [Ana SSL sertifika(.pfx) ve Ara certificate(.p7b) yüklenmesini otomatikleştirmek için nasıl?](#how-to-automate-the-installation-of-main-ssl-certificatepfx-and-intermediate-certificatep7b)

**İzleme ve günlüğe kaydetme**

- [Yönetmek ve uygulamaları izlemek yardımcı olabilecek Azure portalında yaklaşan bulut hizmeti özellikleri nelerdir?](#what-are-the-upcoming-cloud-service-capabilities-in-the-azure-portal-which-can-help-manage-and-monitor-applications)
- [IIS Günlük dizini yazmak neden Durdur?](#why-does-iis-stop-writing-to-the-log-directory)

**Ağ yapılandırması**

- [Boşta kalma zaman aşımı için Azure yük dengeleyici nasıl ayarlarım?](#how-do-i-set-the-idle-timeout-for-azure-load-balancer)
- [Bulut Hizmetimi nasıl statik bir IP adresi ilişkilendirmek?](#how-do-i-associate-a-static-ip-address-to-my-cloud-service)
- [Azure temel IP'leri/Kimlikleri ve DDOS sağlayan özellikleri ve yetenekleri nelerdir?](#what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides)
- [Bulut Hizmetleri VM üzerinde HTTP/2 etkinleştirmek nasıl?](#how-to-enable-http2-on-cloud-services-vm)

**İzinler**

- [Bulut hizmeti örnekleri izni olmadan Microsoft iç mühendisleri Uzak Masaüstü kullanabilir?](#can-microsoft-internal-engineers-remote-desktop-to-cloud-service-instances-without-permission)
- [RDP dosyasını kullanarak bulut hizmeti VM, Uzak Masaüstü'nü bağlanamıyorum. Hatadan sonra Al: bir kimlik doğrulama hatası oluştu (kod: 0x80004005)](#i-cannot-remote-desktop-to-cloud-service-vm--by-using-the-rdp-file-i-get-following-error-an-authentication-error-has-occurred-code-0x80004005)

**Ölçeklendirme**

- [X ölçeklendirme olamaz örnekleri](#i-cannot-scale-beyond-x-instances)
- [Otomatik ölçek bellek ölçümleri temel nasıl yapılandırabilir miyim?](#how-can-i-configure-auto-scale-based-on-memory-metrics)

**Genel**

- [My Web sitesine nasıl "nosniff" eklensin mi?](#how-do-i-add-nosniff-to-my-website)
- [Web rolü için nasıl IIS özelleştirebilir?](#how-do-i-customize-iis-for-a-web-role)
- [My bulut hizmeti için kota sınırı nedir?](#what-is-the-quota-limit-for-my-cloud-service)
- [Bulut hizmeti VM'im sürücüde çok az boş disk alanı neden Göster?](#why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space)
- [Nasıl bir kötü amaçlı yazılımdan koruma uzantı my bulut Hizmetleri için otomatik bir şekilde ekleyebilirim?](#how-can-i-add-an-antimalware-extension-for-my-cloud-services-in-an-automated-way)
- [Sunucu adı göstergesi (SNI) için bulut hizmetlerini etkinleştirmek nasıl?](#how-to-enable-server-name-indication-sni-for-cloud-services)
- [Etiketler my Azure bulut hizmetine nasıl ekleyebilirim?](#how-can-i-add-tags-to-my-azure-cloud-service)
- [Azure portalında bulut Hizmetim SDK sürümü görüntülemez. Bu sorunu nasıl alabilirim?](#the-azure-portal-doesnt-display-the-sdk-version-of-my-cloud-service-how-can-i-get-that)
- [Bulut hizmeti için birkaç ay kapattığınız istiyorsunuz. IP adresi kaybetmeden fatura bulut hizmeti maliyetini azaltmak nasıl?](#i-want-to-shut-down-the-cloud-service-for-several-months-how-to-reduce-the-billing-cost-of-cloud-service-without-losing-the-ip-address)


## <a name="certificates"></a>Sertifikalar

### <a name="why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Neden my bulut hizmeti SSL sertifikası sertifika zinciri eksik mi?
    
Müşteriler yalnızca Yaprak sertifikası yerine tam sertifika zinciri (Yaprak sertifikası, Ara sertifikaları ve kök sertifika) yüklemenizi öneririz. Yalnızca Yaprak sertifikası yüklediğinizde, CTL adım adım ilerlemenizi sağlayarak sertifika zinciri oluşturmak üzere Windows bağlıdır. Sertifikayı doğrulamak Windows çalışırken aralıklı ağ veya DNS sorunlarına Azure veya Windows Update meydana gelirse, sertifika geçersiz kabul. Tam sertifika zinciri yükleyerek bu sorun önlenebilir. Konumundaki blog [zincirleme bir SSL sertifikasını nasıl yükleyeceğinize](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) bunun nasıl yapılacağı gösterilmektedir.

### <a name="what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions"></a>"Windows Azure Araçları şifreleme sertifikası için uzantıları" amacı nedir?

Bulut hizmeti uzantı eklendiğinde bu sertifikaları otomatik olarak oluşturulur. En yaygın olarak, bu WAD uzantısı veya RDP uzantısı, ancak diğer kötü amaçlı yazılımdan koruma veya günlük Toplayıcı uzantısı gibi olabilir. Bu sertifikalar yalnızca şifreleme ve uzantı özel yapılandırması şifresini çözmek için kullanılır. Sertifikanın süresi dolmuşsa önemli değildir şekilde sona erme tarihini hiçbir zaman denetlenir. 

Bu sertifikalar yoksayabilirsiniz. Sertifikaları temizlemek isterseniz, tüm silerek deneyebilirsiniz. Azure, kullanımda olan bir sertifikayı silmeye çalışırsanız bir hata durum oluşturur.

### <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance"></a>Nasıl t bir sertifika imzalama isteği (CSR) "RDP-lık" olmadan içinde örneğine oluşturabilir miyim?

Aşağıdaki yönergeler belgesine bakın:

[Windows Azure Web siteleri (WAWS) ile kullanılacak bir sertifika edinme](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

CSR yalnızca bir metin dosyasıdır. Bu makineden oluşturulacak sertifika sonuçta nerede kullanılacak yok. Bu belgede bir uygulama hizmeti için yazılmış olsa da CSR oluşturma geneldir ve bulut Hizmetleri için de geçerlidir.

### <a name="my-cloud-service-management-certificate-is-expiring-how-to-renew-it"></a>Bulut Hizmet Yönetim sertifikası'My süresi doluyor. Yenilemek nasıl?

Yönetim sertifikaları yenilemek için PowerShell komutlarını kullanabilirsiniz:

    Add-AzureAccount
    Select-AzureSubscription -Current -SubscriptionName <your subscription name>
    Get-AzurePublishSettingsFile

**Get-AzurePublishSettingsFile** içinde yeni bir yönetim sertifikası oluşturacak **abonelik** > **yönetim sertifikaları** Azure portalında. Yeni sertifika adını "YourSubscriptionNam]-[günün tarihine] - kimlik bilgileri gibi" arar.

### <a name="how-to-automate-the-installation-of-main-ssl-certificatepfx-and-intermediate-certificatep7b"></a>Ana SSL sertifika(.pfx) ve Ara certificate(.p7b) yüklenmesini otomatikleştirmek için nasıl?

Bir başlangıç komut dosyası (toplu işlem/cmd/PowerShell) kullanarak bu görevi otomatikleştirmek ve hizmet tanımı dosyasında bu başlangıç komut dosyasını kaydedin. Başlangıç betiği ve sertifika (.p7b dosyası) başlangıç betiği aynı dizine proje klasöründe ekleyin.

Daha fazla bilgi için aşağıdaki makalelere bakın:
- [Nasıl yapılandırılacağı ve başlangıç görevleri çalıştırmak için bir bulut hizmeti](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks)
- [Genel bulut hizmeti başlangıç görevleri](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks-common)

## <a name="monitoring-and-logging"></a>İzleme ve günlüğe kaydetme

### <a name="what-are-the-upcoming-cloud-service-capabilities-in-the-azure-portal-which-can-help-manage-and-monitor-applications"></a>Yönetmek ve uygulamaları izlemek yardımcı olabilecek Azure portalında yaklaşan bulut hizmeti özellikleri nelerdir?

Uzak Masaüstü Protokolü (RDP) için yeni bir sertifika oluşturma olanağı yakında sunulacaktır. Alternatif olarak, bu komut dosyası çalıştırabilirsiniz:

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 20 48 -KeySpec "KeyExchange"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```
Csdef ve cscfg konumu karşıya yüklemek için blob veya yerel seçme yeteneği yakında çıkıyor. Kullanarak [yeni AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-4.0.0), her bir konum değeri ayarlayabilirsiniz.

Örnek düzeyinde ölçümleri izleme yeteneği. Ek izleme işlevleri kullanılabilir [İzleyici bulut hizmetlerini](cloud-services-how-to-monitor.md).

### <a name="why-does-iis-stop-writing-to-the-log-directory"></a>IIS Günlük dizini yazmak neden Durdur?
Günlük dizini yazmak için yerel depolama kotası azaltılmıştır. Bu sorunu gidermek için işlemlerden birini yapabilirsiniz:
* IIS için tanılamayı etkinleştirin ve tanılama düzenli aralıklarla BLOB depolamaya taşınmış.
* El ile günlük dosyalarını günlük dizininden kaldırın.
* Yerel kaynaklar için kota sınırını artırın.

Daha fazla bilgi için aşağıdaki belgelere bakın:
* [Azure Depolama’daki tanılama verilerini depolama ve görüntüleme](cloud-services-dotnet-diagnostics-storage.md)
* [Bulut hizmetinde yazma IIS günlüklerini Durdur](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/)

## <a name="network-configuration"></a>Ağ yapılandırması

### <a name="how-do-i-set-the-idle-timeout-for-azure-load-balancer"></a>Boşta kalma zaman aşımı için Azure yük dengeleyici nasıl ayarlarım?
Bu gibi hizmet tanımı (csdef) dosyanızdaki zaman aşımı belirtebilirsiniz:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="mgVS2015Worker" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10100"   idleTimeoutInMinutes="30" />
    </Endpoints>
  </WorkerRole>
```
Bkz: [yeni: yapılandırılabilir boşta kalma zaman aşımı Azure yük dengeleyici için](https://azure.microsoft.com/blog/new-configurable-idle-timeout-for-azure-load-balancer/) daha fazla bilgi için.

### <a name="how-do-i-associate-a-static-ip-address-to-my-cloud-service"></a>Bulut Hizmetimi nasıl statik bir IP adresi ilişkilendirmek?
Bir statik IP adresi ayarlamak için ayrılmış bir IP oluşturmanız gerekir. Bu ayrılmış IP yeni bir bulut hizmeti veya var olan bir dağıtıma ilişkili olabilir. Ayrıntılar için aşağıdaki belgelere bakın:
* [Ayrılmış bir IP adresi oluşturma](../virtual-network/virtual-networks-reserved-public-ip.md#manage-reserved-vips)
* [Var olan bir bulut hizmetini, IP adresi ayırın](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
* [Ayrılmış IP'si yeni bir bulut hizmeti ile ilişkilendirme](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-new-cloud-service)
* [Ayrılmış IP'si çalışan dağıtımının ile ilişkilendirme](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-running-deployment)
* [Hizmet yapılandırma dosyası kullanarak bir bulut hizmeti için ayrılmış bir IP ilişkilendirin](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

### <a name="what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Azure temel IP'leri/Kimlikleri ve DDOS sağlayan özellikleri ve yetenekleri nelerdir?
Azure veri merkezi fiziksel sunucuları tehditlere karşı korumak için IP'leri/Kimlikler sahiptir. Buna ek olarak, müşteriler, web uygulaması güvenlik duvarı, ağ güvenlik duvarları, kötü amaçlı yazılımdan koruma, izinsiz giriş algılama, önleme sistemleri (Kimlikleri/IP) ve daha fazla gibi üçüncü taraf güvenlik çözümleri dağıtabilirsiniz. Daha fazla bilgi için bkz: [veri ve varlıkların korumak ve genel güvenlik standartları ile uyum](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity).

Microsoft, sunucuları, ağları ve tehditleri algılamak için uygulamaları sürekli olarak izler. Azure'nın multipronged tehdit Yönetimi yaklaşımı kullanır izinsiz giriş algılama dağıtılmış hizmet reddi (DDoS) saldırı önleme, sızma sınama, davranış analizi, anomali algılama ve makine sürekli kendi savunma güçlendirmek öğrenme ve riskleri azaltın. Azure için Microsoft Antimalware Azure Cloud Services ve sanal makineleri korur. Web uygulaması güvenlik duvarları, ağ güvenlik duvarları, kötü amaçlı yazılımdan koruma, izinsiz giriş algılama ve önleme sistemleri (Kimlikleri/IP) ve daha fazlası gibi ek olarak, üçüncü taraf güvenlik çözümlerini dağıtmak için seçeneğiniz vardır.

### <a name="how-to-enable-http2-on-cloud-services-vm"></a>Bulut Hizmetleri VM üzerinde HTTP/2 etkinleştirmek nasıl?

Windows 10 ve Windows Server 2016 HTTP/2 desteği istemci ve sunucu tarafında gelir. İstemci (tarayıcı) bağlanılıyorsa TLS üzerinden IIS sunucusunu, HTTP/2 TLS uzantıları ile görüşür sonra sunucu tarafında herhangi bir değişiklik yapmak gerekmez. Varsayılan olarak HTTP/2 kullanımını belirtme h2-14 üstbilgi TLS gönderilir, olmasıdır. Öte yandan, istemci HTTP/2'ye yükseltmek için bir yükseltme üstbilgisi gönderiyorsa, yükseltme çalışır ve bir HTTP/2 bağlantısıyla elde emin olmak için sunucu tarafında aşağıda değişiklik yapmanız gerekir. 

1. Regedit.exe çalıştırın.
2. Kayıt defteri anahtarına gidin: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HTTP\Parameters.
3. Adlı yeni bir DWORD değeri oluşturun **DuoEnabled**.
4. Değerini 1 olarak ayarlayın.
5. Sunucunuzu yeniden başlatın.
6. Gidin, **varsayılan Web sitesi** ve altında **bağlamaları**, yeni oluşturduğunuz otomatik olarak imzalanan sertifika ile yeni bir TLS bağlama oluşturun. 

Daha fazla bilgi için bkz.

- [IIS üzerinde HTTP/2](https://blogs.iis.net/davidso/http2)
- [Video: HTTP/2 Windows 10: tarayıcı, uygulamaları ve Web sunucusu](https://channel9.msdn.com/Events/Build/2015/3-88)
         

Böylece yeni bir PaaS örneği oluşturulduğunu olduğunda, yukarıda sistem kayıt defterindeki değişiklikler yapmak için bu adımları bir başlangıç görevi otomatik olarak. Daha fazla bilgi için bkz: [nasıl yapılandırılacağı ve başlangıç görevleri çalıştırmak için bir bulut hizmeti](cloud-services-startup-tasks.md).

 
Bunu yaptıktan sonra HTTP/2 etkin olup olmadığını veya aşağıdaki yöntemlerden birini kullanarak değil doğrulayabilirsiniz:

- IIS günlüklerine Protokolü sürüm etkinleştirmek ve IIS günlüklerine bakın. Günlüklerde, HTTP/2 gösterecektir. 
- F12 Geliştirici aracı Internet Explorer/kenar etkinleştirin ve protokolü doğrulamak için ağ sekmesine geçin. 

Daha fazla bilgi için bkz: [HTTP/2 IIS'de](https://blogs.iis.net/davidso/http2).

## <a name="permissions"></a>İzinler

### <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Nasıl ı bulut Hizmetleri için rol tabanlı erişim uygulayabilirsiniz?
Bulut Hizmetleri bir Azure Resource Manager temelli hizmeti olmadığından rol tabanlı erişim denetimi (RBAC) modelini desteklemez.

Bkz: [Klasik abonelik yöneticileri ve Azure RBAC](../role-based-access-control/overview.md#azure-rbac-vs-classic-subscription-administrators).

## <a name="remote-desktop"></a>Uzak masaüstü

### <a name="can-microsoft-internal-engineers-remote-desktop-to-cloud-service-instances-without-permission"></a>Bulut hizmeti örnekleri izni olmadan Microsoft iç mühendisleri Uzak Masaüstü kullanabilir?
Microsoft Uzak Masaüstü iç mühendisleri bulut hizmetiniz (e-posta veya diğer yazılı iletişimde) yazılı izni olmadan uygulamasına izin vermez katı bir işlem sahibi veya kendi yetkilinin izler.

### <a name="i-cannot-remote-desktop-to-cloud-service-vm--by-using-the-rdp-file-i-get-following-error-an-authentication-error-has-occurred-code-0x80004005"></a>RDP dosyasını kullanarak bulut hizmeti VM, Uzak Masaüstü'nü bağlanamıyorum. Hatadan sonra Al: bir kimlik doğrulama hatası oluştu (kod: 0x80004005)

Azure Active Directory'ye katılmış bir makineden RDP dosyası kullanırsanız, bu hata oluşabilir. Bu sorunu çözmek için şu adımları izleyin:

1. İndirdiğiniz RDP dosyasını sağ tıklatın ve ardından **Düzenle**.
2. Ekle "&#92;" olarak önek önce kullanıcı adı. Örneğin, **. \username** yerine **kullanıcıadı**.

## <a name="scaling"></a>Ölçeklendirme

### <a name="i-cannot-scale-beyond-x-instances"></a>X ölçeklendirme olamaz örnekleri
Azure aboneliğinizi kullanabileceğiniz çekirdek sayısına bir sınır vardır. Kullanılabilir tüm çekirdek kullanılırsa ölçeklendirme çalışmaz. Bir sınır 100 çekirdek varsa, örneğin, bu 100 A1 boyuta sahip sanal makine örneklerini bulut hizmetiniz için olabilir veya sanal makine örnekleri 50 A2 boyutu anlamına gelir.

### <a name="how-can-i-configure-auto-scale-based-on-memory-metrics"></a>Otomatik ölçek bellek ölçümleri temel nasıl yapılandırabilir miyim?

Otomatik ölçek için bulut hizmetlerini bellek ölçülerine bağlı olarak şu anda desteklenmiyor. 

Bu sorunu geçici olarak çözmek için Application Insights kullanabilirsiniz. Otomatik ölçek ölçüm kaynağı olarak Application Insights destekler ve Konuk ölçüm "Bellek" gibi temel rol örneği sayısı ölçeklendirebilirsiniz.  Application Insights, bulut hizmeti projesi paket dosyanızı (*.cspkg) içinde yapılandırmak ve bu feat uygulamak için hizmet üzerinde Azure tanılama uzantısını etkinleştirme gerekmez.

Bulut hizmetleri üzerinde otomatik ölçeklendirmeye yapılandırmak için Application Insights ile özel bir ölçü kullanma konusunda daha fazla ayrıntı için bkz: [otomatik ölçek ile özel bir ölçü Azure tarafından kullanmaya başlama](../monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md)

Azure tanılama bulut Hizmetleri için Application Insights ile tümleştirme hakkında daha fazla bilgi için bkz: [Gönder bulut hizmeti, sanal makine ya da Service Fabric tanılama verilerini Application ınsights](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md)

Yaklaşık Application Insights için bulut hizmetlerini etkinleştirmek daha fazla bilgi için bkz: [Azure bulut Hizmetleri için Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-cloudservices)

Bulut Hizmetleri için Azure tanılama günlüğü etkinleştirme hakkında daha fazla bilgi için bkz: [tanılama Azure Cloud Services ve sanal makineler için ayarlama](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md#turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them)

## <a name="generic"></a>Genel

### <a name="how-do-i-add-nosniff-to-my-website"></a>My Web sitesine nasıl "nosniff" eklensin mi?
MIME türleri algılaması istemcilerin önlemek için bir ayar ekleyin, *web.config* dosya.

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

Ayrıca bu ayarı IIS'de olarak ekleyebilirsiniz. Aşağıdaki komutla kullanmak [ortak başlangıç görevleri](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) makalesi.

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="how-do-i-customize-iis-for-a-web-role"></a>Web rolü için nasıl IIS özelleştirebilir?
IIS başlangıç komut dosyasını kullanın [ortak başlangıç görevleri](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) makalesi.

### <a name="what-is-the-quota-limit-for-my-cloud-service"></a>My bulut hizmeti için kota sınırı nedir?
Bkz: [hizmete özgü sınırlar](../azure-subscription-service-limits.md#subscription-limits).

### <a name="why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Bulut hizmeti VM'im sürücüde çok az boş disk alanı neden Göster?
Bu beklenen davranıştır ve uygulamanız için herhangi bir sorun neden döndürmemelidir. Günlük kaydı temelde çift dosyaları normalde kapladığı alanı miktarını tüketir Azure PaaS VM'ler % approot % sürücüde için açık. Ancak aslında, dikkate etmeniz gereken birkaç nokta vardır, bu bir olmayan-sorunu yaşayıp açın.

% Approot % sürücü boyutu hesaplanan < .cspkg + max günlük boyutu boyutunu > + Kenar boşluğu boş alan olarak ya da 1,5 GB, hangisi daha büyüktür. VM boyutu şifrelemeyle Bu hesaplamayı vardır. (VM boyutu yalnızca geçici C: sürücüsü boyutunu etkiler.) 

% Approot % diske yazmak için desteklenmiyor. Azure VM yazıyorsanız, bunu bir geçici LocalStorage kaynak yapmalısınız (veya başka bir seçeneği gibi Blob Depolama, Azure dosyaları, vb..). Bu nedenle % approot % klasöründeki boş alan miktarını anlamı yoktur. Uygulamanızı % approot % diske yazma olup olmadığından emin değilseniz, hizmetiniz birkaç gün boyunca çalıştırın ve ardından karşılaştırın her zaman izin verebilir "önce" ve "sonra" boyutları. 

Azure herhangi bir şey % approot % diske yazma değil. VHD, .cspkg oluşturup Azure VM'de oturum takılı sonra bu sürücüye yazabilir tek şey, uygulamasıdır. 

Kapatamaz şekilde günlük yapılandırılamayan, ayarlardır.

### <a name="how-can-i-add-an-antimalware-extension-for-my-cloud-services-in-an-automated-way"></a>Nasıl bir kötü amaçlı yazılımdan koruma uzantı my bulut Hizmetleri için otomatik bir şekilde ekleyebilirim?

Başlangıç görevi PowerShell Betiği kullanılarak kötü amaçlı yazılımdan koruma uzantısını etkinleştirebilirsiniz. Bu makaleler uygulamak için adımları izleyin: 
 
- [PowerShell başlangıç görev oluşturma](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)
- [Set-AzureServiceAntimalwareExtension](https://docs.microsoft.com/powershell/module/Azure/Set-AzureServiceAntimalwareExtension?view=azuresmps-4.0.0 )

Kötü amaçlı yazılımdan koruma dağıtım senaryoları ve portaldan etkinleştirme hakkında daha fazla bilgi için bkz: [kötü amaçlı yazılımdan koruma dağıtım senaryoları](../security/azure-security-antimalware.md#antimalware-deployment-scenarios).

### <a name="how-to-enable-server-name-indication-sni-for-cloud-services"></a>Sunucu adı göstergesi (SNI) için bulut hizmetlerini etkinleştirmek nasıl?

Bulut Hizmetleri SNI aşağıdaki yöntemlerden birini kullanarak etkinleştirebilirsiniz:

**Yöntem 1: PowerShell kullanma**

SNI bağlamanın PowerShell cmdlet'i kullanılarak yapılandırılabilir **yeni WebBinding** aşağıdaki şekilde bulut Hizmeti rol örneği için bir başlangıç görevi içinde:
    
    New-WebBinding -Name $WebsiteName -Protocol "https" -Port 443 -IPAddress $IPAddress -HostHeader $HostHeader -SslFlags $sslFlags 
    
Açıklandığı gibi [burada](https://technet.microsoft.com/library/ee790567.aspx), $sslFlags olarak aşağıdaki değerlerden biri olabilir:

|Değer|Anlamı|
------|------
|0|Hiçbir SNI|
|1|SNI etkin |
|2 |Merkezi sertifika deposu kullanan bağlama olmayan SNI|
|3|Merkezi sertifika kullanan SNI bağlama depolama |
 
**Yöntem 2: Bir kod kullanın**

SNI bağlama de rol başlatma kodda aracılığıyla bu açıklandığı gibi yapılandırılabilir [blog gönderisi](https://blogs.msdn.microsoft.com/jianwu/2014/12/17/expose-ssl-service-to-multi-domains-from-the-same-cloud-service/):

    
    //<code snip> 
                    var serverManager = new ServerManager(); 
                    var site = serverManager.Sites[0]; 
                    var binding = site.Bindings.Add(“:443:www.test1.com”, newCert.GetCertHash(), “My”); 
                    binding.SetAttributeValue(“sslFlags”, 1); //enables the SNI 
                    serverManager.CommitChanges(); 
    //</code snip> 
    
Yaklaşımlardan birini kullanarak, belirli ana bilgisayar adları için ilgili sertifikaları (*.pfx) önce bir başlangıç görevi kullanılarak rol örnekleri veya kod SNI bağlamanın etkili olması sırayla aracılığıyla yüklenmesi gerekir.

### <a name="how-can-i-add-tags-to-my-azure-cloud-service"></a>Etiketler my Azure bulut hizmetine nasıl ekleyebilirim? 

Bulut hizmeti klasik bir kaynaktır. Yalnızca Azure Kaynak Yöneticisi desteği etiketleri oluşturulan kaynakları. Bulut hizmeti gibi Klasik kaynakları etiketleri uygulayamazsınız. 

### <a name="the-azure-portal-doesnt-display-the-sdk-version-of-my-cloud-service-how-can-i-get-that"></a>Azure portalında bulut Hizmetim SDK sürümü görüntülemez. Bu sorunu nasıl alabilirim?

Bu özellik Azure portalında getirmek çalışıyoruz. Bu sırada, SDK sürümü almak için PowerShell komutlarını kullanabilirsiniz:

    Get-AzureService -ServiceName "<Cloud Service name>" | Get-AzureDeployment | Where-Object -Property SdkVersion -NE -Value "" | select ServiceName,SdkVersion,OSVersion,Slot

### <a name="i-want-to-shut-down-the-cloud-service-for-several-months-how-to-reduce-the-billing-cost-of-cloud-service-without-losing-the-ip-address"></a>Bulut hizmeti için birkaç ay kapattığınız istiyorsunuz. IP adresi kaybetmeden fatura bulut hizmeti maliyetini azaltmak nasıl?

Zaten dağıtılmış bir bulut hizmeti kullandığı depolama ve işlem için fatura. Azure VM kapatma olsa bile, bu nedenle, hala depolama alanı için fatura. 

İşte, faturalama hizmetiniz için IP adresi kaybetmeden azaltmak için yapabilirsiniz:

1. [IP adresi ayırın](../virtual-network/virtual-networks-reserved-public-ip.md) dağıtımları silmeden önce.  Yalnızca bu IP adresi için Fatura edilecek. IP adresi faturalama hakkında daha fazla bilgi için bkz: [fiyatlandırma IP adreslerini](https://azure.microsoft.com/pricing/details/ip-addresses/).
2. Dağıtımları silin. Gelecek için kullanabilmeleri xxx.cloudapp.net silmeyin.
3. Bulut hizmeti aynı ayrılmış IP kullanarak yeniden dağıtmak isterseniz, aboneliğinizde ayrılmış bkz [bulut Hizmetleri ve sanal makineler için ayrılmış IP adresleri](https://azure.microsoft.com/blog/reserved-ip-addresses/).

