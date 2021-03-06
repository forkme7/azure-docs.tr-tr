---
title: 'Azure Active Directory etki alanı Hizmetleri: sorun giderme uyarıları | Microsoft Docs'
description: Uyarılar için Azure AD etki alanı Hizmetleri sorunlarını giderme
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: ergreenl
ms.openlocfilehash: 0c8fc2551f529fbff647d3400144fa2a9600bbd9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD etki alanı Hizmetleri - sorun giderme uyarıları
Bu makalede, yönetilen etki alanınızda karşılaşabileceğiniz herhangi bir uyarı için sorun giderme kılavuzları sağlar.


Karşılık gelen sorun giderme adımlarını kimliği veya ileti uyarıda seçin.

| **Uyarı Kimliği** | **Uyarı iletisi** | **Çözümleme** |
| --- | --- | :--- |
| AADDS001 | *Güvenli LDAP internet üzerinden yönetilen etki alanı için etkinleştirilir. Ancak, bağlantı noktası 636 erişimi bir ağ güvenlik grubu kullanılarak aşağı kilitli değil. Bu parola kaba kuvvet saldırıları yönetilen etki alanına kullanıcı hesaplarında getirebilir.* | [Güvenli LDAP yapılandırması hatalı](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *Yönetilen etki alanı ile ilişkili Azure AD dizini silinmiş olabilir. Yönetilen etki alanı artık desteklenen bir yapılandırma değildir. Microsoft edemez izlemek, yönetmek, düzeltme eki ve yönetilen etki alanınızı eşitlemek.* | [Eksik dizin](#aadds100-missing-directory) |
| AADDS101 | *Azure AD etki alanı Hizmetleri Azure AD B2C dizini etkinleştirilemez.* | [Azure AD B2C bu dizinde çalışıyor](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Azure AD dizininizi Azure AD etki alanı Hizmetleri düzgün çalışması gerekli bir hizmet sorumlusu silinmiş olabilir. Bu yapılandırma, izleme, yönetme, düzeltme eki, Microsoft'un yeteneğini etkiler ve yönetilen etki alanınızı eşitleyin.* | [Hizmet sorumlusu eksik](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz sanal ağı için IP adres aralığı bir ortak IP aralığında değil. Azure AD etki alanı Hizmetleri, bir sanal ağdaki bir özel IP adres aralığı ile etkinleştirilmelidir. Bu yapılandırma, izleme, yönetme, düzeltme eki Microsoft'un yeteneğini etkiler ve yönetilen etki alanınızı eşitleyin.* | [Bir ortak IP aralığında adresidir](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft, bu yönetilen etki alanı için etki alanı denetleyicilerinin ulaşmasını alamıyor. Bu sanal ağ blokları erişiminizi yönetilen etki alanı için yapılandırılmış bir ağ güvenlik grubu (NSG) durum. Başka bir olası neden, bu blokları gelen trafiğin internet'ten bir kullanıcı tanımlı yol olup olmadığını olmasıdır.* | [Ağ hatası](active-directory-ds-troubleshoot-nsg.md) |
| AADDS105 | *Uygulama kimliği ile "d87dcbc6-a371-462e-88e3-28ad15ec4e64" hizmet asıl silinir ve yeniden oluşturulur. Dinlenme tutarsız izinleri yönetilen etki alanınızı hizmet vermek için gereken Azure AD etki alanı Hizmetleri kaynakları bırakır. Eşitleme, yönetilen etki alanınızda parolaların etkilenebilir.* | [Parola Eşitleme uygulama güncel değil](active-directory-ds-troubleshoot-service-principals.md#alert-aadds105-password-synchronization-application-is-out-of-date) |
| AADDS500 | *Yönetilen etki alanı son [date] üzerinde Azure AD ile eşitlendi. Kullanıcılar yönetilen etki alanında oturum açmak olabilir veya grup üyeliklerini Azure AD ile eşitlenmiş olmayabilir.* | [Eşitleme bir süre içinde gerçekleşen tamamlanmadı](#aadds500-synchronization-has-not-completed-in-a-while) |
| AADDS501 | *Yönetilen etki alanı son [date] üzerinde yedeklendi.* | [Bir yedek bir süre gerçekleştirilecek tamamlanmadı](#aadds501-a-backup-has-not-been-taken-in-a-while) |
| AADDS502 | *Yönetilen etki alanı için güvenli LDAP sertifika XX sona erecek.* | [Güvenli LDAP sertifikanın sona ermesinden](active-directory-ds-troubleshoot-ldaps.md#aadds502-secure-ldap-certificate-expiring) |
| AADDS503 | *Yönetilen etki alanı, etki alanı ile ilişkili Azure abonelik etkin olmadığı için askıya alınır.* | [Devre dışı bırakılmış abonelik nedeniyle askıya alma](#aadds503-suspension-due-to-disabled-subscription) |
| AADDS504 | *Yönetilen etki alanı geçersiz bir yapılandırma nedeniyle askıya alınır. Hizmet, düzeltme eki, yönetme veya etki alanı denetleyicileri, yönetilen etki alanınız için uzun bir süredir güncelleştirmek kuramamış.* | [Geçersiz yapılandırma nedeniyle askıya alma](#aadds504-suspension-due-to-an-invalid-configuration) |


## <a name="aadds100-missing-directory"></a>AADDS100: Dizin eksik
**Uyarı iletisi:**

*Yönetilen etki alanı ile ilişkili Azure AD dizini silinmiş olabilir. Yönetilen etki alanı artık desteklenen bir yapılandırma değildir. Microsoft edemez izlemek, yönetmek, düzeltme eki ve yönetilen etki alanınızı eşitlemek.*

**Çözüm:**

Bu hata, genellikle Azure aboneliğiniz için yeni bir Azure yanlış taşıyarak kaynaklanır AD dizini ve eski silme hala Azure AD etki alanı Hizmetleri ile ilişkili Azure AD dizini.

Bu hata kurtarılamaz. Gidermek için şunları yapmanız gerekir [mevcut yönetilen etki alanınızı silme](active-directory-ds-disable-aadds.md) ve yeni dizininizde yeniden oluşturun. Silme konusunda sorun yaşıyorsanız, Azure Active Directory etki alanı Hizmetleri ürün ekibine başvurun [desteği için](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Bu dizinde Azure AD B2C çalışıyor
**Uyarı iletisi:**

*Azure AD etki alanı Hizmetleri Azure AD B2C dizini etkinleştirilemez.*

**Çözüm:**

>[!NOTE]
>Azure AD etki alanı Hizmetleri kullanmaya devam edebilmek için Azure AD etki alanı Hizmetleri örneğinizi Azure AD B2C dizini yeniden oluşturmanız gerekir.

Hizmetinizi geri yüklemek için aşağıdaki adımları izleyin:

1. [Yönetilen etki alanınızı silme](active-directory-ds-disable-aadds.md) mevcut Azure AD dizininizden.
2. Azure AD B2C dizini olmayan yeni bir dizin oluşturun.
3. İzleyin [Başlarken](active-directory-ds-getting-started.md) Kılavuzu yönetilen bir etki alanına yeniden oluşturun.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Bir ortak IP aralığında adresidir

**Uyarı iletisi:**

*Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz sanal ağı için IP adres aralığı bir ortak IP aralığında değil. Azure AD etki alanı Hizmetleri, bir sanal ağdaki bir özel IP adres aralığı ile etkinleştirilmelidir. Bu yapılandırma, izleme, yönetme, düzeltme eki Microsoft'un yeteneğini etkiler ve yönetilen etki alanınızı eşitleyin.*

**Çözüm:**

> [!NOTE]
> Bu sorunu gidermek için var olan yönetilen etki alanınızı silin ve bir sanal ağdaki bir özel IP adres aralığı ile yeniden oluşturun. Bu kesintiye uğratan işlemidir.

Başlamadan önce okuyun **özel IP v4 adresi alanı** bölümüne [bu makalede](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

Sanal ağ içinde makineler istekleri aynı IP adresi aralığında bu alt ağ için yapılandırılmış olarak Azure kaynaklarına kalmasına neden olabilir. Ancak, sanal ağ bu aralık için yapılandırıldıktan sonra bu istekleri sanal ağ içindeki yönlendirilir ve hedeflenen web kaynakları ulaşmaz. Bu yapılandırma, Azure AD etki alanı Hizmetleri ile beklenmeyen hatalara yol açabilir.

**Sanal ağınızda yapılandırılmış internet IP adresi aralığında sahipseniz, bu uyarıyı göz ardı edilebilir. Ancak, Azure AD etki alanı Hizmetleri için gönderilemiyor [SLA](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)] beklenmeyen hatalara yol açabilir beri bu yapılandırmaya sahip.**


1. [Yönetilen etki alanınızı silme](active-directory-ds-disable-aadds.md) dizininizden.
2. Alt ağ için IP adresi aralığı Düzelt
  1. Gidin [sanal ağlar Azure portal sayfasında](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Azure AD etki alanı Hizmetleri için kullanmayı planladığınız sanal ağı seçin.
  3. Tıklayın **adres alanı** ayarlar altında
  4. Adres aralığı var olan adres aralığına tıklatarak ve düzenlemeyi veya ek adres aralığı ekleme güncelleştirin. Yeni adres aralığı bir özel IP aralığında olduğundan emin olun. Yaptığınız değişiklikleri kaydedin.
  5. Tıklayın **alt ağlar** sol gezinti içinde.
  6. Tabloda düzenlemek istediğiniz alt ağ'ı tıklatın.
  7. Adres aralığı güncelleştirin ve değişikliklerinizi kaydedin.
3. İzleyin [alma başlatıldı kullanarak Azure AD etki alanı Hizmetleri Kılavuzu](active-directory-ds-getting-started.md) yönetilen etki alanınızı yeniden oluşturmak için. Bir sanal ağ bir özel IP adres aralığı ile çekme emin olun.
4. Etki alanına katılma için yeni etki alanı, sanal makinelerinizin izleyin [bu kılavuzda](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Uyarı çözümlendiğinde emin olmak için iki saat içinde etki alanınızın sistem durumunu denetleyin.

## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: Eşitleme bir süre içinde tamamlanmadı.

**Uyarı iletisi:**

*Yönetilen etki alanı son [date] üzerinde Azure AD ile eşitlendi. Kullanıcılar yönetilen etki alanında oturum açmak olabilir veya grup üyeliklerini Azure AD ile eşitlenmiş olmayabilir.*

**Çözüm:**

[Etki alanınızın sistem durumu denetimi](active-directory-ds-check-health.md) yönetilen etki alanınızın yapılandırma sorunlarını belirtebilecek herhangi bir uyarı için. Bazı durumlarda, sorunları yapılandırmanızla Microsoft'un, yönetilen etki alanı eşitleme yeteneği engelleyebilirsiniz. Tüm uyarıları çözümleyin, bekleyin tamamlayabilirseniz eşitleme tamamlanıp tamamlanmadığını görmek için iki saat ve onay yedekleyin.


## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: Bir yedek bir süre kullanımda değil

**Uyarı iletisi:**

*Yönetilen etki alanı son [date] üzerinde yedeklendi.*

**Çözüm:**

[Etki alanınızın sistem durumu denetimi](active-directory-ds-check-health.md) yönetilen etki alanınızın yapılandırma sorunlarını belirtebilecek herhangi bir uyarı için. Bazı durumlarda, sorunları yapılandırmanızla Microsoft'un, yönetilen etki alanı eşitleme yeteneği engelleyebilirsiniz. Tüm uyarıları çözümleyin, bekleyin tamamlayabilirseniz eşitleme tamamlanıp tamamlanmadığını görmek için iki saat ve onay yedekleyin.


## <a name="aadds503-suspension-due-to-disabled-subscription"></a>AADDS503: Devre dışı bırakılmış abonelik nedeniyle askıya alma

**Uyarı iletisi:**

*Yönetilen etki alanı, etki alanı ile ilişkili Azure abonelik etkin olmadığı için askıya alınır.*

**Çözüm:**

Hizmetinize geri yüklemek için [Azure aboneliğinizi yenilemek](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable) yönetilen etki alanı ile ilişkilendirilmiş.

## <a name="aadds504-suspension-due-to-an-invalid-configuration"></a>AADDS504: Geçersiz yapılandırma nedeniyle askıya alma

**Uyarı iletisi:**

*Yönetilen etki alanı geçersiz bir yapılandırma nedeniyle askıya alınır. Hizmet, düzeltme eki, yönetme veya etki alanı denetleyicileri, yönetilen etki alanınız için uzun bir süredir güncelleştirmek kuramamış.*

**Çözüm:**

[Etki alanınızın sistem durumu denetimi](active-directory-ds-check-health.md) yönetilen etki alanınızın yapılandırma sorunlarını belirtebilecek herhangi bir uyarı için. Bu uyarıların hiçbirini çözebilmek için bunu yapar. Sonra aboneliğinizi yeniden etkinleştirmek için desteğe başvurun.

## <a name="contact-us"></a>Bizimle iletişim kurun
Azure Active Directory etki alanı Hizmetleri ürün ekibine başvurun [paylaşmak geri bildirim veya destek](active-directory-ds-contact-us.md).
