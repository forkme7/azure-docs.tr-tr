---
title: Azure SQL sunucuları ve veritabanlarını yönetme & Oluştur | Microsoft Docs
description: Azure SQL veritabanı sunucusu ve veritabanı kavramları hakkında ve sunucular ve veritabanları oluşturma ve yönetme hakkında bilgi edinin.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/10/2018
ms.author: carlrab
ms.openlocfilehash: 829cedea9752fe41ad24427339d3f13c2f3e371a
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="create-and-manage-azure-sql-database-servers-and-databases"></a>Azure SQL veritabanı sunucuları ve veritabanları oluşturma ve yönetme

SQL veritabanı veritabanları üç tür sunar:

- İçinde oluşturulan tek bir veritabanı bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) tanımlanan bir dizi [farklı iş yükleri için işlem ve depolama kaynaklarını](sql-database-service-tiers.md). Azure SQL veritabanını, belirli bir Azure bölge içinde oluşturulan bir Azure SQL Database mantıksal sunucusu ile ilişkilidir.
- Bir parçası olarak oluşturulmuş bir veritabanını bir [veritabanları havuzu](sql-database-elastic-pool.md) içinde bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) tanımlanan bir dizi [farklı iş yükleri için işlem ve depolama kaynaklarını](sql-database-service-tiers.md) olan Tüm havuzdaki veritabanları arasında paylaşılan. Azure SQL veritabanını, belirli bir Azure bölge içinde oluşturulan bir Azure SQL Database mantıksal sunucusu ile ilişkilidir.
- Bir [bir SQL server örneğini](sql-database-managed-instance.md) (yönetilen örneği) oluşturulan bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) işlem ve depolama kaynaklarını bu sunucu örneğindeki tüm veritabanları için tanımlanmış bir dizi. Yönetilen bir örneği, sistem ve kullanıcı veritabanlarını içerir. Yönetilen örnek uygulamayı yeniden olmadan veritabanı yükseltme-ve-kaydırma tam yönetilen bir PaaS sağlamak üzere tasarlanmıştır. Yönetilen örneği şirket içi SQL Server programlama modeli ile yüksek uyumluluk sağlar ve SQL Server özellikleri ve eşlik eden araçları ve Hizmetleri büyük çoğunu destekler.  

Microsoft Azure SQL veritabanı tablo veri akışı (TDS) Protokolü istemci sürümü 7.3 veya üst sürümünü destekler ve yalnızca şifrelenmiş TCP/IP bağlantılarını sağlar.

> [!IMPORTANT]
> SQL veritabanı örneği, yönetilen şu anda genel önizlemede, tek bir genel amaçlı hizmet katmanı sunar. Daha fazla bilgi için bkz. [SQL Veritabanı Yönetilen Örneği](sql-database-managed-instance.md). Bu makalenin sonraki bölümlerinde yönetilen örneği için geçerli değildir.

## <a name="what-is-an-azure-sql-logical-server"></a>Bir Azure SQL mantıksal sunucusuna nedir?

Merkezi bir yönetim noktası birden çok tek bir mantıksal sunucu görür veya [havuza alınmış](sql-database-elastic-pool.md) veritabanları, [oturumları](sql-database-manage-logins.md), [güvenlik duvarı kuralları](sql-database-firewall-configure.md), [kurallarıdenetleme](sql-database-auditing.md), [tehdit algılama ilkeleri](sql-database-threat-detection.md), ve [yük devretme grupları](sql-database-geo-replication-overview.md). Bir mantıksal sunucu, kaynak grubu farklı bir bölgede olabilir. Azure SQL veritabanı oluşturmadan önce mantıksal sunucunun mevcut olması gerekir. Bir sunucudaki tüm veritabanları, mantıksal sunucusu olarak aynı bölge içinde oluşturulur.

Bir mantıksal sunucu şirket içi dünyada alışkın olabileceğiniz bir SQL Server örneği farklıdır mantıksal bir yapıdır. SQL Veritabanı hizmeti, veritabanlarının mantıksal sunuculara göre konumuyla ilgili özel bir garanti vermez ve örnek düzeyinde erişim ya da özellik sunmaz. Buna karşılık, şirket içi dünyada alışkın olabileceğiniz bir SQL Server örneği için bir sunucu yönetilen bir SQL veritabanı örneğinde benzer.

