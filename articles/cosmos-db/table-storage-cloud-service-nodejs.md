---
title: 'Azure Table storage: bir Node.js web uygulaması oluşturma | Microsoft Docs'
description: Azure Storage Hizmetleri ve Azure modülü ekleyerek Express öğretici Web uygulamasıyla derlemeler Öğreticisi.
services: cosmos-db
documentationcenter: nodejs
author: SnehaGunda
manager: kfile
ms.assetid: e90959a2-4cb2-4b19-9bfb-aede15b18b1c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 03/29/2018
ms.author: sngun
ms.openlocfilehash: 6608c9a40c017709be4a482c7825729f8d292715
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-table-storage-nodejs-web-application"></a>Azure Table storage: Node.js Web uygulaması
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, uygulamayı oluşturduğunuz [Express kullanarak Node.js Web uygulaması] Öğreticisi, Node.js için Microsoft Azure istemci kitaplıkları veri Yönetim Hizmetleri ile birlikte çalışmak üzere kullanımı genişletilir. Uygulamanız için Azure dağıtabileceğiniz bir web tabanlı görev listesi uygulama oluşturarak genişletir. Görev listesi, kullanıcının görevleri almak, yeni görevler ekleyin ve Görevler tamamlandı olarak işaretle izin verir.

Görev öğeleri Azure Storage veya Azure Cosmos DB depolanır. Azure depolama ve Azure Cosmos DB hataya dayanıklı ve yüksek oranda kullanılabilir yapılandırılmamış veri depolama alanı sağlar. Azure depolama ve Azure Cosmos DB sakladığınız ve veri erişim birkaç veri yapılarını içerir. Depolama ve REST API'leri aracılığıyla veya Node.js için Azure SDK'ın dahil API'ları Azure Cosmos DB hizmetlerini kullanabilirsiniz. Daha fazla bilgi için bkz: [depolama ve veri erişim].

Bu öğretici, tamamladığınızı varsaymaktadır [Node.js Web uygulaması] ve [Node.js Express ile][Express kullanarak Node.js Web uygulaması] öğreticileri.

Aşağıdaki bilgileri içerir:

* Jade şablon altyapısıyla çalışma
* Azure veri Yönetim Hizmetleri ile birlikte çalışma

Aşağıdaki ekran görüntüsünde tamamlanan uygulama gösterilir:

