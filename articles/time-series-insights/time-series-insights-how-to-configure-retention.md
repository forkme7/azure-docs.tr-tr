---
title: "Azure zaman serisi Öngörüler ortamınızda bekletme yapılandırma | Microsoft Docs"
description: "Bu makalede, Azure zaman serisi Öngörüler ortamınızda bekletme yapılandırma açıklar."
services: time-series-insights
ms.service: time-series-insights
author: anshan
ms.author: anshan
manager: kfile
editor: MicrosoftDocs/tsidocs
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 02/09/2018
ms.openlocfilehash: bd688f516e200a37a6c88a8779282f7391eaf8b8
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="configuring-retention-in-time-series-insights"></a>Zaman serisi öngörü bekletme yapılandırma
Bu makalede nasıl yapılandırılacağını açıklar **veri saklama süresini** ve **depolama sınırını aştığından davranışı** Azure zaman serisi ilişkin bilgiler.

Her zaman serisi Öngörüler (TSI) ortamı yapılandırmak için bir ayarının **veri saklama süresini**. Değer 1'den 400 gün olarak yayılır. Veriler ortam depolama kapasitesi veya bekletme süreye göre (1-400) silinir, hangisi önce gelirse.

Her TSI ortamı ek ayarının **depolama sınırını aştığından davranış**. Maksimum kapasite bir ortamın ulaşıldığında bu ayarı; giriş ve temizleme davranışını denetler. Aralarından seçim yapabileceğiniz iki davranışları vardır:
- **Eski veri temizleme** (varsayılan)  
- **Duraklatma giriş**

Bu ayarları daha iyi anlamak ayrıntılı bilgi için gözden [zaman serisi Öngörüler anlama bekletmeyi](time-series-insights-concepts-retention.md).  

## <a name="configure-data-retention"></a>Veri saklama yapılandırın

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Mevcut zaman serisi Öngörüler ortamınızın bulun. Seçin **tüm kaynakları** Azure portalının sol tarafındaki menüde. Zaman Serisi Görüşleri ortamınızı seçin.

3. Altında **ayarları** başlığını seçin **yapılandırma**.

4. Seçin **veri saklama süresini** kaydırıcı çubuğun kullanarak bekletme yapılandırmak veya metin kutusuna bir sayı yazın.

5. Not **kapasite** bu yapılandırma en büyük miktarda veri olaylarını ve verilerini depolamak için toplam depolama kapasitesi etkiler beri ayarlama. 

6. İki durumlu **depolama sınırını aştığından davranışı** ayarı. Seçin **temizleme eski verileri** veya **duraklatma giriş** davranışı.

7. Seçin **kaydetmek** değişiklikleri yapılandırmak için.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için gözden [zaman serisi Öngörüler anlama bekletmeyi](time-series-insights-concepts-retention.md).
