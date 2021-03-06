---
title: Azure ilke tanımı yapısı | Microsoft Docs
description: Kaynak ilke tanımı zaman İlkesi uygulandığında ve hangi eylemin yapılacağını açıklayan, kuruluşunuzda kaynaklar için kuralları oluşturmak için Azure ilke tarafından nasıl kullanıldığını açıklar.
services: azure-policy
keywords: ''
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/18/2018
ms.topic: article
ms.service: azure-policy
ms.custom: ''
ms.openlocfilehash: 8b89e1c8ccfcfd7b53ecdd9172590424d1c7ae4c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-policy-definition-structure"></a>Azure İlkesi tanım yapısı

Azure ilke tarafından kullanılan kaynak ilke tanımı zaman İlkesi uygulandığında ve hangi eylemin yapılacağını açıklayan, kuruluşunuzdaki kaynakların kuralları kurmanızı sağlar. Kuralları tanımlayarak, daha kolay kaynaklarınızı yönetmek ve maliyetleri denetleyebilirsiniz. Örneğin, sanal makineler yalnızca belirli türdeki izin verildiğini belirtebilirsiniz. Veya, tüm kaynakların belirli bir etikete sahip olması gerekir. İlkeler tüm alt kaynaklar tarafından devralınır. Bu nedenle, bir kaynak grubu için bir ilke uygulandığında, bu kaynak grubundaki tüm kaynaklar için geçerlidir.

Bir ilke tanımı oluşturmak için JSON kullanın. İlke tanımı için öğeleri içerir:

* mode
* parametreler
* Görünen ad
* açıklama
* İlke kuralı
  * mantıksal değerlendirme
  * Etkisi

Örneğin, aşağıdaki JSON kaynakları dağıtıldığı sınırlar bir ilke gösterir:

```json
{
  "properties": {
    "mode": "all",
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

Tüm Azure ilke şablonu örnekleri altındadır [Azure ilke şablonları](json-samples.md).

## <a name="mode"></a>Mod

**Modu** hangi kaynak türlerinin için bir ilke değerlendirilir belirler. Desteklenen modları şunlardır:
* `all`: kaynak grupları ve tüm kaynak türleri değerlendir
* `indexed`: yalnızca etiketlerini ve konumunu destekleyen kaynak türleri değerlendir

Ayarlamanızı öneririz **modu** için `all` çoğu durumda. Portal kullanımı oluşturulan tüm ilke tanımları `all` modu. PowerShell veya Azure CLI kullanıyorsanız, belirtmek zorunda **modu** parametresi el ile. İlke tanımı içermiyorsa bir **modu** varsayılan değer `indexed` için geriye dönük uyumluluk.

`indexed` ilkeleri oluşturma etiketler veya konumları zorunlu kılacak olduğunda kullanılmalıdır. Bu gerekli değildir ancak etiketleri ve konumları olarak uyumluluk sonuçlarını uyumlu olmayan göstermesini desteklemeyen kaynakları engeller. Bunun tek istisnası **kaynak grupları**. Konum veya bir kaynak grubu üzerinde etiketleri zorlamak için çalışıyorsunuz ilkeleri ayarlamalıdır **modu** için `all` ve özellikle hedef `Microsoft.Resources/subscriptions/resourceGroup` türü. Bir örnek için bkz: [kaynak grubu etiketleri zorunlu](scripts/enforce-tag-rg.md).

## <a name="parameters"></a>Parametreler

Parametreleri ilke tanımları sayısını azaltarak ilke yönetimini basitleştirmeye yardımcı olur. Formdaki – alanları gibi parametreleri düşünün `name`, `address`, `city`, `state`. Bu parametrelerin her zaman aynı kalır, ancak bunların değerleri değiştirmek tek tek formu dolduran üzerinde temel. Parametreler, ilkeleri oluştururken aynı şekilde çalışır. Bir ilke tanımı'nda parametreleri dahil ederek, farklı değerler kullanarak farklı senaryolar için bu ilkeyi yeniden kullanabilirsiniz.

Örneğin, kaynakları dağıtıldığı konumları sınırlamak bir kaynak özelliği için bir ilke tanımlayabilirsiniz. Bu durumda, ilke oluşturduğunuzda, aşağıdaki parametreleri bildirin:


```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "The list of allowed locations for resources.",
      "displayName": "Allowed locations",
      "strongType": "location"
    }
  }
}
```

Bir parametrenin türü string veya dizi olabilir. Meta veri özelliği, Azure portalı gibi araçlar için kullanıcı dostu bilgileri görüntülemek için kullanılır.

Meta veri özelliği içinde kullandığınız **strongType** Azure portalı içinde seçeneklerini çoklu seçim listesi temin etmek için.  İzin verilen değerler için **strongType** şu anda içerir:

* `"location"`
* `"resourceTypes"`
* `"storageSkus"`
* `"vmSKUs"`
* `"existingResourceGroups"`
* `"omsWorkspace"`

İlke kuralı aşağıdaki söz dizimini parametrelerle başvurusu:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a>Görünen ad ve açıklama

Kullanabileceğiniz **displayName** ve **açıklama** ilke tanımı tanımlamak ve kullanıldığında için bağlamı sağlar.

## <a name="policy-rule"></a>İlke kuralı

İlke kuralı oluşan **varsa** ve **sonra** engeller. İçinde **varsa** bloğu, ilke zaman zorlanır belirten bir veya daha fazla koşullarını tanımlayın. Mantıksal işleçler tam olarak bu senaryo için bir ilke tanımlamak için bu koşullar uygulayabilirsiniz.

İçinde **sonra** bloğu olur etkisi tanımladığınız zaman **varsa** koşullar yerine.

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists"
  }
}
```

