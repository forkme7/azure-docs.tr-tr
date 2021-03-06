---
title: MySQL ilişkisel veritabanı hizmeti için Azure veritabanı'nın genel bakış
description: Azure veritabanı MySQL ilişkisel veritabanı hizmeti için genel bakış.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 03/20/2018
ms.custom: mvc
ms.openlocfilehash: 3b7ae08446e672c218e7278cff4f77739286fead
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="what-is-azure-database-for-mysql"></a>Azure veritabanı için MySQL nedir?
Azure için MySQL veritabanıdır göre Microsoft bulut ilişkisel veritabanı hizmeti [MySQL Community Edition](https://www.mysql.com/products/community/) veritabanı altyapısı. Azure veritabanı için MySQL sunar:

- Ek ücret ödemeden yerleşik yüksek kullanılabilirlik.
- Kapsamlı kullandıkça öde fiyatlandırması kullanılarak öngörülebilir performans.
- Saniyeler içinde hemen ölçeklendirme.
- Bekleyen ve hareket halindeki hassas verileri korumaya yönelik güvenlik.
- Otomatik yedeklemeler ve 35 güne kadar belirli bir noktaya geri yükleme.
- Kurumsal düzeyde güvenlik ve uyumluluk.

Bu özellikler neredeyse hiçbir yönetim gerektirir ve tüm hiçbir ek ücret sağlanır. Hızlı Uygulama geliştirme ve pazara zamanınızı hızlandırmaya yerine değerli zaman ve sanal makineleri ve altyapıyı yönetmek için kaynak ayırma odaklanmanıza olanak sağlar. Ayrıca, uygulamanızı hızını ve verimliliği ile tüm yeni yetenekler öğrenmek zorunda kalmadan, işletme taleplerine teslim etmek için seçtiğiniz platform ve açık kaynaklı araçları ile geliştirme devam edebilirsiniz.

![Azure veritabanı için MySQL kavramsal diyagramı](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Bu makale Azure veritabanı giriş MySQL temel kavramlar ve performans, ölçeklenebilirlik ve yönetilebilirlik ayrıntıları keşfetmek için bağlantılar ile ilgili özellikler için yazılmıştır. Başlamanıza yardımcı olacak şu hızlı başlangıçlara bakın:
- [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure CLI kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-cli.md)

Azure CLI örnekleri için bkz:
- [Azure veritabanı için MySQL için Azure CLI örnekleri](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Saniyeler içinde performansı ve ölçeği ayarlama
MySQL hizmeti için Azure veritabanı birkaç hizmet katmanları sunar: Basic, genel amaçlı ve bellek için iyileştirilmiş. Her katman, hafiften ağıra kadar tüm iş yüklerini desteklemek üzere farklı performans ve özellikler getirir. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca ihtiyacınız olan kaynaklar için ve yalnızca bunlara ihtiyacınız olduğunda ödeme yaparsınız. Ayrıntılar için bkz. [Fiyatlandırma katmanları](concepts-service-tiers.md).

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı
Ne zaman artırılacağına ve ne zaman azaltılacağını nasıl karar verirsiniz? Yerleşik performans izleme ve uyarı özellikleri, vCores üzerinde temel performans değerlendirmeleri birlikte kullanın. Bu Araçları'nı kullanarak, hızlı bir şekilde ölçeklendirme vCores etkisini yukarı veya aşağı geçerli veya tahmini performans ihtiyaçlarınıza göre değerlendirebilirsiniz. Ayrıntılar için bkz. [Uyarılar](howto-alert-on-metric.md).

## <a name="keep-your-app-and-business-running"></a>Uygulamanızın ve işinizin hiç kesintiye uğramamasını sağlayın
Azure'nın endüstri lideri genel bir veri merkezleri, Microsoft tarafından yönetilen ağ tarafından desteklenen % 99,99 kullanılabilirlik hizmet düzeyi sözleşmesi (SLA), uygulamanızın 7/24 çalıştıran tutmaya yardımcı olur. MySQL sunucusu için her Azure veritabanı ile yerleşik güvenlik, hata toleransı ve satın alma veya tasarlama, derleme ve yönetmek için Aksi durumda olması gereken veri koruma yararlanın. MySQL için Azure veritabanı ile bir sunucu a 35 gün kadar daha önceki bir duruma kurtarmak için zaman içinde nokta geri yükleme kullanabilirsiniz.

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama
Azure veritabanı hizmetleri Azure veritabanı için MySQL anlayışına veri güvenliği yılda vardır, erişimi sınırlamak, çalışmıyorken veri ve hareket halinde korumak ve yardımcı özelliklerle etkinliğini izleyin. Azure'ın platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi](https://www.microsoft.com/en-us/trustcenter/security)'ni ziyaret edin.

MySQL hizmeti için Azure veritabanına veri çalışmıyorken için depolama şifrelemesi kullanır. Yedeklemeler dahil verileri (hariç sorgu çalıştırılırken altyapısı tarafından oluşturulan geçici dosyalar) diskte şifrelenir. Hizmet, Azure depolama şifrelemesi kapsamında yer alan AES 256-bit şifrelemesini kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.

Varsayılan olarak, Azure veritabanı için MySQL hizmeti gerektirecek şekilde yapılandırılmış [SSL bağlantı güvenliği](./concepts-ssl-connection-security.md) veri hareket halinde ağ üzerinden için. Veritabanı sunucunuz ve istemci uygulamalarınız arasında SSL bağlantılarını zorlamayı uygulamanız ile sunucu arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korumaya yardımcı olur.  İsteğe bağlı olarak, istemci uygulamanızın SSL bağlantısını desteklememesi durumunda veritabanı hizmetinize bağlanmak için SSL zorunluluğunu devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Azure veritabanı giriş MySQL için okuma ve "Ne olduğu Azure veritabanı için MySQL?" sorusunu yanıtladığınıza göre şunları yapmaya hazırsınız:
- Maliyet karşılaştırmaları ve hesaplayıcıları için fiyatlandırma sayfasını bakın. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/mysql/)
- İlk sunucunuzun oluşturarak başlayın. [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md)
- Tercih ettiğiniz dili kullanarak ilk uygulamanızı oluşturma: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md)  |  [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [gidin](./connect-go.md)
