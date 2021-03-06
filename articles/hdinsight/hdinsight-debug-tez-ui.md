---
title: Tez UI Windows tabanlı Hdınsight ile - Azure kullanma | Microsoft Docs
description: Windows tabanlı Hdınsight hdınsight'ta Tez işlerinde hata ayıklamak için Tez UI kullanmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.assetid: a55bccb9-7c32-4ff2-b654-213a2354bd5c
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 4201fb76ef9b0e711fd48972db86c356d72e6671
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-the-tez-ui-to-debug-tez-jobs-on-windows-based-hdinsight"></a>Windows tabanlı Hdınsight üzerinde Tez işlerinde hata ayıklamak için Tez kullanıcı arabirimini kullanma
Tez UI Tez yürütme altyapısı olarak kullanan Hive işleri hata ayıklamak için kullanılabilir. Bir grafik bağlı öğelerinin her öğenin ayrıntısına ve istatistikleri ve günlük kaydı bilgilerini almak Tez kullanıcı Arabirimi iş visualizes.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Windows kullanan bir Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Önkoşullar
* Bir Windows tabanlı Hdınsight kümesi. Yeni küme oluşturma adımları için bkz: [Windows tabanlı Hdınsight kullanmaya başlama](hdinsight-hadoop-tutorial-get-started-windows.md).

  > [!IMPORTANT]
  > Tez UI yalnızca 8 Şubat 2016 sonrasında oluşturulan Windows tabanlı Hdınsight kümeleri üzerinde kullanılabilir.
  >
  >
* Bir Windows tabanlı uzak masaüstü istemcisi.

## <a name="understanding-tez"></a>Tez anlama
Tez Hadoop verileri işlemek için genişletilebilir bir çerçeve ve geleneksel MapReduce işleme büyük hızlarından sağlar. Tez Hive sorgusu bir parçası olarak aşağıdaki metni dahil olmak üzere etkinleştirebilirsiniz:

    set hive.execution.engine=tez;

Tez yönlendirilmiş Çevrimsiz grafik (yürütme iş tarafından gereken eylemlerin sırasını açıklayan DAG) oluşturur. Tek tek Eylemler köşeleri olarak adlandırılır ve genel iş parçası yürütün. Gerçek yürütme köşe tarafından açıklanan iş bir görev çağrılır ve kümedeki birden çok düğüm arasında dağıtılmış.

### <a name="understanding-the-tez-ui"></a>Tez UI anlama
Tez kullanıcı Arabirimi, bir web sayfası Tez kullanan işlemler hakkında bilgi sağlar ' dir. Aşağıdaki senaryolarda yararlı bilgiler teklif edebilir:

* Uzun süre çalışan izleme harita ilerlemesini görüntüleme, işler ve görevler azaltır.
* İşleme nasıl geliştirilmiş veya neden başarısız öğrenmek başarılı veya başarısız işlemler için geçmiş verileri analiz etme.

## <a name="generate-a-dag"></a>Bir DAG oluştur
Tez UI geçmişte Tez Altyapısı şu anda çalışıyor ya da bırakıldı kullanan çalışan bir iş, verileri içerir. Basit Hive sorguları genellikle Tez kullanmadan çözülebilir. Filtreleme yapan daha karmaşık sorgular gruplandırma, sıralama, birleşimler, vb. Tez gerektirir.

Tez kullanan bir Hive sorgusu çalıştırmak için aşağıdaki adımları kullanın.

1. Bir web tarayıcısında gidin https://CLUSTERNAME.azurehdinsight.net, burada **CLUSTERNAME** Hdınsight kümenizin adıdır.
2. Sayfanın üstündeki menüsünden seçin **Hive Düzenleyicisi**. Aşağıdaki örnek sorgu içeren bir sayfa görüntülenir.

        Select * from hivesampletable

    Örnek sorgu silebilir ve aşağıdakiyle değiştirin.

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. Seçin **gönderme** düğmesi. **Proje oturumunu** sayfanın alt kısmına sorgu durumunu görüntüler. Durum değişikliklerini sonra **tamamlandı**seçin **ayrıntıları görüntüle** sonuçları görüntülemek için bağlantı. **İş çıktısı** aşağıdakine benzer olmalıdır:

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-the-tez-ui"></a>Tez kullanıcı arabirimini kullanma
> [!NOTE]
> Baş düğümler bağlanmak için Uzak Masaüstü'nü kullanmanız gerekir böylece Tez UI yalnızca küme baş düğümler masaüstünden kullanılabilir.
>
>

