---
title: Öğretici - Azure PowerShell ile ölçek kümeleri için diskler oluşturma ve kullanma | Microsoft Docs
description: Disk ekleme, hazırlama, listeleme ve ayırma gibi, Azure PowerShell kullanılarak sanal makine ölçek kümeleri ile Yönetilen Diskler oluşturma ve kullanma işlemlerinin nasıl yapılacağını öğrenin.
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: fd28b7e1f7407b1d1ee08c2f5774d939852e57b5
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="tutorial-create-and-use-disks-with-virtual-machine-scale-set-with-azure-powershell"></a>Öğretici: Azure PowerShell ile sanal makine ölçek kümesi içeren diskler oluşturma ve kullanma
Sanal makine ölçek kümeleri, sanal makine örneğinin işletim sistemini, uygulamalarını ve verilerini depolamak için diskleri kullanır. Bir ölçek kümesi oluştururken ve yönetirken, beklenen iş yüküne uygun disk boyutu ve yapılandırmasını seçmek önemlidir. Bu öğretici, sanal makine disklerinin oluşturulmasını ve yönetilmesini kapsar. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * İşletim sistemi diskleri ve geçici diskler
> * Veri diskleri
> * Standart ve Premium diskler
> * Disk performansı
> * Veri disklerini ekleme ve hazırlama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.6.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir. 


## <a name="default-azure-disks"></a>Varsayılan Azure diskleri
Bir ölçek kümesi oluşturulduğunda veya ölçeklendirildiğinde, her bir sanal makine örneğine otomatik olarak iki disk eklenir. 

**İşletim sistemi diski** - İşletim sistemi diskleri 2 TB’a kadar boyutlandırılabilir ve sanal makinenin işletim sistemini barındırır. İşletim sistemi diski varsayılan olarak */dev/sda* etiketine sahiptir. İşletim sistemi diskinin yapılandırmasını önbelleğe alan disk, işletim sistemi performansı için iyileştirilir. Bu yapılandırma nedeniyle işletim sistemi diski uygulama veya veri **barındırmamalıdır**. Uygulamalar ve veriler için, bu makalede daha sonra ayrıntılı olarak açıklanan veri disklerini kullanın. 

**Geçici disk** - Geçici diskler, sanal makine örneğiyle aynı Azure ana bilgisayarında bulunan bir katı hal sürücüsü kullanır. Bunlar yüksek performansa sahiptir ve geçici veri işleme gibi işlemler için kullanılabilir. Ancak sanal makine örneği yeni bir ana bilgisayara taşınırsa, geçici diskte depolanan tüm veriler kaldırılır. Geçici diskin boyutu, sanal makine örneği tarafından belirlenir. Geçici diskler */dev/sdb* etiketine ve */mnt* bağlama noktasına sahiptir.

### <a name="temporary-disk-sizes"></a>Geçici disk boyutları
| Tür | Ortak boyutlar | En yüksek geçici disk boyutu (GiB) |
|----|----|----|
| [Genel amaçlı](../virtual-machines/windows/sizes-general.md) | A, B ve D serisi | 1600 |
| [İşlem için iyileştirilmiş](../virtual-machines/windows/sizes-compute.md) | F serisi | 576 |
| [Bellek için iyileştirilmiş](../virtual-machines/windows/sizes-memory.md) | D, E, G ve M serisi | 6144 |
| [Depolama için iyileştirilmiş](../virtual-machines/windows/sizes-storage.md) | L serisi | 5630 |
| [GPU](../virtual-machines/windows/sizes-gpu.md) | N serisi | 1440 |
| [Yüksek performans](../virtual-machines/windows/sizes-hpc.md) | A ve H serisi | 2000 |


## <a name="azure-data-disks"></a>Azure veri diskleri
Uygulamalar yüklemeniz ve veri depolamanız gerekirse ek veri diskleri eklenebilir. Dayanıklı ve duyarlı veri depolama gerektiren her koşulda veri diskleri kullanılmalıdır. Her veri diski maksimum 4 TB kapasiteye sahiptir. Sanal makine örneğinin boyutu, kaç veri diskinin eklenebileceğini belirler. Her VM vCPU için iki veri diski eklenebilir.

### <a name="max-data-disks-per-vm"></a>VM başına en fazla veri diski
| Tür | Ortak boyutlar | VM başına en fazla veri diski |
|----|----|----|
| [Genel amaçlı](../virtual-machines/windows/sizes-general.md) | A, B ve D serisi | 64 |
| [İşlem için iyileştirilmiş](../virtual-machines/windows/sizes-compute.md) | F serisi | 64 |
| [Bellek için iyileştirilmiş](../virtual-machines/windows/sizes-memory.md) | D, E, G ve M serisi | 64 |
| [Depolama için iyileştirilmiş](../virtual-machines/windows/sizes-storage.md) | L serisi | 64 |
| [GPU](../virtual-machines/windows/sizes-gpu.md) | N serisi | 64 |
| [Yüksek performans](../virtual-machines/windows/sizes-hpc.md) | A ve H serisi | 64 |


