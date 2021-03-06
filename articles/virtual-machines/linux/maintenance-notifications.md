---
title: Azure Linux VM'ler için bakım bildirimleri işleme | Microsoft Docs
description: Azure'da çalışan Linux sanal makineleri için bakım bildirimleri görüntülemek ve Self Servis bakım başlatın.
services: virtual-machines-linux
documentationcenter: ''
author: zivraf
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: zivr
ms.openlocfilehash: b1b4720c64d2eaa7578def6eac8f8231e4664d53
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="handling-planned-maintenance-notifications-for-linux-virtual-machines"></a>Linux sanal makineleri için işleme planlı bakım bildirimleri

Azure güvenilirliği, performansı ve sanal makineler için konak altyapısının güvenliğini artırmak için güncelleştirmeleri düzenli olarak gerçekleştirir. Barındırma ortamı düzeltme eki uygulama veya yükseltme ve donanım yetkisini gibi değişiklikler güncelleştirmelerdir. Bu güncelleştirmeler çoğunu barındırılan sanal makineler için herhangi bir etkisi olmadan gerçekleştirilir. Ancak, güncelleştirmeler bir etkiye sahip olduğu durumlar vardır:

- Bakım yeniden başlatma gerektirmez, Azure VM konak güncelleştirilirken duraklatmak için yerinde geçiş kullanır.

- Bakım bir yeniden başlatma gerektirirse, ne zaman bunu planlı bakım, bir bildirim alın. Bu durumlarda, burada başlatabilirsiniz bakım kendiniz bir zaman penceresi verilir ne zaman çalıştığını sizin için.


Yeniden başlatma gerektiren bir planlı bakım Dalgalar zamanlanır. Her wave farklı bir kapsam (bölge) sahiptir.

- Bir bildirim müşterilere bir wave başlar. Varsayılan olarak, abonelik sahibi ve ikincil sahipler bildirim gönderilir. Daha fazla alıcı ve e-posta, SMS ve Web kancalarını, gibi Mesajlaşma seçenekleri için Azure kullanarak bildirimleri ekleyebileceğiniz [etkinlik günlüğü uyarıları](../../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).  
- Bildirim zamanında bir *Self Servis penceresi* kullanılabilir hale getirilir. Bu penceresi sırasında sanal makinelerinizin bu wave içerdiği bulmak ve önleyici bakım zamanlama kendi gereksinimlerine göre başlatın.
- Self Servis penceresinde sonra bir *zamanlanmış bakım penceresi* başlar. Bu pencereyi sırasında bir noktada Azure zamanlar ve gerekli bakım, sanal makine için geçerlidir. 

İki windows sahip amacı, bakım başlatmak ve ne zaman Azure bakım otomatik olarak başlatılacak bilerek sanal makinenizi yeniden başlatmanız için yeterli süre vermektir.


Bakım pencereleri Vm'leriniz için sorgu ve Self Servis bakım başlatmak için Azure portal, PowerShell, REST API ve CLI kullanın.

 > [!NOTE]
 > Bakım'ı başlatmayı deneyin ve isteği başarısız olursa, Azure VM olarak işaretler **atlandı**. Artık müşteri tarafından başlatılan Bakım seçeneğini kullanmanız mümkün olmayacaktır. VM, Azure tarafından zamanlanmış bakım aşamasında başlatılması gerekir.


 
## <a name="should-you-start-maintenance-using-during-the-self-service-window"></a>Bakım sırasında Self Servis penceresini kullanarak başlamanız gerekir?  

Aşağıdaki yönergeler, bu özelliği kullanın ve gerekir bakım kendi zamanda başlatmak isteyip karar vermek için yardımcı olmalıdır.

> [!NOTE] 
> Kendi kendine bakım, tüm Vm'leriniz için kullanılabilir olmayabilir. Öngörülü dağıtın, VM için kullanılabilir olup olmadığını belirlemek için Ara **Şimdi Başlat** bakım durumu. Self Servis bakım şu anda bulut Hizmetleri (Web/çalışan rolü), Service Fabric ve sanal makine ölçek kümeleri için kullanılabilir değil.


