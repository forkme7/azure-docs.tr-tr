---
title: Linux üzerinde geliştirme ortamınızı ayarlama | Microsoft Belgeleri
description: Linux üzerinde çalışma zamanını ve SDK'yı yükleyip yerel bir geliştirme kümesi oluşturun. Bu kurulumu tamamladıktan sonra uygulama derlemek için hazır hale gelirsiniz.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: bf88e4c702321a7810ec6a3e50eb6cd47a788734
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="prepare-your-development-environment-on-linux"></a>Linux üzerinde geliştirme ortamınızı hazırlama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Linux geliştirme makinenizde [Azure Service Fabric uygulamaları](service-fabric-application-model.md) dağıtıp çalıştırmak için çalışma zamanını ve ortak SDK'yı yükleyin. Ayrıca isteğe bağlı Java ve .NET Core geliştirme SDK'larını yükleyebilirsiniz. 

Bu makaledeki adımlarda, Linux’a yerel olarak yükleme yaptığınız veya Service Fabric OneBox kapsayıcı görüntüsünü (`microsoft/service-fabric-onebox`) kullandığınız varsayılır. 

Linux için Windows Alt Sistemine SDK ve Service Fabric çalışma zamanının yüklenmesi desteklenmez. Ancak bulutta veya şirket içinde başka bir yerde barındırılan Service Fabric varlıklarını yönetmenize olanak sağlayan Azure Service Fabric komut satırı arabirimi (CLI) desteklenir. CLI yükleme hakkında bilgi için bkz. [Service Fabric CLI'sini ayarlama](./service-fabric-cli.md).


## <a name="prerequisites"></a>Ön koşullar

* Geliştirme için şu işletim sistemi sürümleri desteklenir:

    * Ubuntu 16.04 (`Xenial Xerus`)

      * `apt-transport-https` paketinin yüklendiğinden emin olun:
         
         ```bash
         sudo apt-get install apt-transport-https
         ```
    * Red Hat Enterprise Linux 7.4 (Service Fabric önizleme desteği)


## <a name="installation-methods"></a>Yükleme Yöntemleri

### <a name="1-script-installation-ubuntu"></a>1. Betikle yüklemesi (Ubuntu)

Service Fabric çalışma zamanını ve Service Fabric ortak SDK'sını **sfctl** CLI ile birlikte yüklemek için bir betik sağlanmıştır. Yüklenecek uygulamaları ve kabul edilen lisans şartlarını belirlemek için sonraki bölümde bulunan el ile yükleme adımlarını çalıştırın. Betiği çalıştırdığınızda yüklenen yazılıma ait tüm lisans şartlarını kabul ettiğiniz varsayılır. 

Betik başarılı bir şekilde yürütüldükten sonra [Yerel küme oluşturma](#set-up-a-local-cluster) bölümüne geçebilirsiniz.

```bash
sudo curl -s https://raw.githubusercontent.com/Azure/service-fabric-scripts-and-templates/master/scripts/SetupServiceFabric/SetupServiceFabric.sh | sudo bash
```

### <a name="2-manual-installation"></a>2. El ile Yükleme
Service Fabric çalışma zamanı ve ortak SDK'yı el ile yüklemek için bu kılavuzdaki adımları izleyin.

## <a name="update-your-apt-sourcesyum-repositories"></a>APT kaynaklarınızı/Yum Depolarınızı güncelleştirme
SDK ve ilişkili çalışma zamanı paketini apt-get komut satırı aracıyla yüklemek için, öncelikle Advanced Packaging Tool (APT) kaynaklarınızı güncelleştirmelisiniz.

### <a name="ubuntu"></a>Ubuntu

1. Bir terminal açın.
2. Service Fabric deponuzu kaynaklar listenize ekleyin.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. `dotnet` deposunu kaynak listenize ekleyin.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Yeni Gnu Privacy Guard (GnuPG, veya GPG) anahtarını APT anahtarlığınıza ekleyin.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Resmi Docker GPG anahtarını APT anahtarlığınıza ekleyin.

    ```bash
    sudo apt-get install curl
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. Docker deposunu ayarlayın.

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. Paket listelerinizi yeni eklenen depolara göre yenileyin.

    ```bash
    sudo apt-get update
    ```


### <a name="red-hat-enterprise-linux-74-service-fabric-preview-support"></a>Red Hat Enterprise Linux 7.4 (Service Fabric önizleme desteği)

1. Bir terminal açın.
2. Extra Packages for Enterprise Linux’u (EPEL) indirip yükleyin.

    ```bash
    wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    sudo yum install epel-release-latest-7.noarch.rpm
    ```
3. Sisteminize EfficiOS RHEL7 paket deposunu ekleyin.

    ```bash
    sudo wget -P /etc/yum.repos.d/ https://packages.efficios.com/repo.files/EfficiOS-RHEL7-x86-64.repo
    ```

4. Efficios paketi imzalama anahtarını yerel GPG kimlik anahtarlığına aktarın.

    ```bash
    sudo rpmkeys --import https://packages.efficios.com/rhel/repo.key
    ```

5. Microsoft RHEL deposunu sisteminize ekleyin.

    ```bash
    curl https://packages.microsoft.com/config/rhel/7.4/prod.repo > ./microsoft-prod.repo
    sudo cp ./microsoft-prod.repo /etc/yum.repos.d/
    ```

6. dotnet sdk’sını yükleyin.

    ```bash
    yum install rh-dotnet20 -y
    ```

## <a name="install-and-set-up-the-service-fabric-sdk-for-local-cluster-setup"></a>Yerel küme kurulumu için Service Fabric SDK'sını yükleme ve ayarlama

Kaynaklarınızı güncelleştirdikten sonra SDK’yı yükleyebilirsiniz. Service Fabric SDK paketini yükleyin, yüklemeyi onaylayın ve lisans sözleşmesini kabul edin.

### <a name="ubuntu"></a>Ubuntu

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   Aşağıdaki komutlar, Service Fabric paketlerine yönelik lisansı kabul etme işlemini otomatik hale getirir:
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-ga select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-ga select true" | sudo debconf-set-selections
>   ```

### <a name="red-hat-enterprise-linux-74-service-fabric-preview-support"></a>Red Hat Enterprise Linux 7.4 (Service Fabric önizleme desteği)

```bash
sudo yum install servicefabricsdkcommon
```

Yukarıdaki yüklemeyle birlikte gelen Service Fabric çalışma zamanı, aşağıdaki tabloda yer alan paketleri içerir. 

 | | DotNetCore | Java | Python | NodeJS | 
--- | --- | --- | --- |---
Ubuntu | 2.0.0 | OpenJDK 1.8 | Npm’de örtük | en son |
RHEL | - | OpenJDK 1.8 | Npm’de örtük | en son |

## <a name="set-up-a-local-cluster"></a>Yerel küme oluşturma
  Yükleme başarıyla tamamlandığında yerel bir küme başlatabilmeniz gerekir.

  1. Küme kurulum betiğini çalıştırın.

      ```bash
      sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
      ```

  2. Bir web tarayıcısı açın ve [Service Fabric Explorer](http://localhost:19080/Explorer) (`http://localhost:19080/Explorer`) adresine gidin. Küme başlatıldıysa Service Fabric Explorer panosunu görmeniz gerekir. Kümenin tamamen ayarlanması birkaç dakika sürebilir. Tarayıcınız URL’yi açamazsa veya Service Fabric Explorer, sistemin hazır olduğunu göstermezse birkaç dakika bekleyip tekrar deneyin.

      ![Linux üzerinde Service Fabric Explorer][sfx-linux]

  Bu noktada, önceden oluşturulmuş Service Fabric uygulama paketlerini ve yeni paketleri konuk kapsayıcılar veya konuk yürütülebilir öğelere göre dağıtabilirsiniz. Java veya .NET Core SDK’larını kullanarak yeni hizmetler oluşturmak için aşağıdaki bölümlerde yer alan kurulum adımlarını izleyin.


  > [!NOTE]
  > Tek başına kümeler Linux’da desteklenmez.
  >


>   [!TIP]
    Bir SSD diskiniz varsa, üstün performans için devclustersetup.sh ile `--clusterdataroot` kullanarak bir SSD klasör yolu geçirmeniz önerilir.

## <a name="set-up-the-service-fabric-cli"></a>Service Fabric CLI’sını ayarlama

[Service Fabric CLI](service-fabric-cli.md) kümeler ve uygulamalar da dahil olmak üzere Service Fabric varlıklarıyla etkileşime yönelik komutlar içerir.
CLI'yı yüklemek için [Service Fabric CLI'sı](service-fabric-cli.md) yönergelerini uygulayın.


## <a name="set-up-yeoman-generators-for-containers-and-guest-executables"></a>Kapsayıcılar ve konuk yürütülebilir dosyalar için Yeoman oluşturucularını ayarlama
Service Fabric, Yeoman şablon oluşturucuları kullanarak terminalden Service Fabric uygulamaları oluşturmanıza yardımcı olan yapı iskelesi araçları sağlar. Service Fabric Yeoman şablon oluşturucularını ayarlamak için bu adımları izleyin:

1. Makinenize nodejs ve NPM yükleme

Ubuntu
  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```

Red Hat Enterprise Linux 7.4 (Service Fabric önizleme desteği)
  ```bash
  sudo yum install nodejs
  sudo yum install npm
  ```
2. NPM’den makinenize [Yeoman](http://yeoman.io/) şablon oluşturucuyu yükleme

  ```bash
  sudo npm install -g yo
  ```
3. NPM’den Service Fabric Yeo kapsayıcı oluşturucusunu ve konuk yürütülebilir dosya oluşturucusunu yükleme

  ```bash
  sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
  sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
  ```

Oluşturucuları yükledikten sonra, sırasıyla `yo azuresfguest` ve `yo azuresfcontainer` komutlarını çalıştırarak konuk yürütülebilir dosyası ya da kapsayıcı hizmetleri oluşturabilmeniz gerekir.

## <a name="set-up-net-core-20-development"></a>.NET Core 2.0 ile geliştirmeyi ayarlama

[C# Service Fabric uygulamaları oluşturmaya](service-fabric-create-your-first-linux-application-with-csharp.md) başlamak amacıyla [Ubuntu için .NET Core 2.0 SDK'sını](https://www.microsoft.com/net/core#linuxubuntu) yükleyin. .NET Core 2.0 Service Fabric uygulamaları paketleri NuGet.org üzerinde barındırılmaktadır ve şu anda önizleme sürümündedir.

## <a name="set-up-java-development"></a>Java ile geliştirmeyi ayarlama

Service Fabric hizmetlerini Java kullanarak derlemek için JDK 1.8 ve Gradle yükleyerek derleme görevlerini çalıştırın. Aşağıdaki kod parçacığı Open JDK 1.8’i ve Gradle’ı birlikte yükler. Service Fabric Java kitaplıkları Maven’dan alınır.


Ubuntu 
 ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

Red Hat Enterprise Linux 7.4 (Service Fabric önizleme desteği)
  ```bash
  sudo yum install java-1.8.0-openjdk-devel
  curl -s https://get.sdkman.io | bash
  sdk install gradle
  ```
 
## <a name="install-the-eclipse-plug-in-optional"></a>Eclipse eklentisini yükleme (isteğe bağlı)

Service Fabric için Eclipse eklentisini Java EE Geliştiricileri veya Java Geliştiricileri için Eclipse IDE içinden yükleyebilirsiniz. Eclipse kullanarak, Service Fabric Java uygulamalarına ek olarak Service Fabric konuk yürütülebilir uygulamaları ve kapsayıcı uygulamaları oluşturabilirsiniz.

> [!IMPORTANT]
> Service Fabric eklentisi için Eclipse Neon veya üzeri bir sürüm gerekir. Eclipse sürümünüzün nasıl denetleneceği hakkında bu nottan sonraki yönergelere bakın. Eclipse’in önceki bir sürümünü yüklediyseniz, [Eclipse sitesinden](https://www.eclipse.org) daha yeni sürümleri indirebilirsiniz. Mevcut bir Eclipse yüklemesinin üzerine yüklemeniz (üzerine yazmanız) önerilmez. Yükleyiciyi çalıştırmadan önce kaldırabilir veya yeni sürümü farklı bir dizine yükleyebilirsiniz. 
> 
> Ubuntu üzerinde, paket yükleyici (`apt` veya `apt-get`) kullanmak yerine doğrudan Eclipse sitesinden yükleme yapılmasını öneririz. Böylece, Eclipse’in en güncel sürümünü elde etmeniz sağlanır. Java EE Geliştiricileri için veya Java Geliştiricileri için Eclipse IDE’yi yükleyebilirsiniz.

1. Eclipse’te, Eclipse Neon veya sonraki bir sürümünün ve Buildship 2.2.1 veya sonraki bir sürümünün yüklü olduğundan emin olun. **Yardım** > **Eclipse Hakkında** > **Yükleme Ayrıntıları**’nı seçerek yüklü bileşenlerin sürümlerini denetleyebilirsiniz. [Eclipse Buildship: Gradle için Eclipse eklentileri][buildship-update] bölümünde sağlanan yönergelerden yararlanarak Buildship’i güncelleştirebilirsiniz.

2. Service Fabric eklentisini yüklemek için **Yardım** > **Yeni Yazılım Yükle**’yi seçin.

3. **Birlikte çalış** kutusuna **http://dl.microsoft.com/eclipse** yazın.

4. **Ekle**'ye tıklayın.

    ![Kullanılabilir Yazılım sayfası][sf-eclipse-plugin]

5. **ServiceFabric** eklentisini seçip **İleri**’ye tıklayın.

6. Yükleme adımlarını tamamlayın ve ardından son kullanıcı lisans sözleşmesini kabul edin.

Service Fabric Eclipse eklentisi zaten yüklüyse, en yeni sürümü kullandığınızdan emin olun. **Yardım** > **Eclipse Hakkında** > **Yükleme Ayrıntıları**’nı seçip ardından yüklü eklentiler listesinde Service Fabric araması yaparak denetleyebilirsiniz. Daha yeni bir sürüm varsa, **Güncelleştir**’i seçin.

Daha fazla bilgi için bkz. [Eclipse Java uygulama geliştirmesi için Service Fabric eklentisi](service-fabric-get-started-eclipse.md).

## <a name="update-the-sdk-and-runtime"></a>SDK ve çalışma zamanını güncelleştirme

SDK ve çalışma zamanının son sürümüne güncelleştirmek için aşağıdaki komutları çalıştırın:

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon
```
Maven’dan alınan Java SDK'sı ikili dosyalarını güncelleştirmek için ``build.gradle`` dosyasında karşılık gelen ikili sürüm ayrıntılarını en son sürüme işaret edecek şekilde güncelleştirmeniz gerekir. Sürümü tam olarak nerede güncelleştirmeniz gerektiğini öğrenmek için [buradaki](https://github.com/Azure-Samples/service-fabric-java-getting-started) Service Fabric başlangıç örneklerindeki herhangi bir ``build.gradle`` dosyasına bakabilirsiniz.

> [!NOTE]
> Paketlerin güncelleştirilmesi, yerel geliştirme kümenizin çalışmayı durdurmasına neden olabilir. Yükseltme sonrasında bu sayfadaki yönergeleri uygulayarak yerel kümenizi yeniden başlatın.

## <a name="remove-the-sdk"></a>SDK'yı kaldırma
Service Fabric SDK'larını kaldırmak için aşağıdaki komutu çalıştırın:

### <a name="ubuntu"></a>Ubuntu

```bash
sudo apt-get remove servicefabric servicefabicsdkcommon
sudo npm uninstall generator-azuresfcontainer
sudo npm uninstall generator-azuresfguest
sudo apt-get install -f
```


### <a name="red-hat-enterprise-linux-74-service-fabric-preview-support"></a>Red Hat Enterprise Linux 7.4 (Service Fabric önizleme desteği)

```bash
sudo yum remote servicefabric servicefabicsdkcommon
sudo npm uninstall generator-azuresfcontainer
sudo npm uninstall generator-azuresfguest
```

## <a name="next-steps"></a>Sonraki adımlar

* [Linux üzerinde Yeoman kullanarak ilk Service Fabric Java uygulamanızı oluşturma ve dağıtma](service-fabric-create-your-first-linux-application-with-java.md)
* [Linux üzerinde Eclipse için Service Fabric Eklentisi kullanarak ilk Service Fabric Java uygulamanızı oluşturma ve dağıtma](service-fabric-get-started-eclipse.md)
* [Linux üzerinde ilk CSharp uygulamanızı oluşturma](service-fabric-create-your-first-linux-application-with-csharp.md)
* [OSX üzerinde geliştirme ortamınızı hazırlama](service-fabric-get-started-mac.md)
* [Windows üzerinde Linux geliştirme ortamı hazırlama](service-fabric-local-linux-cluster-windows.md)
* [Uygulamalarınızı yönetmek için Service Fabric CLI'yı kullanma](service-fabric-application-lifecycle-sfctl.md)
* [Service Fabric Windows/Linux farkları](service-fabric-linux-windows-differences.md)
* [Linux kümenizde işletim sistemi düzeltme eki uygulama işlemini otomatikleştirme](service-fabric-patch-orchestration-application-linux.md)
* [Service Fabric CLI kullanmaya başlama](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
