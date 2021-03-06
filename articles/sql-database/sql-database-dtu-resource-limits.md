---
title: Azure SQL veritabanı DTU tabanlı kaynak sınırları | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanı için bazı ortak DTU tabanlı kaynak sınırları açıklar.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/04/2018
ms.author: carlrab
ms.openlocfilehash: fb5c2e16e696ba9eecf4346a0c4e7bc05aacf39f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-sql-database-dtu-based-resource-model-limits"></a>Azure SQL veritabanı DTU tabanlı kaynak modeli sınırları

> [!IMPORTANT]
> VCore tabanlı kaynak sınırları için bkz: [SQL veritabanı vCore tabanlı kaynak sınırları](sql-database-vcore-resource-limits.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Tek veritabanı: depolama boyutlarına ve performans düzeyleri

Tek veritabanları için aşağıdaki tablolarda her hizmeti katmanını ve performans düzeyinde tek bir veritabanı için kullanılabilir kaynakları gösterir. Hizmet katmanı, performans düzeyi ve kullanarak tek veritabanı için bir depolama miktarını ayarlayabilirsiniz [Azure portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](sql-database-single-database-resources.md#manage-single-database-resources-using-transact-sql), [PowerShell](sql-database-single-database-resources.md#manage-single-database-resources-using-powershell), [Azure CLI](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-cli), veya [REST API](sql-database-single-database-resources.md#manage-single-database-resources-using-the-rest-api).

### <a name="basic-service-tier"></a>Temel hizmet katmanı
| **Performans düzeyi** | **Temel** |
| :--- | --: |
| Maks. DTU | 5 |
| Eklenen depolama (GB) | 2 |
| En fazla depolama seçenekleri (GB) | 2 |
| Maks. bellek içi OLTP depolama alanı (GB) |Yok |
| En fazla eşzamanlı çalışan (istek sayısı) | 30 |
| Maks. eş zamanlı oturum | 300 |
|||

### <a name="standard-service-tier"></a>Standart hizmet katmanı
| **Performans düzeyi** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|---:|
| Maks. DTU | 10 | 20 | 50 | 100 |
| Eklenen depolama (GB) | 250 | 250 | 250 | 250 |
| En fazla depolama seçenekleri (GB) * | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok |
| En fazla eşzamanlı çalışan (istek sayısı)| 60 | 90 | 120 | 200 |
| Maks. eş zamanlı oturum |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standart hizmet katmanında (devam)
| **Performans düzeyi** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|---:|
| Maks. DTU | 200 | 400 | 800 | 1600 | 3000 |
| Eklenen depolama (GB) | 250 | 250 | 250 | 250 | 250 |
| En fazla depolama seçenekleri (GB) * | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok |Yok |
| En fazla eşzamanlı çalışan (istek sayısı)| 400 | 800 | 1600 | 3200 |6000 |
| Maks. eş zamanlı oturum |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>Premium hizmet katmanı 
| **Performans düzeyi** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| :--- |---:|---:|---:|---:|---:|---:|
| Maks. DTU | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Eklenen depolama (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| En fazla depolama seçenekleri (GB) * | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| Maks. bellek içi OLTP depolama alanı (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| En fazla eşzamanlı çalışan (istek sayısı)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||


> [!IMPORTANT]
> - Önizlemede bulunan depolama alanı miktarını büyük depolama boyutları ve ek maliyetlerden uygulayın. Ayrıntılar için bkz. [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). 
>
> - Premium katmanındaki birden fazla 1 TB depolama alanı aşağıdaki bölgelerde şu anda kullanılabilir değil: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Orta Kanada, Doğu Kanada, Orta ABD, Fransa Merkezi, Almanya Merkezi, Japonya Doğu, Japonya Batı, Kore Merkezi Kuzey Orta ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Birleşik Krallık Güney, Birleşik Krallık Batı, ABD East2, Batı ABD, ABD kamu Virginia ve Batı Avrupa. Bkz. [P11 P15 Geçerli Sınırlamalar](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 


## <a name="single-database-change-storage-size"></a>Tek veritabanı: depolama boyutunu değiştirme

- Tek bir veritabanı için DTU fiyat belirli bir miktarda depolama hiçbir ek ücret ödemeden içerir. Dahil edilen miktar ötesinde ek depolama alanı için en fazla 1 TB 250 GB artışlarla ve 1 TB ötesinde 256 GB artışlarla en büyük boyut sınırına kadar ek bir maliyet sağlanabilir. Eklenen depolama tutarlar ve en büyük boyut sınırları için bkz [tek veritabanı: depolama boyutlarına ve performans düzeyleri](#single-database-storage-sizes-and-performance-levels).
- En büyük boyut kullanılarak artırarak tek bir veritabanı için ek depolama alanı sağlanabilir [Azure portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update).
- Ek depolama alanı için tek bir veritabanının Hizmet katmanını ek depolama alanı birim fiyatı ile çarpılan ek depolama alanı miktarını fiyatıdır. Ek depolama alanı fiyatı hakkında daha fazla bilgi için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-dtus"></a>Tek veritabanı: değişiklik Dtu'lar

Başlangıçta bir hizmet katmanı, performans düzeyi ve depolama miktarına seçtikten sonra tek bir veritabanı yukarı veya aşağı gerçek deneyimi kullanarak dinamik olarak bağlı ölçeklendirmek [Azure portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update). 

Aşağıdaki videoda, tek bir veritabanı için kullanılabilir Dtu'lar artırmak için performans katmanı dinamik olarak değiştirme gösterir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Bir veritabanının hizmet katmanının ve/veya performansının değiştirilmesi, özgün veritabanının yeni performans düzeyinde bir çoğaltmasını oluşturur ve ardından bağlantıları bu çoğaltmaya geçirir. Bu işlem sırasında veri kaybı olmaz, ancak çoğaltmaya geçişin gerçekleştiği kısa süre zarfında veritabanıyla bağlantılar devre dışı bırakılır, bu nedenle uçuştaki bazı işlemler geri alınabilir. Anahtar üzerinden süreyi değişir, ancak 30 saniyeden zamanın % 99 olur. Varsa büyük sayılar şu anda bağlantıları, yürütülen işlemler devre dışı bırakıldı, anahtar üzerinden süreyi daha uzun olabilir. 

Tüm ölçek artırma işleminin süresi hem veritabanı boyutuna hem de değişiklikten önceki ve sonraki hizmet katmanına bağlı olarak değişir. Örneğin, değiştirilmesi için gelen veya bir standart hizmet katmanı içinde 250 GB veritabanı altı saat içinde tamamlamanız gerekir. Premium Hizmet katmanını içinde performans düzeylerini değiştirme boyutu aynı veritabanı için bir ölçek büyütme üç saat içinde tamamlamanız gerekir.

> [!TIP]
> Edenler işlemleri izlemek için bkz: [SQL REST API kullanarak işlemlerini yönetmek](/rest/api/sql/Operations/List), [CLI kullanarak işlemlerini yönetmek](/cli/azure/sql/db/op), [T-SQL kullanarak işlemlerini izleyin](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) ve bu iki PowerShell komutları: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) ve [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Daha yüksek bir hizmet katmanı veya performans düzeyini yükseltme yapıyorsanız, daha büyük bir boyutu (maxsize) açıkça belirtmediğiniz sürece veritabanı en büyük boyutunu artırmaz.
* Bir veritabanı düşürmek için kullanılan veritabanı alanı hedef hizmeti katmanını ve performans düzeyini boyutu izin verilen üst sınırdan küçük olması gerekir. 
* Öğesinden önceki sürüme indirme zaman **Premium** için **standart** katmanı, bir ek depolama alanı maliyet geçerlidir hem max (1 veritabanının boyutu hedef performans düzeyinde desteklenir ve (2 en fazla boyutu aşıyor Hedef performans düzeyi dahil depolama miktarı. En büyük boyut 500 GB P1 veritabanıyla S3 için downsized, örneğin, daha sonra ek depolama alanı maliyeti S3 500 GB en büyük boyutunu destekler ve yalnızca 250 GB birlikte depolama miktarını olduğundan geçerlidir. Bu nedenle, ek depolama alanı miktarını 500 GB – 250 GB = 250 GB'tır. Ek depolama fiyatlandırma için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). Kullanılan gerçek miktarını dahil depolama miktarına küçükse, ardından bu ekstra maliyet veritabanı boyutu üst sınırını dahil edilen miktarını azaltarak önlenebilir. 
* Bir veritabanıyla yükseltirken [coğrafi çoğaltma](sql-database-geo-replication-portal.md) etkinse, onun ikincil veritabanları için istediğiniz performans katmanı birincil veritabanı (en iyi performans için genel yönergeler) yükseltmeden önce yükseltin. Farklı bir yükseltme yaparken, ikincil veritabanını yükseltmeden önce gereklidir.
* Bir veritabanını önceki sürüme indirme zaman [coğrafi çoğaltma](sql-database-geo-replication-portal.md) istediğiniz performans katmanına birincil veritabanlarını ikincil veritabanı (en iyi performans için genel yönergeler) eski sürüme düşürmeyi önce düşürmek etkin. Farklı bir sürüme eski sürüme düşürmeyi, birincil veritabanı eski sürüme düşürmeyi ilk gereklidir.
* Geri yükleme hizmeti teklifleri, çeşitli hizmet katmanları için farklılık gösterir. İçin olana varsa **temel** katmanı, daha düşük bir yedekleme Bekletme dönemi - bkz [Azure SQL veritabanı yedeklemeleri](sql-database-automated-backups.md).
* Veritabanının yeni özellikleri, değişiklikler tamamlanana kadar uygulanmaz.

## <a name="single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Tek veritabanı: P11 ve en büyük boyut, 1 TB'den büyükse P15 sınırlamaları

P11 ve P15 veritabanı aşağıdaki bölgelerde desteklenen 1 TB'den büyük bir maksimum boyut: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Orta Kanada, Doğu Kanada, Orta ABD, Fransa Merkezi, Almanya Merkezi, Japonya Doğu, Japonya Batı, Kore Merkezi Kuzey Orta ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Birleşik Krallık Güney, Birleşik Krallık Batı, ABD East2, Batı ABD, ABD kamu Virginia ve Batı Avrupa. En büyük boyutu 1 TB'den büyük olan P11 ve P15 veritabanları için aşağıdaki konuları ve sınırlamalar uygulanır:

- En büyük boyutu 1 TB'den büyük (4 TB veya 4096 GB değeri kullanarak) bir veritabanı oluşturulurken seçerseniz, veritabanı desteklenmeyen bir bölgede sağlandığında Oluştur komutu bir hata ile başarısız olur.
- Desteklenen bölgeleri birinde bulunan mevcut P11 ve P15 veritabanları için en fazla depolama için 1 TB ötesinde 256 GB artışlarla artırabilirsiniz en fazla 4 TB. Daha büyük bir boyutu bölgenizde destekleyip desteklemediğini görmek için [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) işlev veya Azure portalında veritabanı boyutu inceleyin. Varolan P11 veya P15 yükseltme veritabanı yalnızca sunucu düzeyinde asıl oturum açma veya dbmanager veritabanı rolünün üyeleri tarafından gerçekleştirilebilir. 
- Bir yükseltme işlemi desteklenen bir bölgede yürütülürse yapılandırma anında güncelleştirilir. Veritabanı yükseltme işlemi sırasında çevrimiçi kalır. Ancak, gerçek veritabanı dosyalarını yeni maksimum boyuta yükseltilene kadar tam tutar 1 TB depolama alanı dışında depolama alanını olamaz. Gereken süreyi yükseltilen veritabanı boyutuna bağlıdır. 
- Oluşturma veya güncelleştirme P11 veya P15 bir veritabanı, yalnızca 1 TB ve 4 TB en büyük boyutu 256 GB artışlarla arasında seçim yapabilirsiniz. P11/P15 oluştururken, varsayılan depolama 1 TB'lık önceden seçilmiş seçeneğidir. Desteklenen bölgeleri birinde bulunan veritabanları için en çok 4 TB yeni veya varolan bir tek veritabanı için depolama en artırabilir. Diğer tüm bölgeler için en büyük boyutu 1 TB yükseltilemez. 4 TB dahil depolama seçtiğinizde fiyat değiştirmez.
- Bir veritabanı en büyük boyutu 1 TB'den büyük ayarlanmışsa, 1 TB kullanılan depolamanın olsa bile, ardından 1 TB'ye kadar değiştirilemez. Bu nedenle, size bir P11 veya P15 TB P11 veya 1 TB P15 ya da daha düşük performans katmanı, P1 P6 gibi 1 1 TB'den büyük bir maksimum boyutu ile düşürülemiyor). Bu kısıtlama noktası zamanında dahil olmak üzere kopyalama senaryoları ve geri yükleme için de geçerlidir coğrafi geri yükleme, uzun-süreli yedekleme-saklama ve veritabanı kopyası. En büyük boyutu 1 TB'den büyük olan bir veritabanı yapılandırıldıktan sonra bu veritabanının tüm geri yükleme işlemleri en büyük boyutu 1 TB'den büyük olan P11/P15 çalıştırmanız gerekir.
- Aktif coğrafi çoğaltma senaryoları için:
   - Bu ilişki bir coğrafi çoğaltma ilişkisi ayarlanırken: birincil veritabanı P11 veya P15 ise, secondary(ies) de P11 veya P15; olması gerekir birden fazla 1 TB destekleme kapasitesine sahip olmadığından, düşük performans katmanı ikincil kopya reddedilir.
   - Coğrafi çoğaltma ilişkisinde birincil veritabanı yükseltme: en büyük boyutu 1 TB'den birincil veritabanında değiştirme ikincil veritabanını aynı değişiminde tetikler. Her iki yükseltmeyi değişikliğin etkili olması için birincil başarılı olması gerekir. Birden fazla 1 TB seçeneği için bölge sınırlamalar uygulanır. İkincil birden fazla 1 TB desteklemeyen bir bölgede ise, birincil yükseltilmez.
- Birden fazla 1 TB olan P11/P15 veritabanları yüklenmesi için içeri/dışarı aktarma hizmeti kullanma desteklenmiyor. SqlPackage.exe için kullanmak [alma](sql-database-import.md) ve [verme](sql-database-export.md) veri.

## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Esnek havuz: depolama boyutlarına ve performans düzeyleri

SQL Database esnek havuzlar için aşağıdaki tablolarda her hizmeti katmanını ve performans düzeyinde kaynakları gösterir. Hizmet katmanı, performans düzeyi ve depolama miktarını kullanarak ayarlayabilirsiniz [Azure portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-powershell), [Azure CLI](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-cli), veya [REST API](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-rest-api).

> [!NOTE]
> Esnek havuzlar bulunan tek veritabanlarını kaynak sınırları genellikle Dtu'lar ve Hizmet katmanını temel alan havuzları dışında tek veritabanları ile aynıdır. Örneğin, S2 veritabanı için en fazla eşzamanlı çalışan 120 çalışanları olur. Bu nedenle, standart havuzdaki bir veritabanı için en fazla eşzamanlı çalışan olduğunu da 120 çalışanları havuzunda veritabanı başına maksimum DTU (hangi S2 eşdeğerdir) 50 Dtu'lar ise.

### <a name="basic-elastic-pool-limits"></a>Temel esnek havuz sınırları

| Havuz başına eDTU | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Eklenen depolama havuzu (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Havuz (GB) başına en fazla depolama seçenekleri | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| En fazla bellek içi OLTP depolama havuzu (GB) | Yok | Yok | Yok | Yok | Yok | Yok | Yok | Yok |
| Havuz başına en fazla veritabanı | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Havuz başına maks. eş zamanlı çalışan (istek) | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Veritabanı başına minimum Edtu seçenekleri | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Veritabanı başına maksimum Edtu seçenekleri | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Veritabanı başına maks. depolama alanı (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 
||||||||

### <a name="standard-elastic-pool-limits"></a>Standart esnek havuz sınırları

| Havuz başına eDTU | **50** | **100** | **200** | **300** | **400** | **800**| 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Eklenen depolama havuzu (GB) | 50 | 100 | 200 | 300 | 400 | 800 | 
| Havuz (GB) başına en fazla depolama seçenekleri * | 50, 250, 500 | 100, 250, 500, 750 | 200, 250, 500, 750, 1024 | 300, 500, 750, 1024, 1280 | 400, 500, 750, 1024, 1280, 1536 | 800, 1024, 1280, 1536, 1792, 2048 | 
| En fazla bellek içi OLTP depolama havuzu (GB) | Yok | Yok | Yok | Yok | Yok | Yok | 
| Havuz başına en fazla veritabanı | 100 | 200 | 500 | 500 | 500 | 500 | 
| Havuz başına maks. eş zamanlı çalışan (istek) | 100 | 200 | 400 | 600 | 800 | 1600 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Veritabanı başına minimum Edtu seçenekleri | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Veritabanı başına maksimum Edtu seçenekleri | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 | 
| Veritabanı başına maks. depolama alanı (GB)* | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Standart esnek havuz limitleri (devam) 

| Havuz başına eDTU | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Eklenen depolama havuzu (GB) | 1200 | 1600 | 2000 | 2500 | 3000 | 
| Havuz (GB) başına en fazla depolama seçenekleri * | 1200, 1280, 1536, 1792, 2048, 2304, 2560 | 1600, 1792, 2048, 2304, 2560, 2816, 3072 | 2000, 2048, 2304, 2560, 2816, 3072, 3328, 3584 | 2500, 2560, 2816, 3072, 3328, 3584, 3840, 4096 | 3000, 3072, 3328, 3584, 3840, 4096 |
| En fazla bellek içi OLTP depolama havuzu (GB) | Yok | Yok | Yok | Yok | Yok | 
| Havuz başına en fazla veritabanı | 500 | 500 | 500 | 500 | 500 | 
| Havuz başına maks. eş zamanlı çalışan (istek) | 2400 | 3200 | 4000 | 5000 | 6000 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Veritabanı başına minimum Edtu seçenekleri | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Veritabanı başına maksimum Edtu seçenekleri | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 | 
| Veritabanı (GB) başına en fazla depolama seçenekleri * | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Premium esnek havuz sınırları

| Havuz başına eDTU | **125** | **250** | **500** | **1000** | **1500**| 
|:---|---:|---:|---:| ---: | ---: | 
| Eklenen depolama havuzu (GB) | 250 | 500 | 750 | 1024 | 1536 | 
| Havuz (GB) başına en fazla depolama seçenekleri * | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 1536 |
| En fazla bellek içi OLTP depolama havuzu (GB) | 1 | 2 | 4 | 10 | 12 | 
| Havuz başına en fazla veritabanı | 50 | 100 | 100 | 100 | 100 | 
| Havuz başına maks. eş zamanlı çalışan (istek) | 200 | 400 | 800 | 1600 | 2400 | 
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Veritabanı başına Min. eDTU | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000, 1500 | 
| Veritabanı başına Maks. eDTU | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000, 1500 |
| Veritabanı başına maks. depolama alanı (GB)* | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Premium esnek havuz limitleri (devam) 

| Havuz başına eDTU | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: | 
| Eklenen depolama havuzu (GB) | 2048 | 2560 | 3072 | 3548 | 4096 |
| Havuz (GB) başına en fazla depolama seçenekleri * | 2048 | 2560 | 3072 | 3548 | 4096|
| En fazla bellek içi OLTP depolama havuzu (GB) | 16 | 20 | 24 | 28 | 32 |
| Havuz başına en fazla veritabanı | 100 | 100 | 100 | 100 | 100 | 
| Havuz başına maks. eş zamanlı çalışan (istek) | 3200 | 4000 | 4800 | 5600 | 6400 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Veritabanı başına minimum Edtu seçenekleri | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Veritabanı başına maksimum Edtu seçenekleri | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Veritabanı başına maks. depolama alanı (GB)* | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

> [!IMPORTANT]
> -  Önizlemede bulunan depolama alanı miktarını büyük depolama boyutları ve ek maliyetlerden uygulayın. Ayrıntılar için bkz [SQL veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/). Önizlemede bulunan depolama alanı miktarını büyük depolama boyutları ve ek maliyetlerden uygulayın. Ayrıntılar için bkz [SQL veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/).
>
> -  Premium katmanındaki birden fazla 1 TB depolama alanı aşağıdaki bölgelerde şu anda kullanılabilir değil: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Orta Kanada, Doğu Kanada, Orta ABD, Fransa Merkezi, Almanya Merkezi, Japonya Doğu, Japonya Batı, Kore Merkezi Kuzey Orta ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Birleşik Krallık Güney, Birleşik Krallık Batı, ABD East2, Batı ABD, ABD kamu Virginia ve Batı Avrupa. Bkz. [P11 P15 Geçerli Sınırlamalar](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
>

Bir elastik havuzun tüm DTU’ları kullanılırsa, sorguları işlemek üzere havuzdaki her bir veritabanı eşit miktarda kaynak alır. SQL Veritabanı hizmeti, eşit dilimlerde işlem süresi sunarak veritabanları arasında kaynak paylaşım eşitliğini sağlar. Elastik havuz kaynak paylaşımı eşitliği, veritabanı başına DTU dakikası sıfır olmayan bir değere ayarlandığında her bir veritabanı için garanti edilen herhangi bir kaynak miktarına ek niteliktedir.

### <a name="database-properties-for-pooled-databases"></a>Havuza alınmış veritabanları için veritabanı özellikleri

Aşağıdaki tabloda, havuza alınmış veritabanları için özellikleri açıklar.

| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına Maks. eDTU |Havuzdaki diğer veritabanlarının kullanımına göre mevcutsa, havuzdaki herhangi bir veritabanının kullanabileceği en fazla eDTU sayısı. Veritabanı başına en fazla eDTU, veritabanı için kaynak garantisi anlamına gelmez. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımının en üst seviyeye çıktığı durumlarla baş edebilmek için veritabanı başına en fazla eDTU sayısını yeterince yüksek bir değere ayarlayın. Havuz genellikle tüm veritabanlarının eşzamanlı olarak en üst kullanım seviyesine çıkmadığı sıcak ve soğuk kullanım modellerini varsaydığından, bir miktar aşırı ayırma beklenir. Örneğin, veritabanı başına en yüksek kullanımın 20 eDTU olduğunu ve havuzdaki 100 veritabanının yalnızca %20’sinin aynı anda en yüksek seviyeye çıktığını varsayalım. Veritabanı başına en fazla eDTU değeri 20 eDTU’ya ayarlanırsa, havuzun 5 kat fazla kullanılması ve havuz başına eDTU sayısının 400’e ayarlanması makuldür. |
| Veritabanı başına Min. eDTU |Havuzdaki herhangi bir veritabanının kullanabileceği en az eDTU sayısı garanti edilir. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına en az eDTU sayısı 0’a ayarlanabilir; bu değer aynı zamanda varsayılan değerdir. Bu özellik, 0 ile veritabanı başına ortalama eDTU kullanımı arasında bir değere ayarlanır. Havuzdaki veritabanı sayısının veritabanı başına en az eDTU sayısıyla çarpımı, havuz başına eDTU sayısını aşamaz. Örneğin, bir havuzda 20 veritabanı varsa ve veritabanı başına en az eDTU sayısı 10 eDTU’ya ayarlanırsa, havuzdaki eDTU sayısı en az 200 eDTU olmalıdır. |
| Veritabanı başına en fazla depolama |Bir veritabanı için kullanıcı tarafından bir havuzda ayarlanan en fazla veritabanı boyutu. Ancak, havuza alınmış veritabanları ayrılmış havuz depolama paylaşır. Olsa bile toplam en fazla depolama **veritabanı başına* toplam kullanılabilir depolama alanına büyük olacak şekilde ayarlanmıştır **havuzunun alanı*, aslında tüm veritabanları tarafından kullanılan toplam alanı aşan mümkün olmayacaktır kullanılabilir havuz sınırı. En büyük veritabanı boyutu, veri dosyalarının en büyük boyuta başvuruyor ve günlük dosyaları tarafından kullanılan alanı içermez. |
|||
 
## <a name="elastic-pool-change-storage-size"></a>Esnek havuz: depolama boyutunu değiştirme

- Bir esnek havuz eDTU fiyat belirli bir miktarda depolama hiçbir ek ücret ödemeden içerir. Dahil edilen miktar ötesinde ek depolama alanı için en fazla 1 TB 250 GB artışlarla ve 1 TB ötesinde 256 GB artışlarla en büyük boyut sınırına kadar ek bir maliyet sağlanabilir. Eklenen depolama tutarlar ve en büyük boyut sınırları için bkz [esnek havuz: depolama boyutlarına ve performans düzeyleri](#elastic-pool-storage-sizes-and-performance-levels).
- En büyük boyut kullanılarak artırarak esnek havuz için ek depolama alanı sağlanabilir [Azure portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), veya [REST API'si ](/rest/api/sql/elasticpools/update).
- Esnek havuz için ek depolama alanı hizmet katmanına ek depolama alanı birim fiyatı ile çarpılan ek depolama alanı miktarını fiyatıdır. Ek depolama alanı fiyatı hakkında daha fazla bilgi için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="elastic-pool-change-edtus"></a>Esnek havuz: değiştirme Edtu

Artırmak veya ihtiyaçlarını kullanarak kaynağını temel bir esnek havuz için kullanılabilir kaynakları azaltmak [Azure portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), veya [ REST API](/rest/api/sql/elasticpools/update).

- Havuz Edtu rescaling, veritabanı bağlantılarını kısaca bırakılır. As (olmayan bir havuzdaki) tek bir veritabanı için Dtu'lar rescaling oluşur aynı davranış budur. Süresini ve rescaling işlemleri sırasında bırakılan bağlantıları için bir veritabanı etkisi hakkında daha fazla bilgi için bkz: [tek bir veritabanı için Dtu'lar Rescaling](#single-database-change-storage-size). 
- Havuz Edtu yeniden Ölçeklendir süresi havuzdaki tüm veritabanları tarafından kullanılan depolama alanının toplam miktarını bağlı olabilir. Genel olarak, rescaling gecikme süresi 90 dakika ortalama başına 100 GB veya daha az. Örneğin, toplam alanı tarafından kullanılan havuzdaki tüm veritabanları ise 200 GB havuzu rescaling için beklenen gecikme süresi 3 saat sonra veya daha az. Bazı durumlarda standart veya temel katmanı içinde altında kullanılan alanı miktarının bağımsız olarak beş dakika rescaling gecikme olabilir.
- Genel olarak, veritabanı veya maksimum Edtu başına veritabanı başına minimum edtu'larını değiştirmek için süre beş dakikadır veya daha az.
- Havuz Edtu downsizing, kullanılan havuzu alanı hedef hizmet katman ve havuz edtu'larını boyutu izin verilen üst sınırdan küçük olması gerekir.
- Havuz Edtu rescaling, ek depolama alanı maliyeti (1 depolama en büyük havuz boyutu hedef havuzu tarafından desteklenir ve (2 depolama en büyük boyut hedef havuzu dahil depolama miktarını aşarsa uygulanır. 100 eDTU standart havuzu en büyük boyutu 100 GB ile 50 standart havuz eDTU downsized, örneğin, daha sonra ek depolama alanı maliyeti en büyük boyutu 100 GB hedef havuzu destekler ve dahil edilen depolama miktarını yalnızca 50 GB olduğundan geçerlidir. Bu nedenle, ek depolama alanı miktarı 100 GB – 50 GB = 50 GB'tır. Ek depolama fiyatlandırma için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). Kullanılan gerçek miktarını dahil depolama miktarına küçükse, ardından bu ekstra maliyet veritabanı boyutu üst sınırını dahil edilen miktarını azaltarak önlenebilir. 

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>Sunucular ve veritabanları maksimum sayısı nedir?

| Maksimum | Değer |
| :--- | :--- |
| Sunucu başına veritabanları | 5000 |
| Her bölge abonelik başına sunucularının sayısı | 20 |
|||

> [!IMPORTANT]
> Sunucu başına izin verilen maksimum veritabanı sayısı yaklaşımı olarak aşağıdaki oluşabilir:
> <br> Sorguları ana veritabanına karşı çalışır • artırma gecikme süresi.  Bu, kaynak kullanımı istatistikleri sys.resource_stats gibi görünümlerini içerir.
> <br> • Yönetim işlemlerini gecikme artırma ve portal görüşlerini işleme, sunucunun veritabanlarında numaralandırma içerir.

## <a name="what-happens-when-database-and-elastic-pool-resource-limits-are-reached"></a>Veritabanı ve esnek havuz kaynak sınırlarını ulaşıldığında ne olur?

### <a name="compute-dtus-and-edtus"></a>İşlem (Dtu ve Edtu)

(Dtu ve Edtu ölçülür) veritabanı işlem kullanımı yüksek olduğunda, sorgu gecikmesi artırır ve hatta zaman aşımı olabilir. Bu koşullar altında sorguları hizmeti tarafından sıraya alınmış ve kaynak olarak yürütme için kaynakları serbest hale sağlanır.
Yüksek işlem kullanımı karşılaşıldığında, azaltma seçenekleri şunlardır:

- Veritabanı veya daha fazla Dtu'lar veya Edtu'lar veritabanı sağlamak için esnek havuz performans düzeyini artırma. Bkz: [tek veritabanı: Dtu'lar değiştirme](#single-database-change-dtus) ve [esnek havuz: Edtu'lar değiştirmek](#elastic-pool-change-edtus).
- Her sorgu kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için bkz: [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Depolama

Kullanılan veritabanı alanı ulaştığında en büyük boyut sınırı, veritabanı ekler ve veri boyutunu artırın güncelleştirmeleri ve istemcileri alır bir [hata iletisi](sql-database-develop-error-messages.md). Veritabanı SEÇER ve SİLER devam başarılı olması.

Yüksek alan kullanımının karşılaşıldığında, azaltma seçenekleri şunlardır:

- Veritabanı veya esnek havuz ya da daha fazla dahil edilen depolama alanı almak için performans düzeyini değiştirme en büyük boyutunu artırma. Bkz: [SQL veritabanı kaynak sınırları](sql-database-dtu-resource-limits.md).
- Veritabanını bir esnek havuz ise, böylece kendi depolama alanını başka veritabanlarıyla paylaşılmayan sonra alternatif olarak veritabanı dışında havuzu taşınabilir.

### <a name="sessions-and-workers-requests"></a>Oturumlar ve çalışanları (istek sayısı) 

Hizmet katmanını ve performans düzeyine göre (Dtu ve Edtu) sayısını oturumlar ve çalışanları belirlenir. Yeni istekler oturumu veya çalışan sınırları ulaştı ve istemciler bir hata iletisi alıyorsunuz reddedilir. Kullanılabilir bağlantı sayısı uygulama tarafından denetlenebilir olsa da, eşzamanlı çalışan sayısı genellikle tahmin etmek ve denetlemek daha zordur. Bu zaman veritabanı kaynak sınırları ulaştı ve uzun çalışan sorguları nedeniyle çalışanları üst üste yığmak yoğun yük dönemlerini sırasında özellikle doğrudur. 

Yüksek oturum ya da çalışan kullanımı karşılaşıldığında, azaltma seçenekleri şunlardır:
- Veritabanı veya esnek havuz hizmet katmanı veya performans düzeyini artırma. Bkz [tek veritabanı: depolama boyutunu değiştirme](#single-database-change-storage-size), [tek veritabanı: Dtu'lar değiştirmek](#single-database-change-dtus), [esnek havuz: depolama boyutunu değiştirme](#elastic-pool-change-storage-size), ve [esnek havuz: Edtu'lar değiştirme ](#elastic-pool-change-edtus).
- İşlem kaynaklarını çakışması nedeniyle artan çalışan kullanımı neden olması durumunda her sorgu kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için bkz: [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için.
- Genel Azure sınırları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md).
- Dtu ve Edtu hakkında daha fazla bilgi için bkz: [Dtu ve Edtu](sql-database-what-is-a-dtu.md).
- Tempdb boyutu sınırları hakkında daha fazla bilgi için bkz: https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
