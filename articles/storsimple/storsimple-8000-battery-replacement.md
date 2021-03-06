---
title: "Microsoft Azure StorSimple 8000 serisi aygıt pilde Değiştir | Microsoft Docs"
description: "Kaldırmak için değiştirin ve StorSimple Cihazınızda yedek pil modülü korumak açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: f8071cde67017ff031418f0d97da15a618c4969b
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>StorSimple Cihazınızda yedek pil modülü değiştirin

## <a name="overview"></a>Genel Bakış
Birincil muhafaza güç ve soğutma Modülü (PCM), Microsoft Azure StorSimple Cihazınızda bir ek pil paketi vardır. Bu paketi güç sunar, böylece birincil kasası AC güç kaybı ise StorSimple cihazı veri kaydedebilirsiniz. Bu pil paketi olarak adlandırılır *yedek pil Modülü*. Yedek pil modülü yalnızca StorSimple Cihazınızı (EBOD muhafazası bir yedek pil modül içermiyor) birincil muhafazada yok.

Bu öğretici açıklar nasıl yapılır:

* Yedek pil modülünü Kaldır
* Yeni bir yedek pil modülünü yükleme
* Yedek pil modülü koru

> [!IMPORTANT]
> Kaldırma ve bir yedek pil modül değiştirme gözden önce güvenlik bilgileri [StorSimple donanım bileşeni değiştirme giriş](storsimple-8000-hardware-component-replacement.md).


## <a name="remove-the-backup-battery-module"></a>Yedek pil modülünü Kaldır
Bir alan değiştirebilen birim StorSimple cihazınız için yedek pil modülüdür. PCM yüklenmeden önce pil modülü kendi özgün paketleme depolanması gerekir. Yedek pil kaldırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-remove-the-backup-battery-module"></a>Yedek pil modülünü kaldırmak için
1. Azure portalında StorSimple cihaz Yöneticisi hizmeti dikey penceresine gidin. Git **aygıtları** ve ardından Cihazınızı aygıtları listesinden seçin. Gidin **İzleyici** > **donanım durumu**. Altında **paylaşılan bileşenleri**, pil durum öğesine bakın.
2. Pil başarısız oldu PCM tanımlayın. Şekil 1, StorSimple cihazı arkası gösterir.
   
    ![Aygıt birincil muhafaza modüllerinin devre kartı](./media/storsimple-battery-replacement/IC740994.png)
   
    **Şekil 1** PCM ve denetleyici modülleri gösteren birincil cihaz arkasına
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Denetleyici 0 |
   | 4 |Denetleyici 1 |
   
    Sayı 3 Şekil 2'de gösterildiği gibi izleme gösterge ÖNCÜLÜK karşılık gelen PCM 0 üzerinde **pil hataya** aydınlatma.
   
    ![Cihaz PCM izleme gösterge LED'lerinin devre kartı](./media/storsimple-battery-replacement/IC740992.png)
   
    **Şekil 2** geri of PCM izleme gösterge LED'leri gösterme
   
   | Etiket | Açıklama |
   |:--- |:--- |
   | 1 |AC güç kesintisi |
   | 2 |Fan hatası |
   | 3 |Pil hatası |
   | 4 |PCM TAMAM |
   | 5 |DC Güç kesintisi |
   | 6 |Sağlıklı pil |
3. Başarısız pille PCM kaldırmak için adımları [bir PCM kaldırmak](storsimple-8000-power-cooling-module-replacement.md#remove-a-pcm).
4. Kaldırılan PCM ile kaldırın ve aşağıdaki çizimde gösterildiği gibi pil modül işleyiciyi Yukarı Döndür ve pil Kaldır kadar çekme.
   
    ![PCM'den pil çıkarılıyor](./media/storsimple-battery-replacement/IC741019.png)
   
    **Şekil 3** PCM'den pil çıkarılıyor
5. Paketleme alan değiştirebilen birim modülünde yer.
6. Arızalı ünite uygun Bakım ve işleme için Microsoft'a döndürür.

## <a name="install-a-new-backup-battery-module"></a>Yeni bir yedek pil modülünü yükleme
StorSimple cihazınızın birincil muhafazada PCM değiştirme pil modülünü yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-battery-module"></a>Pil modülünü yüklemek için
1. Yedek pil modülü PCM uygun yönde yerleştirin.
2. Bu süreç boyunca tüm bağlayıcı oturak için pil modül işleyiciyi basın.
3. Yönergeleri izleyerek birincil muhafazada PCM Değiştir [güç ve soğutma modülü StorSimple Cihazınızda Değiştir](storsimple-8000-power-cooling-module-replacement.md).
4. Bu değişiklik tamamlandıktan sonra aygıtınıza gidin ve gidin **İzleyici** > **donanım durumu** Azure portalında. Yüklemenin başarılı olduğunu emin olmak için pil durumunu doğrulayın. Yeşil durum pil sağlıklı olduğunu gösterir.

## <a name="maintain-the-backup-battery-module"></a>Yedek pil modülü koru
StorSimple Cihazınızı yedek pil modülü güç kaybı olayı sırasında denetleyiciye güç sağlar. Denetimli bir biçimde kapatmadan önce kritik verileri kaydetmek StorSimple cihazı sağlar. İki tam dolu pilleri ile PCMs, iki ardışık kaybı olay sistem işleyebilir.

Azure portalında **donanım durumu** altında **İzleyici** dikey pil arızalı ya da son yaşam yaklaştığını gösterir. Pil durumu belirtilir **PCM 0 pil** veya **pil PCM 1** altında **paylaşılan bileşenleri**. Bu dikey gösterecek bir **DEGRADED** ömrü sonu yaklaşan için durum ve **başarısız** ömrü son ulaştı.

> [!NOTE]
> Pil raporlayabilirsiniz **başarısız** , yalnızca gerektiği zaman uygulanacak.


Varsa **DEGRADED** durumu görünür, eylem aşağıdaki seyri öneririz:

* Sistem son güç kaybı karşılaşmış veya piller düzenli bakım altında. Devam etmeden önce 12 saat sistem gözlemleyin.
  
  * Durum hala ise **DEGRADED** AC sürekli bağlantı 12 saatlik denetleyicileri ve çalışan PCMs ile güç sonra sonra pil değiştirilmesi gerekiyor. Lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) yedek yedek pil modülü için.
  * Durumu Tamam 12 saat sonra olursa, pil işlevseldir ve yalnızca bir bakım ücret gereklidir.
* Ayrıca bir ilişkili AC güç kaybı değil açıldı ve PCM açık ve AC güç kaynağına bağlı, pil değiştirilmesi gerekiyor. [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) bir yedek yedek pil modül sıralamak için.

> [!IMPORTANT]
> Ulusal ve bölgesel düzenlemeleri göre başarısız pil Dispose.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [StorSimple donanım bileşeni değiştirme](storsimple-8000-hardware-component-replacement.md).