Bir mantıksal sunucu oluşturduğunuzda, oturum açma hesabı ve bu sunucu üzerinde asıl veritabanını ve bu sunucuda oluşturulan tüm veritabanları için yönetici haklarına sahip parola bir sunucu sağlayın. Bir SQL oturum açma hesabı bu ilk hesaptır. Azure SQL veritabanı SQL kimlik doğrulaması ve Azure Active Directory kimlik doğrulaması için kimlik doğrulamasını destekler. Oturum açma ve kimlik doğrulama hakkında daha fazla bilgi için bkz: [yönetme veritabanları ve oturum açma bilgileri Azure SQL veritabanında](sql-database-manage-logins.md). Windows Kimlik Doğrulaması desteklenmez. 

> [!TIP]
> Geçerli bir kaynak grubu ve sunucu adları için bkz: [adlandırma kuralları ve sınırlamaları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).
>

Azure SQL Veritabanı mantıksal sunucusu:

- Bir Azure aboneliği içinde oluşturulur, ancak içerdiği kaynaklarla birlikte başka bir aboneliğe taşınabilir
- Veritabanları, elastik havuzlar ve veri ambarları için üst kaynaktır
- Veritabanları, esnek havuzlar ve veri ambarlarında için bir ad alanı sağlar
- Mantıksal bir kapsayıcısıdır güçlü ömrü semantiği ile - bir sunucu silme ve kapsanan veritabanları, esnek havuzlar ve veri ambarlarında siler
- Katılan [Azure rol tabanlı erişim denetimi (RBAC)](/azure/role-based-access-control/overview) -veritabanları, esnek havuzlar ve bir sunucu içindeki veri ambarlarında sunucudan erişim hakları devral
- Veritabanları, esnek havuzlar ve Azure kaynağı için veri ambarlarında kimliğini yüksek düzey öğenin (veritabanları ve havuzları için URL şeması bakın) yönetmek amacıyla mi
- Bir bölgedeki kaynakları birlikte bulundurur
- Veritabanı erişimi için bağlantı uç noktası sağlar (<serverName>.database.windows.net)
- Bir ana veritabanına bağlanarak DMV’ler aracılığıyla içerdiği kaynaklarla ilgili meta verilere erişim sağlar 
- Kendi veritabanlarına - oturumları uygulanır, güvenlik duvarı, Denetim, tehdit algılama, vb. yönetim ilkeleri için kapsam sağlar. 
- Üst abonelik içindeki bir kota tarafından kısıtlanmış (varsayılan - abonelik başına yirmi sunucuları [abonelik sınırlar buraya bakın](../azure-subscription-service-limits.md))
- Veritabanı kotasına ve DTU veya vCore kota kapsamın (45000 DTU gibi) içerdiği kaynakların sağlar
- Kapsanan kaynaklardaki etkinleştirilen özellikleri için sürüm oluşturma kapsamı 
- Sunucu düzeyinde asıl kullanıcı bilgileri bir sunucudaki tüm veritabanlarını yönetebilir
- Şirketinizde sunucu üzerindeki bir veya daha fazla veritabanına erişim verilmiş SQL Server örneklerinde bulunanlara benzer kullanıcı bilgileri içerebilir ve sınırlı yönetici hakları alabilir. Daha fazla bilgi için bkz. [Kullanıcı Bilgileri](sql-database-manage-logins.md).
- Mantıksal bir sunucuda oluşturulan tüm kullanıcı veritabanları için varsayılan harmanlama `SQL_LATIN1_GENERAL_CP1_CI_AS`, burada `LATIN1_GENERAL` İngilizce (Birleşik Devletler) `CP1` kod sayfası 1252, `CI` , küçük harf duyarlıdır ve `AS` aksan duyarlı değil.

## <a name="azure-sql-databases-protected-by-sql-database-firewall"></a>SQL veritabanı güvenlik duvarı tarafından korunan azure SQL veritabanları