Kendi kendine bakım, kullanarak dağıtımları için önerilmez **kullanılabilirlik kümeleri** bu yüksek oranda kullanılabilir ayarlar, herhangi bir anda yalnızca tek bir güncelleştirme etki alanı burada etkilenir olduğundan. 
    - Azure tetikleyici bakım sağlar, ancak güncelleştirme etki alanları etkilenip sırasını mutlaka sıralı olarak gerçekleşmez olduğunu ve 30 dakikalık Duraklat güncelleştirme etki alanları arasında olduğunu unutmayın.
    - Kapasite (1/güncelleştirme etki alanı sayısı) bazıları geçici kaybı önemliyse, kolayca için bakım süresi boyunca ek örneklerini ayırarak dengelenebilmesi. 

**Verme** kendi kendine bakım aşağıdaki senaryolarda kullanın: 
    - Vm'leriniz sık kapatırsanız da el ile otomatik kapatma kullanarak veya izleyen bir zamanlama DevTest labs kullanarak bunu bakım durumu dönmek ve bu nedenle ek kesinti süresine neden.
    - Hangi bakım wave bitişinden önce silinecek bildiğiniz kısa süreli Vm'lerinde. 
    - Güncelleştirme sırasında sürdürülebilmesi için istenen yerel (kısa ömürlü) disk depolanan büyük durumuna sahip iş yükleri için. 
    - Burada, genellikle VM'yi yeniden boyutlandırın durumlarda, olarak bakım durumu geri döndürülemedi. 
    - Öngörülü yük devretme veya işleminizi iş yükü normal şekilde kapatılmasını bakım kapatma başlamadan 15 dakika önce etkinleştiren zamanlanmış olaylar benimseyen varsa

**Kullanım** zamanlanmış bakım aşamasında kesintisiz VM çalıştırmayı planlıyorsanız ve yukarıda belirtilen karşı göstergeleri hiçbiri geçerli kendi kendine bakım,. 

Kendi kendine bakım aşağıdaki durumlarda kullanılması idealdir:
    - Bir tam bakım penceresi yönetim ya da son müşteriye bildirmeniz gerekir. 
    - Belirli bir tarihte bakım tamamlamanız gerekir. 
    - Bakım, örn., Güvenli Kurtarma güvence altına almak için çok katmanlı uygulama dizisini denetlemeniz gerekir.
    - 30 dakikadan fazla VM kurtarma süresini iki güncelleştirme etki alanı (UDs) arasında gerekir. Güncelleştirme etki alanları arasındaki zaman denetlemek için bakım aynı anda sanal makineleri bir güncelleştirme etki alanınızda (UD) tetiklemek gerekir.



## <a name="find-vms-scheduled-for-maintenance-using-cli"></a>CLI kullanarak bakım için zamanlanmış sanal makineleri Bul

