---
title: 'Öğretici: Azure Stream Analytics işleri ile Azure İşlevlerini çalıştırma | Microsoft Docs'
description: Bu öğreticide, Stream Analytics işlerine bir çıktı havuzu olarak Azure İşlevlerini yapılandırma hakkında bilgi edineceksiniz.
services: stream-analytics
author: jasonwhowell
manager: kfile
ms.service: stream-analytics
ms.topic: tutorial
ms.custom: mvc
ms.workload: data-services
ms.date: 04/09/2018
ms.author: jasonh
ms.reviewer: jasonh
ms.openlocfilehash: 1d33c3f0a4c36dc681aaa42bc68ae56eec234401
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="run-azure-functions-from-azure-stream-analytics-jobs"></a>Azure Stream Analytics işlerinden Azure İşlevleri’ni çalıştırma 

İşlevleri Stream Analytics işinin çıktı havuzlarından biri olarak yapılandırarak, Azure Stream Analytics'ten Azure İşlevleri’ni çalıştırabilirsiniz. İşlevler, Azure veya üçüncü taraf hizmetlerinde gerçekleşen olayların tetiklediği kodu uygulamanıza olanak tanıyan olay odaklı, isteğe bağlı bir işlem deneyimidir. İşlevlerin tetikleyicilere yanıt vermeye yönelik bu özelliği, hizmeti Stream Analytics işleri için doğal bir çıktı haline getirmektedir.

Stream Analytics, HTTP tetikleyicileri aracılığıyla İşlevleri çağırır. İşlevlerin çıkış bağdaştırıcısı, kullanıcıların Stream Analytics sorgularına göre olayların tetiklenmesini sağlayacak şekilde Stream Analytics’e İşlevleri bağlamasına olanak tanır. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Akış Analizi işi oluşturma
> * Azure işlevi oluşturma
> * Azure işlevini işinizin çıktısı olarak yapılandırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="configure-a-stream-analytics-job-to-run-a-function"></a>İşlev çalıştırmak için bir Stream Analytics işi yapılandırma 

Bu bölümde, Azure Redis Cache’e veri yazan bir işlev çalıştırmak üzere Stream Analytics işini yapılandırma işlemi gösterilmektedir. Stream Analytics işi Azure Event Hubs’dan olayları okur ve işlevi çağıran bir sorgu çalıştırır. Bu işlev, Stream Analytics işinden verileri okur ve Azure Redis Cache’e yazar.

![Azure hizmetleri arasındaki ilişkileri gösteren diyagram](./media/stream-analytics-with-azure-functions/image1.png)

