---
title: Azure Log Analytics'teki çalışma alanlarını yönetme | Microsoft Docs
description: Kullanıcılar, hesaplar, çalışma alanları ve Azure hesapları ile ilgili çeşitli yönetim görevlerini kullanarak Azure Log Analytics'teki çalışma alanlarını yönetebilirsiniz.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2017
ms.author: magoedte
ms.openlocfilehash: 25a68fb535300e80efdf2adf9f3a8afe1b304667
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="manage-workspaces"></a>Çalışma alanlarını yönetme

Log Analytics'e erişimi yönetmek için çalışma alanları ile ilgili çeşitli yönetim görevleri gerçekleştirirsiniz. Bu makalede çalışma alanlarını yönetmeye yönelik en iyi uygulama önerileri ve yordamları verilmektedir. Çalışma alanı, temelde hesap bilgilerini ve hesaba ilişkin basit yapılandırma bilgilerini içeren bir kapsayıcıdır. Siz veya kuruluşunuzun diğer üyeleri, IT altyapınızın tümünden veya bir bölümünden toplanan farklı veri kümelerini yönetmek için birden çok çalışma alanı kullanabilirsiniz.

Bir çalışma alanı oluşturmak için şunlar gereklidir:

1. Bir Azure aboneliğine sahip olmanız.
2. Bir çalışma alanı adı seçmeniz.
3. Çalışma alanını aboneliğinizle ilişkilendirmeniz.
4. Coğrafi bir konum seçmeniz.

## <a name="determine-the-number-of-workspaces-you-need"></a>İhtiyacınız olan çalışma alanı sayısını belirleme
Çalışma alanı, bir Azure kaynağıdır. Bu alan, verilerin toplandığı, derlendiği, çözümlendiği ve Azure portalında sunulduğu bir kapsayıcıdır.

Azure aboneliği başına birden çok çalışma alanına sahip olabilir ve birden çok çalışma alanına erişebilirsiniz. Daha önce verileri yalnızca geçerli çalışma alanından çözümleyebiliyordunuz ve bu, aboneliğinizde tanımlanmış olan birden çok çalışma alanında sorgu yapabilmenizi kısıtlıyordu. Artık [birden çok çalışma alanında sorgu yapabilirsiniz](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search), bu da verilerinize sistem genelinde bir görünüm sunar. Bu bölümde birden çok çalışma alanı oluşturmanın yararlı olabileceği durumlar açıklanır.

Günümüzde bir çalışma alanı aşağıdakileri sağlar:

* Veri depolama için coğrafi bir konum
* Fatura için ayrıntı düzeyi
* Veri yalıtımı
* Yapılandırma kapsamı

Önceki özelliklere bağlı olarak, şu durumlarda birden çok çalışma alanı oluşturmak isteyebilirsiniz:

* Global bir şirketseniz ve veri bağımsızlığı veya uyumluluk nedenleriyle verilerin belirli bölgelerde depolanmasına gerek duyuyorsanız.
* Azure kullanıyorsanız ve çalışma alanını, yönettiği Azure kaynaklarıyla aynı bölgede bulundurarak giden veri aktarımı ücretlerini ortadan kaldırmak istiyorsanız.
* Kullanımlarına dayalı olarak, ücretleri farklı departmanlara veya iş gruplarına göre ayırmak istiyorsanız. Her departman veya iş grubu için bir çalışma alanı oluşturduğunuzda, Azure faturanız ve kullanım ekstreniz her çalışma alanına ilişkin ücretleri ayrı olarak gösterir.
* Yönetilen bir hizmet sağlayıcısıysanız ve yönettiğiniz her bir müşteriye ilişkin Log Analytics verilerini diğer müşterilerin verilerinden yalıtmak istiyorsanız.
* Birden çok müşteriyi yönetiyorsanız ve her bir müşterinin/departmanın/iş grubunun yalnızca kendi verilerini görmesini istiyorsanız.

Verileri toplamak için aracıları kullanıyorsanız [her bir aracıyı, bir veya daha fazla çalışma alanına raporlama yapacak şekilde yapılandırabilirsiniz](log-analytics-windows-agent.md).

System Center Operations Manager'ı kullanıyorsanız her bir Operations Manager yönetim grubu yalnızca bir çalışma alanıyla bağlantılı olabilir. Ancak, bilgisayardaki Microsoft Monitoring Agent, hem Operations Manager hem de farklı bir Log Analytics çalışma alanına rapor verecek şekilde yapılandırılabilir.  

