---
title: Azure Table storage veya Azure Cosmos Node.js DB'den nasıl kullanılacağını | Microsoft Docs
description: Azure Table storage veya Azure Cosmos DB kullanarak bulutta yapılandırılmış veri depolayın.
services: cosmos-db
documentationcenter: nodejs
author: SnehaGunda
manager: kfile
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 04/05/2018
ms.author: sngun
ms.openlocfilehash: 3f1908a6c2d129da44e0719b2cf69cf09baef356
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Node.js'den Azure Table storage kullanma
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, Azure depolama tablo hizmeti ya da Azure Cosmos DB bir Node.js uygulaması kullanarak genel senaryolar gerçekleştirme gösterilmektedir.

## <a name="create-an-azure-service-account"></a>Bir Azure hizmet hesabı oluşturma

[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

### <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma

[!INCLUDE [cosmos-db-create-storage-account](../../includes/cosmos-db-create-storage-account.md)]

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Bir Azure Cosmos DB tablo API hesabı oluşturma

[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

## <a name="configure-your-application-to-access-azure-storage"></a>Azure depolama alanına erişmek için uygulamanızı yapılandırın
Azure Storage veya Azure Cosmos DB kullanmak için bir dizi depolama REST Hizmetleri ile iletişim kolaylık kitaplıkları içerir Node.js için Azure depolama SDK'sı gerekir.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Paketi yüklemek için düğüm paketi Yöneticisi (NPM) kullanın
1. Bir komut satırı arabirimi gibi kullandığınız **PowerShell** (Windows) **Terminal** (Mac) veya **Bash** (UNIX) ve uygulamanızı oluşturduğunuz klasöre gidin.
2. Tür **npm yükleme azure depolama** komut penceresinde. Komut çıktısı aşağıdaki örneğe benzer.

       azure-storage@0.5.0 node_modules\azure-storage
       +-- extend@1.2.1
       +-- xmlbuilder@0.4.3
       +-- mime@1.2.11
       +-- node-uuid@1.4.3
       +-- validator@3.22.2
       +-- underscore@1.4.4
       +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
       +-- xml2js@0.2.7 (sax@0.5.2)
       +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
3. El ile çalıştırabilirsiniz **ls** doğrulamak için komutu bir **node_modules** klasörü oluşturuldu. Bu klasör içinde bulacaksınız **azure depolama** depolama birimine erişmesi gereken kitaplıkları içeren paket.

### <a name="import-the-package"></a>Paket alma
En üst kısmına aşağıdaki kodu ekleyin **server.js** uygulamanızı dosyasında:

```nodejs
var azure = require('azure-storage');
```

## <a name="add-an-azure-storage-connection"></a>Bir Azure depolama bağlantı Ekle
Azure modül Azure depolama hesabınıza bağlanmak için gerekli bilgileri için ortam değişkenleri AZURE_STORAGE_ACCOUNT ve AZURE_STORAGE_ACCESS_KEY veya AZURE_STORAGE_CONNECTION_STRING okur. Bu ortam değişkenleri ayarlanmamışsa çağrılırken hesap bilgileri belirtmelisiniz **TableService**. Örneğin, aşağıdaki kod oluşturur bir **TableService** nesnesi:

```nodejs
var tableSvc = azure.createTableService('myaccount', 'myaccesskey');
```

## <a name="add-an-azure-comsos-db-connection"></a>Bir Azure Comsos DB Bağlantısı Ekle
Bir Azure Cosmos DB bağlantısı eklemek için oluşturma bir **TableService** nesne ve hesap adını, birincil anahtar ve uç nokta belirtin. Bu değerleri kopyalayabilirsiniz **ayarları** > **bağlantı dizesi** Cosmos DB hesabınız için Azure Portalı'nda. Örneğin:

```nodejs
var tableSvc = azure.createTableService('myaccount', 'myprimarykey', 'myendpoint');
```  

## <a name="create-a-table"></a>Bir tablo oluşturma
Aşağıdaki kod oluşturur bir **TableService** nesne ve yeni bir tablo oluşturmak için kullanır. 

```nodejs
var tableSvc = azure.createTableService();
```

Çağrı **createTableIfNotExists** zaten yoksa, belirtilen ada sahip yeni bir tablo oluşturur. Aşağıdaki örnek, zaten yoksa, 'mytable' adlı yeni bir tablo oluşturur:

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

`result.created` Olan `true` yeni bir tablo oluşturduysanız ve `false` tablo zaten varsa. `response` İsteğiyle ilgili bilgileri içerir.

### <a name="filters"></a>Filtreler
İsteğe bağlı kullanılarak gerçekleştirilen işlemler için filtre uygulayabilirsiniz **TableService**. İşlemleri filtreleme içerebilir günlüğe kaydetme, otomatik yeniden denemeler vb. İmzalı bir yöntem uygulayan nesneler filtreleri şunlardır:

```nodejs
function handle (requestOptions, next)
```

İstek seçenekleri önişleme yaptıktan sonra yöntemini çağırmalı **sonraki**, bir geri çağırma aşağıdaki imzayla geçirme:

```nodejs
function (returnObject, finalCallback, next)
```

İşleme sonra bu geri çağırma **returnObject** (yanıt istek sunucuya), geri çağırma ya da çağırmanız gerekir **sonraki** diğer filtreleri işleme devam etmek için varsa ya da yalnızca Çağırma **finalCallback** hizmet başlatma sonuna Aksi takdirde.

Yeniden deneme mantığını uygulaması iki filtre Node.js için Azure SDK'sı ile birlikte **ExponentialRetryPolicyFilter** ve **LinearRetryPolicyFilter**. Aşağıdaki oluşturur bir **TableService** kullanan nesneyi **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Bir varlık eklemek için önce varlık özelliklerinizi tanımlayan bir nesne oluşturun. Tüm varlıklar içermesi gereken bir **PartitionKey** ve **RowKey**, varlık için benzersiz tanımlayıcı olduğu.

* **PartitionKey** -varlık depolandığı bölüm belirler.
* **RowKey** - benzersiz olarak varlığın bölüm içinde tanımlar.

Her ikisi de **PartitionKey** ve **RowKey** dize değerleri olmalıdır. Daha fazla bilgi için bkz: [tablo hizmeti veri modelini anlama](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Bir varlık tanımlayan bir örnek verilmiştir. Unutmayın **vade tarihi** bir türü olarak tanımlanmış **Edm.DateTime**. Türünü belirtme isteğe bağlıdır ve türleri çıkarımı yapılan belirtilen değilse.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> Ayrıca bir **zaman damgası** bir varlık eklendiğinde veya, Azure tarafından ayarlanan her kayıt için alan.
>
>

Aynı zamanda **entityGenerator** varlıklar oluşturmak için. Aşağıdaki örnek, varlık aynı görevini kullanarak oluşturur **entityGenerator**.

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out the trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

Bir varlığı tablonuza eklemek için varlık nesnesine geçirmek **insertEntity** yöntemi.

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

İşlem başarılı olursa `result` içeren [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) eklenen kaydın ve `response` işlemi hakkında bilgiler içerir.

Örnek yanıt:

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> Varsayılan olarak, **insertEntity** parçası olarak eklenen varlık döndürmüyor `response` bilgi. Planı bu varlık üzerinde başka işlemler gerçekleştirme veya bilgileri önbelleğe almak istediğiniz bir parçası olarak döndürülen yararlı olabilir `result`. Etkinleştirerek bunu yapabilirsiniz **echoContent** gibi:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a>Bir varlığı güncelleştirir
Var olan bir varlığı güncelleştirmek için kullanılabilir birden çok yöntemi vardır:

* **replaceEntity** -değiştirme tarafından var olan bir varlığı güncelleştirir.
* **mergeEntity** -var olan varlık kümesine yeni özellik değerlerinin birleştirerek var olan bir varlığı güncelleştirir.
* **insertOrReplaceEntity** -değiştirme tarafından var olan bir varlığı güncelleştirir. Hiçbir varlık varsa, yeni bir tane eklenir.
* **insertOrMergeEntity** -mevcut yeni özellik değerlerinin birleştirerek var olan bir varlığı güncelleştirir. Hiçbir varlık varsa, yeni bir tane eklenir.

Aşağıdaki örnek, bir varlık kullanarak güncelleştirme gösterir **replaceEntity**:

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> Varsayılan olarak, bir varlık güncelleştirme güncelleştirilen veri daha önce başka bir işlem tarafından değiştirilip değiştirilmediğini görmek için kontrol etmez. Eşzamanlı güncelleştirmelerini desteklemek için:
>
> 1. Güncelleştirilecek nesnenin ETag alın. Bu bir parçası olarak döndürülür `response` tüm ilgili varlık işlemi için ve alınabilir `response['.metadata'].etag`.
> 2. Bir varlık üzerinde bir güncelleştirme işlemi gerçekleştirirken, daha önce yeni varlığa alınan ETag bilgileri ekleyin. Örneğin:
>
>       entity2 ['.metadata'] .etag currentEtag; =
> 3. Güncelleştirme işlemi gerçekleştirin. Varlık, uygulamanızın başka bir örneği gibi ETag değeri alınan beri değiştirilmişse bir `error` istekte belirtilen güncelleştirme koşulu karşılanmadı belirten döndürülür.
>
>

İle **replaceEntity** ve **mergeEntity**, güncelleştirilen varlığı yoksa güncelleştirme işlemi başarısız sonra; bu nedenle, bir varlık mi bağımsız olarak depolamak istiyorsanız, bunu zaten yoksa, kullanın **insertOrReplaceEntity** veya **insertOrMergeEntity**.

`result` Başarılı güncelleştirme işlemleri için içeren **Etag** güncelleştirilmiş varlık.

## <a name="work-with-groups-of-entities"></a>Varlıkları gruplarıyla çalışma
Bazen birden çok sunucu tarafından işleme atomik emin olmak için birlikte toplu iş işlemlerinde göndermek için mantıklıdır. Bunu yapmaya yönelik kullanmak **TableBatch** toplu oluşturmak için sınıf ve sonra **executeBatch** yöntemi **TableService** toplu işlemleri gerçekleştirmek için.

 Aşağıdaki örnek, bir toplu işte iki varlık gönderme gösterir:

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash the dishes'},
  dueDate: {'_':new Date(2015, 6, 20)}
};

var batch = new azure.TableBatch();

batch.insertEntity(task1, {echoContent: true});
batch.insertEntity(task2, {echoContent: true});

tableSvc.executeBatch('mytable', batch, function (error, result, response) {
  if(!error) {
    // Batch completed
  }
});
```

Başarılı toplu işlemler için `result` her bir işlemde toplu iş bilgilerini içerir.

### <a name="work-with-batched-operations"></a>Toplu işlemleri ile çalışma
Bir toplu iş görüntüleyerek eklenen işlemleri inceleyebilirsiniz `operations` özelliği. İşlemleri ile çalışmak için aşağıdaki yöntemleri de kullanabilirsiniz:

* **Clear** -tüm işlemler bir batch temizler.
* **getOperations** -bir işlem toplu işten alır.
* **hasOperations** -toplu işlemler varsa true değerini döndürür.
* **removeOperations** -bir işlem kaldırır.
* **boyutu** -toplu işlemde işlemlerinin sayısını döndürür.

## <a name="retrieve-an-entity-by-key"></a>Anahtara göre bir varlık alma
Göre belirli bir varlık döndürülecek **PartitionKey** ve **RowKey**, kullanın **retrieveEntity** yöntemi.

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains the entity
  }
});
```

Bu işlem tamamlandıktan sonra `result` varlık içerir.

## <a name="query-a-set-of-entities"></a>Varlık kümesi sorgulama
Bir tabloyu sorgulamak için kullanın **TableQuery** nesne aşağıdaki yan tümceleri kullanarak bir sorgu ifadesi oluşturmak için:

* **seçin** -sorgudan döndürülecek alanları.
* **Burada** -where yan tümcesi.

  * **ve** - bir `and` koşul.
  * **veya** - bir `or` koşul.
* **üst** -getirmek için öğe sayısı.

Aşağıdaki örnek, bir PartitionKey 'hometasks' ın üst beş öğeleri döndüren bir sorgu oluşturur.

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

Çünkü **seçin** , tüm alanları döndürülen kullanılmaz. Tablo sorgusu gerçekleştirmek için kullanın **queryEntities**. Aşağıdaki örnekte, varlıklar 'mytable' döndürmek için bu sorguyu kullanır.

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

Başarılı olursa, `result.entries` sorguyla eşleşen varlıkları dizisi içerir. Sorgu tüm varlıkları döndüremedi varsa `result.continuationToken` olan olmayan*null* ve üçüncü parametresi olarak kullanılabilir **queryEntities** daha fazla sonuç almak için. İlk sorguyu için *null* üçüncü parametre.

### <a name="query-a-subset-of-entity-properties"></a>Giriş özellikleri alt kümesi sorgulama
Sorguda bir tabloya bir varlık birkaç alanları alabilir.
Bu, bant genişliğini azaltır ve özellikle büyük varlıklar için sorgu performansını iyileştirebilir. Kullanım **seçin** yan tümcesi ve döndürmek için alanların adlarını geçirin. Örneğin, aşağıdaki sorguyu yalnızca döndürür **açıklama** ve **vade tarihi** alanları.

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlığın bölüm ve satır anahtarını kullanarak silebilirsiniz. Bu örnekte, **task1** nesnesini içeren **RowKey** ve **PartitionKey** silinecek varlığın değerleri. Nesne geçirilir sonra **deleteEntity** yöntemi.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'}
};

tableSvc.deleteEntity('mytable', task, function(error, response){
  if(!error) {
    // Entity deleted
  }
});
```

> [!NOTE]
> Öğeleri silerken bir öğe başka bir işlem tarafından değiştirilip değiştirilmediğini emin olmak için Etag'ler kullanmayı düşünün. Bkz: [bir varlığı güncelleştirmek](#update-an-entity) Etag'ler kullanma hakkında bilgi için.
>
>

## <a name="delete-a-table"></a>Bir tablo silme
Aşağıdaki kod, bir depolama hesabından bir tablo siler.

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

Tablo var olup olmadığından emin değilseniz kullanmak **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Devamlılık belirteçleri kullanın
İçin devamlılık belirteci tablolar için sonuçları büyük miktarlarda sorgulanırken arayın. Sorgunuz için bir devamlılık belirteci bulunduğunda tanımak için oluşturacağınız değil, farkına varmazsınız kullanılabilir büyük miktarlarda verinin olabilir.

**Sonuçları** nesnesi döndürülen varlık kümeleri sorgulama sırasında bir `continuationToken` böyle bir belirteç bulunduğunda özelliği. Ardından bu sorgu gerçekleştirirken bölüm ve tablo varlıkları arasında taşımak devam etmek için kullanabilirsiniz.

Sorgulama yapıldığında sağlayabilir bir `continuationToken` sorgu nesne örneği geri çağırma işlevi arasındaki parametre:

```nodejs
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

İnceleyin, `continuationToken` nesnesi bulacaksınız özellikler gibi `nextPartitionKey`, `nextRowKey` ve `targetLocation`, tüm sonuçları yinelemek için kullanılabilecek.

## <a name="work-with-shared-access-signatures"></a>Paylaşılan erişim imzaları ile çalışma
Paylaşılan erişim imzaları (SAS) depolama hesabı adı veya anahtarları sağlamadan tabloları ayrıntılı erişim sağlamak için güvenli bir yoludur. SAS genellikle, verileri kayıtlarını sorgulamak için mobil uygulama izin verme gibi sınırlı erişim sağlamak için kullanılır.

SAS kullanarak bulut tabanlı bir hizmete gibi güvenilir bir uygulama oluşturur **generateSharedAccessSignature** , **TableService**ve bir mobil uygulama gibi güvenilmeyen veya yarı güvenilir bir uygulama sağlar. SAS SAS sahibi verilen erişim düzeyini yanı sıra SAS geçerli olduğu başlangıç ve bitiş tarihleri açıklar, bir ilke kullanılarak oluşturulur.

Aşağıdaki örnek, sorgu ('r') tablo için SAS sahibi izin veren yeni bir paylaşılan erişim ilkesi oluşturur ve 100 dakika oluşturulduktan sonra süresi dolar.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
};

