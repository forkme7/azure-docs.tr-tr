---
title: Azure Content Delivery Network (CDN) ürün özellikleri | Microsoft Docs
description: Her Azure Content Delivery Network (CDN) ürününün desteklediği özellikler hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/23/2018
ms.author: rli
ms.custom: mvc
ms.openlocfilehash: 40638e2180b63c90fbcbe15cc2c1e944a97e166e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="azure-cdn-product-features"></a>Azure CDN ürün özellikleri

Üç Azure Content Delivery Network (CDN) ürünü mevcuttur: **Akamai’den Azure CDN Standart**, **Verizon’dan Azure CDN Standart** ve **Verizon’dan Azure CDN Premium**. Aşağıdaki tabloda her ürünle birlikte sunulan özellikler karşılaştırılmaktadır.

| **Performans özellikleri ve iyileştirmeler** | Standart Akamai | Standart Verizon | Premium Verizon |
| --- | --- | --- | --- |
| [Dinamik site hızlandırma](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration) | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dinamik site hızlandırma - uyarlamalı görüntü sıkıştırma](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only) | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dinamik site hızlandırma - nesneleri önceden getirme](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only) | **&#x2713;**  |  |  |
| [Video akışı iyileştirmesi](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization) | **&#x2713;**  | \* |  \* |
| [Büyük dosyaları iyileştirme](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization) | **&#x2713;**  | \* |  \* |
| [Genel sunucu yük dengelemesi (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Hızlı temizleme](cdn-purge-endpoint.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Varlık önceden yükleme](cdn-preload-endpoint.md) | |**&#x2713;** |**&#x2713;** |
| Önbellek/üst bilgi ayarları ([önbelleğe alma](cdn-caching-rules.md) kullanılarak) |**&#x2713;** |**&#x2713;** | |
| Önbellek/üstbilgi ayarları ([kurallar altyapısı](cdn-rules-engine.md) kullanılarak) | | |**&#x2713;** |
| [Sorgu dizesini önbelleğe alma](cdn-query-string.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| IPv4/IPv6 ikili yığını |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTP/2 desteği](cdn-http2.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
 **Güvenlik** | **Standart Akamai** | **Standart Verizon** | **Premium Verizon** | 
| CDN uç noktasıyla HTTPS desteği |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Özel etki alanı HTTPS](cdn-custom-ssl.md) | |**&#x2713;** |**&#x2713;** |
| [Özel etki alanı adı desteği](cdn-map-content-to-custom-domain.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Coğrafi filtreleme](cdn-restrict-access-by-country.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Belirteç kimlik doğrulaması](cdn-token-auth.md)|  |  |**&#x2713;**| 
| [DDOS koruması](https://www.us-cert.gov/ncas/tips/ST04-015) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
| **Analiz ve raporlama** | **Standart Akamai** | **Standart Verizon** | **Premium Verizon** | 
| [Azure tanılama günlükleri](cdn-azure-diagnostic-logs.md) | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Verizon'dan alınan çekirdek raporlar](cdn-analyze-usage-patterns.md) | |**&#x2713;** |**&#x2713;** |
| [Verizon özel raporları](cdn-verizon-custom-reports.md) | |**&#x2713;** |**&#x2713;** |
| [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md) | | |**&#x2713;** |
| [Gerçek zamanlı istatistikler](cdn-real-time-stats.md) | | |**&#x2713;** |
| [Uç düğümü performansı](cdn-edge-performance.md) | | |**&#x2713;** |
| [Gerçek zamanlı uyarılar](cdn-real-time-alerts.md) | | |**&#x2713;** |
||||
| **Kullanım kolaylığı** | **Standart Akamai** | **Standart Verizon** | **Premium Verizon** | 
| [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service/app-service-web-tutorial-content-delivery-network.md) ve [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) gibi Azure hizmetleriyle kolay tümleştirme |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md), veya [PowerShell](cdn-manage-powershell.md) aracılığıyla yönetim |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Özelleştirilebilir, kural tabanlı içerik teslim altyapısı](cdn-rules-engine.md) | | |**&#x2713;** |
| URL yeniden yönlendirme/yeniden yazma ([kurallar altyapısı](cdn-rules-engine.md) kullanılarak) | | |**&#x2713;** |
| Mobil cihaz kuralları ([kurallar altyapısı](cdn-rules-engine.md) kullanılarak) | | |**&#x2713;** |

\* Verizon, doğrudan genel web teslimatı iyileştirmesi aracılığıyla büyük dosya ve medya göndermeyi destekler.