### <a name="workspace-information"></a>Çalışma alanı bilgileri

Azure portalında çalışma alanınızla ilgili bilgileri görüntüleyebilirsiniz. 

#### <a name="view-workspace-information-in-the-azure-portal"></a>Çalışma alanı bilgilerini Azure portalında görüntüleme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Tüm hizmetler**’e tıklayın.  Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i tıklayın.  
    ![Azure sol menüsünü gösteren ekran görüntüsü](./media/log-analytics-manage-access/hub.png)  
3. Log Analytics abonelikler sayfasında bir çalışma alanı seçin.
4. Çalışma alanı sayfasında çalışma alanıyla ilgili ayrıntılar ve ek bilgi bağlantıları gösterilir.  
    ![çalışma alanı ayrıntıları](./media/log-analytics-manage-access/workspace-details.png)  


## <a name="manage-accounts-and-users"></a>Hesapları ve kullanıcıları yönetme
Her çalışma alanı kendisiyle ilişkilendirilmiş birden çok hesap içerebilir ve her hesap (Microsoft hesabı veya Kuruluş hesabı) birden çok çalışma alanına erişim sahibi olabilir.

Çalışma alanını oluşturan Microsoft hesabı veya Kuruluş hesabı, varsayılan olarak çalışma alanının yöneticisi olur.

Bir Log Analytics çalışma alanına erişimi denetleyen iki izin modeli vardır:

1. Eski Log Analytics kullanıcı rolleri
2. [Azure rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md)

Aşağıdaki tabloda her bir izin modeli kullanılarak ayarlanabilen erişim özellikleri özetlenmektedir:

|                          | Log Analytics portalı | Azure portalına | API (PowerShell dahil) |
|--------------------------|----------------------|--------------|----------------------------|
| Log Analytics kullanıcı rolleri | Yes                  | Hayır           | Hayır                         |
| Azure rol tabanlı erişim  | Yes                  | Yes          | Yes                        |

> [!NOTE]
> Log Analytics, izin modeli olarak Log Analytics kullanıcı rolleri yerine Azure rol tabanlı erişime geçiş yapıyor.
>
>