1. Gelen [Azure portal](https://portal.azure.com), Hdınsight kümenize seçin. Hdınsight dikey üstten seçin **Uzak Masaüstü** simgesi. Bu bağlantı Uzak Masaüstü dikey penceresinde görüntüler

    ![Uzak Masaüstü simgesi](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. Uzak Masaüstü dikey penceresinden seçin **Bağlan** küme baş düğümüne bağlanmak için. İstendiğinde, bağlantı kimliğini doğrulamak için küme Uzak Masaüstü kullanıcı adı ve parola kullanın.

    ![Uzak Masaüstü Bağlantısı simgesi](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > Uzak Masaüstü bağlantısı etkin değil, bir kullanıcı adı, parola ve sona erme tarihi sağlayın, sonra seçin **etkinleştirmek** Uzak Masaüstü'nü etkinleştirmek için. Etkinleştirildikten sonra bağlanmak için önceki adımları kullanın.
   >
   >
3. Bağlantı kurulduktan sonra Internet Explorer'ı Uzak Masaüstü'nü açın, sağ üst tarafındaki tarayıcı içinde dişli simgesini seçin ve ardından **Uyumluluk Görünümü Ayarları**.
4. Aşağıdan **Uyumluluk Görünümü Ayarları**, onay kutusunu temizleyin **görüntüleme intranet sitelerini Uyumluluk Görünümü'nde** ve **kullanım Microsoft Uyumluluk listeleri**ve ardından **Kapat**.
5. Internet Explorer'da göz http://headnodehost:8188/tezui/#/. Tez UI görüntüler.

    ![Tez kullanıcı Arabirimi](./media/hdinsight-debug-tez-ui/tezui.png)

    Tez UI yüklediğinde, çalışmakta olan ya da silinmiş Dag'leri listesini küme üzerinde çalışan bakın. Varsayılan görünüm DAG adı, kimliği, gönderen, durumu, başlangıç saati, bitiş zamanı, süresi, uygulama kimliği ve kuyruk içerir. Daha fazla sütun, sayfanın sağ taraftaki dişli simgesini kullanarak eklenebilir.

    Yalnızca bir giriş varsa, önceki bölümde çalıştırdığınız sorgu içindir. Birden çok girdi varsa, Dag'leri yukarıda alanlarında arama ölçütü girerek arayın, ardından isabet **Enter**.
6. Seçin **Dag adı** en son DAG girişi. Bu bağlantı, DAG hakkında bilgi içeren JSON dosyaları zip yükleme seçeneği yanı sıra DAG hakkında bilgi görüntüler.

    ![DAG ayrıntıları](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. Yukarıdaki **DAG ayrıntıları** DAG hakkındaki bilgileri görüntülemek için kullanılan birkaç bağlantılardır.

   * **DAG sayaçları** bu DAG sayaçları bilgilerini görüntüler.
   * **Grafik görünümü** bu DAG grafik gösterimi görüntüler.
   * **Tüm köşeleri** bu DAG köşeleri listesini görüntüler.
   * **Tüm Görevler** bu DAG tüm köşeleri görevlerde listesini görüntüler.
   * **Tüm TaskAttempts** görevleri çalıştırmak için bu DAG girişimleri hakkındaki bilgileri görüntüler.

     > [!NOTE]
     > Sütun görüntü köşeleri, görevleri ve TaskAttempts kaydırırsanız, görüntülemek için bağlantıları olduğuna dikkat edin **sayaçları** ve **görüntülemek veya günlükleri indirmek** her satır için.
     >
     >

     İşi ile hatası varsa, DAG ayrıntıları durumunu başarısız oldu, başarısız görev hakkında bilgi için bağlantılar ile birlikte görüntüler. Tanılama bilgileri olması görüntülenir DAG ayrıntıları.
8. Seçin **grafik görünümü**. Bu grafik gösterimi DAG görüntüler. Her köşe ilgili bilgileri görüntülemek için görünümünde üzerinden fare yerleştirebilirsiniz.

    ![Grafik görünümü](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. Bir köşe tıklatarak yükler **köşe ayrıntıları** bu öğe için. Tıklayın **harita 1** köşe bu öğenin ayrıntılarını görüntülemek için. Seçin **Onayla** Gezinti onaylamak için.

    ![Köşe ayrıntıları](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. Şimdi köşeleri ve görevlerle ilgili bağlantıları sayfanın üst kısmındaki gerektiğini unutmayın.

    > [!NOTE]
    > Bu sayfaya geri giderek de ulaşması **DAG ayrıntıları**, seçme **köşe ayrıntıları**ve ardından seçerek **harita 1** köşe.
    >
    >

    * **Köşe sayaçları** bu köşe sayaç bilgilerini görüntüler.
    * **Görevleri** bu köşe için görevleri görüntüler.
    * **Görev denemeleri** bu köşe için görevleri çalıştırma denemelerini hakkındaki bilgileri görüntüler.
    * **Kaynakları & iç havuzlar** bu köşe için iç havuzlar ve veri kaynakları görüntüler.

      > [!NOTE]
      > Olarak önceki menüsüyle, görevler, görev denemeleri ve kaynakları & Sinks__ her öğe için daha fazla bilgi için bağlantılar görüntülenecek sütun görüntü gezinebilirsiniz.
      >
      >
11. Seçin **görevleri**ve ardından adlı bir öğe seçin **00_000000**. Bu bağlantı görüntüler **görev ayrıntıları** bu görev için. Bu ekranda görüntüleyebileceğiniz **görev sayaçları** ve **görev denemeleri**.

    ![Görev ayrıntıları](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a>Sonraki Adımlar
Tez görünümü kullanmak öğrendiniz, daha fazla bilgi edinmek [hdınsight'ta Hive kullanarak](hadoop/hdinsight-use-hive.md).

Daha ayrıntılı Tez teknik bilgi için bkz: [Hortonworks Tez sayfanın](http://hortonworks.com/hadoop/tez/).
