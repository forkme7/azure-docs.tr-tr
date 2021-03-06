---
title: Azure Depolama Gezgini’nde Azure Data Lake Store kaynaklarını yönetme
description: Azure Depolama Gezgini’nde Azure Data Lake Store verilerinize ve kaynaklarınıza nasıl erişileceğini ve bunların nasıl yönetileceğini öğreneceksiniz
Keywords: Azure Data Lake Store, Azure Storage Explorer
services: Data Lake Store
documentationcenter: ''
author: jejiang
manager: DJ
editor: Jenny Jiang
ms.assetid: ''
ms.service: Data Lake Store
ms.custom: Azure Data Lake Store
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 02/05/2018
ms.author: jejiang
ms.openlocfilehash: 396452faf36bc43a185fd6492f952b7bfd9511cc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="manage-azure-data-lake-store-resources-by-using-storage-explorer"></a>Depolama Gezgini’ni kullanarak Azure Data Lake Store kaynaklarını yönetme

[Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview), büyük miktarlarda metin veya ikili veriler gibi yapılandırılmamış verileri depolamak için bir hizmettir. HTTP veya HTTPS aracılığıyla verilere herhangi bir yerden erişebilirsiniz. Azure Depolama Gezgini’de Data Lake Store, blob ve kuyruk gibi diğer Azure varlıklarının yanı sıra Data Lake Store verileri ve kaynaklarına erişmenize ve bunları yönetmenize olanak sağlar. Artık farklı Azure varlıklarını aynı aracı kullanarak tek bir yerde yönetebilirsiniz.

Başka bir avantajı, Data Lake Store verilerini yönetmek için abonelik iznine sahip olmanızın gerekmemesidir. Depolama Gezgini’nde birisi izin verdiği sürece **Yerel ve Eklenmiş** düğümüne Data Lake Store yolunu ekleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu makaledeki adımları tamamlayabilmeniz için şu önkoşullar gereklidir:

*   Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial).
*   Bir Azure Data Lake Store hesabı. Hesap oluşturmaya ilişkin yönergeler için bkz. [Azure Data Lake Store kullanmaya başlama](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

## <a name="install-storage-explorer"></a>Depolama Gezgini’ni yükleme

[Ürün web sayfasından](https://azure.microsoft.com/features/storage-explorer/) en yeni Azure Depolama Gezgini bileşenlerini yükleyin. Yükleme Windows, Linux ve Mac sürümlerini destekler.

## <a name="connect-to-an-azure-subscription"></a>Bir Azure aboneliğine Bağlanma

1. Depolama Gezgini’nde soldaki eklenti simgesini seçin.
       
   ![Eklenti simgesi](./media/data-lake-store-in-storage-explorer/plug-in-icon.png)
 
2. **Azure Hesabı Ekle**’yi seçip **Oturum açın**’a tıklayın.

   !["Azure Depolama’ya Bağlan" iletişim kutusu](./media/data-lake-store-in-storage-explorer/connect-to-azure-subscription.png)

2. **Hesabınızda oturum açın** iletişim kutusuna Azure kimlik bilgilerinizi girin.

    ![Azure oturum açma İletişim Kutusu](./media/data-lake-store-in-storage-explorer/sign-in.png)

3. Listeden aboneliğinizi seçip **Uygula**’yı seçin.

    ![Abonelik bilgileri ve "Uygula" düğmesi](./media/data-lake-store-in-storage-explorer/apply-subscription.png)

    **GEZGİN** bölmesi güncelleştirilir ve seçili abonelikteki hesapları gösterir.

    ![Hesap listesi](./media/data-lake-store-in-storage-explorer/account-list.png)

Azure Data Lake Store’u Azure aboneliğinize başarıyla bağladınız.

## <a name="connect-to-data-lake-store"></a>Data Lake Store’a bağlanma
Birisi kaynakların URI’sini size verirse aboneliğinizde mevcut olmayan kaynaklara erişebilirsiniz. Bu durumda, oturum açtıktan sonra URI’yi kullanarak Data Lake Store’a bağlanabilirsiniz.
1. Depolama Gezgini'ni açın.
2. Sol bölmede, **Yerel ve Ekli** öğesini genişletin.
3. **Data Lake Store**’a sağ tıklayıp **Data Lake Store’a bağlan** seçeneğini belirleyin.

      ![Kısayol menüsündeki "Data Lake Store’a bağlan"](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach.png)

4. URI’yi girin. Araç, yeni girdiğiniz URL’nin konumuna gider.

      ![URI’yi girmek için metin kutusuyla birlikte "Data Lake Store’a bağlan" iletişim kutusu](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

      ![Data Lake Store’a bağlantı sonucu](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-attach-finish.png)

## <a name="view-an-azure-data-lake-store-accounts-contents"></a>Azure Data Lake Store hesabının içeriklerini görüntüleme
Bir Azure Data Lake Store hesabının kaynakları klasörler ve dosyaları içerir.

Aşağıdaki adımlar, Depolama Gezgini’ndeki bir Data Lake Store hesabının içeriklerini görüntüleme işlemini göstermektedir:

1. Depolama Gezgini'ni açın.
2. Sol bölmede, görüntülemek istediğiniz Azure Data Lake Store hesabını içeren aboneliği genişletin.
3. **Data Lake Store**’u genişletin.
4. Görüntülemek istediğiniz Azure Data Lake Store hesabı düğümüne sağ tıklayın ve **Aç**’ı seçin. Data Lake Store hesabını çift tıklayarak da açabilirsiniz. 
   
   Ana bölme, Data Lake Store hesabının içeriklerini gösterir.

   ![Klasörler listesini içeren ana bölme](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-toolbar-mainpane.png) 

## <a name="manage-resources-in-azure-data-lake-store"></a>Azure Data Lake Store’da kaynakları yönetme

Aşağıdaki işlemleri gerçekleştirerek Azure Data Lake Store kaynaklarını yönetebilirsiniz:
*   Birden fazla Azure Data Lake Store hesabında Data Lake Store kaynaklarına göz atma.  
*   Data Lake Store’a doğrudan bağlanmak ve Data Lake Store’u doğrudan yönetmek için bir bağlantı dizesi kullanma. 
*   **Yerel ve Eklenmiş** bölümündeki bir ACL üzerinden diğer kullanıcılar tarafından paylaşılan Data Lake Store kaynaklarını görüntüleme.
*   Dosya ve klasör CRUD işlemlerini gerçekleştirin: özyinelemeli klasör ve çoklu seçimli dosya desteği. 
*   En son konumlara hızlıca erişmek için bir klasörü sürükleyip bırakma ve ekleme. Bu işlem, masaüstü Dosya Gezgini deneyimini yansıtır. 
*   Depolama Gezgini’nde bir tıklamayla Azure Data Lake köprüsünü kopyalayıp açma. 
*   Etkinlik durumunu görüntülemek için sağ alt bölmede Etkinlik Günlüğünü görüntüleme.
*   Klasör istatistiklerini ve dosya özelliklerini görüntüleme.

## <a name="manage-resources-in-azure-storage-explorer"></a>Azure Depolama Gezgini’nde kaynakları yönetme
Azure Data Lake Store hesabı oluşturduktan sonra aşağıdakileri yapabilirsiniz:

* Klasörleri ve dosyaları karşıya yükleme, klasörleri ve dosyaları indirme ve kaynakları yerel bilgisayarınızda açma.
* **Hızlı Erişim**’e sabitleme, yeni bir klasör oluşturma, bir URL’yi kopyalama ve tümünü seçme.
* Kopyalayıp yapıştırma, yeniden adlandırma, silme, klasör istatistikleri alma ve yenileme.

Aşağıdaki öğelerde bir Azure Data Lake Store hesabındaki kaynakların nasıl yönetileceği gösterilmektedir. Gerçekleştirmek istediğiniz görev için adımları izleyin.

### <a name="upload-files"></a>Dosyaları karşıya yükleme

1. Ana bölmedeki araç çubuğunda **Karşıya Yükle** öğesini ve ardından açılan menüdeki **Dosyaları Karşıya Yükle** öğesini seçin.

   !["Dosyaları Karşıya Yükle" menü öğesi](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-menu.png) 

2. **Karşıya yüklenecek dosyaları seçin** iletişim kutusunda, karşıya yüklemek istediğiniz dosyaları seçin.

   ![Dosyaları karşıya yükleme iletişim kutusu](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-dialog.png)

3. Karşıya yüklemeyi başlatmak için **Aç**’ı seçin.

### <a name="upload-a-folder"></a>Bir klasörü karşıya yükleme

1. Ana bölmedeki araç çubuğunda **Karşıya Yükle** öğesini ve ardından açılan menüdeki **Dosyaları Karşıya Yükle** öğesini seçin.

   !["Klasörü Karşıya Yükle" menü öğesi](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-menu.png) 
     
2. **Karşıya yüklenecek klasörü seçin** iletişim kutusunda, karşıya yüklemek istediğiniz bir klasörü seçin. Ardından **Klasör Seç**’e tıklayın.

   ![Klasörleri karşıya yükleme iletişim kutusu](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-dialog.png)      

   Karşıya yükleme başlatılır.

   ![Karşıya yükleme devam ederken iletişim kutusu](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-drag.png) 

> [!NOTE] 
> Karşıya yüklemeyi başlatmak için doğrudan yerel bilgisayarda klasör ve dosyaları sürükleyebilirsiniz. 
       
### <a name="download-folders-or-files-to-your-local-computer"></a>Klasörleri veya dosyaları yerel bilgisayarınıza indirme

1. İndirmek istediğiniz klasörleri veya dosyaları seçin.
2. Ana bölmedeki araç çubuğunda **İndir**’i seçin.
3. **İndirilen dosyaların kaydedileceği klasörü seçin** iletişim kutusunda, konumu ve adı belirtin.
4. **Kaydet**’i seçin.

### <a name="open-a-folder-or-file-from-your-local-computer"></a>Yerel bilgisayarınızdan klasör veya dosya açma

1. Açmak istediğiniz klasörü veya dosyayı seçin.
2. Ana bölmedeki araç çubuğunda **Aç**’ı seçin. Veya seçilen klasöre ya da dosyaya sağ tıklayıp kısayol menüsünden **Aç**’ı seçin.

Dosya indirilir ve temel alınan dosya türü ile ilişkili uygulama kullanılarak açılır. Alternatif olarak, klasör ana bölmede açılır.

![Açılan dosya](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-open.png) 

### <a name="copy-folders-or-files-to-the-clipboard"></a>Dosya veya klasörleri panoya kopyalama

1. Kopyalamak istediğiniz klasörleri veya dosyaları seçin.
2. Ana bölmedeki araç çubuğunda **Kopyala**’yı seçin. Veya seçilen klasörlere ya da dosyalara sağ tıklayıp kısayol menüsünden **Kopyala**’yı seçin.
3. Sol bölmede başka bir Data Lake Store hesabına gidin ve ana bölmede görüntülemek için çift tıklayın.
4. Ana bölmedeki araç çubuğunda **Yapıştır**’ı seçerek bir kopyasını oluşturun. Veya hedefin kısayol menüsünden **Yapıştır**’ı seçin.

![Klasörü kopyalama seçimleri](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-copy-paste.png)

> [!NOTE] 
> Depolama türleri arasında kopyalama/yapıştırma işlemleri desteklenmez. Data Lake Store klasörlerini veya dosyalarını kopyalayıp başka bir Data Lake Store hesabına yapıştırabilirsiniz. Ancak Data Lake Store klasör ve dosyalarını Azure Blob depolamaya *kopyalayıp yapıştıramaz* veya bunun tersini gerçekleştiremezsiniz.
> 
> Kopyalama/yapıştırma işlemi, klasör ve dosyaları yerel bilgisayara indirip ardından hedef konuma yükleyerek çalışır. Araç, eylemi arka uçta *gerçekleştirmez*. Büyük dosyalarda kopyalama/yapıştırma işlemi yavaştır. Yüksek performanslı dosya kopyalama/taşıma iyileştirmeleri devam ediyor.

### <a name="delete-folders-or-files"></a>Klasörleri veya dosyaları silme

1. Silmek istediğiniz klasörleri veya dosyaları seçin.
2. Ana bölmedeki araç çubuğunda **Sil**’i seçin. Veya seçilen klasörlere veya dosyalara sağ tıklayıp kısayol menüsünden **Sil**’i seçin.
3. Onay iletişim kutusunda **Evet**’i seçin.

![Klasörü silme seçimleri](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-delete.png)

### <a name="pin-to-quick-access"></a>Hızlı Erişime sabitleme

1. Sabitlemek istediğiniz klasörü seçin.
2. Ana bölmedeki araç çubuğunda **Hızlı erişime sabitle**’yi seçin.

   Sol bölmede, seçili klasör **Hızlı Erişim** düğümüne eklenir.

   ![Bir klasörü "Hızlı Erişim" düğümüne sabitleme seçimleri](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-quick-access.png)

Bir klasörü **Hızlı Erişim** düğümüne sabitledikten sonra kolayca kaynaklara erişebilirsiniz.

### <a name="use-deep-links"></a>Ayrıntılı bağlantıları kullanma
Bir URL’niz varsa, URL’yi Dosya Gezgini’nde veya tarayıcıda adres yoluna girebilirsiniz. Daha sonra URL’nin konumuna gitmek için Storage Explorer.exe otomatik olarak çalıştırılır.

![Dosya Gezgininde ayrıntılı bağlantı](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-deep-link.png)


## <a name="next-steps"></a>Sonraki adımlar
* [En son Depolama Gezgini yayın notlarını ve videolarını](http://www.storageexplorer.com) görüntüleyin.
* [Azure Depolama Gezgini’nde Azure Cosmos DB’yi yönetmeyi](https://docs.microsoft.com/azure/cosmos-db/storage-explorer) öğrenin.
* [Depolama Gezgini ile çalışmaya başlayın](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).
* [Azure Data Lake Store ile çalışmaya başlayın](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview).
* [Azure Depolama Gezgini’nde Azure Cosmos DB’yi kullanma hakkındaki YouTube videosunu](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be) izleyin.
