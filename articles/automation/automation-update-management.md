---
title: Güncelleştirme yönetimi çözümü Azure
description: Bu makale, Windows ve Linux bilgisayarlarınızın güncelleştirmelerini yönetmek için bu çözümün nasıl kullanılacağına yönelik bilgiler verir.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/23/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: c4b05044b0894e565ec4136f368314cb22041a7b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="update-management-solution-in-azure"></a>Güncelleştirme yönetimi çözümü Azure

Güncelleştirme yönetimi çözümü Azure automation'da Azure, şirket içi ortamları veya diğer bulut sağlayıcıları dağıtılmış, Windows ve Linux bilgisayarlar için işletim sistemi güncelleştirmeleri yönetmenize olanak sağlar. Tüm aracı bilgisayarlardaki kullanılabilir güncelleştirmelerin durumunu hızla değerlendirebilir ve sunucular için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz.

[Azure Otomasyonu](automation-offering-get-started.md) hesabınızdan sanal makineleriniz için Güncelleştirme yönetimini etkinleştirebilirsiniz.
Otomasyon hesabınızdan sanal makineler için güncelleştirme yönetimini nasıl etkinleştireceğinizi öğrenmek için bkz. [Birden fazla sanal makine için güncelleştirmeleri yönetme](manage-update-multi.md). Güncelleştirme yönetimi, Azure portalında sanal makine sayfasından tek bir sanal makine için de etkinleştirebilirsiniz. Bu senaryo için hem de kullanılabilir [Linux](../virtual-machines/linux/tutorial-monitoring.md#enable-update-management) ve [Windows](../virtual-machines/windows/tutorial-monitoring.md#enable-update-management) sanal makineler.

## <a name="solution-overview"></a>Çözüme genel bakış

Değerlendirme ve güncelleştirme dağıtımları gerçekleştirmek için aşağıdaki yapılandırmaları güncelleştirme yönetimi kullanarak yönetilen bilgisayarlara:

* Microsoft Monitoring Aracısı Windows veya Linux
* Linux için PowerShell İstenen Durum Yapılandırması (DSC)
* Otomasyon Karma Runbook Çalışanı
* Windows bilgisayarları için Microsoft Update veya Windows Server Update Hizmetleri

Aşağıdaki diyagramda davranışının kavramsal bir görünüm gösterir ve Windows Server ve Linux bilgisayarları bir çalışma alanında veri akışı nasıl çözüm değerlendirir ve tüm güvenlik güncelleştirmeleri uygular bağlı.

![Güncelleştirme yönetimi işlem akışı](media/automation-update-management/update-mgmt-updateworkflow.png)

Bir bilgisayarı güncelleştirme uyumluluğu için bir tarama yapar sonra aracı için günlük analizi toplu bilgi iletir. Window bilgisayarında, uyumluluk taraması varsayılan olarak her 12 saatte bir gerçekleştirilir. Tarama zamanlaması ek olarak, Microsoft İzleme Aracısı'nı (MMA) yeniden başlatılırsa 15 dakika içinde güncelleştirmeyi yüklemeden önce ve güncelleştirme yüklemesi sonrasında, güncelleştirme uyumluluğu taraması başlatılır. Linux bilgisayarıyla, uyumluluk taraması varsayılan olarak her 3 saatte bir gerçekleştirilir ve MMA aracısının yeniden başlatılması durumunda 15 dakika içinde uyumluluk taraması başlatılır.

Çözüm, eşitlenmek üzere yapılandırdığınız kaynağa dayanarak bilgisayarınızın ne düzeyde güncel olduğunu raporlar. Windows bilgisayarı WSUS’a raporlayacak şekilde yapılandırıldıysa, WSUS’un Microsoft Update ile en son ne zaman eşitlendiğine bağlı olarak, sonuçlar Microsoft Update tarafından gösterilenlerden farklı olabilir. Bu rapor Yerel Depodaki ortak depodaki karşılaştırması için yapılandırılmış olan Linux bilgisayarlar için aynıdır.

Zamanlanmış bir dağıtım oluşturarak, yazılım güncelleştirmelerinin gerekli olduğu bilgisayarlara güncelleştirmeleri dağıtabilir ve yükleyebilirsiniz. *İsteğe Bağlı* olarak sınıflandırılmış güncelleştirmeler Windows bilgisayarları için dağıtım kapsamına alınmaz; yalnızca gerekli güncelleştirmeler alınır. Zamanlanmış dağıtım hangi hedef bilgisayarlara uygulanabilir, açıkça bilgisayar belirtilmesi veya seçerek güncelleştirmelerle tanımlayan bir [bilgisayar grubu](../log-analytics/log-analytics-computer-groups.md) belirli bir bilgisayar kümesi günlük aramaları dışına dayanır. Ayrıca, güncelleştirmelerin yüklenmesine izin verilen zaman aralığını onaylamak ve belirlemek için bir zamanlama da belirtebilirsiniz. Güncelleştirmeler Azure Automation’daki runbook'lar tarafından yüklenir. Bu runbook’ları görüntüleyemezsiniz ve bunlar için herhangi bir yapılandırma gerekmez. Bir Güncelleştirme Dağıtımı oluşturulduğunda, dahil edilen bilgisayarlar için belirtilen zamanda ana güncelleştirme runbook’unu başlatan bir zamanlama oluşturur. Bu ana runbook, gerekli güncelleştirmelerin yüklemesini gerçekleştiren her aracıda bir alt runbook başlatır.

Güncelleştirme dağıtımında belirtilen tarih ve saatte, hedef bilgisayarlar dağıtımı paralel olarak yürütür. İlk olarak güncelleştirmelerin hala gerekli olduğunu doğrulamak için bir tarama yapılır ve bunlar yüklenir. WSUS istemci bilgisayarlar için güncelleştirmeleri, WSUS onaylanmamış güncelleştirme dağıtımı başarısız olur.

## <a name="clients"></a>İstemciler

### <a name="supported-client-types"></a>Desteklenen istemci türleri

Aşağıdaki tabloda, desteklenen işletim sistemlerinin bir listesini gösterir:

|İşletim Sistemi  |Notlar  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Yalnızca destekler değerlendirmeleri güncelleştir         |
|Windows Server 2008 R2 SP1 ve üzeri     |Windows PowerShell 4.0 veya üstü gereklidir ([WMF 4.0 indirme](https://www.microsoft.com/download/details.aspx?id=40855)).</br> Windows PowerShell 5.1 ([karşıdan WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616)) daha fazla güvenilirlik için önerilir.         |
|CentOS 6 (x86/x64) ve 7 (x64)      | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|Red Hat Enterprise 6 (x86/x64) ve 7 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)      |Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.         |

### <a name="unsupported-client-types"></a>Desteklenmeyen istemci türleri

Aşağıdaki tabloda, desteklenmeyen işletim sistemleri listelenmektedir:

|İşletim Sistemi  |Notlar  |
|---------|---------|
|Windows istemcisi     | İstemci işletim sistemleri (Windows 7, Windows 10, vb.) desteklenmez.        |
|Windows Server 2016 Nano Server     | Desteklenmiyor       |

### <a name="client-requirements"></a>İstemci gereksinimleri

#### <a name="windows"></a>Windows

Windows aracılarının bir Windows Server Update Services (WSUS) sunucusu ile iletişim kurmak veya Microsoft Update erişimi için yapılandırılmış olması gerekir. System Center Configuration Manager ile güncelleştirme yönetimi kullanılabilir, öğrenmek için tümleştirme senaryoları hakkında daha fazla ziyaret [güncelleştirme yönetimi ile System Center Configuration Manager tümleştirme](oms-solution-updatemgmt-sccmintegration.md#configuration). [Windows Aracısı](../log-analytics/log-analytics-agent-windows.md) gereklidir. Onboarding bir Azure VM varsa bu aracıyı otomatik olarak yüklenir.

#### <a name="linux"></a>Linux

Linux için makine özel veya genel olabilir bir güncelleştirme deposu için erişimi olmalıdır. Rapor birden fazla günlük analizi çalışma alanları için yapılandırılmış Linux için bir OMS Aracısı bu çözüm ile desteklenmiyor.

Linux için OMS aracısı yükleyin ve en son sürümü karşıdan yükleme hakkında daha fazla bilgi için bkz: [Linux için Operations Management Suite Aracısı](https://github.com/microsoft/oms-agent-for-linux). Windows için OMS Aracısı’nı yükleme hakkında bilgi için, [Windows için Operations Management Suite Aracısı](../log-analytics/log-analytics-windows-agent.md) konusunu gözden geçirin.

## <a name="permissions"></a>İzinler

Oluşturma ve güncelleştirme dağıtımları yönetmek için belirli izinler gerekir. Bu izinler hakkında daha fazla ziyaret öğrenmek için [rol tabanlı erişim - güncelleştirme yönetimi](automation-role-based-access-control.md#update-management)

## <a name="solution-components"></a>Çözüm bileşenleri

Bu çözüm, Otomasyon hesabınıza eklenen aşağıdaki kaynaklardan ve doğrudan bağlanılan aracılardan veya Operations Manager bağlantılı yönetim grubundan oluşur.

### <a name="hybrid-worker-groups"></a>Karma Çalışanı grupları

Bu çözüm etkinleştirdikten sonra bu çözümde bulunan runbook'ları desteklemek için bir karma Runbook çalışanı olarak doğrudan günlük analizi çalışma alanına bağlı herhangi bir Windows bilgisayarı otomatik olarak yapılandırılır. Çözümü tarafından yönetilen her Windows bilgisayar için karma çalışan grupları sayfanın altında adlandırma kuralı aşağıdaki Automation hesabınız için bir sistem karma çalışanı grubu olarak listelenen *ana bilgisayar adı FQDN_GUID*. Başarısız hesabınızda, aksi takdirde bu grupları runbook'larla hedefleyemez. Bu gruplar yalnızca yönetim çözümünü desteklemeye yöneliktir.

Bununla birlikte, Çözüm ve Karma Runbook Çalışanı grup üyeliği için aynı hesabı kullandığınız sürece Otomasyon gruplarını desteklemek için Windows bilgisayarlarını Otomasyon hesabınızdaki bir Karma Runbook Çalışanı grubuna ekleyebilirsiniz. Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.

### <a name="management-packs"></a>Yönetim paketleri

System Center Operations Manager yönetim grubunuzu günlük analizi çalışma alanına bağlıysa, aşağıdaki yönetim paketleri Operations Manager'da yüklenir. Bu çözüm eklendikten sonra bu yönetim paketleri doğrudan bağlı Windows bilgisayarlarına da yüklenir. Bu yönetim paketlerinde yapılandırılacak veya yönetilecek hiçbir şey yoktur.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* MP Dağıtımını güncelleştirme

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../log-analytics/log-analytics-om-agents.md).

### <a name="confirm-non-azure-machines-are-onboarded"></a>Azure olmayan makinelerin edildi olduğunu onaylayın

Doğrudan bağlı makineler onaylamak için birkaç dakika aşağıdaki günlük arama çalıştırdıktan sonra günlük analizi ile iletişim kuran:

#### <a name="linux"></a>Linux

```
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table`
```

Bir Windows bilgisayarda günlük analizi ile Aracısı bağlanabilirliği doğrulamak için aşağıdakileri gözden geçirebilirsiniz:

1. Açık Microsoft Monitoring Agent Denetim Masası'ndaki ve üzerinde **Azure günlük analizi** sekmesinde, aracıyı görüntüler belirten iletiyi: **Microsoft Monitoring Agent için günlük analizi başarıyla bağlandı** .
2. Windows Olay Günlüğü’nü açın, **Uygulama ve Hizmet Günlükleri\Operations Manager** bölümüne gidin ve kaynak Hizmet Bağlayıcısı’nda Olay Kimliği 3000 ve 5002’yi arayın. Bu olaylar, bilgisayar günlük analizi çalışma alanı ile kayıtlı ve yapılandırmayı almayı gösterir.

Aracı günlük analizi ile iletişim kurabildiğinden değil ve bir güvenlik duvarı veya proxy sunucu üzerinden internet ile iletişim kuracak şekilde yapılandırıldı, güvenlik duvarı veya proxy sunucusu düzgün yapılandırılmış gözden geçirerek onaylayın [için ağ yapılandırması Windows Aracısı](../log-analytics/log-analytics-agent-windows.md) veya [Linux aracısı için ağ yapılandırması](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Linux sistemlerinizi bir proxy veya OMS ağ geçidi ile iletişim kurmak için yapılandırılır ve ekleme eminseniz bu çözüm, güncelleştirme *proxy.conf* omiuser grup izinleri okuma izni dosyasında aşağıdaki gerçekleştirerek komutlar: `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Yeni eklenen Linux aracılarında, değerlendirme yapıldıktan sonra **Güncelleştirildi** durumu gösterilir. Bu işlem 6 saat kadar sürebilir.

Bir Operations Manager yönetim grubu günlük analizi ile iletişim onaylamak için bkz: [günlük analizi ile Operations Manager tümleştirmesini doğrulama](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-oms).

## <a name="data-collection"></a>Veri toplama

### <a name="supported-agents"></a>Desteklenen aracılar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| Windows aracıları |Evet |Çözüm, Windows aracılarından sistem güncelleştirme bilgilerini toplar ve gerekli güncelleştirmelerin yüklemesini başlatır. |
| Linux aracıları |Evet |Çözüm, Linux aracılarından sistem güncelleştirme bilgilerini toplar ve desteklenen dağıtımlarda gerekli güncelleştirmelerin yüklemesini başlatır. |
| Operations Manager yönetim grubu |Evet |Çözüm, bağlı bir yönetim grubundaki aracılardan sistem güncelleştirmeleri hakkında bilgi toplar.</br>Operations Manager aracısının doğrudan Log Analytics’e bağlanması gerekmez. Veri yönetim grubu için günlük analizi çalışma alanına iletilir. |

### <a name="collection-frequency"></a>Toplama sıklığı

Yönetilen her Windows bilgisayarı için günde iki kez tarama gerçekleştirilir. Her 15 dakikada bir Windows API’si çağrılarak son güncelleştirme zamanı sorgulanır; böylelikle durumun değişip değişmediği saptanır ve değişmişse bir uyumluluk taraması başlatılır. Yönetilen her Linux bilgisayarı için 3 saatte bir tarama gerçekleştirilir.

Yönetilen bilgisayarlardan gelen güncelleştirilmiş verilerin panoda görüntülenmesi 30 dakika ile 6 saat arasında bir zaman alabilir.

## <a name="viewing-update-assessments"></a>Güncelleştirme değerlendirmelerini görüntüleme

Tıklayın **güncelleştirme yönetimi** Otomasyon hesabınızda makinelerinizi durumunu görüntüleyin.

Bu görünüm güncelleştirmeleri, güncelleştirme dağıtımları ve zamanlanan güncelleştirme dağıtımları eksik makinelerinizi hakkında bilgi sağlar.

Öğe listeden seçerek makine, güncelleştirme veya dağıtım bilgileri döndürür günlük arama çalıştırabilirsiniz. Bu açılır **günlük arama** sayfası seçilen öğe için bir sorgu ile.

## <a name="installing-updates"></a>Güncelleştirmeleri yükleme

Güncelleştirmeler çalışma alanınızdaki tüm Linux ve Windows bilgisayarlar için değerlendirildikten sonra bir *Güncelleştirme Dağıtımı* oluşturarak gerekli güncelleştirmeleri yükleyebilirsiniz. Bir veya daha fazla bilgisayar için gerekli güncelleştirmelerin zamanlanmış bir yükleme bir güncelleştirme dağıtımıdır. Dağıtımın tarih ve saatine ek olarak, dağıtım kapsamına alınacak bilgisayarı veya bilgisayar grubunu belirtmeniz gerekir. Bilgisayar grupları hakkında daha fazla bilgi edinmek için bkz. [Log Analytics’te bilgisayar grupları](../log-analytics/log-analytics-computer-groups.md). Bilgisayar grupları güncelleştirme dağıtımınızda eklediğinizde, grup üyeliği zamanlama oluşturma aynı anda yalnızca bir kez değerlendirilir. Grupta daha sonra yapılan değişiklikler yansıtılmaz. Bu soruna geçici bir çözüm olarak, zamanlanmış güncelleştirme dağıtımını silin ve yeniden oluşturun.

> [!NOTE]
> Azure Market’ten dağıtılan Windows VM’leri varsayılan olarak Windows Update hizmetinden otomatik güncelleştirmeleri alacak şekilde ayarlanmıştır. Çalışma alanınıza bu çözümü veya Windows VM’lerini ekledikten sonra bu davranış değişmez. Etkin olarak güncelleştirmeleri bu çözüm, varsayılan davranışı ile yönettiğiniz değil ise (güncelleştirmelerini otomatik olarak uygulama) uygulanır.

Güncelleştirmelerin Ubuntu'daki bakım penceresinin dışında uygulanmasının önüne geçmek için Katılımsız Yükseltme paketini otomatik güncelleştirmeler devre dışı bırakılacak şekilden yeniden yapılandırın. Bahsedilen yapılandırma işlemiyle ilgili bilgi için bkz. [Ubuntu Server Kılavuzu'ndaki Otomatik Güncelleştirmeler konu başlığı](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Azure Market’ten edinilebilen isteğe bağlı Red Hat Enterprise Linux (RHEL) görüntülerinden oluşturulan sanal makineler için Azure’da dağıtılmış [Red Hat Update Infrastructure (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) erişimi için kaydedilir. Diğer tüm Linux dağıtımları, ilgili dağıtımın desteklediği yöntemler izlenerek çevrimiçi dosya deposundan güncelleştirilmelidir.

## <a name="viewing-missing-updates"></a>Eksik güncelleştirmeleri görüntüleme

Tıklatın **eksik güncelleştirmeler** , makinelerden eksik güncelleştirmeler listesini görüntülemek için. Her güncelleştirme listelenir ve daha fazla bilgi için güncelleştirme, işletim sistemi ve bağlantı gerektiren makineler sayısı ile ilgili bilgileri görüntüler. Her güncelleştirme seçilebilir ve **günlük arama** sayfasını görüntüler ve güncelleştirmeler hakkında daha fazla ayrıntı gösterir.

## <a name="viewing-update-deployments"></a>Güncelleştirme dağıtımlarını görüntüleme

Tıklatın **güncelleştirme dağıtımları** var olan güncelleştirme dağıtımları listesini görüntülemek için. Herhangi bir güncelleştirme dağıtımlarının tablosundaki tıklayarak açılan **güncelleştirme dağıtımı çalıştırın** Bu güncelleştirme dağıtımı için sayfa.

![Güncelleştirme Dağıtımı Sonuçlarına genel bakış](./media/automation-update-management/update-deployment-run.png)

## <a name="creating-an-update-deployment"></a>Güncelleştirme Dağıtımı oluşturma

Tıklayarak yeni bir güncelleştirme dağıtımı oluşturmak **zamanlama güncelleştirme dağıtımı** açmak için ekranın üstündeki düğmesi **yeni güncelleştirme dağıtımını** sayfası. Aşağıdaki tabloda özellikleri için değer sağlamanız gerekir:

| Özellik | Açıklama |
| --- | --- |
| Adı |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad. |
|İşletim Sistemi| Linux veya Windows|
| Makineleri güncelleştirmek için |Kayıtlı aramayı seçin veya makine açılan listeden seçin ve tek tek makineleri seçin |
|Güncelleştirme sınıflandırmaları|Gereksinim duyduğunuz tüm güncelleştirme sınıflandırmalarını seçin.|
|Dışlanacak güncelleştirmeleri|'KB' öneki de hariç KB girin|
|Zamanlama ayarları|Başlangıç ve her iki kez seçmek için saati seçin veya yineleme için yinelenen|
| Bakım penceresi |Güncelleştirmeler için belirlediğiniz dakika sayısı. Değeri olması olamaz az 30 dakika ve en fazla 6 saat |

## <a name="update-classifications"></a>Güncelleştirme sınıflandırmaları

Aşağıdaki tablolar, güncelleştirme Yönetimi'nde güncelleştirme sınıflandırmalarını her sınıflandırması için bir tanım birlikte listesini sağlar.

### <a name="windows"></a>Windows

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler     | Kritik, güvenlikle ilgili olmayan bir hatayı gidermeye belirli bir sorun için bir güncelleştirme.        |
|Güvenlik güncelleştirmeleri     | Ürüne özgü, güvenlikle ilgili bir sorun için bir güncelleştirme.        |
|Güncelleştirme paketleri     | Bir toplu setini kolay dağıtım için bir arada paketlenmiş.        |
|Özellik paketleri     | Bir ürünün sürümü dışında dağıtılan yeni ürün özelliklerini.        |
|Hizmet paketleri     | Bir uygulamaya uygulanan düzeltmeleri toplu kümesi.        |
|Tanım güncelleştirmeleri     | Bir güncelleştirme virüs veya diğer tanım dosyalarına.        |
|Araçlar     | Bir yardımcı programı veya bir veya daha fazla görevi tamamlamayı sağlayan bir özellik.        |
|Güncelleştirmeler     | Bir uygulama ya da şu an yüklü olan dosya için bir güncelleştirme.        |

### <a name="linux"></a>Linux

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri     | Belirli bir sorun veya ürüne özgü, güvenlikle ilgili bir sorun için güncelleştirmeleri.         |
|Diğer güncelleştirmeler     | Doğası veya güvenlik güncelleştirmeleri kritik olmayan tüm diğer güncelleştirmeleri.        |

## <a name="search-logs"></a>Arama günlükleri

Portalı'nda sağlanan Ayrıntılar ek olarak, arama günlüklerini karşı yapılabilir. İle **değişiklik izleme** sayfa açık tıklatın **günlük analizi**, bu açılır **günlük arama** sayfası

### <a name="sample-queries"></a>Örnek sorgular

Aşağıdaki tabloda bu çözüm tarafından toplanan güncelleştirme kayıtları için örnek günlük aramaları sağlar:

| Sorgu | Açıklama |
| --- | --- |
|Güncelleştirme</br>&#124;Burada UpdateState "Gerekli" ve isteğe bağlı == == false</br>&#124;Bilgisayar, başlık, KBID, Sınıflandırma, PublishedDate proje |Eksik güncelleştirmeleri olan tüm bilgisayarlar</br>İşletim sistemi sınırlamak için aşağıdakilerden birini ekleyin:</br>OSType = "Windows"</br>OSType "Linux" == |
| Güncelleştirme</br>&#124;Burada UpdateState "Gerekli" ve isteğe bağlı == == false</br>&#124;Burada bilgisayar "ContosoVM1.contoso.com" ==</br>&#124;Bilgisayar, başlık, KBID, ürün, PublishedDate proje |Belirli bir bilgisayarda eksik güncelleştirmeler (değeri kendi bilgisayarınızın adıyla değiştirin)|
| Olay</br>&#124;Burada EventLevelName "error" ve bilgisayar == ((güncelleştirme &#124; where (sınıflandırma "Güvenlik güncelleştirmeleri" veya sınıflandırma == "Kritik güncelleştirmeler" ==)</br>&#124;Burada UpdateState "Gerekli" ve isteğe bağlı == == false </br>&#124;farklı bilgisayar)) |Kritik güncelleştirmeleri veya gerekli güvenlik güncelleştirmeleri eksik olan makineler için hata olayları |
| Güncelleştirme</br>&#124;Burada UpdateState "Gerekli" ve isteğe bağlı == == false</br>&#124;ayrı başlığı |Tüm bilgisayarlardaki ayrı eksik güncelleştirmeler |
| UpdateRunProgress</br>&#124;Burada InstallationStatus "başarısız" == </br>&#124;AggregatedValue özetlemek bilgisayar, başlık, UpdateRunName tarafından count() = |Bir güncelleştirme çalışması başarısız güncelleştirmelere sahip bilgisayarlar</br>İşletim sistemi sınırlamak için aşağıdakilerden birini ekleyin:</br>OSType = "Windows"</br>OSType "Linux" == |
| Güncelleştirme</br>&#124;Burada OSType "Linux" ==</br>&#124;Burada UpdateState! "Gerekli" = ve (sınıflandırma "Kritik güncelleştirmeler" veya sınıflandırma == "Güvenlik güncelleştirmeleri" ==)</br>&#124;AggregatedValue özetlemek bilgisayar tarafından count() = |Paket güncelleştirme kullanılabilir olan tüm Linux makineler, kritik güncelleştirmeler veya güvenlik açığına listesi |
| UpdateRunProgress</br>&#124;Burada UpdateRunName "DeploymentName" ==</br>&#124;AggregatedValue özetlemek bilgisayar tarafından count() =|Bu güncelleştirme çalıştırmasında güncelleştirilmiş olan bilgisayarlar (değeri kendi Güncelleştirme Dağıtımı adınızla değiştirin) |

## <a name="integrate-with-system-center-configuration-manager"></a>System Center Configuration Manager ile tümleştirme

PC, sunucu ve mobil cihazları yönetmek için System Center Configuration Manager’a yatırım yapmış müşteriler aynı zamanda yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçası olarak yazılım güncelleştirmelerini yönetme gücünden ve olgunluğundan yararlanmaktadır.

Yönetim çözümü System Center Configuration Manager ile tümleştirmek öğrenmek için bkz: [güncelleştirme yönetimi ile System Center Configuration Manager tümleştirme](oms-solution-updatemgmt-sccmintegration.md).

## <a name="patching-linux-machines"></a>Linux makineler düzeltme eki uygulama

Aşağıdaki bölümlerde Linux düzeltme eki uygulama ile ilgili olası sorunları açıklanmaktadır.

### <a name="package-exclusion"></a>Paket dışlama

Red Hat Enterprise Linux gibi bazı Linux çeşitleri üzerinde düzey işletim sistemi yükseltme paketleri ortaya çıkabilir. Bu güncelleştirme yönetimi çalıştırmaları için burada işletim sistemi sürüm numarasını değiştirir neden olabilir. Güncelleştirme yönetimi, bir yönetici Linux bilgisayarda yerel olarak olduğu gibi güncelleştirme paketleri için aynı yöntemleri kullandığından, bu kasıtlı bir davranıştır.

Güncelleştirme yönetimi çalışmalarını aracılığıyla işletim sistemi sürümü güncelleştirme önlemek için kullandığınız **dışlama** özelliği.

Red Hat Enterprise Linux dışlamak için paket adı olacaktır: redhat sürüm server.x86_64

![Linux için dışlamak için paketler](./media/automation-update-management/linuxpatches.png)

### <a name="security-patches-not-being-applied"></a>Güvenlik yamaları uygulanmıyor

Güncelleştirmeler için Linux makine dağıtırken, güncelleştirme sınıflandırmalarını seçebilirsiniz. Belirtilen ölçütlere uyan o uygulanan güncelleştirmeleri filtreler. Güncelleştirme dağıtıldığında bu filtre makinede yerel olarak uygulanır. Güncelleştirme yönetimi bulutta güncelleştirme iyileştirmesini gerçekleştirdiğinden, bazı güncelleştirmeler güncelleştirme Yönetimi'nde yerel makine bu bilgileri gerekmese de güvenlik etkisine sahip olarak işaretlenmesini. Sonuç olarak, kritik güncelleştirmeler için Linux makine uygularsanız, olabilir, bu makinede güvenlik etkisine sahip olarak işaretlenmez ve uygulanmamış güncelleştirmeler. Ancak, güncelleştirme yönetimi hala ilgili güncelleştirme hakkında ek bilgi sahip olduğundan uyumlu olacak şekilde bu makine bildirebilir.

Güncelleştirme sınıflandırması güncelleştirmelerini dağıtma openSUSE Linux kullanılır farklı düzeltme eki uygulama modeli nedeniyle çalışmayabilir.

## <a name="troubleshooting"></a>Sorun giderme

Bu bölümde, Güncelleştirme Yönetimi çözümüyle ilgili sorunları gidermeye yardımcı olacak bilgiler sağlanır.

Çözümü veya bir sanal makineyi eklemeye çalışırken sorun yaşarsanız, olay kimliği 4502 olan olaylar ve **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** içeren olay iletisi için **Uygulama ve Hizmet Günlükleri\Operations Manager** olay günlüğünü denetleyin. Aşağıdaki tabloda belirli hata iletileri ve her birinin olası çözümü vurgulanmıştır.

| İleti | Neden | Çözüm |
|----------|----------|----------|
| Yama Yönetimi için Makine Kaydedilemiyor,</br>Kayıt Özel Durumla Başarısız Oldu</br>System.InvalidOperationException: {"Message":"Makine zaten</br>farklı bir hesaba kaydedildi. "} | Makine, Güncelleştirme Yönetimi için zaten başka bir çalışma alanına eklendi | Karma runbook grubunu silerek [eski yapıtları temizleyin](automation-hybrid-runbook-worker.md#remove-hybrid-worker-groups)|
| Düzeltme Eki Yönetimi için makine kaydedilemedi, kayıt özel durumu ile başarısız</br>System.Net.Http.HttpRequestException: İstek gönderilirken bir hata oluştu. ---></br>System.Net.WebException: Temel alınan bağlantı</br>kapatıldı: Alma işlemi sırasında</br>beklenmeyen bir hata oluştu. ---> System.ComponentModel.Win32Exception:</br>İstemci ve sunucu iletişim kuramıyor,</br>çünkü ortak bir algoritmaya sahip değiller | Proxy/Ağ Geçidi/Güvenlik Duvarı iletişimi engelliyor | [Ağ gereksinimlerini gözden geçirin](automation-offering-get-started.md#network-planning)|
| Yama Yönetimi için Makine Kaydedilemiyor,</br>Kayıt Özel Durumla Başarısız Oldu</br>Newtonsoft.Json.JsonReaderException: Pozitif sonsuz değer ayrıştırılırken hata oluştu. | Proxy/Ağ Geçidi/Güvenlik Duvarı iletişimi engelliyor | [Ağ gereksinimlerini gözden geçirin](automation-offering-get-started.md#network-planning)|
| Hizmet tarafından sunulan sertifika \<wsid\>. oms.opinsights.azure.com</br>Microsoft hizmetleri için kullanılan bir sertifika yetkilisi</br>tarafından verilmemiş. İletişim</br>ağ yöneticinize başvurarak</br>TLS/SSL iletişimini engelleyen bir proxy çalıştırıp çalıştırmadıklarına bakın. |Proxy/Ağ Geçidi/Güvenlik Duvarı iletişimi engelliyor | [Ağ gereksinimlerini gözden geçirin](automation-offering-get-started.md#network-planning)|
| Yama Yönetimi için Makine Kaydedilemiyor,</br>Kayıt Özel Durumla Başarısız Oldu</br>AgentService.HybridRegistration.</br>PowerShell.Certificates.CertificateCreationException:</br>Otomatik olarak imzalanan sertifika oluşturulamadı. ---></br>System.UnauthorizedAccessException: Erişim reddedildi. | Otomatik olarak imzalanan sertifika oluşturma hatası | Sistem hesabının</br>klasöre okuma erişiminin olduğunu doğrulayın:</br>**C:\ProgramData\Microsoft\**</br>** Crypto\RSA **|

## <a name="next-steps"></a>Sonraki adımlar

Öğretici Windows Vm'leriniz için güncelleştirmeleri yönetme konusunda bilgi almak için devam edin.

> [!div class="nextstepaction"]
> [Güncelleştirme ve düzeltme eklerini Azure Windows Vm'leriniz için yönetme](automation-tutorial-update-management.md)

* Ayrıntılı güncelleştirme verilerini görüntülemek için [Log Analytics](../log-analytics/log-analytics-log-searches.md)’te Günlük Aramalarını kullanın.
* Bilgisayardan eksik kritik güncelleştirmeler algılandığında veya bilgisayarda otomatik güncelleştirmeler devre dışı olduğunda [uyarılar oluşturun](../log-analytics/log-analytics-alerts.md).