Bu görevi gerçekleştirmek için aşağıdaki adımlar gereklidir:
* [Girdi olarak Event Hubs ile bir Stream Analytics işi oluşturma](#create-stream-analytics-job-with-event-hub-as-input)  
* [Bir Azure Redis Cache örneği oluşturma](#create-an-azure-redis-cache)  
* [Azure İşlevleri’nde Azure Redis Cache’e veri yazabilen bir işlev oluşturma](#create-an-azure-function-that-can-write-data-to-the-redis-cache)    
* [Çıktı olarak işlevle Stream Analytics işini güncelleştirme](#update-the-stream-analytic-job-with-azure-function-as-output)  
* [Sonuçlar için Azure Redis Cache’i kontrol etme](#check-redis-cache-for-results)  

## <a name="create-a-stream-analytics-job-with-event-hubs-as-input"></a>Girdi olarak Event Hubs ile bir Stream Analytics işi oluşturma

Bir olay hub’ı oluşturmak, olay oluşturucu uygulamasını başlamak ve bir Stream Analytics işi oluşturmak için [Gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md) öğreticisini takip edin. (Sorgu ve çıktı oluşturma adımlarını atlayın. Bunun yerine, İşlevler çıkışını ayarlamak için aşağıdaki bölümlere bakın.)

## <a name="create-an-azure-redis-cache-instance"></a>Bir Azure Redis Cache örneği oluşturma

1. [Önbellek oluşturma](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache) bölümünde açıklanan adımları kullanarak Azure Redis Cache’te bir önbellek oluşturun.  

2. Önbelleği oluşturduktan sonra **Ayarlar** altında **Erişim Anahtarları**’nı seçin. **Birincil bağlantı dizesi**’ni not edin.

   ![Azure Redis Cache bağlantı dizesinin ekran görüntüsü](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-a-function-in-azure-functions-that-can-write-data-to-azure-redis-cache"></a>Azure İşlevleri’nde Azure Redis Cache’e veri yazabilen bir işlev oluşturma

1. İşlevler belgesinin [İşlev uygulaması oluşturma](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) bölümüne bakın. Bu bölümde CSharp dili kullanılarak bir işlev uygulaması ve [Azure İşlevleri’nde HTTP ile tetiklenen işlev](../azure-functions/functions-create-first-azure-function.md#create-function) oluşturma adımları gösterilir.  

2. **run.csx** işlevine göz atın. Aşağıdaki kodla güncelleştirin. (“\<redis cache bağlantı dizeniz buraya gelecek\>” ifadesini, önceki bölümde aldığınız Azure Redis Cache birincil bağlantı dizesi ile değiştirdiğinizden emin olun.)  

   ```csharp
   using System;
   using System.Net;
   using System.Threading.Tasks;
   using StackExchange.Redis;
   using Newtonsoft.Json;
   using System.Configuration;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");
    
      // Get the request body
      dynamic dataArray = await req.Content.ReadAsAsync<object>();

      // Throw an HTTP Request Entity Too Large exception when the incoming batch(dataArray) is greater than 256 KB. Make sure that the size value is consistent with the value entered in the Stream Analytics portal.

      if (dataArray.ToString().Length > 262144)
      {        
         return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
      var connection = ConnectionMultiplexer.Connect("<your redis cache connection string goes here>");
      log.Info($"Connection string.. {connection}");
    
      // Connection refers to a property that returns a ConnectionMultiplexer
      IDatabase db = connection.GetDatabase();
      log.Info($"Created database {db}");
    
      log.Info($"Message Count {dataArray.Count}");

      // Perform cache operations using the cache object. For example, the following code block adds few integral data types to the cache
      for (var i = 0; i < dataArray.Count; i++)
      {
        string time = dataArray[i].time;
        string callingnum1 = dataArray[i].callingnum1;
        string key = time + " - " + callingnum1;
        db.StringSet(key, dataArray[i].ToString());
        log.Info($"Object put in database. Key is {key} and value is {dataArray[i].ToString()}");
       
      // Simple get of data types from the cache
      string value = db.StringGet(key);
      log.Info($"Database got: {value}");
      }

      return req.CreateResponse(HttpStatusCode.OK, "Got");
    }    

   ```

   Stream Analytics işlevden "HTTP İstek Varlığı Çok Büyük" özel durumunu aldığında İşlevler’e gönderdiği toplu işlerin boyutunu azaltır. İşlevinizde Stream Analytics’in aşırı büyük toplu işler göndermediğinden emin olmak için aşağıdaki kodu kullanın. İşlevde kullanılan en büyük toplu iş sayı ve boyut değerlerinin Stream Analytics portalına girilen değerlerle tutarlı olduğundan emin olun.

   ```csharp
   if (dataArray.ToString().Length > 262144)
      {        
        return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
   ```

3. Tercih ettiğiniz bir metin düzenleyicisinde **project.json** adlı bir JSON dosyası oluşturun. Aşağıdaki kodu kullanın ve yerel bilgisayarınıza kaydedin. Bu dosya, C# işlevinin gerektirdiği NuGet paket bağımlılıklarını içerir.  
   
   ```json
       {
         "frameworks": {
             "net46": {
                 "dependencies": {
                     "StackExchange.Redis":"1.1.603",
                     "Newtonsoft.Json": "9.0.1"
                 }
             }
         }
     }

   ```
 
4. Azure portalına geri dönün. **Platform özellikleri** sekmesinden işlevinize göz atın. **Geliştirme Araçları** altında **App Service Düzenleyicisi**’ni seçin. 
 
   ![App Service Düzenleyicisi ekran görüntüsü](./media/stream-analytics-with-azure-functions/image3.png)

5. App Service Düzenleyicisi’nde kök dizininize sağ tıklayın ve **project.json** dosyasını karşıya yükleyin. Karşıya yükleme başarılı olduktan sonra sayfayı yenileyin. Şu anda **project.lock.json** adlı otomatik olarak oluşturulmuş bir dosya görmeniz gerekir. Otomatik olarak oluşturulan dosya, project.json dosyasındaki .dll dosyalarının başvurularını içerir.  

   ![App Service Düzenleyicisi ekran görüntüsü](./media/stream-analytics-with-azure-functions/image4.png)

 

## <a name="update-the-stream-analytics-job-with-the-function-as-output"></a>Çıktı olarak işlevle Stream Analytics işini güncelleştirme

1. Stream Analytics işinizi Azure portalında açın.  

2. İşlevinize göz atın ve **Genel Bakış** > **Çıktılar** > **Ekle**’yi seçin. Yeni bir çıktı eklemek için havuz seçeneği olarak **Azure İşlevi**’ni seçin. Yeni İşlevler çıkış bağdaştırıcısı aşağıdaki özelliklerle kullanılabilir:  

   |**Özellik adı**|**Açıklama**|
   |---|---|
   |Çıktı diğer adı| İşin sorgusunda çıktıya başvurmak için kullandığınız kolay ad. |
   |İçeri aktarma seçeneği| Geçerli abonelikteki işlevi kullanabilir veya işlev başka bir abonelikte bulunuyorsa ayarları el ile sağlayabilirsiniz. |
   |İşlev Uygulaması| İşlevler uygulamanızın adı. |
   |İşlev| İşlevler uygulamanızdaki işlevin adı (run.csx işlevinizin adı).|
   |En Büyük Toplu İş Boyutu|İşlevinize gönderilen her bir çıktı toplu işinin en büyük boyutunu ayarlar. Varsayılan olarak, bu değer 256 KB olarak ayarlanır.|
   |En Büyük Toplu İş Sayısı|İşleve gönderilen her toplu işteki en büyük olay sayısını belirtir. Varsayılan değer 100’dür. Bu özellik isteğe bağlıdır.|
   |Anahtar|Başka bir abonelikteki işlevi kullanmanıza olanak sağlar. İşlevinize erişmek için anahtar değerini sağlayın. Bu özellik isteğe bağlıdır.|

3. Çıktı diğer adı için bir ad belirtin. Bu öğreticide bu adı **saop1** olarak belirttik (tercih ettiğiniz herhangi bir adı kullanabilirsiniz). Diğer ayrıntıları girin.  

4. Stream Analytics işinizi açın ve sorguyu aşağıdaki gibi güncelleştirin. (Çıktı havuzunu farklı şekilde adlandırdıysanız “saop1” ifadesini değiştirmeyi unutmayın.)  

   ```sql
    SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        INTO saop1
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
           JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum
   ```

5. Komut satırında aşağıdaki komutu çalıştırarak telcodatagen.exe uygulamasını başlatın (`telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]` biçimini kullanın):  
   
   **telcodatagen.exe 1000 .2 2**
    
6.  Stream Analytics işini başlatın.

## <a name="check-azure-redis-cache-for-results"></a>Sonuçlar için Azure Redis Cache’i kontrol etme

1. Azure portalına gidin ve Azure Cache’i bulun. **Konsol**’u seçin.  

2. Verilerinizin Redis önbelleğinde olduğunu doğrulamak için [Redis önbelleği komutları](https://redis.io/commands)’nı kullanın. (Komut Get {key} biçimini alır.) Örnek:

   **Get "19.12.2017 21:32:24 - 123414732"**

   Bu komut, belirtilen anahtarın değerini yazdırmalıdır:

   ![Azure Redis Cache çıktısının ekran görüntüsü](./media/stream-analytics-with-azure-functions/image5.png)

## <a name="known-issues"></a>Bilinen sorunlar

Azure portalında En Büyük Toplu İş Boyutu/En Büyük Toplu İş Sayısı değerini boş (varsayılan) olarak sıfırlamaya çalıştığınızda değer kaydedildikten sonra daha önce girilen değerle değiştirilir. Bu durumda bu alanların varsayılan değerlerini el ile girin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, akış işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, durdurup daha sonra gerektiğinde yeniden başlatabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak bu hızlı başlangıçla oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın.  
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure İşlevi çalıştıran basit bir Stream Analytics işi oluşturdunuz. Stream Analytics işleri hakkında daha fazla bilgi almak için sonraki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Stream Analytics işlerinde JavaScript kullanıcı tanımlı işlevleri çalıştırma](stream-analytics-javascript-user-defined-functions.md)