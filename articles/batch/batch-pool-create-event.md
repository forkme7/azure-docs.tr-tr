---
title: Azure Batch havuzu oluşturma olay | Microsoft Docs
description: Batch havuzundaki başvurusunu olayı oluşturun.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: danlep
ms.openlocfilehash: bf7dfc2600c3d94faeb8d03561f6f2b30a0ee2d2
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="pool-create-event"></a>Havuz oluşturma olayı

 Bu olay, bir havuz oluşturulduktan sonra yayınlanır. Günlük içeriğini havuzu hakkındaki genel bilgileri açığa çıkarır. Bir havuzu yeniden boyutlandırma başlangıç olay havuzunun hedef boyutu 0 işlem düğümleri büyükse, bu olay hemen sonra oluşacak unutmayın.

 Aşağıdaki örnek, bir havuz gövdesi oluşturma olayı CloudServiceConfiguration özelliği kullanılarak oluşturulan bir havuz için gösterir.

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|id|Dize|Havuzun kimliği.|
|displayName|Dize|Havuz görünen adı.|
|vmSize|Dize|Sanal makineler havuzunda boyutu. Bir havuzdaki tüm sanal makineleri aynı boyuttadır. <br/><br/> Hakkında bilgi için kullanılabilir boyutları sanal makineleri bulut Hizmetleri için havuzları (cloudServiceConfiguration ile oluşturulan havuzlarının), bkz: [Cloud Services boyutları](http://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/). Toplu işlem dışında tüm Cloud Services VM boyutlarını destekler `ExtraSmall`.<br/><br/> Kullanılabilir VM hakkında bilgi için sanal makineleri marketten (virtualMachineConfiguration ile oluşturulan havuzlarının) görüntüleri kullanarak havuzları için Boyutlar bkz [sanal makineler için Boyutlar](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (Linux) veya [sanal boyutları Makineler](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (Windows). Batch `STANDARD_A0` ve premium depolama alanına sahip olanlar (`STANDARD_GS`, `STANDARD_DS` ve `STANDARD_DSV2` serisi) dışında tüm Azure sanal makinelerini destekler.|
|[cloudServiceConfiguration](#bk_csconf)|Karmaşık Tür|Havuz için bulut hizmeti yapılandırması.|
|[virtualMachineConfiguration](#bk_vmconf)|Karmaşık Tür|Havuz için sanal makine yapılandırma.|
|[networkConfiguration](#bk_netconf)|Karmaşık Tür|Havuz için ağ yapılandırması.|
|resizeTimeout|Zaman|İşlem düğümleri havuzu son yeniden boyutlandırma işlemi için belirtilen havuzuna ayırma için zaman aşımı.  (Havuzu oluşturulduğunda ilk boyutlandırma yeniden boyutlandırma sayılır.)|
|targetDedicated|Int32|Havuzu için istenen işlem düğümleri sayısı.|
|enableAutoScale|Bool|Havuz boyutu otomatik olarak zaman içinde ayarlar olup olmadığını belirtir.|
|enableInterNodeCommunication|Bool|Havuzun düğümler arasında doğrudan iletişim için ayarlanmış olup olmadığını belirtir.|
|isAutoPool|Bool|Speficies havuzu bir işin AutoPool mekanizması olup oluşturuldu.|
|maxTasksPerNode|Int32|Havuz tek işlem düğümünde eşzamanlı olarak çalışabilecek görevleri maksimum sayısı.|
|vmFillType|Dize|Toplu İşlem hizmetinin görevleri havuzdaki işlem düğümleri arasında nasıl dağıtacağını tanımlar. Geçerli değerler paketindeki veya yayılır.|

###  <a name="bk_csconf"></a> cloudServiceConfiguration

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|osFamily|Dize|Havuzdaki sanal makinelere yüklenecek Azure konuk işletim sistemi ailesi.<br /><br /> Olası değerler şunlardır:<br /><br /> **2** – işletim sistemi ailesi 2, Windows Server 2008 R2 SP1'e eşit.<br /><br /> **3** – işletim sistemi ailesi 3, Windows Server 2012'ye eşdeğer.<br /><br /> **4** – işletim sistemi ailesi 4, Windows Server 2012 R2 için eşdeğer.<br /><br /> Daha fazla bilgi için bkz: [Azure konuk işletim sistemi sürümleri](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|
|targetOSVersion|Dize|Havuzdaki sanal makinelerde yüklü olmasını Azure konuk işletim sistemi sürümü.<br /><br /> Varsayılan değer **\*** belirtilen ailesi için en son işletim sistemi sürümünü belirtir.<br /><br /> İzin verilen diğer değerleri için bkz: [Azure konuk işletim sistemi sürümleri](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|

###  <a name="bk_vmconf"></a> virtualMachineConfiguration

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|[imageReference](#bk_imgref)|Karmaşık Tür|Platform veya kullanılacak Market görüntüsü hakkındaki bilgileri belirtir.|
|nodeAgentSKUId|Dize|İşlem düğümü üzerinde sağlanan toplu işlem düğüm Aracısı SKU.|
|[windowsConfiguration](#bk_winconf)|Karmaşık Tür|Sanal makine üzerinde Windows işletim sistemi ayarlarını belirtir. Bu özellik olmamalıdır Imagereference Linux işletim sistemi görüntüsü başvuran varsa belirtildi.|

###  <a name="bk_imgref"></a> Imagereference

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|publisher|Dize|Görüntünün yayımcısı.|
|teklif|Dize|Görüntü sunar.|
|SKU|Dize|Görüntü SKU.|
|sürüm|Dize|Görüntü sürümü.|

###  <a name="bk_winconf"></a> windowsConfiguration

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|enableAutomaticUpdates|Boole|Sanal makine otomatik güncelleştirmeler için etkinleştirilip etkinleştirilmediğini gösterir. Bu özellik belirtilmezse, varsayılan değer true olur.|

###  <a name="bk_netconf"></a> networkConfiguration

|Öğe adı|Tür|Notlar|
|------------------|--------------|----------|
|subnetId|Dize|Havuzun işlem düğümleri oluşturulduğu alt kaynak tanımlayıcısını belirtir.|
