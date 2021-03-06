---
title: Azure Logic Apps için Bağlayıcılar | Microsoft Docs
description: Mantıksal uygulamalar derlemek ve oluşturmak için kullanılabilir tüm Microsoft bağlayıcıları arasından seçim yapın
services: logic-apps
documentationcenter: ''
author: ecfan
manager: anneta
editor: ''
tags: connectors
ms.assetid: f1f1fd50-b7f9-4d13-824a-39678619aa7a
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: estfan; ladocs
ms.openlocfilehash: a17d887e829252231e0f2e0bac137bd63a24e0d9
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="connectors-list"></a>Bağlayıcıların listesi
Bağlayıcıların Swagger açıklaması tarafından tanımlanan tetikleyicilerin ve eylemlerin yanı sıra bağlayıcı sınırlarını bulmak için bkz. [Bağlayıcı ayrıntıları](/connectors/).

Bağlayıcılar, mantıksal uygulama oluşturma işleminin ayrılmaz bir parçasıdır. Bu bağlayıcıları kullanarak, oluşturduğunuz veriler ve zaten sahip olduğunuz veriler ile farklı işlemler yapmak üzere şirket içi ve bulut uygulamalarınızı genişletebilirsiniz. Bağlayıcılar, yerleşik eylemler veya yönetilen bağlayıcılar olarak kullanılabilir.

**Yerleşik eylemler**: Logic Apps altyapısı, uç noktalara iletmek ve görevleri gerçekleştirmek için kullanılan yerleşik eylemlere sahiptir. Örneğin bu eylemleri HTTP uç noktalarını, Azure İşlevleri'ni ve Azure API Management işlemlerini çağırmanın yanı sıra iletileri veri işlemleri ve değişkenleriyle değiştirmek için kullanabilirsiniz.

**Yönetilen bağlayıcılar**: Logic Apps hizmetinin barındırdığı ve yönettiği API bağlantıları oluşturarak farklı hizmetler için API erişimi sunar. Yönetilen bağlayıcılar şu kategorilere ayrılır:

* **Standart bağlayıcılar**: Mantıksal uygulamaları kullandığınızda otomatik olarak kullanılabilir durumdadır ve eklenir. Service Bus, Power BI, OneDrive ve daha birçok örnek mevcuttur.

* **Şirket içi bağlayıcılar**: [Şirket içi veri ağ geçidi][gatewaydoc] kullanarak şirket içi sunucu uygulamalarına bağlanmanızı sağlar. Şirket içi bağlayıcılar SharePoint Server, SQL Server, Oracle DB, dosya paylaşımları ve diğerleri gibi sunucu uygulamalarına bağlantı sunar.

