---
title: Azure portalını kullanarak OpenAPI belirtimini içeri aktarma | Microsoft Docs
description: API Management ile OpenAPI belirtimini içeri aktarmayı öğrenin.
services: api-management
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: 0ea24b58deb7b33755a44737a66938883f74a13c
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="import-an-openapi-specification"></a>OpenAPI belirtimini içeri aktarma

Bu makalede, http://conferenceapi.azurewebsites.net?format=json konumunda bulunan bir "OpenAPI belirtimi" arka uç API’sinin nasıl içeri aktarılacağı gösterilmektedir. Bu arka uç API’si, Microsoft tarafından sağlanır ve Azure’da barındırılır. Makale, APIM API’sinin nasıl test edileceğini de göstermektedir.

> [!IMPORTANT]
> OpenAPI içeri aktarma ile ilgili önemli bilgiler ve ipuçları için bu [belgeye](https://blogs.msdn.microsoft.com/apimanagement/2018/03/28/important-changes-to-openapi-import-and-export/) bakın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * "OpenAPI belirtimi" arka uç API’sini içeri aktarma
> * Azure portalında API’yi test etme
> * Geliştirici portalında API’yi test etme

## <a name="prerequisites"></a>Ön koşullar

Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Arka uç API’sini içeri aktarma ve yayımlama

1. **API YÖNETİMİ** bölümünden **API’ler** öğesini seçin.
2. **Yeni API ekle** listesinden **OpenAPI belirtimi**’ni seçin.
    ![OpenAPI belirtimi](./media/import-api-from-oas/oas-api.png)
3. Uygun ayarları girin. Oluşturma sırasında tüm API değerlerini ayarlayabilirsiniz. Alternatif olarak, **Ayarlar** sekmesine giderek bunların bazılarını daha sonra ayarlayabilirsiniz. <br/> **Sekme** tuşuna basarsanız, alanların bazıları (veya tümü), belirtilen arka uç hizmetindeki bilgilerle doldurulur.

    ![Bir API oluşturma](./media/api-management-get-started/create-api.png)

    |Ayar|Değer|Açıklama|
    |---|---|---|
    |**OpenAPI Belirtimi**|http://conferenceapi.azurewebsites.net?format=json|API’yi uygulayan hizmete başvurur. API Management istekleri bu adrese iletir.|
    |**Görünen ad**|*Tanıtım Konferansı API’si*|Hizmet URL’sini girdikten sonra sekme tuşuna basarsanız APIM, json'da ne olduğuna bağlı olarak bu alanı doldurur. <br/>Bu ad, Geliştirici portalında görüntülenir.|
    |**Ad**|*demo-conference-api*|API için benzersiz bir ad sağlar. <br/>Hizmet URL’sini girdikten sonra sekme tuşuna basarsanız APIM, json'da ne olduğuna bağlı olarak bu alanı doldurur.|
    |**Açıklama**|API için isteğe bağlı bir açıklama sağlayın.|Hizmet URL’sini girdikten sonra sekme tuşuna basarsanız APIM, json'da ne olduğuna bağlı olarak bu alanı doldurur.|
    |**API URL’si soneki**|*conference*|Sonek, API yönetimi hizmetinin temel URL’sinin sonuna eklenir. API Management API'leri soneklerine bakarak ayırt eder; dolayısıyla belirli bir yayımcıdaki her API için sonekin benzersiz olması gerekir.|
    |**URL düzeni**|*HTTPS*|API’ye erişmek için hangi protokollerin kullanılabileceğini belirler. |
    |**Ürünler**|*Sınırsız*| API’yi bir ürünle ilişkilendirerek yayımlayın. İsteğe bağlı olarak bu yeni API’yi bir ürüne ekleyin, ürün adını yazın. Bu adım, API’yi birden fazla ürüne eklemek için birçok defa yinelenebilir.<br/>Ürünler bir veya daha fazla API arasındaki ilişkilendirmelerdir. Bir dizi API ekleyebilir ve geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Geliştiricilerin bir API’ye erişebilmesi için önce ürüne abone olması gerekir. Abone olduklarında, ilgili üründeki tüm API’ler için geçerli olan bir abonelik anahtarı edinirler. APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve varsayılan olarak tüm ürünlere abone olmuşsunuz demektir.<br/> Varsayılan olarak, her bir API Management örneği iki örnek ürün ile birlikte gelir: **Başlangıç** ve **Sınırsız**. |

4. **Oluştur**’u seçin.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Azure portalında yeni APIM API’sini test etme

İşlemler doğrudan bir API’nin işlemlerini görüntülemek ve test etmek için kullanışlı bir yol sağlayan Azure portalından çağrılabilir.

1. Önceki adımda oluşturduğunuz API’yi seçin.
2. **Test** sekmesine basın.

    ![API’yi test et](./media/api-management-get-started/test-api.png)
1. **GetSpeakers** seçeneğine tıklayın.

    Sayfa, sorgu parametreleri için alanları görüntüler, ancak bu durumda bir alan yok. Sayfa, üst bilgiler için de alanları görüntüler. Bu API ile ilişkilendirilmiş ürünün abonelik anahtarı için, üst bilgilerden biri "Ocp-Apim-Subscription-Key" üst bilgisidir. APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve anahtar otomatik olarak doldurulur.
4. **Gönder**’e basın.

    Arka uç, **200 OK** ve bazı verilerle yanıt verir.

## <a name="call-operation"> </a>Geliştirici portalından işlem çağırma

API’leri test etmek için **Geliştirici portalından** da işlemler çağrılabilir.

1. "Arka uç API’sini içeri aktarma ve yayımlama" adımında oluşturduğunuz API’yi seçin.
2. **Geliştirici portalı** düğmesine basın.

    ![Geliştirici portalında test etme](./media/api-management-get-started/developer-portal.png)

    "Geliştirici portalı" sitesi açılır.
3. **API**’yi seçin.
4. **Tanıtım Konferansı API’si** seçeneğini belirleyin.
5. **GetSpeakers**’a tıklayın.

    Sayfa, sorgu parametreleri için alanları görüntüler, ancak bu durumda bir alan yok. Sayfa, üst bilgiler için de alanları görüntüler. Bu API ile ilişkilendirilmiş ürünün abonelik anahtarı için, üst bilgilerden biri "Ocp-Apim-Subscription-Key" üst bilgisidir. APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve anahtar otomatik olarak doldurulur.
6. **Deneyin**’e basın.
7. **Gönder**’e basın.

    Bir işlem çağrıldıktan sonra, geliştirici portalı **Yanıt durumu**, **Yanıt üst ilgileri** ve tüm **Yanıt içeriğini** gösterir.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yayımlanan API’yi dönüştürme ve koruma](transform-api.md)
