---
title: Azure Dosyalarını Azure'a yedekleme
description: Bu makalede Azure dosya paylaşımlarınızı yedekleme ve geri yükleme işlemlerinin ayrıntıları verilir ve yönetim görevleri açıklanır.
services: backup
author: markgalioto
ms.author: markgal
ms.date: 3/23/2018
ms.topic: tutorial
ms.service: backup
manager: carmonm
ms.openlocfilehash: 440cee4309fad0a22d8964982f3aad2178397124
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="back-up-azure-file-shares"></a>Azure dosya paylaşımlarını yedekleme
Bu makalede, Azure portalını kullanarak [Azure dosya paylaşımlarını](../storage/files/storage-files-introduction.md) yedekleme ve geri yükleme işlemlerinin nasıl yapılacağı açıklanmaktadır.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Azure Dosyalarını yedeklemek için Kurtarma Hizmetleri kasasını yapılandırma
> * Geri yükleme noktası oluşturmak için isteğe bağlı yedekleme işini çalıştırma
> * Bir veya birden çok dosyayı geri yükleme noktasından geri yükleme
> * Yedekleme işlerini yönetme
> * Azure Dosyaları üzerindeki korumayı durdurma
> * Yedekleme verilerinizi silme

## <a name="prerequisites"></a>Ön koşullar
Azure dosya paylaşımını yedekleyebilmeniz için önce [desteklenen Depolama Hesabı türlerinden](troubleshoot-azure-files.md#preview-boundaries) birinde yer aldığından emin olmalısınız. Bunu doğruladıktan sonra dosya paylaşımlarınızı koruyabilirsiniz.

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Önizleme sırasında Azure dosya paylaşımı yedeklemesine yönelik sınırlamalar
Azure dosya paylaşımları için yedekleme önizlemede sunulmaktadır. Önizleme sırasında aşağıdaki sınırlamaları unutmayın:
- [Bölgesel olarak yedekli depolama (ZRS)](../storage/common/storage-redundancy-zrs.md) veya [okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](../storage/common/storage-redundancy-grs.md) çoğaltması ile depolama hesaplarında Azure dosya paylaşımlarını koruyamazsınız.
- Sanal Ağların etkin olduğu depolama hesaplarında Azure dosya paylaşımlarını koruyamazsınız.
- Azure Dosyalarını korumak için bir PowerShell veya CLI yoktur.
- Günlük zamanlanan maksimum yedekleme sayısı birdir.
- Günlük zamanlanan maksimum istek üzerine yedekleme sayısı dörttür.
- Kurtarma Hizmetleri kasanızdaki yedeklemelerin yanlışlıkla silinmesini önlemek için depolama hesabındaki [kaynak kilitlerini](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) kullanın.
- Azure Backup tarafından oluşturulan anlık görüntülerin silmeyin. Anlık görüntülerin silinmesi, kurtarma noktalarının kaybolması ve/veya geri yükleme işlemlerinin başarısız olmasıyla sonuçlanabilir 

## <a name="configuring-backup-for-an-azure-file-share"></a>Azure dosya paylaşımı için yedeklemeyi yapılandırma
Tüm yedekleme verileri Kurtarma Hizmetleri kasalarında depolanır. Bu öğreticide zaten yerleşik bir Azure dosya paylaşımınız olduğu varsayılır. Azure dosya paylaşımınızı yedeklemek için:

1. Dosya paylaşımınızla aynı bölgede bir Kurtarma Hizmetleri kasası oluşturun. Zaten bir kasanız varsa, kasanızın Genel Bakış sayfasını açın ve **Yedekle**'ye tıklayın.

    ![Yedekleme hedefi olarak Azure Dosya Paylaşımı'nı seçin](./media/backup-file-shares/overview-backup-page.png)

2. Yedekleme Hedefi menüsünde, **Neleri yedeklemek istiyorsunuz?** alanında Azure Dosya Paylaşımı'nı seçin.

    ![Yedekleme hedefi olarak Azure Dosya Paylaşımı'nı seçin](./media/backup-file-shares/choose-azure-fileshare-from-backup-goal.png)

3. Azure dosya paylaşımınızı Kurtarma Hizmetleri kasasına yapılandırmak için **Yedekle**'ye tıklayın. 

   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/backup-file-shares/set-backup-goal.png)

    Kasa Azure dosya paylaşımıyla ilişkilendirildikten sonra, Yedekle menüsü açılır ve Depolama hesabı seçmeniz istenir. Menüde kasanızın bulunduğu bölgede desteklenen ve henüz Kurtarma Hizmetleri kasasıyla ilişkilendirilmemiş olan tüm Depolama Hesapları görüntülenir.

   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/backup-file-shares/list-of-storage-accounts.png)

4. Depolama hesapları listesinde bir hesap seçin ve **Tamam**'a tıklayın. Azure, depolama hesabında yedeklenebilecek dosya paylaşımlarını arar. Dosya paylaşımlarınızı kısa süre önce eklediyseniz ve listede görmüyorsanız, dosya paylaşımlarının gösterilmesi için biraz zaman tanıyın.

   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/backup-file-shares/discover-file-shares.png)

