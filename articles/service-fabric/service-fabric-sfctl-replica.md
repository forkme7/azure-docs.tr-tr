---
title: Azure Service Fabric CLI - sfctl çoğaltma | Microsoft Docs
description: Service Fabric CLI sfctl çoğaltma komutlarını açıklar.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: ba67a2a20d3f3e8e9fbccb2674cea500bfbde3fb
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="sfctl-replica"></a>sfctl replica
Hizmet bölümlerini ait çoğaltmaları yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
|    Dağıtılan  | Service Fabric düğümde dağıtılan çoğaltma ayrıntılarını alır.|
|    dağıtılan listesi| Service Fabric düğümde dağıtılan çoğaltmaların listesini alır.|
|    sistem durumu    | Service Fabric durum bilgisi olan hizmet çoğaltma veya durum bilgisiz hizmet örneği durumunu alır.|
|    bilgileri      | Service Fabric bölümü bir çoğaltma bilgilerini alır.|
|    liste      | Bir Service Fabric hizmeti bölüm çoğaltmaları bilgilerini alır.|
|    kaldır    | Bir düğüm üzerinde çalışan bir hizmet çoğaltmaları kaldırır.|
|    Sistem Durumu raporu| Service Fabric çoğaltma sistem durumu raporu gönderir.|
|    Yeniden başlatma   | Bir düğüm üzerinde çalışan kalıcı bir hizmet hizmet çoğaltmasını yeniden başlatır.|


## <a name="sfctl-replica-deployed"></a>dağıtılan sfctl çoğaltma
Service Fabric düğümde dağıtılan çoğaltma ayrıntılarını alır.

Service Fabric düğümde dağıtılan çoğaltma ayrıntılarını alır. Bu bilgiler, hizmet türü, hizmet adı, geçerli hizmet işleminin içerir, geçerli hizmet işleminin başlangıç tarih saat, bölüm kimliği, çoğaltma/örnek kimliği, bildirilen yük ve diğer bilgileri.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli]| Düğümün adı.|
| --bölüm kimliği [gerekli]| Bölüm kimliği.|
| --Çoğaltma kimliği [gerekli]| Çoğaltma tanımlayıcısı.|
| --zaman aşımı -t          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım             | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu               | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için bkz: http://jmespath.org/.|
| --ayrıntılı             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-replica-health"></a>sfctl çoğaltma durumu
Service Fabric durum bilgisi olan hizmet çoğaltma veya durum bilgisiz hizmet örneği durumunu alır.

Service Fabric çoğaltma durumunu alır. Sistem durumu olayları sistem durumuna bağlıdır çoğaltma bildirilen koleksiyonu filtrelemek için EventsHealthStateFilter kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli]| Bölüm kimliği.|
| --Çoğaltma kimliği [gerekli]| Çoğaltma tanımlayıcısı.|
| --Sağlık Durumu Filtresi olayları| Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir.|
| --zaman aşımı -t             | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                  | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı              | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                  | JMESPath sorgu dizesi. Daha fazla bilgi için bkz: http://jmespath.org/.|
| --ayrıntılı                | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-replica-info"></a>sfctl çoğaltma bilgileri
Service Fabric bölümü bir çoğaltma bilgilerini alır.

Yanıt kimliği, rol, durum, sistem durumu, düğüm adı, açık kalma süresi ve çoğaltma ile ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli]| Bölüm kimliği.|
| --Çoğaltma kimliği [gerekli]| Çoğaltma tanımlayıcısı.|
| --devamlılık belirteci  | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değere sahip API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır.|
| --zaman aşımı -t          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım             | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu               | JMESPath sorgu dizesi. Daha fazla bilgi için bkz: http://jmespath.org/.|
| --ayrıntılı             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-replica-list"></a>sfctl çoğaltma listesi
Bir Service Fabric hizmeti bölüm çoğaltmaları bilgilerini alır.

GetReplicas endpoint belirtilen bölüm çoğaltmaları hakkında bilgi döndürür.
Respons kimliği, rol, durum, sistem durumu, düğüm adı, açık kalma süresi ve çoğaltma ile ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --bölüm kimliği [gerekli]| Bölüm kimliği.|
| --devamlılık belirteci  | Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değere sahip API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır.|
| --zaman aşımı -t          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım             | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu               | JMESPath sorgu dizesi. Bkz: http://jmespath.org/ daha fazla bilgi ve örnekler.|
| --ayrıntılı             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-replica-remove"></a>sfctl çoğaltma Kaldır
Bir düğüm üzerinde çalışan bir hizmet çoğaltmaları kaldırır.

Bu API, Service Fabric kümesinden bir çoğaltma kaldırarak bir Service Fabric çoğaltma hatası benzetimini yapar. Kaldırma çoğaltma kapatır, çoğaltma rolü yok ve ardından kaldırır tüm geçişler kümeden çoğaltma durumu bilgileri. Bu API çoğaltma durumu temizleme yolu sınar ve istemci API rapor hataya kalıcı yolundan benzetimini yapar. Uyarı - orada yapılmaz bu API kullanıldığında gerçekleştirilen güvenliği. Bu API yanlış kullanımı durum bilgisi olan hizmetler için veri kaybına neden olabilir. Ayrıca, aynı işlemde barındırılan tüm çoğaltmalar forceRemove bayrağı etkiler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli]| Düğümün adı.|
| --bölüm kimliği [gerekli]| Bölüm kimliği.|
| --Çoğaltma kimliği [gerekli]| Çoğaltma tanımlayıcısı.|
| --zorla Kaldır        | Bir Service Fabric uygulaması veya hizmeti zorla kapama sırası geçmeden kaldırın. Bu parametre zorla bir uygulamayı silmek için kullanılan veya hizmet için hangi silmeyi zaman aşımına uğramadan engelleyen hizmet kodda sorunları nedeniyle normal olduğundan kopyaları kapatın.|
| --zaman aşımı -t          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım             | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu               | JMESPath sorgu dizesi. Bkz: http://jmespath.org/ daha fazla bilgi ve örnekler.|
| --ayrıntılı             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-replica-restart"></a>sfctl çoğaltma yeniden başlatma
Bir düğüm üzerinde çalışan kalıcı bir hizmet hizmet çoğaltmasını yeniden başlatır.

Bir düğüm üzerinde çalışan kalıcı bir hizmet hizmet çoğaltmasını yeniden başlatır. Uyarı - orada yapılmaz bu API kullanıldığında gerçekleştirilen güvenliği. Bu API yanlış kullanımı durum bilgisi olan hizmetler için kullanılabilirlik kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli]| Düğümün adı.|
| --bölüm kimliği [gerekli]| Bölüm kimliği.|
| --Çoğaltma kimliği [gerekli]| Çoğaltma tanımlayıcısı.|
| --zaman aşımı -t          | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama               | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım             | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı           | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu               | JMESPath sorgu dizesi. Bkz: http://jmespath.org/ daha fazla bilgi ve örnekler.|
| --ayrıntılı             | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).