* **Tümleştirme hesabı bağlayıcıları**: Bir tümleştirme hesabı satın aldığınızda kullanılabilir. Bu bağlayıcıları kullanarak, XML dönüştürüp doğrulayabilir, AS2 / X12 / EDIFACT ile işletmeden işletmeye iletileri işleyebilir ve düz dosyaları kodlayıp kod çözebilirsiniz. BizTalk Server ile çalışıyorsanız, bu bağlayıcılar BizTalk iş akışlarınızı Azure'a genişletmek için uygundur.  

    BizTalk Server ayrıca bir mantıksal uygulamadan alma ve mantıksal uygulama gönderme işlemini içeren bir [Logic Apps bağdaştırıcısına](https://msdn.microsoft.com/library/mt787163.aspx) sahiptir.

* **Enterprise bağlayıcıları**: MQ ve SAP içerir. Ek bir maliyet karşılığında sunulur. 

Maliyetler hakkında daha fazla bilgi için bkz. Logic Apps için [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/logic-apps/) ve [fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md). 

## <a name="popular-connectors"></a>Popüler bağlayıcılar
Bu bağlayıcıları kullanarak veri ve bilgileri başarılı bir şekilde işleyen binlerce uygulama ve milyonlarca yürütme vardır. 

### <a name="built-in-actions"></a>Yerleşik eylemler
Logic Apps altyapısı verileri değiştirebilen, HTTP üzerinden iletişim kurabilen ve mantıksal uygulama tanımının akışını denetleyebilen eylemler sunar. Bu eylemlerin bazıları şunlardır:

| |  |  |  |
| --- | --- | --- | --- |
| [![API Simgesi][HTTPicon]<br/>**HTTP**][httpdoc] | HTTP üzerinden herhangi bir uç nokta ile iletişim kurmak için mantıksal uygulamaları kullanın.| [![API Simgesi][Azure-Functionsicon]<br/>**Azure İşlevleri**][azure-functionsdoc] | Özel C# veya node.js kod parçacıkları çalıştıran işlevler oluşturun ve sonra bu işlevleri mantıksal uygulamalarınızda kullanın.  |
| [![API Simgesi][HTTP-Requesticon]<br/>**İstek**][HTTP-Requestdoc] | Genelde diğer uygulamalarda web kancası olarak kullanılan çağrılabilir bir HTTPS URL'si sunar. Mantıksal uygulama bu URL’ye yönelik bir istek aldığında mantıksal uygulama başlatılır. | [![API Simgesi][Recurrenceicon]<br/>**Zamanlama**][recurrencedoc] | Basit veya karmaşık yinelenme zamanlamalarını temel alan mantıksal uygulamalar oluşturun. Örneğin her gün yinelenen gibi basit veya her ayın son Cuma günü saat 09:00 ile 17:00 arasında saat başı yinelenen gibi karmaşık zamanlamalar oluşturabilirsiniz. |
| [![API Simgesi][CallLogicApp-icon]<br/>**Çağrı<br/>Mantıksal Uygulama**][nested-logic-appdoc] | İç içe geçmiş mantıksal uygulamayı çağırın. İstek tetikleyicisine sahip tüm mantıksal uygulamalar, iç içe geçmiş mantıksal uygulama olarak çağrılabilir.| [![API Simgesi][API/Web-Appicon]<br/>**API Apps**][api/web-appdoc] | App Service API Apps'ı çağırın. Swagger'a sahip API Apps, diğer birinci sınıf eylemler gibi oluşturulur.|

### <a name="standard-connectors"></a>Standart bağlayıcılar
Aşağıdaki tabloda en popüler olanları ve kullanıcılarımızın en sık kullandığı bazı örnekler listelenmektedir:

| |  |  |  |
| --- | --- | --- | --- |
| [![API Simgesi][AzureBlobStorageicon]<br/>**Azure Blob<br/>Depolama**][AzureBlobStoragedoc] | Depolama hesabınızda herhangi bir görevi otomatik hale getirmek isterseniz bu bağlayıcıya bakmanız gerekir. CRUD (create, read, update, delete) işlemlerini destekler. | [![API Simgesi][Dynamics-365icon]<br/>**Dynamics 365<br/>CRM Online**][Dynamics-365doc] | En çok talep gören bağlayıcılardan biridir. Müşteri adayları ile iş akışlarını otomatikleştirmeye ve daha birçok işleme yardımcı olan tetikleyici ve eylemler içerir. |
| [![API Simgesi][Event-Hubs-icon]<br/>**Event Hubs**][event-hubs-doc] | Bir Olay Hub’ındaki olayları tüketin ve yayımlayın. Örneğin, Event Hubs'ı kullanarak mantıksal uygulamanızdan çıkış alabilir ve ardından söz konusu çıkışı gerçek zamanlı bir analiz sağlayıcısına gönderebilirsiniz. | [![API Simgesi][FTPicon]<br/>**FTP**][FTPdoc] | FTP sunucunuza İnternet'ten erişilebiliyorsa, iş akışlarını dosya ve klasörlerle çalışacak şekilde otomatikleştirebilirsiniz. <br/><br/>SFTP ayrıca SFTP bağlayıcısı ile kullanılabilir. |
| [![API Simgesi][Office-365-Outlookicon]<br/>**Office 365<br/>Outlook**][office365-outlookdoc] | İş akışlarınızda Office 365 e-posta ve olaylarını kullanmaya yönelik çok sayıda tetikleyici ve çok daha fazla sayıda eylem. <br/><br/>Bu bağlayıcı, tatil isteklerini, gider raporlarını, vb. onaylamaya yönelik bir *onay e-postası* eylemi içerir. <br/><br/>Office 365 kullanıcıları ayrıca Office 365 Kullanıcıları bağlayıcısı ile kullanılabilir.| [![API Simgesi][Salesforceicon]<br/>**Salesforce**][salesforcedoc] | Müşteri Adayları gibi nesnelere ve daha fazlasına erişmek için Salesforce hesabınızla kolayca oturum açın. | 
| [![API Simgesi][Service-Busicon]<br/>**Service Bus**][Service-Busdoc] | Mantıksal uygulamalardaki en popüler bağlayıcı olmasının yanı sıra, zaman uyumsuz mesajlaşma ve kuyruklar, abonelikler ve konular ile yayımlama/abone olma işlemlerine yönelik tetikleyiciler ve eylemler içerir. |  [![API Simgesi][SharePointicon]<br/>**SharePoint<br/>Online**][SharePointdoc] | SharePoint ile bir işlem yapıyor ve otomasyondan yararlanabiliyorsanız, bu bağlayıcıya bakmanız önerilir. Şirket içi SharePoint ve SharePoint Online ile birlikte kullanılabilir. |
| [![API Simgesi][SQL-Servericon]<br/>**SQL Server**][SQL-Serverdoc] | En çok kullanılan bağlayıcılardan biri olmasının yanı sıra, şirket içi SQL Server ve bir Azure SQL Veritabanına bağlanabilir. | [![API Simgesi][Twittericon]<br/>**Twitter**][Twitterdoc] | Bir Twitter hesabıyla kolayca oturum açın ve yeni tweet gönderildiğinde bir iş akışı başlatın. Daha sonra bu tweetleri bir SQL veritabanı veya SharePoint listesine kaydedin. | 

### <a name="on-premises-connectors"></a>Şirket içi bağlayıcılar 

Şirket içi bağlayıcılar, şirket içi sunuculardaki verilere erişim sağlar.  Şirket içi sunucularla bağlantı kurulabilmesi için ağ altyapısını yapılandırmaya gerek kalmadan güvenli bir iletişim kanalı sağlayan [şirket içi veri ağ geçidi][gatewaydoc] gerekir.  Bağlayıcılardan bazıları şunlardır:

|  |  |  |  |
| --- | --- | --- | --- |
| [![API Simgesi][db2icon]<br/>**DB2**][db2doc] | [![API Simgesi][oracle-DB-icon]<br/>**Oracle DB**][oracle-db-doc] | [![API Simgesi][sharepointicon]<br/>**SharePoint</br> Server**][sharepointserver] | [![API Simgesi][filesystem-icon]<br/>**Dosya</br> Sistemi**][filesystemdoc] |
[![API Simgesi][sql-servericon]<br/>**SQL</br> Server**][sql-serverdoc] | ![API Icon][Biztalk-Servericon]<br/>**BizTalk</br> Server**| |

### <a name="integration-account-connectors"></a>Tümleştirme hesabı bağlayıcıları 

Enterprise Integration Pack (EIP), BizTalk Server topluluğu tarafından iyi bilinen bağlayıcılar içerir. Bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) satın aldığınızda aşağıdaki bağlayıcıları da elde edersiniz: 

