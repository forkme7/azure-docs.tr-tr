---
title: Azure Hdınsight Linux kümelerinde presto yükleme | Microsoft Docs
description: Presto ve Airpal betik eylemleri kullanarak Linux tabanlı Hdınsight Hadoop kümeleri üzerinde nasıl yükleneceğini öğrenin.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: 32b7925b7414f00dfdd7d5c8a45b3601bf58942e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a>Yükleme ve Presto Hdınsight Hadoop kümeleri kullanma

Bu belgede, Presto Hdınsight Hadoop kümeleri üzerinde betik eylemi kullanarak yüklemeyi öğrenin. Ayrıca varolan bir Presto Hdınsight kümesinde Airpal yüklemeyi öğrenin.

> [!IMPORTANT]
> Bu belgede yer alan adımlar gerektiren bir **Hdınsight 3.5 Hadoop kümesi** Linux kullanır. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz: [Hdınsight sürümleri](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Presto nedir?
[Presto](https://prestodb.io/overview.html) bir hızlı dağıtılmış SQL sorgu alt büyük veri yapısıdır. Presto etkileşimli veri petabaytlarca sorgulamak için uygundur. Presto ve nasıl çalıştıkları bileşenler hakkında daha fazla bilgi için bkz: [Presto kavramları](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]
> Hdınsight kümesi ile sağlanan bileşenler tam olarak desteklenir ve Microsoft Support yalıtmak ve bu bileşenleri ilgili sorunları gidermek için yardımcı olur.
> 
> Presto gibi özel bileşenleri daha fazla sorunu gidermeye yardımcı olmak üzere ticari koşulların elverdiği oranda makul desteği alabilirsiniz. Bu sorunu çözmek veya bu teknoloji derin uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları devreye isteyen neden olabilir. Örneğin, olduğu gibi kullanılabilecek birçok topluluk siteleri vardır: [Hdınsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Apache projeleri proje siteleri de [ http://apache.org ](http://apache.org), örneğin: [Hadoop](http://hadoop.apache.org/).
> 
> 


## <a name="install-presto-using-script-action"></a>Betik eylemi kullanarak Presto yükleyin

Bu bölümde Azure Portalı'nı kullanarak yeni bir küme oluştururken, örnek komut dosyası kullanma hakkında yönergeler sağlar. 

1. ' Ndaki adımları kullanarak bir küme hazırlama Başlat [sağlama Linux tabanlı Hdınsight kümeleri](hdinsight-hadoop-create-linux-clusters-portal.md). Küme kullanarak oluşturduğunuz emin olun **özel** küme oluşturma akış. Küme, aşağıdaki gereksinimleri karşılaması gerekir.

    * Bir Hadoop kümesine Hdınsight sürüm 3.5 ile olmalıdır.

    * Veri deposu olarak Azure Storage kullanmanız gerekir. Azure Data Lake Store depolama seçeneği olarak kullanan bir kümede presto kullanarak henüz desteklenmiyor. 

    ![Özel seçenekleri kullanarak Hdınsight kümesi oluşturma](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. Üzerinde **Gelişmiş ayarları** alanında **betik eylemleri**ve aşağıdaki bilgileri sağlayın:
   
   * **AD**: betik eylemi için kolay bir ad girin.
   * **Bash betiği URI’si**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`
   * **HEAD**: Bu seçeneği işaretleyin.
   * **ÇALIŞAN**: Bu seçeneği işaretleyin.
   * **ZOOKEEPER**: Bu onay kutusunu temizleyin
   * **PARAMETRELERİ**: Bu alanı boş bırakın


3. Ekranın alt kısmındaki **betik eylemleri** alanında tıklatın **seçin** düğmesi yapılandırmayı kaydetmek için. Son olarak, tıklatın **seçin** düğmesini alt kısmındaki **Gelişmiş ayarları** alan yapılandırma bilgilerini kaydedin.

4. Bölümünde açıklandığı gibi küme hazırlama devam [sağlama Linux tabanlı Hdınsight kümeleri](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]
    > Azure PowerShell, Azure CLI, Hdınsight .NET SDK veya Azure Resource Manager şablonları betik eylemleri uygulamak için de kullanılabilir. Zaten çalışan kümeler için betik eylemleri de uygulayabilirsiniz. Daha fazla bilgi için bkz: [özelleştirme Hdınsight betik eylemleri ile kümeleri](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Hdınsight ile presto kullanma

Presto bir Hdınsight kümesi çalışmak için aşağıdaki adımları kullanın:

1. SSH kullanarak HDInsight kümesine bağlanma:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).
     

2. Aşağıdaki komutu kullanarak Presto Kabuğu'nu başlatın.
   
        presto --schema default

3. Bir örnek tablo sorgusu **hivesampletable**, varsayılan olarak tüm Hdınsight kümeleri üzerinde kullanılabilir olduğu.
   
        select count (*) from hivesampletable;
   
    Varsayılan olarak, [Hive](https://prestodb.io/docs/current/connector/hive.html) ve [TPCH](https://prestodb.io/docs/current/connector/tpch.html) bağlayıcıları Presto için zaten yapılandırılmış. Hive bağlayıcı Hive tüm tablolardan Presto içinde otomatik olarak görünür olması için varsayılan olarak yüklenen Hive yükleme kullanmak için yapılandırılır.

    Daha fazla bilgi için bkz: [Presto belgelerine](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Airpal Presto ile kullanma

[Airpal](https://github.com/airbnb/airpal#airpal) Presto için bir açık kaynak web tabanlı sorgu arabirimdir. Airpal hakkında daha fazla bilgi için bkz: [Airpal belgelerine](https://github.com/airbnb/airpal#airpal).

Airpal kenar düğümüne yüklemek için aşağıdaki adımları kullanın:

1. SSH kullanarak, Presto yüklü olduğu Hdınsight küme headnode için Bağlan:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Bağlandıktan sonra aşağıdaki komutu çalıştırın.

        sudo slider registry  --name presto1 --getexp presto 
   
    Aşağıdaki JSON benzer bir çıktı görürsünüz:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. Çıktıdan değeri Not **değeri** özelliği. Bu değer, küme edgenode Airpal yüklenirken gerekir. Yukarıdaki çıktısını ihtiyacınız olacak değerdir **10.0.0.12:9090**.

4. Şablon kullanmak **[burada](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** bir Hdınsight kümesi edgenode oluşturma ve aşağıdaki ekran görüntüsünde gösterildiği gibi değerlerini belirtin.

    ![Hdınsight yükle Airpal Presto küme](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. **Satın al**’a tıklayın.

6. Küme yapılandırmasında değişiklikler uygulandıktan sonra aşağıdaki adımları kullanarak Airpal web arabirimi erişebilirsiniz.

    1. Küme iletişim kutusundan tıklatın **uygulamaları**.

        ![Hdınsight başlatılırken Airpal Presto küme](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    2. Gelen **yüklü uygulamalar** alanında tıklatın **Portal** airpal karşı.

        ![Hdınsight başlatılırken Airpal Presto küme](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    3. İstendiğinde, Hdınsight Hadoop kümesi oluşturulurken belirtilen yönetici kimlik bilgilerini girin.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>Hdınsight kümesinde Presto yüklemeyi özelleştirme

Yüklemeyi özelleştirmek için aşağıdaki adımları kullanın:

1. SSH kullanarak, Presto yüklü olduğu Hdınsight küme headnode için Bağlan:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Yapılandırma değişiklik dosyasında `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. Presto yapılandırma hakkında daha fazla bilgi için bkz: [YARN tabanlı kümeler için Presto yapılandırma](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).

3. Durdurun ve Presto geçerli çalışan örneği sonlandırın.

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. Presto yeni bir örneğini özelleştirme ile başlatın.

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. Presto Düzenleyicisi adresini not ve hazır olması için yeni örnek bekleyin.


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Kıyaslama verileri Presto çalıştırmak Hdınsight kümeleri oluşturma

TPC DS birçok karar destek büyük veri sistemlerini sistemlerle performansını ölçmek için endüstri standardıdır. Presto veriler oluşturulmasına neden ve nasıl kendi Hdınsight Kıyaslama verilerle karşılaştırır değerlendirmek için kullanabilirsiniz. Daha fazla bilgi için bkz: [burada](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="see-also"></a>Ayrıca bkz.
* [Yükleme ve Hdınsight kümelerinde ton kullanma](hdinsight-hadoop-hue-linux.md). Hue web kullanıcı arabirimini oluşturmak, çalıştırmak, kolaylaştırır ve Pig ve Hive işleri bulunuyor.

* [Hdınsight kümelerinde Giraph yükleme](hdinsight-hadoop-giraph-install-linux.md). Küme özelleştirme Giraph Hdınsight Hadoop kümelerine yüklemek için kullanın. Giraph Hadoop kullanarak işleme grafik işlemleri yapmanıza olanak tanır ve Azure Hdınsight ile kullanılabilir.

* [Hdınsight kümelerinde Solr yükleme](hdinsight-hadoop-solr-install-linux.md). Küme özelleştirme Solr Hdınsight Hadoop kümelerine yüklemek için kullanın. Solr, depolanan verileri güçlü arama işlemleri olanak sağlar.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
