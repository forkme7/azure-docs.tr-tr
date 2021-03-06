---
title: Azure sayfa bloblarını benzersiz özelliklerini | Microsoft Docs
description: Azure sayfa blobları ve kullanım örnekleri ile örnek komut dosyaları da dahil olmak üzere faydaları genel bakış.
services: storage
author: anasouma
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/21/2018
ms.author: wielriac
ms.openlocfilehash: 5d1ad1555cb1e01e363456af5c50ecd090ce7147
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="unique-features-of-azure-page-blobs"></a>Azure sayfa bloblarını benzersiz özellikleri

Azure Storage üç tür blob depolama sunar: blok Blobları, ekleme Blobları ve sayfa bloblarını. Blok blobları bloklardan oluşan ve metin veya ikili dosyaları depolamak için ve verimli bir şekilde büyük dosyaları karşıya yükleme için idealdir. Ekleme blobları de yapılma bloklarını, ancak için iyileştirilmiş ekleme işlemleri, günlük kaydı senaryoları için ideal hale getirme. Sayfa bloblarını yapılma 512 baytlık sayfalarının 8 TB toplam boyutu ve öğeler için sık rasgele okuma/yazma işlemleri için tasarlanmıştır. Sayfa BLOB'ları, Azure Iaas disklerini temelidir. Bu makalede, sayfa bloblarını avantajları ve özelliklerini açıklayan üzerine odaklanır.

Sayfa bloblarını rastgele bayt aralıkları okuma/yazma yeteneği sağlamak 512 baytlık sayfaların koleksiyonudur. Bu nedenle, sayfa bloblarını işletim sistemi ve veri diskleri gibi dizin tabanlı ve seyrek veri yapılarını sanal makineler ve veritabanları için depolanması için idealdir. Örneğin, Azure SQL DB, sayfa bloblarını kalıcı temel alınan depolama alanı olarak veritabanları için kullanır. Ayrıca, sayfa bloblarını de genellikle aralık tabanlı güncelleştirmeli dosyalar için kullanılır.  

Azure sayfa bloblarını anahtar özellikleri, REST arabirimi, temel alınan depolama ve Azure sorunsuz geçiş yeteneklerine dayanıklılık verilmiştir. Bu özellikler sonraki bölümünde daha ayrıntılı olarak ele alınmıştır. Ayrıca, Azure sayfa bloblarını iki tür depolama üzerinde şu anda desteklenir: Premium depolama ve standart depolama. Premium depolama özellikle tutarlı yüksek performans ve düşük gecikme süresi premium sayfa bloblarını yüksek kullanıcı veri depolama veritabanları için ideal hale gerektiren iş yükleri için tasarlanmıştır.  Standart depolama daha maliyet gecikmeye duyarlı olmayan iş yüklerini çalıştırmak için etkili olur.

> [!WARNING]
> Sayfa bloblarını Premium depolama yalnızca VHD'ler olarak kullanılmak üzere tasarlanmıştır. Maliyetini önemli ölçüde daha büyük olabilir gibi diğer veri türleri Premium depolama, sayfa blobları depolamak Microsoft önermez. Blok blobları, bir VHD değil veri depolamak için kullanın.

## <a name="sample-use-cases"></a>Örnek kullanım durumları

Şimdi birkaç kullanım durumları ile Azure Iaas disklerini başlangıç sayfa BLOB'ları için tartışın. Azure sayfa BLOB'ları Azure Iaas sanal diskler platform omurga ' dir. Azure işletim sistemi ve veri diskleri burada verileri Azure Storage platform işlemi kalıcı ve en yüksek performans için sanal makineler için teslim sanal diskleri olarak uygulanır. Azure diskleri Hyper-V kalıcı [VHD biçimi](https://technet.microsoft.com/library/dd979539.aspx) ve olarak depolanan bir [sayfa blobu](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs) Azure depolama. Azure Iaas VM'ler için sanal diskler kullanarak ek olarak, sayfa bloblarını de PaaS ve DBaaS senaryoları şu anda sayfa bloblarını hızlı rastgele okuma-yazma işlemleri için veritabanını etkinleştirme SQL verileri depolamak için kullandığı Azure SQL DB hizmeti gibi etkinleştirin. Başka bir örnek işbirliği video düzenleme uygulamalar için paylaşılan ortam erişim için bir PaaS hizmeti varsa, sayfa bloblarını hızlı medya rastgele konumlarda erişmesini olabilir. Ayrıca, hızlı ve verimli düzenleme ve birden çok kullanıcı tarafından aynı ortamının birleştirme de sağlar. 