![Internet Explorer'da tamamlanan web sayfası](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="create-an-azure-service-account"></a>Bir Azure hizmet hesabı oluşturma
[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

### <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma
[!INCLUDE [cosmos-db-create-storage-account](../../includes/cosmos-db-create-storage-account.md)]

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Bir Azure Cosmos DB tablo API hesabı oluşturma
[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

## <a name="setting-storage-credentials-in-webconfig"></a>Web.Config dosyasında depolama kimlik bilgilerini ayarlama
Azure Storage veya Azure Cosmos DB erişmek için depolama kimlik bilgilerini geçmesi gerekir. Bu, web.config uygulama ayarları yararlanarak gerçekleştirilir.
Web.config ayarları ortam değişkenleri olarak düğümü için Azure SDK'sı tarafından ardından okuma geçirilir.

> [!NOTE]
> Azure için uygulama dağıtıldığında depolama kimlik bilgileri yalnızca kullanılır. Öykünücüde çalıştırırken, uygulama depolama öykünücüsü kullanır.
>
>

Depolama hesabı bilgilerini almak ve bunları web.config ayarları eklemek için aşağıdaki adımları gerçekleştirin:

1. Zaten açık değilse, gelen Azure PowerShell'i başlatın **Başlat** genişleterek menü **tüm programları, Azure**, sağ **Azure PowerShell**ve ardından seçin **Yönetici olarak çalıştır**.
2. Dizinleri uygulamanızı içeren klasöre gidin. Örneğin, C:\\düğümü\\tasklist\\WebRole1.
3. Azure Powershell penceresinden depolama hesabı bilgilerini almak için aşağıdaki cmdlet'i girin:

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccount
    ```

   Yukarıdaki cmdlet depolama hesaplarını ve anahtarlarını barındırılan hizmetiniz ile ilişkili hesap listesini alır.

   > [!NOTE]
   > Bir hizmet dağıttığınızda Azure SDK'yı bir depolama hesabı oluşturduğundan, bir depolama hesabı zaten önceki kılavuzları uygulamanızda dağıtma var olmalıdır.
   >
   >
4. Açık **ServiceDefinition.csdef** uygulamayı Azure'a dağıtıldığında kullanılan ortam ayarlarını içeren dosyayı:

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. Altında aşağıdaki blok Ekle **ortam** {depolama hesabı} değiştirerek öğesi ve {depolama erişim tuşu} hesap adını ve dağıtım için kullanmak istediğiniz depolama hesabı için birincil anahtar:

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![Web.cloud.config dosya içerikleri](./media/table-storage-cloud-service-nodejs/node37.png)

6. Dosyayı kaydedin ve Not Defteri'ni kapatın.

### <a name="install-additional-modules"></a>Ek modüller yükleme
1. [Azure], yüklemek için aşağıdaki komutu kullanın [düğüm-UUID], [nconf] ve bunların bir girdiyi kaydetmek için de yerel olarak [async] modülleri **package.json** dosyası:

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  Bu komutun çıktısı aşağıdakine benzer görünmelidir:

  ```
  node-uuid@1.4.1 node_modules\node-uuid

  nconf@0.6.9 node_modules\nconf
  ├── ini@1.1.0
  ├── async@0.2.9
  └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

  azure-storage@0.1.0 node_modules\azure-storage
  ├── extend@1.2.1
  ├── xmlbuilder@0.4.3
  ├── mime@1.2.11
  ├── underscore@1.4.4
  ├── validator@3.1.0
  ├── node-uuid@1.4.1
  ├── xml2js@0.2.7 (sax@0.5.2)
  └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)
  ```

## <a name="using-the-table-service-in-a-node-application"></a>Tablo hizmetinde bir düğüm uygulamasında kullanma
Bu bölümde, temel uygulama tarafından oluşturulan **express** komutu ekleyerek genişletilir bir **task.js** görevleriniz için model içeren dosya. Varolan değiştirme **app.js** dosya ve yeni bir **tasklist.js** modelini kullanan dosya.

### <a name="create-the-model"></a>Modeli oluşturma
1. İçinde **WebRole1** dizin adlı yeni bir dizin oluşturun **modelleri**.
2. İçinde **modelleri** dizin adlı yeni bir dosya oluşturun **task.js**. Bu dosya, uygulamanız tarafından oluşturulan görevlerin modelini içerir.
3. Başında **task.js** dosya, gerekli kitaplıklar başvurmak için aşağıdaki kodu ekleyin:

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. Ardından, tanımlayın ve görev nesnesi dışarı aktarmak için kodu ekleyin. Görev nesnesi tabloya bağlanmak için sorumludur.

    ```nodejs
    module.exports = Task;

    function Task(storageClient, tableName, partitionKey) {
      this.storageClient = storageClient;
      this.tableName = tableName;
      this.partitionKey = partitionKey;
      this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
        if(error) {
          throw error;
        }
      });
    };
    ```

5. Ardından, ek yöntemleri Task nesnesinde tanımlamak için aşağıdaki kodu tabloda depolanan verileri ile etkileşim sağlayan ekleyin:

    ```nodejs
    Task.prototype = {
      find: function(query, callback) {
        self = this;
        self.storageClient.queryEntities(this.tablename, query, null, null, function entitiesQueried(error, result) {
          if(error) {
            callback(error);
          } else {
            callback(null, result.entries);
          }
        });
      },

      addItem: function(item, callback) {
        self = this;
        // use entityGenerator to set types
        // NOTE: RowKey must be a string type, even though
        // it contains a GUID in this example.
        var itemDescriptor = {
          PartitionKey: entityGen.String(self.partitionKey),
          RowKey: entityGen.String(uuid()),
          name: entityGen.String(item.name),
          category: entityGen.String(item.category),
          completed: entityGen.Boolean(false)
        };

        self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
          if(error){
            callback(error);
          }
          callback(null);
        });
      },

      updateItem: function(rKey, callback) {
        self = this;
        self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
          if(error) {
            callback(error);
          }
          entity.completed._ = true;
          self.storageClient.replaceEntity(self.tableName, entity, function entityUpdated(error) {
            if(error) {
              callback(error);
            }
            callback(null);
          });
        });
      }
    }
    ```

6. Kaydet ve Kapat **task.js** dosya.

### <a name="create-the-controller"></a>Denetleyiciyi oluşturma
1. İçinde **WebRole1/yollar** dizin adlı yeni bir dosya oluşturun **tasklist.js** ve bir metin düzenleyicisinde açın.
2. Aşağıdaki kodu **tasklist.js**'ye ekleyin. Bu kod tarafından kullanılan azure ve async modüllerini yükler **tasklist.js** ve tanımlar **TaskList** bir örneği olarak geçirilmiş işlevi **görev** biz nesnesi daha önce tanımlanan:

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. Eklemeye devam **tasklist.js** için kullanılan yöntemleri ekleyerek dosya **showTasks**, **addTask**, ve **tasklist.js**:

    ```nodejs
    TaskList.prototype = {
      showTasks: function(req, res) {
        self = this;
        var query = new azure.TableQuery()
          .where('completed eq ?', false);
        self.task.find(query, function itemsFound(error, items) {
          res.render('index',{title: 'My ToDo List ', tasks: items});
        });
      },

      addTask: function(req,res) {
        var self = this
        var item = {
            name: req.body.name, 
            category: req.body.category
        };
        self.task.addItem(item, function itemAdded(error) {
          if(error) {
            throw error;
          }
          res.redirect('/');
        });
      },

      completeTask: function(req,res) {
        var self = this;
        var completedTasks = Object.keys(req.body);
        async.forEach(completedTasks, function taskIterator(completedTask, callback) {
          self.task.updateItem(completedTask, function itemsUpdated(error) {
            if(error){
              callback(error);
            } else {
              callback(null);
            }
          });
        }, function goHome(error){
          if(error) {
            throw error;
          } else {
            res.redirect('/');
          }
        });
      }
    }
    ```

4. Kaydet **tasklist.js** dosya.

### <a name="modify-appjs"></a>App.js'yi değiştirme
1. İçinde **WebRole1** dizin, açık **app.js** dosyasını bir metin düzenleyicisinde.
2. Dosyasının başında azure modülünü yüklemek ve tablo adı ve bölüm anahtarı ayarlamak için aşağıdakileri ekleyin:

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. App.js dosyasında aşağıdaki satırı gördüğünüz aşağıya doğru kaydırma:

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    Önceki satırları aşağıdaki kodla değiştirin. Bu kod örneğini <strong>görev</strong> depolama hesabınıza bir bağlantıyla. <strong>Görev</strong> geçirilir <strong>TaskList</strong>, kullanan, tablo hizmeti ile iletişim kurmak için:

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. Kaydet **app.js** dosya.

### <a name="modify-the-index-view"></a>Dizin görünümünü değiştirme
1. Değiştirme dizinleri **görünümleri** dizin ve açık **index.jade** dosyasını bir metin düzenleyicisinde.
2. Değiştir **index.jade** aşağıdaki kod ile dosya. Bu kod, var olan görevleri görüntülemek için görünümü tanımlayan ve yeni görev eklemeye ve varolanları tamamlandı olarak işaretlemek için bir form tanımlar.

    ```
    extends layout

    block content
      h1= title
      br

      form(action="/completetask", method="post")
        table.table.table-striped.table-bordered
          tr
            td Name
            td Category
            td Date
            td Complete
          if tasks == []
            tr
              td
          else
            each task in tasks
              tr
                td #{task.name._}
                td #{task.category._}
                - var day   = task.Timestamp._.getDate();
                - var month = task.Timestamp._.getMonth() + 1;
                - var year  = task.Timestamp._.getFullYear();
                td #{month + "/" + day + "/" + year}
                td
                  input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
        button.btn(type="submit") Update tasks
      hr
      form.well(action="/addtask", method="post")
        label Item Name:
        input(name="name", type="textbox")
        label Item Category:
        input(name="category", type="textbox")
        br
        button.btn(type="submit") Add item
    ```

3. Kaydet ve Kapat **index.jade** dosya.

### <a name="modify-the-global-layout"></a>Genel düzenini değiştirme
**views** dizinindeki **layout.jade** dosyası diğer **.jade** dosyaları için genel bir şablon olarak kullanılır. Bu adımda, değişiklik **layout.jade** kullanmak üzere bir dosya [Twitter Bootstrap](https://github.com/twbs/bootstrap), iyi görünümlü bir Web sitesi tasarlamayı kolaylaştıran bir araç olan.

1. Karşıdan yükleyip dosyaları ayıklayın [Twitter Bootstrap](http://getbootstrap.com/). Kopya **bootstrap.min.css** dosya **önyükleme\\dağ\\css** klasörüne **ortak\\stil sayfaları** tasklist uygulamanızın dizin.
2. Gelen **görünümleri** klasörü, açık **layout.jade** Metin Düzenleyicisi'nde dosya ve içeriğini aşağıdakilerle değiştirin:
 
```jade
    doctype html
    html
      head
        title= title
        link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
        link(rel='stylesheet', href='/stylesheets/style.css')
      body.app
        nav.navbar.navbar-default
          div.navbar-header
            a.navbar-brand(href='/') My Tasks
        block content
