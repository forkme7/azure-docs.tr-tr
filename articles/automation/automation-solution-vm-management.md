---
title: Başlat/Durdur VM'ler sırasında yoğun olmayan saatlerde çözüm (Önizleme)
description: Bu VM yönetim çözümü başlatır ve Azure Resource Manager sanal makinelerinizi bir zamanlamaya göre durdurur ve günlük analizi proaktif olarak izler.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/20/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 41a5ff2613706b7454a96daa52c7cb20c734c394
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="startstop-vms-during-off-hours-solution-preview-in-azure-automation"></a>Azure Otomasyonu (Önizleme) çözümde yoğun olmayan saatlerde sırasında Başlat/Durdur VM'ler

Çözüm başlatır ve kullanıcı tanımlı zamanlamada Azure sanal makinelerinizi durdurur, Azure günlük analizi aracılığıyla Öngörüler sağlar ve kullanarak isteğe bağlı bir e-posta gönderen yoğun olmayan saatlerde sırasında Başlat/Durdur VM'ler [SendGrid](https://azuremarketplace.microsoft.com/marketplace/apps/SendGrid.SendGrid?tab=Overview). Azure Resource Manager ve klasik sanal makineleri için çoğu senaryoları destekler.

Bu çözüm sunucusuz, düşük maliyetli kaynakları kullanarak maliyetlerini azaltmak için isteyen kullanıcılar için bir protokoldür Otomasyon seçeneği sunar. Bu çözüm ile şunları yapabilirsiniz:

* Başlatma ve durdurma VM'ler zamanlayın.
* Azure (Klasik sanal makineleri için desteklenmez) etiketleri kullanarak artan düzende durdurmak ve başlatmak için VM'ler zamanlayın.
* Düşük CPU kullanımı dikkate alarak VM'ler otomatik durdurma.

## <a name="prerequisites"></a>Önkoşullar

* Runbook'lar, [Azure Farklı Çalıştır hesabı](automation-offering-get-started.md#authentication-methods) ile çalışır. Süresi dolacak veya sık sık değişen parola yerine sertifika kimlik doğrulaması kullandığından farklı çalıştır hesabı tercih edilen kimlik doğrulama yöntemidir.
* Bu çözüm, Azure Automation hesabınız ile aynı abonelikte bulunan sanal makineleri yönetir.
* Bu çözüm yalnızca aşağıdaki Azure bölgeler ile dağıtılır: Avustralya Güneydoğu, Kanada Merkezi, Orta Hindistan, Doğu ABD, Japonya Doğu, Güneydoğu Asya, Birleşik Krallık Güney ve Batı Avrupa.

  > [!NOTE]
  > VM zamanlama yönetme runbook'ları tüm bölgelerdeki VM'ler hedefleyebilirsiniz.

