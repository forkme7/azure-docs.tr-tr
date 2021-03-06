---
title: "Azure olay kılavuz kaynak grubu olay şeması"
description: "Kaynak grubu Azure olay kılavuz olaylarla için sağlanan özellikler açıklar"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: 109f5af5cc1647cebee805c3141f4bc83c73bcfc
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="azure-event-grid-event-schema-for-resource-groups"></a>Kaynak grupları için Azure olay kılavuz olay şeması

Bu makale, kaynak grubu olaylar için şema ve özellikleri sağlar. Olay şemaları giriş için bkz: [Azure olay kılavuz olay şema](event-schema.md).

Azure Abonelikleriniz ve kaynak grupları aynı olay türlerini yayma. Olay türleri kaynakları değişiklikleri ilgilidir. Birincil olayları kaynak grubunda kaynaklar için kaynak gruplarını yayma ve Azure abonelikleri olayları kaynaklar için abonelik üzerinden yayma farktır. 

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Bir VM oluşturulduğunda veya bir depolama hesabı silinir gibi kaynak grupları Yönetimi olayları Azure Kaynak Yöneticisi'nden yayma.

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Resources.ResourceWriteSuccess | Yükseltilmiş ne zaman bir kaynak oluşturma veya güncelleştirme işlemi başarılı olur. |
| Microsoft.Resources.ResourceWriteFailure | Kaynak oluşturma veya güncelleştirme işlemi başarısız olduğunda oluşturulur. |
| Microsoft.Resources.ResourceWriteCancel | Yükseltilmiş ne zaman bir kaynak oluşturma veya güncelleştirme işlemi iptal edildi. |
| Microsoft.Resources.ResourceDeleteSuccess | Bir kaynak silme işlemi başarılı olduğunda oluşturulur. |
| Microsoft.Resources.ResourceDeleteFailure | Bir kaynak silme işlemi başarısız olduğunda oluşturulur. |
| Microsoft.Resources.ResourceDeleteCancel | Bir kaynak silme işlemi iptal edildiğinde oluşturulur. Bu olay, bir şablon dağıtımı iptal ettiğinizde gerçekleşir. |

## <a name="example-event"></a>Örnek olayı

Aşağıdaki örnek, olay oluşturulan bir kaynak şeması gösterir: 

```json
[
  {
    "topic":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"{request-operation}",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
    },
    "dataVersion": "",
    "metadataVersion": "1"
  }
]
```

Silinen kaynak olay için şemayı benzer:

```json
[{
  "topic":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}",
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicApp0ecd6c02-2296-4d7c-9865-01532dc99c93",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2017-11-07T21:24:19.6959483Z",
  "id": "7995ecce-39d4-4851-b9d7-a7ef87a06bf5",
  "data": {
    "authorization": "{azure_resource_manager_authorizations}",
    "claims": "{azure_resource_manager_claims}",
    "correlationId": "7995ecce-39d4-4851-b9d7-a7ef87a06bf5",
    "httpRequest": "{request-operation}",
    "resourceProvider": "Microsoft.EventGrid",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "operationName": "Microsoft.EventGrid/eventSubscriptions/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Konu | dize | Olay kaynağı tam kaynak yolu. Bu alan yazılabilir değil. Bu değer olay kılavuz sağlar. |
| Konu | dize | Olay konu yayımcı tarafından tanımlanan yolu. |
| eventType | dize | Bu olay kaynağı için kayıtlı olay türünden biri. |
| EventTime | dize | Olayı oluşturan zaman sağlayıcının UTC zamanı temel alınarak. |
| id | dize | Olay için benzersiz tanımlayıcı. |
| veriler | nesne | Kaynak grubu olay verileri. |
| dataVersion | dize | Veri nesnesi şema sürümü. Yayımcı şema sürümü tanımlar. |
| metadataVersion | dize | Olay meta veri şema sürümü. Olay kılavuz, şemanın en üst düzey özellikleri tanımlar. Bu değer olay kılavuz sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Yetkilendirme | dize | İstenen Yetkilendirme işlemi için. |
| Talepleri | dize | Talep özellikleri. |
| correlationId | dize | Sorun giderme için bir işlem kimliği. |
| httpRequest | dize | İşlem ayrıntıları. |
| resourceProvider | dize | İşlemi gerçekleştiren kaynak sağlayıcısı. |
| resourceUri | dize | İşlemi kaynak URI'si. |
| operationName | dize | Gerçekleştirilen işlem. |
| durum | dize | İşlem durumu. |
| subscriptionId | dize | Kaynak abonelik kimliği. |
| tenantId | dize | Kaynak Kiracı kimliği. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure olay kılavuz giriş için bkz: [olay kılavuz nedir?](overview.md)
* Bir Azure olay kılavuz abonelik oluşturma hakkında daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).
