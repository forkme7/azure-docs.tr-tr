---
title: Lisans Self Servis parola sıfırlama - Azure Active Directory
description: Lisans gereksinimlerini Azure AD Self Servis parola sıfırlama
services: active-directory
keywords: ''
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: cebfc1dcaca562d26eb9e85d9b8bf0fa14e5edc9
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Lisans gereksinimleri için Azure AD Self Servis parola sıfırlama

Azure Active Directory (Azure AD) parola sıfırlama işlevi için sırayla, *kuruluşunuzda atanan en az bir Lisansı olmalıdır*. Kullanıcı başına parola sıfırlama deneyimi lisans biz zorlamaz. Microsoft lisans sözleşmenize ile uyumluluğu korumak için premium özellikleri kullanan kullanıcılara lisans atamanız gerekir.

* **Yalnızca bulut kullanıcıları**: Office 365 herhangi SKU veya Azure AD temel Ücretli
* **Bulut** veya **şirket içi kullanıcıların**: Azure AD Premium P1 veya P2, Enterprise Mobility + Security (EMS) veya Microsoft 365

## <a name="licenses-required-for-password-writeback"></a>Parola geri yazma için gerekli lisansları

Parola geri yazma özelliğini kullanmak için aşağıdaki lisansları Kiracı'atanmış olması gerekir:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Microsoft 365 (planı E3)
* Microsoft 365 (planı E5)

> [!WARNING]
> Tek başına Office 365 planları lisans *parola geri yazma desteklemeyen* ve bu işlevlerin çalışması önceki planlardan sahip olması gerekir.
>

Maliyetleri de dahil olmak üzere ek lisans bilgileri şu sayfalarda bulunabilir:

* [Site fiyatlandırma Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory özellikleri ve yetenekleri](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Kurumsal](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Grup veya kullanıcı tabanlı lisans etkinleştirme

Artık Azure AD, Grup tabanlı lisans destekler. Yöneticiler, bunları birer birer atama yerine kullanıcıların bir gruba toplu lisansları atayabilirsiniz. Daha fazla bilgi için bkz: [atayabilir, doğrulayın ve lisans sorunları gidermek](../active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses).

Bazı Microsoft Hizmetleri tüm konumlarda kullanılabilir değil. Yönetici bir kullanıcıya bir lisans atanabilmesi için önce belirtmelidir **kullanım konumu** kullanıcı özelliği. Lisans atama altında yapılabilir **kullanıcı** > **profil** > **ayarları** Azure portalı bölümünde. *Grup lisans atamasını kullandığınızda, belirtilen bir kullanım konumu olmayan tüm kullanıcılar dizininin konumunu devralır.*

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../active-directory-passwords-reset-register.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)