### <a name="logical-operators"></a>Mantıksal işleçler

Desteklenen mantıksal işleçler şunlardır:

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

**Değil** sözdizimi koşul sonucunu tersine çevirir. **Tümü** sözdizimi (benzer şekilde mantıksal **ve** işlemi) tüm koşulların doğru olmasını gerektirir. **Herhangi** sözdizimi (benzer şekilde mantıksal **veya** işlemi) doğru olması için bir veya birden çok koşul gerektirir.

Mantıksal işleçler yerleştirebilirsiniz. Aşağıdaki örnekte gösterildiği bir **değil** içinde iç içe işlemi bir **tümü** işlemi.

```json
"if": {
  "allOf": [
    {
      "not": {
        "field": "tags",
        "containsKey": "application"
      }
    },
    {
      "field": "type",
      "equals": "Microsoft.Storage/storageAccounts"
    }
  ]
},
```

### <a name="conditions"></a>Koşullar

Bir koşulu değerlendirir olup bir **alan** belirli kriterlere uyan. Desteklenen koşullar şunlardır:

* `"equals": "value"`
* `"notEquals": "value"`
* `"like": "value"`
* `"notLike": "value"`
* `"match": "value"`
* `"notMatch": "value"`
* `"contains": "value"`
* `"notContains": "value"`
* `"in": ["value1","value2"]`
* `"notIn": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"notContainsKey": "keyName"`
* `"exists": "bool"`

Kullanırken **gibi** ve **notLike** koşullar, bir joker (*) değer sağlayabilir.

Kullanırken **eşleşen** ve **notMatch** koşullar sağlamak `#` bir basamak temsil etmek için `?` bir harf ve o gerçek karakteri temsil etmesi için başka bir karakter. Örnekler için bkz: [onaylanmış VM görüntüleri](scripts/allowed-custom-images.md).

### <a name="fields"></a>Alanlar
Koşullar alanlar kullanılarak oluşturulur. Bir alan kaynağının durumu tanımlamak için kullanılan kaynak istek yükünde özelliklerini temsil eder.  

Aşağıdaki alanları desteklenir:

* `name`
* `fullName`
  * Tüm üst (örneğin "myServer/Veritabanım") dahil olmak üzere kaynak tam adını döndürür
* `kind`
* `type`
* `location`
* `tags`
* `tags.tagName`
* `tags[tagName]`
  * Nokta içeren etiket adları bu köşeli ayraç sözdizimini destekler