|  |  |  |  |
| --- | --- | --- | --- |
| [![API Simgesi][as2icon]<br/>**AS2</br> kodunu çözme**][as2decode] | [![API Simgesi][as2icon]<br/>**AS2</br> kodlama**][as2encode] | [![API Simgesi][x12icon]<br/>**EDIFACT</br> kodunu çözme**][EDIFACTdecode] | [![API Simgesi][x12icon]<br/>**EDIFACT</br> kodlama**][EDIFACTencode] |
[![API Simgesi][flatfileicon]<br/>**Düz dosya</br> kodlama**][flatfiledoc] | [![API Simgesi][flatfiledecodeicon]<br/>**Düz dosya</br> kodunu çözme**][flatfiledecodedoc] | [![API Simgesi][integrationaccounticon]<br/>**Tümleştirme<br/>hesabı**][integrationaccountdoc] | [![API Simgesi][xmltransformicon]<br/>**Dönüştürme<br/>XML**][xmltransformdoc] |
| [![API Simgesi][x12icon]<br/>**X12</br> kodunu çözme**][x12decode] | [![API Simgesi][x12icon]<br/>**X12</br> kodlama**][x12encode] | [![API Simgesi][xmlvalidateicon]<br/>**XML <br/>doğrulama**][xmlvalidatedoc] | [![API Icon][liquidicon]<br/>**Transform <br/>JSON**][JSONliquidtransformdoc] |

### <a name="enterprise-connectors"></a>Kurumsal bağlayıcılar

Mantıksal uygulamalarınızın içinden kurumsal uygulamalarınıza bağlanın.

|  |  |
| --- | --- |
|[![API Simgesi][MQicon]<br/>**MQ**][mqdoc]|[![API Simgesi][SAPicon]<br/>**SAP**][sapconnector]|