## <a name="vm-disk-types"></a>VM disk türleri
Azure iki disk türü sunar.

### <a name="standard-disk"></a>Standart disk
Standart Depolama, HDD’ler ile desteklenir ve uygun maliyetli depolama ve yüksek performans sağlar. Standart diskler, uygun maliyetli bir geliştirme ve iş yükü testi için idealdir.

### <a name="premium-disk"></a>Premium disk
Premium diskler, SSD tabanlı, yüksek performanslı ve düşük gecikme süreli disk ile desteklenir. Üretim iş yüklerini çalıştıran sanal makineler için bu diskler önerilir. Premium Depolama, DS serisi, DSv2 serisi, GS serisi ve FS serisi VM'lerini destekler. Disk boyutu seçilirken boyutun değeri sonraki türe yuvarlanır. Örneğin disk boyutu 128 GB’den azsa disk türü P10’dur. Disk boyutu 129 GB ile 512 GB arasında ise boyut P20’dir. 512 GB’ın üstündeki diskler P30 boyutundadır.

### <a name="premium-disk-performance"></a>Premium disk performansı
|Premium depolama diski türü | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Disk boyutu (yuvarlanmış değer) | 32 GB | 64 GB | 128 GB | 512 GB | 1.024 GB (1 TB) | 2.048 GB (2 TB) | 4.095 GB (4 TB) |
| Disk başına en fazla IOPS | 120 | 240 | 500 | 2.300 | 5.000 | 7.500 | 7.500 |
Disk başına aktarım hızı | 25 MB/sn | 50 MB/sn | 100 MB/s | 150 MB/s | 200 MB/sn | 250 MB/sn | 250 MB/sn |

Yukarıdaki tabloda, disk başına maksimum IOPS tanımlanmış olsa da birden çok veri diski bölümlenerek daha yüksek performansa ulaşılabilir. Örneğin bir Standard_GS5 VM’si en fazla 80.000 IOPS’ye ulaşabilir. VM başına IOPS üst sınırı hakkında ayrıntılı bilgi için bkz. [Windows VM boyutları](../virtual-machines/windows/sizes.md).


## <a name="create-and-attach-disks"></a>Disk oluşturma ve ekleme
Bir ölçek kümesi oluştururken veya mevcut bir ölçek kümesi ile diskler oluşturabilir ve ekleyebilirsiniz.

### <a name="attach-disks-at-scale-set-creation"></a>Ölçek kümesi oluşturulurken diskler ekleme
[New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) ile bir sanal makine ölçek kümesi oluşturun. İstendiğinde, sanal makine örnekleri için bir kullanıcı adı ve parola sağlayın. Tek tek sanal makine örneklerine trafiği dağıtmak için bir yük dengeleyici de oluşturulur. Yük dengeleyici hem 80 numaralı TCP bağlantı noktasında trafiği dağıtmak hem de 3389 numaralı TCP bağlantı noktasında uzak masaüstü trafiğine ve 5985 numaralı TCP bağlantı noktasında PowerShell uzaktan iletişimine olanak tanımak için kurallar içerir.

`-DataDiskSizeGb` parametresiyle iki disk oluşturulur. İlk diskin boyutu *64* GB, ikinci diskin boyutuysa *128* GB’tır:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic" `
  -DataDiskSizeGb 64,128
```

Tüm ölçek kümesi kaynaklarının ve sanal makine örneklerinin oluşturulup yapılandırılması birkaç dakika sürer.

### <a name="attach-a-disk-to-existing-scale-set"></a>Mevcut ölçek kümesine bir disk ekleme
Mevcut bir ölçek kümesine de diskler ekleyebilirsiniz. [Add-AzureRmVmssDataDisk](/powershell/module/azurerm.compute/add-azurermvmssdatadisk) komutunu kullanarak başka bir disk eklemek için önceki adımda oluşturulan ölçek kümesini kullanın. Aşağıdaki örnekte, mevcut bir ölçek kümesine ek bir *128* GB disk eklenmektedir:

```azurepowershell-interactive
# Get scale set object
$vmss = Get-AzureRmVmss `
          -ResourceGroupName "myResourceGroup" `
          -VMScaleSetName "myScaleSet"

# Attach a 128 GB data disk to LUN 2
Add-AzureRmVmssDataDisk `
  -VirtualMachineScaleSet $vmss `
  -CreateOption Empty `
  -Lun 2 `
  -DiskSizeGB 128