5. **Dosya Paylaşımları** listesinde, yedeklemek istediğiniz bir veya birden çok dosya paylaşımını seçin ve **Tamam**'a tıklayın.

6. Dosya Paylaşımlarınızı seçtikten sonra, Yedekle menüsü **Yedekleme ilkesi**'ne dönüşür. Bu menüde mevcut yedekleme ilkelerinden birini seçin veya yeni ilke oluşturun ve ardından **Yedeklemeyi Etkinleştir**'e tıklayın. 

   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/backup-file-shares/apply-backup-policy.png)

    Yedekleme ilkesi oluşturulduktan sonra, planlanan zamanda Dosya Paylaşımlarının anlık görüntüsü alınır ve seçilen süre için kurtarma noktası korunur.

## <a name="create-an-on-demand-backup"></a>İsteğe bağlı yedekleme oluşturma
Bazen yedekleme ilkesinde planlanan zamanların dışında bir yedekleme anlık görüntüsü veya kurtarma noktası oluşturmak isteyebilirsiniz. İsteğe bağlı yedekleme oluşturmak için tercih edilen bir zaman, yedekleme ilkesini yapılandırmanızdan hemen sonrasıdır. Yedekleme ilkesindeki zamanlamaya bağlı olarak, bir anlık görüntünün alınması için saatler veya günler geçebilir. Yedekleme ilkesi devreye girene kadar verilerinizi korumak için, bir isteğe bağlı yedekleme başlatın. Dosya paylaşımlarınızda planlı değişiklikler yapmadan önce çoğunlukla bir İsteğe bağlı yedekleme oluşturmak gerekir. 

### <a name="to-create-an-on-demand-backup"></a>İsteğe bağlı yedekleme oluşturmak için

1. Dosya paylaşımı kurtarma noktalarını içeren Kurtarma Hizmetleri kasasını açın ve **Yedekleme Öğeleri**'ne tıklayın. Yedekleme Öğesi türlerinin listesi gösterilir.

   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/backup-file-shares/list-of-backup-items.png)

2. Listeden **Azure Depolama (Azure Dosyaları)** öğesini seçin. Azure dosya paylaşımlarının listesi görüntülenir.

   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/backup-file-shares/list-of-azure-files-backup-items.png)

3. Azure dosya paylaşımları listesinden istediğiniz dosya paylaşımını seçin. Seçili dosya paylaşımının Yedekleme Öğesi menüsü açılır.

   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/backup-file-shares/backup-item-menu.png)

4. Yedekleme Öğesi menüsünde **Şimdi Yedekle**'ye tıklayın. Bu isteğe bağlı bir yedekleme işi olduğundan, kurtarma noktasıyla ilişkilendirilmiş bir bekletme ilkesi yoktur. **Şimdi Yedekle** iletişim kutusu açılır. Kurtarma noktasını bekletmek istediğiniz son günü belirtin. 
  
   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/backup-file-shares/backup-now-menu.png)

## <a name="restore-from-backup-of-azure-file-share"></a>Azure dosya paylaşımını yedekten geri yükleme
Dosya paylaşımının tamamını ya da tek tek dosya veya klasörleri bir Geri Yükleme Noktasından geri yüklemeniz gerekiyorsa, önceki bölümde ayrıntılarıyla açıklandığı gibi Yedekleme Öğesi'ne gidin. İstenen Belirli bir noktadan dosya paylaşımının tamamını geri yüklemek için **Paylaşımı Geri Yükle**'yi seçin. Görüntülenen Geri Yükleme Noktaları listesinden birini seçerek Geçerli dosya paylaşımınızın üzerine yazın veya Bunu aynı bölgede başka bir dosya paylaşımına geri yükleyin.

   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/backup-file-shares/select-restore-location.png)

