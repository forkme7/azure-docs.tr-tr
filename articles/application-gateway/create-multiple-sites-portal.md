---
title: Birden çok web sitesi - Azure portalında barındıran bir uygulama ağ geçidi oluşturma | Microsoft Docs
description: Azure portalını kullanarak birden çok web sitesini barındıran bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 12/28/2017
ms.author: davidmu
ms.openlocfilehash: 078252fe3413c702a93da59484be9fc678ddec1e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-and-configure-an-application-gateway-to-host-multiple-web-sites-using-the-azure-portal"></a>Oluşturma ve Azure portalını kullanarak birden çok web sitesini barındırmak için bir uygulama ağ geçidi yapılandırma

Azure portalına kullanabilirsiniz [birden çok web sitesi barındırma yapılandırma](multiple-site-overview.md) oluştururken bir [uygulama ağ geçidi](overview.md). Bu öğreticide, sanal makineleri kullanarak arka uç adres havuzlarında tanımlayın. Daha sonra dinleyicileri ve uygun sunucuları havuzlarındaki web trafiği ulaştığında emin olmak için kendi etki alanlarını temel alan kurallar yapılandırın. Bu öğretici birden çok etki alanları ve kullanım örnekleri sahibi olduğunu varsayar *www.contoso.com* ve *www.fabrikam.com*.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturun
> * Arka uç sunucular ile arka uç havuzları oluşturma
> * Arka uç dinleyicileri oluşturma
> * Yönlendirme kuralları oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturun

