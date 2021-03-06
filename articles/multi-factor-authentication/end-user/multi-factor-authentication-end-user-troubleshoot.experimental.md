---
title: İki aşamalı doğrulama sorunlarını giderme | Microsoft Docs
description: Bu belge kullanıcılar Azure çok faktörlü kimlik doğrulaması ile ilgili bir sorunu içine çalıştırırsanız yapmanız gerekenler hakkında bilgi sağlar.
services: multi-factor-authentication
keywords: çok faktörlü kimlik doğrulama istemcisi, kimlik doğrulama sorunu bağıntı kimliği
documentationcenter: ''
author: barlanmsft
manager: mtillman
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: barlan
ms.reviewer: richagi
ms.custom: end-user
ms.openlocfilehash: 736a1f03ef87850fdaaee7ce636d8dd0f3ae1a84
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="get-help-with-two-step-verification"></a>İki aşamalı doğrulama konusunda yardım alın
Bu makalede, iki aşamalı doğrulama hakkında yapmalarını isteriz en yaygın sorular yanıtlanmaktadır. 

## <a name="why-do-i-have-to-perform-two-step-verification-can-i-turn-it-off"></a>İki aşamalı doğrulamayı gerçekleştirmek neden var mı? I kapatabilirsiniz?

İki aşamalı doğrulama, kuruluşunuzun hesaplarınızı korumak üzere kullanmak için seçtiğiniz bir güvenlik özelliğidir. Kimlik doğrulaması iki formlarında kullandığından yalnızca bir paroladan daha güvenli olan: bir şey bildiğiniz ve bir şey, sahip olursunuz. Bildiğiniz parolanızı bir şeydir. İle kullandığınız telefon veya aygıt ile yaygın olan şeydir. Hesabınız iki basamaklı doğrulama ile korunuyorsa, parolanızı alırsanız bile kötü amaçlı bir korsanın oturum açamazsınız. Telefonunuza erişiminiz olmadığı için kullanıcılar oturum açamaz. 

İki aşamalı doğrulamayı Microsoft sunar, ancak bu özelliği kullanmak, kuruluşunuzun seçer. Hesabınızı korumak için bir parola kullanarak dışında yalnızca kabul edemiyor gibi şirket destek, bunu gerektiriyorsa, çıkma olamaz. 

İki aşamalı doğrulamayı kişisel Microsoft hesabınız için açık varsa ve ayarlarınızı değiştirmek istediğiniz okuma [iki basamaklı doğrulama hakkında](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) yerine. 

## <a name="i-dont-have-my-phone-with-me-today"></a>Telefonum benimle bugün sahibim yok

İş için oturum açmak telefonunuz evde, ancak hala bırakın bazı günlerde gerekir. Oturum farklı bir doğrulama yöntemi deneyin ilk şey imzalama. İki aşamalı doğrulama için kaydolurken birden fazla telefon numarası ayarlamak? Farklı bir yöntem oturum açmayı deneyin için şu adımları izleyin:

