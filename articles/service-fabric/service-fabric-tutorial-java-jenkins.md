---
title: Azure Service Fabric Java uygulaması için CI/CD ayarlama | Microsoft Docs
description: Bu öğreticide, bir Java Service Fabric uygulaması dağıtmak için Jenkins kullanarak sürekli tümleştirmenin nasıl ayarlanacağını öğreneceksiniz.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/26/2018
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: bd986b8741b55a10018f7400c9415e7aedfbf119
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-set-up-a-jenkins-environment-with-service-fabric"></a>Öğretici: Service Fabric ile Jenkins ortamı ayarlama
Bu öğretici, bir serinin beşinci kısmıdır. Yükseltmeleri uygulamanızda dağıtmak için Jenkins’in nasıl kullanılacağını gösterir. Bu öğreticide Service Fabric Jenkins eklentisi, uygulamayı bir kümede dağıtmak için Oylama uygulamasını barındıran bir Github deposuyla birlikte kullanılır. 

Serinin beşinci kısmında öğrenecekleriniz:
> [!div class="checklist"]
> * Makinenizde Service Fabric Jenkins kapsayıcısını dağıtma
> * Service Fabric’te dağıtım için Jenkins ortamını ayarlama
> * Uygulamanızı yükseltme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Java Service Fabric Güvenilir Hizmetler uygulaması derleme](service-fabric-tutorial-create-java-app.md)
> * [Yerel kümede uygulamayı dağıtma ve uygulamanın hatasını ayıklama](service-fabric-tutorial-debug-log-local-cluster.md)
> * [Azure kümesine uygulama dağıtma](service-fabric-tutorial-java-deploy-azure.md)
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-java-elk.md)
> * CI/CD ayarlama


