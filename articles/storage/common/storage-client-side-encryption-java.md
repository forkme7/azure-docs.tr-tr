---
title: Microsoft Azure depolama için istemci tarafı şifreleme Java ile | Microsoft Docs
description: Java için Azure Storage istemci kitaplığı, Azure Storage uygulamalarınız için en yüksek güvenlik için istemci tarafı şifreleme ve Azure anahtar kasası ile tümleştirmeyi destekler.
services: storage
documentationcenter: java
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: 3df49907-554c-404a-9b0c-b3e3269ad04f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 05/11/2017
ms.author: lakasa
ms.openlocfilehash: b4f3814ac2dbc8b74cef8f5fcb0540b7509efa0d
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="client-side-encryption-and-azure-key-vault-with-java-for-microsoft-azure-storage"></a>Microsoft Azure depolama için Java ile istemci tarafı şifreleme ve Azure anahtar kasası
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Genel Bakış
[Java için Azure Storage istemci Kitaplığı](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) Azure depolama alanına yüklemek ve istemciye indirirken verilerin şifresini çözmek önce istemci uygulamalar içinde verilerin şifrelenmesi destekler. Kitaplık ayrıca ile tümleştirmeyi destekler [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) depolama hesabı anahtarı yönetimi için.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Şifreleme ve şifre çözme Zarf teknik aracılığıyla
Şifreleme ve şifre çözme işlemleri Zarf teknik izleyin.  

### <a name="encryption-via-the-envelope-technique"></a>Zarf teknik aracılığıyla şifreleme
Zarf teknik aracılığıyla şifreleme aşağıdaki şekilde çalışır:  

1. Azure storage istemci kitaplığı simetrik anahtar bir kerelik kullan bir içerik şifreleme anahtarı (CEK) oluşturur.  
2. Kullanıcı verileri bu CEK kullanılarak şifrelenir.   
3. CEK (anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir) paketlenir. KEK anahtar bir tanımlayıcıyla tanımlanır ve asimetrik anahtar çifti ya da bir simetrik anahtar olması ve yerel olarak yönetilebilir veya Azure anahtar kasası depolanan.  
   Depolama istemcisi kitaplığı kendisini KEK hiçbir zaman erişebilir. Anahtar kasası tarafından sağlanan anahtar kaydırma algoritma kitaplığı çağırır. Kullanıcıların özel sağlayıcıları anahtar kaydırma/açmak için isterseniz kullanmayı da seçebilirsiniz.  
4. Şifrelenmiş veriler daha sonra Azure Storage hizmetine yüklenir. Bazı ek şifreleme meta verilerle birlikte Sarmalanan anahtarın meta veriler (hakkında bir blob) olarak depolanır veya şifrelenmiş verilerle (iletileri kuyruğa ve tablo varlıkları) değiştirilmiş.

### <a name="decryption-via-the-envelope-technique"></a>Zarf teknik aracılığıyla şifre çözme
Şifre çözme Zarf teknik aracılığıyla aşağıdaki şekilde çalışır:  

1. Kullanıcının yerel olarak veya Azure anahtar kasası anahtar şifreleme anahtarı (KEK) yönetiyor istemci kitaplığı varsayar. Kullanıcı, şifreleme için kullanılan özel anahtarını bilmesi gerekmez. Bunun yerine, farklı anahtar tanımlayıcıları anahtarları çözümlenen bir anahtar çözümleyici ayarlayabilir ve kullanılır.  
2. İstemci Kitaplığı hizmette depolanan tüm şifreleme malzeme birlikte şifrelenmiş verileri yükler.  
3. Sarmalanmamış (şifresi) anahtarı şifreleme kullanarak anahtarı (KEK) Sarmalanan içerik şifreleme anahtarı (CEK) olduğundan. Burada yeniden istemci kitaplığı KEK erişimi yok. Yalnızca özel veya anahtar kasası sağlayıcının açma algoritmasını çağırır.  
4. İçerik şifreleme anahtarı (CEK), ardından şifrelenmiş kullanıcı verileri şifrelemek için kullanılır.

