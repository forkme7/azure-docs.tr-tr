---
title: "Güvenlik uyarılarının nasıl yapılandırılacağı | Microsoft Docs"
description: "Azure Privileged Identity Management uzantısı güvenlik uyarılarını yapılandırmak öğrenin."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 4e0c911a-36c6-42a0-8f79-a01c03d2d04f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 8037942cb3700f8e46d3be24b5fed04004333335
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management güvenlik uyarılarını yapılandırma
## <a name="security-alerts"></a>Güvenlik uyarıları
Azure Privileged Identity Management (PIM) ortamınızda kuşkulu veya güvenli olmayan etkinliği olduğunda uyarı verir. Bir uyarı tetiklendiğinde, PIM panosunda görüntülenir. Kullanıcıların veya uyarının rollerin listeleyen bir rapor görmek için uyarıyı seçin.

![PIM Pano güvenlik uyarıları - ekran görüntüsü][1]

| Uyarı | Önem Derecesi | Tetikleyici | Öneri |
| --- | --- | --- | --- |
| **Roller PIM dışında atanmış durumda** |Yüksek |Bir kullanıcının PIM arabirimi dışında ayrıcalıklı bir rol kalıcı olarak atanmıştır. |Liste ve atamayı onlardan PIM dışında atanan rollerin ayrıcalıklı kullanıcılar gözden geçirin. |
| **Roller çok sık etkinleştirilmekte** |Orta |Ayarlarında izin verilen süre içinde çok fazla yeniden etkinleştirmeleri aynı rolünün vardı. |Bunlar çok fazla kez rolü neden etkinleştirdikten görmek için kullanıcıyla iletişime geçin. Belki de zaman sınırı için çok fazla bunları görevlerini tamamlamak için veya belki de bunlar komut dosyaları otomatik olarak bir rolü etkinleştirmek için kullanmakta olduğunuz yoktur. Etkinleştirme süresi rolleri için bunları görevlerini gerçekleştirmeye yetecek uzunlukta ayarlandığından emin olun. |
| **Rol etkinleştirmesi için çok faktörlü kimlik doğrulaması gerektirmeyen** |Orta |MFA ayarları etkinse olmadan rolü vardır. |Biz en üst düzey ayrıcalıklı rolleri için MFA gerekir, ancak tüm rolleri etkinleştirme için MFA etkinleştirin kesinlikle öneririz. |
| **Ayrıcalıklı rollerini kullanıcıların kullanmadığınız** |Düşük |Rollerine son etkinleştirmediyseniz uygun Yöneticiler vardır. |Erişim artık gerekmeyen kullanıcılar belirlemek için bir erişim gözden geçirme başlatın. |
| **Çok sayıda genel Yöneticiler vardır** |Düşük |Önerilen daha fazla genel Yöneticiler vardır. |Çok sayıda genel Yöneticiler varsa, kullanıcıların ihtiyaç duyduklarından fazla izinler aldıklarından olasıdır. Daha az ayrıcalıklı rollere kullanıcıları taşıyın ya da bunlardan bazıları kalıcı olarak atanan yerine rolü için uygun hale getirmek. |

### <a name="severity"></a>Önem Derecesi
* **Yüksek**: bir ilke ihlali nedeniyle Acil eylem gerektirir. 
* **Orta**: Acil eylem gerekli değildir ancak olası ilke ihlalinin işaret eder.
* **Düşük**: Acil eylem gerekli değildir ancak preferrable ilke değişikliğini önerir.

## <a name="configure-security-alert-settings"></a>Güvenlik uyarı ayarlarını yapılandırma
Güvenlik Uyarıları ortamınız ve Güvenlik amaçları ile çalışmak için PIM bazıları özelleştirebilirsiniz. Ayarlar dikey ulaşmak için şu adımları izleyin:

1. Oturum [Azure portal](https://portal.azure.com/) seçip **Azure AD Privileged Identity Management** kutucuğu panodan.
2. Seçin **yönetilen ayrıcalıklı rolleri** > **ayarları** > **uyarıları ayarları**.
   
    ![Güvenlik Uyarıları ayarlarına gidin][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>"Rolleri çok sık etkinleştirilmekte" Uyarısı
Bir kullanıcı birden çok kez belirli bir süre içinde aynı ayrıcalıklı rolün etkinleştirir, bu uyarı tetikler. Zaman aralığını ve etkinleştirme sayısını yapılandırabilirsiniz.

* **Etkinleştirme yenileme zaman çerçevesi**: gün, saat, dakika belirtin ve ikinci şüpheli yenilemeleri izlemek için kullanmak istediğiniz süre.
* **Etkinleştirme yenilemesi sayısı**: 2 seçtiğiniz zaman çerçevesi içinde uyarı worthy dikkate 100'den etkinleştirme sayısını belirtin. Bu, kaydırıcıyı hareket veya bir sayı metin kutusuna yazarak ayarı değiştirebilirsiniz.

### <a name="there-are-too-many-global-administrators-alert"></a>"Çok sayıda genel Yöneticiler bulunur" Uyarısı
PIM bu uyarı, iki farklı ölçütler karşılanıyorsa ve bunların her ikisi de yapılandırabilirsiniz tetikler. İlk olarak, genel yöneticilerin belirli bir eşiğe ulaşmasına gerekir. İkinci olarak, belirli bir yüzdesi toplam rol atamalarınızı genel yönetici olması gerekir. Yalnızca bu ölçümleri birini karşılamıyorsa, uyarıyı görünmez.  

* **Genel yöneticiler en az sayıda**: 2 güvenli olmayan bir miktarını göz önünde bulundurun 100'den genel Yöneticiler sayısını belirtin.
* **Genel yönetici oranı**: Genel Yöneticiler yöneticilerine yüzdesini belirtin, % %100 0, yani ortamınızda güvenli.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>"Yöneticiler ayrıcalıklı rollerini kullanmadığınız" Uyarısı
Bir kullanıcı bir rolünü etkinleştirmeden belirli bir süre kalırsa bu uyarı tetikler.

* **Gün sayısı**: 0-bir kullanıcı bir rolünü etkinleştirmeden gidebilirsiniz 100, gün sayısını belirtin.

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
