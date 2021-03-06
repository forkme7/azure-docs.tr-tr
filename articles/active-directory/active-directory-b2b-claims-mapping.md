---
title: B2B işbirliği kullanıcı talepleri, Azure Active Directory'de eşleme | Microsoft Docs
description: Azure Active Directory (Azure AD) B2B kullanıcılar için SAML belirtecinde verilen kullanıcı taleplerini özelleştirin.
services: active-directory
documentationcenter: ''
author: twooley
manager: mtillman
editor: ''
tags: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/06/2018
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 8f5e471d4e7102300cd5581976b45c9fa8cc57bc
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Eşleme Azure Active Directory B2B işbirliği kullanıcı talepleri

B2B işbirliği kullanıcılar için SAML belirtecinde verilen talepler özelleştiriliyor azure Active Directory (Azure AD) destekler. Kullanıcı uygulamaya doğruladığında, Azure AD benzersiz olarak tanımlayan kullanıcı hakkında bilgileri (veya talep) içeren uygulamaya bir SAML belirteci verir. Varsayılan olarak, bu kullanıcının kullanıcı adı, e-posta adresi, ad ve Soyadı içerir.

İçinde [Azure portal](https://portal.azure.com), görüntüleyebilir veya uygulama için SAML belirtecinde gönderilen talepleri düzenleyebilirsiniz. Ayarlara erişmek için seçin **Azure Active Directory** > **kurumsal uygulamalar** > çoklu oturum açma için yapılandırılmış uygulama > **çoklu oturum açma** . SAML belirteci ayarlarında bkz **kullanıcı öznitelikleri** bölümü.

![SAML belirteci öznitelikleri kullanıcı Arabiriminde gösterir](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

Neden SAML belirtecinde verilen talepler düzenlemek için gerekebilecek iki olası nedeni vardır:

1. Farklı bir dizi uygulama gerektiriyor URI'ler talep veya değerleri.

2. Uygulama Azure AD'de depolanan kullanıcı asıl adı (UPN) dışında bir şey olması NameIdentifier talep gerektirir.

Ekleme ve talep düzenleme hakkında daha fazla bilgi için bkz: [Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen talepler özelleştiriliyor](develop/active-directory-saml-claims-customization.md).

B2B işbirliği için NameID ve UPN arası Kiracı eşleme kullanıcıları, güvenlik nedenleriyle engellenir.

## <a name="next-steps"></a>Sonraki adımlar

- B2B işbirliği kullanıcı özellikleri hakkında daha fazla bilgi için bkz: [bir Azure Active Directory B2B işbirliği kullanıcının özelliklerini](active-directory-b2b-user-properties.md).
- B2B işbirliği kullanıcılar için kullanıcı belirteçleri hakkında daha fazla bilgi için bkz: [Azure AD B2B işbirliği kullanıcı belirteçleri anlamak](active-directory-b2b-user-token.md).

