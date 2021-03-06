---
title: Openıd Connect kimlik sağlayıcıları yerleşik ilkeleri - Azure AD B2C ekleme | Microsoft Docs
description: Azure AD B2C içinde yerleşik ilkelerinde Openıd Connect sağlayıcıları ekleme hakkında genel bakış Kılavuzu
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: parakhj
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/23/2018
ms.author: davidmu
ms.openlocfilehash: 52cefd4fad96e94dd2cf0871fac12a9a4d74b96f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-active-directory-b2c-add-a-custom-openid-connect-identity-provider-in-built-in-policies"></a>Azure Active Directory B2C: özel bir Openıd Connect kimlik sağlayıcısı yerleşik ilkelerinde ekleyin.

[Openıd Connect](http://openid.net/specs/openid-connect-core-1_0.html) güvenli bir şekilde kullanıcıların oturum açmak için kullanılan OAuth 2.0 üstünde yerleşik bir kimlik doğrulama protokolüdür. Azure AD gibi bu protokolü kullanan çoğu kimlik sağlayıcıları Azure AD B2C'de desteklenir. Bu makalede, özel Openıd Connect kimlik sağlayıcıları yerleşik ilkelerinizin nasıl ekleyebileceğiniz açıklanmaktadır.

## <a name="configuring-a-custom-openid-connect-identity-provider"></a>Özel bir Openıd Connect kimlik sağlayıcı yapılandırma

Özel bir Openıd Connect kimlik sağlayıcısı eklemek için:

1. Aşağıdaki adımları izleyin [Azure AD B2C ayarlarına gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
1. Tıklayın **kimlik sağlayıcıları**.
1. **Ekle**'ye tıklayın.
1. İçin **kimlik sağlayıcısı türü**seçin **Openıd Connect**.

### <a name="setting-up-the-openid-connect-identity-provider"></a>Openıd Connect kimlik sağlayıcısı ayarlama

#### <a name="metadata-url"></a>Meta veri URL'si

Her Openıd Connect kimlik sağlayıcıları belirtimine göre oturum açma gerçekleştirmek için gereken bilgilerin çoğunu içeren bir meta veri belgesi açıklar. Bu, kullanılacak URL'ler gibi bilgileri ve hizmetin genel İmzalama anahtarları konumunu içerir. Openıd Connect meta veri belgesi ile biten bir uç noktada her zaman bulunduğu `.well-known\openid-configuration`.

Openıd Connect kimlik sağlayıcısı için aradığınız eklemek için meta veri URL'sini girin.

#### <a name="client-id-and-secret"></a>İstemci kimliği ve parolası

Oturum açmasına olanak vermek için kimlik sağlayıcısı geliştiricilerin kendi hizmetinde bir uygulamayı kaydetme gerektirir. Bu uygulama bir Kimliğe sahip (olarak adlandırılan **istemci kimliği**) ve bir **gizli**. Kimlik sağlayıcısı'ndan bu değerleri kopyalayın ve karşılık gelen alanlara girin.

> [!NOTE]
> İstemci gizli anahtarı isteğe bağlıdır. Ancak, kullanmak istediğiniz, istemci parolasını girmeniz gerekir [yetkilendirme kodu akışını](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), belirteç kodunu değişimi için gizli anahtarı kullanır.

#### <a name="scope"></a>Kapsam

Kapsamları toplamak için özel kimlik sağlayıcınızdan aradığınız izinleri ve bilgi tanımlayın. Openıd Connect istekleri içermelidir `openid` kimlik sağlayıcısı'ndan kimliği belirteci almak için değer kapsam. Kimliği belirteci kullanıcılar Azure AD B2C oturum mümkün olmayacaktır özel kimlik sağlayıcısı kullanarak.

Diğer kapsamları (boşlukla ayrılmış olarak) eklenmiş. Hangi diğer kapsamları olabilir görmek için özel kimlik sağlayıcısının belgelerine başvurun kullanılabilir.

#### <a name="response-type"></a>Yanıt türü

Hangi bilgilerin iletildiği yapılan ilk çağrıda gönderilecek yanıt türü açıklanır `authorization_endpoint` özel kimlik sağlayıcısı. 

* `code`: Göre [yetkilendirme kodu akışını](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), bir kod Azure AD B2C geri döndürülür. Azure AD B2C çağırmak için devam sonra `token_endpoint` belirteç kodunu Exchange.
* `token`: Bir erişim belirteci geri Azure AD B2C için özel kimlik sağlayıcısı'ndan döndürülür.
* `id_token`: Bir kimliği belirteci geri Azure AD B2C için özel kimlik sağlayıcısı'ndan döndürülür.


#### <a name="response-mode"></a>Yanıt modu

Yanıt modu verileri Azure AD B2C'ye özel kimlik sağlayıcısı'ndan geri göndermek için kullanılması gereken yöntemi tanımlar.

* `form_post`: Bu yanıt modu için en iyi güvenlik önerilir. Yanıt HTTP üzerinden aktarılan `POST` kodu veya gövde kullanımında kodlanan belirteci ile yöntemi `application/x-www-form-urlencoded` biçimi.
* `query`: Kod veya belirteç sorgu parametresi olarak döndürülür.


#### <a name="domain-hint"></a>Etki alanı İpucu

Etki alanı ipucu doğrudan oturum açma kullanıcı yapma liste arasında bir seçim kullanılabilir kimlik sağlayıcıları sahip yerine belirtilen kimlik sağlayıcısı sayfasına atlamak için kullanılabilir. Bu tür bir davranış izin vermek için etki alanı ipucu bir değer girin.

Parametre için özel kimlik sağlayıcısı atlama sonuna `domain_hint=<domain hint value>` sonuna kadar Azure AD B2C oturum açmak için çağrılırken isteğiniz.


### <a name="mapping-the-claims-from-the-openid-connect-identity-provider"></a>Openıd Connect kimlik sağlayıcısından gelen talep eşleme

Sonra özel kimlik sağlayıcısı kimliği belirteci geri Azure AD B2C, Azure AD B2C alınan belirteci gelen talepleri, Azure AD B2C tanır ve kullanır talepleri eşleyebilir olması gerekiyor gönderir. 

Her eşlemeleri için kimlik sağlayıcısının belirteçleri geri döndürülen talepleri anlamak için özel kimlik sağlayıcısı belgelerine başvurun.

* `User ID`: Oturum açmış olan kullanıcı için benzersiz tanımlayıcı sağlar talep girin.
* `Display Name`: Görünen adı veya kullanıcının tam adını sağlayan talep girin.
* `Given Name`: Kullanıcının ilk adını sağlayan talep girin.
* `Surname`: Son kullanıcı adını sağlayan talep girin.
* `Email`: Kullanıcının e-posta adresi sağlayan talep girin.

## <a name="next-steps"></a>Sonraki Adımlar

Özel Openıd Connect kimlik sağlayıcısı için ekleyin, [yerleşik ilke](active-directory-b2c-reference-policies.md).
