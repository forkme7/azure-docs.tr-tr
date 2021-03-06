---
title: "Çoklu oturum açma uygulama proxy'si uygulamaya yapılandırma | Microsoft Docs"
description: "Nasıl çoklu oturum açma uygulama proxy uygulamanıza hızla yapılandırabilirsiniz"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 3fddcb9387017bfaf48199bdacd839bfc357a820
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Çoklu oturum açma uygulama proxy'si uygulamaya yapılandırma

Çoklu oturum açma (SSO) kullanıcılarınızın bir uygulama birden çok kez doğrulanmadan erişmesine izin verir. Bulutta Azure Active Directory'de karşı gerçekleşmesi tek bir kimlik doğrulaması sağlar ve hizmet veya uygulamada herhangi ek kimlik doğrulama sınaması tamamlamak için kullanıcının kimliğine bürünmek için bağlayıcı sağlar.

## <a name="how-to-configure-single-sign-on"></a>Çoklu oturum yapılandırma
SSO yapılandırmak için ilk uygulamanızı Azure Active Directory üzerinden ön kimlik doğrulama için yapılandırıldığından emin olun. Bunu yapmak için şu adrese gidin **Azure Active Directory**  - &gt; **kurumsal uygulamalar**  - &gt; **tüm uygulamaları**  - &gt; Uygulamanız  **- &gt; uygulama proxy'si**. Bu sayfada, "Ön kimlik doğrulaması" alanına bakın ve "Azure Active Directory için ayarlandığından emin olun 

Ön kimlik doğrulama yöntemleri hakkında daha fazla bilgi için bkz., dört adım [uygulama yayımlama belge](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

   ![Azure Portalı'nda ön kimlik doğrulama yöntemi](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Çoklu oturum açma modu uygulama proxy'si uygulamalar için yapılandırma
Ardından çoklu oturum açma belirli türünü yapılandırın. Oturum açma yöntemleri, arka uç uygulama kimlik doğrulama türünü kullanan üzerinde göre sınıflandırılır. Uygulama proxy'si uygulamaları üç tür oturum açma destekler:

-   **Parola tabanlı oturum açma**: parola tabanlı oturum açma için kullanılabilir oturum açma için kullanıcı adı ve parola alanlarına kullanan herhangi bir uygulama. Yapılandırma adımları bulunabilir bizim [parola SSO yapılandırma belgelerine](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#bring-your-own-password-sso-applications).

-   **Tümleşik Windows kimlik doğrulaması**: tümleşik Windows kimlik doğrulaması (IWA) kullanan uygulamalar için çoklu oturum açma Kerberos Kısıtlı temsilci (KCD) aracılığıyla etkinleştirilir. Bu, kullanıcıların kimliğine ve göndermek ve şirket adına belirteçleri almak için Active Directory'de uygulama Proxy bağlayıcıları izin verir. İçinde KCD yapılandırma hakkında ayrıntılar bulunabilir [çoklu oturum açma KCD belgelerine](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd).

-   **Üstbilgi tabanlı oturum açma**: üst bilgisi tabanlı oturum açma ortaklığı etkinleştirilir ve bazı ek yapılandırma gerektirmez. Çoklu oturum açma kimlik doğrulaması için üstbilgiler kullanan bir uygulama için yapılandırmaya yönelik adım adım yönergeler ve ortaklık hakkında daha fazla bilgi için bkz: [PingAccess Azure AD belgeleri için](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).

Bu seçeneklerin her biri "Kurumsal uygulamalar" uygulamanızda giderek ve açma bulunabilir **çoklu oturum açma** soldaki menüden sayfasında. Uygulamanız eski Portalı'nda oluşturduysanız, tüm bu seçenekler göremeyebilirsiniz unutmayın.

Bu sayfada, ayrıca bir gördüğünüz ek oturum açma seçeneği: bağlantılı oturum açma. Bu uygulama proxy'si tarafından da desteklenir. Ancak, bu seçenek çoklu oturum açma uygulamaya eklemez olduğunu unutmayın. Uygulama zaten tek Active Directory Federasyon Hizmetleri gibi başka bir hizmet kullanılarak uygulanan oturum olabilir Bununla. 

Bu seçenek, bir uygulamaya bir bağlantı, kullanıcıların ilk kara uygulama erişirken oluşturmak için bir yönetici sağlar. Active Directory Federasyon Hizmetleri 2.0 kullanan kullanıcıların kimliklerini doğrulamak için yapılandırılmış bir uygulama varsa, örneğin, bir yönetici "bağlantılı oturum açma" seçeneği erişim panelinde, bir bağlantı oluşturmak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Çoklu oturum açma uygulamalarınızı uygulama proxy'si ile sağlayın.](active-directory-application-proxy-sso-using-kcd.md)