* özellik diğer adlar - bir listesi için bkz: [diğer adlar](#aliases).

### <a name="alternative-accessors"></a>Alternatif erişimciler
**Alan** olan ilke kurallarında kullanılan birincil erişimcisi. Doğrudan değerlendiriliyor kaynak olup olmadığını denetler. Ancak, diğer bir erişimci İlkesi destekler **kaynak**.

```json
"source": "action",
"equals": "Microsoft.Compute/virtualMachines/write"
```

**Kaynak** yalnızca bir değeri destekleyen **eylem**. Eylem şu anda değerlendiriliyor isteğinin yetkilendirme eylem döndürür. Yetkilendirme Eylemler yetkilendirme bölümünde açığa [etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-activity-log-schema.md).

Ne zaman değerlendirme İlkesi ayarlar arka planda var olan kaynakların **eylem** için bir `/write` kaynağın türünü yetkilendirme eylem.

### <a name="effect"></a>Etki
İlke etkili aşağıdaki türlerini destekler:

* **Reddetme**: Denetim günlüğüne bir olay oluşturur ve isteği başarısız olur
* **Denetim**: Denetim günlüğüne bir uyarı olayı oluşturur ama isteği başarısız değil
* **Append**: alanları dizi tanımlanmış isteğe ekler
* **AuditIfNotExists**: kaynak yoksa, denetim sağlar
* **DeployIfNotExists**: zaten yoksa, bir kaynak dağıtır. Şu anda bu etkiyi yalnızca yerleşik ilkeler aracılığıyla desteklenir.

İçin **sona**, aşağıdaki ayrıntıları sağlamanız gerekir:

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of the field"
  }
]
```

Değer bir dize veya bir JSON biçimi nesnesi olabilir.

İle **AuditIfNotExists** ve **DeployIfNotExists** ilişkili bir kaynak varlığını değerlendirin ve bu kaynak mevcut değil, bir kural ve karşılık gelen bir efekt uygulayın. Örneğin, bir Ağ İzleyicisi için tüm sanal ağları dağıtılır gerektirebilir.
Bir sanal makine uzantısı değil dağıtıldığında denetim bir örnek için bkz: [uzantısı yoksa, Denetim](scripts/audit-ext-not-exist.md).


## <a name="aliases"></a>Diğer adlar

Bir kaynak türü için belirli özelliklere erişmek için özellik diğer adlar kullanın. Diğer adlar hangi değerleri veya koşullara bir özelliği bir kaynak için izin verilen kısıtlamak etkinleştirin. Her bir diğer ad belirli kaynak türlerine yönelik farklı API sürümleri yollarında eşler. İlke değerlendirmesi sırasında ilke altyapısı bu API sürümü için özellik yolu alır.

**Microsoft.Cache/Redis**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Cache/Redis/enableNonSslPort | Ayarlanmış olup olmadığını ssl olmayan Redis bağlantı noktası (6379) etkindir. |
| Microsoft.Cache/Redis/shardCount | Premium küme önbelleği üzerinde oluşturulacak parça sayısını ayarlayın.  |
| Microsoft.Cache/Redis/sku.capacity | Dağıtmak için Redis önbelleği boyutunu ayarlayın.  |
| Microsoft.Cache/Redis/sku.family | Kullanmak için SKU ailesi ayarlayın. |
| Microsoft.Cache/Redis/sku.name | Dağıtmak için Redis önbelleği türünü ayarlayın. |

**Microsoft.Cdn/profiles**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.CDN/profiles/sku.name | Fiyatlandırma katmanı adını ayarlayın. |

**Microsoft.Compute/disks**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü teklif ayarlayın. |
| Microsoft.Compute/imagePublisher | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü yayımcısına ayarlayın. |
| Microsoft.Compute/imageSku | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü SKU'su ayarlayın. |
| Microsoft.Compute/imageVersion | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü sürümünü ayarlayın. |


**Microsoft.Compute/virtualMachines**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Compute/imageId | Sanal makine oluşturmak için kullanılan görüntü tanıtıcısı ayarlayın. |
| Microsoft.Compute/imageOffer | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü teklif ayarlayın. |
| Microsoft.Compute/imagePublisher | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü yayımcısına ayarlayın. |
| Microsoft.Compute/imageSku | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü SKU'su ayarlayın. |
| Microsoft.Compute/imageVersion | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü sürümünü ayarlayın. |
| Microsoft.Compute/licenseType | Görüntü veya disk içi lisanslı ayarlanır. Bu değer yalnızca Windows Server işletim sistemi içeren görüntüler için kullanılır.  |
| Microsoft.Compute/virtualMachines/imageOffer | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü teklif ayarlayın. |
| Microsoft.Compute/virtualMachines/imagePublisher | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü yayımcısına ayarlayın. |
| Microsoft.Compute/virtualMachines/imageSku | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü SKU'su ayarlayın. |
| Microsoft.Compute/virtualMachines/imageVersion | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü sürümünü ayarlayın. |
| Microsoft.Compute/virtualMachines/osDisk.Uri | Vhd URI ayarlayın. |
| Microsoft.Compute/virtualMachines/sku.name | Sanal makine boyutunu ayarlayın. |
| Microsoft.Compute/virtualMachines/availabilitySet.id | Kullanılabilirlik kimliği sanal makine için ayarlar. |

**Microsoft.Compute/virtualMachines/extensions**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Compute/virtualMachines/extensions/publisher | Uzantının yayımcının adını ayarlayın. |
| Microsoft.Compute/virtualMachines/extensions/type | Uzantı türü olarak ayarlayın. |
| Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion | Uzantı sürümü ayarlayın. |

**Microsoft.Compute/virtualMachineScaleSets**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Compute/imageId | Sanal makine oluşturmak için kullanılan görüntü tanıtıcısı ayarlayın. |
| Microsoft.Compute/imageOffer | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü teklif ayarlayın. |
| Microsoft.Compute/imagePublisher | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü yayımcısına ayarlayın. |
| Microsoft.Compute/imageSku | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü SKU'su ayarlayın. |
| Microsoft.Compute/imageVersion | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü sürümünü ayarlayın. |
| Microsoft.Compute/licenseType | Görüntü veya disk içi lisanslı ayarlanır. Bu değer yalnızca Windows Server işletim sistemi içeren görüntüler için kullanılır. |
| Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix | Tüm sanal makineler için bilgisayar adı öneki ölçek kümesinde ayarlayın. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl | Kullanıcı görüntüsü blob URI'si ayarlayın. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers | Ölçek kümesi için işletim sistemi disklerini depolamak için kullanılan kapsayıcı URL'lerini ayarlayın. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.name | Ölçek kümesindeki sanal makinelerin boyutunu ayarlayın. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.tier | Sanal makine ölçek kümesindeki kümesi. |

**Microsoft.Network/applicationGateways**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Network/applicationGateways/sku.name | Ağ geçidi boyutunu ayarlayın. |

**Microsoft.Network/virtualNetworkGateways**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Network/virtualNetworkGateways/gatewayType | Bu sanal ağ geçidi türünü ayarlayın. |
| Microsoft.Network/virtualNetworkGateways/sku.name | Ağ geçidi SKU adına ayarlayın. |

**Microsoft.Sql/servers**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Sql/servers/version | Sunucu sürümü ayarlayın. |

**Microsoft.Sql/databases**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Sql/servers/databases/edition | Veritabanı sürümünü ayarlayın. |
| Microsoft.Sql/servers/databases/elasticPoolName | Esnek havuz veritabanı adını ayarlayın. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveId | Yapılandırılan hizmet düzeyi hedefi kimliği veritabanının ayarlayın. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveName | Yapılandırılan hizmet düzeyi hedefi veritabanının adını ayarlayın.  |

**Microsoft.Sql/elasticpools**

| Diğer ad | Açıklama |
| ----- | ----------- |
| sunucuları/elasticpools | Microsoft.Sql/servers/elasticPools/dtu | Veritabanı esnek havuz için toplam paylaşılan DTU ayarlayın. |
| sunucuları/elasticpools | Microsoft.Sql/servers/elasticPools/edition | Esnek havuz sürümünü ayarlayın. |

**Microsoft.Storage/storageAccounts**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Storage/storageAccounts/accessTier | Faturalama için kullanılan erişim katmanı ayarlayın. |
| Microsoft.Storage/storageAccounts/accountType | SKU adına ayarlayın. |
| Microsoft.Storage/storageAccounts/enableBlobEncryption | Blob depolama hizmetinde depolanan gibi hizmet verileri şifreler gerekip gerekmediğini belirleyin. |
| Microsoft.Storage/storageAccounts/enableFileEncryption | Dosya depolama hizmetinde depolanan gibi hizmet verileri şifreler gerekip gerekmediğini belirleyin. |
| Microsoft.Storage/storageAccounts/sku.name | SKU adına ayarlayın. |
| Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly | Yalnızca depolama hizmeti https trafiğine izin verecek şekilde ayarlayın. |
| Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*].id | Sanal Ağ Hizmeti uç noktası etkin olup olmadığını denetleyin. |

## <a name="initiatives"></a>Girişimler

Birkaç Grup girişimleri etkinleştir grup için tek bir öğe olarak çalışmak için atamalarını ve yönetimini basitleştirmek için İlke tanımları ilgili. Örneğin, tek bir girişim içindeki tüm ilgili etiketleme ilke tanımları gruplandırabilirsiniz. Her ilke tek tek atamak yerine Initiative uygulayın.

Aşağıdaki örnekte nasıl iki etiket işlemek için bir girişimi oluşturulacağı gösterilmektedir: `costCenter` ve `productName`. Varsayılan etiket değeri uygulamak için iki yerleşik ilkeleri kullanır.


```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure ilke şablonu örnekleri, inceleyin [Azure ilke şablonları](json-samples.md).