> [!TIP]
> Azure hesabına kaydolmadan Azure Logic Apps’i kullanmaya başlamak için [Logic Apps’i Deneyin](https://tryappservice.azure.com/?appservice=logic) sayfasına gidin. Başlangıç düzeyinde kısa süreli mantıksal uygulamayı hemen oluşturabilirsiniz. Kredi kartı ve taahhüt gerekmez.

## <a name="connectors-as-triggers-and-actions"></a>Tetikleyici ve eylem olarak bağlayıcılar

**Tetikleyici**, mantıksal uygulamanızın bir örneğini başlatır veya çalıştırır. Bazı bağlayıcılar, belirli olaylar meydana geldiğinde uygulamanızı bilgilendiren tetikleyiciler sağlar. Örneğin, FTP bağlayıcısı bir dosya güncelleştirildiğinde mantıksal uygulamanızı başlatan `OnUpdatedFile` tetikleyicisine sahiptir. 

Mantıksal uygulamalar aşağıdaki tetikleyici türlerini içerir:  

* *Yoklama tetikleyicileri*: Bu tetikleyiciler yeni verileri denetlemek için hizmetinizi belirtilen aralıkta yoklar. 

    Yeni veriler kullanılabilir olduğunda, mantıksal uygulamanızın yeni bir örneği girdi olarak verilerle çalışır. 

* *Anında iletme tetikleyicileri*: Bu tetikleyiciler, uç noktada bir olayın meydana gelmesine ilişkin verileri dinler ve ardından mantıksal uygulamanızın yeni bir örneğini tetikler.

* *Yineleme tetikleyicisi*: Bu tetikleyici, önceden belirlenmiş bir zamanlamaya göre mantıksal uygulamanızın bir örneğini başlatır.

Bağlayıcılar ayrıca iş akışınızda kullanabileceğiniz **eylemler** sağlar. Örneğin, mantıksal uygulamanız verileri arayabilir ve bu verileri daha sonra mantıksal uygulamanızda kullanabilir. Daha ayrıntılı olarak, bir SQL veritabanından müşteri verilerini arayabilir ve sonra iş akışınızı oluşturmak için bu müşteri verilerinizi kullanabilirsiniz. 

> [!TIP]
> [Bağlayıcılara genel bakış](connectors-overview.md) bölümünde tetikleyici ve eylemler hakkında daha fazla bilgi verilmektedir. 


## <a name="message-manipulation-actions"></a>İleti işleme eylemleri

Mantıksal uygulamalar, yük verilerinizi değiştirebilen veya işleyebilen yerleşik eylemler içerir. Yerleşik **Veri İşlemleri** bağlayıcısı aşağıdaki eylemleri içerir: 

| | |
|---|---|
| **Oluştur** | Daha sonra veya iş akışınızı derlerken kullanabileceğiniz değerler ya da nesneler derleyin veya oluşturun. Örneğin, birden çok adımın değerleriyle bir JSON nesnesi yazabilir veya daha sonra bir mantıksal uygulama çalıştırmasında başvurmak üzere sabit değer hesaplayabilirsiniz. |
| **CSV tablosu oluşturma**<br/>**HTML tablosu oluşturma** | Dizi sonuç kümesini bir CSV veya HTML tablosuna dönüştürün. Örneğin, CRM "Liste kayıtları" eylemini ekleyin ve bugün eklenen kayıtlar için bir filtre ekleyin. Sonra sonuçları bir e-posta ile HTML tablosu halinde gönderin. |
| **Filtre dizisi** (sorgu) | Bir sonuç kümesini, sizi ilgilendiren girişlerle filtreleyin. Örneğin, `#Azure` içeren tüm tweetleri arayın ve sonra yalnızca `Tweeted_by_followers > 50` olan sonuçları döndürmek için döndürülen tweetleri “filtreleyin”. |
| **Birleştir** | Bir diziyi bazı sınırlayıcılarla birleştirin. Örneğin, Anahtar Tümcecikleri Algıla işlemi, bir anahtar tümcecik dizisi döndürür. Bu tümcecikleri bir `,` veya benzer bir şey ile “birleştirebilirsiniz”. Bu nedenle, `["Some", "Phrase"]` yerine `"Some, Phrase"` elde edersiniz. |
| **JSON Ayrıştırma** | Tasarımcıda bir JSON nesnesinden değerleri ayrıştırabilir ve değerlere erişebilirsiniz. Örneğin, Azure İşleviniz bir JSON yükü döndürürse, bu yükü başka bir adımda JSON özelliklerine erişmek için ayrıştırabilirsiniz. Eylem ayrıca JSON yükünün çalışma zamanında belirtilen şema ile eşleştiğini doğrular. | 
| **Seç** | Daha fazla işleme için bir dizinin belirli özelliklerini seçin. SQL'de "Kayıtları listeleme" seçeneğini belirlerseniz ve 15 sütun döndürülürse daha fazla işleme için bu sütunların yalnızca birkaçını seçin. Çıkış, yalnızca seçtiğiniz özellikleri içeren bir dizidir. |

## <a name="custom-connectors-and-azure-certification"></a>Özel bağlayıcılar ve Azure sertifikaları 

Özel kod çalıştıran veya bağlayıcı olarak kullanılamayan API'leri çağırmak için REST tabanlı API Apps oluşturarak [Logic Apps platformunu genişletebilirsiniz](../logic-apps/logic-apps-create-api-app.md). Ayrıca aboneliğinizdeki tüm mantıksal uygulamalar tarafından kullanılabilecek kendi [özel bağlayıcılarınızı](../logic-apps/custom-connector-overview.md) da oluşturabilirsiniz.

Özel API Apps bileşenlerinizi genel kullanıma sunmak ve Azure'da kullanılabilir hale getirmek isterseniz, [bağlayıcılarınızı Microsoft sertifikası almak üzere gönderebilirsiniz](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-help"></a>Yardım alın

Sorular sormak, soruları yanıtlamak ve diğer Azure Logic Apps kullanıcılarının neler yaptığını görmek için [Azure Logic Apps forumuna](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) gidin.

Azure Logic Apps ve bağlayıcıları geliştirmeye yardımcı olmak için, [Logic Apps kullanıcı geri bildirim sitesinde](http://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın.

Bağlayıcılarla ilgili olarak değinmediğimiz bir konu başlığı veya önemli olduğunu düşündüğünüz herhangi bir ayrıntı var mı? Yanıtınız evet ise mevcut konu başlıklarımıza ekleme yaparak veya kendi konu başlığınızı oluşturarak bize yardımcı olabilirsiniz. Belgelerimiz açık kaynak olup GitHub'da barındırılır. Başlamak için [GitHub depomuza](https://github.com/Microsoft/azure-docs) gidin. 

## <a name="next-steps"></a>Sonraki adımlar
* [İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Mantıksal uygulamalar için özel API’ler oluşturma](../logic-apps/logic-apps-create-api-app.md)
* [Mantıksal uygulamalarınızı izleyin](../logic-apps/logic-apps-monitor-your-logic-apps.md)

<!--Connectors Documentation-->

[gatewaydoc]: ../logic-apps/logic-apps-gateway-connection.md "Şirket içi veri ağ geçidiyle mantıksal uygulamalardan şirket içi veri kaynaklarına bağlanın"
[api/web-appdoc]: ../logic-apps/logic-apps-custom-hosted-api.md "Mantıksal uygulamaları App Service API Apps ile tümleştirin"
[azureblobstoragedoc]: ./connectors-create-api-azureblobstorage.md "Blob kapsayıcınızdaki dosyaları Azure blob depolama bağlayıcısı ile yönetin"
[azure-functionsdoc]: ../logic-apps/logic-apps-azure-functions.md "Mantıksal uygulamaları Azure İşlevleri ile tümleştirin"
[db2doc]: ./connectors-create-api-db2.md "Bulutta veya şirket içinde IBM DB2’ye bağlanın. Bir satırı güncelleştirin, bir tabloyu alın ve diğer işlemleri yapın"
[Dynamics-365doc]: ./connectors-create-api-crmonline.md "CRM Online verileriyle çalışabilmek için Dynamics CRM Online’a bağlanın"
[event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Azure Event Hubs’a bağlanma. Logic Apps ile Event Hubs arasında olay gönderip alma"
[filesystemdoc]: ../logic-apps/logic-apps-using-file-connector.md "Şirket içi dosya sistemine bağlanın"
[ftpdoc]: ./connectors-create-api-ftp.md "Dosyaları karşıya yükleme, alma, silme ve diğer FTP görevleri için bir FTP / FTPS sunucusuna bağlanın"
[httpdoc]: ./connectors-native-http.md "HTTP bağlayıcısı ile HTTP aramaları yapın"
[http-requestdoc]: ./connectors-native-reqres.md "HTTP istekleri ve mantıksal uygulama yanıtları için eylemler ekleyin"
[http-swaggerdoc]: ./connectors-native-http-swagger.md "HTTP + Swagger bağlayıcısı ile HTTP çağrıları yapın"
[informixdoc]: ./connectors-create-api-informix.md "Bulutta veya şirket içinde Informix’e bağlanın. Bir satırı okuyun, tabloları listeleyin ve daha fazlasını yapın"
[nested-logic-appdoc]: ../logic-apps/logic-apps-http-endpoint.md "Mantıksal uygulamaları iç içe geçmiş iş akışları ile tümleştirin"
[office365-outlookdoc]: ./connectors-create-api-office365-outlook.md "Office 365 hesabınıza bağlanın. E-posta gönderip alın, takvim ve kişilerinizi yönetin ve daha fazlasını yapın"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Bir Oracle veritabanına bağlanarak satır ekleme, silme ve daha fazlası"
[mqdoc]: ./connectors-create-api-mq.md "Şirket içi MQ veya Azure bağlantısı gerçekleştirin ve ileti gönderip alın"
[recurrencedoc]:  ./connectors-native-recurrence.md "Mantıksal uygulamalar için yinelenen eylemleri tetikleyin"
[salesforcedoc]: ./connectors-create-api-salesforce.md "Salesforce hesabınıza bağlanın. Hesapları, müşteri adaylarını, fırsatları ve daha fazlasını yönetin"
[sapconnector]: ../logic-apps/logic-apps-using-sap-connector.md "Şirket içi SAP sistemine bağlanın"
[service-busdoc]: ./connectors-create-api-servicebus.md "Service Bus Kuyruklarından ve Konu Başlıklarından iletiler gönderin ve Service Bus Kuyrukları ile Aboneliklerinden iletiler alın"
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "SharePoint Online’a bağlanın. Belgeleri yönetin, öğeleri listeleyin ve daha fazlasını yapın"
[sharepointserver]: ./connectors-create-api-sharepointserver.md "Şirket içi SharePoint sunucusuna bağlanın. Belgeleri yönetin, öğeleri listeleyin ve daha fazlasını yapın"
[sql-serverdoc]: ./connectors-create-api-sqlazure.md "Azure SQL Veritabanı veya SQL sunucusuna bağlanın. SQL veritabanı tablosunda girdiler oluşturun, güncelleştirin, alın ve silin."
[twitterdoc]: ./connectors-create-api-twitter.md "Twitter’a bağlanın. Zaman çizelgelerini alın, tweetler gönderin ve daha fazlasını yapın"
[webhookdoc]: ./connectors-native-webhook.md "Mantıksal uygulamalarınıza Web kancası eylemleri ve tetikleyiciler ekleyin"

[as2doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Enterprise integration AS2 hakkında bilgi edinin."
[x12doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Enterprise integration X12 hakkında bilgi edinin."
[flatfiledoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Enterprise integration düz dosyası hakkında bilgi edinin."
[flatfiledecodedoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Enterprise integration düz dosyası hakkında bilgi edinin."
[xmlvalidatedoc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Enterprise integration XML doğrulaması hakkında bilgi edinin."
[xmltransformdoc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Enterprise integration dönüştürmeleri hakkında bilgi edinin."
[as2decode]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Enterprise integration AS2 kod çözme hakkında bilgi edinin"
[as2encode]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Enterprise integration AS2 kodlama hakkında bilgi edinin"
[X12decode]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Enterprise integration X12 kod çözme hakkında bilgi edinin"
[X12encode]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Enterprise integration X12 kodlama hakkında bilgi edinin"
[EDIFACTdecode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Enterprise integration EDIFACT kod çözme hakkında bilgi edinin"
[EDIFACTencode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Enterprise integration EDIFACT kodlama hakkında bilgi edinin"
[integrationaccountdoc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Tümleştirme hesabınızda şemalar, haritalar, iş ortakları ve daha fazlasını arayın"
[JSONliquidtransformdoc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Liquid ile JSON dönüştürmeleri hakkında bilgi edinin"


[boxDoc]: ./connectors-create-api-box.md "Box’a bağlanın. Dosyalarınızı karşıya yükleyin, silin, listeleyin ve diğer işlemleri yapın"
[delaydoc]: ./connectors-native-delay.md "Gecikmeli eylemleri gerçekleştirin"
[dropboxdoc]: ./connectors-create-api-dropbox.md "Dropbox'a bağlanın. Dosyalarınızı karşıya yükleyin, silin, listeleyin ve diğer işlemleri yapın"
[facebookdoc]: ./connectors-create-api-facebook.md "Facebook’a bağlanın. Zaman tünelinde gönderi yapın, sayfa akışı alın ve daha fazlasını yapın"
[githubdoc]: ./connectors-create-api-github.md "GitHub’a bağlanın ve sorunları izleyin"
[google-drivedoc]: ./connectors-create-api-googledrive.md "Verilerinizle çalışabilmek için GoogleDrive’a bağlanın"
[google-sheetsdoc]: ./connectors-create-api-googlesheet.md "Elektronik tablolarınızı değiştirebilmek için Google E-Tablolar’a bağlanın"
[google-tasksdoc]: ./connectors-create-api-googletasks.md "Görevlerinizi yönetebilmek için Google Görevler’e bağlanın"
[google-calendardoc]: ./connectors-create-api-googlecalendar.md "Google Takvime bağlanır ve takvimi yönetebilir."
[http-responsedoc]: ./connectors-native-reqres.md "HTTP istekleri ve mantıksal uygulama yanıtları için eylemler ekleyin"
[instagramdoc]: ./connectors-create-api-instagram.md "Instagram’a bağlanın. Olayları tetikleyin veya üzerinde işlem yapın"
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "MailChimp hesabınıza bağlanın. Postaları yönetin ve otomatikleştirin"
[mandrilldoc]: ./connectors-create-api-mandrill.md "İletişim için Mandrill’e bağlanın"
[microsoft-translatordoc]: ./connectors-create-api-microsofttranslator.md "Microsoft Translator’a bağlanın. Metinleri çevirin, dilleri algılayın ve daha fazlasını yapın" 
[office365-usersdoc]: ./connectors-create-api-office365-users.md 
[office365-videodoc]: ./connectors-create-api-office365-video.md "Office 365 için video bilgileri, video listeleri ile kanallar ve kayıttan yürütme URL'lerini alın"
[onedrivedoc]: ./connectors-create-api-onedrive.md "Kişisel Microsoft OneDrive’ınıza bağlanın. Dosyaları karşıya yükleyin, silin, listeleyin ve daha fazlasını yapın"
[onedrive-for-businessdoc]: ./connectors-create-api-onedriveforbusiness.md "İş için Microsoft OneDrive’ınıza bağlanın. Dosyalarınızı karşıya yükleyin, silin, listeleyin ve diğer işlemleri yapın"
[outlook.comdoc]: ./connectors-create-api-outlook.md "Outlook posta kutunuza bağlanın. E-posta, takvim, kişi ve diğerlerini yönetin"
[project-onlinedoc]: ./connectors-create-api-projectonline.md "Microsoft Project Online’a bağlanın. Proje, görev, kaynaklar ve daha fazlasını yönetin"
[querydoc]: ./connectors-native-query.md "Sorgu eylemi ile dizileri seçip filtreleyin"
[rssdoc]: ./connectors-create-api-rss.md "Akış öğeleri yayımlayıp alın, RSS akışında yeni bir öğe yayımlandığında işlemleri tetikleyin."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "SendGrid’e bağlanın. E-posta gönderin ve alıcı listelerini yönetin"
[sftpdoc]: ./connectors-create-api-sftp.md "SFTP hesabınıza bağlanın. Dosyalarınızı karşıya yükleyin, alın, silin ve diğer işlemleri yapın"
[slackdoc]: ./connectors-create-api-slack.md "Slack’e bağlanın ve Slack kanallarında iletiler yayınlayın"
[smtpdoc]: ./connectors-create-api-smtp.md "SMTP sunucusuna bağlanın ve ekler içeren e-postalar gönderin"
[sparkpostdoc]: ./connectors-create-api-sparkpost.md "İletişim için SparkPost’a bağlanın"
[trellodoc]: ./connectors-create-api-trello.md "Trello’ya bağlanın. Projelerinizi yönetin ve herhangi bir şeyi herhangi bir kimseyle düzenleyin"
[twiliodoc]: ./connectors-create-api-twilio.md "Twilio’ya bağlanın. İletiler gönderip alın, mevcut numaraları alın, gelen telefon numaralarını yönetin ve daha fazlasını yapın"
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Wunderlist’e bağlanın. Görevlerinizi ve yapılacaklar listenizi yönetin, tüm işlerinizi eşitleyin ve daha fazlasını yapın"
[yammerdoc]: ./connectors-create-api-yammer.md "Yammer’a bağlanın. İleti gönderin, yeni iletiler alın ve daha fazlasını yapın"
[youtubedoc]: ./connectors-create-api-youtube.md "YouTube’a bağlanın. Videolarınızı ve kanallarınızı yönetin"


<!--Icon references-->
[appFiguresicon]: ./media/apis-list/appfigures.png
[AppServices-icon]: ./media/apis-list/AppServices.png
[Asanaicon]: ./media/apis-list/asana.png
[Azure-Automation-icon]: ./media/apis-list/azure-automation.png
[AzureBlobStorageicon]: ./media/apis-list/azureblob.png
[Azure-Data-Lake-icon]: ./media/apis-list/azure-data-lake.png
[Azure-MLicon]: ./media/apis-list/azureml.png
[Azure-Resource-Manager-icon]: ./media/apis-list/azure-resource-manager.png
[Azure-Queues-icon]: ./media/apis-list/azure-queues.png
[Basecamp-3icon]: ./media/apis-list/basecamp.png
[Bitbucket-icon]: ./media/apis-list/bitbucket.png
[Bitlyicon]: ./media/apis-list/bitly.png
[BizTalk-Servericon]: ./media/apis-list/biztalk.png
[Bloggericon]: ./media/apis-list/blogger.png
[Boxicon]: ./media/apis-list/box.png
[Campfireicon]: ./media/apis-list/campfire.png
[Cognitive-Services-Text-Analyticsicon]: ./media/apis-list/cognitiveservicestextanalytics.png
[DB2icon]: ./media/apis-list/db2.png
[Dropboxicon]: ./media/apis-list/dropbox.png
[Dynamics-365icon]: ./media/apis-list/dynamicscrmonline.png
[Dynamics-365-for-Financialsicon]: ./media/apis-list/madeira.png
[Dynamics-365-for-Operationsicon]: ./media/apis-list/dynamicsax.png
[Easy-Redmineicon]: ./media/apis-list/easyredmine.png
[Event-Hubs-icon]: ./media/apis-list/eventhubs.png
[Facebookicon]: ./media/apis-list/facebook.png
[FileSystem-icon]: ./media/apis-list/filesystem.png
[FileSystemIcon]: ./media/apis-list/filesystem.png
[FTPicon]: ./media/apis-list/ftp.png
[GitHubicon]: ./media/apis-list/github.png
[Google-Calendaricon]: ./media/apis-list/googlecalendar.png
[Google-Driveicon]: ./media/apis-list/googledrive.png
[Google-Sheetsicon]: ./media/apis-list/googlesheet.png
[Google-Tasksicon]: ./media/apis-list/googletasks.png
[HideKeyicon]: ./media/apis-list/hidekey.png
[HipChaticon]: ./media/apis-list/hipchat.png
[Informixicon]: ./media/apis-list/informix.png
[Insightlyicon]: ./media/apis-list/insightly.png
[Instagramicon]: ./media/apis-list/instagram.png
[Instapapericon]: ./media/apis-list/instapaper.png
[JIRAicon]: ./media/apis-list/jira.png
[MailChimpicon]: ./media/apis-list/mailchimp.png
[Mandrillicon]: ./media/apis-list/mandrill.png
[Microsoft-Translatoricon]: ./media/apis-list/microsofttranslator.png
[MQicon]: ./media/apis-list/mq.png
[Office-365-Outlookicon]: ./media/apis-list/office365.png
[Office-365-Usersicon]: ./media/apis-list/office365users.png
[Office-365-Videoicon]: ./media/apis-list/office365video.png
[OneDriveicon]: ./media/apis-list/onedrive.png
[OneDrive-for-Businessicon]: ./media/apis-list/onedriveforbusiness.png
[Oracle-DB-icon]: ./media/apis-list/oracle-db.png
[Outlook.comicon]: ./media/apis-list/outlook.png
[PagerDutyicon]: ./media/apis-list/pagerduty.png
[Pinteresticon]: ./media/apis-list/pinterest.png
[Project-Onlineicon]: ./media/apis-list/projectonline.png
[Redmineicon]: ./media/apis-list/redmine.png
[RSSicon]: ./media/apis-list/rss.png
[Common-Data-Serviceicon]: ./media/apis-list/runtimeservice.png
[Salesforceicon]: ./media/apis-list/salesforce.png
[SAPicon]: ./media/apis-list/sap.png
[SendGridicon]: ./media/apis-list/sendgrid.png
[Service-Busicon]: ./media/apis-list/servicebus.png
[SFTPicon]: ./media/apis-list/sftp.png
[SharePointicon]: ./media/apis-list/sharepointonline.png
[Slackicon]: ./media/apis-list/slack.png
[Smartsheeticon]: ./media/apis-list/smartsheet.png
[SMTPicon]: ./media/apis-list/smtp.png
[SparkPosticon]: ./media/apis-list/sparkpost.png
[SQL-Servericon]: ./media/apis-list/sql.png
[Todoisticon]: ./media/apis-list/todoist.png
[Trelloicon]: ./media/apis-list/trello.png
[Twilioicon]: ./media/apis-list/twilio.png
[Twittericon]: ./media/apis-list/twitter.png
[Vimeoicon]: ./media/apis-list/vimeo.png
[Visual-Studio-Team-Servicesicon]: ./media/apis-list/visualstudioteamservices.png
[WordPressicon]: ./media/apis-list/wordpress.png
[Wunderlisticon]: ./media/apis-list/wunderlist.png
[Yammericon]: ./media/apis-list/yammer.png
[YouTubeicon]: ./media/apis-list/youtube.png

<!-- Primitive Icons -->
[API/Web-Appicon]: ./media/apis-list/appservices.png
[Azure-Functionsicon]: ./media/apis-list/function.png
[CallLogicApp-icon]: ./media/apis-list/calllogicapp.png
[Delayicon]: ./media/apis-list/delay.png
[HTTPicon]: ./media/apis-list/http.png
[HTTP-Requesticon]: ./media/apis-list/request.png
[HTTP-Responseicon]: ./media/apis-list/response.png
[HTTP-Swaggericon]: ./media/apis-list/http_swagger.png
[Nested-Logic-Appicon]: ./media/apis-list/workflow.png
[Recurrenceicon]: ./media/apis-list/recurrence.png
[Queryicon]: ./media/apis-list/query.png
[Webhookicon]: ./media/apis-list/webhook.png

<!-- EIP Icons -->
[as2icon]: ./media/apis-list/as2.png
[x12icon]: ./media/apis-list/x12new.png
[flatfileicon]: ./media/apis-list/flatfileencoding.png
[flatfiledecodeicon]: ./media/apis-list/flatfiledecoding.png
[xmlvalidateicon]: ./media/apis-list/xmlvalidation.png
[xmltransformicon]: ./media/apis-list/xsltransform.png
[integrationaccounticon]: ./media/apis-list/integrationaccount.png
[liquidicon]: ./media/apis-list/liquidtransform.png
