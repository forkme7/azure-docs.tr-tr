---
title: Azure SSIS tümleştirmesi çalışma zamanı zamanlama | Microsoft Docs
description: Bu makalede, başlatma ve Azure Automation ve Data Factory kullanarak bir Azure SSIS tümleştirme çalışma zamanını durdurma zamanlama açıklar.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: article
ms.date: 04/17/2018
ms.author: douglasl
ms.openlocfilehash: 3e69c147201ab7f3c5e2cf61e72bdb8073354e67
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="how-to-schedule-starting-and-stopping-of-an-azure-ssis-integration-runtime"></a>Başlatma ve durdurma bir Azure SSIS tümleştirmesi çalışma zamanı zamanlama 
Çalıştıran bir Azure SSIS (SQL Server Integration Services) Tümleştirmesi çalışma zamanı (IR) ilişkili bir ücret sahiptir. Bu nedenle, yalnızca Azure'da SSIS paketleri çalıştırmak ve onu gerekmediğinde durdurmak gerektiğinde IR çalıştırmak isteyebilirsiniz. Veri Fabrikası UI veya Azure PowerShell kullanabileceğiniz [el ile başlatma veya bir Azure SSIS IR durdurma](manage-azure-ssis-integration-runtime.md)). Bu makalede, başlatma ve Azure Otomasyonu ve Azure Data Factory kullanarak bir Azure SSIS tümleştirmesi çalışma zamanı (IR) durdurma zamanlama açıklar. Bu makalede açıklanan üst düzey adımlar şunlardır:

1. **Oluşturma ve bir Azure Otomasyonu runbook'u sınayın.** Bu adımda, başlatıldığında veya bir Azure SSIS IR durdurulduğunda betiği ile bir PowerShell runbook oluşturma Ardından, başlatma ve durdurma senaryolarda runbook'u test ve IR başlatıldığında veya durdurulduğunda onaylayın. 
2. **Runbook için iki zamanlamalar oluşturun.** İlk zamanlama için runbook işlem olarak Başlat ile yapılandırın. İkinci zamanlamasını, runbook ile durdurma işlemi yapılandırın. Her iki zamanlamalarını, runbook çalıştığı tempoyla belirtin. Örneğin, her gün ve 23: 00 günlük çalıştırmak için ikinci bir 8'de çalıştırmak için ilk tek zamanlamak isteyebilirsiniz. İlk runbook çalıştığında Azure SSIS IR başlatır İkinci runbook çalıştığında Azure SSIS IR durdurur 
3. **Runbook için iki Web kancası oluşturma**, bir başlangıç işlemi diğeri durdurma işlemi. Bu Web kancalarını URL'lerini web etkinlikleri bir Data Factory işlem hattı yapılandırırken kullanın. 
4. **Data Factory işlem hattı oluşturma**. Oluşturduğunuz işlem hattını üç etkinliklerini oluşur. İlk **Web** etkinlik Azure SSIS IR başlatmak için ilk Web kancası çağırır **Saklı yordam** etkinlik SSIS paketi çalıştırır SQL komut dosyasını çalıştırır. İkinci **Web** etkinlik Azure SSIS IR durdurur Saklı yordam etkinliği kullanarak Data Factory işlem hattı SSIS paketinden çağırma hakkında daha fazla bilgi için bkz: [SSIS paketi çağırma](how-to-invoke-ssis-package-stored-procedure-activity.md). Ardından, belirttiğiniz tempoyla çalıştırmak için ardışık düzen zamanlamak için bir zamanlama tetikleyici oluşturun.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [saklı yordam etkinliği sürüm 1 kullanılarak çağırma SSIS paketleri](v1/how-to-invoke-ssis-package-stored-procedure-activity.md).

 
## <a name="prerequisites"></a>Önkoşullar
Bir Azure SSIS tümleştirmesi çalışma zamanı zaten sağlanan yapmadıysanız, aşağıdaki yönergeleri tarafından sağlama [Öğreticisi](tutorial-create-azure-ssis-runtime-portal.md). 

## <a name="create-and-test-an-azure-automation-runbook"></a>Oluşturma ve bir Azure Otomasyonu runbook'u test etme
Bu bölümde, aşağıdaki adımları gerçekleştirin: 

