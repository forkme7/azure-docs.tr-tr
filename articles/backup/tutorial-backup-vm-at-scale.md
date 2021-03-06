---
title: "Azure sanal makinelerini uygun ölçekte yedekleme | Microsoft Docs"
description: "Azure’da aynı anda birden çok sanal makineyi yedekleme"
services: backup
keywords: "sanal makine yedeklemesi; sanal makine yedeklemek; VM yedeklemesi; VM’yi yedekleme; Azure VM yedekleme; yedekleme ve olağanüstü durum kurtarma"
author: markgalioto
ms.author: markgal
ms.date: 2/14/2018
ms.topic: tutorial
ms.service: backup
ms.custom: mvc
ms.openlocfilehash: f1cfa72d0fb3c83ef6265649b740dec317f0e4b2
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="use-azure-portal-to-back-up-multiple-virtual-machines"></a>Birden çok sanal makineyi yedeklemek için Azure portalını kullanma

Azure’da verileri yedeklediğinizde söz konusu veriler Kurtarma Hizmetleri kasası adında bir Azure kaynağında depolanır. Kurtarma Hizmetleri kasası kaynağı, Azure hizmetlerinin birçoğunun Ayarlar menüsünden kullanılabilir. Kurtarma Hizmetleri kasasının, Azure hizmetlerinden birçoğunun Ayarlar menüsüyle tümleştirilmiş halde olmasının avantajı, verilerin yedeklenmesini kolaylaştırmasıdır. Ancak işletmenizde her bir veritabanı veya sanal makine ile tek tek çalışmak yorucu olabilir. Tüm sanal makinelere ait verileri tek bir departmanda veya tek bir konumda yedeklemek istediğinizde ne yapabilirsiniz? Birden çok sanal makineyi yedekleyerek bir yedekleme ilkesi oluşturmak ve bu ilkeyi istenen sanal makinelere uygulamak kolay bir işlemdir. Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

> [!div class="checklist"]
> * Kurtarma Hizmetleri kasası oluşturma
> * Yedekleme ilkesi tanımlama
> * Birden çok sanal makineyi korumak için yedekleme ilkesini uygulama
> * Korumalı sanal makineler için isteğe bağlı bir yedekleme işi tetikleme

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri kasası, yedekleme verilerini ve korumalı sanal makinelere uygulanan yedekleme ilkesini içerir. Sanal makineleri yedekleme işlemi, yerel bir işlemdir. Belirli bir konumdaki bir sanal makineyi başka bir konumdaki Kurtarma Hizmetlerine yönelik olarak yedekleyemezsiniz. Bu nedenle, yedeklenecek sanal makineler içeren her Azure konumu için, söz konusu konumda en az bir Kurtarma Hizmetleri kasası mevcut olmalıdır.

1. Sol taraftaki menüden **Tüm hizmetler**’i seçin ve hizmet listesinde *Kurtarma Hizmetleri* yazın. Siz yazarken kaynakların listesini filtrelenir. Listede Kurtarma Hizmetleri kasalarını gördüğünüzde Kurtarma Hizmetleri kasası menüsünü açmak için seçin.

    ![Kurtarma Hizmetleri Kasası menüsünü açma](./media/tutorial-backup-vm-at-scale/full-browser-open-rs-vault.png) <br/>

2. **Kurtarma Hizmetleri kasaları** menüsünde, **Ekle**’ye tıklayarak Kurtarma Hizmetleri kasası menüsünü açın.

    ![Kasa menüsünü açma](./media/tutorial-backup-vm-at-scale/provide-vault-detail-2.png)

3. Kurtarma Hizmetleri kasası menüsünde, 

    - **Ad** alanına *myRecoveryServicesVault* yazın.
    - **Abonelik** bölümünde geçerli abonelik kimliği görüntülenir. Ek abonelikleriniz varsa yeni kasa için başka bir abonelik seçebilirsiniz.
    - **Kaynak grubu** için **Var olanı kullan**’ı seçin ve *myResourceGroup* seçeneğini belirleyin. *myResourceGroup* yoksa **Yeni oluştur**’u seçin ve *myResourceGroup* yazın.
    - **Konum** açılan menüsünden *Batı Avrupa*’yı seçin.
    - Kurtarma Hizmetleri kasanızı oluşturmak için **Oluştur**’a tıklayın.

Kurtarma Hizmetleri kasası, korunan sanal makinelerle aynı konumda olmalıdır. Birden çok bölgede sanal makineniz varsa her bölgede bir Kurtarma Hizmetleri kasası oluşturun. Bu öğreticide, *Batı Avrupa*, *myVM*’nin (hızlı başlangıçla oluşturulan sanal makine) oluşturulduğu konum olduğu için burada bir Kurtarma Hizmetleri kasası oluşturulmaktadır.

Kurtarma Hizmetleri kasasının oluşturulması birkaç dakika sürebilir. Portalın sağ üst kısmından durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra Kurtarma Hizmetleri kasaları listesinde görünür.