## <a name="prerequisites"></a>Ön koşullar
- [Git indirmeleri sayfasından](https://git-scm.com/downloads) Git’i yerel bilgisayarınıza yükleyin. Git hakkında daha fazla bilgi için [Git belgelerini](https://git-scm.com/docs) okuyun.
- [Jenkins](https://jenkins.io/) ile çalışma hakkında bilgi sahibi olun.
- Bir [GitHub](https://github.com/) hesabı oluşturun ve GitHub’ın nasıl kullanılacağını öğrenin.
- Bilgisayarınıza [Docker](https://www.docker.com/community-edition)’ı yükleyin.

## <a name="pull-and-deploy-service-fabric-jenkins-container-image"></a>Service Fabric Jenkins kapsayıcı görüntüsünü çekme ve dağıtma
Jenkins’i bir Service Fabric kümesinin içinde veya dışında ayarlayabilirsiniz. Aşağıdaki yönergeler, sağlanan bir Docker görüntüsü kullanılarak bunun bir küme dışında nasıl ayarlanacağını göstermektedir. Ancak önceden yapılandırılmış bir Jenkins derleme ortamı da kullanılabilir. Aşağıdaki kapsayıcı görüntüsü, Service Fabric eklentisi yüklenmiş şekilde gelir ve Service Fabric ile hemen kullanıma hazırdır. 

1. Service Fabric Jenkins kapsayıcı görüntüsünü çekin: ``docker pull rapatchi/jenkins:v10``. Bu görüntü, Service Fabric Jenkins eklentisi önceden yüklenmiş şekilde gelir.

2. Sertifikalarınızın yerel makinenizde bulunduğu konum takılı şekilde kapsayıcı görüntüsünü çalıştırma

    ```bash
    docker run -itd -p 8080:8080 -v /Users/suhuruli/Documents/Work/Samples/service-fabric-java-quickstart/AzureCluster:/tmp/myCerts rapatchi/jenkins:v10
    ```

3. Kapsayıcı görüntüsü örneğinin kimliğini alın. ``docker ps –a`` komutuyla tüm Docker kapsayıcılarını listeleyebilirsiniz

4. Aşağıdaki komutu çalıştırarak Jenkins örneğinizin parolasını alın:

    ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```

    Kapsayıcı kimliği 2d24a73b5964 ise, 2d24 kullanın.
    * ``http://<HOST-IP>:8080`` olan bu parola, portaldan Jenkins panosunda oturum açmak için gereklidir
    * İlk kez oturum açtıktan sonra kendi kullanıcı hesabınızı oluşturabilir veya yönetici hesabını kullanabilirsiniz.
    
5. [Yeni bir SSH anahtarı oluşturma ve SSH aracısına ekleme](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) bölümünde anlatılan adımları kullanarak, GitHub’ı Jenkins ile çalışacak şekilde ayarlayın. Komutlar Docker kapsayıcısından çalıştırıldığından, Linux ortamı için yönergeleri izleyin. 
    * SSH anahtarı oluşturmak için GitHub tarafından sağlanan yönergeleri kullanın. Ardından, depoyu barındıran GitHub hesabına SSH anahtarını ekleyin.
    * Önceki bağlantıda belirtilen komutları Jenkins Docker kabuğunda (ana bilgisayarınızda değil) çalıştırın.
    * Ana bilgisayarınızdan Jenkins kabuğunda oturum açmak için aşağıdaki komutları kullanın:

    ```sh
    docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
    ```

    Jenkins kapsayıcı görüntüsünün barındırıldığı küme veya makinede genel kullanıma yönelik bir IP bulunduğundan emin olun. Genel kullanıma yönelik IP olması, Jenkins örneğinin GitHub'dan bildirimler almasını sağlar.    

## <a name="create-and-configure-a-jenkins-job"></a>Bir Jenkins işi oluşturma ve yapılandırma

1. İlk olarak, Github'da Oylama projesini barındırmak için kullanabileceğiniz bir deponuz yoksa oluşturun. Bu öğreticinin geri kalan kısmında depo, **dev_test** olarak anılmıştır.

2. Jenkins panonuzda **yeni öğe** oluşturun.

3. Bir öğe adını girin (örneğin, **MyJob**). **Serbest stil proje**’yi seçip **Tamam**’a tıklayın.

4. İş sayfasına gidip **Yapılandır** öğesine tıklayın.

   a. Genel bölümünde, **GitHub projesi**’nin onay kutusunu seçin ve GitHub projesi URL’nizi belirtin. Bu URL, Jenkins sürekli tümleştirme, sürekli dağıtım (CI/CD) akışı ile tümleştirmek istediğiniz Service Fabric Java uygulamasını barındırır (örneğin, ``https://github.com/testaccount/dev_test``).

   b. **Kaynak Kodu Yönetimi** bölümünde **Git**’i seçin. Jenkins CI/CD akışıyla tümleştirmek istediğiniz Service Fabric Java uygulamasını barındıran deponun URL'sini belirtin (örneğin, *https://github.com/testaccount/dev_test.git*). Ayrıca, burada hangi dalın derleneceğini belirtebilirsiniz (örneğin, **/master**).

5. *GitHub*’ınızı (depoyu barındıran) Jenkins ile konuşabilecek şekilde yapılandırın. Aşağıdaki adımları kullanın:

   a. GitHub depo sayfanıza gidin. **Ayarlar** > **Tümleştirmeler ve Hizmetler** öğesine gidin.

   b. **Hizmet Ekle**’yi seçin, **Jenkins** yazın ve **Jenkins-GitHub eklentisi**’ni seçin.

   c. Jenkins web kancası URL'nizi girin (varsayılan olarak, ``http://<PublicIPorFQDN>:8081/github-webhook/`` olmalıdır). **Hizmet ekle/güncelleştir** öğesine tıklayın.

   d. Jenkins örneğinize bir test olayı gönderilir. GitHub’da web kancasının yanında yeşil renkli bir onay işareti görürsünüz ve projeniz derlenir.

   ![Service Fabric Jenkins Yapılandırması](./media/service-fabric-tutorial-java-jenkins/jenkinsconfiguration.png)

6. **Derleme Tetikleyicileri** bölümünde, istediğiniz derleme seçeneğini belirleyin. Bu örnekte, depoda bazı itmeler gerçekleştiğinde derleme tetiklemeyi istersiniz. Bu nedenle **GITScm yoklaması için GitHub kanca tetikleyicisi**’ni seçin.

7. **Derleme bölümü** altında, **Derleme adımı ekle** açılır listesinden **Gradle Betiğini Çağır** seçeneğini belirleyin. Açılan pencere öğesinde gelişmiş menüyü açın, uygulamanız için **Kök derleme betiği** yolunu belirtin. Belirtilen yoldan build.gradle’ı alır ve buna göre çalışır.

    ![Service Fabric Jenkins Derleme eylemi](./media/service-fabric-tutorial-java-jenkins/jenkinsbuildscreenshot.png)
  
8. **Derleme Sonrası Eylemler** açılır listesinden **Service Fabric Projesini Dağıt**’ı seçin. Burada, Jenkins tarafından derlenen Service Fabric uygulamasının dağıtılacağı kümenin ayrıntılarını sağlamanız gerekir. Sertifika yolu, birimin takılı olduğu sertifikanın yoludur (/tmp/myCerts). 
   
    Uygulamayı dağıtmak için kullanılan ek ayrıntıları da sağlayabilirsiniz. Uygulama ayrıntılarına yönelik bir örnek için aşağıdaki ekran görüntüsüne bakın:

    ![Service Fabric Jenkins Derleme eylemi](./media/service-fabric-tutorial-java-jenkins/sfjenkins.png)

    > [!NOTE]
    > Burada küme, Jenkins kapsayıcı görüntüsünü dağıtmak için Service Fabric kullandığınız durumda Jenkins kapsayıcı uygulamasını barındıran kümeyle aynı olabilir.
    >

## <a name="update-your-existing-application"></a>Mevcut uygulamanızı güncelleştirme 

1. *VotingApplication/VotingWebPkg/Code/wwwroot/index.html* dosyasında HTML başlığını **Service Fabric Voting Sample V2** ile güncelleştirin. 

    ```html 
    <div ng-app="VotingApp" ng-controller="VotingAppController" ng-init="refresh()">
        <div class="container-fluid">
            <div class="row">
                <div class="col-xs-8 col-xs-offset-2 text-center">
                    <h2>Service Fabric Voting Sample V2</h2>
                </div>
            </div>
        </div>
    </div>
    ```

2. **ApplicationTypeVersion** ve **ServiceManifestVersion** sürümünü, *Voting/VotingApplication/ApplicationManifest.xml* dosyasında **2.0.0** olarak güncelleştirin. 

    ```xml
    <?xml version="1.0" encoding="utf-8" standalone="no"?>
    <ApplicationManifest xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingApplicationType" ApplicationTypeVersion="2.0.0">
      <Description>Voting Application</Description>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="2.0.0"/>
      </ServiceManifestImport>
      <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="VotingDataServicePkg" ServiceManifestVersion="1.0.0"/>
        </ServiceManifestImport>
        <DefaultServices>
          <Service Name="VotingWeb">
             <StatelessService InstanceCount="1" ServiceTypeName="VotingWebType">
                <SingletonPartition/>
             </StatelessService>
          </Service>      
       <Service Name="VotingDataService">
                <StatefulService MinReplicaSetSize="3" ServiceTypeName="VotingDataServiceType" TargetReplicaSetSize="3">
                    <UniformInt64Partition HighKey="9223372036854775807" LowKey="-9223372036854775808" PartitionCount="1"/>
                </StatefulService>
            </Service>
        </DefaultServices>      
    </ApplicationManifest>
    ```

3. *Voting/VotingApplication/VotingWebPkg/ServiceManifest.xml* dosyasında **CodePackage** etiketindeki **Sürüm** alanını ve **ServiceManifest** içindeki **Sürüm** alanını **2.0.0** olarak güncelleştirin.

    ```xml
    <CodePackage Name="Code" Version="2.0.0">
    <EntryPoint>
        <ExeHost>
        <Program>entryPoint.sh</Program>
        </ExeHost>
    </EntryPoint>
    </CodePackage>
    ```

4. Uygulama yükseltmesi gerçekleştiren bir Jenkins işini başlatmak için yeni değişikliklerinizi Github deponuza gönderin. 

5. Service Fabric Explorer’da **Uygulamalar** açılan listesine tıklayın. Yükseltmenizin durumunu görmek için **Devam Eden Yükseltmeler** sekmesine tıklayın.

    ![Devam eden yükseltme](./media/service-fabric-tutorial-create-java-app/upgradejava.png)

6. **http://\<Host-IP>:8080** adresine erişirseniz Oylama uygulaması tüm işlevselliğiyle çalışır durumda olur. 

    ![Yerel Oylama Uygulaması](./media/service-fabric-tutorial-java-jenkins/votingv2.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Makinenizde Service Fabric Jenkins kapsayıcısını dağıtma
> * Service Fabric’te dağıtım için Jenkins ortamını ayarlama
> * Uygulamanızı yükseltme

* Diğer [Java Örnekleri](https://github.com/Azure-Samples/service-fabric-java-getting-started)’ne göz atın