* Azure Marketi'nden ekleme sırasında başlatma ve durdurma VM runbook'lar bitirdikten sonra e-posta bildirimleri göndermek için seçin **Evet** SendGrid dağıtmak için.

  > [!IMPORTANT]
  > SendGrid bir üçüncü taraf hizmetidir. Destek için iletişim [SendGrid](https://sendgrid.com/contact/).

  SendGrid kısıtlamalarla şunlardır:

  * En fazla kullanıcı abonelik başına bir SendGrid hesabı.
  * İki SendGrid hesap başına abonelik sayısı.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Başlat/Durdur VM'ler sırasında yoğun olmayan saatlerde çözüm Otomasyon hesabınızı eklemek için aşağıdaki adımları gerçekleştirin ve ardından çözümü özelleştirmek için değişkenlerini yapılandırın.

1. Azure portalında **Kaynak oluştur**’a tıklayın.
1. Market sayfasında, bir anahtar sözcük gibi yazın **Başlat** veya **Başlat/Durdur**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Alternatif olarak, çözümü tam adı bir veya daha fazla kelimeleri yazın ve Enter tuşuna basın. Seçin **Başlat/Durdur VM'ler yoğun olmayan saatlerde [Önizleme] sırasında** Arama sonuçlarından.
1. İçinde **Başlat/Durdur VM'ler yoğun olmayan saatlerde [Önizleme] sırasında** sayfasında seçilen çözümü için özet bilgilerini inceleyin ve ardından **oluşturma**.

   ![Azure portalına](media/automation-solution-vm-management/azure-portal-01.png)

1. **Çözüm Ekle** sayfası görüntülenir. Otomasyon aboneliğinizi içeri aktarmadan önce çözümü yapılandırmak için istenir.

   ![VM yönetim Çözüm Ekle sayfası](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

1. Üzerinde **Çözüm Ekle** sayfasında, **çalışma**. Otomasyon hesabının bulunduğu aynı Azure aboneliğine bağlı bir günlük analizi çalışma alanını seçin. Bir çalışma alanı yoksa, seçin **yeni çalışma alanı oluştur**. Üzerinde **OMS çalışma** sayfasında, aşağıdakileri yapın:
   * Yeni **OMS Çalışma Alanı** için bir ad belirtin.
   * Seçin bir **abonelik** varsayılan seçili uygun değilse, aşağı açılan listeden seçerek bağlantı sağlamak için.
   * İçin **kaynak grubu**, yeni bir kaynak grubu oluşturabilir veya varolan bir tanesini seçin.
   * Bir **Konum** seçin. Şu anda yalnızca kullanılabilir konumlarının **Avustralya Güneydoğu**, **Kanada Merkezi**, **Orta Hindistan**, **Doğu ABD**, **Doğu Japonya**, **Güneydoğu Asya**, **Birleşik Krallık Güney**, ve **Batı Avrupa**.
   * Bir **Fiyatlandırma katmanı** seçin. Seçin **başına (tek başına) GB** seçeneği. Günlük analizi güncelleştirdi [fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/) ve GB başına katmanı tek seçenektir.

1. Üzerinde gerekli bilgileri girdikten sonra **OMS çalışma** sayfasında, **oluşturma**. Altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünden döndüğü size **Çözüm Ekle** sayfasında yapıldığında.
1. Üzerinde **Çözüm Ekle** sayfasında, **Otomasyon hesabı**. Yeni bir günlük analizi çalışma alanı oluşturuyorsanız, kendisiyle ilişkilendirilmiş olması için yeni bir Otomasyon hesabı da oluşturmanız gerekir. Seçin **Automation hesabı oluşturma**ve **eklemek Otomasyon hesabı** sayfasında, şunları sağlar:
   * **Ad** alanına Otomasyon hesabının adını girin.

    Diğer tüm seçenekleri seçili günlük analizi çalışma alanı otomatik olarak doldurulur. Bu seçenek değiştirilemez. Bu çözüme dahil olan runbook'lar için varsayılan kimlik doğrulama yöntemi, bir Azure Farklı Çalıştır hesabıdır. Tıklattıktan sonra **Tamam**, yapılandırma seçenekleri doğrulanır ve Automation hesabı oluşturulur. Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

1. Son olarak, üzerinde **Çözüm Ekle** sayfasında, **yapılandırma**. **Parametreleri** sayfası görüntülenir.

   ![Çözüm için parametreleri sayfası](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   Burada, sizden istenir:
   * Belirtin **hedef kaynak grubu adları**. Bu çözüm tarafından yönetilecek VM'ler içeren kaynak grubu adları şunlardır. Birden fazla ad girin ve her (değerleri büyük küçük harfe duyarlı değildir) virgül kullanarak ayırın. Abonelikte yer alan tüm kaynak gruplarındaki sanal makineleri hedeflemek istiyorsanız, joker karakter kullanılması desteklenir. Bu değer depolanan **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** değişkenleri.
   * Belirtin **VM dışlama listesi (dize)**. Hedef kaynak grubu bir veya daha fazla sanal makinelerden adıdır. Birden fazla ad girin ve her (değerleri büyük küçük harfe duyarlı değildir) virgül kullanarak ayırın. Bir joker karakter kullanılması desteklenir. Bu değer depolanan **External_ExcludeVMNames** değişkeni.
   * Seçin bir **zamanlama**. Bir yinelenen tarih ve saat başlatma ve durdurma hedef kaynak grupları içindeki VM'ler için budur. Varsayılan olarak, zamanlama için UTC saat dilimi yapılandırılır. Farklı bir bölge seçmek kullanılabilir değil. Çözüm yapılandırdıktan sonra belirli saat diliminiz için zamanlama yapılandırmak için bkz [başlatma ve kapatma zamanlamasını değiştirme](#modify-the-startup-and-shutdown-schedule).
   * Almak için **e-posta bildirimleri** SendGrid varsayılan değerini kabul **Evet** ve geçerli bir eposta adresi belirtin. Seçerseniz **Hayır** ancak daha sonraki bir tarihte karar e-posta bildirimleri almak istediğiniz, güncelleştirebilirsiniz **External_EmailToAddress** değişkeni geçerli bir e-posta adreslerine sahip bir virgülle ayrılmış ve ardından değişkeni değiştirme **External_IsSendEmail** değerle **Evet**.

> [!IMPORTANT]
> İçin varsayılan değer **hedef kaynak grubu adları** olan bir **&ast;**. Bir Abonelikteki tüm VM'ler hedefler. Aboneliğinizdeki tüm sanal makineleri hedeflemek için çözüm istemiyorsanız bu değeri zamanlamaları etkinleştirilmeden önce kaynak grubu adları listesini için güncelleştirilmesi gerekir.

1. Çözüm için gereken ilk ayarlarını yapılandırdıktan sonra tıklatın **Tamam** kapatmak için **parametreleri** sayfasından seçim yapıp **oluşturma**. Tüm ayarlar doğrulandıktan sonra çözümü aboneliğinize dağıtılır. Bu işlemin tamamlanması birkaç saniye sürebilir ve altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde.

## <a name="scenarios"></a>Senaryolar

Çözümü üç farklı senaryolar içerir. Bu senaryolar şunlardır:

### <a name="scenario-1-startstop-vms-on-a-schedule"></a>Senaryo 1: Başlat/Durdur VM'ler bir zamanlamaya göre

Bu, çözümü ilk kez dağıttığınızda varsayılan yapılandırmadır. Örneğin, akşam işlerinde bırakın ve office geri olduğunda sabah başlatmak tüm sanal makineleri bir abonelik üzerinden durdurulmasına yapılandırabilirsiniz. Zamanlamaları yapılandırırken **zamanlanmış-StartVM** ve **zamanlanmış StopVM** dağıtımı sırasında bunlar durdurup başlatın hedeflenen VM'ler. Yalnızca sanal makineleri durdurmak için bu çözümün yapılandırılması desteklenir, bkz: [başlatma ve kapatma zamanlamalarını değiştirmek](#modify-the-startup-and-shutdown-schedules) özel bir zamanlama yapılandırmak hakkında bilgi edinmek için.

>[!NOTE]
>Zamanlama süresi parametresi yapılandırdığınızda saat dilimi geçerli saat diliminizde alınır. Ancak, Azure Automation UTC biçiminde depolanır. Bu dağıtım sırasında gerçekleştirilir gibi herhangi bir saat dilimi dönüştürmeye yapmak gerekmez.

Kapsamındaki VM'ler aşağıdaki değişkenleri yapılandırarak denetim: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, ve **External_ ExcludeVMNames**.

Bir abonelik ve kaynak grubu karşı eylem hedefleme ya da sanal makineleri, ancak ikisini belirli listesini hedefleme etkinleştirebilirsiniz.

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Bir abonelik ve kaynak grubu karşı başlatma ve durdurma Eylemler hedef

1. Yapılandırma **External_Stop_ResourceGroupNames** ve **External_ExcludeVMNames** değişkenleri VM'ler hedef belirtin.
2. Etkinleştirme ve güncelleştirme **zamanlanmış-StartVM** ve **zamanlanmış StopVM** zamanlamaları.
3. Çalıştırma **ScheduledStartStop_Parent** ayarlamak eylem parametresi ile runbook **Başlat** ve kümesine WHATIF parametresi **True** değişikliklerinizi önizlemek için.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Başlatma ve durdurma eylemi VM listeyi hedef

1. Çalıştırma **ScheduledStartStop_Parent** ayarlamak eylem parametresi ile runbook **Başlat**, Vm'lerde virgülle ayrılmış bir listesi ekleme *VMList* parametre olarak ayarlayın ve ardından WHATIF parametresi **doğru**. Değişikliklerinizi önizlemede.
2. Yapılandırma **External_ExcludeVMNames** parametresi virgülle ayrılmış listesini içeren VM'ler (VM1, VM2, VM3).
3. Bu senaryo dikkate almayabilir **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri. Bu senaryo için kendi Otomasyon zamanlama oluşturmanız gerekir. Ayrıntılar için bkz [Azure Otomasyon runbook'u zamanlama](../automation/automation-schedules.md).

>[!NOTE]
> Değeri **hedef kaynak grubu adları** değeri olarak her ikisi için depolanan **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames**. Daha fazla ayrıntı için her biri farklı kaynak grupları hedeflemek için bu değişkenleri değiştirebilirsiniz. Başlangıç eylemi için kullanabilirsiniz **External_Start_ResourceGroupNames**ve durdurma eylemi için kullanabilirsiniz **External_Stop_ResourceGroupNames**. Sanal makineleri Başlat'a otomatik olarak eklenir ve zamanlamaları durdurabilirsiniz.

### <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a>Senaryo 2: Başlat/Durdur etiketleri kullanarak sıralı VM'ler

Dağıtılmış bir iş yükünü destekleme birden çok VM iki veya daha fazla bileşenlerini içeren bir ortamda, sırayla bileşenleri başlatıldığında ve durdurulduğunda dizisi destekleme önemlidir. Bu, şu adımları izleyerek gerçekleştirebilirsiniz:

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Bir abonelik ve kaynak grubu karşı başlatma ve durdurma Eylemler hedef

1. Ekleme bir **SequenceStart** ve **SequenceStop** etiketi içinde hedeflenen VM'ler için bir pozitif tamsayı değeri olan **External_Start_ResourceGroupNames** ve  **External_Stop_ResourceGroupNames** değişkenleri. Başlatma ve durdurma eylemleri artan düzende gerçekleştirilir. Bir VM etiketi öğrenmek için bkz: [Azure'da Windows sanal makine etiketi](../virtual-machines/windows/tag.md) ve [Azure'da bir Linux sanal makine etiketi](../virtual-machines/linux/tag.md).
2. Zamanlamaları değiştirme **sıralı-StartVM** ve **sıralı StopVM** gereksinimlerinizi karşılamak ve zamanlamayı etkinleştir saat ve tarihi için.
3. Çalıştırma **SequencedStartStop_Parent** ayarlamak eylem parametresi ile runbook **Başlat** ve kümesine WHATIF parametresi **True** değişikliklerinizi önizlemek için.
4. Eylem Önizleme ve sanal makineleri üretim karşı uygulamadan önce gerekli değişiklikleri yapın. Hazır, el ile runbook kümesine parametresiyle yürütülürken **False**, ya da Otomasyon zamanlama izin **sıralı-StartVM** ve **sıralı StopVM** çalıştırın otomatik olarak belirlenen zamanlamanızı aşağıdaki.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Başlatma ve durdurma eylemi VM listeyi hedef

1. Ekleme bir **SequenceStart** ve **SequenceStop** etiketi eklemek için planlama VM'ler için pozitif bir tamsayı değeri olan **VMList** değişkeni. 
2. Çalıştırma **SequencedStartStop_Parent** ayarlamak eylem parametresi ile runbook **Başlat**, Vm'lerde virgülle ayrılmış bir listesi ekleme *VMList* parametre olarak ayarlayın ve ardından WHATIF parametresi **doğru**. Değişikliklerinizi önizlemede.
3. Yapılandırma **External_ExcludeVMNames** parametresi virgülle ayrılmış listesini içeren VM'ler (VM1, VM2, VM3).
4. Bu senaryo dikkate almayabilir **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri. Bu senaryo için kendi Otomasyon zamanlama oluşturmanız gerekir. Ayrıntılar için bkz [Azure Otomasyon runbook'u zamanlama](../automation/automation-schedules.md).
5. Eylem Önizleme ve sanal makineleri üretim karşı uygulamadan önce gerekli değişiklikleri yapın. Hazır, el ile runbook kümesine parametresiyle yürütülürken **False**, ya da Otomasyon zamanlama izin **sıralı-StartVM** ve **sıralı StopVM** çalıştırın otomatik olarak belirlenen zamanlamanızı aşağıdaki.

### <a name="scenario-3-startstop-automatically-based-on-cpu-utilization"></a>Senaryo 3: Başlat/Durdur CPU kullanımını temel alarak otomatik olarak

Bu çözüm, yoğun olmayan zamanlarda gibi saat sonra kullanılmayan Azure VM'ler değerlendirmek ve işlemci kullanımı olması durumunda otomatik olarak onları kapatılıyor tarafından satıcısı %x aboneliğinizde çalışan sanal makineler maliyetini yönetmenize yardımcı olabilir.

Varsayılan olarak, yüzde CPU ölçüsünün ortalama kullanım yüzde 5 olup olmadığını değerlendirmek için önceden yapılandırılmış veya daha az çözümüdür. Bu, aşağıdaki değişkenleri tarafından denetlenir ve varsayılan değerlerin gereksinimlerinizi karşılamazsa değiştirilebilir:

* External_AutoStop_MetricName
* External_AutoStop_Threshold
* External_AutoStop_TimeAggregationOperator
* External_AutoStop_TimeWindow

Bir abonelik ve kaynak grubu karşı eylem hedefleme ya da sanal makineleri, ancak ikisini belirli listesini hedefleme etkinleştirebilirsiniz.

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Bir abonelik ve kaynak grubu karşı durdurma eylemi hedef

1. Yapılandırma **External_Stop_ResourceGroupNames** ve **External_ExcludeVMNames** değişkenleri VM'ler hedef belirtin.
2. Etkinleştirme ve güncelleştirme **Schedule_AutoStop_CreateAlert_Parent** zamanlama.
3. Çalıştırma **AutoStop_CreateAlert_Parent** ayarlamak eylem parametresi ile runbook **Başlat** ve kümesine WHATIF parametresi **True** değişikliklerinizi önizlemek için.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Başlatma ve durdurma eylemi VM listeyi hedef

1. Çalıştırma **AutoStop_CreateAlert_Parent** ayarlamak eylem parametresi ile runbook **Başlat**, Vm'lerde virgülle ayrılmış bir listesi ekleme *VMList* parametre olarak ayarlayın ve ardından WHATIF parametresi **doğru**. Değişikliklerinizi önizlemede.
2. Yapılandırma **External_ExcludeVMNames** parametresi virgülle ayrılmış listesini içeren VM'ler (VM1, VM2, VM3).
3. Bu senaryo dikkate almayabilir **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri. Bu senaryo için kendi Otomasyon zamanlama oluşturmanız gerekir. Ayrıntılar için bkz [Azure Otomasyon runbook'u zamanlama](../automation/automation-schedules.md).

CPU kullanımı temelinde sanal makineleri durdurmak için bir zamanlama sahip olduğunuza göre bunları başlatmak için aşağıdaki zamanlamaları birini etkinleştirmeniz gerekir.

* Hedef eylem abonelik ve kaynak grubu tarafından başlatın. Açıklanan adımları izleyerek [Senaryo 1](#scenario-1:-daily-stop/start-vms-across-a-subscription-or-target-resource-groups) test ve etkinleştirme için **zamanlanmış-StartVM** zamanlamaları.
* Hedef abonelik, kaynak grubu ve etiket eylemi başlatın. Açıklanan adımları izleyerek [Senaryo 2](#scenario-2:-sequence-the-stop/start-vms-across-a-subscription-using-tags) test ve etkinleştirme için **sıralı-StartVM** zamanlamaları.

## <a name="solution-components"></a>Çözüm bileşenleri

Bu çözüm önceden yapılandırılmış runbook'ları, tabloları ve günlük analizi ile tümleştirme içerir, böylece başlatma ve kapatma, iş gereksinimlerinize uyacak şekilde, sanal makinelerin uyarlayabilirsiniz.

### <a name="runbooks"></a>Runbook'lar

Aşağıdaki tabloda, Automation hesabınız için bu çözümü tarafından dağıtılmış runbook'ları listeler. Runbook kodda değişiklik yapmamalısınız. Bunun yerine, yeni işlevsellik için kendi runbook yazma.

> [!IMPORTANT]
> Doğrudan "alt adının sonuna" ile herhangi bir runbook çalıştırmayın.

Tüm üst runbook'ları dahil *whatIf* parametresi. Ayarlandığında **True**, *whatIf* olmadan çalıştırdığınızda tam davranışı ayrıntılı destekler runbook alır *whatIf* parametre ve doğru doğrular VM'ler bırakılıyor Hedeflenen. Bir runbook yalnızca tanımlı eylemlerini gerçekleştirir, *whatIf* parametrenin ayarlanmış **False**.

|**runbook** | **Parametreler** | **Açıklama**|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Üst runbook'tan çağrılır. Bu runbook DISPLAYFILTER senaryosu için kaynak başına temelinde uyarılar oluşturur.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: True veya False  | Oluşturur veya vm'lerinde Azure uyarı kuralları hedeflenen abonelik veya kaynak gruplarındaki güncelleştirir. <br> VMList: Virgülle ayrılmış listesi VM'ler. Örneğin, *vm1, vm2, vm3*.<br> *WhatIf* çalıştırmadan runbook mantığının doğrular.|
|AutoStop_Disable | yok | DISPLAYFILTER uyarılar ve varsayılan zamanlama devre dışı bırakır.|
|AutoStop_StopVM_Child | WebHookData | Üst runbook'tan çağrılır. Uyarı kuralları bu runbook için VM'yi durdurmak iyi çağırın.|
|Bootstrap_Main | yok | Bir kez, genellikle Azure Kaynak Yöneticisi'nden erişilebilir olmayan webhookURI gibi önyükleme yapılandırmaları ayarlamak için kullanılır. Bu runbook başarılı bir şekilde dağıtıldıktan sonra otomatik olarak kaldırılır.|
|ScheduledStartStop_Child | VMName <br> Eylem: Başlatma veya durdurma <br> ResourceGroupName | Üst runbook'tan çağrılır. Zamanlanmış durdurma için bir başlatma veya durdurma eylemi yürütür.|
|ScheduledStartStop_Parent | Eylem: Başlatma veya durdurma <br>VMList <br> WhatIf: True veya False | Bu Abonelikteki tüm sanal makineleri etkiler. Düzen **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** kaynak grupları yalnızca bunlar üzerinde yürütülmesi hedeflenen. Belirli sanal makineleri güncelleştirerek de hariç tutabilirsiniz **External_ExcludeVMNames** değişkeni.<br> VMList: Virgülle ayrılmış listesi VM'ler. Örneğin, *vm1, vm2, vm3*.<br> *WhatIf* çalıştırmadan runbook mantığının doğrular.|
|SequencedStartStop_Parent | Eylem: Başlatma veya durdurma <br> WhatIf: True veya False<br>VMList| Adlı etiketler oluşturma **SequenceStart** ve **SequenceStop** dizisi Başlat/Durdur etkinliğinin istediğiniz her bir VM üzerinde. Etiket değeri başlatmak veya durdurmak istediğiniz sıranın karşılık gelen bir pozitif tamsayı (1, 2, 3) olmalıdır. <br> VMList: Virgülle ayrılmış listesi VM'ler. Örneğin, *vm1, vm2, vm3*. <br> *WhatIf* çalıştırmadan runbook mantığının doğrular. <br> **Not**: VM'ler External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames ve External_ExcludeVMNames Azure Otomasyon değişkenleri tanımlanmış kaynak grubu içinde olmalıdır. Etkili olması eylemler için uygun etiketler olmaları gerekir.|

### <a name="variables"></a>Değişkenler

Aşağıdaki tabloda, Automation hesabında oluşturulan değişkenleri listeler. Önekine sahip değişkenler yalnızca değiştirmelisiniz **dış**. Değişkenleri değiştirme önekiyle **dahili** istenmeyen etkilere neden oluyor.

|**değişken** | **Açıklama**|
---------|------------|
|External_AutoStop_Condition | Bir uyarı tetiklemeden önce koşul yapılandırmak için gerekli koşullu işleç. Kabul edilebilir değerler **GreaterThan**, **GreaterThanOrEqual**, **LessThan**, ve **LessThanOrEqual**.|
|External_AutoStop_Description | CPU yüzdesi eşiği aşarsa VM durdurmak için uyarı.|
|External_AutoStop_MetricName | Azure uyarı kuralı yapılandırılacak olduğu performans ölçüm adı.|
|External_AutoStop_Threshold | Değişkeninde belirtilen Azure uyarı kuralı eşiği *External_AutoStop_MetricName*. Yüzde değerleri 1 ile 100 arasında olabilir.|
|External_AutoStop_TimeAggregationOperator | Koşulu değerlendirmek için seçilen pencere boyutu uygulandığı zaman Toplama işleci. Kabul edilebilir değerler **ortalama**, **Minimum**, **maksimum**, **toplam**, ve **son**.|
|External_AutoStop_TimeWindow | Pencere boyutunu sırasında Azure bir uyarıyı tetiklemek için seçilen ölçümleri analiz eder. Bu parametre timespan biçim girişinde kabul eder. Olası değerler 5 dakika ile 6 saat olur.|
|External_EmailFromAddress | Gönderenin e-postanın belirtir.|
|External_EmailSubject | E-postanın konu satırı metnini belirtir.|
|External_EmailToAddress | E-posta alıcılarını belirtir. Adları virgül kullanarak ayırın.|
|External_ExcludeVMNames | Hariç tutulacak VM adları, boşluk virgül kullanarak adları ayırarak girin.|
|External_IsSendEmail | Tamamlandıktan sonra e-posta bildirim gönderme seçeneği belirtir. Belirtin **Evet** veya **Hayır** e-posta göndermek için. Bu seçenek olmalıdır **Hayır** ilk dağıtım sırasında e-posta bildirimleri etkinleştirmediyseniz.|
|External_Start_ResourceGroupNames | Değerleri başlatma eylemleri için hedeflenen virgül kullanarak ayırarak bir veya daha fazla kaynak grupları belirtir.|
|External_Stop_ResourceGroupNames | Stop eylemler için hedeflenen virgül kullanarak değerleri ayırarak bir veya daha fazla kaynak grupları belirtir.|
|Internal_AutomationAccountName | Otomasyon hesabının adını belirtir.|
|Internal_AutoSnooze_WebhookUri | Web kancası URI DISPLAYFILTER senaryo için adlı belirtir.|
|Internal_AzureSubscriptionId | Azure abonelik kimliğini belirtir.|
|Internal_ResourceGroupName | Otomasyon hesabı kaynak grubu adı belirtir.|
|Internal_SendGridAccountName | SendGrid hesap adını belirtir.|
|Internal_SendGridPassword | SendGrid hesabının parolasını belirtir.|

Tüm senaryolar arasında **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, ve **External_ExcludeVMNames** değişkenlerdir gerekli VM'ler için virgülle ayrılmış bir listesini sağlayan hariç olmak üzere sanal makineleri, hedefleme için **AutoStop_CreateAlert_Parent**, **SequencedStartStop_Parent**, ve  **ScheduledStartStop_Parent** runbook'lar. Diğer bir deyişle, Vm'leriniz, başlangıç için hedef kaynak grupları bulunması gerekir ve gerçekleşecek Eylemler durdurun. Azure ilke için abonelik veya kaynak grubu hedef ve Eylemler sahip benzer mantığı works yeni oluşturulan VM'ler tarafından devralınır. Bu yaklaşım, her VM için ayrı bir zamanlama korumak ve başlatır yönetme gereğini ortadan kaldırır ve ölçek durdurur.

### <a name="schedules"></a>Zamanlamalar

Aşağıdaki tabloda her Otomasyon hesabınızda oluşturulan varsayılan zamanlamalar listeler.  Bunları değiştirebilir veya kendi özel zamanlamalar oluşturabilirsiniz. Varsayılan olarak, her birini devre dışı bırakılır dışında **Scheduled_StartVM** ve **Scheduled_StopVM**.

Bu çakışan zamanlama eylemleri oluşturabilirsiniz çünkü tüm zamanlamalar, etkinleştirmemeniz gerekir. Gerçekleştirmek ve buna uygun olarak değiştirmek için istediğiniz hangi iyileştirmeleri belirlemek en iyisidir. Örnek senaryolar genel bakış bölümünde daha fazla açıklama için bkz.

|**Zamanlama adı** | **Sıklık** | **Açıklama**|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | 8 saatte bir | Azure Otomasyonu değişken VM tabanlı değerleri External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames ve External_ExcludeVMNames sırayla durdurur AutoStop_CreateAlert_Parent runbook her 8 saatte çalışır. Alternatif olarak, VMList parametresini kullanarak sanal makineleri virgülle ayrılmış bir listesini belirtebilirsiniz.|
|Scheduled_StopVM | Kullanıcı tanımlı, her gün | Scheduled_Parent runbook bir parametreyle çalıştırır *durdurmak* her gün belirtilen zamanda. Varlık değişkenleri tarafından tanımlanmış kurallarını karşılayan tüm sanal makineleri otomatik olarak durdurur. İlgili zamanlama etkinleştirmelisiniz **zamanlanmış-StartVM**.|
|Scheduled_StartVM | Kullanıcı tanımlı, her gün | Scheduled_Parent runbook bir parametreyle çalıştırır *Başlat* her gün belirtilen zamanda.  Uygun değişkenleri tarafından tanımlanmış kurallarını karşılayan tüm sanal makineleri otomatik olarak başlar. İlgili zamanlama etkinleştirmelisiniz **zamanlanmış StopVM**.|
|Sıralı StopVM | 1: 00'da (UTC), her Cuma | Sequenced_Parent runbook bir parametreyle çalıştırır *durdurmak* her Cuma belirtilen saatte. Sırayla bir etiketi olan tüm VM'ler durdurur (artan) **SequenceStop** uygun değişkenleri tarafından tanımlanmış. Etiket değerlerini ve varlık değişkenleri hakkında daha fazla ayrıntı için runbook'ları bölümüne bakın. İlgili zamanlama etkinleştirmelisiniz **sıralı-StartVM**.|
|Sıralı StartVM | 1:00 PM (UTC), her Pazartesi | Sequenced_Parent runbook bir parametreyle çalıştırır *Başlat* belirtilen saatte her Pazartesi. Sırayla bir etiketle başlayan tüm sanal makineleri (Azalan) **SequenceStart** uygun değişkenleri tarafından tanımlanmış. Etiket değerlerini ve varlık değişkenleri hakkında daha fazla ayrıntı için runbook'ları bölümüne bakın. İlgili zamanlama etkinleştirmelisiniz **sıralı StopVM**.|

## <a name="log-analytics-records"></a>Log Analytics kayıtları

Otomasyon için günlük analizi çalışma alanında iki tür kayıtları oluşturur: İş günlükleri ve iş akışları.

### <a name="job-logs"></a>İş günlükleri

Özellik | Açıklama|
----------|----------|
Çağıran |  İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
Kategori | Veri türü sınıflandırması. Otomasyon için değer JobLogs olacaktır.|
CorrelationId | Runbook işi bağıntı kimliği GUID.|
JobId | Runbook işi kimliği GUID.|
operationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için iş bir değerdir.|
resourceId | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir. Otomasyon için değer, Azure Otomasyonu olacaktır.|
ResourceType | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
resultType | Runbook işinin durumudur. Olası değerler şunlardır:<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>- Başarılı oldu|
resultDescription | Runbook iş sonucu durumunu açıklar. Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı|
RunbookName | Runbook’un adını belirtir.|
SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir. Otomasyon için OpsManager değerdir|
StreamType | Olay türünü belirtir. Olası değerler şunlardır:<br>- Ayrıntılı<br>- Çıktı<br>- Hata<br>- Uyarı|
SubscriptionId | İşin abonelik kimliğini belirtir.
Zaman | Runbook işinin yürütüldüğü tarih ve saat.|

### <a name="job-streams"></a>İş akışları

Özellik | Açıklama|
----------|----------|
Çağıran |  İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
Kategori | Veri türü sınıflandırması. Otomasyon için değer JobStreams olacaktır.|
JobId | Runbook işi kimliği GUID.|
operationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için iş bir değerdir.|
ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
resourceId | Kaynak Kimliği ile Azure belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir. Otomasyon için değer, Azure Otomasyonu olacaktır.|
ResourceType | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
resultType | Runbook işinin, olayın oluşturulduğu andaki sonucu. Olası bir değer verilmiştir:<br>- Devam Ediyor|
resultDescription | Runbook’un çıktı akışını içerir.|
RunbookName | Runbook’un adı.|
SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir. Otomasyon için OpsManager bir değerdir.|
StreamType | İş akışı türü. Olası değerler şunlardır:<br>-İlerleme durumu<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı|
Zaman | Runbook işinin yürütüldüğü tarih ve saat.|

Kategori kayıtlarını döndürür tüm günlük arama yaptığınızda **JobLogs** veya **JobStreams**, seçebileceğiniz **JobLogs** veya **JobStreams**araması tarafından döndürülen güncelleştirmeleri özetlemeye döşeme kümesini görüntüler görünümü.

## <a name="sample-log-searches"></a>Örnek günlük aramaları

Aşağıdaki tabloda, bu çözüm tarafından toplanan iş kayıtlarına ilişkin örnek günlük aramaları sunulmaktadır.

Sorgu | Açıklama|
----------|----------|
Runbook başarıyla tamamlandı ScheduledStartStop_Parent işleri bulma | Kategori arama "JobLogs" == &#124; burada (RunbookName_s "ScheduledStartStop_Parent" ==) &#124; burada (ResultType "Tamamlandı" ==) &#124; AggregatedValue özetlemek ResultType, bin (TimeGenerated, 1 h) tarafından count() = &#124; TimeGenerated göre sırala desc|
Runbook başarıyla tamamlandı SequencedStartStop_Parent işleri bulma | Kategori arama "JobLogs" == &#124; burada (RunbookName_s "SequencedStartStop_Parent" ==) &#124; burada (ResultType "Tamamlandı" ==) &#124; AggregatedValue özetlemek ResultType, bin (TimeGenerated, 1 h) tarafından count() = &#124; TimeGenerated göre sırala desc

## <a name="viewing-the-solution"></a>Çözüm görüntüleme

Çözüm erişmek için Otomasyon hesabı seçin gidin **çalışma** altında **ilgili kaynaklar**. Günlük analizi sayfasında seçin **çözümleri** altında **genel**. Üzerinde **çözümleri** sayfasında, çözümü seçin **Start-Stop-VM [çalışma]** listeden.

Çözümü seçme görüntüler **Start-Stop-VM [çalışma]** çözüm sayfası. Burada önemli ayrıntılar gibi gözden geçirebilirsiniz **StartStopVM** döşeme. Günlük analizi çalışma alanı olduğu gibi bu kutucuğu bir sayısı ve çözüm için başlamış olması ve başarıyla tamamlandı runbook işleri grafik gösterimi görüntüler.

![Otomasyon güncelleştirme yönetimi çözümü sayfası](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Buradan, halka kutucuğa tıklayarak daha fazla iş kaydı analizini gerçekleştirebilirsiniz. Çözüm Panosu günlük arama sorguları önceden tanımlanmış ve iş geçmişini gösterir. Aramak için günlük Advanced Analytics portalı geçin, arama sorguları temel alan.

## <a name="configure-email-notifications"></a>E-posta bildirimleri yapılandırma

Çözüm dağıtıldıktan sonra e-posta bildirimlerini yapılandırmak için aşağıdaki üç değişkenleri değiştirin:

* External_EmailFromAddress: gönderenin e-posta adresi belirtin.
* External_EmailToAddress: e-posta adreslerini virgülle ayrılmış bir listesini belirtin (user@hotmail.com, user@outlook.com) bildirim e-postaları almak için.
* External_IsSendEmail: Kümesine **Evet** e-postaları için. E-posta bildirimlerini devre dışı bırakmak için değer kümesine **Hayır**.

## <a name="modify-the-startup-and-shutdown-schedules"></a>Başlatma ve kapatma zamanlamalarını Değiştir

Bu çözümdeki başlatma ve kapatma zamanlamaları yönetme izleyen adımların aynısını kısmında özetlendiği gibi [Azure Otomasyon runbook'u zamanlama](automation-schedules.md).

Yalnızca belirli bir süre sonunda sanal makineleri durdurmak için Çözüm yapılandırılırken desteklenir. Bunu yapmanız için gerekenler:

1. In kapanması VM'ler için kaynak gruplarını eklediğiniz olun **External_Start_ResourceGroupNames** değişkeni.
2. Sanal makineleri kapatmak için istediğiniz zaman kendi zamanlamasını oluşturun.
3. Gidin **ScheduledStartStop_Parent** runbook tıklatıp **zamanlama**. Bu önceki adımda oluşturduğunuz zamanlamayı seçmenize olanak sağlar.
4. Seçin **parametreler ve çalıştırma ayarları** ve "Durdur" için eylem parametresi olarak ayarlayın.
5. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın.

## <a name="update-the-solution"></a>Güncelleştirme çözümü

Bu çözüm önceki bir sürümünü dağıttıysanız, bu hesabınızdan güncelleştirilmiş bir sürümü dağıtmadan önce silmeniz gerekir. Adımlarını izleyin [çözümü kaldırmak](#remove-the-solution) ve yukarıdaki adımları izleyin [çözümü dağıtmak](#deploy-the-solution).

## <a name="remove-the-solution"></a>Çözüm Kaldır

Artık çözümü kullanmanıza gerek karar verirseniz, Otomasyon hesabı silebilirsiniz. Çözüm silme runbook'ları yalnızca kaldırır. Zamanlamaları veya çözümü eklediğinizde, oluşturulan değişkenleri silmez. Bu varlıkları, bunları ile diğer runbook'ları kullanmıyorsanız el ile silmeniz gerekiyor.

Çözümü silmek için aşağıdaki adımları gerçekleştirin:

1. Otomasyon hesabınızdan seçin **çalışma** sol sayfasından.
1. Üzerinde **çözümleri** sayfasında, çözümü seçin **Start-Stop-VM [çalışma]**. Üzerinde **VMManagementSolution [çalışma]** menüsünden seçin sayfasında **silmek**.<br><br> ![VM yönetimi çözümü Sil](media/automation-solution-vm-management/vm-management-solution-delete.png)
1. İçinde **silmek çözüm** penceresinde çözümü silmek istediğinizi onaylayın.
1. Bilgi doğrulanır ve çözüm silinmiş olsa da, altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde. Döndürülürsünüz **çözümleri** çözümü kaldırma işlemini başladıktan sonra sayfa.

Otomasyon hesabı ve günlük analizi çalışma alanı bu işlemin bir parçası olarak silinmez. Günlük analizi çalışma alanı korumak istemiyorsanız, el ile silmeniz gerekiyor. Bu Azure portalından gerçekleştirilebilir:

1. Azure portal giriş ekranından, seçin **günlük analizi**.
1. Üzerinde **günlük analizi** sayfasında, çalışma alanını seçin.
1. Seçin **silmek** çalışma alanı ayarları sayfası menüsünden.

## <a name="next-steps"></a>Sonraki adımlar

* Farklı arama sorguları oluşturmak ve günlük analizi ile Otomasyon iş günlükleri gözden geçirmek hakkında daha fazla bilgi için bkz: [günlük analizi aramaları oturum](../log-analytics/log-analytics-log-searches.md).
* Runbook yürütme, runbook işlerini izleme ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Runbook işi izleme](automation-runbook-execution.md).
* Günlük analizi ve veri toplama kaynakları hakkında daha fazla bilgi için bkz: [toplama Azure storage veri günlük analizi genel bakış](../log-analytics/log-analytics-azure-storage.md).
