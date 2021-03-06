---
title: Azure SQL veri ambarı Data Warehouse birimlerinde (Dwu, cDWUs) | Microsoft Docs
description: Veri ambarı birimi (Dwu, cDWUs) fiyat ve performans ve birim sayısını değiştirmek nasıl en iyi duruma getirme ideal sayısını seçmeye ilişkin öneriler.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 94791e4dc3d3c841dde4685d34d4e3fdaf7d9af7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="data-warehouse-units-dwus-and-compute-data-warehouse-units-cdwus"></a>Veri ambarı birimlerini (Dwu'lar) ve bilgi işlem Data Warehouse birimleri (cDWUs)
Veri ambarı birimi (Dwu, cDWUs) fiyat ve performans ve birim sayısını değiştirmek nasıl en iyi duruma getirme ideal sayısını seçmeye ilişkin öneriler. 

## <a name="what-are-data-warehouse-units"></a>Data Warehouse birimleri nelerdir?
Veri ambarı birimlerini (Dwu'lar) olarak adlandırılan hesaplama ölçek birimleri, SQL veri ambarı CPU, bellek ve g/ç ile paketlenmiştir. Bir DWU soyut, normalleştirilmiş bir ölçü birimi performans ve işlem kaynaklarını temsil eder. Hizmet düzeyi değiştirerek sırayla performansını ve maliyetini sisteminizi ayarlar sistemine ayrılan Dwu sayısı değiştirin. 

Daha yüksek performans için ödeme için veri ambarı birim sayısını artırabilirsiniz. Daha az performans için ödeme için data warehouse birimleri azaltın. Veri ambarı birimlerini değiştirme depolama maliyetleri etkilemesini depolama ve işlem maliyetleri ayrı olarak faturalandırılır.

Performans veri ambarı birimleri için bu veri ambarı iş yükü ölçümlerinin dayanır:

- Ne kadar hızlı standart veri ambarı sorgusu çok sayıda satır tarayabilir ve karmaşık bir toplama gerçekleştirmek? Bu işlem, g/ç ve CPU yoğun olur.
- Ne kadar hızlı veri ambarı verileri Azure Storage Bloblarında veya Azure Data Lake işleyebilen? Bu işlem, ağ ve CPU yoğun olur. 
- Ne kadar hızlı yapabilirsiniz [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) T-SQL komutunu kopyalayın tablo? Bu işlem, verilerin depolama alanından okunmasını, gerecin tüm düğümlerine dağıtılmasını ve yeniden depolama alanına yazılmasını içerir. Bu CPU, g/ç ve ağ yoğunluklu bir işlemdir.

Dwu artırma:
- Doğrusal olarak taramaları, toplamalar ve CTAS deyimleri için sistem performansını değiştirir
- Okuyucular ve PolyBase yükleme işlemleri için yazarları sayısını artırır
- Eş zamanlı sorgular ve eşzamanlılık yuvaları en fazla sayısını artırır.

## <a name="service-level-objective"></a>Hizmet düzeyi hedefi
Hizmet düzeyi hedefi (SLO), veri ambarı maliyet ve performans düzeyini belirler ölçeklenebilirlik ayardır. Hizmet düzeyleri Gen2 için işlem data warehouse birimlerinde (cDWU), örneğin DW2000c ölçülür. Gen1 hizmet düzeyleri Dwu, örneğin DW2000 ölçülür. 

T-SQL servıce_objectıve ayarı, hizmet düzeyi ve veri ambarınız için performans katmanı belirler.

```sql
--Gen1
CREATE DATABASE myElasticSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000'
)
;

--Gen2
CREATE DATABASE myComputeSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000c'
)
;
```

## <a name="performance-tiers-and-data-warehouse-units"></a>Performans katmanı ve Data Warehouse birimleri

Her performans katmanı biraz farklı bir ölçü kendi data warehouse birimleri için kullanır. Ölçek birimi için faturalama doğrudan çevirir bu fark fatura üzerinde yansıtılır.

- Gen1 veri ambarları, veri ambarı birimi (Dwu) ölçülür.
- Gen2 veri warehousesr işlem Data Warehouse birimleri (cDWUs) ölçülür. 

Dwu ve cDWUs yukarı veya aşağı ölçeklendirme işlem desteği ve duraklatma işlem veri ambarını kullan gerekmiyorsa. Bu işlemlerin tümü isteğe bağlı. Gen2 performansını artırmak için işlem düğümlerinde bir yerel disk tabanlı önbelleği kullanır. Ölçeklendirme veya sistem duraklatmak, önbellekte geçersiz kılınan ve önbellek ısıtma, bir süre en iyi performans elde önce böylece gereklidir.  

Data warehouse birimleri arttıkça, bilgi işlem kaynakları doğrusal olarak artmaktadır. Gen2 yüksek ölçek ve en iyi sorgu performansını sağlar ancak daha yüksek bir giriş fiyat sahiptir. Performans için sabit bir isteğe bağlı olan işletmeler için tasarlanmıştır. Bu sistemler önbellek çoğu kullanılmasını sağlamak. 

### <a name="capacity-limits"></a>Kapasite sınırları
Her bir SQL server (örneğin, myserver.database.windows.net) sahip bir [veritabanı işlem birimi (DTU)](../sql-database/sql-database-what-is-a-dtu.md) kota data warehouse birimleri belirli bir sayıda izin verir. Daha fazla bilgi için bkz: [iş yükü yönetim kapasite limitlerini](sql-data-warehouse-service-capacity-limits.md#workload-management).


## <a name="how-many-data-warehouse-units-do-i-need"></a>Kaç tane data warehouse birimleri ihtiyacım var mı?
Data warehouse birimleri ideal sayısı çok yükünüzü ve sisteme yüklenen veri miktarına bağlıdır.

İş yükü için en iyi DWU bulmak için adımlar:

1. Daha küçük bir DWU seçerek başlayın. 
2. Seçili Dwu sayısı, gözlemlemek performans karşılaştırıldığında Gözlemleme sisteme, veri yüklerini test gibi uygulama performansını izleyin.
3. Tüm ek gereksinimleri için en yüksek etkinlik düzenli dönemleri tanımlayın. İş yükünü önemli yükselmeleri ve troughs etkinliği gösterir ve sık ölçeklendirmek için iyi bir neden yoktur.

SQL Data Warehouse, işlem ve sorgu oldukça büyük miktarda veri çok büyük miktarda kaynak sağlayabilirsiniz genişleme sistemidir. Özellikle büyük Dwu ölçeklendirmeye yönelik gerçek kapasitesini görmek için veri kümesi, CPU'lar akış için yeterli veri sağlamak için ölçeklendirmenize göre ölçeklendirme önerilir. Ölçek test etmek için en az 1 TB kullanmanızı öneririz.

> [!NOTE]
>
> İş işlem düğümleri arasında bölünebilir, sorgu performansı ile daha fazla paralelleştirme yalnızca artırır. Ölçeklendirme performansınızı değişmeyen olduğunu fark ederseniz, tablo tasarımı ve/veya sorgularınızı ayarlamak gerekebilir. Kılavuzu ayarlama sorgusu için bkz: [kullanıcı sorguları yönetme](sql-data-warehouse-overview-manage-user-queries.md). 

## <a name="permissions"></a>İzinler

Veri ambarı birimlerini değiştirme açıklanan izinleri gerektirir [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql). 

## <a name="view-current-dwu-settings"></a>Geçerli DWU ayarlarını görüntüle

Geçerli DWU ayarını görüntülemek için:

1. SQL Server Nesne Gezgini Visual Studio'da açın.
2. Mantıksal SQL veritabanı sunucusu ile ilişkili ana veritabanına bağlanın.
3. Sys.database_service_objectives dinamik yönetim görünümünden seçin. Örnek aşağıda verilmiştir: 

```sql
SELECT  db.name [Database]
,       ds.edition [Edition]
,       ds.service_objective [Service Objective]
FROM    sys.database_service_objectives   AS ds
JOIN    sys.databases                     AS db ON ds.database_id = db.database_id
;
```

## <a name="change-data-warehouse-units"></a>Veri ambarı birimlerini değiştirme

### <a name="azure-portal"></a>Azure portalına
Dwu veya cDWUs değiştirmek için:

1. Açık [Azure portal](https://portal.azure.com), veritabanınızı açın ve'ı tıklatın **ölçek**.

2. Altında **ölçek**, kaydırıcıyı sola veya DWU ayarını değiştirmek için sağ.

3. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Tıklatın **Evet** onaylamak için veya **hiçbir** iptal etmek için.

### <a name="powershell"></a>PowerShell
Dwu veya cDWUs değiştirmek için kullanın [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) PowerShell cmdlet'i. Aşağıdaki örnek, MyServer sunucusunda barındırılan MySQLDW veritabanı için DW1000 için hizmet düzeyi hedefi ayarlar.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

Daha fazla bilgi için bkz: [SQL Data Warehouse için PowerShell cmdlet'leri](sql-data-warehouse-reference-powershell-cmdlets.md)

### <a name="t-sql"></a>T-SQL
T-SQL ile geçerli DWU veya cDWU ayarlarını görüntülemek, ayarlarını değiştirin ve ilerleme durumunu denetleyin. 

Dwu veya cDWUs değiştirmek için:

1. Mantıksal SQL veritabanı sunucunuzla ilişkili ana veritabanına bağlanın.
2. Kullanım [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql) TSQL deyimi. Aşağıdaki örnek, DW1000 için MySQLDW veritabanı için hizmet düzeyi hedefi ayarlar. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

### <a name="rest-apis"></a>REST API'leri

Dwu değiştirmek için kullanın [oluşturma veya güncelleştirme veritabanı](/rest/api/sql/databases/createorupdate) REST API. Aşağıdaki örnek, MyServer sunucusunda barındırılan MySQLDW veritabanı için DW1000 için hizmet düzeyi hedefi ayarlar. Sunucu ResourceGroup1 adlı bir Azure kaynak grubunda yer alıyor.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

Daha fazla REST API örnekler için bkz: [SQL Data Warehouse için REST API'leri](sql-data-warehouse-manage-compute-rest-api.md).

## <a name="check-status-of-dwu-changes"></a>DWU değişiklikleri durumunu denetleme

DWU değişiklikleri tamamlanması birkaç dakika sürebilir. Otomatik ölçeklendirme, belirli işlemleri başka bir eylem işlemine devam etmeden önce tamamlandığından emin olmak için mantığı uygulayın. 

Çeşitli uç noktaları aracılığıyla veritabanı durumunu denetleme, doğru Otomasyon uygulamaya olanak tanır. Portal bir işlem ve veritabanları geçerli durumunun tamamlandıktan sonra bildirim sağlar ancak programlı durumunu denetlemek için izin vermiyor. 

Azure portal ile ölçek genişletme işlemleri için veritabanı durumunu denetleyemezsiniz.

DWU değişiklikleri durumunu denetlemek için:

1. Mantıksal SQL veritabanı sunucunuzla ilişkili ana veritabanına bağlanın.
2. Veritabanı durumunu denetlemek için aşağıdaki sorguyu gönderin.


```sql
SELECT    *
FROM      sys.databases
;
```

3. İşlemin durumunu denetlemek için aşağıdaki sorguyu Gönder

```sql
SELECT    *
FROM      sys.dm_operation_status
WHERE     resource_type_desc = 'Database'
AND       major_resource_id = 'MySQLDW'
;
```

Bu DMV çeşitli yönetimi hakkında bilgi işlem ve da işlemin durumunu gibi SQL veri ambarı işlemleri IN_PROGRESS olması döndürür veya tamamlandı.

## <a name="the-scaling-workflow"></a>Ölçeklendirme iş akışı

Bir ölçeklendirme işlemi başlattığınızda sistem önce açık olan tüm oturumları, tutarlı bir duruma emin olmak için açık işlemler geri alma devre dışı bırakır. Bu işlem geri alma işlemi tamamlandıktan sonra ölçeklendirme ölçek işlemleri için yalnızca oluşur.  

- Bir ölçek büyütme işleminin sistem hükümleri ek işlem ve depolama katmanı reattaches. 
- Gereksiz ölçek azaltma işlemi için düğümleri depolama biriminden ayırmak ve geri kalan düğümlerde yeniden ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Performansı yönetme hakkında daha fazla bilgi için bkz: [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md) ve [bellek ve eşzamanlılık sınırları](memory-and-concurrency-limits.md).