Azure Site Recovery, Azure Backup yanı sıra, çok sayıda üçüncü taraf geliştiriciler gibi birinci taraf Microsoft hizmetlerini sayfa blob'un REST arabirimini kullanarak endüstri lideri yenilikleri uyguladık. Azure üzerinde uygulanan benzersiz senaryolardan bazıları şunlardır: 
* Uygulama yönelik artımlı anlık görüntü Yönetimi: uygulamaları yararlanabilir sayfa blob anlık görüntüler ve REST API'leri uygulama denetim noktaları veri maliyetli çoğaltma yansıtılmasını olmadan kaydetmek için. Azure Storage tüm blob kopyalama gerektirmeyen sayfa bloblarını için yerel anlık görüntüleri destekler. Bu ortak anlık görüntü, API erişme ve farkları arasında anlık olarak kopyalanmasını etkinleştirin.
* Dinamik geçiş uygulama ve şirket verilerini buluta: şirket içi veri kopyalayın ve şirket içi VM çalışmaya devam ederken doğrudan bir Azure sayfa blobu yazma için REST API'leri kullanın. Hedef yakalanan sonra hızlı bir şekilde bu verileri kullanarak bir Azure VM yük devretme gerçekleştirebilirsiniz. Bu şekilde, VM'lerin geçirebilirsiniz ve VM ve yük devretme için gereken kapalı kalma süresi kullanmaya devam ederken veri geçişi arka planda gerçekleşir bu yana en az kapalı kalma süresi ile bulut için şirket içi sanal diskleri (dakika) kısa olacaktır.
* [SAS tabanlı](../common/storage-dotnet-shared-access-signature-part-1.md) paylaşılan birden çok okuyucular ve eşzamanlılık denetimi desteğiyle tek yazıcı gibi senaryolara olanak sağlar erişim.

## <a name="page-blob-features"></a>Sayfa blobu özellikleri

### <a name="rest-api"></a>REST API
Kullanmaya başlamak için aşağıdaki belgesine bakın [geliştirme sayfa BLOB'ları kullanarak](storage-dotnet-how-to-use-blobs.md). Örnek olarak, .NET için depolama istemci kitaplığı kullanılarak sayfa bloblarını erişmek nasıl bakın. 

Aşağıdaki diyagramda, hesap, kapsayıcıları ve sayfa BLOB'ları arasındaki genel ilişkileri açıklar.

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure1.png)

#### <a name="creating-an-empty-page-blob-of-a-specified-size"></a>Belirtilen boyutta bir boş sayfa blobu oluşturma
Bir sayfa blob'u oluşturmak için önce oluşturduğumuz bir **CloudBlobClient** nesnesiyle depolama hesabınız için blob depolama alanına erişim için ana URI (*pbaccount* Şekil 1'de) ile birlikte  **StorageCredentialsAccountAndKey** , aşağıdaki örnekte gösterildiği gibi nesne. Örnek, ardından bir başvuru oluşturma gösterir bir **CloudBlobContainer** nesne ve kapsayıcı oluşturma (*testvhds*) zaten yoksa. Ardından kullanarak **CloudBlobContainer** nesne, bir başvuru oluşturmak bir **CloudPageBlob** sayfa blob adı (os4.vhd) erişimi belirterek nesnesi. Sayfa blobu oluşturmak için arama [CloudPageBlob.Create](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.create?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Create_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) blob oluşturmak için en büyük boyutu geçirme. *BlobSize* 512 baytın katlarından biri olması gerekir.

```csharp
using Microsoft.WindowsAzure.StorageClient;
long OneGigabyteAsBytes = 1024 * 1024 * 1024;
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("testvhds");

// Create the container if it doesn't already exist.
container.CreateIfNotExists();

CloudPageBlob pageBlob = container.GetPageBlobReference("os4.vhd");
pageBlob.Create(16 * OneGigabyteAsBytes);
```

#### <a name="resizing-a-page-blob"></a>Bir sayfa blob'u yeniden boyutlandırma
Oluşturulduktan sonra bir sayfa blob'u yeniden boyutlandırmak için kullanın [yeniden boyutlandırma](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.resize?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Resize_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) API. İstenen boyutu 512 baytın katlarından biri olması gerekir.
```csharp
pageBlob.Resize(32 * OneGigabyteAsBytes); 
```

