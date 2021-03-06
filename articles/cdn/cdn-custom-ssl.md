---
title: 'Öğretici: Azure CDN özel etki alanı üzerinde HTTPS yapılandırma | Microsoft Docs'
description: Bu öğreticide, Azure CDN uç noktası özel etki alanınızda HTTPS’yi nasıl etkinleştirip devre dışı bırakacağınızı öğrenirsiniz.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/12/2018
ms.author: rli
ms.custom: mvc
ms.openlocfilehash: a8f2da5a68552c35a55a7bbb764afc7b36af6962
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-configure-https-on-an-azure-cdn-custom-domain"></a>Öğretici: Azure CDN özel etki alanı üzerinde HTTPS yapılandırma

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Bu öğreticide bir Azure Content Delivery Network (CDN) uç noktası ile ilişkili özel bir etki alanı için HTTP protokolünün nasıl etkinleştirileceği gösterilir. Özel etki alanınızda HTTPS protokolünü kullanarak (örneğin, https:\//www.contoso.com), hassas veriler internet üzerinden gönderildiğinde bunların SSL şifrelemesi ile güvenli bir şekilde teslim edilmesini sağlarsınız. HTTPS, güven ve kimlik doğrulaması sunmasının yanı sıra web uygulamalarınızı saldırılara karşı korur. HTTPS'yi etkinleştirmeye yönelik iş akışı, hiçbir ek maliyet olmadan tek tıklamayla etkinleştirme ve eksiksiz sertifika yönetimi sayesinde basitleştirilir.

Azure CDN varsayılan olarak CDN uç noktası ana bilgisayar adı üzerinde HTTPS’yi destekler. Örneğin, bir CDN uç noktası (https:\//contoso.azureedge.net gibi) oluşturursanız HTTPS otomatik olarak etkinleştirilir.  

HTTPS özelliğinin en önemli niteliklerinden bazıları şunlardır:

- Ek ücret yoktur: Sertifika edinme veya yenileme işlemleri için herhangi bir maliyet söz konusu değildir ve HTTPS trafiği için ek ücret alınmaz. Yalnızca CDN’den GB çıkışı için ücret ödersiniz.

- Basit etkinleştirme: Tek tıklamayla sağlama özelliği [Azure portalından](https://portal.azure.com) kullanılabilir. Özelliği etkinleştirmek için REST API’yi veya diğer geliştirici araçlarını kullanabilirsiniz.

- Eksiksiz sertifika yönetimi: Sizin için tüm sertifika tedariki ve yönetimi gerçekleştirilir. Sertifikalar sona ermeden önce otomatik olarak sağlanır ve yenilenir. Bu da sertifika süre sonu nedeniyle hizmette yaşanabilecek kesinti risklerini ortadan kaldırır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> - Özel etki alanınızda HTTPS protokolünü etkinleştirme
> - Etki alanını doğrulama
> - Özel etki alanınızda HTTPS protokolünü devre dışı bırakma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticideki adımları tamamlayabilmeniz için öncelikle bir CDN profili ve en az bir CDN uç noktası oluşturmanız gerekir. Daha fazla bilgi için bkz. [Hızlı Başlangıç: Azure CDN profili ve uç noktası oluşturma](cdn-create-new-endpoint.md).

Ayrıca CDN uç noktanızda bir Azure CDN özel etki alanını ilişkilendirebilirsiniz. Daha fazla bilgi için bkz. [Öğretici: Azure CDN uç noktanıza özel etki alanı ekleme](cdn-map-content-to-custom-domain.md)

## <a name="enable-the-https-feature"></a>HTTPS özelliğini etkinleştirme

Özel bir etki alanı üzerinde HTTPS'yi etkinleştirmek için aşağıdaki adımları uygulayın:

1. [Azure portalında](https://portal.azure.com) **Verizon'dan Azure CDN Standart** veya **Verizon'dan Azure CDN Premium** CDN profiline gidin.

2. CDN uç noktaları listesinde özel etki alanınızı içeren uç noktayı seçin.

    ![Uç noktalar listesi](./media/cdn-custom-ssl/cdn-select-custom-domain-endpoint.png)

    **Uç Nokta** sayfası görünür.

3. Özel etki alanları listesinde, HTTPS'yi etkinleştirmek istediğiniz özel etki alanını seçin.

    ![Özel etki alanları listesi](./media/cdn-custom-ssl/cdn-custom-domain.png)

    **Özel etki alanı** sayfası görünür.

4. HTTPS’yi etkinleştirmek için **Açık**’ı, daha sonra **Uygula**’yı seçin.

    ![Özel etki alanı HTTPS durumu](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="validate-the-domain"></a>Etki alanını doğrulama

CNAME kaydı kullanılarak özel uç noktanızla eşlenen ve kullanılmakta olan bir özel etki alanınız zaten varsa şu adıma geçin:  
[Özel etki alanı, CDN uç noktanızla eşlendi](#custom-domain-is-mapped-to-your-cdn-endpoint-by-a-cname-record). Böyle bir etki alanınız yoksa uç noktanıza yönelik CNAME kaydı girişinin artık mevcut olmaması veya cdnverify alt etki alanını içeriyor olması durumunda [Özel etki alanı, CDN uç noktanızla eşlenmedi](#custom-domain-is-not-mapped-to-your-cdn-endpoint) adımına geçin.

### <a name="custom-domain-is-mapped-to-your-cdn-endpoint-by-a-cname-record"></a>Özel etki alanı bir CNAME kaydı kullanılarak CDN uç noktanızla eşlendi

Uç noktanıza özel etki alanı eklediğinizde etki alanı kayıt yetkilinizin DNS tablosunda, CDN uç noktası ana bilgisayar adınızla eşlenecek bir CNAME kaydı oluşturmuş oldunuz. Bu CNAME kaydı hala mevcutsa ve cdnverify alt etki alanını içermiyorsa DigiCert sertifika yetkilisi (CA), özel etki alanınızın sahipliğini doğrulamak için bunu kullanır. 

CNAME kaydınız, *Ad*’ın özel etki alanınız, *Değer*’in ise CDN uç noktası ana bilgisayar adınız olduğu aşağıdaki biçimde olmalıdır:

| Adı            | Tür  | Değer                 |
|-----------------|-------|-----------------------|
| www.contoso.com | CNAME | contoso.azureedge.net |

CNAME kayıtları hakkında daha fazla bilgi için bkz. [CNAME DNS kaydı oluşturma](https://docs.microsoft.com/en-us/azure/cdn/cdn-map-content-to-custom-domain#create-the-cname-dns-records).

CNAME kaydınız doğru biçimdeyse DigiCert, özel etki alanı adınızı otomatik olarak doğrular ve Konu Diğer Adları (SAN) sertifikasına ekler. DigitCert size doğrulama e-postası göndermez ve isteğinizi onaylamanız gerekmez. Sertifika bir yıl süreyle geçerlidir ve süresi dolmadan önce otomatik olarak yenilenir. [Yayılma için bekleme](#wait-for-propagation) adımına geçin. 

Otomatik doğrulama genellikle birkaç dakika sürer. Bir saat içinde etki alanınızı doğrulanmış olarak görmüyorsanız destek bileti açın.

>[!NOTE]
>DNS sağlayıcınız ile bir Sertifika Yetkilisi Yetkilendirme (CAA) kaydınız varsa bunun geçerli CA olarak DigiCert‘i içermesi gerekir. CAA kaydı; etki alanı sahiplerinin DNS sağlayıcıları ile hangi CA'ların, etki alanları için sertifika vermeye yetkili olduğunu belirtmesini sağlar. Bir CA, CAA kaydına sahip bir etki alanı için sertifika siparişi alırsa ve bu CA, sertifika vermeye yetkili olarak listelenmemişse bu CA’nın, söz konusu etki alanına veya alt etki alanına sertifika vermesi yasaklanır. CAA kayıtlarını yönetme ile ilgili bilgi için bkz. [CAA kayıtlarını yönetme](https://support.dnsimple.com/articles/manage-caa-record/). CAA kayıt aracı için bkz. [CAA Kayıt Yardımcısı](https://sslmate.com/caa/).

### <a name="custom-domain-is-not-mapped-to-your-cdn-endpoint"></a>Özel etki alanı, CDN uç noktanızla eşlenmedi

Uç noktanız için CNAME kaydı girişi artık mevcut değilse veya cdnverify alt etki alanını içeriyorsa bu adımın diğer yönergelerini uygulayın.

Özel etki alanınızda HTTPS etkinleştirdikten sonra DigiCert sertifika yetkilisi (CA), etki alanının [WHOIS](http://whois.domaintools.com/) kayıt yetkilisi bilgisine göre kayıt yetkilisiyle iletişim kurarak etki alanınızın sahipliğini doğrular. İletişim, WHOIS kaydında belirtilen e-posta adresi (varsayılan) veya telefon numarası aracılığıyla kurulur. HTTPS, özel etki alanınızda etkin hale gelmeden önce etki alanı doğrulamasını tamamlamanız gerekir. Etki alanını onaylamak için altı iş gününüz vardır. Altı iş günü içinde onaylanmamış istekler otomatik olarak iptal edilir. 

![WHOIS kaydı](./media/cdn-custom-ssl/whois-record.png)

DigiCert ayrıca ek e-posta adreslerine de bir doğrulama e-postası gönderir. WHOIS kayıt yetkilisi bilgileri özelse doğrudan şu adreslerden birini kullanarak onaylayabildiğinizi doğrulayın:

admin@&lt;etki-alanı-adınız.com&gt;  
administrator@&lt;etki-alanı-adınız.com&gt;  
webmaster@&lt;etki-alanı-adınız.com&gt;  
hostmaster@&lt;etki-alanı-adınız.com&gt;  
postmaster@&lt;.com&gt;  

Birkaç dakika içinde sizden isteği onaylamanızı isteyen, aşağıdaki örneğe benzer bir e-posta alırsınız. İstenmeyen posta filtresi kullanıyorsanız admin@digicert.com adresini bu filtrenin beyaz listesine ekleyin. E-postayı 24 saat içinde almazsanız Microsoft destek ekibine başvurun.
    
![Etki alanı doğrulama e-postası](./media/cdn-custom-ssl/domain-validation-email.png)

Onay bağlantısına tıkladığınızda, aşağıdaki çevrimiçi onay formuna yönlendirilirsiniz: 
    
![Etki alanı doğrulama formu](./media/cdn-custom-ssl/domain-validation-form.png)

Formdaki yönergeleri uygulayın. İki doğrulama seçeneğiniz vardır:

- contoso.com gibi aynı kök etki alanı için aynı hesap üzerinden verilen gelecekteki tüm siparişleri onaylayabilirsiniz. Aynı kök etki alanı için ek özel etki alanları eklemeyi planlıyorsanız bu yaklaşım önerilir.

- Yalnızca bu istekte kullanılan söz konusu ana bilgisayar adını onaylayabilirsiniz. Sonraki istekler için ek onay gereklidir.

Onaydan sonra DigiCert, SAN sertifikasına için özel etki alanı adınızı ekler. Sertifika bir yıl süreyle geçerlidir ve süresi dolmadan önce otomatik olarak yenilenir.

## <a name="wait-for-propagation"></a>Yayılma için bekleme

Etki alanı doğrulandıktan sonra özel etki alanı HTTPS özelliğinin etkinleştirilmesi 6-8 saate kadar sürebilir. İşlem tamamlandığında, Azure portalındaki özel HTTPS durumu **Etkin** olarak ayarlanır ve özel etki alanı iletişim kutusundaki dört işlem adımı tamamlandı olarak işaretlenir. Özel etki alanınız artık HTTPS’yi kullanmak için hazırdır.

![HTTPS iletişim kutusunu etkinleştirme](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>İşlem ilerleme durumu

Aşağıdaki tabloda, HTTPS’yi etkinleştirdiğinizde oluşan işlem ilerleme durumunu gösterir. HTTPS’yi etkinleştirmenizin ardından özel etki alanı iletişim kutusunda dört işlem adımı görünür. Her adım etkin hale gelir ve ilerledikçe adımın altında ek alt adım ayrıntıları görüntülenir. Bu alt adımların hiçbiri gerçekleşmez. Bir adım başarıyla tamamlandıktan sonra adımın yanında yeşil bir onay işareti görünür. 

| İşlem adımı | İşlem alt adımı ayrıntıları | 
| --- | --- |
| 1 İstek gönderiliyor | İstek gönderiliyor |
| | HTTPS isteğiniz gönderiliyor. |
| | HTTPS isteğiniz başarıyla gönderildi. |
| 2 Etki alanı doğrulaması | CNAME için CDN Uç Noktası ile eşleme yapıldıysa etki alanı otomatik olarak doğrulanır. Eşleme yapılmadıysa etki alanınızın kayıt kaydında listelenen e-posta adresine (WHOIS kayıt şirketi) bir doğrulama isteği gönderilir. Lütfen etki alanını mümkün olan en kısa süre içinde doğrulayın. |
| | Etki alanı sahipliğiniz başarıyla doğrulandı. |
| | Etki alanı doğrulama isteğinin süresi doldu. (Müşteri büyük olasılıkla 6 gün içinde yanıt vermedi.) HTTPS, etki alanınızda etkinleştirilmeyecek. * |
| | Etki alanı sahipliğini doğrulama isteği, müşteri tarafından reddedildi. HTTPS, etki alanınızda etkinleştirilmeyecek. * |
| 3 Sertifika sağlanıyor | Sertifika yetkilisi şu anda etki alanınızdaki HTTPS'nin etkinleştirilmesi için gereken sertifikayı veriyor. |
| | Sertifika verildi ve şu anda CDN ağına dağıtılıyor. Bu işlem 6 saat kadar sürebilir. |
| | Sertifika, CDN ağına başarıyla dağıtıldı. |
| 4 Tamamlandı | HTTPS, etki alanınızda başarıyla etkinleştirildi. |

\* Bir hata oluşmazsa sürece bu ileti görüntülenmez. 

İstek gönderilmeden önce hata oluşursa, aşağıdaki hata iletisi görüntülenir:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>

## <a name="clean-up-resources---disable-https"></a>Kaynakları temizleme - HTTPS’yi devre dışı bırakma

Önceki adımlarda özel etki alanınızda HTTPS protokolünü etkinleştirdiniz. Artık HTTPS ile özel etki alanınızı kullanmak istiyorsanız, aşağıdaki adımları uygulayarak HTTPS’yi devre dışı bırakabilirsiniz:

### <a name="disable-the-https-feature"></a>HTTPS özelliğini devre dışı bırakma 

1. [Azure portalında](https://portal.azure.com) **Verizon'dan Azure CDN Standart** veya **Verizon'dan Azure CDN Premium** CDN profiline gidin.

2. Uç noktalar listesinde, özel etki alanınızı içeren uç noktaya tıklayın.

3. HTTPS’yi devre dışı bırakmak istediğiniz özel etki alanına tıklayın.

    ![Özel etki alanları listesi](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

4. HTTPS’yi devre dışı bırakmak için **Kapalı**’ya ve sonra **Uygula**’ya tıklayın.

    ![Özel HTTPS iletişim kutusu](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="wait-for-propagation"></a>Yayılma için bekleme

Özel etki alanı HTTPS özelliği devre dışı bırakıldıktan sonra bu işlemin geçerli olması 6-8 saate kadar sürebilir. İşlem tamamlandığında, Azure portalındaki özel HTTPS durumu **Devre Dışı** olarak ayarlanır ve özel etki alanı iletişim kutusundaki üç işlem adımı tamamlandı olarak işaretlenir. Özel etki alanınız artık HTTPS’yi kullanamaz.

![HTTPS iletişim kutusunu devre dışı bırakma](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

#### <a name="operation-progress"></a>İşlem ilerleme durumu

Aşağıdaki tabloda, HTTPS’yi devre dışı bıraktığınızda oluşan işlem ilerleme durumunu gösterir. HTTPS’yi devre dışı bırakmanızın ardından özel etki alanı iletişim kutusunda üç işlem adımı görünür. Her adım etkin hale gelir ve adımın altında ek ayrıntılar görüntülenir. Bir adım başarıyla tamamlandıktan sonra adımın yanında yeşil bir onay işareti görünür. 

| İşlem ilerleme durumu | İşlem ayrıntıları | 
| --- | --- |
| 1 İstek gönderiliyor | İsteğiniz gönderiliyor |
| 2 Sertifika sağlaması kaldırılıyor | Sertifika siliniyor |
| 3 Tamamlandı | Sertifika silindi |

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

1. *Sertifika sağlayıcısı kimdir ve ne tür bir sertifika kullanılır?*

    Microsoft, DigiCert tarafından sağlanan bir Konu Diğer Adları (SAN) sertifikasını kullanır. Bir SAN sertifikası, tek sertifika ile birden fazla tam etki alanı adının güvenliğini sağlayabilir.

2. *Ayrılmış sertifikamı kullanabilir miyim?*
    
    Şu anda değil, ancak bu, yol haritasında planlanmaktadır.

3. *DigiCert’ten etki alanı doğrulama e-postası almazsam ne olur?*

    Özel etki alanınız için doğrudan uç nokta ana bilgisayar adına işaret eden bir CNAME girişiniz varsa (ve cdnverify alt etki alanı adını kullanmıyorsanız) etki alanı doğrulama e-postası almazsınız. Doğrulama otomatik olarak gerçekleşir. Ancak CNAME girişiniz yoksa ve e-postayı 24 saat içinde almazsanız Microsoft destek ekibine başvurun.

4. *Ayrılmış sertifika kullanmak, SAN sertifikasından daha mı güvenlidir?*
    
    SAN sertifikası, ayrılmış sertifika ile aynı şifreleme ve güvenlik standartlarını uygular. Verilen tüm SSL sertifikaları, gelişmiş sunucu güvenliği için SHA-256 standardını kullanır.

5. *Akamai'den Azure CDN ile özel etki alanı HTTPS’si kullanabilir miyim?*

    Şu anda bu özellik yalnızca Verizon'dan Azure CDN ile kullanılabilir. Microsoft, önümüzdeki aylarda Akamai'den Azure CDN ile bu özelliği destekleme üzerinde çalışmaktadır.

6. *DNS sağlayıcım ile Sertifika Yetkilisi Yetkilendirme kaydı kullanmam gerekir mi?*

    Hayır, Sertifika Yetkilisi Yetkilendirme kaydı şu anda gerekli değildir. Ancak, varsa, geçerli CA olarak DigiCert’i içermelidir.


## <a name="next-steps"></a>Sonraki adımlar

Öğrendikleriniz:

> [!div class="checklist"]
> - Özel etki alanınızda HTTPS protokolünü etkinleştirme
> - Etki alanını doğrulama
> - Özel etki alanınızda HTTPS protokolünü devre dışı bırakma

CDN uç noktanızda önbelleğe almayı yapılandırma hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure CDN önbelleğe alma davranışını önbelleğe alma kurallarıyla denetleme](cdn-caching-rules.md)