# Update the scale set to apply the change
Update-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmss
```


## <a name="prepare-the-data-disks"></a>Veri disklerini hazırlama
Oluşturulan ve ölçek kümesi sanal makine örneklerinize eklenen diskler, ham disklerdir. Bunları veri ve uygulamalarınızla kullanabilmeniz için disklerin hazırlanması gerekir. Diskleri hazırlamak için bir bölüm oluşturun, bir dosya sistemi oluşturun ve bunları bağlayın.

Bir ölçek kümesindeki birden çok sanal makine örneğinde işlemi otomatikleştirmek için Azure Özel Betik Uzantısı’nı kullanabilirsiniz. Bu uzantı, örneğin, eklenen veri disklerini hazırlamak için her bir sanal makine örneğinde betikleri yerel olarak yürütebilir. Daha fazla bilgi için bkz. [Özel Betik Uzantısı'na genel bakış](../virtual-machines/windows/extensions-customscript.md).

Aşağıdaki örnek, tüm eklenen ham veri disklerini hazırlayan [Add-AzureRmVmssExtension](/powershell/module/AzureRM.Compute/Add-AzureRmVmssExtension) komutuyla her bir sanal makine örneğindeki GitHub örnek deposundan bir betik yürütür:

```azurepowershell-interactive
# Get scale set object
$vmss = Get-AzureRmVmss `
          -ResourceGroupName "myResourceGroup" `
          -VMScaleSetName "myScaleSet"

# Define the script for your Custom Script Extension to run
$publicSettings = @{
  "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.ps1");
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File prepare_vm_disks.ps1"
}

# Use Custom Script Extension to prepare the attached data disks
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.8 `
  -Setting $publicSettings

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmss
```

Disklerin düzgün şekilde hazırlandığını onaylamak için, sanal makine örneklerinden birinde RDP oturumu açın. 

İlk olarak [Get-AzureRmLoadBalancer](/powershell/module/AzureRM.Network/Get-AzureRmLoadBalancer) komutunu kullanarak yük dengeleyici nesnesini alın. Ardından, [Get-AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/AzureRM.Network/Get-AzureRmLoadBalancerInboundNatRuleConfig) komutuyla gelen NAT kurallarını görüntüleyin. NAT kuralları, RDP’nin dinlediği her sanal makine örneği için *FrontendPort* değerini listeler. Son olarak [Get-AzureRmPublicIpAddress](/powershell/module/AzureRM.Network/Get-AzureRmPublicIpAddress) komutunu kullanarak yük dengeleyicinin genel IP adresini alın:

```azurepowershell-interactive
# Get the load balancer object
$lb = Get-AzureRmLoadBalancer -ResourceGroupName "myResourceGroup" -Name "myLoadBalancer"

# View the list of inbound NAT rules
Get-AzureRmLoadBalancerInboundNatRuleConfig -LoadBalancer $lb | Select-Object Name,Protocol,FrontEndPort,BackEndPort

# View the public IP address of the load balancer
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" -Name myPublicIPAddress | Select IpAddress
```

Sanal makinenize bağlanmak için, önceki komutlarda gösterildiği gibi, gerekli sanal makine örneği için kendi genel IP adresinizi ve bağlantı noktası numaranızı belirtin. İstendiğinde, ölçek kümesini oluştururken kullandığınız kimlik bilgilerini girin. Azure Cloud Shell kullanıyorsanız, bu adımı yerel PowerShell isteminden veya Uzak Masaüstü İstemcisinden gerçekleştirin. Aşağıdaki örnek sanal makine örneği *1*'e bağlanır:

```powershell
mstsc /v 52.168.121.216:50001
```

Sanal makine örneğinde yerel bir PowerShell oturumunu açın ve [Get-Disk](/powershell/module/storage/get-disk) komutunu kullanarak bağlı disklere bakın:

```powershell
Get-Disk
```

Aşağıdaki örnek çıktı, sanal makine örneğine üç veri diskinin eklendiğini gösterir.

```powershell
Number  Friendly Name      HealthStatus  OperationalStatus  Total Size  Partition Style
------  -------------      ------------  -----------------  ----------  ---------------
0       Virtual HD         Healthy       Online                 127 GB  MBR
1       Virtual HD         Healthy       Online                  14 GB  MBR
2       Msft Virtual Disk  Healthy       Online                  64 GB  MBR
3       Msft Virtual Disk  Healthy       Online                 128 GB  MBR
4       Msft Virtual Disk  Healthy       Online                 128 GB  MBR
```

Aşağıdaki adımları uygulayarak, sanal makine örneğindeki dosya sistemlerini ve bağlama noktalarını inceleyin:

```powershell
Get-Partition
```

Aşağıdaki örnek çıktı, üç veri diskine sürücü harfleri atandığını gösterir:

```powershell
   DiskPath: \\?\scsi#disk&ven_msft&prod_virtual_disk#000001

PartitionNumber  DriveLetter  Offset   Size  Type
---------------  -----------  ------   ----  ----
1                F            1048576  64 GB  IFS

   DiskPath: \\?\scsi#disk&ven_msft&prod_virtual_disk#000002

PartitionNumber  DriveLetter  Offset   Size   Type
---------------  -----------  ------   ----   ----
1                G            1048576  128 GB  IFS

   DiskPath: \\?\scsi#disk&ven_msft&prod_virtual_disk#000003

PartitionNumber  DriveLetter  Offset   Size   Type
---------------  -----------  ------   ----   ----
1                H            1048576  128 GB  IFS
```

Ölçeğinizdeki her sanal makine örneğindeki diskler otomatik olarak aynı şekilde hazırlanır. Ölçek kümeniz yukarı ölçeklendirilirken, yeni sanal makine örneklerine gerekli veri diskleri eklenir. Diskleri otomatik olarak hazırlamak için Özel Betik Uzantısı da çalıştırılır.

Sanal makine örneğiyle uzak masaüstü bağlantısı oturumunu kapatın.


## <a name="list-attached-disks"></a>Eklenen diskleri listeleme
Bir ölçek kümesine bağlanan disklerle ilgili bilgileri görüntülemek için aşağıdaki gibi [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) komutunu kullanın:

```azurepowershell-interactive
Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet"
```

*VirtualMachineProfile.StorageProfile* özelliğinin altında, *DataDisks* listesi gösterilir. Disk boyutu, depolama katmanı ve LUN (Mantıksal Birim Numarası) ile ilgili bilgiler gösterilir. Aşağıdaki örnek çıktıda, ölçek kümesine eklenen üç veri diski açıklanmaktadır:

```powershell
DataDisks[0]                            :
  Lun                                   : 0
  Caching                               : None
  CreateOption                          : Empty
  DiskSizeGB                            : 64
  ManagedDisk                           :
    StorageAccountType                  : PremiumLRS
DataDisks[1]                            :
  Lun                                   : 1
  Caching                               : None
  CreateOption                          : Empty
  DiskSizeGB                            : 128
  ManagedDisk                           :
    StorageAccountType                  : PremiumLRS
DataDisks[2]                            :
  Lun                                   : 2
  Caching                               : None
  CreateOption                          : Empty
  DiskSizeGB                            : 128
  ManagedDisk                           :
    StorageAccountType                  : PremiumLRS
```


## <a name="detach-a-disk"></a>Disk ayırma
Belirli bir disk artık gerekli olmadığında o diski ölçek kümesinden ayırabilirsiniz. Disk, ölçek kümesindeki tüm sanal makine örneklerinden kaldırılır. Ölçek kümesinden bir diski ayırmak için [Remove-AzureRmVmssDataDisk](/powershell/module/azurerm.compute/remove-azurermvmssdatadisk) komutunu kullanın ve diskin LUN’unu belirtin. Önceki bölümde [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) komutundan elde edilen çıktıda LUN’lar gösterilir. Aşağıdaki örnek, ölçek kümesinden LUN *3*’ü ayırır:

```azurepowershell-interactive
# Get scale set object
$vmss = Get-AzureRmVmss `
          -ResourceGroupName "myResourceGroup" `
          -VMScaleSetName "myScaleSet"

# Detach a disk from the scale set
Remove-AzureRmVmssDataDisk `
  -VirtualMachineScaleSet $vmss `
  -Lun 2

# Update the scale set and detach the disk from the VM instances
Update-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmss
```


## <a name="clean-up-resources"></a>Kaynakları temizleme
Ölçek kümenizi ve disklerinizi kaldırmak için [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve bu kaynak grubunun tüm kaynaklarını silin. `-Force` parametresi kaynakları ek bir komut istemi olmadan silmek istediğinizi onaylar. `-AsJob` parametresi işlemin tamamlanmasını beklemeden denetimi komut istemine döndürür.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure PowerShell ile ölçek kümeleri içeren diskler oluşturma ve kullanma işleminin nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * İşletim sistemi diskleri ve geçici diskler
> * Veri diskleri
> * Standart ve Premium diskler
> * Disk performansı
> * Veri disklerini ekleme ve hazırlama

Ölçek kümesi sanal makine örnekleriniz için özel görüntünün nasıl kullanılacağını öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Ölçek kümesi sanal makine örnekleri için özel görüntü kullanma](tutorial-use-custom-image-powershell.md)