## <a name="restore-individual-files-or-folders-from-backup-of-azure-file-shares"></a>Azure dosya paylaşımlarının yedeğinden tek tek dosya veya klasörleri geri yükleme
Azure Backup, Azure Portal'ın içinde bir Geri Yükleme Noktasına göz atabilmenizi sağlar. Seçtiğiniz dosya veya klasörü geri yüklemek için, Yedekleme Öğesi sayfasında Dosya Kurtarma'ya tıklayın ve Geri Yükleme Noktaları listesinden seçim yapın. Kurtarma Hedefi'ni seçin ve ardından geri yükleme noktasına göz atmak için **Dosya Seç**'e tıklayın. Dilediğiniz dosya veya klasörü seçin ve sonra da **Geri Yükle**'yi seçin.

   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/backup-file-shares/restore-individual-files-folders.png)

## <a name="manage-azure-file-share-backups"></a>Azure dosya paylaşımı yedeklemelerini yönetme

**Yedekleme İşleri** sayfasında, Dosya paylaşımı yedeklemeleri için aşağıda gösterilen çeşitli yönetim görevlerini yürütebilirsiniz:
- [İşleri izleme](backup-azure-files.md#monitor-jobs)
- [Yeni ilke oluşturma](backup-azure-files.md#create-a-new-policy)
- [Dosya paylaşımı üzerindeki korumayı durdurma](backup-azure-files.md#stop-protecting-an-azure-file-share)
- [Dosya paylaşımı üzerindeki korumayı sürdürme](backup-azure-files.md#resume-protection-for-azure-file-share)
- [Yedekleme verilerini silme](backup-azure-files.md#delete-backup-data)

### <a name="monitor-jobs"></a>İşleri izleme

**Yedekleme İşleri** sayfasında tüm işlerin ilerleme durumunu izleyebilirsiniz.

**Yedekleme İşleri** sayfasını açmak için:

- İzlemek istediğiniz Kurtarma Hizmetleri kasasını açın ve Kurtarma Hizmetleri kasası menüsünde **İşler**'e ve ardından **Yedekleme İşleri**'ne tıklayın.
   ![İzlemek istediğiniz işi seçin](./media/backup-file-shares/open-backup-jobs.png)

    Yedekleme işlerinin listesi ve bu işlerin durumu gösterilir.
   ![İzlemek istediğiniz işi seçin](./media/backup-file-shares/backup-jobs-progress-list.png)

### <a name="create-a-new-policy"></a>Yeni ilke oluşturma

Kurtarma Hizmetleri kasasının **Yedekleme İlkeleri**'nde, Azure dosya paylaşımlarını yedeklemek için yeni bir ilke oluşturabilirsiniz. Dosya paylaşımları için Yedeklemeyi yapılandırdığınız sırada oluşturulan tüm ilkeler, Azure dosya Paylaşımı İlke Türü değeriyle gösterilir.

Mevcut Yedekleme ilkelerini görüntülemek için:

- İstediğiniz Kurtarma Hizmetleri kasasını açın ve Kurtarma Hizmetleri kasası menüsünde **Yedekleme ilkeleri**'ne tıklayın. Tüm Yedekleme ilkeleri listelenir.

   ![İzlemek istediğiniz işi seçin](./media/backup-file-shares/list-of-backup-policies.png)

Yeni bir Yedekleme ilkesi oluşturmak için:

1. Kurtarma Hizmetleri kasası menüsünde **Yedekleme ilkeleri**'ne tıklayın.
2. Yedekleme ilkeleri listesinde **Ekle**'ye tıklayın.

   ![İzlemek istediğiniz işi seçin](./media/backup-file-shares/new-backup-policy.png)

3. **Ekle** menüsünde **Azure Dosya Paylaşımı**'nı seçin. Azure dosya paylaşımının Yedekleme ilkesi menüsü açılır. İlke için bir ad, yedekleme sıklığı ve kurtarma noktalarının bekletme aralığını sağlayın. İlkeyi tanımladığınızda Tamam'a tıklayın.

   ![İzlemek istediğiniz işi seçin](./media/backup-file-shares/create-new-policy.png)

### <a name="stop-protecting-an-azure-file-share"></a>Azure dosya paylaşımının korumasını durdurma

Azure dosya paylaşımının korumasını durdurmayı seçerseniz, kurtarma noktalarını tutmak isteyip istemediğiniz sorulur. Azure dosya paylaşımlarını korumayı durdurmanın iki yolu vardır:

- Gelecek tarihli tüm yedekleme işlerini durdurma ve tüm kurtarma noktalarını silme, veya
- Gelecek tarihli tüm yedekleme işlerini durdurma ama kurtarma noktalarını bırakma

Azure Backup tarafından oluşturulan temel anlık görüntüler tutulacağından, kurtarma noktalarını depolama alanında bırakmanın bir maliyeti olabilir. Bununla birlikte, kurtarma noktalarını bırakmanın avantajı, daha sonra isterseniz Dosya paylaşımını geri yükleyebilmenizdir. Kurtarma noktalarını bırakmanın maliyeti hakkında bilgi için fiyatlandırma ayrıntılarına bakın. Tüm kurtarma noktalarını silmeyi seçerseniz, Dosya paylaşımını geri yükleyemezsiniz.

Azure dosya paylaşımının korumasını durdurmak için:

1. Dosya paylaşımı kurtarma noktalarını içeren Kurtarma Hizmetleri kasasını açın ve **Yedekleme Öğeleri**'ne tıklayın. Yedekleme Öğesi türlerinin listesi gösterilir.

   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/backup-file-shares/list-of-backup-items.png) 

2. **Yedekleme Yönetimi Türü** listesinde **Azure Depolama (Azure Dosyaları)** öğesini seçin. Yedekleme Öğeleri (Azure Depolama (Azure Dosyaları)) listesi görüntülenir.

   ![ek menüyü açmak için öğeye tıklayın](./media/backup-file-shares/azure-file-share-backup-items.png) 

3. Yedekleme Öğeleri (Azure Depolama (Azure Dosyaları)) listesinde, durdurmak istediğiniz yedekleme öğesini seçin.

4. Azure dosya paylaşımı öğelerinde, **Diğer** menüsüne tıklayın ve **Yedeklemeyi Durdur**'u seçin. 

   ![ek menüyü açmak için öğeye tıklayın](./media/backup-file-shares/stop-backup.png)

5. Yedeklemeyi Durdur menüsünde **Yedekleme Verilerini Koru**'yu veya **Yedekleme Verilerini Sil**'i seçin ve **Yedeklemeyi Durdur**'a tıklayın.

   ![ek menüyü açmak için öğeye tıklayın](./media/backup-file-shares/retain-data.png)

### <a name="resume-protection-for-azure-file-share"></a>Azure dosya paylaşımının korumasını sürdürme

Dosya paylaşımının koruması durdurulurken Yedekleme Verilerini Koru seçeneği belirtildiyse, korumayı sürdürmek mümkündür. **Yedekleme Verilerini Sil** seçeneği belirtildiyse, dosya paylaşımının koruması sürdürülemez.

Dosya paylaşımının korumasını sürdürmek için Yedekleme Öğesi'ne gidin ve Yedeklemeyi Sürdür'e tıklayın. Yedekleme İlkesi açılır ve yedeklemeyi sürdürmek için dilediğiniz ilkeyi seçebilirsiniz.

   ![İzlemek istediğiniz işi seçin](./media/backup-file-shares/resume-backup-job.png)

### <a name="delete-backup-data"></a>Yedekleme verilerini silme 

Dosya paylaşımının yedeklemesini, Yedeklemeyi durdurma işi sırasında veya korumayı durdurduktan sonra istediğiniz zaman silebilirsiniz. Hatta kurtarma noktalarını silmeden önce birkaç gün veya birkaç hafta beklemek yararlı olabilir. Kurtarma noktalarını geri yüklemekten farklı olarak, yedekleme verilerini silerken belirli kurtarma noktalarının silinmesini seçemezsiniz. Yedekleme verilerinizi silmeyi seçtiyseniz, öğeye ilişkilendirilmiş tüm kurtarma noktalarını silersiniz.

Aşağıdaki yordamda sanal makine için Yedekleme işinin durdurulmuş olduğu varsayılır. Yedekleme işi durdurulduktan sonra, Yedekleme öğesi panosunda Yedeklemeyi Sürdür ve Yedekleme Verilerini Sil seçenekleri sağlanır. Yedekleme Verilerini Sil'e tıklayın ve silme işlemini onaylamak için Dosya paylaşımının adını yazın. İsteğe bağlı olarak, silmek için bir Neden veya Açıklama sağlayın.

## <a name="see-also"></a>Ayrıca Bkz.
Azure dosya paylaşımları hakkında daha fazla bilgi için bkz.
- [Azure dosya paylaşımını yedekleme hakkında SSS](backup-azure-files-faq.md)
- [Azure dosya paylaşımını yedekleme sorunlarını giderme](troubleshoot-azure-files.md)