Verilerinizi korumaya yardımcı olmak için bir [SQL veritabanı Güvenlik Duvarı](sql-database-firewall-configure.md) veritabanı sunucunuz ya da kendi veritabanlarından bağlantınızı dışında Azure aboneliği bağlantısı üzerinden doğrudan sunucuya hiçbirini tüm erişimi engeller. Ek bağlantı etkinleştirmek için şunları yapmalısınız [bir veya daha fazla güvenlik duvarı kuralları oluşturma](sql-database-firewall-configure.md#creating-and-managing-firewall-rules). Oluşturma ve esnek havuzlarını yönetmek için bkz: [esnek havuzlar](sql-database-elastic-pool.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-portal"></a>Azure SQL sunucuları, veritabanları ve güvenlik duvarları Azure Portalı'nı kullanarak yönetme

Azure SQL Database'in kaynak grubu vaktinden veya sunucu oluştururken oluşturabilirsiniz. 

### <a name="create-a-blank-sql-server-logical-server"></a>Boş bir SQL server (mantıksal sunucu) oluşturun

Bir Azure SQL veritabanı sunucusu (olmadan bir veritabanı) kullanarak oluşturmak için [Azure portal](https://portal.azure.com), boş bir SQL server (mantıksal) form gidin.  

### <a name="create-a-blank-or-sample-sql-database"></a>Boş veya örnek bir SQL veritabanı oluşturma

Kullanarak bir Azure SQL veritabanı oluşturmak için [Azure portal](https://portal.azure.com), boş bir SQL veritabanı formu gidin ve istenen bilgileri sağlayın. Azure SQL Database'in kaynak grubu ve mantıksal sunucu vaktinden veya veritabanı oluşturulurken oluşturabilirsiniz. Boş bir veritabanı oluşturmayı veya Adventure Works LT. üzerinde temel bir örnek veritabanı oluşturma 

  ![create database-1](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Veritabanınız için fiyatlandırma katmanı seçme konusunda daha fazla bilgi için bkz: [hizmet katmanları](sql-database-service-tiers.md).

Yönetilen bir örneğini oluşturmak için bkz: [yönetilen örneği oluşturma](sql-database-managed-instance-create-tutorial-portal.md)

### <a name="manage-an-existing-sql-server"></a>Mevcut bir SQL server'ı yönetme

Var olan bir sunucuyu yönetmek için çeşitli yöntemler - gibi belirli SQL veritabanı sayfası uğradıysa kullanarak sunucuya gidin **SQL sunucuları** sayfasında veya **tüm kaynakları** sayfası. 

Varolan bir veritabanını yönetmek için gidin **SQL veritabanları** sayfasında ve yönetmek istediğiniz veritabanını tıklayın. Aşağıdaki ekran görüntüsü, bir veritabanından için sunucu düzeyinde güvenlik duvarı ayarı başlamak gösterilmiştir **genel bakış** bir veritabanı için sayfası. 

   ![sunucu güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> Bir veritabanı performans özelliklerini yapılandırmak için bkz: [hizmet katmanları](sql-database-service-tiers.md).
>

> [!TIP]
> Bir Azure portalı Hızlı Başlangıç için bkz: [Azure portalında bir Azure SQL veritabanı oluşturma](sql-database-get-started-portal.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-powershell"></a>Azure SQL sunucuları, veritabanları ve güvenlik duvarları PowerShell kullanarak yönetme

Azure SQL server, veritabanları ve güvenlik duvarları Azure PowerShell ile oluşturmak ve yönetmek için aşağıdaki PowerShell cmdlet'lerini kullanın. Gerekirse yükleyin veya PowerShell yükseltme, bakın [yükleme Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps). Oluşturma ve esnek havuzlarını yönetmek için bkz: [esnek havuzlar](sql-database-elastic-pool.md).

| Cmdlet | Açıklama |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Bir veritabanı oluşturur |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Bir veya daha fazla veritabanı alır|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Bir veritabanı özelliklerini ayarlar ya da varolan bir veritabanını bir esnek havuza taşır|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Bir veritabanı kaldırır|
|[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Bir kaynak grubu oluşturur]
|[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Sunucu oluşturur|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Sunucuları hakkında bilgi döndürür|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlserver)|Bir sunucu özelliklerini değiştirir|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Bir sunucuyu kaldırır|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Bir sunucu düzeyinde güvenlik duvarı kuralı oluşturur |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Bir sunucu için güvenlik duvarı kurallarını alır|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Bir sunucu güvenlik duvarı kuralında değiştirir|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Bir güvenlik duvarı kuralı bir sunucudan siler.|
| New-AzureRmSqlServerVirtualNetworkRule | Oluşturur bir [ *sanal ağ kuralı*](sql-database-vnet-service-endpoint-rule-overview.md)bağlı bir sanal ağ hizmeti uç noktası olan bir alt ağ olarak. |

> [!TIP]
> PowerShell Hızlı Başlangıç için bkz: [PowerShell kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-get-started-portal.md). PowerShell örnek komut dosyaları için bkz: [kullanım tek bir Azure SQL veritabanı oluşturmak ve bir güvenlik duvarı kuralı yapılandırmak için PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) ve [İzleyici ve ölçek tek bir SQL veritabanı PowerShell kullanarak](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-cli"></a>Azure SQL sunucuları, veritabanları ve güvenlik duvarları Azure CLI kullanarak yönetme

Azure SQL server, veritabanları ve güvenlik duvarları ile oluşturmak ve yönetmek için [Azure CLI](/cli/azure), aşağıdaki [Azure CLI SQL veritabanı](/cli/azure/sql/db) komutları. CLI’yi tarayıcınızda çalıştırmak için [Cloud Shell](/azure/cloud-shell/overview) kullanın veya macOS, Linux ya da Windows’da [yükleyin](/cli/azure/install-azure-cli). Oluşturma ve esnek havuzlarını yönetmek için bkz: [esnek havuzlar](sql-database-elastic-pool.md).

| Cmdlet | Açıklama |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az_sql_db_create) |Bir veritabanı oluşturur|
|[az sql db listesi](/cli/azure/sql/db#az_sql_db_list)|Veritabanları ve tüm veri ambarlarında bir sunucu veya esnek havuzdaki tüm veritabanları listeler.|
|[az sql db listesi-sürümleri](/cli/azure/sql/db#az_sql_db_list_editions)|Listeleri kullanılabilir hizmet hedefleri ve depolama sınırları|
|[az sql db listesi-kullanımları](/cli/azure/sql/db#az_sql_db_list_usages)|Kullanımları döndürür veritabanı|
|[az sql db Göster](/cli/azure/sql/db#az_sql_db_show)|Bir veritabanı veya veri ambarı alır|
|[az sql db update](/cli/azure/sql/db#az_sql_db_update)|Bir veritabanını güncelleştirir|
|[az sql db Sil](/cli/azure/sql/db#az_sql_db_delete)|Bir veritabanı kaldırır|
|[az group create](/cli/azure/group#az_group_create)|Bir kaynak grubu oluşturur|
|[az sql server create](/cli/azure/sql/server#az_sql_server_create)|Sunucu oluşturur|
|[az sql sunucu listesi](/cli/azure/sql/server#az_sql_server_list)|Sunucuları listeler|
|[az sql server listesi-kullanımları](/cli/azure/sql/server#az_sql_server_list_usages)|Sunucu kullanımları döndürür|
|[az sql server Göster](/cli/azure/sql/server#az_sql_server_show)|Bir sunucu alır|
|[az sql server güncelleştirmesi](/cli/azure/sql/server#az_sql_server_update)|Bir sunucu güncelleştirir|
|[az sql server silme](/cli/azure/sql/server#az_sql_server_delete)|Bir sunucu siler|
|[az sql server güvenlik duvarı kuralı oluşturma](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Sunucu güvenlik duvarı kuralı oluşturur|
|[az sql server güvenlik duvarı kuralı listesi](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Bir sunucudaki güvenlik duvarı kurallarını listeler|
|[az sql server güvenlik duvarı kuralı Göster](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Bir güvenlik duvarı kuralı ayrıntılarını gösterir|
|[az sql server güvenlik duvarı kuralı güncelleştirme](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Bir güvenlik duvarı kuralını güncelleştirir|
|[az sql server güvenlik duvarı kuralını Sil](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Bir güvenlik duvarı kuralını siler|

> [!TIP]
> Azure CLI Hızlı Başlangıç için bkz: [Azure CLI kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-get-started-cli.md). Azure CLI örnek komut dosyaları için bkz: [kullanım tek bir Azure SQL veritabanı oluşturmak ve bir güvenlik duvarı kuralı yapılandırmak için CLI](scripts/sql-database-create-and-configure-database-cli.md) ve [kullanımı izlemek ve tek bir SQL veritabanı ölçeklendirmek için CLI](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-transact-sql"></a>Azure SQL sunucuları, veritabanları ve güvenlik duvarları Transact-SQL kullanarak yönetme

Azure SQL server, veritabanları ve güvenlik duvarları Transact-SQL ile oluşturmak ve yönetmek için aşağıdaki T-SQL komutlarını kullanın. Azure portalını kullanarak bu komutlar gönderebilirsiniz [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), veya bir Azure SQL veritabanı sunucusuna bağlanın ve Transact-SQL geçirmek başka bir programı komutları. Esnek havuzlarını yönetmek için bkz: [esnek havuzlar](sql-database-elastic-pool.md).

> [!IMPORTANT]
> Oluşturamaz veya Transact-SQL kullanarak bir sunucu silme.
>

| Komut | Açıklama |
| --- | --- |
|[Veritabanı (Azure SQL veritabanı) oluşturma](/sql/t-sql/statements/create-database-azure-sql-database)|Yeni bir veritabanı oluşturur. Yeni bir veritabanı oluşturmak için ana veritabanına bağlanması gerekir.|
| [ALTER DATABASE (Azure SQL veritabanı)](/sql/t-sql/statements/alter-database-azure-sql-database) |Bir Azure SQL veritabanı değiştirir. |
|[ALTER DATABASE (Azure SQL veri ambarı)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Bir Azure SQL veri ambarı değiştirir.|
|[VERİTABANINI (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Bir veritabanını siler.|
|[sys.database_service_objectives (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Edition (hizmet katmanı), hizmet hedefi (fiyatlandırma katmanı) ve esnek havuz adı, varsa Azure SQL veritabanına veya Azure SQL Data Warehouse için döndürür. Azure SQL Database sunucusu ana veritabanında oturum açtıysanız, bilgiler tüm veritabanlarını döndürür. Azure SQL Data Warehouse için ana veritabanına bağlı olmalıdır.|
|[sys.dm_db_resource_stats (Azure SQL veritabanı)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Bir Azure SQL Database veritabanı için CPU, g/ç ve bellek tüketimini döndürür. Veritabanında hiçbir etkinlik olsa 15 dakikada için bir satır yok.|
|[sys.resource_stats (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|CPU kullanımı ve depolama verileri için bir Azure SQL veritabanına döndürür. Veriler toplanır ve beş dakikalık aralıklarla içinde toplanır.|
|[sys.database_connection_stats (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|SQL veritabanı veritabanı bağlantı olayları, bir veritabanı bağlantısı başarı ve başarısızlık durumlarının özetini istatistiklerini içerir. |
|[sys.event_log (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Başarılı Azure SQL Database veritabanı bağlantıları, bağlantı hataları ve kilitlenmeleri döndürür. Veritabanı etkinliklerinizi SQL veritabanı ile ilgili sorunları giderme veya izlemek için bu bilgileri kullanın.|
|[sp_set_firewall_rule (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Oluşturur veya SQL veritabanı sunucunuzun sunucu düzeyinde güvenlik duvarı ayarlarını güncelleştirir. Bu saklı yordam yalnızca ana veritabanında sunucu düzeyinde asıl oturum açmak için kullanılabilir. Sunucu düzeyinde güvenlik duvarı kuralı yalnızca Azure düzeyi izinleri olan bir kullanıcı tarafından ilk sunucu düzeyinde güvenlik duvarı kural oluşturulduktan sonra Transact-SQL kullanılarak oluşturulabilir|
|[sys.firewall_rules (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Microsoft Azure SQL veritabanı ile ilişkili sunucu düzeyinde güvenlik duvarı ayarları hakkında bilgi verir.|
|[sp_delete_firewall_rule (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Sunucu düzeyinde güvenlik duvarı ayarları, SQL veritabanı sunucusundan kaldırır. Bu saklı yordam yalnızca ana veritabanında sunucu düzeyinde asıl oturum açmak için kullanılabilir.|
|[sp_set_database_firewall_rule (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Oluşturur veya Azure SQL Database veya SQL Data Warehouse için veritabanı düzeyinde güvenlik duvarı kuralları güncelleştirir. Veritabanı güvenlik duvarı kuralları SQL veritabanı kullanıcı veritabanlarında ve ana veritabanı için yapılandırılabilir. Veritabanı güvenlik duvarı kurallarını kullanarak, veritabanı kullanıcıları bağımsız faydalıdır. |
|[sys.database_firewall_rules (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Microsoft Azure SQL veritabanı ile ilişkili veritabanı düzeyinde güvenlik duvarı ayarları hakkında bilgi verir. |
|[sp_delete_database_firewall_rule (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Veritabanı düzeyi güvenlik duvarı ayarı, Azure SQL Database veya SQL Data Warehouse kaldırır. |


> [!TIP]
> Microsoft Windows üzerinde SQL Server Management Studio'yu kullanarak hızlı başlangıç için bkz: [Azure SQL Database: bağlanmak ve verileri sorgulamak için kullanım SQL Server Management Studio](sql-database-connect-query-ssms.md). Visual Studio Code macOS, Linux veya Windows kullanarak bir hızlı başlangıç için bkz: [Azure SQL Database: kullanım Visual Studio Code bağlanmak ve verileri sorgulamak için](sql-database-connect-query-vscode.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-rest-api"></a>Azure SQL sunucuları, veritabanları ve güvenlik duvarları REST API kullanarak yönetme

Azure SQL server, veritabanları ve güvenlik duvarları oluşturmak ve yönetmek için bu REST API istekleri kullanın.

| Komut | Açıklama |
| --- | --- |
|[Sunucuları - oluştur veya güncelleştir](/rest/api/sql/servers/createorupdate)|Oluşturur veya yeni bir sunucu güncelleştirir.|
|[Sunucuları - Sil](/rest/api/sql/servers/delete)|Bir SQL server siler.|
|[Sunucuları - Al](/rest/api/sql/servers/get)|Bir sunucu alır.|
|[Sunucuları - liste](/rest/api/sql/servers/list)|Sunucularının bir listesini döndürür.|
|[Sunucuları - kaynak grubuna göre listesi](/rest/api/sql/servers/listbyresourcegroup)|Bir kaynak grubunda sunucularının bir listesini döndürür.|
|[Sunucuları - güncelleştirme](/rest/api/sql/servers/update)|Var olan bir sunucu güncelleştirir.|
|[Veritabanları - oluştur veya güncelleştir](/rest/api/sql/databases/createorupdate)|Yeni bir veritabanı oluşturur veya varolan bir veritabanını güncelleştirir.|
|[Veritabanları - Al](/rest/api/sql/databases/get)|Bir veritabanı alır.|
|[Veritabanı - esnek havuz tarafından Al](/rest/api/sql/databases/getbyelasticpool)|Bir veritabanını bir esnek havuz içinde alır.|
|[Önerilen esnek havuzu tarafından veritabanları - Al](/rest/api/sql/databases/getbyrecommendedelasticpool)|Bir veritabanı içinde recommented bir esnek havuz alır.|
|[Veritabanı - esnek havuz göre listesi](/rest/api/sql/databases/listbyelasticpool)|Bir esnek havuz veritabanlarının bir listesini döndürür.|
|[Veritabanları - önerilen esnek havuz göre listesi](/rest/api/sql/databases/listbyrecommendedelasticpool)|Önerilen esnek havuz içindeki veritabanlarının bir listesini döndürür.|
|[Veritabanları - sunucu tarafından listesi](/rest/api/sql/databases/listbyserver)|Bir sunucu veritabanlarının bir listesini döndürür.|
|[Veritabanları - güncelleştirme](/rest/api/sql/databases/update)|Varolan bir veritabanını güncelleştirir.|
|[Güvenlik duvarı kuralları - oluştur veya güncelleştir](/rest/api/sql/firewallrules/createorupdate)|Oluşturur veya bir güvenlik duvarı kuralı güncelleştirir.|
|[Güvenlik duvarı kuralları - Sil](/rest/api/sql/firewallrules/delete)|Bir güvenlik duvarı kuralını siler.|
|[Güvenlik duvarı kuralları - Al](/rest/api/sql/firewallrules/get)|Bir güvenlik duvarı kuralı alır.|
|[Güvenlik duvarı kuralları - sunucu tarafından listesi](/rest/api/sql/firewallrules/listbyserver)|Güvenlik duvarı kurallarının bir listesini döndürür.|

## <a name="next-steps"></a>Sonraki adımlar

- Azure için SQL Server veritabanını geçirme hakkında bilgi edinmek için [Azure SQL veritabanına geçirme](sql-database-cloud-migrate.md).
- Desteklenen özellikler hakkında bilgi edinmek için bkz. [Özellikler](sql-database-features.md).
