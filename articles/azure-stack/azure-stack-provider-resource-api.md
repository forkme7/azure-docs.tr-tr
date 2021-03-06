---
title: "Sağlayıcı kaynak kullanımı API | Microsoft Docs"
description: "API, kaynak kullanımı için Azure yığın kullanım bilgilerini alır başvurusu"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: 763b0af9c258a70392e8c7ebbb4c107e94fce5b2
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="provider-resource-usage-api"></a>Sağlayıcı kaynak kullanım API’si
Terim *sağlayıcı* Hizmet Yöneticisi ve tüm yetkilendirilmiş sağlayıcılarını geçerlidir. Azure yığın işleçler ve temsilci sağlayıcıları sağlayıcısı kullanım API'si doğrudan kiracıları kullanımını görüntülemek için kullanabilirsiniz. Örneğin, aşağıdaki çizimde gösterildiği gibi P0 P1'ın ilgili kullanım bilgileri almak için API sağlayıcısı çağırabilir ve P2'ın doğrudan kullanımını ve P1 kullanım bilgileri P3 ve P4 çağırabilir.

![Sağlayıcı hiyerarşisinin kavramsal model](media/azure-stack-provider-resource-api/image1.png)

## <a name="api-call-reference"></a>API çağrısı başvurusu
### <a name="request"></a>İstek
İstek tüketim ayrıntıları istenen zaman çerçevesi ve istenen abonelikler için alır. İstek gövdesinde yok.

Bu kullanım API'si sağlayıcısı API, olduğundan, çağıran bir sağlayıcının abonelik sahibi, katkıda bulunan veya okuyucu rol atanması gerekir.

| **Yöntemi** | **İstek URI'si** |
| --- | --- |
| AL |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&subscriberId={sub1.1}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Bağımsız Değişkenler
| **Bağımsız değişken** | **Açıklama** |
| --- | --- |
| *armendpoint* |Azure yığın ortamınızın Azure Kaynak Yöneticisi uç noktası. Azure Resource Manager uç noktanın adı şu biçimdedir Azure yığın kuraldır `https://adminmanagement.{domain-name}`. Etki alanı adı ise, örneğin, Geliştirme Seti için *local.azurestack.external*, kaynak yöneticisi uç noktası ise `https://adminmanagement.local.azurestack.external`. |
| *subId* |Çağrıyı yapan kullanıcının abonelik kimliği. |
| *reportedStartTime* |Sorgunun başlarsınız. Değeri *DateTime* Eşgüdümlü Evrensel Saat (UTC) ve saat, örneğin, 13:00 başında olması gerekir. Günlük toplama için bu değer UTC gece yarısına ayarlanmış. Biçim *kaçışlı* ISO 8601. Örneğin, *2015 06 %16T18 %3a53 %3a11 %2b00 3a00Z*, iki nokta üst üste için kaçışlı burada *% 3a* ve artı için kaçışlı *% 2b* URI kolay olmasını sağlayın. |
| *reportedEndTime* |Sorgunun bitiş saati. Geçerli kısıtlamaları *reportedStartTime* bu bağımsız değişken için de geçerlidir. Değeri *reportedEndTime* ya da geçerli tarihi gelecekte olamaz. İse, sonuç "tam işlenmiyor." ayarlanır |
| *aggregationGranularity* |İki ayrı olası değerlere sahip isteğe bağlı bir parametre: günlük ve saatlik. Değerleri önermek gibi veri yer günlük ayrıntı düzeyi döndürür ve diğeri saatlik bir çözüm. Günlük varsayılan seçenektir. |
| *subscriberId* |Abonelik kimliği Filtrelenmiş veri almak için sağlayıcının doğrudan Kiracı abonelik kimliği gereklidir. Hiç abonelik kimliği parametresi belirtilirse, çağrı Sağlayıcısı'nın doğrudan kiracılar için kullanım verilerini döndürür. |
| *api-version* |Bu isteği yapmak için kullanılan protokol sürümü. Bu değer ayarlanırsa *2015-06-01-Önizleme*. |
| *continuationToken* |Belirteci son çağrı kullanım API'si sağlayıcısına alınır. Bu belirteç yanıt 1.000 satırları büyüktür ve ilerleme durumu için bir yer işareti gibi davranır gereklidir. Belirteç mevcut değilse, veriler gün baştan alınır veya saat üzerinde ayrıntı göre geçirilen. |

