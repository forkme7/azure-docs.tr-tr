---
title: "MySQL için Azure veritabanında izleme"
description: "Bu makalede izleme ve Azure veritabanı için CPU, depolama ve bağlantı istatistikleri de dahil olmak üzere MySQL için uyarı için ölçümleri açıklanmaktadır."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 03/15/2018
ms.openlocfilehash: c3cba00077fd65239382d6fdd98e73a55f926b3b
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="monitoring-in-azure-database-for-mysql"></a>MySQL için Azure veritabanında izleme
İzleme verilerini sunucularınız hakkında sorun giderme ve işleminizi iş yükü için en iyi duruma yardımcı olur. Azure veritabanı MySQL için MySQL server destekleyen kaynaklarda davranışını bir anlayış veren çeşitli ölçümleri sağlar. 

## <a name="metrics"></a>Ölçümler
Tüm Azure ölçümlerini bir dakikalık sıklık, ve her ölçümü geçmişi 30 gün sağlar. Ölçümleri uyarılar yapılandırabilirsiniz. Adım adım yönergeler için bkz [uyarıları ayarlamak nasıl](howto-alert-on-metric.md). Diğer görevleri otomatik eylemler oluşturma, gelişmiş analizler gerçekleştirme ve geçmiş arşivleme içerir. Daha fazla bilgi için bkz: [Azure ölçümleri genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Ölçümleri listesi
Bu ölçümleri, Azure veritabanı için MySQL için kullanılabilir:

|Ölçüm|Ölçüm görünen adı|Birim|Açıklama|
|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|CPU yüzdesi kullanımda.|
|memory_percent|Bellek yüzdesi|Yüzde|Kullanılan bellek yüzdesi.|
|io_consumption_percent|G/ç yüzdesi|Yüzde|G/ç yüzdesi kullanımda.|
|storage_percent|Depolama yüzdesi|Yüzde|En fazla sunucunun dışında kullanılan depolama alanı yüzdesi.|
|storage_used|Kullanılan depolama|Bayt|Kullanımdaki depolama miktarı. Veritabanı dosyaları, işlem günlüklerinin ve sunucu günlüklerini hizmet tarafından kullanılan depolama alanını içerir.|
|storage_limit|Depolama sınırı|Bayt|Bu sunucu için en fazla depolama alanı.|
|active_connections|Toplam etkin bağlantılar|Sayı|Sunucuya etkin bağlantı sayısı.|
|connections_failed|Toplam başarısız bağlantıları|Sayı|Sunucuya başarısız bağlantılarının sayısı.|


## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [uyarıları ayarlamak nasıl](howto-alert-on-metric.md) ölçüm üzerinde bir uyarı oluşturma konusunda yönergeler için.
- Erişim ve Azure portalı, REST API veya CLI kullanarak ölçümleri dışarı aktarma hakkında daha fazla bilgi için bkz: [Azure ölçümleri genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
