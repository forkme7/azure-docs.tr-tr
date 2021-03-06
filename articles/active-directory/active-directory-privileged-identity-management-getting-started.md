---
title: "Azure AD Privileged Identity Management ile çalışmaya başlama | Microsoft Belgeleri"
description: "Azure portalında Azure Active Directory Privileged Identity Management uygulaması ile ayrıcalıklı kimlikleri nasıl yöneteceğinizi öğrenin."
services: active-directory
documentationcenter: 
author: barclayn
manager: mtillman
editor: 
ms.assetid: 2299db7d-bee7-40d0-b3c6-8d628ac61071
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: 9f51daabef7d1e02917869e4e6943b8ea28b56f5
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management'ı kullanmaya başlama

Azure Active Directory (AD) Privileged Identity Management sayesinde kuruluşunuz içindeki erişimi yönetebilir, denetleyebilir ve izleyebilirsiniz. Azure kaynaklarındaki, Azure AD’deki ve Office 365 ya da Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerindeki kaynaklara erişim de bu kapsama dahildir.

Bu makalede Azure AD PIM uygulamasını Azure portalı panonuza nasıl ekleyeceğiniz anlatılmaktadır.

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme

Azure AD Privileged Identity Management'ı kullanmadan önce uygulamayı Azure portalı panonuza eklemeniz gerekir.

1. [Azure portalında](https://portal.azure.com/) dizininizin genel yöneticisi olarak oturum açın.
2. Kuruluşunuz birden fazla dizine sahipse Azure portalının sağ üst köşesinde kullanıcı adınızı seçin. PIM'i kullanmak istediğiniz dizini seçin.
3. **Tüm hizmetler** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

Dizininizde Azure AD Privileged Identity Management’ı kullanan ilk kişiyseniz, dizinde **Güvenlik yöneticisi** ve **Ayrıcalıklı rol yöneticisi** rolleri otomatik olarak size atanır. Kullanıcıların Azure AD dizini rol atamaları, yalnızca ayrıcalıklı rol yöneticileri tarafından yönetilebilir. Ayrıca [güvenlik sihirbazını](active-directory-privileged-identity-management-security-wizard.md) çalıştırmayı da seçebilirsiniz. Bu sihirbaz ilk keşif ve atama deneyiminde size yol gösterir.

## <a name="navigate-to-your-tasks"></a>Görevlerinize gitme

Azure AD Privileged Identity Management ayarlandığında, uygulamayı her açtığınızda gezinti dikey penceresini görürsünüz. Kimlik yönetimi görevlerinizi gerçekleştirmek için bu dikey pencereyi kullanın.

![PIM için üst düzey görevler - ekran görüntüsü](./media/active-directory-privileged-identity-management-getting-started/PIM_Tasks_New.png)

- **Rollerim** bölümü, size atanan uygun ve etkin rollerin bir listesini görüntüler. Burada atanan uygun rolleri etkinleştirebilirsiniz.
- **İstekleri Onayla (Önizleme)** bölümü, dizininizdeki kullanıcılar tarafından uygun Azure AD dizini rollerini etkinleştirmeniz için gönderilen ve onaylamanız için size atanan isteklerin bir listesini görüntüler. [Daha fazla bilgi edinin.](./privileged-identity-management/azure-ad-pim-approval-workflow.md)
- **Bekleyen İstekler (Önizleme)** bölümü, uygun rol atamalarını etkinleştirmek için bekleyen isteklerinizi görüntüler.
- **Erişimi Gözden Geçir** bölümü, erişimi kendiniz veya başka bir kullanıcı için gözden geçiriyor olmanız fark etmeksizin, tamamlamanız için size atanan beklemedeki erişim gözden geçirmelerini listeler.
- Sol gezinti menüsünün yönetim bölümünün altında yer alan **Azure AD dizin rolleri** bölümü, ayrıcalıklı rol yöneticileri tarafından rol atamalarını yönetmek, rol etkinleştirme ayarlarını değiştirmek, erişim gözden geçirmelerini başlatmak ve daha birçok işlevi gerçekleştirmek üzere kullanılan panoyu görüntüler. Bu pano, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır. Bu kullanıcılar Görünümüm adlı özel bir panoya erişebilir. Görünümüm panosu, tüm kiracı değil yalnızca panoya erişen kullanıcı hakkında bilgileri görüntüler.
- Sol gezinti menüsünün yönetim bölümünün altında yer alan **Azure Kaynak rolleri (Önizleme)** bölümü, rol atamalarını seçtiğiniz abonelik kaynaklarının bir listesini görüntüler 

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Privileged Identity Management'a genel bakış](active-directory-privileged-identity-management-configure.md), kuruluşunuzdaki yönetim erişimini nasıl yönetebileceğinize ilişkin daha fazla ayrıntı içerir.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
