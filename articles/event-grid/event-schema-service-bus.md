---
title: "Azure olay kılavuz Service Bus şeması"
description: "Azure olay kılavuz olan Service Bus olaylar için sağlanan özellikler açıklar"
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: article
ms.date: 02/21/2018
ms.author: babanisa
ms.openlocfilehash: 72780bff3807534efb456a9a7998f7d4de3c6f12
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="azure-event-grid-event-schema-for-service-bus"></a>Hizmet veri yolu için Azure olay kılavuz olay şeması

Bu makale, Service Bus olaylar için şema ve özellikleri sağlar. Olay şemaları giriş için bkz: [Azure olay kılavuz olay şema](event-schema.md).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Hizmet veri yolu aşağıdaki olay türlerini gösterir:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners | Bir kuyruk veya abonelik ve dinleme hiçbir alıcılar etkin iletiler olduğunda oluşturulur. |
| Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener | Sahipsiz sıra ve hiçbir etkin dinleyicileri etkin iletiler olduğunda oluşturulur. |

## <a name="example-event"></a>Örnek olayı

Aşağıdaki örnek, hiçbir dinleyicileri olay etkin iletilerle şeması gösterir:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Sahipsiz Sıra olay için şemayı benzer:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Konu | string | Olay kaynağı tam kaynak yolu. Bu alan yazılabilir değil. Bu değer olay kılavuz sağlar. |
| Konu | string | Olay konu yayımcı tarafından tanımlanan yolu. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türünden biri. |
| EventTime | string | Olayı oluşturan zaman sağlayıcının UTC zamanı temel alınarak. |
| id | string | Olay için benzersiz tanımlayıcı. |
| veriler | nesne | BLOB Depolama olay verileri. |
| dataVersion | string | Veri nesnesi şema sürümü. Yayımcı şema sürümü tanımlar. |
| metadataVersion | string | Olay meta veri şema sürümü. Olay kılavuz, şemanın en üst düzey özellikleri tanımlar. Bu değer olay kılavuz sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| nameSpaceName | string | Hizmet veri yolu ad alanı kaynak bulunmaktadır. |
| requestUri | string | Belirli bir sıraya veya olay yayma Abonelik URI. |
| entityType | string | Olaylar (kuyruk veya abonelik) yayma Service Bus varlık türü. |
| queueName | string | Sıranın bir sıraya abone olursa etkin iletilerle. Konular kullanıyorsanız null değer / abonelikleri. |
| TopicName | string | Konu etkin iletileri Service Bus aboneliğe ait. Bir kuyruk kullanıyorsanız null değer. |
| varlığıyla subscriptionName | string | Hizmet veri yolu abonelik etkin iletilerle. Bir kuyruk kullanıyorsanız null değer. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure olay kılavuz giriş için bkz: [olay kılavuz nedir?](overview.md)
* Bir Azure olay kılavuz abonelik oluşturma hakkında daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).
* Azure olay kılavuz Service Bus ile kullanma hakkında daha fazla bilgi için bkz [olay kılavuz tümleştirmesine genel bakış için Service Bus](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md).
* Deneyin [işlevleri veya Logic Apps ile Service Bus olayları almasını](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json).
