---
title: 'Azure Cosmos DB: .NET değişiklik akış işlemci API, SDK & kaynakları | Microsoft Docs'
description: Tüm değişiklik akış işlemci API ve yayın tarih, sona erme tarihlerini ve .NET değişiklik akış işlemci SDK'sı her sürümü arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
services: cosmos-db
documentationcenter: .net
author: ealsur
manager: kfile
ms.assetid: f2dd9438-8879-4f74-bb6c-e1efc2cd0157
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/19/2018
ms.author: maquaran
ms.openlocfilehash: 72eb329c03893f801e112ad33bca0c57c5ee46a0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>.NET değişiklik akış işlemci SDK: İndirme ve sürüm notları
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET değişiklik besleme](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

|   |   |
|---|---|
|**SDK yükleme**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**API belgeleri**|[Akış işlemci kitaplığı API başvuru belgeleri değiştirme](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Kullanmaya başlama**|[Değişiklik akış işlemci .NET SDK ile çalışmaya başlama](change-feed.md)|
|**Geçerli desteklenen çerçevelerden**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Sürüm notları

### <a name="stable-builds"></a>Kararlı yapılar

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2
* Bekleyen iş tahmininde giderir.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1
* İstikrara yönelik iyileştirmeler.
* El ile denetim noktası oluşturma desteği.
* Uyumlu [SQL .NET SDK'sı](sql-api-sdk-dotnet.md) sürümleri 1.21 ve üstü.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* .NET standart 2.0 desteği ekler. Paket artık destekliyor `netstandard2.0` ve `net451` framework adlar.
* Uyumlu [SQL .NET SDK'sı](sql-api-sdk-dotnet.md) sürümleri 1.17.0 ve üstü.
* Uyumlu [SQL .NET Core SDK](sql-api-sdk-dotnet-core.md) sürümleri 1.5.1 ve üstü.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Bir sorunu kalan çalışma tahmin hesaplanması değişiklik akışı boş veya hiçbir iş bekleyen giderir.
* Uyumlu [SQL .NET SDK'sı](sql-api-sdk-dotnet.md) sürümleri 1.13.2 ve üstü.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Kalan iş akışı değişiklik işlenmek üzere bir tahmin elde etmek için bir yöntemi eklendi.
* Uyumlu [SQL .NET SDK'sı](sql-api-sdk-dotnet.md) sürümleri 1.13.2 ve üstü.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK'SI
* Uyumlu [SQL .NET SDK'sı](sql-api-sdk-dotnet.md) sürümleri 1.14.1 ve aşağıdaki.

### <a name="pre-release-builds"></a>Yayın öncesi derlemeleri

### <a name="a-name201-prerelease201-prerelease"></a><a name="2.0.1-prerelease"/>2.0.1-prerelease
* Yeni v2 API:
  * İşlemci, Esnek yapımı için oluşturucu deseni: ChangeFeedProcessorBuilder sınıfı.
    * Herhangi bir bileşimini parametreleri alabilir.
    * DocumentClient örneği için izleme ve/veya kira toplanamıyor (v1) alabilir.
  * IChangeFeedObserver.ProcessChangesAsync şimdi CancellationToken alır.
  * IRemainingWorkEstimator - kalan iş tahmin işlemcisine ayrı olarak kullanılabilir.
  * Yeni genişletilebilirlik noktaları:
    * IParitionLoadBalancingStrategy - işlemci örnekleri arasında bölümlerinin özel Yük Dengeleme için.
    * ILease, ILeaseManager - özel kiralama Yönetimi'ni tıklatın.
    * IPartitionProcessor - bir bölüme özel işleme değişiklikleri için.
* Günlüğü - kullanan [LibLog](https://github.com/damianh/LibLog) kitaplığı.
* % 100 v1 API ile geriye dönük uyumlu.
* Uyumlu [SQL .NET SDK'sı](sql-api-sdk-dotnet.md) sürümleri 1.21.1 ve üstü.

## <a name="release--retirement-dates"></a>Yayın & sona erme tarihleri
Microsoft sağlayacaktır bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş kesintisiz için bir SDK devre dışı bırakmadan önce.

Yeni özellikler ve işlevsellik ve en iyi duruma getirme geçerli SDK'sı yalnızca eklenir, bu nedenle, her zaman en son SDK sürüme erken mümkün olduğunca yükseltmeniz önerilir. 

Cosmos devre dışı bırakılan bir SDK'sını kullanarak DB'de herhangi bir istek hizmeti tarafından reddedilir.

<br/>

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [1.3.2](#1.3.2) |18 Nisan 2018 |--- |
| [1.3.1](#1.3.1) |13 Mart 2018 |--- |
| [1.2.0](#1.2.0) |31 Ekim 2017 |--- |
| [1.1.1](#1.1.1) |29 Ağustos 2017 |--- |
| [1.1.0](#1.1.0) |13 Ağustos 2017 |--- |
| [1.0.0](#1.0.0) |07 Temmuz 2017 |--- |


## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası. 

