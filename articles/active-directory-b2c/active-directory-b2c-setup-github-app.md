---
title: GitHub kimlik sağlayıcı yapılandırması - Azure AD B2C | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C tarafından güvenliği sağlanan GitHub hesaplarıyla müşterilere kaydolma ve oturum açma sağlar.
services: active-directory-b2c
documentationcenter: ''
author: davidmu
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 02/06/2017
ms.author: davidmu
ms.openlocfilehash: 17ec4ebd8406f56c431666340ca8834c5ccf9670
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-github-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma GitHub hesaplarıyla tüketicileri sağlayın

> [!NOTE]
> Bu özelliğin önizlemede değil.
> 

Bu makalede, oturum açma GitHub hesabı olan kullanıcılar için etkinleştirme gösterilmektedir.

## <a name="create-a-github-oauth-application"></a>GitHub OAuth uygulama oluşturma

Azure AD B2C, kimlik sağlayıcısı GitHub kullanmak için GitHub OAuth uygulaması oluşturma ve doğru parametrelerle sağlamanız gerekir.

1. Git [GitHub Geliştirici ayarları](https://github.com/settings/developers) GitHub oturum açtıktan sonra.
1. Tıklatın **yeni OAuth uygulama**
1. Girin bir **uygulama adı** ve **giriş sayfası URL'si**.
1. İçin **yetkilendirme geri çağırma URL'si**, girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Değiştir **{tenant}** ile Azure AD B2C kiracınızın adı (örneğin, contosob2c.onmicrosoft.com).

    >[!NOTE]
    >"Kiracı" değeri, tüm küçük harfli olması gerektiğini **oturum açma URL'si**.

1. Tıklatın **kaydetmek uygulama**.
1. Değerlerini kaydetmek **istemci kimliği** ve **gizli**. Sonraki bölümde hem de gerekir.

## <a name="configure-github-as-an-identity-provider-in-your-azure-ad-b2c-tenant"></a>Azure AD B2C kiracınızda kimlik sağlayıcısı GitHub yapılandırın

1. Aşağıdaki adımları izleyin [B2C özellikleri dikey penceresine gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalındaki.
1. B2C özellikleri dikey penceresinde **kimlik sağlayıcıları**.
1. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
1. Kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "GitHub" girin.
1. Tıklatın **kimlik sağlayıcısı türü**seçin **GitHub**, tıklatıp **Tamam**.
1. Tıklatın **bu kimlik sağlayıcısı'nı ayarlama** ve istemci Kimliğini ve daha önce kopyaladığınız GitHub OAuth uygulamanın istemci parolasını girin.
1. Tıklatın **Tamam** ve ardından **oluşturma** GitHub yapılandırmanızı kaydetmek için.

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma veya düzenleme bir [yerleşik ilke](active-directory-b2c-reference-policies.md) ve GitHub kimlik sağlayıcısı olarak ekleyin.