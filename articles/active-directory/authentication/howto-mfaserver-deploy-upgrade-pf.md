---
title: PhoneFactor’u Azure MFA Sunucusuna yükseltme | Microsoft Belgeleri
description: Eski phonefactor aracından yükseltme yaptığınızda Azure MFA Sunucusu ile çalışmaya başlayın.
services: multi-factor-authentication
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: 42838ff7-bdf2-4d06-bacc-b3839a00cd76
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/06/2017
ms.author: joflore
ms.openlocfilehash: eca7e846aaa17c245a1d904489a25e6de2c3cca7
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>PhoneFactor Aracısı’nı Azure Multi-Factor Authentication Sunucusu’na yükseltme
PhoneFactor Aracısı v5.x veya önceki bir sürümünü Azure Multi-Factor Authentication Sunucusuna yükseltmek için ilk olarak PhoneFactor Aracısını ve bağlı bileşenleri kaldırın. Bundan sonra Multi-Factor Authentication Sunucusu ve bağlı bileşenleri yüklenebilir.

## <a name="uninstall-the-phonefactor-agent"></a>PhoneFactor Aracısını kaldırma

1. İlk olarak, PhoneFactor veri dosyasını yedekleyin. Varsayılan yükleme konumu C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata şeklindedir.

2. Kullanıcı Portalı yüklü ise:
  1. Yükleme klasörüne gidin ve web.config dosyasını yedekleyin. Varsayılan yükleme konumu C:\inetpub\wwwroot\PhoneFactor şeklindedir.

  2. Portala özel temalar eklediyseniz, C:\inetpub\wwwroot\PhoneFactor\App_Themes dizini altındaki özel klasörünüzü yedekleyin.

  3. Kullanıcı Portalı’nı, PhoneFactor Aracısı aracılığıyla (yalnızca PhoneFactor Aracısı ile aynı sunucuda yüklüyse kutlanılabilir) ya da Windows Programlar ve Özellikler aracılığıyla kaldırın.

3. Mobil Uygulama Web Hizmeti yüklü ise:

  1. Yükleme klasörüne gidin ve web.config dosyasını yedekleyin. Varsayılan yükleme konumu C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService şeklindedir.

  2. Mobil Uygulama Web Hizmeti’ni Windows Programlar ve Özellikler aracılığıyla kaldırın.

4. Web Hizmeti SDK’sı yüklü ise, PhoneFactor Aracısı aracılığıyla ya da Windows Programlar ve Özellikler aracılığıyla kaldırın.

5. PhoneFactor Aracısı’nı Windows Programlar ve Özellikler aracılığıyla kaldırın.

## <a name="install-the-multi-factor-authentication-server"></a>Multi-Factor Authentication Sunucusu’nu yükleme

Yükleme yolu önceki PhoneFactor Aracısı yüklemesinin kayıt defterinden alınır, bu nedenle aynı konuma (örneğin, C:\Program Files\PhoneFactor) yükleme yapılmalıdır. Yeni yüklemeler farklı bir varsayılan yükleme yoluna (örneğin, C:\Program Files\Multi-Factor Authentication Server) sahip olacaktır. Önceki PhoneFactor Aracısı’ndan kalan veri dosyası yükleme sırasında yükseltilmelidir, bu nedenle kullanıcılarınız ve ayarlarınız yeni Multi-Factor Authentication Sunucusu yüklemesi sonrasında da burada kalmalıdır.

1. İstenirse, Multi-Factor Authentication Sunucusu’nu etkinleştirin ve uygun çoğaltma grubuna atandığından emin olun.

2. Web Hizmeti SDK’sı daha önce yüklendiyse, Multi-Factor Authentication Sunucusu Kullanıcı Arabirimi aracılığıyla yeni Web Hizmeti SDK’sını yükleyin.

  Bu durumda varsayılan sanal dizin adı **PhoneFactorWebServiceSdk** yerine **MultiFactorAuthWebServiceSdk** şeklindedir. Önceki adı kullanmak istiyorsanız, yükleme sırasında sanal dizin adını değiştirmeniz gerekir. Aksi halde, yeni varsayılan adı kullanacak şekilde yüklemeye izin verirseniz doğru konuma yönlendirmek için, Web Hizmeti SDK'sına (Kullanıcı Portalı ve Mobil Uygulama Web Hizmeti gibi) başvuran tüm uygulamalarda URL'yi değiştirmeniz gerekecektir.