Kurtarma Hizmetleri kasası oluşturduğunuzda kasa varsayılan olarak coğrafi olarak yedekli depolama işlevine sahip olur. Veri esnekliği sağlamak için coğrafi olarak yedekli depolama, verileri iki Azure bölgesi arasında birden çok kez çoğaltır.

## <a name="set-backup-policy-to-protect-vms"></a>VM’leri korumak için yedekleme ilkesi ayarlama

Kurtarma Hizmetleri kasasını oluşturduktan sonraki adım, veri türü için kasanın yapılandırılması ve yedekleme ilkesinin ayarlanmasıdır. Yedekleme ilkesi, kurtarma noktalarının ne sıklıkta ve ne zaman alınacağına yönelik zamanlamadır. İlke aynı zamanda kurtarma noktaları için bekletme aralığını içerir. Bu öğreticide, işletmenizin bir otel, stadyum, restoran ve diğer olanaklara sahip bir spor tesisi olduğunu ve verileri sanal makinelerde koruduğunuzu varsayalım. Aşağıdaki adımlarda, finansal veriler için bir yedekleme ilkesi oluşturulmaktadır.

1. Kurtarma Hizmetleri kasalarının listesinden, ilgili panoyu seçmek için **myRecoveryServicesVault** seçeneğini belirleyin.

   ![Senaryo menüsünü açma](./media/tutorial-backup-vm-at-scale/open-vault-from-list.png)

2. Kasa panosu menüsünden Yedekleme menüsünü açmak için **Yedekle** seçeneğine tıklayın.

3. Yedekleme Hedefi menüsünde, **İş yükünüz nerede çalışıyor?** açılan menüsünden *Azure*’ı seçin. **Neleri yedeklemek istiyorsunuz?** açılan menüsünde *Sanal makine*’yi seçin ve **Yedekle**’ye tıklayın.

    Bu eylemler, Kurtarma Hizmetleri kasasını bir sanal makine ile etkileşime geçmesi için hazırlar. Kurtarma Hizmetleri kasaları her gün bir geri yükleme noktası oluşturan ve geri yükleme noktalarını 30 gün boyunca tutan, varsayılan bir ilkeye sahiptir.

    ![Senaryo menüsünü açma](./media/tutorial-backup-vm-at-scale/backup-goal.png)

4. Yeni bir ilke oluşturmak için Yedekleme ilkesi menüsünde, **Yedekleme ilkesi seçin** açılan menüsünden *Yeni Oluştur* seçeneğini belirleyin.

    ![İş yükünü seçme](./media/tutorial-backup-vm-at-scale/create-new-policy.png)

5. **Yedekleme ilkesi** menüsünde, **İlke Adı** türü için *Finans* yazın. Yedekleme ilkesi için aşağıdaki değişiklikleri girin: 
    - **Yedekleme sıklığı** için saat dilimini *Merkezi Saat* olarak ayarlayın. Spor tesisi Teksas’ta olduğundan, tesisin sahibi zamanlamanın yerel olarak ayarlanmasını istemektedir. Yedekleme sıklığını Günlük 03:30 olarak ayarlanmış halde bırakın.
    - **Günlük yedekleme noktası bekletmesi** için süreyi 90 gün olarak ayarlayın.
    - **Haftalık yedekleme noktası bekletmesi** için *Pazartesi* geri yükleme noktasını kullanın ve 52 hafta boyunca tutun.
    - **Aylık yedekleme noktası bekletmesi** için her ayın İlk Pazar günündeki geri yükleme noktasını kullanın ve 36 ay boyunca tutun.
    - **Yıllık yedekleme noktası bekletmesi** seçeneğinin işaretini kaldırın. Finans lideri, verileri 36 aydan uzun süre tutmak istememektedir.
    - Yedekleme ilkesini oluşturmak için **Tamam**’a tıklayın.

    ![İş yükünü seçme](./media/tutorial-backup-vm-at-scale/set-new-policy.png) 

    Yedekleme ilkesini oluşturduktan sonra ilkeyi sanal makineler ile ilişkilendirin.

6. **Sanal makine seç** iletişim kutusunda *myVM*’yi seçip **Tamam**’a tıklayarak yedekleme ilkesini sanal makinelere dağıtın. 

    Aynı konumda olan ve bir yedekleme ilkesi ile ilişkili olmayan tüm sanal makineler görünür. *myVMH1* ve *myVMR1* seçilerek *Finans* ilkesiyle ilişkilendirilir.

    ![İş yükünü seçme](./media/tutorial-backup-vm-at-scale/choose-vm-to-protect.png) 

    Dağıtım tamamlandığında, bu dağıtımın başarıyla tamamlandığına ilişkin bir bildirim alırsınız.

## <a name="initial-backup"></a>İlk yedekleme

Kurtarma Hizmetleri kasaları için yedeklemeyi etkinleştirdiniz, ancak ilk yedekleme oluşturulmadı. Verilerinizin korunabilmesi açısından ilk yedeklemeyi tetiklemek, olağanüstü durum kurtarma konusunda en iyi uygulamadır. 