var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
var host = tableSvc.host;
```

SAS sahibi tablo erişmeyi denediğinde, gerekli olduğu gibi ana bilgisayar bilgileri de belirtmeniz gerekir unutmayın.

İstemci uygulama ile SAS kullanan **TableServiceWithSAS** tablo karşı işlemlerini gerçekleştirmek için. Aşağıdaki örnek, tabloya bağlanır ve bir sorgu gerçekleştirir.

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains the entities
  }
});
```

SAS yalnızca sorgu erişimle oluşturduğu için Ekle, Güncelleştir veya varlıklarını silme çalışırsanız, bir hata döndürülür.

### <a name="access-control-lists"></a>Erişim denetim listeleri
Erişim ilkesi için bir SAS ayarlamak için erişim denetim listesi (ACL) da kullanabilirsiniz. Bu tabloya erişim, ancak her istemci için farklı erişim ilkeleri sağlamak birden çok istemciye izin vermek istiyorsanız yararlıdır.

Bir ACL her ilkesiyle ilişkili bir Kimliğe sahip bir dizi erişim ilkeleri kullanılarak uygulanır. Aşağıdaki örnekte, iki ilke, 'User1' diğeri için 'kullanıcı2' tanımlar:

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Aşağıdaki örnek için geçerli ACL alır **hometasks** tablo ve yeni ilkeleri kullanılarak ekler **setTableAcl**. Bu yaklaşım sağlar:

```nodejs
var extend = require('extend');
tableSvc.getTableAcl('hometasks', function(error, result, response) {
if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

ACL ayarladıktan sonra daha sonra bir ilke kimliği dayalı bir SAS oluşturabilirsiniz. Aşağıdaki örnek, 'kullanıcı2' için yeni bir SAS oluşturur:

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın.

* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
* [Node.js için Azure depolama SDK](https://github.com/Azure/azure-storage-node) github'daki.
* [Azure Node.js geliştiricileri için](https://docs.microsoft.com/javascript/azure/?view=azure-node-latest)
* [Node.js web uygulaması oluşturma](../app-service/app-service-web-get-started-nodejs.md)
* [Derleme ve Azure bulut hizmeti bir Node.js uygulamasını dağıtma](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (Windows PowerShell kullanarak)