## <a name="encryption-mechanism"></a>Şifreleme mekanizması
Depolama istemcisi kitaplığı kullanır [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) kullanıcı verilerini şifrelemek için. Özellikle, [Şifre blok zincirleme (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) AES moduyla. Her bir hizmet works biraz farklı şekilde Burada bunların her birini aşağıdakiler ele alınacaktır.

### <a name="blobs"></a>Bloblar
İstemci Kitaplığı şu anda yalnızca tüm blobları şifreleme desteklemektedir. Kullanıcıların kullandığınızda Şifreleme özellikle, desteklenen **karşıya*** yöntemleri veya **openOutputStream** yöntemi. Yüklemeleri, hem tam hem aralık yüklemeleri desteklenen için.  

Şifreleme sırasında istemci kitaplığının rastgele başlatma vektörü (IV), 32 bayt rasgele bir içerik şifreleme anahtarı (CEK) ile birlikte 16 bayt oluşturmak ve bu bilgileri kullanarak blob verisi Zarf şifreleme gerçekleştirin. Sarmalanan CEK ve bazı ek şifreleme meta veriler şifrelenmiş bir blobu hizmetinin yanı sıra meta verileri blob olarak depolanır.

> [!WARNING]
> Düzenleme veya kendi meta veriler blob'a karşıya yükleme, bu meta veriler korunduğundan emin olmak gerekir. Bu meta veriler olmadan yeni meta veriler karşıya yükleme, Sarmalanan CEK, IV ve diğer meta veriler kaybolur ve blob içeriğinin hiçbir zaman tekrar alınabilir.
> 
> 

Şifrelenmiş bir blobu indirme içerir kullanarak tüm blob içerik alma **indirme * / openInputStream** kullanışlı yöntemler. Sarmalanan CEK sarılmamış ve kullanıcılara şifresi çözülmüş veriler döndürmek için (Bu durumda blob meta verileri depolanır) IV ile birlikte kullanılır.

Rastgele bir aralığı indirme (**downloadRange*** yöntemleri) çok küçük miktarda başarıyla İstenen aralık şifresini çözmek için kullanılan ek veri alabilmek için kullanıcılar tarafından sağlanan aralığı ayarlama şifrelenmiş bir blobu içerir.  

Tüm türleri blob (blok blobları, sayfa blobları ve ilave bloblarını) şifrelenmiş/bu şeması kullanarak şifresi.

### <a name="queues"></a>Kuyruklar
İletileri kuyruğa herhangi biçimi olabileceği için istemci kitaplığının başlatma vektörü (IV) ve şifrelenmiş içerik şifreleme anahtarı (CEK) ileti metnini içeren özel bir biçim tanımlar.  

Şifreleme sırasında istemci kitaplığı 16 bayt rastgele CEK 32 bayt yanı sıra rasgele bir IV oluşturur ve bu bilgileri kullanarak sıraya ileti metninin Zarf şifreleme gerçekleştirir. Sarmalanan CEK ve bazı ek şifreleme meta veriler şifrelenmiş kuyruk iletisini eklenir. (Aşağıda gösterilen) bu değiştirilmiş ileti hizmette depolanır.

```
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

Şifre çözme sırasında Sarmalanan anahtar sırası iletiden ayıklanır ve sarılmamış. IV ayrıca kuyruk iletiden ayıklanan ve kuyruk iletisi verilerin şifresini çözmek için sarmalanmamış anahtarı ile birlikte kullanılan. Bir kuyruk iletisi 64 KB sınırını doğru sayım sırasında etkisi yönetilebilir olması şifreleme meta verileri (altında 500 bayt), küçük olduğunu unutmayın.

### <a name="tables"></a>Tablolar
İstemci Kitaplığı varlık özellikleri şifrelenmesi için INSERT destekler ve değiştirme işlemlerini.

> [!NOTE]
> Birleştirme şu anda desteklenmiyor. Bir alt özellikler kümesini daha önce farklı bir anahtar kullanılarak şifrelenmiş olabilecek olduğundan, sadece yeni özellikleri birleştirme ve meta verilerini güncelleştirme veri kaybına neden olur. Ya da birleştirme önceden var olan varlık hizmetinden okumak için fazladan hizmeti çağrıları yapma gerektirir veya yeni bir anahtar özellik başına kullanarak, her ikisi de performansı artırmak için uygun değildir.
> 
> 

Tablo verileri şifreleme gibi çalışır:  

1. Kullanıcıları şifrelenmesi için özellikleri belirtin.  
2. İstemci Kitaplığı rastgele başlatma vektörü (IV) yanı sıra rasgele içerik bir şifreleme anahtarı (CEK) 32 bayt her varlık için 16 bayt oluşturur ve yeni bir IV özellik başına türetme tarafından şifrelenmesi için ayrı ayrı özellikler Zarf şifreleme gerçekleştirir. Şifrelenmiş özelliği ikili veri olarak depolanır.  
3. Sarmalanan CEK ve bazı ek şifreleme meta verileri iki ek ayrılmış özellikleri olarak depolanır. İlk ayrılmış özelliği (_ClientEncryptionMetadata1) IV, sürüm ve Sarmalanan anahtar hakkında bilgi içeren bir dize özelliğidir. İkinci ayrılmış özelliği (_ClientEncryptionMetadata2) şifrelenmiş özellikler hakkında bilgi içeren bir ikili özelliğidir. Bu ikinci özelliğinde (_ClientEncryptionMetadata2) kendisi şifrelenmiş bilgilerdir.  
4. Şifreleme için gereken bu ek ayrılmış özellikleri nedeniyle, kullanıcılar artık 252 yerine yalnızca 250 özel özelliklere sahip olabilir. Varlık toplam boyutu 1 MB'tan az olması gerekir.  
   
   Yalnızca dize özellikleri şifrelenmiş olduğunu unutmayın. Diğer özelliklerin türleri, şifreli olarak varsa, bunlar dizelere dönüştürülmesi gerekir. Şifrelenmiş dizelerin hizmette ikili özellikleri olarak depolanır ve şifre çözme sonra dizelere geri dönüştürülür.
   
   Şifreleme İlkesi yanı sıra, tablolar için kullanıcıların şifrelenecek özelliklerini belirtmeniz gerekir. Bu, ya da bir [şifrele] özniteliği (için TableEntity türetilen POCO varlıklar) veya bir şifreleme çözümleyici isteği seçenekleri belirterek yapılabilir. Bir şifreleme çözümleyici bölüm anahtarı, satır anahtarını ve özellik adını alır ve bu özellik şifrelenmesi gerekip gerekmediğini belirten bir Boole değeri döndüren bir temsilci ' dir. Şifreleme sırasında istemci kitaplığı, bir özellik için kablo yazılırken şifrelenmesi gerekip gerekmediğine karar vermek için bu bilgileri kullanır. Temsilci özellikler nasıl şifrelenir geçici mantığı olasılığı için de sağlar. (Örneğin, X ise ardından özelliği A şifrelemek; Aksi takdirde özellikleri A ve b şifreleme) Bu bilgileri okurken veya varlıkları sorgulamak için gerekli olmadığını göz önünde bulundurun.

### <a name="batch-operations"></a>Toplu işlemleri
İstemci Kitaplığı yalnızca bir seçenekleri nesnesi (ve bu nedenle bir ilke/KEK) izin verdiği için toplu işlemde aynı KEK bu toplu işlem içindeki tüm satırların üzerinden toplu işlem kullanılır. Ancak, istemci kitaplığının dahili olarak yeni rastgele IV ve satır başına rastgele CEK toplu işlemde oluşturur. Kullanıcılar, şifreleme Çözümleyicisi Bu davranış tanımlayarak toplu işlemdeki her işlem için farklı özellikleri şifrelemek de seçebilirsiniz.

### <a name="queries"></a>Sorgular
> [!NOTE]
> Varlıkları şifrelendiği, filtre sorgularını bir şifrelenmiş özellikte çalıştırılamıyor.  Denerseniz, şifrelenmiş veriler şifrelenmemiş verilerle karşılaştırmak hizmet çalışırken çünkü sonuçlar hatalı olacaktır.
> 
>
Sorgu işlemleri gerçekleştirmek için sonuç kümesindeki tüm anahtarları çözümleyebildiğini anahtar bir çözümleyici belirtmeniz gerekir. Sorgu sonucunda bulunan bir varlık için bir sağlayıcı çözümlenemezse, istemci kitaplığının bir hata durum oluşturur. Sunucu tarafı tahminleri gerçekleştirir herhangi bir sorgu için istemci kitaplığının özel şifreleme meta veri özellikleri (_ClientEncryptionMetadata1 ve _ClientEncryptionMetadata2) varsayılan olarak seçilen sütunlara ekleyeceksiniz.

## <a name="azure-key-vault"></a>Azure Key Vault
Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Azure anahtar kasası kullanarak, kullanıcılar anahtarları ve gizli anahtarları (örneğin, kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları,. şifreleyebilirsiniz PFX dosyaları ve parolalar), donanım güvenlik modülleri (HSM'ler) tarafından korunan anahtarları kullanarak. Daha fazla bilgi için bkz: [Azure anahtar kasası nedir?](../../key-vault/key-vault-whatis.md).

Depolama istemcisi kitaplığı anahtar kasası çekirdek kitaplığı anahtarlarını yönetmek için Azure arasında ortak bir çerçeve sağlamak için kullanır. Kullanıcılar ayrıca anahtar kasası uzantıları kitaplığı kullanılarak ek bir avantaja alır. Uzantıları kitaplığı basit ve sorunsuz simetrik/RSA yerel ve bulut anahtar sağlayıcıları ve aynı zamanda toplama ve önbelleğe alma ile yararlı işlevsellik sağlar.

### <a name="interface-and-dependencies"></a>Arabirim ve bağımlılıkları
Üç anahtar kasası paketi vardır:  

* Azure keyvault çekirdek IKey ve IKeyResolver içerir. Hiçbir bağımlılıkları olan küçük bir pakettir. Java için depolama istemci kitaplığı bağımlılık olarak tanımlar.
* keyvault Azure anahtar kasası REST istemcisi içerir.  
* Azure keyvault uzantıları şifreleme algoritmalarının ve bir RSAKey ve bir SymmetricKey uygulamaları içeren uzantısı kodunu içerir. Çekirdek ve KeyVault ad alanında bulunan bağlıdır ve bir toplama Çözümleyicisi (kullanıcılar birden çok anahtar sağlayıcısı kullanmak istediğinizde) ve önbelleğe alma anahtar çözümleyici tanımlamak için işlevsellik sağlar. Kullanıcılar kendi anahtarları depolamak için veya yerel kullanabilir ve şifreleme sağlayıcıları bulut anahtar kasası uzantıları kullanmak için Azure anahtar kasası kullanmak istiyorsanız, depolama istemci kitaplığı doğrudan bu pakete, olmasa da, bunlar bu paketi gerekir.  
  
  Anahtar kasası yüksek değerli ana anahtarları için tasarlanmıştır ve azaltma sınırları anahtar kasası başına bunu göz önünde tasarlanmıştır. İstemci tarafı şifreleme anahtar kasası ile gerçekleştirirken, tercih edilen modeli simetrik ana anahtar kasasına gizli olarak depolanır ve önbelleğe alınan anahtarları yerel olarak kullanmaktır. Kullanıcılar aşağıdakileri yapmanız gerekir:  

1. Çevrimdışı bir gizli anahtar oluşturma ve anahtar Kasası'na yükleyin.  
2. Parolanın temel tanımlayıcısı geçerli sürümü, şifreleme için gizli anahtar çözmek ve bu bilgileri yerel olarak önbelleğe parametre olarak kullanın. CachingKeyResolver önbelleğe almak için kullanın. kullanıcılara uygulamak için kendi mantığı önbelleğe alma beklenmez.  
3. Önbellek Çözümleyicisi şifreleme ilkesi oluşturulurken bir giriş olarak kullanın.
   Şifreleme kod örnekleri anahtar kasası kullanımıyla ilgili daha fazla bilgi bulunabilir. <fix URL>  

## <a name="best-practices"></a>En iyi uygulamalar
Şifreleme desteği yalnızca Java için depolama istemci Kitaplığı'nda kullanılabilir.

> [!IMPORTANT]
> İstemci tarafı şifreleme kullanırken bu önemli noktalara dikkat edin:
> 
> * Okuma veya yazma şifrelenmiş bir blobu, tüm blob karşıya yükleme komutlarını ve aralığı/bütün blob yükleme komutlarını kullanın. Yerleştirme blok, Put engelleme listesi, yazma sayfaları, Temizle sayfaları veya blok ekleme gibi işlemleri protokolü kullanılarak şifrelenmiş bir blobu yazma kaçının; Aksi takdirde şifrelenmiş bir blobu bozuk ve okunamaz olun.
> * Tablolar için benzer bir kısıtlama var. Şifrelenmiş özellikler şifreleme meta verisini güncelleştirmeden yükseltmemeye dikkatli olun.  
> * Meta veriler üzerinde şifrelenmiş bir blobu ayarlarsanız, meta verileri ayarlama eklenebilir olmadığından şifre çözme için gerekli şifrelemeyle ilgili meta veriler üzerine yazabilir. Bu da anlık görüntüler için geçerlidir; meta veri anlık görüntüsünü şifrelenmiş bir blobu oluşturulurken belirtmekten kaçının. Meta veriler ayarlanmalıdır, çağırdığınızdan emin olun **downloadAttributes** önce geçerli şifreleme meta verilerini almak ve meta veri ayarlanırken eşzamanlı yazma önlemek için yöntem.  
> * Etkinleştirme **requireEncryption** yalnızca şifrelenmiş verileri ile çalışması gereken kullanıcılar için varsayılan istek seçenekler bayrağı. Aşağıda daha fazla bilgi için bkz.  
> 
> 

## <a name="client-api--interface"></a>İstemci API / arabirim
Bir EncryptionPolicy nesnesi oluşturulurken, kullanıcılara (IKey uygulama) yalnızca bir anahtar sağlayabilir Çözümleyicisi (IKeyResolver uygulama) ya da her ikisini de. IKey anahtar tanımlayıcısını kullanarak tanımlanır ve kaydırma/açmak için mantığı sağlayan temel anahtar türü değil. IKeyResolver bir anahtar şifre çözme işlemi sırasında çözmek için kullanılır. Anahtar tanımlayıcısını verilen IKey döndüren bir ResolveKey yöntemi tanımlar. Bu kullanıcılar birden fazla konumda yönetilen birden çok anahtar arasında seçim yapma olanağı sağlar.

* Şifreleme için bir anahtarın olmaması bir hataya neden olur ve anahtarı her zaman kullanılır.  
* Şifre çözme için:  
  
  * Anahtar çözümleyici anahtarını almak için belirtilmişse çağrılır. Çözümleyici belirtildi, ancak anahtar tanımlayıcısı için bir eşleme yok, bir hata oluşturulur.  
  * Gerekli anahtar tanımlayıcısı tanıtıcısını eşleşmesi durumunda çözümleyici belirtilmedi, ancak belirtilen bir anahtar, anahtar kullanılır. Tanımlayıcı eşleşmiyorsa, bir hata oluşturulur.  
    
    [Şifreleme örnekleri](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) <fix URL>daha ayrıntılı bir uçtan uca senaryoyu BLOB, kuyruklar ve tablolar, bunların ile anahtar kasası tümleştirme göstermektedir.

### <a name="requireencryption-mode"></a>RequireEncryption modu
Kullanıcıların isteğe bağlı olarak bir çalışma modu, burada tüm ve indirmelere şifrelenmelidir sağlayabilirsiniz. Bu modda, istemcide bir şifreleme ilkesi olmadan verileri karşıya yükleme veya hizmette şifrelenmez veri indirme girişimleri başarısız olur. **RequireEncryption** bayrağı isteği seçenekleri nesnesinin bu davranışı denetler. Uygulamanızı Azure depolama alanında depolanan tüm nesnelere şifreler sonra ayarlayabileceğiniz **requireEncryption** özelliği varsayılan istek seçeneklerine hizmeti istemci nesnesi.   

Örneğin, **CloudBlobClient.getDefaultRequestOptions().setRequireEncryption(true)** tüm bu istemci nesnesi üzerinden gerçekleştirilen işlemler blob için şifreleme istemek için.

### <a name="blob-service-encryption"></a>BLOB hizmeti şifreleme
Oluşturma bir **BlobEncryptionPolicy** nesne ve istek seçeneklerinde ayarlayın (API başına veya kullanarak bir istemci düzeyinde **DefaultRequestOptions**). Şey istemci kitaplığı tarafından dahili olarak ele alınacaktır.

```java
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

// Set the encryption policy on the request options.
BlobRequestOptions options = new BlobRequestOptions();
options.setEncryptionPolicy(policy);

// Upload the encrypted contents to the blob.
blob.upload(stream, size, null, options, null);

// Download and decrypt the encrypted contents from the blob.
ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
blob.download(outputStream, null, options, null);
```

### <a name="queue-service-encryption"></a>Kuyruk hizmeti şifreleme
Oluşturma bir **QueueEncryptionPolicy** nesne ve istek seçeneklerinde ayarlayın (API başına veya kullanarak bir istemci düzeyinde **DefaultRequestOptions**). Şey istemci kitaplığı tarafından dahili olarak ele alınacaktır.

```java
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

// Add message
QueueRequestOptions options = new QueueRequestOptions();
options.setEncryptionPolicy(policy);

queue.addMessage(message, 0, 0, options, null);

// Retrieve message
CloudQueueMessage retrMessage = queue.retrieveMessage(30, options, null);
```

### <a name="table-service-encryption"></a>Tablo hizmeti şifreleme
Bir şifreleme ilkesi oluşturma ve istek seçeneklerini ayarlama ek olarak, ya da belirtmeniz gerekir bir **EncryptionResolver** içinde **TableRequestOptions**, veya varlığın alıcı ve ayarlayıcıya [şifrele] özniteliğini ayarlayın.

### <a name="using-the-resolver"></a>Çözücü kullanma

```java
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

TableRequestOptions options = new TableRequestOptions()
options.setEncryptionPolicy(policy);
options.setEncryptionResolver(new EncryptionResolver() {
    public boolean encryptionResolver(String pk, String rk, String key) {
        if (key == "foo")
        {
            return true;
        }
        return false;
    }
});

// Insert Entity
currentTable.execute(TableOperation.insert(ent), options, null);

// Retrieve Entity
// No need to specify an encryption resolver for retrieve
TableRequestOptions retrieveOptions = new TableRequestOptions()
retrieveOptions.setEncryptionPolicy(policy);

TableOperation operation = TableOperation.retrieve(ent.PartitionKey, ent.RowKey, DynamicTableEntity.class);
TableResult result = currentTable.execute(operation, retrieveOptions, null);
```

### <a name="using-attributes"></a>Öznitelikleri kullanma
Varlık TableEntity uyguluyorsa, yukarıda belirtildiği gibi sonra Özellikler alıcı ve ayarlayıcıya belirtme yerine [şifrele] özniteliğiyle tasarlanabilir **EncryptionResolver**.

```java
private string encryptedProperty1;

@Encrypt
public String getEncryptedProperty1 () {
    return this.encryptedProperty1;
}

@Encrypt
public void setEncryptedProperty1(final String encryptedProperty1) {
    this.encryptedProperty1 = encryptedProperty1;
}
```

## <a name="encryption-and-performance"></a>Şifreleme ve performans
Depolama veri sonuçlarınızda ek performans yükü şifreleme unutmayın. IV ve içerik anahtarı oluşturulmuş olması gerekir, içeriği şifrelenmelidir ve ek meta veri biçimlendirilmiş ve karşıya gerekir. Bu ek yükü Şifrelenmekte veri miktarı bağlı olarak değişir. Müşterilerin kendi uygulamalarında geliştirme sırasında performansı için her zaman sınamanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
* Karşıdan [Java Maven paket için Azure Storage istemci kitaplığı](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)  
* Karşıdan [Java kaynak kodu github'dan için Azure Storage istemci kitaplığı](https://github.com/Azure/azure-storage-java)   
* Java Maven paketler için Azure anahtar kasası Maven Kitaplığı yükleyin:
  * [Çekirdek](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault-core) paketi
  * [İstemci](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault) paketi
* Ziyaret [Azure anahtar kasası belgeleri](../../key-vault/key-vault-whatis.md)
