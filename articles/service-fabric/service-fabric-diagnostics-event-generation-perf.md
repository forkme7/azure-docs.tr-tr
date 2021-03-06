---
title: Azure Service Fabric performans izleme | Microsoft Docs
description: İzleme ve tanılama Azure Service Fabric kümeleri için performans sayaçları hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/16/2018
ms.author: dekapur; srrengar
ms.openlocfilehash: 54837b2a43f93f43d089a2fdf35d8b74ecacfc3e
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="performance-metrics"></a>Performans ölçümleri

Ölçümleri kümenizi ve bunun yanı sıra içinde çalışan uygulamaların performansını anlamak için toplanan. Service Fabric kümeleri için aşağıdaki performans sayaçlarını toplama öneririz.

## <a name="nodes"></a>Düğümler

Kümenizdeki makineler için daha iyi her makinede yük anlamak ve uygun küme kararları ölçeklendirme yapmak için aşağıdaki performans sayaçlarını toplama göz önünde bulundurun.

| Sayacı kategorisi | Sayaç adı |
| --- | --- |
| PhysicalDisk (her Disk için) | Ort. Disk okuma sırası uzunluğu |
| PhysicalDisk (her Disk için) | Ort. Disk yazma sırası uzunluğu |
| PhysicalDisk (her Disk için) | Ort. Disk sn/Okuma |
| PhysicalDisk (her Disk için) | Ort. Disk sn/yazma |
| PhysicalDisk (her Disk için) | Disk Okuma/sn |
| PhysicalDisk (her Disk için) | Disk Okuma Bayt/sn |
| PhysicalDisk (her Disk için) | Disk Yazma/sn |
| PhysicalDisk (her Disk için) | Disk Yazma Bayt/sn |
| Bellek | Kullanılabilir MBayt |
| PagingFile | % Kullanım |
| Processor(Total) | % İşlemci zamanı |
| İşlem (her hizmeti) | % İşlemci zamanı |
| İşlem (her hizmeti) | İşlem kimliği |
| İşlem (her hizmeti) | Özel Baytlar |
| İşlem (her hizmeti) | İş Parçacığı Sayısı |
| İşlem (her hizmeti) | Sanal bayt sayısı |
| İşlem (her hizmeti) | Çalışma kümesi |
| İşlem (her hizmeti) | Çalışma kümesi - özel |
| Ağ Interface(all-instances) | Çıkış sırası uzunluğu |
| Ağ Interface(all-instances) | Atılan çıkış paketlerinin |
| Ağ Interface(all-instances) | Atılan alınan paket sayısı |
| Ağ Interface(all-instances) | Giden paket hataları |
| Ağ Interface(all-instances) | Hataları alınan paket sayısı |

## <a name="net-applications-and-services"></a>.NET uygulamaları ve Hizmetleri

.NET Hizmetleri kümenize dağıtıyorsanız aşağıdaki sayaçları toplayın. 

| Sayacı kategorisi | Sayaç adı |
| --- | --- |
| .NET CLR bellek (her hizmeti) | İşlem Kimliği |
| .NET CLR bellek (her hizmeti) | # Toplam kaydedilmiş bayt |
| .NET CLR bellek (her hizmeti) | # Toplam bayt ayrılmış |
| .NET CLR bellek (her hizmeti) | # Tüm yığınlardaki bayt |
| .NET CLR bellek (her hizmeti) | # Gen 0 koleksiyonları |
| .NET CLR bellek (her hizmeti) | # Gen 1 koleksiyonları |
| .NET CLR bellek (her hizmeti) | # Gen 2 koleksiyonları |
| .NET CLR bellek (her hizmeti) | GC % zaman |

### <a name="service-fabrics-custom-performance-counters"></a>Service Fabric'ın özel performans sayaçları

Service Fabric özel performans sayaçları önemli miktarda oluşturur. SDK yüklü değilse, Performans İzleyicisi uygulamanızda kapsamlı listesi Windows makinenizde görebilirsiniz (Başlat > Performans İzleyicisi). 

Uygulamalarda Reliable Actors kullanıyorsanız, kümeniz için dağıtıyorsanız, gelen countes ekleme `Service Fabric Actor` ve `Service Fabric Actor Method` kategorileri (bkz [Service Fabric güvenilir aktörler tanılama](service-fabric-reliable-actors-diagnostics.md)).

Güvenilir hizmetlerini kullanıyorsanız, benzer şekilde sahibiz `Service Fabric Service` ve `Service Fabric Service Method` toplamanız gerekir sayacı kategorileri sayaçları. 

Güvenilir koleksiyonları kullanırsanız, eklemenizi öneririz `Avg. Transaction ms/Commit` gelen `Service Fabric Transactional Replicator` işlem ölçüm başına ortalama yürütme gecikmesi toplanacak.


## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [olay oluşturma platform düzeyinde](service-fabric-diagnostics-event-generation-infra.md) Service Fabric içinde
* Aracılığıyla performans ölçümlerini derleme [OMS Aracısı](service-fabric-diagnostics-oms-agent.md)