```
3. Kaydet **layout.jade** dosya.

### <a name="running-the-application-in-the-emulator"></a>Uygulamayı Öykünücüde çalıştırma
Öykünücüde uygulamayı başlatmak için aşağıdaki komutu kullanın.

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

Tarayıcı açılır ve aşağıdaki sayfasını görüntüler:

![Disk belleğine alınan web görevleri ve yeni bir görev eklemek için alanları içeren bir tablo ile My görev listesi başlıklı.](./media/table-storage-cloud-service-nodejs/node44.png)

Öğeler eklemek için form kullanın veya var olan öğeleri tamamlanmış olarak işaretleyerek kaldırın.

## <a name="publishing-the-application-to-azure"></a>Uygulamayı Azure'a yayımlama
Windows PowerShell penceresinde Azure barındırılan hizmete yeniden dağıtmak için aşağıdaki cmdlet'i çağırın.

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

Değiştir **myuniquename** bu uygulama için benzersiz bir ada sahip. Değiştir **datacentername** bir Azure veri merkezi adı gibi **Batı ABD**.

Dağıtım tamamlandıktan sonra aşağıdakine benzer bir yanıt görmeniz gerekir:

```
  PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
  WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
  WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
  WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
  WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
  WARNING: 2:19:01 PM - Connecting...
  WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
  WARNING: 2:19:40 PM - Upgrading...
  WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
  WARNING: 2:22:48 PM - Initializing...
  WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
  WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.