Planlı bakım bilgileri kullanarak görülme [azure vm get örneği Görünüm](/cli/azure/vm?view=azure-cli-latest#az_vm_get_instance_view).
 
Yalnızca planlı bakım ise bakım bilgileri döndürülür. Varsa bu etkileri VM herhangi bir bakım zamanlanmış, komut herhangi bir bakım bilgi döndürmez. 

```azure-cli
az vm get-instance-view -g rgName -n vmName
```

Aşağıdaki değerleri MaintenanceRedeployStatus altında döndürülür: 

| Değer | Açıklama   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Bakım VM üzerinde şu anda başlatabilirsiniz olup olmadığını gösterir ||
| PreMaintenanceWindowStartTime         | VM üzerinde bakım başlatabilir, bakım Self Servis penceresi başlangıcı ||
| PreMaintenanceWindowEndTime           | VM üzerinde bakım başlatabilir, bakım Self Servis penceresi sonu ||
| MaintenanceWindowStartTime            | Azure VM'nizi bakım başlatır zamanlanmış bakım penceresi başlangıcı ||
| MaintenanceWindowEndTime              | Azure VM'nizi bakım başlatır zamanlanmış bakım penceresi sonu ||
| LastOperationResultCode               | Son VM bakım başlatma girişimi sonucu ||




## <a name="start-maintenance-on-your-vm-using-cli"></a>CLI kullanarak, VM bakımını Başlat

Aşağıdaki arama, bir VM üzerinde bakım başlatır `IsCustomerInitiatedMaintenanceAllowed` ayarlanmış true.

```azure-cli
az vm perform-maintenance rgName vmName 
```

[!INCLUDE [virtual-machines-common-maintenance-notifications](../../../includes/virtual-machines-common-maintenance-notifications.md)]

## <a name="classic-deployments"></a>Klasik dağıtımlar

Olan eski VM'ler yaşamaya devam ediyorsanız Klasik dağıtım modeli kullanılarak dağıtılan CLI 1.0 sorguya VM'ler için ve kullanabilirsiniz bakım başlatın.

Yazarak Klasik VM ile çalışmak için doğru moda olduğundan emin olun:

```
azure config mode asm
```

Adlı bir VM bakım durumunu almak için *myVM*, türü:

```
azure vm show myVM 
``` 

Bakım adlı, Klasik VM başlatmak için *myVM* içinde *myService* hizmet ve *myDeployment* dağıtım, türü:

```
azure compute virtual-machine initiate-maintenance --service-name myService --name myDeployment --virtual-machine-name myVM
```


## <a name="faq"></a>SSS


**S: neden my sanal makineleri şimdi yeniden başlatmak gerekiyor mu?**

**Y:** güncelleştirmeleri ve yükseltmeleri Azure platformu çoğunluğu değil etkisi sırasında sanal makinenin kullanılabilirlik burada biz olamaz kaçının Azure üzerinde barındırılan sanal makineleri yeniden başlatıldığı durumlar vardır. Biz sanal makine yeniden başlatma sonucunda sunucularımızda yeniden başlatmak bize gerektiren bazı değişiklikler toplanan.

**S: ı önerilerinizi yüksek kullanılabilirlik için bir kullanılabilirlik kümesi kullanarak izlerseniz, güvenli miyim?**

**Y:** kullanılabilirlik dağıtılan sanal makineleri ayarlama veya sanal makine ölçek kümeleri güncelleştirme etki alanları (UD) kavram vardır. Bakımı gerçekleştirirken Azure UD kısıtlaması geliştirir ve sanal makinelerden farklı UD (içinde aynı kullanılabilirlik kümesinde) yeniden değil.  Azure, sanal makinelerin sonraki grubuna geçmeden önce en az 30 dakika bekler. 

Yüksek kullanılabilirlik hakkında daha fazla bilgi için bkz: [bölgeler ve Azure sanal makineler için kullanılabilirlik](regions-and-availability.MD).

**S: nasıl planlı bakım hakkında bildirim?**

**Y:** bir veya daha fazla Azure bölgeleri için bir zamanlama ayarlayarak planlı bakım wave başlatır. Hemen sonra abonelik sahipleri (bir e-posta abonelik başına) için bir e-posta bildirimi gönderilir. Ek Kanallar ve bu bildirim için alıcıları etkinlik günlüğü uyarıları kullanarak yapılandırılabilir. Burada planlı bakım zaten zamanlanmış bir bölge için bir sanal makine dağıtma durumda bildirim almaz ancak VM bakım durumunu denetlemek yerine gerekir.

**S: portalında, Powershell veya CLI sorunun ne olduğunu, planlı bakım herhangi bir gösterge görmüyorum?**

**Y:** planlı bakım için ilgili bilgiler kullanılabilir olduğu tarafından etkilenmiş olacak VM'ler için planlı bakım wave sırasında. Diğer bir deyişle, verileri değil görürseniz, bakım wave zaten tamamlandı (veya başlatılmamış olduğunu) veya sanal makinenize güncelleştirilmiş bir sunucu zaten barındırılan olabilir.

**S: sanal Makinem tam olarak etkilenecek zaman bilmenin bir yolu var mı?**

**Y:** zamanlama ayarlarken, birkaç gün zaman penceresi tanımlarız. Ancak, tam sıralama ve sunucuları (VM'ler) bu pencereyi içinde bilinmiyor. Vm'leri için tam zaman bilmek ister misiniz müşteriler kullanabilir [zamanlanmış olayları](scheduled-events.md) ve sanal makinede bulunan sorgu gelen ve VM yeniden başlatmadan önce 15 dakikalık bildirim alırsınız.

**S: ne kadar süreyle sanal Makinem yeniden başlatılmasını sürer?**

**Y:** VM boyutuna bağlı olarak, yeniden başlatma için Self Servis bakım penceresi sırasında birkaç dakika sürebilir. Yeniden başlatma sırasında Azure başlatan zamanlanmış bakım penceresinde yeniden başlatma genellikle yaklaşık 25 dakika sürer. Bulut Hizmetleri (Web/çalışan rolü), sanal makine ölçek ayarlar ya da kullanılabilirlik kümeleri kullanmanız durumunda, 30 dakika arasında her grup, sanal makineleri (UD) zamanlanmış bakım penceresi sırasında verilen unutmayın.

**S: deneyimi bulut Hizmetleri (Web/çalışan rolü), Service Fabric ve sanal makine ölçek kümeleri söz konusu olduğunda nedir?**

**Y:** bu platformlar tarafından planlı bakım etkilenen olsa da, müşteriler bu platformu kullanarak güvenli bir tek yükseltme etki alanı (UD içinde) Bu yalnızca VM'ler verilen herhangi bir zamanda etkilenecek olarak kabul edilir. Self Servis bakım şu anda bulut Hizmetleri (Web/çalışan rolü), Service Fabric ve sanal makine ölçek kümeleri için kullanılabilir değil.

**S: donanım yetki alma hakkında bir e-posta aldığınız, planlı bakım ile aynıdır?**

**Y:** donanım yetkisini planlanan bakım olayı olsa da, henüz edildi Bu yeni deneyimi kullanım örneğine sahip olduğumuz.  

**S: herhangi bir bakım bilgi my Vm'lerinde görmüyorum. Nelerin yanlış gittiğini?**

**Y:** neden değil gördüğünüz herhangi bir bakım bilgi Vm'leriniz birkaç nedeni vardır:
1.  Microsoft iç olarak işaretlenmiş bir aboneliği kullanıyorsunuz.
2.  Vm'leriniz için bakım zamanlanmış değil. Bakım wave, böylece Vm'leriniz artık tarafından etkilenen değiştiren veya iptal sona erdiğini olabilir.
3.  Yok **Bakım** sütun VM liste görünümüne eklenir. Bu sütun için varsayılan görünüm ekledik olsa da, varsayılan olmayan sütunları görmek için yapılandırılmış müşteriler el ile eklemelisiniz **Bakım** kendi VM liste görünümü için sütun.

**S: VM'im bakım için ikinci kez zamanlandı. Neden?**

**Y:** bakım-dağıtın zaten tamamladıktan sonra bakım için zamanlanmış VM göreceğiniz birkaç kullanım örnekleri vardır:
1.  Biz bakım wave iptal ve farklı bir yükü ile yeniden başlatılır. Hatalı yükü algıladık ve kimliğinizi yalnızca bir ek yükü dağıtma gerekiyor olabilir.
2.  VM edildi *gösteriyor hizmet* bir donanım hatası nedeniyle başka bir düğüme
3.  Durdurmak için seçtiğiniz (deallocate) ve VM yeniden başlatma
4.  Sahip olduğunuz **otomatik kapatma** VM için açık


**S: Bakım my kullanılabilirlik kümesinin çok uzun sürüyor ve artık bazı my kullanılabilirlik "atlandı" durumu örnekleri ayarlanmış görebilir. Neden?** 

**Y:** bir kullanılabilirlik kümesinde kısa art arda birden çok örneği güncelleştirmeye tıklarsanız, Azure bu istekleri sıraya alacağı ve aynı anda yalnızca tek bir güncelleştirme etki alanında (UD) sanal makineleri güncelleştirmek başlatır. Ancak, olabileceğinden güncelleştirme etki alanları arasında bir duraklama, güncelleştirmeyi daha uzun sürer görünebilir. Güncelleştirme sıra 60 dakikadan uzun sürerse, bazı örnekleri gösterilir **atlandı** başarıyla güncelleştirildi olsa bile belirtin. Yanlış bu durumu önlemek için yalnızca bir kullanılabilirlik örneğinde tıklayarak, kullanılabilirlik kümelerini güncelleştirme ayarlayın ve farklı bir güncelleştirme etki alanındaki sonraki VM'de tıklatmadan önce tamamlamak için bu VM'de güncelleştirilmesini bekleyin.


## <a name="next-steps"></a>Sonraki Adımlar

VM kullanarak içinde bakım olaylarından ne kaydolabilirsiniz öğrenin [zamanlanmış olayları](scheduled-events.md).