#### <a name="writing-pages-to-a-page-blob"></a>Bir sayfa blobu yazma sayfalarına
Sayfaları yazmak için [CloudPageBlob.WritePages](/library/microsoft.windowsazure.storageclient.cloudpageblob.writepages.aspx) yöntemi.  Bu, sıralı 4MBs kadar sayfalar kümesi yazmanıza olanak sağlar. Üzerine yazılan uzaklığı bir 512 baytlık sınırında başlatmanız gerekir (startingOffset % 512 == 0) ve 512 sınır - 1 sonlandır.  Aşağıdaki kod örneğinde nasıl çağrılacağını gösterir **WritePages** bir blob için:

```csharp
pageBlob.WritePages(dataStream, startingOffset); 
```

Sıralı sayfalar kümesi yazma isteği blob hizmetinde başarılı ve dayanıklılık ve dayanıklılık için çoğaltılır hemen yazma tamamlandığı ve başarı istemciye döndürülür.  

Diyagramda gösterildiği 2 ayrı yazma işlemleri:

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure2.png)

1.  Başlayarak bir yazma işlemi 0 uzunluğu 1024 bayt uzaklığı. 
2.  Uzunluğu 1024, 4096 başlayarak bir yazma işlemi uzaklığı 

#### <a name="reading-pages-from-a-page-blob"></a>Bir sayfa blobu okuma sayfaları
Sayfaları okumak için kullandığınız [CloudPageBlob.DownloadRangeToByteArray](/dotnet/api/microsoft.windowsazure.storage.blob.icloudblob.downloadrangetobytearray?view=azure-dotnet) sayfa blob üzerinden bir bayt aralığı okumaya yöntemi. Bu, tam blob veya blob herhangi uzaklığından başlayarak bir bayt aralığı indirmek sağlar. Okurken uzaklık katlarından biri 512 başlatmaya sahip değil. Bayt NUL sayfasından okurken, hizmet sıfır bayt döndürür.
```csharp
byte[] buffer = new byte[rangeSize];
pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, pageBlobOffset, rangeSize); 
```
Aşağıdaki şekilde 256 ve 4352 rangeSize BlobOffSet ile okuma işlemi gösterilmektedir. Döndürülen veriler içinde turuncu vurgulanır. Sıfır NUL sayfalar için döndürülür

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure3.png)

Seyrek doldurulmuş blob varsa, yalnızca geçerli sayfa bölgeleri sıfır bayt egressing için ödeme yapmaktan kaçınmak için ve indirme gecikme süresini azaltmak için indirme isteyebilirsiniz.  Hangi sayfaların veri tarafından yedeklenen belirlemek için [CloudPageBlob.GetPageRanges](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.getpageranges?view=azure-dotnet). Ardından, döndürülen aralıkları numaralandırır ve her aralığındaki veriler indirin. 
```csharp
IEnumerable<PageRange> pageRanges = pageBlob.GetPageRanges();

foreach (PageRange range in pageRanges)
{
    // Calculate the rangeSize
    int rangeSize = (int)(range.EndOffset + 1 - range.StartOffset);

    byte[] buffer = new byte[rangeSize];

    // Read from the correct starting offset in the page blob and place the data in the bufferOffset of the buffer byte array
    pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, range.StartOffset, rangeSize); 

    // TODO: use the buffer for the page range just read
} 
```

#### <a name="leasing-a-page-blob"></a>Bir sayfa blob'u kiralama
Kira blob'u işlemi oluşturur ve yazma için bir blob üzerinde bir kilit yönetir ve silme işlemleri. Bu işlem, aynı anda yalnızca bir istemci için blob yazabilirsiniz emin olmak için birden çok istemciden alınan bir sayfa blob'u burada erişiliyor senaryolarda kullanışlıdır. Azure diskleri, örneğin, yararlanır bu kiralama mekanizması disk yalnızca tek bir VM tarafından yönetildiğinden emin olun. Kilit süresi 15 ila 60 saniye olabilir veya sonsuz olabilir. Belgelerine bakın [burada](/rest/api/storageservices/lease-blob) daha fazla ayrıntı için.

> Almak için aşağıdaki bağlantıyı kullanın [kod örnekleri](/resources/samples/?service=storage&term=blob&sort=0 ) diğer birçok uygulama senaryoları için. 

Zengin REST API'lerini ek olarak, sayfa bloblarını paylaşılan erişim, dayanıklılık ve Gelişmiş güvenlik sağlar. Biz bu avantajlar sonraki paragrafları daha ayrıntılı olarak ele alınacaktır. 