Eski Log Analytics kullanıcı rolleri yalnızca [Log Analytics portalında](https://mms.microsoft.com) gerçekleştirilen eylemlere erişimi denetler.

Şu etkinlikler de Azure izinleri gerektirir:

| Eylem                                                          | Gereken Azure İzni | Notlar |
|-----------------------------------------------------------------|--------------------------|-------|
| Yönetim çözümlerini ekleme ve kaldırma                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | |
| Fiyatlandırma katmanını değiştirme                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| *Backup* ve *Site Recovery* çözüm kutucuklarındaki verileri görüntüleme | Yönetici / Ortak yönetici | Klasik dağıtım modeli kullanılarak dağıtılan kaynaklara erişir |
| Azure portalında bir çalışma alanı oluşturma                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Azure izinlerini kullanarak Log Analytics’e erişimi yönetme
Azure izinlerini kullanarak Log Analytics çalışma alanına izin vermek için, [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](../role-based-access-control/role-assignments-portal.md) bölümündeki adımları izleyin.

Azure Log Analytics için iki yerleşik kullanıcı rolüne sahiptir:
- Log Analytics Okuyucusu
- Log Analytics Katkıda Bulunan

*Log Analytics Okuyucusu* rolünün üyeleri aşağıdakileri yapabilir:
- Tüm izleme verilerini görüntüleme ve arama 
- Tüm Azure kaynakları üzerinde Azure tanılama yapılandırmasını görüntüleme dahil olmak üzere, izleme ayarlarını görüntüleyin.

| Tür    | İzin | Açıklama |
| ------- | ---------- | ----------- |
| Eylem | `*/read`   | Tüm kaynakların ve kaynak yapılandırmalarının görüntülenmesine imkan sağlar. Aşağıdakileri görüntülemeyi içerir: <br> Sanal makine uzantısı durumu <br> Kaynaklarda Azure tanılamalarının yapılandırması <br> Tüm kaynakların tüm özellikleri ve ayarları |
| Eylem | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Günlük araması v2 sorguları gerçekleştirme becerisi |
| Eylem | `Microsoft.OperationalInsights/workspaces/search/action` | Günlük araması v1 sorguları gerçekleştirme becerisi |
| Eylem | `Microsoft.Support/*` | Destek olayları açma özelliği |
|Eylem Dışı | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Veri koleksiyonu API’sini kullanmak ve aracıları yüklemek için gereken çalışma alanının okunmasını engeller |


*Log Analytics Katkıda Bulunan* rolünün üyeleri aşağıdakileri yapabilir:
- Tüm izleme verilerini okuma 
- Otomasyon hesaplarını oluşturma ve yapılandırma
- Yönetim çözümlerini ekleme ve kaldırma
- Depolama hesabı anahtarlarını okuma 
- Azure Depolama’dan günlüklerin toplanmasını yapılandırma
- Azure kaynaklarına ilişkin izleme ayarlarını düzenleme:
  - VM'lere VM uzantısı ekleme
  - Tüm Azure kaynaklarında Azure tanılamayı yapılandırma

> [!NOTE] 
> Özelliği, bir sanal makine üzerinde tam denetim kazanmak için bir sanal makineye sanal makine uzantısı eklemek için kullanabilirsiniz.

| İzin | Açıklama |
| ---------- | ----------- |
| `*/read`     | Tüm kaynakların ve kaynak yapılandırmalarının görüntülenmesine imkan sağlar. Aşağıdakileri görüntülemeyi içerir: <br> Sanal makine uzantısı durumu <br> Kaynaklarda Azure tanılamalarının yapılandırması <br> Tüm kaynakların tüm özellikleri ve ayarları |
| `Microsoft.Automation/automationAccounts/*` | Runbook'ları ekleme ve düzenleme dahil olmak üzere Azure Otomasyonu hesapları oluşturma ve yapılandırma olanağı |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Linux uzantısı için OMS aracısı ve Microsoft Monitoring Agent gibi sanal makine uzantılarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Depolama hesabı anahtarını görüntüleyin. Log Analytics’i Azure depolama hesaplarındaki günlükleri okuyacak şekilde yapılandırmak için gereklidir |
| `Microsoft.Insights/alertRules/*` | Uyarı kurallarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.Insights/diagnosticSettings/*` | Azure kaynaklarında tanılama ayarlarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.OperationalInsights/*` | Log Analytics çalışma alanları için yapılandırmaları ekleme, güncelleştirme ve kaldırma |
| `Microsoft.OperationsManagement/*` | Yönetim çözümlerini ekleme ve kaldırma |
| `Microsoft.Resources/deployments/*` | Dağıtımları oluşturma ve silme. Çözümleri, çalışma alanlarını ve otomasyon hesaplarını eklemek ve kaldırmak için gereklidir |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Dağıtımları oluşturma ve silme. Çözümleri, çalışma alanlarını ve otomasyon hesaplarını eklemek ve kaldırmak için gereklidir |

Kullanıcıları bir kullanıcı rolüne eklemek ve bu rolden kaldırmak için `Microsoft.Authorization/*/Delete` ve `Microsoft.Authorization/*/Write` izinleri gereklidir.

Bu rolleri, kullanıcılara farklı kapsamlarda erişim vermek için kullanın:
- Abonelik - Abonelikteki tüm çalışma alanlarına erişim
- Kaynak grubu - Kaynak grubundaki tüm çalışma alanına erişim
- Kaynak - Yalnızca belirtilen çalışma alanına erişim

Gereken özel izinlere sahip rolleri oluşturmak için [özel rolleri](../role-based-access-control/custom-roles.md) kullanın.

### <a name="azure-user-roles-and-log-analytics-portal-user-roles"></a>Azure kullanıcı rolleri ve Log Analytics portalı kullanıcı rolleri
Log Analytics çalışma alanında en az Azure okuma izniniz varsa, Log Analytics çalışma alanını görüntülerken **OMS Portal** görevine tıklayarak OMS portalını açabilirsiniz.

OMS portalını açarken, eski Log Analytics kullanıcı rollerine geçiş yaparsınız. Log Analytics portalında bir rol atamanız yoksa, hizmet [çalışma alanında sahip olduğunuz Azure izinlerini denetler](https://docs.microsoft.com/rest/api/authorization/permissions#Permissions_ListForResource).
OMS portalındaki rol atamanız aşağıdaki şekilde belirlenir:

| Koşullar                                                   | Atanan Log Analytics kullanıcı rolü | Notlar |
|--------------------------------------------------------------|----------------------------------|-------|
| Hesabınız eski bir Log Analytics kullanıcı rolüne ait     | Belirtilen Log Analytics kullanıcı rolü | |
| Hesabınız eski bir Log Analytics kullanıcı rolüne ait değil <br> Çalışma alanı için tam Azure izinleri (`*` izin <sup>1</sup>) | Yönetici ||
| Hesabınız eski bir Log Analytics kullanıcı rolüne ait değil <br> Çalışma alanı için tam Azure izinleri (`*` izin <sup>1</sup>) <br> `Microsoft.Authorization/*/Delete` ve `Microsoft.Authorization/*/Write` *eylemleri değil* | Katılımcı ||
| Hesabınız eski bir Log Analytics kullanıcı rolüne ait değil <br> Azure okuma izni | Salt Okunur ||
| Hesabınız eski bir Log Analytics kullanıcı rolüne ait değil <br> Azure izinleri anlaşılmadı | Salt Okunur ||
| Bulut Çözümü Sağlayıcısı (CSP) tarafından yönetilen abonelikler için <br> Oturum açtığınız hesap, çalışma alanına bağlı Azure Active Directory’dedir | Yönetici | Genellikle bir CSP’nin müşterisi |
| Bulut Çözümü Sağlayıcısı (CSP) tarafından yönetilen abonelikler için <br> Oturum açtığınız hesap, çalışma alanına bağlı Azure Active Directory’de değildir | Katılımcı | Genellikle CSP |

<sup>1</sup> Rol tanımları hakkında daha fazla bilgi için [Azure izinlerine](../role-based-access-control/custom-roles.md) bakın. Roller değerlendirilirken, `*` eylemi `Microsoft.OperationalInsights/workspaces/*` öğesine eşit değildir.

Azure portalı hakkında dikkate alınması gereken bazı noktalar:

* http://mms.microsoft.com adresini kullanarak OMS portalında oturum açtığınızda **Çalışma alanı seçin listesini** görürsünüz. Bu liste yalnızca bir Log Analytics kullanıcı rolüne sahip olduğunuz çalışma alanlarını içerir. Azure abonelikleri ile erişim sahibi olduğunuz çalışma alanlarını görmek için, URL'nin parçası olarak bir kiracı belirtmeniz gerekir. Örneğin:  `mms.microsoft.com/?tenant=contoso.com`. Kiracı tanımlayıcısı, genellikle oturum açmak için kullandığınız e-posta adresinin bu son bölümüdür.
* Azure izinlerini kullanarak, doğrudan erişim sahibi olduğunuz bir portala gitmek istiyorsanız URL'nin parçası olarak kaynağı belirtmeniz gerekir. Bu URL'yi PowerShell kullanarak elde edebilirsiniz.

  Örneğin, `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  URL şuna benzer: `https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`

### <a name="managing-users-in-the-oms-portal"></a>OMS portalında kullanıcıları yönetme
Ayarlar sayfasının **Hesaplar** sekmesinin altında yer alan **Kullanıcıları Yönet** sekmesinde kullanıcıları ve grupları yönetebilirsiniz.   

![Kullanıcıları yönetme](./media/log-analytics-manage-access/setup-workspace-manage-users.png)


#### <a name="add-a-user-to-an-existing-workspace"></a>Mevcut bir çalışma alanına kullanıcı ekleme
Bir çalışma alanına kullanıcı veya grup eklemek için aşağıdaki adımları kullanın.

1. OMS portalında **Ayarlar** kutucuğuna tıklayın.
2. **Hesaplar** sekmesine ve ardından **Kullanıcıları Yönet** sekmesine tıklayın.
3. **Kullanıcıları Yönet** bölümünde, eklenecek hesap türünü seçin: **Kuruluş Hesabı**, **Microsoft Hesabı** ve **Microsoft Destek**.

   * Microsoft Hesabı seçeneğini belirlerseniz Microsoft Hesabı ile ilişkili kullanıcının e-posta adresini yazın.
   * Kuruluş Hesabı seçeneğini belirlerseniz kullanıcı / grup adının bir bölümünü ya da e-posta diğer adını girin. Ardından açılan bir kutuda eşleşen kullanıcıların ve grupların listesi görünür. Bir kullanıcı veya grup seçin.
   * Sorun gidermeye yardımcı olması için bir Microsoft Destek mühendisine veya başka bir Microsoft çalışanına, çalışma alanınıza geçici erişim izni vermek üzere Microsoft Destek seçeneğini kullanın.

     > [!NOTE]
     > En iyi performansı elde etmek için, tek bir OMS hesabı ile ilişkili Active Directory gruplarının sayısını; yöneticiler için, katkıda bulunanlar için ve yalnızca okuma erişimi olan kullanıcılar için birer grup olacak şekilde üç grup ile sınırlayın. Daha fazla grubun kullanılması, Log Analytics performansını etkileyebilir.
     >
     >
4. Eklenecek kullanıcının veya grubun türünü seçin: **Yönetici**, **Katkıda Bulunan** ya da **Yalnızca Okuma Erişimi Olan Kullanıcı**.  
5. **Ekle**'ye tıklayın.

   Bir Microsoft hesabı ekliyorsanız sağladığınız e-posta adresine, çalışma alanına katılmaya yönelik bir davet gönderilir. Kullanıcı OMS’ye katılmak için davetteki yönergeleri uyguladıktan sonra, bu çalışma alanına erişebilir.
   Bir kuruluş hesabı ekliyorsanız kullanıcı Log Analytics'e hemen erişebilir.  

#### <a name="edit-an-existing-user-type"></a>Mevcut bir kullanıcı türünü düzenleme
OMS hesabınızla ilişkili bir kullanıcı için hesap rolünü değiştirebilirsiniz. Şu rol seçeneklerine sahipsiniz:

* *Yönetici*: Kullanıcıları yönetebilir, tüm uyarıları görüntüleyip bu uyarılar üzerinde işlem yapabilir ve sunucu ekleyip kaldırabilir
* *Katkıda Bulunan*: Tüm uyarıları görüntüleyip bu uyarılar üzerinde işlem yapabilir ve sunucu ekleyip kaldırabilir
* *Yalnızca Okuma Erişimi Olan Kullanıcı*: Yalnızca okuma erişimine sahip olarak işaretlenen kullanıcılar şunları yapamaz:

  1. Çözüm ekleme/kaldırma. Çözüm galerisi gizlidir.
  2. **Panom** üzerinde kutucuk ekleme/değiştirme/kaldırma.
  3. **Ayarlar** sayfalarını görüntüleme. Sayfalar gizlidir.
  4. Arama görünümünde; Power BI yapılandırması, Kaydedilen Aramalar ve Uyarılar görevleri gizlidir.

#### <a name="to-edit-an-account"></a>Bir hesabı düzenleme
1. OMS portalında **Ayarlar** kutucuğuna tıklayın.
2. **Hesaplar** sekmesine ve ardından **Kullanıcıları Yönet** sekmesine tıklayın.
3. Değiştirmek istediğiniz kullanıcı için rolü seçin.
4. Onay iletişim kutusunda, **Evet**'e tıklayın.

### <a name="remove-a-user-from-a-workspace"></a>Çalışma alanından bir kullanıcıyı kaldırma
Çalışma alanından bir kullanıcıyı kaldırmak için aşağıdaki adımları izleyin. Kullanıcının kaldırılması çalışma alanını kapatmaz. Bunun yerine, kullanıcı ve çalışma alanı arasındaki ilişki kaldırılır. Bir kullanıcı birden çok çalışma alanıyla ilişkilendirilmişse OMS'de oturum açmaya ve diğer çalışma alanlarını görmeye devam eder.

1. OMS portalında **Ayarlar** kutucuğuna tıklayın.
2. **Hesaplar** sekmesine ve ardından **Kullanıcıları Yönet** sekmesine tıklayın.
3. Kaldırmak istediğiniz kullanıcı adının yanındaki **Kaldır** düğmesine tıklayın.
4. Onay iletişim kutusunda, **Evet**'e tıklayın.

### <a name="add-a-group-to-an-existing-workspace"></a>Mevcut bir çalışma alanına grup ekleme
1. Yukarıdaki "Mevcut bir çalışma alanına kullanıcı ekleme" bölümünde yer alan 1 ila 4. adımları uygulayın.
2. **Kullanıcı/Grup Seç** bölümünde **Grup**'u seçin.  
   ![mevcut bir çalışma alanına grup ekleme](./media/log-analytics-manage-access/add-group.png)
3. Eklemek istediğiniz grup için Görünen Ad veya E-posta adresi girin.
4. Liste sonuçlarından grubu seçin ve ardından **Ekle**'ye tıklayın.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Mevcut bir çalışma alanını Azure aboneliğine bağlama
26 Eylül 2016'dan sonra oluşturulan tüm çalışma alanları, oluşturma zamanında bir Azure aboneliğine bağlanmalıdır. Bu tarihten önce oluşturulan çalışma alanları, oturum açtığınızda bir aboneliğe bağlanmalıdır. Çalışma alanını Azure portalından oluşturduğunuzda veya çalışma alanınızı bir Azure aboneliğine bağladığınızda, Azure Active Directory'niz kuruluş hesabınız olarak bağlanır.

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Çalışma alanını OMS portalında bir Azure aboneliğine bağlama

- OMS portalında oturum açtığınızda, bir Azure aboneliği seçmeniz istenir. Çalışma alanınıza bağlamak istediğiniz aboneliği seçin ve ardından **Bağla**’ya tıklayın.  
    ![Azure aboneliğini bağlama](./media/log-analytics-manage-access/required-link.png)

    > [!IMPORTANT]
    > Bir çalışma alanını bağlamak için, Azure hesabınızın bağlamak istediğiniz çalışma alanına erişiminin olması gerekir.  Diğer bir deyişle Azure portalına erişmek için kullandığınız hesap, çalışma alanına erişmek için kullandığınız hesapla **aynı** olmalıdır. Aynı değilse bkz. [Mevcut bir çalışma alanına kullanıcı ekleme](#add-a-user-to-an-existing-workspace).

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Çalışma alanını, Azure portalında bir Azure aboneliğine bağlama
1. [Azure portal](http://portal.azure.com) oturum açın.
2. **Log Analytics**’e göz atın ve ardından bu seçeneği belirleyin.
3. Mevcut çalışma alanlarınızın listesini görürsünüz. **Ekle**'ye tıklayın.  
   ![çalışma alanlarının listesi](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4. **OMS Çalışma Alanı** altında, **Or link existing (Veya var olanı bağla)** seçeneğine tıklayın.  
   ![mevcut olanı bağlama](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5. **Gerekli ayarları yapılandır**'a tıklayın.  
   ![gerekli ayarları yapılandırma](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6. Henüz Azure hesabınıza bağlanmamış olan çalışma alanlarının listesini görürsünüz. Bir çalışma alanı seçin.  
   ![çalışma alanları seçme](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7. Gerekirse şu öğelere ilişkin değerleri değiştirebilirsiniz:
   * Abonelik
   * Kaynak grubu
   * Konum
   * Fiyatlandırma katmanı  
     ![değerleri değiştirme](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8. **Tamam**’a tıklayın. Çalışma alanı artık Azure hesabınıza bağlı.

> [!NOTE]
> Bağlamak istediğiniz çalışma alanını görmüyorsanız Azure aboneliğinizin, OMS portalını kullanarak oluşturduğunuz çalışma alanına erişimi yoktur.  OMS portalından bu hesaba erişim vermek için bkz. [Mevcut bir çalışma alanına kullanıcı ekleme](#add-a-user-to-an-existing-workspace).
>
>

## <a name="change-an-azure-active-directory-organization-for-a-workspace"></a>Çalışma alanı için Azure Active Directory Kuruluşunu değiştirme

Bir çalışma alanının Azure Active Directory kuruluşunu değiştirebilirsiniz. Azure Active Directory Kuruluşunun değiştirilmesi, bu dizinden çalışma alanına kullanıcı ve grup eklemenize olanak sağlar.

### <a name="to-change-the-azure-active-directory-organization-for-a-workspace"></a>Çalışma alanındaki Azure Active Directory Kuruluşunu değiştirmek için

1. OMS portalındaki Ayarlar sayfasında **Hesaplar**'a ve ardından **Kullanıcıları Yönet** sekmesine tıklayın.  
2. Kuruluş hesapları ile ilgili bilgileri gözden geçirin ve ardından **Kuruluşu Değiştir** düğmesine tıklayın.  
    ![kuruluşu değiştir](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Azure Active Directory etki alanınızın yöneticisine ilişkin kimlik bilgilerini girin. Daha sonra, çalışma alanınızın Azure Active Directory etki alanınıza bağlandığını belirten bir bildirim görürsünüz.  
    ![bağlanan çalışma alanı bildirimi](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

## <a name="next-steps"></a>Sonraki adımlar
* Çözümler tarafından toplanan ve bilgisayarlardan gönderilen veri hacimlerini nasıl çözümleyeceğinizi öğrenmek için bkz. [Veri kullanımını anlama](log-analytics-usage.md).
* İşlev eklemek ve veri toplamak için bkz. [Azure marketinden Log Analytics yönetim çözümleri ekleme](log-analytics-add-solutions.md).
