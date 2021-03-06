---
title: Azure Service Fabric CLI sfctl düğümlü | Microsoft Docs
description: Service Fabric CLI sfctl düğümü komutlarını açıklar.
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
ms.date: 02/22/2018
ms.author: ryanwi
ms.openlocfilehash: 50c7fe38d8bf7b14adf437f85c758e465e7d231d
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="sfctl-node"></a>sfctl node
Bir küme düğümleri yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
|    devre dışı bırak       | Belirtilen devre dışı bırakma hedefine olan bir Service Fabric küme düğümü devre dışı bırakın.|
|    etkinleştir        | Şu anda devre dışı bir Service Fabric küme düğümü etkinleştirin.|
|    sistem durumu        | Service Fabric düğümü durumunu alır.|
|    bilgileri          | Service Fabric kümesi içinde belirli bir düğüme hakkındaki bilgileri alır.|
|    liste          | Service Fabric kümedeki düğümlerin listesini alır.|
|    yükleme          | Service Fabric düğümü yük bilgilerini alır.|
|    durumu kaldırma  | Service Fabric kalıcı durum bir düğümde kalıcı olarak kaybolur veya kaldırılmış olduğunu bildirir.|
|    Sistem Durumu raporu | Service Fabric düğümü üzerindeki sistem durumu raporu gönderir.|
|    Yeniden başlatma       | Service Fabric küme düğümü yeniden başlatır.|
|    geçiş    | Başlatır veya bir küme düğümü durdurur.|
|    Geçiş durumu| StartNodeTransition kullanmaya bir işlemin ilerlemesini alır.|


## <a name="sfctl-node-disable"></a>sfctl düğümü devre dışı bırak
Belirtilen devre dışı bırakma hedefine olan bir Service Fabric küme düğümü devre dışı bırakın.

Belirtilen devre dışı bırakma hedefine olan bir Service Fabric küme düğümü devre dışı bırakın. Devre dışı bırakma işlemi devam ediyor sonra devre dışı bırakma hedefine artar, ancak azaltılan değil (örneğin, duraklatma amacıyla devre dışı bir düğüm daha fazla yeniden başlatma, ancak şekilde geçici devre dışı bırakılabilir. Düğümler, düğüm işlemi devre dışı bırakılır sonra istediğiniz zaman etkinleştir kullanarak yeniden. Devre dışı bırakma tam değilse, bunu devre dışı bırakma işlemini iptal eder. Arıza ve devre dışı durumdayken geri gelir bir düğüm hala Hizmetleri bu düğümde yerleştirilebilir önce yeniden etkinleştirilmesi gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli]| Düğümün adı.|
| --devre dışı bırakma hedefi | Hedefi veya düğüm devre dışı bırakma nedeni açıklanmaktadır. |
| --zaman aşımı -t       | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama            | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım          | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı        | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu            | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için bkz: http://jmespath.org/.|
| --ayrıntılı          | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-node-enable"></a>sfctl düğüm etkinleştir
Şu anda devre dışı bir Service Fabric küme düğümü etkinleştirin.

Şu anda devre dışı bir Service Fabric küme düğümü etkinleştirir. Etkin, düğüm yeniden yeni çoğaltmaları yerleştirme için uygun bir hedef olur ve kalan düğüm üzerinde devre dışı bırakılmış tüm yinelemeleri yeniden sonra.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli]| Düğümün adı.|
| --zaman aşımı -t       | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama            | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım          | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı        | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu            | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için bkz: http://jmespath.org/.|
| --ayrıntılı          | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-node-health"></a>sfctl düğüm durumu
Service Fabric düğümü durumunu alır.

Service Fabric düğümü durumunu alır. Sistem durumu olayları sistem durumuna bağlıdır düğümde bildirilen koleksiyonu filtrelemek için EventsHealthStateFilter kullanın. Health store içinde adına göre belirttiğiniz düğüm mevcut değilse, bu bir hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli]| Düğümün adı.|
| --Sağlık Durumu Filtresi olayları| Döndürülen HealthEvent nesnelerin sistem durumuna bağlıdır koleksiyonu filtrelemeye izin verir. Bu parametre için olası değerler aşağıdaki sistem durumlarının bir tamsayı değeri içerir. Filtreyle eşleşen olaylar döndürülür. Tüm olayları toplanmış sistem durumunu değerlendirmek için kullanılır. Belirtilmezse, tüm girişleri döndürülür. Durum değerleri bayrağı tabanlı numaralandırma olduğundan, değer, bu değerlerin Bitsel 'Veya' işleci kullanılarak edinilen bir bileşimi olabilir. Sağlanan değer 6 ise, örneğin, ardından tüm olaylar Tamam (2) ve uyarı (4), HealthState değeriyle döndürülür. -Varsayılan - varsayılan değer. Tüm HealthState eşleşir. Değer sıfır olur. -Hiçbiri - herhangi bir HealthState değer eşleşmeyen filtreleyin. Sonuç durumları belirli bir koleksiyon döndürmek için kullanılır. Değer 1'dir. -Tamam - eşleşmeleri HealthState değerle Tamam giriş filtreleyin. Değer 2'dir. -Uyarı - filtre HealthState eşleşme girişle uyarı değer. Değer 4'tür. -Hata - Giriş hata HealthState değeriyle eşleşen Filtresi. Değer 8'dir. -Tüm - giriş herhangi bir HealthState değeri ile eşleşen filtre. Değer, 65535 ' dir.|
| --zaman aşımı -t             | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                  | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı              | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu                  | JMESPath sorgu dizesi. Bkz: http://jmespath.org/ daha fazla bilgi ve örnekler.|
| --ayrıntılı                | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-node-info"></a>sfctl düğüm bilgisi
Service Fabric kümesi içinde belirli bir düğüme hakkındaki bilgileri alır.