İsteğe bağlı yedekleme işi çalıştırmak için:

1. Kasa panosunda, Yedekleme Öğeleri menüsünü açmak için **Yedekleme Öğeleri** altındaki **3** sayısına tıklayın.

    ![Ayarlar simgesi](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)

    **Yedekleme Öğeleri** menüsü açılır.

2. **Yedekleme Öğeleri** menüsünde, kasayla ilişkili sanal makinelerin listesini açmak için **Azure Sanal Makine** seçeneğine tıklayın.

    ![Ayarlar simgesi](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    **Yedekleme Öğeleri** listesi açılır.

    ![Tetiklenmiş yedekleme işi](./media/tutorial-backup-vm-at-scale/initial-backup-context-menu.png)

3. **Yedekleme Öğeleri** listesinde üç noktaya **...** tıklayarak Bağlam menüsünü açın.

4. Bağlam menüsünde **Şimdi yedekle** seçeneğine tıklayın.

    ![Bağlam menüsü](./media/tutorial-backup-vm-at-scale/context-menu.png)

    Şimdi Yedekle menüsü açılır.

5. Şimdi Yedekle menüsünde, kurtarma noktasının tutulacağı son günü girin ve **Yedekle** seçeneğine tıklayın.

    ![Şimdi Yedekle kurtarma noktasının korunduğu son günü ayarlayın](./media/tutorial-backup-vm-at-scale/backup-now-short.png)

    Dağıtım bildirimleri, yedekleme işinin tetiklendiğini ve Yedekleme işleri sayfasında işin ilerleme durumunu izleyebileceğinizi bilmenizi sağlar. Sanal makinenizin boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.

    İlk yedekleme işi tamamlandığında Yedekleme işi menüsünde işin durumunu görebilirsiniz. İsteğe bağlı yedekleme işi, *myVM* için ilk yükleme noktasını oluşturmuştur. Diğer sanal makineleri yedeklemek istiyorsanız, her sanal makine için bu adımları yineleyin. 

    ![Yedekleme İşleri kutucuğu](./media/tutorial-backup-vm-at-scale/initial-backup-complete.png)
  
## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğreticilerle çalışmaya devam etmeyi planlıyorsanız bu öğreticide oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız Azure portalında bu öğretici tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. **myRecoveryServicesVault** panosunda, Yedekleme Öğeleri menüsünü açmak için **Yedekleme Öğeleri** altındaki **3** sayısına tıklayın.

    ![Ayarlar simgesi](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)


2. **Yedekleme Öğeleri** menüsünde, kasayla ilişkili sanal makinelerin listesini açmak için **Azure Sanal Makine** seçeneğine tıklayın.

    ![Ayarlar simgesi](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    **Yedekleme Öğeleri** listesi açılır.

3. **Yedekleme Öğeleri** menüsünde, Bağlam menüsünü açmak için üç noktaya tıklayın.

    ![Ayarlar simgesi](./media/tutorial-backup-vm-at-scale/context-menu-to-delete-vm.png)

4. Bağlam menüsünde, Yedeklemeyi Durdur menüsünü açmak için **Yedeklemeyi durdur** seçeneğini belirleyin. 

    ![Ayarlar simgesi](./media/tutorial-backup-vm-at-scale/context-menu-for-delete.png)

5. **Yedeklemeyi Durdur** menüsünde, üst taraftaki açılan menüyü seçip **Yedekleme Verilerini Sil** seçeneğini belirleyin.

6. **Yedekleme öğesinin adını yazın** iletişim kutusuna *myVM* yazın.
 
7. Yedekleme öğesi doğrulandıktan sonra (bir onay işareti görünür), **Yedeklemeyi durdur** düğmesi etkinleşir. İlkeyi durdurup, geri yükleme noktalarını silmek için **Yedeklemeyi Durdur** seçeneğine tıklayın. 

    ![Kasayı silmek için Yedeklemeyi durdur seçeneğine tıklayın](./media/tutorial-backup-vm-at-scale/provide-reason-for-delete.png).

8. **myRecoveryServicesVault** menüsünde, **Sil** seçeneğine tıklayın.

    ![Kasayı silmek için Yedeklemeyi durdur seçeneğine tıklayın](./media/tutorial-backup-vm-at-scale/deleting-the-vault.png)

    Kasa silindikten sonra, Kurtarma Hizmetleri kasalarının listesine geri dönersiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalını kullanarak şu işlemleri gerçekleştirdiniz:

> [!div class="checklist"]
> * Kurtarma Hizmetleri kasası oluşturma
> * Kasayı sanal makineleri koruyacak şekilde ayarlama
> * Özel yedekleme ve bekletme ilkesi oluşturma
> * Birden çok sanal makineyi korumak için ilkeyi atama
> * Sanal makineler için isteğe bağlı bir yedeklemeyi tetikleme

Bir Azure sanal makinesini diskten geri yüklemek için sonraki öğreticiye devam edin. 

> [!div class="nextstepaction"]
> [CLI kullanarak VM’leri geri yükleme](./tutorial-restore-disk.md)