1. Normal olarak oturum açın.
2. İki aşamalı doğrulama sayfası açıldığında seçin **farklı bir doğrulama seçeneği kullanma**.

   ![Farklı bir doğrulama](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Kullanmak istediğiniz doğrulama seçeneğini seçin. 
  - Alternatif yöntemlerinizi erişimi yoksa ya da, kişi hesabınızda oturum açarken yardım almak için şirketinizin destekler.
  - Alternatif yöntemlerinizi erişiminiz varsa, iki aşamalı doğrulama ile devam edin.

Görmüyorsanız, **farklı bir doğrulama seçeneği kullanma** bağlantı kaydetmedi ayarladığınız alternatif yöntemleri için iki aşamalı doğrulamayı ilk kaydolurken anlamına gelir. Hesabınızda oturum açarken yardım almak için şirket desteğe başvurun. Oturum açtınız sonra emin olun [ayarlarınızı yönetmenize](multi-factor-authentication-end-user-manage-settings.md) gelecek sefer için ek doğrulama yöntemleri eklemek için. 

## <a name="i-lost-my-phone-or-got-a-new-number"></a>Telefonum kayıp veya yeni bir sayı var
Hesabınıza dönmek için iki yolu vardır. İlk alternatif kimlik doğrulama telefon numaranızı kullanarak, bir ayarlamışsanız oturum sağlamaktır. Ayarlarınızı temizlemek için şirketin destek istemek için saniyedir.

Telefonunuz kaybolur veya çalınırsa, ayrıca şirket destek söyleyin öneririz. Uygulama parolaları sıfırlama gerekir ve temizleyin herhangi aygıtları anımsanacak. 

### <a name="use-an-alternate-phone-number"></a>Alternatif bir telefon numarası kullanın
İkincil bir telefon numarası veya farklı bir cihaz üzerinde Doğrulayıcı uygulama gibi birden çok doğrulama seçeneklerini ayarladıysanız, oturum açmak için bunlardan birini kullanın.

Alternatif bir telefon numarası kullanarak oturum için şu adımları izleyin:

1. Normal olarak oturum açın.
2. Daha fazla hesabınızı doğrulamak için istendiğinde seçin **farklı bir doğrulama seçeneği kullanma**.
   
   ![Farklı bir doğrulama](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Telefon numarası veya erişiminiz cihazı seçin.
4. Hesabınızdaki geri girdikten sonra [ayarlarınızı yönetmenize](multi-factor-authentication-end-user-manage-settings.md) kimlik doğrulama telefon numaranızı değiştirmek için.

### <a name="clear-your-settings"></a>Ayarlarınızı temizleyin
İkincil kimlik doğrulama telefon numaranızı yapılandırmadıysanız, Yardım için şirket desteğe başvurun sahip. Clear sahip bir sonraki şekilde, ayarlarınızı oturum açın, vermesi istenir [kaydetmek için iki aşamalı doğrulamayı](multi-factor-authentication-end-user-first-time.md) yeniden.

## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Bir metin alıyorum değil veya telefonum üzerinde çağırın
Neden oturum açın, ancak metin ya da telefon araması almamayı deneyebilirsiniz birkaç nedeni vardır. Başarıyla metinleri ya da telefon aramaları telefonunuza geçmişte aldığınız, bu sorun büyük olasılıkla değil hesabınızı telefon sağlayıcısı ile ilgili bir sorun olur. İyi hücre sinyal sahip olduğunuzdan emin olun. Ve bir SMS mesajı almaya çalışıyorsanız, metin iletileri almasına mümkün olduğundan emin olun. Siz veya metin çağırmak için bir arkadaş isteyin, bir test olarak. 

Bir metin veya arama için birkaç dakika beklediğinizden, farklı bir seçenek hesabınızda almanın en hızlı yolu çalışmaktır.

1. Seçin **farklı bir doğrulama seçeneği kullanma** sayfasında doğrulama için bekliyor.
   
    ![Farklı bir doğrulama](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)
2. Kullanmak istediğiniz telefon numarasını veya teslim yöntemini seçin.
   
    Birden çok doğrulama kodları aldıysanız, en yeni birini kullanın.

Yapılandırılmış başka bir yöntem yoksa, şirket desteğine başvurun ve ayarlarınızı temizlemek isteyin. ' De, oturum açtığınızda, istenir [çok faktörlü kimlik doğrulamasını kurma](multi-factor-authentication-end-user-first-time.md) yeniden.

Hatalı hücre sinyali nedeniyle gecikmeler genellikle varsa kullanmanız önerilir [Microsoft Authenticator uygulaması](microsoft-authenticator-app-how-to.md) smartphone üzerinde. Uygulama oturum açmak için kullandığınız rastgele güvenlik kodlarını oluşturabilir ve bu kodları herhangi bir hücre sinyal ya da Internet bağlantısı gerektirmez.

## <a name="app-passwords-are-not-working"></a>Uygulama parolaları çalışmıyor
İlk olarak, uygulama parolası doğru girdiğinizden emin olun. İki aşamalı doğrulamayı desteklemeyen ancak yalnızca eski Masaüstü uygulamaları için normal parolanızı oluşturulan uygulama parolasını değiştirir. Hala çalışmıyorsa, oturum açma deneyin ve [yeni bir uygulama parolası oluşturmanız](multi-factor-authentication-end-user-app-passwords.md).  Hala çalışmıyorsa, şirket desteğine başvurun ve bunları [mevcut uygulama parolalarınızın silme](../../active-directory/authentication/howto-mfa-userdevicesettings.md) ve daha sonra yeni bir tane oluşturabilirsiniz.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Bir yanıt sorunumu Bul alamadık.
Sorun giderme adımları çalıştınız, ancak hala sorunlarla çalıştıran şirket desteğinize başvurun. Bunlar size yardımcı olmak üzere görebilmeniz gerekir.

## <a name="related-topics"></a>İlgili konular
* [İki aşamalı doğrulama için ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)  
* [Microsoft Authenticator uygulaması hakkında SSS](microsoft-authenticator-app-faq.md)