```

Belirterek **-başlatma** Önceki cmdlet, tarayıcı seçeneğinde açar ve uygulamanızı yayımlama tamamlandığında, Azure'da çalışan görüntüler.

![My görev listesi sayfasını gösteren bir tarayıcı penceresi. URL sayfası artık Azure üzerinde barındırılan gösterir.](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Durdurma ve uygulamanızı silme
Uygulamanızı dağıttıktan sonra maliyetleri önlemek veya yapılandırabilir ve ücretsiz deneme süre içinde diğer uygulamaları dağıtmak için devre dışı bırakmak isteyebilirsiniz.

Azure web rolü örneklerini harcanan sunucu saati başına faturalandırır.
Uygulamanız dağıtıldıktan sonra örnekler çalışmadığında ve durdurulmuş halde olduğunda bile sunucu saati harcanır.

Aşağıdaki adımlar durdurmak ve uygulamanızı silmek nasıl gösterir.

1. Windows PowerShell penceresinde önceki bölümde oluşturulan hizmet dağıtımını aşağıdaki cmdlet ile durdurun:

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   Hizmetin durdurulması birkaç dakika sürebilir. Hizmet durdurulduğunda bunu belirten bir ileti alırsınız.

2. Hizmeti silmek için aşağıdaki cmdlet'i çağırın:

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   İstendiğinde hizmeti silmek için **Y** yazın.

   Hizmetin silinmesi birkaç dakika sürebilir. Hizmet silindikten sonra hizmet silinmiş olduğunu belirten bir ileti alırsınız.

[Express kullanarak Node.js Web uygulaması]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
[depolama ve veri erişim]: https://docs.microsoft.com/azure/storage/
[Node.js Web uygulaması]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/


