---
title: Azure CLI kullanarak Azure dosya paylaşımlarını yönetme
description: Azure CLI kullanarak Azure Dosyaları'nı yönetmeyi öğrenin.
services: storage
documentationcenter: na
author: wmgries
manager: jeconnoc
editor: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2018
ms.author: wgries
ms.openlocfilehash: 85b69b7b27153139494539ba3eaa882a4ddcd4ef
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="manage-azure-file-shares-using-azure-cli"></a>Azure CLI kullanarak Azure dosya paylaşımlarını yönetme
[Azure Dosyaları](storage-files-introduction.md), Microsoft’un kullanımı kolay bulut dosya sistemidir. Azure dosya paylaşımları, Windows, Linux ve macOS platformlarına bağlanabilir. Bu makalede, Azure CLI kullanarak Azure dosya paylaşımlarıyla çalışmanın temelleri gösterilir. Şunları nasıl yapacağınızı öğrenin: 

> [!div class="checklist"]
> * Kaynak grubu ve depolama hesabı oluşturma
> * Azure dosya paylaşımı oluşturma 
> * Dizin oluşturma
> * Dosyayı karşıya yükleme 
> * Dosya indirme
> * Paylaşım anlık görüntüsü oluşturma ve kullanma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Azure CLI’yı yerel olarak yükleyip kullanmaya karar verirseniz, bu makaledeki adımlar için Azure CLI 2.0.4 veya sonraki bir sürümü çalıştırıyor olmanız gerekir. Azure CLI sürümünüzü bulmak için **az --version** komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

Varsayılan olarak, Azure CLI komutları JavaScript Nesne Gösterimi (JSON) döndürür. JSON, REST API'lerinden ileti gönderip almanın standart yoludur. JSON yanıtlarıyla çalışmayı kolaylaştırmak için, bu kılavuzdaki bazı örneklerde Azure CLI komutları üzerinde *query* parametresi kullanılır. Bu parametre, JSON ayrıştırmak için [JMESPath sorgu dilini](http://jmespath.org/) kullanır. JMESPath sorgu dilini takip ederek Azure CLI komutlarının sonuçlarını kullanma hakkında daha fazla bilgi almak için bkz. [JMESPath öğreticisi](http://jmespath.org/tutorial.html).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Henüz bir Azure kaynak grubunuz yoksa, [az group create](/cli/azure/group#create) komutunu kullanarak bir tane oluşturabilirsiniz. 

Aşağıdaki örnek *EastUS* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Depolama hesabı, Azure dosya paylaşımlarını veya bloblar veya sorgular gibi diğer depolama kaynaklarını dağıtabileceğiniz, paylaşılan bir depolama havuzudur. Bir depolama hesabında sınırsız sayıda dosya paylaşımı olabilir. Bir paylaşım, depolama hesabının kapasite limitlerine kadar sınırsız sayıda dosyayı depolayabilir.

Aşağıdaki örnekte, [az storage account create](/cli/azure/storage/account#create) komutu kullanılarak *mystorageaccount\<random number\>* adlı bir depolama hesabı oluşturulur ve bu depolama hesabının adı `$STORAGEACCT` değişkenine yerleştirilir. Depolama hesabı adları benzersiz olmalıdır. `$RANDOM` kullanıldığında depolama hesabı adına bir sayı eklenerek benzer hale getirilir. 

```azurecli-interactive 
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

### <a name="get-the-storage-account-key"></a>Depolama hesabı anahtarını alma
Depolama hesabı anahtarları, depolama hesabındaki kaynaklara erişimi denetler. Bu anahtarlar, depolama hesabını oluşturduğunuzda otomatik olarak oluşturulur. [az storage account keys list](/cli/azure/storage/account/keys#list) komutunu kullanarak depolama hesabınızın depolama hesabı anahtarlarını alabilirsiniz: 

```azurecli-interactive 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma
Artık ilk Azure dosya paylaşımınızı oluşturabilirsiniz. Dosya paylaşımlarını oluşturmak için [az storage share create](/cli/azure/storage/share#create) komutunu kullanın. Bu örnekte *myshare* adlı bir Azure dosya paylaşımı oluşturulur: 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

> [!IMPORTANT]  
> Paylaşım adları yalnızca küçük harf, sayı ve tek kısa çizgi içerebilir (ancak kısa çizgiyle başlayamaz). Dosya paylaşımlarının ve dosyaların adlandırılması hakkında tüm ayrıntılara ulaşmak için bkz. [Paylaşım, dizin, dosya ve meta verileri adlandırma ve bunlara başvuruda bulunma](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

## <a name="work-with-the-contents-of-an-azure-file-share"></a>Bir Azure dosya paylaşımının içerikleriyle çalışma
Artık Azure dosya paylaşımını oluşturduğunuza göre, [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) veya [macOS](storage-how-to-use-files-mac.md)'ta SMB kullanarak dosya paylaşımını bağlayabilirsiniz. Alternatif olarak, Azure CLI’yi kullanarak Azure dosya paylaşımınızla çalışabilirsiniz. SMB kullanarak dosya paylaşımınızı bağlamak yerine Azure CLI kullanmanın avantajı, Azure CLI ile yapılan tüm isteklerin Dosya REST API’si kullanılarak yapılmasıdır. Dosya REST API’sini kullanarak, aşağıdaki konumlardan dosya paylaşımınızda dosya ve dizinler oluşturabilir, değiştirebilir ve silebilirsiniz:

- Bash Azure Cloud Shell (SMB üzerinden dosya paylaşımlarını bağlayamaz)
- Engeli kaldırılmış 445 numaralı bağlantı noktasına sahip olmayan şirket içi istemciler gibi, SMB paylaşımlarını bağlayamayan istemciler
- [Azure İşlevleri](../../azure-functions/functions-overview.md) gibi sunucusuz senaryolar

### <a name="create-a-directory"></a>Dizin oluşturma
Azure dosya paylaşımınızın kökünde *myDirectory* adlı yeni bir dizin oluşturmak için [`az storage directory create`](/cli/azure/storage/directory#az_storage_directory_create) komutunu kullanın:

```azurecli-interactive
az storage directory create \
   --account-name $STORAGEACCT \
   --account-key $STORAGEKEY \
   --share-name "myshare" \
   --name "myDirectory" 
```

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme
[`az storage file upload`](/cli/azure/storage/file#az_storage_file_upload) komutunu kullanarak bir dosyayı karşıya yükleme işlemini göstermek için öncelikle Cloud Shell karalama sürücüsünde karşıya yüklenecek bir dosya oluşturun. Aşağıdaki örnekte dosyayı oluşturup karşıya yüklersiniz:

```azurecli-interactive
date > ~/clouddrive/SampleUpload.txt

az storage file upload \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --source "~/clouddrive/SampleUpload.txt" \
    --path "myDirectory/SampleUpload.txt"
```

Azure CLI'yi yerel olarak çalıştırıyorsanız, `~/clouddrive` değerini makinenizde var olan bir yolla değiştirin.

Dosyayı karşıya yükledikten sonra, [`az storage file list`](/cli/azure/storage/file#az_storage_file_list) komutunu kullanarak dosyanın Azure dosya paylaşımınıza yüklendiğinden emin olabilirsiniz:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory" \
    --output table
```

### <a name="download-a-file"></a>Dosya indirme
Cloud Shell karalama sürücünüze yüklediğiniz dosyanın bir kopyasını indirmek için [`az storage file download`](/cli/azure/storage/file#az_storage_file_download) komutunu kullanabilirsiniz:

```azurecli-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists, because you've run this example before
rm -rf ~/clouddrive/SampleDownload.txt

az storage file download \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt" \
    --dest "~/clouddrive/SampleDownload.txt"
```

### <a name="copy-files"></a>Dosyaları kopyalama
Yaygın görevlerden biri dosyaları bir dosya paylaşımından diğerine veya Azure Blob depolama kapsayıcısına/kapsayıcısından kopyalamaktır. Bu işlevselliği göstermek için yeni bir paylaşım oluşturun. Bu yeni paylaşıma yüklediğiniz dosyayı [az storage file copy](/cli/azure/storage/file/copy) komutunu kullanarak kopyalayın: 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare2"

az storage directory create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --name "myDirectory2"

az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-share "myshare" \
    --source-path "myDirectory/SampleUpload.txt" \
    --destination-share "myshare2" \
    --destination-path "myDirectory2/SampleCopy.txt"
```

Şimdi, yeni paylaşımdaki dosyaları listelerseniz kopyalanan dosyanızı görmeniz gerekir:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --output table
```

`az storage file copy start` komutu Azure dosya paylaşımları ile Azure Blob depolama kapsayıcıları arasında dosya taşıma işlemleri için kullanışlı olsa da, daha büyük taşıma işlemlerinde AzCopy kullanmanız önerilir. (Taşınan dosyaların sayısı veya boyutu bakımından daha büyük.) [Linux için AzCopy](../common/storage-use-azcopy-linux.md) ve [Windows için AzCopy](../common/storage-use-azcopy.md) hakkında daha fazla bilgi edinin. AzCopy yerel olarak yüklü olmalıdır. AzCopy, Cloud Shell’de kullanılamaz. 

## <a name="create-and-modify-share-snapshots"></a>Paylaşım anlık görüntülerini oluşturma ve değiştirme
Azure dosya paylaşımıyla yerine getirebileceğiniz bir diğer yararlı görev ise paylaşım anlık görüntüleri oluşturmaktır. Anlık görüntü, Azure dosya paylaşımının zamanın bir noktasındaki kopyasını saklar. Paylaşım anlık görüntüleri, zaten tanıyor olabileceğiniz bazı işletim sistemi teknolojilerine benzerdir:
- Linux sistemleri için [Mantıksal Birim Yöneticisi (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) anlık görüntüleri
- macOS için [Apple Dosya Sistemi (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) anlık görüntüleri
- NTFS ve ReFS gibi Windows dosya sistemleri için [Birim Gölge Kopyası Hizmeti (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636)

[`az storage share snapshot`](/cli/azure/storage/share#az_storage_share_snapshot) komutunu kullanarak bir paylaşım anlık görüntüsü oluşturabilirsiniz:

```azurecli-interactive
SNAPSHOT=$(az storage share snapshot \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --query "snapshot" | tr -d '"')
```

### <a name="browse-share-snapshot-contents"></a>Paylaşım anlık görüntüsünün içeriğine göz atma
`$SNAPSHOT` değişkeninde yakaladığınız paylaşım anlık görüntüsünün zaman damgasını `az storage file list` komutuna geçirerek, paylaşım anlık görüntüsünün içeriğine göz atabilirsiniz:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --snapshot $SNAPSHOT \
    --output table
```

### <a name="list-share-snapshots"></a>Paylaşım anlık görüntülerini listeleme
Paylaşımınız için aldığınız anlık görüntülerin listesini görmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az storage share list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --include-snapshot \
    --query "[? name=='myshare' && snapshot!=null]" | tr -d '"'
```

### <a name="restore-from-a-share-snapshot"></a>Paylaşım anlık görüntüsünden geri yükleme
Daha önce kullandığınız `az storage file copy start` komutunu kullanarak dosyayı geri yükleyebilirsiniz. İlk olarak, anlık görüntüden geri yükleyebilmek için, karşıya yüklediğiniz SampleUpload.txt dosyasını silin:

```azurecli-interactive
# Delete SampleUpload.txt
az storage file delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt"

# Build the source URI for a snapshot restore
URI=$(az storage account show \
    --resource-group "myResourceGroup" \
    --name $STORAGEACCT \
    --query "primaryEndpoints.file" | tr -d '"')

URI=$URI"myshare/myDirectory/SampleUpload.txt?sharesnapshot="$SNAPSHOT

# Restore SampleUpload.txt from the share snapshot
az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-uri $URI \
    --destination-share "myshare" \
    --destination-path "myDirectory/SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>Paylaşım anlık görüntüsünü silme
[`az storage share delete`](/cli/azure/storage/share#az_storage_share_delete) komutunu kullanarak bir paylaşım anlık görüntüsünü silebilirsiniz. `--snapshot` parametresine `$SNAPSHOT` başvurusunu içeren değişkeni kullanın:

```azurecli-interactive
az storage share delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --snapshot $SNAPSHOT
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
İşiniz bittiğinde, [`az group delete`](/cli/azure/group#delete) komutunu kullanarak kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz: 

```azurecli-interactive 
az group delete --name "myResourceGroup"
```

Alternatif olarak, kaynakları ayrı ayrı kaldırabilirsiniz.
- Bu makale boyunca oluşturduğunuz Azure dosya paylaşımlarını kaldırmak için:

    ```azurecli-interactive
    az storage share delete \
        --account-name $STORAGEACCT \
        --account-key $STORAGEKEY \
        --name "myshare" \
        --delete-snapshots include

    az storage share delete \
        --account-name $STORAGEACCT \
        --account-key $STORAGEKEY \
        --name "myshare2" \
        --delete-snapshots include
    ```

- Depolama hesabını kaldırmak için. (Bu işlem, oluşturduğunuz Azure dosya paylaşımlarını ve bir Azure Blob depolama kapsayıcısı gibi oluşturmuş olabileceğiniz diğer depolama kaynaklarını örtülü olarak kaldırır.)

    ```azurecli-interactive
    az storage account delete \
        --resource-group "myResourceGroup" \
        --name $STORAGEACCT \
        --yes
    ```

## <a name="next-steps"></a>Sonraki adımlar
- [Dosya paylaşımlarını Azure portalıyla yönetme](storage-how-to-use-files-portal.md)
- [Dosya paylaşımlarını Azure PowerShell ile yönetme](storage-how-to-use-files-powershell.md)
- [Depolama Gezgini ile dosya paylaşımlarını yönetme](storage-how-to-use-files-storage-explorer.md)
- [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)