![Çok siteli yönlendirme örneği](./media/create-multiple-sites-portal/scenario.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum açın [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte, iki alt ağ oluşturulur: bir uygulama ağ geçidi ve arka uç sunucular için diğer için. Uygulama ağ geçidi oluşturma aynı anda bir sanal ağ oluşturabilirsiniz.

1. Tıklatın **yeni** Azure portalında sol üst köşesinde bulundu.
2. Seçin **ağ** ve ardından **uygulama ağ geçidi** öne çıkan listesinde.
3. Uygulama ağ geçidi için bu değerleri girin:

    - *myAppGateway* - uygulama ağ geçidi adı.
    - *myResourceGroupAG* - yeni kaynak grubu için.

    ![Yeni uygulama ağ geçidi oluşturma](./media/create-multiple-sites-portal/application-gateway-create.png)

4. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
5. Tıklatın **sanal ağ seçin**, tıklatın **Yeni Oluştur**ve ardından sanal ağ için bu değerleri girin:

    - *myVNet* - sanal ağın adı.
    - *10.0.0.0/16* - sanal ağ adres alanı.
    - *myAGSubnet* - alt ağ adı.
    - *10.0.0.0/24* - alt ağ adres alanı.

    ![Sanal ağ oluşturma](./media/create-multiple-sites-portal/application-gateway-vnet.png)

6. Tıklatın **Tamam** sanal ağ ve alt ağ oluşturmak için.
7. Tıklatın **genel bir IP adresi seçin**, tıklatın **Yeni Oluştur**ve ortak IP adresini girin. Bu örnekte adlı ortak IP adresi *myAGPublicIPAddress*. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
8. Dinleyici yapılandırması için varsayılan değerleri kabul edin, Web uygulaması güvenlik duvarı devre dışı bırakın ve ardından **Tamam**.
9. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** ağ kaynaklarını ve uygulama ağ geçidi oluşturmak için. Oluşturulması, dağıtımı, sonraki bölüme geçmeden önce başarıyla tamamlanana kadar bekleyin uygulama ağ geçidi için birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Bir alt ağ Ekle

1. Tıklatın **tüm kaynakları** sol taraftaki menüyü ve ardından **myVNet** kaynakları listesinden.
2. Tıklatın **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluşturma](./media/create-multiple-sites-portal/application-gateway-subnet.png)

3. Girin *myBackendSubnet* 'ye tıklayın ve alt ağ adı için **Tamam**.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu örnekte uygulama ağ geçidi için arka uç sunucuları olarak kullanılacak iki sanal makine oluşturun. Ayrıca sanal makinelere trafik doğru yönlendirme doğrulamak için IIS yükleyin.

1. **Yeni**’ye tıklayın.
2. Tıklatın **işlem** ve ardından **Windows Server 2016 Datacenter** öne çıkan listesinde.
3. Sanal makine için bu değerleri girin:

    - *contosoVM* - sanal makine adı için.
    - Yönetici kullanıcı adı için *azureuser*.
    - *Azure123456!* parolası.
    - Seçin **var olanı kullan**ve ardından *myResourceGroupAG*.

4. **Tamam**’a tıklayın.
5. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
6. Olduğundan emin olun **myVNet** sanal ağ ve alt ağ için seçili olan **myBackendSubnet**. 
7. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
8. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.

### <a name="install-iis"></a>IIS yükleme

1. Etkileşimli Kabuğu'nu açın ve onu ayarlandığından emin olun **PowerShell**.

    ![Özel uzantısını yükleyin](./media/create-multiple-sites-portal/application-gateway-extension.png)

2. IIS sanal makineye yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/davidmu1/samplescripts/master/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzureRmVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName contosoVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. İkinci sanal makine oluşturun ve yalnızca tamamlandı adımları kullanarak IIS yükleyin. Adlarını girin *fabrikamVM* kümesi AzureRmVMExtension VMName değerini ve ad için.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Sanal makineler ile arka uç havuzları oluşturma

1. Tıklatın **tüm kaynakları** ve ardından **myAppGateway**.
2. Tıklatın **arka uç havuzları**ve ardından **Ekle**.
3. Bir adı girin *contosoPool* ve ekleme *contosoVM* kullanarak **Ekle hedef**.

    ![Arka uç sunucuları ekleme](./media/create-multiple-sites-portal/application-gateway-multisite-backendpool.png)

4. **Tamam**’a tıklayın.
5. Tıklatın **arka uç havuzları** ve ardından **Ekle**.
6. Oluşturma *fabrikamPool* ile *fabrikamVM* yalnızca tamamlandı adımları kullanarak.

## <a name="create-backend-listeners"></a>Arka uç dinleyicileri oluşturma

1. Tıklatın **dinleyicileri** ve ardından **çok siteli**.
2. Bu değerleri için dinleyici girin:
    
    - *contosoListener* - dinleyici adı.
    - *www.contoso.com* -bu ana bilgisayar adı örneği, etki alanı adıyla değiştirin.

3. **Tamam**’a tıklayın.
4. Adını kullanarak ikinci bir dinleyici oluşturun *fabrikamListener* ve ikinci etki alanı adınızı kullanın. Bu örnekte, *www.fabrikam.com* kullanılır.

## <a name="create-routing-rules"></a>Yönlendirme kuralları oluşturma

Kurallar işlenir listelendikleri sırayla ve trafik belirginliğe bağımsız olarak eşleşen ilk kural kullanarak yönlendirilir. Temel bir dinleyici ve çok siteli dinleyicisi hem aynı bağlantı noktasında kullanan bir kural kullanarak bir kuralı varsa, örneğin, çok siteli dinleyicisi kuralla beklendiği şekilde çalışması için temel dinleyicisi çok siteli kuralı için sırada olan kural önce listelenmiş olması gerekir. 

Bu örnekte, iki yeni kural oluşturma ve uygulama ağ geçidi oluşturduğunuzda, oluşturulan varsayılan kuralı silin. 

1. Tıklatın **kuralları** ve ardından **temel**.
2. Girin *contosoRule* adı.
3. Seçin *contosoListener* dinleyicisinin.
4. Seçin *contosoPool* arka uç havuzu için.

    ![Yol tabanlı bir kural oluşturun](./media/create-multiple-sites-portal/application-gateway-multisite-rule.png)

5. **Tamam**’a tıklayın.
6. Adlarını kullanarak ikinci bir kural oluşturun *fabrikamRule*, *fabrikamListener*, ve *fabrikamPool*.
7. Adlı varsayılan kuralını silmek *kuralı 1* tıklatarak ve tıklatarak **silmek**.

## <a name="create-a-cname-record-in-your-domain"></a>Etki alanınızda bir CNAME kaydı oluşturun

Uygulama ağ geçidi genel IP adresiyle oluşturulduktan sonra DNS adresi alın ve etki alanınızdaki bir CNAME kaydı oluşturmak için kullanın. Uygulama ağ geçidi yeniden başlatıldığında VIP değişebilir çünkü A kayıtlarını kullanılması önerilmez.

1. Tıklatın **tüm kaynakları**ve ardından **myAGPublicIPAddress**.

    ![Kayıt uygulama ağ geçidi DNS adresi](./media/create-multiple-sites-portal/application-gateway-multisite-dns.png)

2. DNS adresi kopyalayın ve yeni bir CNAME kaydı, etki alanınızdaki değeri olarak kullanın.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

1. Tarayıcınızın adres çubuğuna, etki alanı adınızı girin. Gibi http://www.contoso.com.

    ![Uygulama ağ geçidi test contoso sitede](./media/create-multiple-sites-portal/application-gateway-iistest.png)

2. Diğer etki alanınıza adresini değiştirin ve aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Uygulama ağ geçidi test fabrikam sitede](./media/create-multiple-sites-portal/application-gateway-iistest2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrenilen nasıl yapılır:

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturun
> * Arka uç sunucular ile arka uç havuzları oluşturma
> * Arka uç dinleyicileri oluşturma
> * Yönlendirme kuralları oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturun
