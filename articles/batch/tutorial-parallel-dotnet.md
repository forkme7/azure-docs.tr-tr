---
title: Paralel iş yükü çalıştırma - Azure Batch .NET
description: Öğretici - Batch .NET istemci kitaplığını kullanarak Azure Batch’te ffmpeg ile paralel medya dosyaları dönüştürme
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 01/23/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 57fc70d5b47f18affa90e1153884e8af23d937ec
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-run-a-parallel-workload-with-azure-batch-using-the-net-api"></a>Öğretici: .NET API’si kullanarak Azure Batch ile paralel iş yükü çalıştırma

Büyük ölçekli paralel ve yüksek performanslı bilgi işlem (HPC) toplu işlerini Azure’da verimli bir şekilde çalıştırmak için Azure Batch’i kullanın. Bu öğreticide, Batch kullanarak paralel iş yükü çalıştırmaya ilişkin bir C# örneği açıklanmaktadır. Genel bir Batch uygulaması iş akışı hakkında bilgi alacak ve Batch ve Depolama kaynakları ile programlı olarak etkileşimde bulunmayı öğreneceksiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Batch hesabınıza bir uygulama paketi ekleme
> * Batch ve Depolama hesapları ile kimlik doğrulaması
> * Depolama hizmetine giriş dosyaları yükleme
> * Bir uygulamayı çalıştırmak için işlem düğümleri havuzu oluşturma
> * Giriş dosyalarını işlemek için bir iş ve görevler oluşturma
> * Görev yürütmeyi izleme
> * Çıkış dosyalarını alma

Bu öğreticide, [ffmpeg](http://ffmpeg.org/) açık kaynak aracını kullanarak MP4 medya dosyalarını MP3 biçimine paralel şekilde dönüştürürsünüz. 

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio IDE](https://www.visualstudio.com/vs) (Visual Studio 2015 veya daha yeni bir sürüm). 

* Bir Batch hesabı ve bağlı bir Azure Depolama hesabı. Bu hesapları oluşturmak için [Azure portalı](quick-create-portal.md) veya [Azure CLI](quick-create-cli.md) kullanan Batch hızlı başlangıçlarına bakın.

* [ffmpeg 3.4’ün Windows 64 bit sürümü](https://ffmpeg.zeranoe.com/builds/win64/static/ffmpeg-3.4-win64-static.zip) (.zip). Zip dosyasını yerel bilgisayarınıza indirin. Bu öğreticide yalnızca zip dosyası gereklidir. Dosyanın sıkıştırmasını açmanız veya yerel olarak yüklemeniz gerekmez. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.


## <a name="add-an-application-package"></a>Uygulama paketi ekleme

Batch hesabınıza [uygulama paketi](batch-application-packages.md) olarak ffmpeg eklemek için Azure portalını kullanın. Uygulama paketleri, görev uygulamalarını ve havuzunuzdaki işlem düğümlerine dağıtımlarını yönetmenize yardımcı olur. 

1. Azure portalında **Diğer hizmetler** > **Batch hesapları** seçeneğine ve Batch hesabınızın adına tıklayın.
3. **Uygulamalar** > **Ekle**’ye tıklayın.
4. **Uygulama kimliği** için *ffmpeg*, paket sürümü için *3.4* girin. Daha önce indirdiğiniz ffmpeg zip dosyasını seçip **Tamam**’a tıklayın. ffmpeg uygulama paketi, Batch hesabınıza eklenir.

![Uygulama paketi ekleme](./media/tutorial-parallel-dotnet/add-application.png)

[!INCLUDE [batch-common-credentials](../../includes/batch-common-credentials.md)]

## <a name="download-and-run-the-sample"></a>Örneği indirme ve çalıştırma

### <a name="download-the-sample"></a>Örneği indirme

GitHub’dan [örnek uygulamayı indirin veya kopyalayın](https://github.com/Azure-Samples/batch-dotnet-ffmpeg-tutorial). Örnek uygulama deposunu bir Git istemcisi ile kopyalamak için aşağıdaki komutu kullanın:

```
git clone https://github.com/Azure-Samples/batch-dotnet-ffmpeg-tutorial.git
```

Visual Studio `BatchDotNetFfmpegTutorial.sln` çözüm dosyasını içeren dizine gidin.

Çözüm dosyasını Visual Studio'da açın ve `program.cs` içindeki kimlik bilgisi dizelerini hesaplarınız için edindiğiniz değerlerle güncelleştirin. Örnek:

```csharp
// Batch account credentials
private const string BatchAccountName = "mybatchaccount";
private const string BatchAccountKey  = "xxxxxxxxxxxxxxxxE+yXrRvJAqT9BlXwwo1CwF+SwAYOxxxxxxxxxxxxxxxx43pXi/gdiATkvbpLRl3x14pcEQ==";
private const string BatchAccountUrl  = "https://mybatchaccount.mybatchregion.batch.azure.com";

// Storage account credentials
private const string StorageAccountName = "mystorageaccount";
private const string StorageAccountKey  = "xxxxxxxxxxxxxxxxy4/xxxxxxxxxxxxxxxxfwpbIC5aAWA8wDu+AFXZB827Mt9lybZB1nUcQbQiUrkPtilK5BQ==";
```

Ayrıca, çözümdeki ffmpeg uygulama paketi başvurusunun Batch hesabınıza yüklediğiniz ffmpeg paketinin kimlik ve sürümüyle eşleştiğinden emin olun.

```csharp
const string appPackageId = "ffmpeg";
const string appPackageVersion = "3.4";
```
### <a name="build-and-run-the-sample-project"></a>Örnek projeyi derleme ve çalıştırma

* Çözüm Gezgini'nde çözüme sağ tıklayın ve **Derleme Çözümü**’ne tıklayın. 

* İstenirse, herhangi bir NuGet paketinin geri yüklenmesini onaylayın. Eksik paketleri indirmeniz gerekirse, [NuGet Paket Yöneticisi](https://docs.nuget.org/consume/installing-nuget)’nin yüklü olduğundan emin olun.

Ardından çalıştırın. Örnek uygulamayı çalıştırdığınızda, konsol çıktısı aşağıdakine benzer. Yürütme sırasında, havuzun işlem düğümleri başlatıldığı sırada `Monitoring all tasks for 'Completed' state, timeout in 00:30:00...` konumunda bir duraklama yaşarsınız. 

```
Sample start: 12/12/2017 3:20:21 PM

Container [input] created.
Container [output] created.
Uploading file LowPriVMs-1.mp4 to container [input]...
Uploading file LowPriVMs-2.mp4 to container [input]...
Uploading file LowPriVMs-3.mp4 to container [input]...
Uploading file LowPriVMs-4.mp4 to container [input]...
Uploading file LowPriVMs-5.mp4 to container [input]...
Creating pool [WinFFmpegPool]...
Creating job [WinFFmpegJob]...
Adding 5 tasks to job [WinFFmpegJob]...
Monitoring all tasks for 'Completed' state, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Deleting container [input]...

Sample end: 12/12/2017 3:29:36 PM
Elapsed time: 00:09:14.3418742
```


Havuz, işlem düğümleri, iş ve görevleri izlemek için Azure portalında Batch hesabınıza gidin. Örneğin, havuzunuzdaki işlem düğümlerinin ısı haritasını görmek için **Havuzlar** > *WinFFmpegPool* öğesine tıklayın.

Görevler çalıştırılırken ısı haritası aşağıdakine benzer:

![Havuz ısı haritası](./media/tutorial-parallel-dotnet/pool.png)


Varsayılan yapılandırmasında uygulama çalıştırıldığında tipik yürütme süresi yaklaşık **10 dakikadır**. En uzun süreyi havuz oluşturma işlemi alır.

[!INCLUDE [batch-common-tutorial-download](../../includes/batch-common-tutorial-download.md)]

## <a name="review-the-code"></a>Kodu gözden geçirin

Aşağıdaki bölümlerde örnek uygulama, Batch hizmetinde iş yükünü işlemeyi gerçekleştiren adımlara ayrılmıştır. Örneğin her kod satırı tartışılmadığından, bu makalenin kalanını okurken Visual Studio’daki açık çözüme başvurun.

### <a name="authenticate-blob-and-batch-clients"></a>Blob ve Batch istemcilerinde kimlik doğrulaması

Bağlı depolama hesabı ile etkileşimde bulunmak üzere uygulama, .NET için Azure Depolama İstemci Kitaplığı’nı kullanır. [CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) ile hesaba başvuru oluşturur ve paylaşılan anahtar kimlik doğrulamasını kullanarak kimlik doğrulaması yapar. Sonra bir [CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient) oluşturur.

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
StorageAccountName, StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConnectionString);

CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Uygulama, Batch hizmetinde havuz, iş ve görevleri oluşturup yönetmek üzere bir [BatchClient](/dotnet/api/microsoft.azure.batch.batchclient) nesnesi oluşturur. Örnekteki Batch istemcisi, paylaşılan anahtar kimlik doğrulaması kullanır. Batch ayrıca bireysel kullanıcıların ya da katılımsız bir uygulamanın kimlik doğrulamasını yapmak için [Azure Active Directory](batch-aad-auth.md) aracılığıyla kimlik doğrulamayı destekler.

```csharp
BatchSharedKeyCredentials sharedKeyCredentials = new BatchSharedKeyCredentials(BatchAccountUrl, BatchAccountName, BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(sharedKeyCredentials))
...
```

### <a name="upload-input-files"></a>Giriş dosyalarını karşıya yükleme

Uygulama, giriş dosyaları için bir depolama kapsayıcısı (MP4 biçimi) ve görev çıkışı için bir kapsayıcı oluşturmak üzere `blobClient` nesnesini `CreateContainerIfNotExist` yöntemine geçirir.

```csharp
  CreateContainerIfNotExist(blobClient, inputContainerName;
  CreateContainerIfNotExist(blobClient, outputContainerName);
```

Sonra, dosyalar yerel `InputFiles` klasöründen giriş kapsayıcısına yüklenir. Depolama alanındaki dosyalar, Batch hizmetinin daha sonra işlem düğümlerine indirebileceği Batch [ResourceFile](/dotnet/api/microsoft.azure.batch.resourcefile) nesneleri olarak tanımlanır. 

Dosyalar karşıya yüklenirken `Program.cs` içindeki iki yöntem kullanılır:

* `UploadResourceFilesToContainer`: ResourceFile nesnelerinin bir koleksiyonunu döndürür ve `filePaths`parametresine geçirilen her dosyayı karşıya yüklemek için `UploadResourceFileToContainer` çağrısı yapar.
* `UploadResourceFileToContainer`: Her dosyayı blob olarak giriş kapsayıcısına yükler. Blob karşıya yükledikten sonra, dosya için paylaşılan erişim imzasını (SAS) alır ve temsil ettiği bir ResourceFile nesnesini döndürür. 

```csharp
  List<string> inputFilePaths = new List<string>(Directory.GetFileSystemEntries(@"..\..\InputFiles", "*.mp4",
      SearchOption.TopDirectoryOnly));

  List<ResourceFile> inputFiles = UploadResourceFilesToContainer(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Dosyaları .NET ile blob olarak bir depolama hesabına yükleme hakkında ayrıntılar için bkz. [.NET kullanarak Azure Blob depolama kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

### <a name="create-a-pool-of-compute-nodes"></a>İşlem düğümleri havuzu oluşturma

Ardından örnek, `CreatePoolIfNotExist` çağrısıyla Batch hesabında bir işlem düğümü havuzu oluşturur. Bu tanımlı yöntem, düğüm sayısını, VM boyutunu ve havuz yapılandırmasını ayarlamak üzere [BatchClient.PoolOperations.CreatePool](/dotnet/api/microsoft.azure.batch.pooloperations.createpool) yöntemini kullanır. Burada [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration) nesnesi, Azure Market’te yayımlanmış bir Windows Server görüntüsüne [ImageReference](/dotnet/api/microsoft.azure.batch.imagereference) belirtir. Batch, Azure Market’te çok çeşitli VM görüntülerinin yanı sıra özel VM görüntülerini destekler.

Düğüm sayısı ve VM boyutu, tanımlı sabitler kullanılarak ayarlanır. Batch, adanmış düğümleri ve [düşük öncelikli düğümleri](batch-low-pri-vms.md) destekler ve havuzlarınızda bunlardan birini ya da her ikisini birden kullanabilirsiniz. Adanmış düğümler, havuzunuz için ayrılmıştır. Düşük öncelikli düğümler ise Azure’daki fazlalık VM kapasitesinden indirimli bir fiyat karşılığında sunulur. Azure’da yeterli kapasite yoksa düşük öncelikli düğümler kullanılamaz duruma gelir. Örnek, varsayılan olarak *Standard_A1_v2* boyutunda yalnızca 5 düşük öncelikli düğüm içeren bir havuz oluşturur. 

ffmpeg uygulaması, havuz yapılandırmasına bir [ApplicationPackageReference](/dotnet/api/microsoft.azure.batch.applicationpackagereference) eklenerek işlem düğümlerine dağıtılır. 

[Commit](/dotnet/api/microsoft.azure.batch.cloudpool.commit) yöntemi, havuzu Batch hizmetine gönderir.

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "MicrosoftWindowsServer",
    offer: "WindowsServer",
    sku: "2012-R2-Datacenter-smalldisk",
    version: "latest");

VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
    imageReference: imageReference,
    nodeAgentSkuId: "batch.node.windows amd64");

pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: DedicatedNodeCount,
    targetLowPriorityComputeNodes: LowPriorityNodeCount,
    virtualMachineSize: PoolVMSize,                                                
    virtualMachineConfiguration: virtualMachineConfiguration);

pool.ApplicationPackageReferences = new List<ApplicationPackageReference>
    {
    new ApplicationPackageReference {
    ApplicationId = appPackageId,
    Version = appPackageVersion}};

pool.Commit();  
```

### <a name="create-a-job"></a>Bir iş oluşturma

Bir Batch işi, üzerinde görevlerin çalıştırılacağı bir havuz ve iş için öncelik ile zamanlama gibi isteğe bağlı ayarları belirtir. Örnek, `CreateJobIfNotExist` çağrısıyla bir iş oluşturur. Bu tanımlı yöntem, havuzunuzda bir iş oluşturmak üzere [BatchClient.JobOperations.CreateJob](/dotnet/api/microsoft.azure.batch.joboperations.createjob) yöntemini kullanır. 

[Commit](/dotnet/api/microsoft.azure.batch.cloudjob.commit) yöntemi, işi Batch hizmetine gönderir. Başlangıçta iş hiçbir görev içermez.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = JobId;
    job.PoolInformation = new PoolInformation { PoolId = PoolId };

job.Commit();        
```

### <a name="create-tasks"></a>Görev oluşturma

Örnek, `AddTasks` yöntemini çağırarak iş içinde görevler oluşturur ve [CloudTask](/dotnet/api/microsoft.azure.batch.cloudtask) nesnelerinin bir listesini oluşturur. Her `CloudTask`, bir [CommandLine](/dotnet/api/microsoft.azure.batch.cloudtask.commandline) özelliği kullanarak giriş `ResourceFile` nesnesini işlemek üzere ffmpeg çalıştırır. ffmpeg, daha önce havuz oluşturulduğunda her bir düğüme yüklenmiştir. Burada komut satırı, her bir giriş MP4 (video) dosyasını bir MP3 (ses) dosyasına dönüştürmek için ffmpeg çalıştırır.

Örnek, komut satırını çalıştırdıktan sonra MP3 dosyası için bir [OutputFile](/dotnet/api/microsoft.azure.batch.outputfile) nesnesi oluşturur. Her bir görevin çıkış dosyaları (bu örnekte bir tane), görevin [OutputFiles](/dotnet/api/microsoft.azure.batch.cloudtask.outputfiles) özelliği kullanılarak bağlı depolama hesabındaki bir kapsayıcıya yüklenir.

Sonra örnek, [AddTask](/dotnet/api/microsoft.azure.batch.joboperations.addtask) yöntemi ile görevleri işe ekler ve işlem düğümleri üzerinde çalışmak üzere kuyruğa alır. 

```csharp
for (int i = 0; i < inputFiles.Count; i++)
{
    string taskId = String.Format("Task{0}", i);

    // Define task command line to convert each input file.
    string appPath = String.Format("%AZ_BATCH_APP_PACKAGE_{0}#{1}%", appPackageId, appPackageVersion);
    string inputMediaFile = inputFiles[i].FilePath;
    string outputMediaFile = String.Format("{0}{1}",
        System.IO.Path.GetFileNameWithoutExtension(inputMediaFile),
        ".mp3");
    string taskCommandLine = String.Format("cmd /c {0}\\ffmpeg-3.4-win64-static\\bin\\ffmpeg.exe -i {1} {2}", appPath, inputMediaFile, outputMediaFile);

    // Create a cloud task (with the task ID and command line) 
    CloudTask task = new CloudTask(taskId, taskCommandLine);
    task.ResourceFiles = new List<ResourceFile> { inputFiles[i] };
   

    // Task output file
    List<OutputFile> outputFileList = new List<OutputFile>();
    OutputFileBlobContainerDestination outputContainer = new OutputFileBlobContainerDestination(outputContainerSasUrl);
    OutputFile outputFile = new OutputFile(outputMediaFile,
       new OutputFileDestination(outputContainer),
       new OutputFileUploadOptions(OutputFileUploadCondition.TaskSuccess));
    outputFileList.Add(outputFile);
    task.OutputFiles = outputFileList;
    tasks.Add(task);
}

// Add tasks as a collection
batchClient.JobOperations.AddTask(jobId, tasks);
```

### <a name="monitor-tasks"></a>Görevleri izleme

Batch bir işe görevler eklediğinde, hizmet bu görevleri ilişkili havuzdaki işlem düğümleri üzerinde yürütülmek üzere otomatik olarak kuyruğa alır ve zamanlar. Belirttiğiniz ayarlara göre, Batch tüm kuyruğa alma, zamanlama, yeniden deneme görevlerini ve diğer görev yönetimi sorumluluklarını yerine getirir. 

Görevin yürütülüşünün izlenmesi için birçok yaklaşım vardır. Bu örnek yalnızca tamamlandıktan sonra ve görevin başarısız ya da başarılı durumlarını bildirmek üzere bir `MonitorTasks` yöntemi tanımlar. `MonitorTasks` kodu, görevler hakkında yalnızca çok az bilgiyi verimli bir şekilde seçmek üzere bir [ODATADetailLevel](/dotnet/api/microsoft.azure.batch.odatadetaillevel) belirtir. Sonra, görev durumlarını izlemeye yönelik yardımcı programlar sağlayan bir [TaskStateMonitor](/dotnet/api/microsoft.azure.batch.taskstatemonitor) oluşturur. `MonitorTasks` içinde, örnek tüm görevlerin bir süre sınırı içerisinde `TaskState.Completed` düzeyine ulaşmasını bekler. Sonra işi sonlandırır ve tamamlanan tüm görevleri bildirir, ancak sıfır olmayan çıkış kodu gibi bir arıza ile karşılaşmış olabilir.

```csharp
TaskStateMonitor taskStateMonitor = batchClient.Utilities.CreateTaskStateMonitor();
try
{
    batchClient.Utilities.CreateTaskStateMonitor().WaitAll(addedTasks, TaskState.Completed, timeout);
}
catch (TimeoutException)
{
    batchClient.JobOperations.TerminateJob(jobId, failureMessage);
    Console.WriteLine(failureMessage);
}
batchClient.JobOperations.TerminateJob(jobId, successMessage);
...

```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Görevleri çalıştırdıktan sonra, uygulama kendi oluşturduğu giriş depolama kapsayıcısını otomatik olarak siler ve Batch havuzu ve işini silme seçeneğini sunar. BatchClient'ın [JobOperations](/dotnet/api/microsoft.azure.batch.batchclient.joboperations) ve [PoolOperations](/dotnet/api/microsoft.azure.batch.batchclient.pooloperations) sınıflarının her ikisi de, silmeyi onaylamanız durumunda çağrılan ilgili silme yöntemlerini içerir. İşlerin ve görevlerin kendileri için sizden ücret alınmasa da işlem düğümleri için ücret alınır. Bu nedenle, havuzları yalnızca gerektiğinde ayırmanız önerilir. Havuzu sildiğinizde düğümler üzerindeki tüm görev çıkışları silinir. Ancak, giriş ve çıkış dosyaları depolama hesabında kalır.

Kaynak grubunu, Batch hesabını ve depolama hesabını artık gerekli değilse silin. Azure portalında bu işlemi yapmak için Batch hesabına ait kaynak grubunu seçin ve **Kaynak Grubunu Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunlar hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Batch hesabınıza bir uygulama paketi ekleme
> * Batch ve Depolama hesapları ile kimlik doğrulaması
> * Depolama hizmetine giriş dosyaları yükleme
> * Bir uygulamayı çalıştırmak için işlem düğümleri havuzu oluşturma
> * Giriş dosyalarını işlemek için bir iş ve görevler oluşturma
> * Görev yürütmeyi izleme
> * Çıkış dosyalarını alma

Batch iş yüklerini zamanlamak ve işlemek üzere .NET API kullanmaya ilişkin daha fazla örnek için GitHub üzerindeki örneklere bakın.

> [!div class="nextstepaction"]
> [Batch C# örnekleri](https://github.com/Azure/azure-batch-samples/tree/master/CSharp)
