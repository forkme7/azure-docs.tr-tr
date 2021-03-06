---
title: 'Azure Active Directory B2C: Özel öznitelikleri | Microsoft Docs'
description: Tüketicileriniz hakkında bilgi toplamak için Azure Active Directory B2C'de özel öznitelikleri kullanma
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.openlocfilehash: 6f285c10b7d8ff92c8568c42b6a78dc4ea9bcc74
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: tüketicileriniz hakkında bilgi toplamak için özel öznitelikler kullanın.
Azure Active Directory (Azure AD) B2C dizininize bilgi (öznitelikler) yerleşik bir dizi birlikte gelir: verilen ad, Soyadı, şehir, posta kodu ve diğer öznitelikleri. Bununla birlikte, her tüketiciye yönelik uygulama tüketici toplamak için benzersiz gereksinimleri ne öznitelikleri üzerinde vardır. Azure AD B2C ile her tüketici hesapta depolanan öznitelikler kümesi genişletebilirsiniz. Özel öznitelikler oluşturabileceğiniz [Azure portal](https://portal.azure.com/) ve aşağıda gösterildiği gibi kaydolma ilkelerinizi kullanın. Aynı zamanda okuma ve yazma bu öznitelikleri kullanarak [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [!NOTE]
> Özel öznitelikler kullanın [Azure AD Graph API Directory şema uzantıları](https://msdn.microsoft.com/library/azure/dn720459.aspx).
> 
> 

## <a name="create-a-custom-attribute"></a>Özel bir öznitelik oluşturma
1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Tıklatın **kullanıcı öznitelikleri**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Sağlayan bir **adı** özel öznitelik (örneğin, "ShoeSize") ve isteğe bağlı olarak bir **açıklama**. **Oluştur**’a tıklayın.
   
   > [!NOTE]
   > Yalnızca "Dize", "Mantıksal" ve "Int" **veri türleri** şu anda kullanılabilir.
   > 
   > 

Özel öznitelik listesinde kullanılabilir **kullanıcı öznitelikleri**ve kaydolma ilkelerinizi kullanmak için.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Kaydolma ilkenizde özel bir öznitelik kullanın
1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. **Kaydolma ilkeleri**’ne tıklayın.
3. Açmak için kayıt ilkesi (örneğin, "B2C_1_SiUp") tıklayın. Tıklatın **Düzenle** dikey pencerenin üstündeki.
4. Tıklatın **kaydolma özniteliklerini** ve özel öznitelik (örneğin, "ShoeSize") seçin. **Tamam**’a tıklayın.
5. Tıklatın **uygulama talepleri** ve özel özniteliği seçin. **Tamam**’a tıklayın.
6. Tıklatın **kaydetmek** dikey pencerenin üstündeki.

Müşteri Deneyimi doğrulamak için ilkeyi "Şimdi Çalıştır" özelliğini kullanabilirsiniz. Şimdi "ShoeSize" tüketici kaydolma sırasında toplanan özniteliklerinin listesini görmek ve uygulamanıza geri gönderilen belirteçte bkz gerekir.

## <a name="notes"></a>Notlar
* Kayıt ilkeleri ile birlikte özel öznitelikler de oturum açma veya kaydolma ilkeleri ve profil düzenleme ilkeleri kullanılabilir.
* Özel özniteliklerin bilinen bir sınırlama yoktur. Yalnızca ilk defa herhangi bir ilkede kullanıldığında oluşturulan ve listesine değil eklendiğinde olan **kullanıcı öznitelikleri**.

