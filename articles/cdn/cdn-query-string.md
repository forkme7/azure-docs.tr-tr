---
title: Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - standart katmanı denetlemek | Microsoft Docs
description: Azure CDN sorgu dizesini önbelleğe alma denetimleri web isteğine bir sorgu dizesi içerdiğinde nasıl dosyaları önbelleğe alınır. Bu makalede Azure CDN standart ürünlerinde önbelleğe alma sorgu dizesi.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: mazha
ms.openlocfilehash: ed6f0b2c021fc4b31b85986c07df0502dba826f2
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---standard-tier"></a>Denetim Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - standart katmanı
> [!div class="op_single_selector"]
> * [Standart katman](cdn-query-string.md)
> * [Premium katman](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Genel Bakış
Sorgu dizesini önbelleğe alma ile Azure içerik teslim ağı (CDN) dosyaları için bir sorgu dizesi içeren bir web isteği nasıl önbelleğe denetler. Sorgu dizesi olan bir web isteğinde sorgu dizesi, bir soru işareti (?) sonra oluşan istek bölümüdür. Bir sorgu dizesi alan adını ve değerini bir eşittir işareti (=) tarafından ayrılır bir veya daha fazla anahtar-değer çiftleri içerebilir. Her anahtar-değer çifti ampersan tarafından ayrılmış (&). Örneğin, http:\//www.contoso.com/content.mov?field1=value1 & alan2 Value2. Bir isteğin sorgu dizesi içinde birden fazla anahtar-değer çifti varsa, bunların sırası önemli değildir. 

> [!NOTE]
> Azure CDN standart ve premium ürünleri aynı sorgu dizesini önbelleğe alma işlevselliğini sağlar, ancak kullanıcı arabirimi farklıdır.  Bu makalede arabirim için **akamai'den Azure CDN standart** ve **verizon'dan Azure CDN standart**. İle sorgu dizesi önbelleğe alma için **verizon'dan Azure CDN Premium**, bkz: [denetim Azure CDN önbelleğe alma davranışını sorgu dizeleriyle - premium katmanı](cdn-query-string-premium.md).
>

Üç sorgu dizesi modu kullanılabilir:

- **Sorgu dizelerini yoksayabilir**: varsayılan mod. Bu modda, CDN bulunma noktası (POP) düğüm sorgu dizeleri istek için kaynak sunucu üzerinde yapılan ilk istek geçirir ve varlık önbelleğe alır. Önbelleğe alınan varlık süresi doluncaya kadar POP sunulan tüm sonraki istekleri varlığı için sorgu dizelerini yoksayabilir.

- **Sorgu dizeleri için önbelleğe almayı atla**: Bu modda, CDN POP düğümde sorgu dizeleri içeren istekleri önbelleğe alınmaz. POP düğüm doğrudan kaynak sunucudan varlığı alır ve her istek ile istek sahibi geçirir.

- **Her benzersiz URL'yi önbelleğe**: Bu modda, sorgu dizesi dahil olmak üzere benzersiz bir URL'ye sahip her isteği kendi önbelleği ile benzersiz bir varlık olarak kabul edilir. Örneğin, example.ashx?q=test1 için bir istek için kaynak sunucudan yanıt POP düğümde önbelleğe ve sonraki önbellekleri ile aynı sorgu dizesi döndürdü. Bir istek example.ashx?q=test2 için kendi yaşam süresi ayarı ile ayrı bir varlık olarak önbelleğe alınır.
   
    >[!IMPORTANT] 
    > Düşük bir önbellek isabet oranı neden sorgu dizesi bir oturum kimliği veya bir kullanıcı adı gibi her istek ile değiştirilecek parametreleri içerdiğinde, bu mod kullanmayın.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Sorgu dizesini önbelleğe alma standart CDN profili ayarlarını değiştirme
1. CDN profili açın ve sonra yönetmek istediğiniz CDN uç noktası seçin.
   
   ![CDN profili uç noktaları](./media/cdn-query-string/cdn-endpoints.png)
   
2. Sol bölmede ayarları altında tıklatın **kuralları önbelleğe alma**.
   
    ![Kuralları düğmesini CDN önbelleğe alma](./media/cdn-query-string/cdn-caching-rules-btn.png)
   
3. İçinde **sorgu dizesini önbelleğe alma davranışı** listesinde, bir sorgu dizesi modunu seçin ve ardından **kaydetmek**.
   
   ![CDN sorgu dizesini önbelleğe alma seçenekleri](./media/cdn-query-string/cdn-query-string.png)

> [!IMPORTANT]
> Kaydın yayılması zaman alır çünkü önbelleği dize ayarları değişiklikleri hemen görünür olmayabilir: 
> - İçin **akamai'den Azure CDN standart** profilleri yayma işlemi genellikle bir dakika içinde tamamlanır. 
> - İçin **verizon'dan Azure CDN standart** profilleri yayma işlemi genellikle 90 dakika içinde tamamlanır.
>