### <a name="response"></a>Yanıt
/Subscriptions/sub1/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime Al 2015-06-%01T00 %3a00 %3a00 %2b00 3a00 = & aggregationGranularity = günlük & subscriberId = sub1.1 & API sürümü 1.0 =

```json
{
"value": [
{

"id":
"/subscriptions/sub1.1/providers/Microsoft.Commerce/UsageAggregate/sub1.1-

meterID1",
"name": "sub1.1-meterID1",
"type": "Microsoft.Commerce/UsageAggregate",

"properties": {
"subscriptionId":"sub1.1",
"usageStartTime": "2015-03-03T00:00:00+00:00",
"usageEndTime": "2015-03-04T00:00:00+00:00",
"instanceData":"{\"Microsoft.Resources\":{\"resourceUri\":\"resourceUri1\",\"location\":\"Alaska\",\"tags\":null,\"additionalInfo\":null}}",
"quantity":2.4000000000,
"meterId":"meterID1"

}
},

…
```

### <a name="response-details"></a>Yanıt Ayrıntıları
| **Bağımsız değişken** | **Açıklama** |
| --- | --- |
| *id* |Kullanım toplama benzersiz kimliği. |
| *Adı* |Kullanım toplama adı. |
| *Türü* |Kaynak tanımı. |
| *subscriptionId* |Azure yığın kullanıcı abonelik tanımlayıcısı. |
| *usageStartTime* |UTC bu kullanım toplama ait olduğu kullanım demet süresini başlatın.|
| *usageEndTime* |Bu kullanım toplama ait olduğu kullanım demet UTC bitiş saati. |
| *instanceData* |Örnek ayrıntıları (yeni biçimde) anahtar-değer çiftleri:<br> *resourceUri*: tam olarak kaynak grupları ve örnek adını içeren kaynak kimliği. <br> *Konum*: bölge bu hizmet çalıştırıldı. <br> *Etiketler*: kullanıcı tarafından belirtilen kaynak etiketleri. <br> *additionalınfo alanına*: daha fazla bilgi için örneğin, kullanılan kaynak hakkında işletim sistemi sürümü veya görüntü türü. |
| *Miktar* |Bu zaman dilimi içinde oluştu kaynak tüketimi miktarı. |
| *meterId* |Benzersiz kimliği için kullanılan kaynak (olarak da bilinir *ResourceId*). |


## <a name="retrieve-usage-information"></a>Kullanım bilgilerini alma

Kullanım verilerini oluşturmak için çalışan ve etkin olarak sistem, örneğin kullanan kaynaklar, etkin bir sanal makineye veya bazı veri vb. içeren bir depolama hesabı olmalıdır. Dikey emin olmak için izleme olup Azure yığın Marketi'nde çalıştıran herhangi bir kaynağı var, bir sanal makine (VM) dağıtmanıza ve VM doğrulayın konusunda emin değilseniz çalıştığı. Kullanım verileri görüntülemek için aşağıdaki PowerShell cmdlet'lerini kullanın:

1. [PowerShell için Azure yığın yükleyin.](azure-stack-powershell-install.md)
2. [Azure yığın kullanıcının yapılandırmak](user/azure-stack-powershell-configure-user.md) veya [Azure yığın işlecin](azure-stack-powershell-configure-admin.md) PowerShell ortamı 
3. Kullanım verilerini almak için kullanın [Get-UsageAggregates](/powershell/module/azurerm.usageaggregates/get-usageaggregates) PowerShell cmdlet:
```powershell
Get-UsageAggregates -ReportedStartTime "<Start time for usage reporting>" -ReportedEndTime "<end time for usage reporting>" -AggregationGranularity <Hourly or Daily>
```

## <a name="next-steps"></a>Sonraki adımlar
[Kiracı kaynak kullanımını API Başvurusu](azure-stack-tenant-resource-usage-api.md)

[Kullanımı ile ilgili SSS](azure-stack-usage-related-faq.md)