1. Bir Azure Otomasyonu hesabı oluşturun.
2. Bir PowerShell runbook Azure Otomasyonu hesabı oluşturun. Bu runbook'la ilişkilendirilmiş PowerShell komut dosyasını başlatır veya bir Azure SSIS işlemi parametresi için belirlediğiniz komutu göre IR durdurur. 
3. Her iki başlangıç runbook'u test ve çalışır durumda olduğunu doğrulamak için senaryolar durdurun. 

### <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma
Bir Azure Otomasyonu hesabı yoksa, bu adımı'ndaki yönergeleri izleyerek oluşturun. Ayrıntılı adımlar için bkz: [Azure Automation hesabı oluşturma](../automation/automation-quickstart-create-account.md). Bu adım bir parçası olarak, oluşturduğunuz bir **Azure Farklı Çalıştır** hesabı (hizmet Azure Active Directory'yi sorumlusu) ve ekleyin **katkıda bulunan** Azure aboneliğinizin rol. Azure SSIS IR sahip veri fabrikası içeren abonelik aynı olduğundan emin olun Azure Otomasyonu, Azure Resource Manager kimliğini doğrulamak ve kaynaklarınızı üzerinde çalıştırmak için bu hesabı kullanır. 

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
2. [Azure Portal](https://portal.azure.com/)’da oturum açın.    
3. Seçin **yeni** soldaki menüden seçin **izleme + Yönetim**seçip **Otomasyon**. 

    ![Yeni İzleme -> + Yönetimi Otomasyonu ->](./media/how-to-schedule-azure-ssis-integration-runtime/new-automation.png)
2. İçinde **Automation hesabı Ekle** penceresinde, aşağıdaki adımları uygulayın: 

    1. Belirtin bir **adı** Otomasyon hesabının. 
    2. Seçin **abonelik** data factory ile Azure SSIS IR sahip 
    3. İçin **kaynak grubu**seçin **Yeni Oluştur** yeni bir kaynak grubu oluşturmak veya seçmek için **var olanı kullan** var olan bir kaynak grubunu seçin. 
    4. Seçin bir **konumu** Otomasyon hesabının. 
    5. Onaylayın **Çalıştır hesabı oluştur** ayarlanır **Evet**. Bir hizmet sorumlusu Azure Active Directory'yi oluşturulur. Eklendiğinden **katkıda bulunan** Azure aboneliğinizin rolü
    6. Seçin **panoya Sabitle** böylece portal panosunda görürsünüz. 
    7. **Oluştur**’u seçin. 

        ![Yeni İzleme -> + Yönetimi Otomasyonu ->](./media/how-to-schedule-azure-ssis-integration-runtime/add-automation-account-window.png)
3. Gördüğünüz **dağıtım durumu** Panoda ve bildirimler. 
    
    ![Otomasyon dağıtma](./media/how-to-schedule-azure-ssis-integration-runtime/deploying-automation.png) 
4. Başarılı bir şekilde oluşturulduktan sonra Otomasyon hesabının giriş sayfasına bakın. 

    ![Otomasyon giriş sayfası](./media/how-to-schedule-azure-ssis-integration-runtime/automation-home-page.png)

### <a name="import-data-factory-modules"></a>Veri Fabrikası modülleri alın

1. Seçin **modülleri** içinde **paylaşılan kaynakları** bölümünde soldaki menüden ve sahip olup olmadığınızı doğrulayın **AzureRM.Profile** ve **AzureRM.DataFactoryV2** modülleri listesinde. Yoksa, seçin **Gözat galeri** araç çubuğunda.

    ![Otomasyon giriş sayfası](./media/how-to-schedule-azure-ssis-integration-runtime/automation-modules.png)
2. İçinde **Gözat galeri** penceresinde, türü **AzureRM.Profile** arama penceresi ve tuşuna **ENTER**. Seçin **AzureRM.Profile** listesinde. Ardından **alma** araç çubuğunda. 

    ![AzureRM.Profile seçin](./media/how-to-schedule-azure-ssis-integration-runtime/select-azurerm-profile.png)
1. İçinde **alma** penceresinde, seçin **tüm Azure modülleri güncelleştirmek kabul ediyorum** seçeneğini ve tıklayın **Tamam**.  

    ![İçeri aktarma AzureRM.Profile](./media/how-to-schedule-azure-ssis-integration-runtime/import-azurerm-profile.png)
4. Kapatmak için geri almak için windows **modülleri** penceresi. İçeri aktarma listesinde durumunu görmeniz gerekir. Listeyi yenilemek için **Yenile**’yi seçin. Görene kadar bekleyin **durum** olarak **kullanılabilir**.

    ![Alma durumu](./media/how-to-schedule-azure-ssis-integration-runtime/module-list-with-azurerm-profile.png)
1. İçeri aktarmak için gereken adımları yineleyin **AzureRM.DataFactoryV2** modülü. Bu modül durumunu ayarlamak Onayla **kullanılabilir** devam etmeden önce. 

    ![Son alma durumu](./media/how-to-schedule-azure-ssis-integration-runtime/module-list-with-azurerm-datafactoryv2.png)

### <a name="create-a-powershell-runbook"></a>PowerShell runbook’u oluşturma
Aşağıdaki yordam, bir PowerShell runbook oluşturmak için adımları sağlar. Bu runbook'la ilişkilendirilmiş ya da komut dosyasını başlatır/Azure SSIS belirtmek için komut göre IR durdurur **işlemi** parametresi. Bu bölümde, bir runbook oluşturmak için tüm ayrıntıları sağlamaz. Daha fazla bilgi için bkz: [bir runbook oluşturmak](../automation/automation-quickstart-create-runbook.md) makalesi.

1. Geçiş **Runbook'lar** sekmesini tıklatın ve seçin **+ runbook Ekle** araç çubuğundan. 

    ![Runbook düğme ekleme](./media/how-to-schedule-azure-ssis-integration-runtime/runbooks-window.png)
2. Seçin **yeni bir runbook oluşturmak**ve aşağıdaki adımları gerçekleştirin: 

    1. İçin **adı**, türü **StartStopAzureSsisRuntime**.
    2. İçin **Runbook türü**seçin **PowerShell**.
    3. **Oluştur**’u seçin.
    
        ![Runbook düğme ekleme](./media/how-to-schedule-azure-ssis-integration-runtime/add-runbook-window.png)
3. Runbook komut penceresine aşağıdaki betiği kopyalayıp yapıştırın. Kaydedin ve ardından kullanarak runbook'u yayımlamak **kaydetmek** ve **Yayımla** araç çubuğundaki düğmeler. 

    ```powershell
    Param
    (
          [Parameter (Mandatory= $true)]
          [String] $ResourceGroupName,
    
          [Parameter (Mandatory= $true)]
          [String] $DataFactoryName,
    
          [Parameter (Mandatory= $true)]
          [String] $AzureSSISName,
    
          [Parameter (Mandatory= $true)]
          [String] $Operation
    )
    
    $connectionName = "AzureRunAsConnection"
    try
    {
        # Get the connection "AzureRunAsConnection "
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
    
        "Logging in to Azure..."
        Connect-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    }
    catch {
        if (!$servicePrincipalConnection)
        {
            $ErrorMessage = "Connection $connectionName not found."
            throw $ErrorMessage
        } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
        }
    }
    
    if($Operation -eq "START" -or $operation -eq "start")
    {
        "##### Starting #####"
        Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $AzureSSISName -Force
    }
    elseif($Operation -eq "STOP" -or $operation -eq "stop")
    {
        "##### Stopping #####"
        Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    }  
    "##### Completed #####"    
    ```

    ![PowerShell runbook'unu Düzenle](./media/how-to-schedule-azure-ssis-integration-runtime/edit-powershell-runbook.png)
5. Runbook'u seçerek test **Başlat** araç çubuğunda. 

    ![Başlat runbook düğmesi](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-button.png)
6. İçinde **Runbook'u Başlat** penceresinde, aşağıdaki adımları gerçekleştirin: 

    1. İçin **kaynak grubu adı**, Azure SSIS IR sahip data factory ile kaynak grubunun adını girin 
    2. İçin **veri FABRİKASI adı**, Azure SSIS IR sahip data factory adını girin 
    3. İçin **AZURESSISNAME**, Azure SSIS IR adını girin 
    4. İçin **işlemi**, girin **Başlat**. 
    5. **Tamam**’ı seçin.  

        ![Runbook penceresi başlangıç](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-window.png)
7. İş penceresinde seçin **çıkış** döşeme. İş çıktı penceresinde iletisini görene kadar bekleyin **### tamamlandı ###** gördükten sonra **### başlangıç ###**. Bir Azure SSIS IR başlangıç yaklaşık 20 dakika sürer. Kapat **iş** penceresinde ve için geri alma **Runbook** penceresi.

    ![Azure SSIS IR - başlatıldı](./media/how-to-schedule-azure-ssis-integration-runtime/start-completed.png)
8.  Önceki iki adımı yineleyin, ancak kullanarak **DURDURMAK** değeri olarak **işlemi**. Runbook seçerek yeniden Başlat **Başlat** araç çubuğunda. Kaynak grubu adı, veri fabrikası adı ve Azure SSIS IR adını belirtin. İçin **işlemi**, girin **DURDURMAK**. 

    İş çıktı penceresinde iletisini görene kadar bekleyin **### tamamlandı ###** gördükten sonra **### durdurma ###**. Bir Azure SSIS IR durdurma Azure SSIS IR başlangıç olarak olarak uzun sürmez Kapat **iş** penceresinde ve için geri alma **Runbook** penceresi.

## <a name="create-schedules-for-the-runbook-to-startstop-the-azure-ssis-ir"></a>Zamanlamalar runbook'un Azure SSIS IR Başlat/Durdur oluşturma
Önceki bölümde oluşturduğunuz ya da başlatabilir veya durdurabilirsiniz Azure SSIS IR bir Azure Otomasyonu runbook Bu bölümde, runbook için iki zamanlamalar oluşturun. İlk zamanlama yapılandırırken, başlatma işlemi parametresini belirtin. Benzer şekilde, ikinci zamanlamasını yapılandırırken, durdurma işlemi için belirtin. Zamanlama oluşturmak için ayrıntılı adımlar için bkz: [bir zamanlama oluşturmak](../automation/automation-schedules.md#creating-a-schedule).

1. İçinde **Runbook** penceresinde, seçin **zamanlamaları**seçip **+ bir zamanlama Ekle** araç çubuğunda. 

    ![Azure SSIS IR - başlatıldı](./media/how-to-schedule-azure-ssis-integration-runtime/add-schedules-button.png)
2. İçinde **zamanlama Runbook** penceresinde, aşağıdaki adımları gerçekleştirin: 

    1. Seçin **runbook'a bir zamanlama Bağla**. 
    2. Seçin **yeni bir zamanlama oluşturmak**.
    3. İçinde **yeni zamanlama** penceresinde, türü **IR Başlat günlük** için **adı**. 
    4. İçinde **başlatır bölüm**, saat için geçerli saati aşan birkaç dakika saati belirtin. 
    5. İçin **yineleme**seçin **yinelenen**. 
    6. İçinde **Yinele her** bölümünde, select **gün**. 
    7. **Oluştur**’u seçin. 

        ![Azure SSIS IR başlangıç için zamanlama](./media/how-to-schedule-azure-ssis-integration-runtime/new-schedule-start.png)
3. Geçiş **parametreler ve çalıştırma ayarları** sekmesi. Kaynak grubu adı, veri fabrikası adı ve Azure SSIS IR adını belirtin. İçin **işlemi**, girin **Başlat**. **Tamam**’ı seçin. Seçin **Tamam** zamanlamayı yeniden görmek için **zamanlamaları** runbook'un sayfası. 

    ![Azure SSIS IR başlamanızı zamanlama](./media/how-to-schedule-azure-ssis-integration-runtime/start-schedule.png)
4. Adlı bir zamanlama oluşturmak için önceki iki adımı tekrarlayın **IR Durdur günlük**. Bu süre, saat için belirtilen en az 30 dakika sonra belirtin **IR Başlat günlük** zamanlama. İçin **işlemi**, belirtin **DURDURMAK**. 
5. İçinde **Runbook** penceresinde, seçin **işleri** sol menüde. Belirtilen zamanlama tarafından oluşturulan işleri görmelisiniz zamanları ve bunların durumlarını. Runbook test edildiğinde ne açtığınız için benzer çıktısını gibi iş ayrıntılarını görebilirsiniz. 

    ![Azure SSIS IR başlamanızı zamanlama](./media/how-to-schedule-azure-ssis-integration-runtime/schedule-jobs.png)
6. Tamamlandıktan sonra test, zamanlama bunları düzenleme ve seçerek devre dışı bırak **Hayır** için **etkin**. Seçin **zamanlamaları** soldaki menüde seçin **Başlat IR günlük/durdurma IR günlük**seçip **Hayır** için **etkin**. 

## <a name="create-webhooks-to-start-and-stop-the-azure-ssis-ir"></a>Azure SSIS IR durdurmak ve başlatmak için Web kancası oluşturma
' Ndaki yönergeleri izleyin [bir Web kancası oluşturma](../automation/automation-webhooks.md#creating-a-webhook) runbook için iki Web kancası oluşturulamadı. İlk bir işlem olarak başlangıç belirtin ve ikincisi için STOP işlemi belirtin. Her iki Web kancalarını herhangi bir yerde (bir metin dosyası veya gibi bir OneNote Not Defteri) için URL'leri kaydedin. Web etkinlikleri veri fabrikası ardışık düzeninde yapılandırırken bu URL'leri kullanabilirsiniz. Bir Web kancası oluşturma örneği gösterilen aşağıdaki resim Azure SSIS IR başlar:

1. İçinde **Runbook** penceresinde, seçin **Kancalarını** sol menüsünden ve select **+ Web kancası eklemek** araç çubuğunda. 

    ![Web kancası -> Web kancası ekleme](./media/how-to-schedule-azure-ssis-integration-runtime/add-web-hook-menu.png)
2. İçinde **ekleme Web kancası** penceresinde, seçin **yeni Web kancası oluşturma**, ve aşağıdaki eylemleri gerçekleştirebilirsiniz: 

    1. İçin **adı**, girin **StartAzureSsisIR**. 
    2. Onaylayın **etkin** ayarlanır **Evet**. 
    3. Kopya **URL** ve başka bir yere kaydedin. Bu önemli bir adımdır. URL daha sonra görmezsiniz. 
    4. **Tamam**’ı seçin. 

        ![Yeni bir Web kancası penceresi](./media/how-to-schedule-azure-ssis-integration-runtime/new-web-hook-window.png)
3. Geçiş **parametreler ve çalıştırma ayarları** sekmesi. Kaynak grubu adı, veri fabrikası adı ve Azure SSIS IR adını belirtin. İçin **işlemi**, girin **Başlat**. **Tamam**’a tıklayın. Sonra, **Oluştur**’a tıklayın. 

    ![Web kancası - parametreler ve çalıştırma ayarları](./media/how-to-schedule-azure-ssis-integration-runtime/webhook-parameters.png)
4. Adlı başka bir Web kancası oluşturmak için önceki üç adımı tekrarlayın **StopAzureSsisIR**. URL'yi kopyalamak unutmayın. Parametreler ve çalıştırma ayarları belirtirken girin **DURDURMAK** için **işlemi**. 

İçin iki URL'leri olmalıdır **StartAzureSsisIR** Web kancası ve diğer **StopAzureSsisIR** Web kancası. Bir HTTP POST isteği Azure SSIS IR Başlat/Durdur için bu URL'leri Gönder 

## <a name="create-and-schedule-a-data-factory-pipeline-that-startsstops-the-ir"></a>Oluşturma ve IR başlatır/durdurur, bir Data Factory işlem hattı zamanlama
Bu bölümde bir Web etkinliğini önceki bölümde oluşturduğunuz Web kancalarını çağırmak için nasıl kullanılacağını gösterir.

Oluşturduğunuz işlem hattını üç etkinliklerini oluşur. 

1. İlk **Web** etkinlik Azure SSIS IR başlatmak için ilk Web kancası çağırır 
2. **SSIS paketi yürütme** etkinlik veya **saklı yordam** etkinlik SSIS paketi çalışır.
3. İkinci **Web** etkinlik Azure SSIS IR durdurmak için Web kancası çağırır 

Oluşturun ve ardışık düzen test sonra zamanlama tetikleyici oluşturmak ve ardışık düzen ile ilişkilendirebilirsiniz. Zamanlama tetikleyici ardışık düzeni için bir zamanlama tanımlar. Günlük 23: 00 çalıştırmak üzere zamanlanmış bir tetikleyici oluşturmak varsayalım. Tetikleyici ardışık düzen 23: 00 her gün çalışır. Ardışık Düzen Azure SSIS IR başlatır, SSIS paketi yürütür ve ardından Azure SSIS IR durdurur 

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.    
2. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. **Yeni veri fabrikası** sayfasında **ad** için **MyAzureSsisDataFactory** adını girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin adınızMyAzureSsisDataFactory) ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
      Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Listede yalnızca veri fabrikası oluşturma için desteklenen konumlar gösterilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
   ![Data factory giriş sayfası](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Azure Data Factory Kullanıcı Arabirimini (UI) ayrı bir sekmede açmak için **Geliştir ve İzle**’ye tıklayın.

### <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. İçinde **başlama** sayfasında, **oluşturma ardışık düzen**. 

   ![Başlarken sayfası](./media/how-to-schedule-azure-ssis-integration-runtime/get-started-page.png)
2. İçinde **etkinlikleri** araç kutusu, genişletin **genel**, sürükle ve bırak **Web** ardışık düzen Tasarımcı yüzeyine etkinlik. İçinde **genel** sekmesinde **özellikleri** penceresinde, etkinliğin adını değiştirmek **StartIR**.

   ![İlk Web etkinlik - Genel sekmesi](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-general-tab.png)
3. Geçiş **ayarları** sekmesinde **özellikleri** penceresinde ve aşağıdaki eylemleri gerçekleştirebilirsiniz: 

    1. İçin **URL**, Azure SSIS IR başlatır Web kancası URL'sini yapıştırın 
    2. İçin **yöntemi**seçin **POST**. 
    3. İçin **gövde**, girin `{"message":"hello world"}`. 
   
        ![İlk Web etkinlik - Ayarlar sekmesi](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-settnigs-tab.png)

4. SSIS paketi yürütme etkinliği veya saklı yordam etkinliğinden sürükleyip **genel** bölümünü **etkinlikleri** araç. Etkinliğin adını ayarlayın **RunSSISPackage**. 

5. SSIS paketi yürütme etkinliği seçin,'ndaki yönergeleri izleyin. [SSIS etkinliği Azure Data Factory kullanarak bir SSIS paketi çalıştırmak](how-to-invoke-ssis-package-ssis-activity.md) etkinlik oluşturma işlemini tamamlamak için.  Yeterli sayıda başlaması 30 dakika sürer beri Azure SSIS IR kullanılabilirlik için beklenecek sık yeniden deneme belirttiğinizden emin olun. 

    ![Yeniden deneme ayarları](media/how-to-schedule-azure-ssis-integration-runtime/retry-settings.png)

6. Saklı yordam etkinliği seçin,'ndaki yönergeleri izleyin. [saklı yordam etkinliği Azure Data Factory kullanarak bir SSIS paketi çağırma](how-to-invoke-ssis-package-stored-procedure-activity.md) etkinlik oluşturma işlemini tamamlamak için. Başlaması 30 dakika sürer beri Azure SSIS IR kullanılabilirlik için beklediği bir Transact-SQL betiği yerleştirdiğinizden emin olun.
    ```sql
    DECLARE @return_value int, @exe_id bigint, @err_msg nvarchar(150)

    -- Wait until Azure-SSIS IR is started
    WHILE NOT EXISTS (SELECT * FROM [SSISDB].[catalog].[worker_agents] WHERE IsEnabled = 1 AND LastOnlineTime > DATEADD(MINUTE, -10, SYSDATETIMEOFFSET()))
    BEGIN
        WAITFOR DELAY '00:00:01';
    END

    EXEC @return_value = [SSISDB].[catalog].[create_execution] @folder_name=N'YourFolder',
        @project_name=N'YourProject', @package_name=N'YourPackage',
        @use32bitruntime=0, @runincluster=1, @useanyworker=1,
        @execution_id=@exe_id OUTPUT 

    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1

    EXEC [SSISDB].[catalog].[start_execution] @execution_id = @exe_id, @retry_count = 0

    -- Raise an error for unsuccessful package execution, check package execution status = created (1)/running (2)/canceled (3)/
    -- failed (4)/pending (5)/ended unexpectedly (6)/succeeded (7)/stopping (8)/completed (9) 
    IF (SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id = @exe_id) <> 7 
    BEGIN
        SET @err_msg=N'Your package execution did not succeed for execution ID: '+ CAST(@execution_id as nvarchar(20))
        RAISERROR(@err_msg, 15, 1)
    END
    ```

7. Bağlantı **Web** etkinliğe **SSIS paketi yürütme** veya **saklı yordam** etkinlik. 

    ![Web ve saklı yordam etkinlikleri Bağlan](./media/how-to-schedule-azure-ssis-integration-runtime/connect-web-sproc.png)

8. Sürükleme ve bırakma başka **Web** etkinliğinin sağındaki **SSIS paketi yürütme** etkinlik veya **saklı yordam** etkinlik. Etkinliğin adını ayarlayın **StopIR**. 
9. Geçiş **ayarları** sekmesinde **özellikleri** penceresinde ve aşağıdaki eylemleri gerçekleştirebilirsiniz: 

    1. İçin **URL**, Azure SSIS IR durdurur Web kancası URL'sini yapıştırın 
    2. İçin **yöntemi**seçin **POST**. 
    3. İçin **gövde**, girin `{"message":"hello world"}`.  
10. Bağlantı **SSIS paketi yürütme** etkinlik veya **saklı yordam** son etkinlik **Web** etkinlik.

    ![Tam ardışık düzen](./media/how-to-schedule-azure-ssis-integration-runtime/full-pipeline.png)
11. Ardışık düzen ayarlarını tıklayarak doğrula **doğrulama** araç çubuğunda. Kapat **ardışık düzen doğrulama raporu** tıklayarak **>>** düğmesi. 

    ![İşlem hattını doğrulama](./media/how-to-schedule-azure-ssis-integration-runtime/validate-pipeline.png)

### <a name="test-run-the-pipeline"></a>İşlem hattında test çalıştırması yapma

1. Seçin **testi** araç ardışık düzeni için. Çıktıda gördüğünüz **çıkış** alt bölmedeki penceresi. 

    ![Test çalıştırma](./media/how-to-schedule-azure-ssis-integration-runtime/test-run-output.png)
2. İçinde **Runbook** sayfa, Azure Automation hesabınız Azure SSIS IR durdurmak ve başlatmak için işler çalıştığını doğrulayabilirsiniz 

    ![Runbook işleri](./media/how-to-schedule-azure-ssis-integration-runtime/runbook-jobs.png)
3. SQL Server Management Studio'yu başlatın. İçinde **sunucuya Bağlan** penceresinde, aşağıdaki eylemleri gerçekleştirebilirsiniz: 

    1. İçin **sunucu adı**, belirtin  **&lt;Azure SQL veritabanınızı&gt;. database.windows.net**.
    2. Seçin **Seçenekleri >>**.
    3. İçin **veritabanına bağlan**seçin **SSISDB**.
    4. **Bağlan**’ı seçin. 
    5. Genişletme **tümleştirme hizmetleri kataloglar** -> **SSISDB** klasörünüze -> -> **projeleri** bilgisayarınızı SSIS Proje -> -> **paketleri** . 
    6. SSIS paketi sağ tıklatın ve seçin **raporları** -> **standart raporları** -> **tüm yürütmeleri**. 
    7. SSIS paketi çalıştırıldığını doğrulayın. 

        ![SSIS paketi Çalıştır doğrulayın](./media/how-to-schedule-azure-ssis-integration-runtime/verfiy-ssis-package-run.png)

### <a name="schedule-the-pipeline"></a>Ardışık Düzen zamanlama 
Ardışık Düzen beklediğiniz gibi çalışır, bu ardışık düzen sırasında belirtilen bir tempoyla çalıştırmak için bir tetikleyici oluşturabilirsiniz. Bir zamanlama tetikleyici sahip işlem hattı ilişkilendirme hakkında daha fazla bilgi için bkz [bir zamanlamaya göre ardışık tetiklemek](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

1. Ardışık düzeni için araç çubuğunda seçin **tetikleyici**seçip **yeni/Düzenle**. 

    ![Tetikleyici -> Yeni/Düzenle](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-new-menu.png)
2. İçinde **ekleme Tetikleyicileri** penceresinde, seçin **+ yeni**.

    ![Tetikleyiciler - Yeni Ekle](./media/how-to-schedule-azure-ssis-integration-runtime/add-triggers-new.png)
3. İçinde **Yeni Tetikleyici**, aşağıdaki eylemleri gerçekleştirebilirsiniz: 

    1. İçin **adı**, tetikleyici için bir ad belirtin. Aşağıdaki örnekte, **günlük olarak çalıştırılması** tetikleyici adıdır. 
    2. İçin **türü**seçin **zamanlama**. 
    3. İçin **başlangıç tarihi**, başlangıç tarihi ve saati seçin. 
    4. İçin **yineleme**, tetikleyici için tempoyla belirtin. Aşağıdaki örnekte, günlük kez. 
    5. İçin **son**, tarih ve saat seçerek belirtebilirsiniz **tarihini** seçeneği. 
    6. Seçin **etkinleştirilmiş**. Tetikleyici hemen Data Factory Çözümü yayımladıktan sonra etkinleştirilir. 
    7. **İleri**’yi seçin.

        ![Tetikleyici -> Yeni/Düzenle](./media/how-to-schedule-azure-ssis-integration-runtime/new-trigger-window.png)
4. İçinde **tetikleyici çalıştırmak parametreleri** sayfasında, uyarıyı gözden geçirme ve seçme **son**. 
5. Çözümü seçerek Data Factory yayımlamak **tümünü Yayımla** sol bölmede. 

    ![Tüm yayımlama](./media/how-to-schedule-azure-ssis-integration-runtime/publish-all.png)
6. Tetikleyici çalıştırır izlemek ve çalıştırmalarını kanalı için kullanın **İzleyici** sol sekmesinde. Ayrıntılı adımlar için bkz: [işlem hattını izleme](quickstart-create-data-factory-portal.md#monitor-the-pipeline).

    ![İşlem hattı çalıştırmaları](./media/how-to-schedule-azure-ssis-integration-runtime/pipeline-runs.png)
7. Çalıştırma sahip işlem hattı ilişkili etkinlik çalışması görüntülemek için ilk bağlantı seçin (**etkinlik görünümü çalışır**) içinde **Eylemler** sütun. Her işlem hattında etkinlik ilişkili üç Etkinlik çalışması bakın (ilk Web etkinlik, saklı yordam etkinliği ve ikinci Web etkinliği). Ardışık Düzen çalıştırır geri görüntülemek için geçiş yapmak için seçin **ardışık düzen** üst bağlantı.

    ![Etkinlik çalıştırmaları](./media/how-to-schedule-azure-ssis-integration-runtime/activity-runs.png)
8. Seçerek tetikleyici çalıştırır görüntüleyebilirsiniz **tetiklemek çalışır** yanındaki aşağı açılan listeden **ardışık düzen çalışır** üstünde. 

    ![Tetikleyici çalıştırmaları](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-runs.png)

## <a name="next-steps"></a>Sonraki adımlar
SSIS belgelerinde aşağıdaki makalelere bakın: 

- [Azure’da bir SSIS paketi dağıtma, çalıştırma ve izleme](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Azure’da SSIS kataloğuna bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Azure’da paket yürütmeyi zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Windows kimlik doğrulaması ile şirket içi veri kaynaklarına bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth)