Bilgi alır yanıt Service Fabric Cluster.The belirli bir düğümünde hakkında adı, durumu, kimliği, sistem durumu, açık kalma süresi ve diğer düğümün ayrıntılarını içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli]| Düğümün adı.|
| --zaman aşımı -t       | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama            | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım          | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı        | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu            | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için bkz: http://jmespath.org/.|
| --ayrıntılı          | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-node-list"></a>sfctl düğüm listesi
Service Fabric kümedeki düğümlerin listesini alır.

Service Fabric kümedeki düğümlerin listesini alır. Yanıt adı, durumu, kimliği, sistem durumu, açık kalma süresi ve diğer düğümün ayrıntılarını içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --devamlılık belirteci| Devamlılık belirteci parametresi, bir sonraki sonuç kümesi elde etmek için kullanılır. Sistem sonuçlarından tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değere sahip API yanıt olarak dahil edilir.      Bu değer geçirilen zaman sonraki API çağrısı API sonraki sonuç kümesi döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır.|
| --düğüm Durumu Filtresi| Üzerinde NodeStatus bağlı düğümler filtrelemeye izin verir. Yalnızca belirtilen filtre değeri eşleşen düğümleri döndürülür. Filtre değeri aşağıdakilerden biri olabilir. Varsayılan: varsayılan.|
| --zaman aşımı -t     | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama          | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım        | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı      | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu          | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için bkz: http://jmespath.org/.|
| --ayrıntılı        | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-node-load"></a>sfctl düğümü yükleme
Service Fabric düğümü yük bilgilerini alır.

Yük ya da tanımlanmış kapasite sahip tüm ölçümler için Service Fabric düğümünün yük bilgilerini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli]| Düğümün adı.|
| --zaman aşımı -t       | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama            | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım          | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı        | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu            | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için bkz: http://jmespath.org/.|
| --ayrıntılı          | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-node-restart"></a>sfctl düğümü yeniden başlatma
Service Fabric küme düğümü yeniden başlatır.

Zaten başlatılmış bir Service Fabric küme düğümü yeniden başlatır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğüm adı [gerekli]| Düğümün adı.|
| --oluşturma-doku-dökümü  | Fabric düğümü işleminin döküm oluşturmak için true değerini belirtin. Büyük küçük harfe duyarlı budur.  Varsayılan: False.|
| --düğümü örnek kimliği | Hedef düğüm örnek kimliği. Örnek kimliği yalnızca düğüm geçerli örnekle eşleşmesi durumunda düğüm yeniden belirtilir. Varsayılan değer "0" herhangi bir örnek kimliği eşleşir Örnek kimliği get düğümü sorgusu kullanılarak edinilebilir.  Varsayılan: 0.|
| --zaman aşımı -t       | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama            | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım          | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı        | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.  Varsayılan: json.|
| --Sorgu            | JMESPath sorgu dizesi. Daha fazla bilgi ve örnekler için bkz: http://jmespath.org/.|
| --ayrıntılı          | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="sfctl-node-transition"></a>sfctl düğüm geçiş
Başlatır veya bir küme düğümü durdurur.

Başlatır veya bir küme düğümü durdurur.  Bir küme düğümü bir, işletim sistemi örneği kendisini değil işlemidir.
Bir düğüm başlatmak için NodeTransitionType parametresi için "Başlangıç" geçirin. Bir düğüm durdurmak için NodeTransitionType parametresi için "Durdur" geçirin. Düğüm henüz geçiş tamamlanmadığından API geri döndüğünde bu API - işlemini başlatır. İşlemin ilerlemesi almak için aynı Operationıd ile GetNodeTransitionProgress çağırın. 

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --düğümü-örnek-kimliği [gerekli]| Hedef düğüm düğümü örnek kimliği. Bu GetNodeInfo API aracılığıyla belirlenebilir.|
| --düğüm adı [gerekli]| Düğümün adı.|
| --düğümü-geçiş-type [gerekli]| Gerçekleştirmek için geçiş türünü belirtir.                       NodeTransitionType.Start durdurulmuş bir düğüm başlatır.                       NodeTransitionType. Stop çalışır durumda bir düğüm durdurur. |
| --işlem kimliği [gerekli]| Bu API çağrısının tanımlayan bir GUID.  Bu, karşılık gelen GetProgress API geçirilir.|
| --stop-süresi-içinde-[gerekli] (saniye)| Saniye cinsinden süre düğümünü korumak için durduruldu.  En düşük değer 600, en fazla 14400. Bu süre dolduktan sonra düğümü otomatik olarak geri gelir.|
| --zaman aşımı -t                      | Sunucu zaman aşımı saniye cinsinden.  Varsayılan: 60.|

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama                           | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın.|
| ---h Yardım                         | Bu yardım iletisini ve çıkış gösterir.|
| ---o çıktı                       | Çıktı biçimi.  İzin verilen değerler: json, jsonc, tablo, tsv.                       Varsayılan: json.|
| --Sorgu                           | JMESPath sorgu dizesi. Bkz: http://jmespath.org/ daha fazla bilgi ve örnekler.|
| --ayrıntılı                         | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama.|

## <a name="next-steps"></a>Sonraki adımlar
- [Kurulum](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).