3. Kullanıcı Portalı önceden PhoneFactor Aracısı Sunucusu’nda yüklüyse, yeni Multi-Factor Authentication Kullanıcı Portalı’nı Multi-Factor Authentication Sunucusu Kullanıcı Arabirimi aracılığıyla yükleyin.

  Bu durumda varsayılan sanal dizin adı **PhoneFactor** yerine **MultiFactorAuth** şeklindedir. Önceki adı kullanmak istiyorsanız, yükleme sırasında sanal dizin adını değiştirmeniz gerekir. Aksi takdirde, yeni varsayılan adı kullanacak şekilde yüklemeye izin verirseniz, Multi-Factor Authentication Sunucusu’nda Kullanıcı Portalı simgesine tıklamanız ve Ayarlar sekmesinde Kullanıcı Portalı URL’sini güncelleştirmeniz gerekir.

4. Kullanıcı Portalı ve/veya Mobil Uygulama Web hizmeti daha önce PhoneFactor Aracısı’ndan farklı bir sunucuya yüklendiyse:

  1. Yükleme konumuna (örneğin, C:\Program Files\PhoneFactor) gidin ve bir veya daha fazla yükleyiciyi diğer sunucuya kopyalayın. Kullanıcı Portalı ve Mobil Uygulama Web Hizmeti için 32-bit ve 64-bit yükleyiciler vardır. Bunlara MultiFactorAuthenticationUserPortalSetupXX.msi ve MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi adı verilir.

  2. Web sunucusunda Kullanıcı Portalı'nı yüklemek için, yönetici olarak bir komut istemi açın ve MultiFactorAuthenticationUserPortalSetupXX.msi komutunu çalıştırın.

    Bu durumda varsayılan sanal dizin adı **PhoneFactor** yerine **MultiFactorAuth** şeklindedir. Önceki adı kullanmak istiyorsanız, yükleme sırasında sanal dizin adını değiştirmeniz gerekir. Aksi takdirde, yeni varsayılan adı kullanacak şekilde yüklemeye izin verirseniz, Multi-Factor Authentication Sunucusu’nda Kullanıcı Portalı simgesine tıklamanız ve Ayarlar sekmesinde Kullanıcı Portalı URL’sini güncelleştirmeniz gerekir. Mevcut kullanıcıların yeni URL konusunda bilgilendirilmesi gerekir.

  3. Kullanıcı Portalı yükleme konumuna (örneğin, C:\inetpub\wwwroot\MultiFactorAuth) gidin ve web.config dosyasını düzenleyin. Yükseltme öncesi yedeklenen özgün web.config dosyasındaki appSettings ve applicationSettings bölümlerindeki değerleri yeni web.config dosyasına kopyalayın. Web Hizmeti SDK’sını yüklerken yeni varsayılan sanal dizin adını sakladıysanız, doğru konuma yönlendirmek için applicationSettings bölümünde URL’yi değiştirin. Önceki web.config dosyasında diğer varsayılan değerler değiştirildiyse, aynı değişiklikleri yeni web.config dosyasına uygulayın.

  4. Web sunucusunda Mobil Uygulama Web Hizmeti’ni yüklemek için, yönetici olarak bir komut istemi açın ve MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi komutunun çalıştırın.

    Bu durumda varsayılan sanal dizin adı **PhoneFactorPhoneAppWebService** yerine **MultiFactorAuthMobileAppWebService** şeklindedir. Önceki adı kullanmak istiyorsanız, yükleme sırasında sanal dizin adını değiştirmeniz gerekir. Son kullanıcıların mobil cihazlarda yazmalarını kolaylaştırmak için kısa bir ad seçmek isteyebilirsiniz. Aksi takdirde, yeni varsayılan adı kullanacak şekilde yüklemeye izin verirseniz, Multi-Factor Authentication Sunucusu’nda Mobil Uygulama simgesine tıklamanız ve Mobil Uygulama Web Hizmeti URL’sini güncelleştirmeniz gerekir.

  5. Mobil Uygulama Web Hizmeti yükleme konumuna (örneğin, C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) gidin ve web.config dosyasını düzenleyin. Yükseltme öncesi yedeklenen özgün web.config dosyasındaki appSettings ve applicationSettings bölümlerindeki değerleri yeni web.config dosyasına kopyalayın. Web Hizmeti SDK’sını yüklerken yeni varsayılan sanal dizin adını sakladıysanız, doğru konuma yönlendirmek için applicationSettings bölümünde URL’yi değiştirin. Önceki web.config dosyasında diğer varsayılan değerler değiştirildiyse, aynı değişiklikleri yeni web.config dosyasına uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Multi-Factor Authentication Sunucusu için [kullanıcı portalını yükleyin](howto-mfaserver-deploy-userportal.md).

- Uygulamalarınız için [Windows Kimlik Doğrulamasını yapılandırın](howto-mfaserver-windows.md). 
