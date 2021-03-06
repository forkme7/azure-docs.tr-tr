---
title: Windows Azure VM görüntülerini ile Packer oluşturma | Microsoft Docs
description: Azure'da Windows sanal makine görüntülerini oluşturmak için Packer kullanmayı öğrenin
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/29/2018
ms.author: iainfou
ms.openlocfilehash: f174837b8d370ffabdf4148b18d3425d9f3d9f10
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-use-packer-to-create-windows-virtual-machine-images-in-azure"></a>Azure'da Windows sanal makine görüntülerini oluşturmak için Packer kullanma
Azure her sanal makine (VM) Windows Dağıtım ve işletim sistemi sürümü tanımlayan bir görüntüden oluşturulur. Görüntüleri, önceden yüklenmiş uygulamalar ve yapılandırmalar içerebilir. Azure Market birçok ilk ve üçüncü taraf en yaygın işletim sistemi için sağlar ve uygulama ortamları veya gereksinimlerinize göre tasarlanmıştır, kendi özel görüntülerinizi oluşturabilirsiniz. Bu makalede açık kaynak aracının nasıl kullanılacağını ayrıntıları [Packer](https://www.packer.io/) tanımlamak ve Azure özel görüntülerinizi oluşturmak için.


## <a name="create-azure-resource-group"></a>Azure kaynak grubu oluşturun
Kaynak VM oluştururken oluşturma işlemi sırasında geçici Azure kaynaklarını Packer oluşturur. Bir görüntü olarak kullanmak için bu kaynak VM yakalamak için bir kaynak grubu tanımlamanız gerekir. Çıktısı Packer oluşturma işlemi, bu kaynak grubunda depolanır.

Bir kaynak grubu ile oluşturmak [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```powershell
$rgName = "myResourceGroup"
$location = "East US"
New-AzureRmResourceGroup -Name $rgName -Location $location
```

## <a name="create-azure-credentials"></a>Azure kimlik bilgileri oluşturun
Packer bir hizmet sorumlusu kullanarak Azure ile kimliğini doğrular. Bir Azure hizmet sorumlusu uygulamaları, hizmetleri ve Packer gibi Otomasyon araçları ile birlikte kullanabileceğiniz bir güvenlik kimliğidir. Denetim ve hizmet sorumlusu Azure'da gerçekleştirebilirsiniz ne gibi işlemler için izinler tanımlar.

Bir hizmet sorumlusu ile oluşturma [yeni AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) ve hizmet sorumlusu oluşturmak ve kaynakları yönetmek izinleri atayın [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```powershell
$sp = New-AzureRmADServicePrincipal -DisplayName "Azure Packer" `
    -Password (ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force)
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Azure için kimlik doğrulaması için Azure Kiracı ve abonelik kimlikleri ile elde etmeniz [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription):

```powershell
$sub = Get-AzureRmSubscription
$sub.TenantId[0]
$sub.SubscriptionId[0]
```

Sonraki adımda bu iki kimlikleri kullanın.


## <a name="define-packer-template"></a>Packer şablon oluştur
Görüntüleri oluşturmak için bir şablon bir JSON dosyası oluşturun. Şablonda oluşturucular ve gerçek derleme işlemini yürütmek provisioners tanımlayın. Packer sahip bir [Azure için Oluşturucusu](https://www.packer.io/docs/builders/azure.html) sağlayan Azure kaynaklarını tanımlamak önceki oluşturulan hizmet asıl kimlik bilgilerini adım gibi.

Adlı bir dosya oluşturun *windows.json* ve aşağıdaki içeriği yapıştırın. Aşağıdakiler için kendi değerlerinizi girin:

| Parametre                           | Nereden |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | Görünüm hizmet asıl kimliği ile `$sp.applicationId` |
| *client_secret*                     | Belirttiğiniz parola `$securePassword` |
| *tenant_id*                         | Çıktı `$sub.TenantId` komutu |
| *subscription_id*                   | Çıktı `$sub.SubscriptionId` komutu |
| *object_id*                         | Görünüm hizmet asıl nesne kimliği ile `$sp.Id` |
| *managed_image_resource_group_name* | İlk adımda oluşturduğunuz kaynak grubunun adı |
| *managed_image_name*                | Oluşturulan yönetilen disk görüntüsü için adı |

```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "0831b578-8ab6-40b9-a581-9a880a94aab1",
    "client_secret": "P@ssw0rd!",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "object_id": "a7dfb070-0d5b-47ac-b9a5-cf214fff0ae2",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Windows",
    "image_publisher": "MicrosoftWindowsServer",
    "image_offer": "WindowsServer",
    "image_sku": "2016-Datacenter",

    "communicator": "winrm",
    "winrm_use_ssl": "true",
    "winrm_insecure": "true",
    "winrm_timeout": "3m",
    "winrm_username": "packer",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "type": "powershell",
    "inline": [
      "Add-WindowsFeature Web-Server",
      "& $env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /quiet /quit",
      "while($true) { $imageState = Get-ItemProperty HKLM:\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Setup\\State | Select ImageState; if($imageState.ImageState -ne 'IMAGE_STATE_GENERALIZE_RESEAL_TO_OOBE') { Write-Output $imageState.ImageState; Start-Sleep -s 10  } else { break } }"
    ]
  }]
}
```

Bu şablon Windows Server 2016 VM oluşturur, IIS yükler ve sonra Sysprep ile VM genelleştirir. IIS Yükleme ek komutları çalıştırmak için PowerShell sağlayıcısı nasıl kullanabileceğinizi gösterir. Son Packer görüntü sonra gerekli yazılım yükleme ve yapılandırma içerir.


## <a name="build-packer-image"></a>Packer yansıması oluştur
Yerel makinenizde yüklü Packer zaten yoksa [Packer yükleme yönergelerini izleyin](https://www.packer.io/docs/install/index.html).

Görüntü, Packer belirterek şablon dosyası gibi oluşturun:

```bash
./packer build windows.json
```

Çıkış örneği önceki komutlarındaki aşağıdaki gibidir:

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> task : Image deployment
==> azure-arm:  ->> dept : Engineering
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the certificate’s URL ...
==> azure-arm:  -> Key Vault Name        : ‘pkrkvpq0mthtbtt’
==> azure-arm:  -> Key Vault Secret Name : ‘packerKeyVaultSecret’
==> azure-arm:  -> Certificate URL       : ‘https://pkrkvpq0mthtbtt.vault.azure.net/secrets/packerKeyVaultSecret/8c7bd823e4fa44e1abb747636128adbb'
==> azure-arm: Setting the certificate’s URL ...
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.55.35’
==> azure-arm: Waiting for WinRM to become available...
==> azure-arm: Connected to WinRM!
==> azure-arm: Provisioning with Powershell...
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-powershell-provisioner902510110
    azure-arm: #< CLIXML
    azure-arm:
    azure-arm: Success Restart Needed Exit Code      Feature Result
    azure-arm: ------- -------------- ---------      --------------
    azure-arm: True    No             Success        {Common HTTP Features, Default Document, D...
    azure-arm: <Objs Version=“1.1.0.1” xmlns=“http://schemas.microsoft.com/powershell/2004/04"><Obj S=“progress” RefId=“0"><TN RefId=“0”><T>System.Management.Automation.PSCustomObject</T><T>System.Object</T></TN><MS><I64 N=“SourceId”>1</I64><PR N=“Record”><AV>Preparing modules for first use.</AV><AI>0</AI><Nil /><PI>-1</PI><PC>-1</PC><T>Completed</T><SR>-1</SR><SD> </SD></PR></MS></Obj></Objs>
==> azure-arm: Querying the machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-pq0mthtbtt/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Compute Name              : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```

VM oluşturmak için provisioners çalıştırıp dağıtım temizlemek Packer birkaç dakika sürer.


## <a name="create-a-vm-from-the-packer-image"></a>Packer görüntüden bir VM oluşturma
Artık bir VM ile görüntüden oluşturabilirsiniz [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Zaten mevcut destekleyen ağ kaynaklarına oluşturulur. İstendiğinde, bir yönetici kullanıcı adı ve parola VM oluşturulacak girin. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVM* gelen *myPackerImage*:

```powershell
New-AzureRmVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80 `
    -Image "myPackerImage"
```

VM Packer görüntünüzü oluşturmak için birkaç dakika sürer.


## <a name="test-vm-and-webserver"></a>Test VM ve Web sunucusu
[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ile VM’nizin genel IP adresini alın. Aşağıdaki örnek, daha önce oluşturulan *myPublicIP* için IP adresini alır:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName $rgName `
    -Name "myPublicIPAddress" | select "IpAddress"
```

Packer sağlayıcısı IIS yükle eylemde içeren, VM'nizi görmek için bir web tarayıcısı ortak IP adresini girin.

![Varsayılan IIS sitesi](./media/build-image-with-packer/iis.png) 


## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, Packer IIS zaten yüklü bir VM görüntüsü oluşturmak için kullanılır. Varolan dağıtım iş akışları, yanı sıra bu VM görüntüsü gibi Team Services, Ansible, Chef veya Puppet görüntüden oluşturulan VM'ler için uygulamanızı dağıtmak için kullanabilirsiniz.

Diğer Windows distro'lar için ek örnek Packer şablonları için bkz: [bu GitHub deposuna](https://github.com/hashicorp/packer/tree/master/examples/azure).