### <a name="concurrent-access"></a>Eş zamanlı erişim
REST API sayfa blobları ve kiralama mekanizmasının uygulamaların birden fazla istemcilerden sayfa blobu erişmesine izin verir. Örneğin, depolama nesneleri ile birden çok kullanıcı paylaşan bir dağıtılmış bulut hizmeti oluşturmak gereken varsayalım. Büyük bir görüntü koleksiyonu için birkaç kullanıcıya hizmet veren bir web uygulaması olabilir. Bu uygulama için bir seçenek, bir VM ekli disklerle kullanmaktır. Bu INCLUDE downsides (i) bir disk yalnızca bu nedenle, ölçeklenebilirlik, esneklik, sınırlama ve riskleri artırma tek bir VM için eklenebilecek kısıtlaması. VM veya VM'de çalışan hizmeti ile ilgili bir sorun varsa, kira süresi dolana veya bozuk kadar sonra kira nedeniyle görüntünün erişilemez; ve (II) bir Iaas VM sonucunda ek maliyet. 

Azure Storage REST API'leri aracılığıyla doğrudan sayfa BLOB'ları kullanmak için alternatif bir seçenek vardır. Bu seçenek maliyetli Iaas VM'ler gereksinimini ortadan kaldırır, birden çok istemcilerden doğrudan erişim tam esneklik sunar, diskleri ekleme/ayırma gereğini ortadan kaldırarak Klasik dağıtım modeli basitleştirir ve VM sorunları riskini ortadan kaldırır. Ve aynı düzeyde bir disk olarak rasgele okuma/yazma işlemleri için performans sağlar

### <a name="durability-and-high-availability"></a>Dayanıklılık ve yüksek kullanılabilirlik
Standart ve premium depolama olan dayanıklı depolama burada sayfa blob verileri her zaman dayanıklılık ve yüksek kullanılabilirlik sağlamak için çoğaltılır. Bu Azure depolama artıklığı hakkında daha fazla bilgi için bkz [belgelerine](../common/storage-redundancy.md). Azure tutarlı bir şekilde teslim Kurumsal düzeyde dayanıklılık Iaas disklerini ve sayfa blobları, endüstri lideri ile sıfır % [değer yıllık hata oranı](https://en.wikipedia.org/wiki/Annualized_failure_rate). Diğer bir deyişle, Azure hiçbir zaman bir müşteri'nin sayfa blob verileri kaybetti. 

### <a name="seamless-migration-to-azure"></a>Azure'a sorunsuz geçiş
Müşteriler ve kendi özelleştirilmiş yedekleme çözümü uygulamak ilgilenen geliştiriciler için Azure yalnızca farkları tutun artımlı anlık görüntüleri de sunar. Bu özellik, büyük ölçüde yedekleme maliyetini düşürür ilk tam kopya maliyetini önler. Verimli bir şekilde okuma ve kopyalama fark verilere becerisinin yanı sıra, bu Azure üzerinde bir sınıf en iyi yedekleme ve olağanüstü durum kurtarma (DR) deneyimi baştaki geliştiricilerden daha da fazla yenilikleri sağlayan başka bir güçlü bir özelliktir. Azure kullanma Vm'leriniz için kendi yedekleme veya DR çözüm ayarlayabilirsiniz [Blob anlık görüntü](/rest/api/storageservices/snapshot-blob) ile birlikte [Get sayfa aralıklarını](/rest/api/storageservices/get-page-ranges) API ve [artımlı kopya Blob](/rest/api/storageservices/incremental-copy-blob) yapabilecekleriniz API DR için kolayca artımlı veri kopyalamak için kullanın. 

Ayrıca, birçok işletmenin zaten şirket içi veri merkezlerinde çalışan kritik iş yükleri vardır. İş yükü buluta geçirmek için ana endişelere birini kapalı kalma süresini veri ve beklenmeyen sorunlar riskini sonra geçiş kopyalamak için gerekli olacaktır. Çoğu durumda, bir showstopper buluta geçiş için kapalı kalma süresi olabilir. REST API BLOB ' sayfasını kullanarak, Azure kritik iş yükleri için en az kesintiyi ile bulut geçiş sağlayarak bu sorunu giderir. 

Bir anlık görüntünün nasıl alınacağı ve bir sayfa blob'u bir anlık görüntüden geri yükleme hakkında daha fazla örnekler için lütfen [artımlı anlık görüntülerini kullanarak bir yedekleme işlemi Kurulum](../../virtual-machines/windows/incremental-snapshots.md) makalesi.
