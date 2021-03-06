---
title: Windows VM görüntüleri seçin | Microsoft Docs
description: Yayımcı, teklif, SKU ve sürümü Market VM görüntüleri belirlemek için Azure PowerShell kullanmayı öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/28/2018
ms.author: danlep
ms.openlocfilehash: 858b5976ef422ab410f42c44f94473aa4fd10970
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="how-to-find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Azure PowerShell ile Azure Market'te Windows VM görüntüleri bulma

Bu makalede, Azure Marketi'nde VM görüntüleri bulmak için Azure PowerShell kullanmayı açıklar. Bir VM PowerShell ile programlı olarak oluşturduğunuzda bir Market görüntüsü belirtmek için bu bilgileri kullanın Resource Manager şablonları veya diğer araçlar.

Yüklü ve en son yapılandırıldığından emin olun [Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps).

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="table-of-commonly-used-windows-images"></a>Yaygın olarak kullanılan Windows görüntülerinin tablosu
| Yayımcı | Sunduğu | Sku |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-Server-Core |
| MicrosoftWindowsServer |WindowsServer |Kapsayıcılar ile 2016 Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016 Nano Server |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2008-R2-SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2016-WS2016 |Enterprise |
| MicrosoftSQLServer |SQL2014SP2-WS2012R2 |Enterprise |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |

## <a name="navigate-the-images"></a>Görüntüleri gidin

Görüntüyü bir konumda bulmak için başka bir yolu çalıştırmaktır [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), ve [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) dizisi cmdlet'leri. Bu komutları ile bu değerler belirler:

1. Görüntü yayımcılarını listeleyin.
2. Belirli bir yayımcı varsa yayımcının tekliflerini listeleyin.
3. Belirli bir teklif varsa SKU’larını listeleyin.

Ardından, seçilen bir SKU için çalıştırın [Get-AzureRMVMImage](/powershell/module/azurerm.compute/get-azurermvmimage) dağıtmak için sürümleri listelemek için.

İlk olarak aşağıdaki komutlarla yayımcıları listeleyin:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Seçtiğiniz yayımcı adını girin ve aşağıdaki komutları çalıştırın:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Seçtiğiniz teklif adını girin ve aşağıdaki komutları çalıştırın:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Seçilen SKU adınızı doldurun ve aşağıdaki komutları çalıştırın:

```powershell
$skuName="<SKU>"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

Çıktısından `Get-AzureRMVMImage` komutu, yeni bir sanal makine dağıtmak için bir sürümü görüntüsü seçebilirsiniz.

Aşağıdaki komutlar, tam bir örnek göstermektedir:

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

Çıktı:

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

İçin *MicrosoftWindowsServer* yayımcı:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Çıktı:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServerSemiAnnual
```

İçin *Windows Server* sunar:

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Çıktı:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Datacenter-with-RDSH
2016-Nano-Server
```

Sonra *2016 Datacenter* SKU:

```powershell
$skuName="2016-Datacenter"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

Seçilen yayımcı, teklif, SKU ve sürümü bir URN birleştirebilirsiniz artık (değerleri ayrılmış:). Bu URN ile geçirmek `--image` bir VM ile oluşturduğunuzda parametresi [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet'i. "Son" URN sürüm numarası isteğe bağlı olarak değiştirebilirsiniz unutmayın. Bu her zaman en son sürümünü görüntü sürümüdür. URN ile de kullanabileceğiniz [kümesi AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell cmdlet'i. 

Resource Manager şablonu ile bir VM'yi dağıtmak istiyorsanız, görüntü parametreleri ayrı ayrı olarak ayarlayın `imageReference` özellikleri. Bkz: [şablon başvurusu](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]


### <a name="view-plan-properties"></a>Planın özelliklerini görüntüle

Görüntünün satın alma planı bilgilerini görüntülemek için Çalıştır `Get-AzureRMVMImage` cmdlet'i. Varsa `PurchasePlan` özelliği çıkışı değil `null`, koşulları resimle programlı dağıtım öncesinde kabul etmeniz gerekir.  

Örneğin, Windows Server 2016 Datacenter görüntüsü ek koşullar, çünkü yok `PurchasePlan` bilgileri `null`:

```powershell
$version = "2016.127.20170406"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Skus $skuName -Version $version
```

Çıktı:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/
                   Versions/2016.127.20170406
Location         : westus
PublisherName    : MicrosoftWindowsServer
Offer            : WindowsServer
Skus             : 2016-Datacenter
Version          : 2016.127.20170406
FilterExpression :
Name             : 2016.127.20170406
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : null
DataDiskImages   : []

```

Aşağıdaki Windows 2016 görüntü gösterir veri bilimi sanal makine için - benzer bir komut çalıştıran `PurchasePlan` özellikleri: `name`, `product`, ve `publisher`. (Bazı görüntüleri de bir `promotion code` özelliğini.) Bu görüntü dağıtmak için koşulları kabul edin ve programlı dağıtımı etkinleştirmek için aşağıdaki bölümlere bakın.

```powershell
Get-AzureRMVMImage -Location "westus" -Publisher "microsoft-ads" -Offer "windows-data-science-vm" -Skus "windows2016" -Version "0.2.02"
```

Çıktı:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/microsoft-ads/ArtifactTypes/VMIma
                   ge/Offers/windows-data-science-vm/Skus/windows2016/Versions/0.2.02
Location         : westus
PublisherName    : microsoft-ads
Offer            : windows-data-science-vm
Skus             : windows2016
Version          : 0.2.02
FilterExpression :
Name             : 0.2.02
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : {
                     "publisher": "microsoft-ads",
                     "name": "windows2016",
                     "product": "windows-data-science-vm"
                   }
DataDiskImages   : []

```

### <a name="accept-the-terms"></a>Koşulları kabul et

Lisans koşullarını görüntülemek için kullanın [Get-AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/get-azurermmarketplaceterms) cmdlet'i ve satın alma geçişinde planlama parametreleri. Çıkış koşullarını Market görüntüsü için bir bağlantı sağlar ve koşulları önceden kabul olup olmadığını gösterir. Örneğin:

```powershell
Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"
```

Çıktı:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : 2UMWH6PHSAIM4U22HXPXW25AL2NHUJ7Y7GRV27EBL6SUIDURGMYG6IIDO3P47FFIBBDFHZHSQTR7PNK6VIIRYJRQ3WXSE6BTNUNENXA
Accepted          : False
Signdate          : 2/23/2018 7:43:00 PM
```

Kullanım [kümesi AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/set-azurermmarketplaceterms) kabul edin veya koşulları reddetmek için cmdlet. Yalnızca görüntü için her abonelik için bir kez koşullarını kabul etmeniz gerekir. Örneğin:

```powershell

$agreementTerms=Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"

Set-AzureRmMarketplaceTerms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016" -Terms $agreementTerms -Accept

```

Çıktı:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : VNMTRJK3MNJ5SROEG2BYDA2YGECU33GXTD3UFPLPC4BAVKAUL3PDYL3KBKBLG4ZCDJZVNSA7KJWTGMDSYDD6KRLV3LV274DLBJSS4GQ
Accepted          : True
Signdate          : 2/23/2018 7:49:31 PM
```

### <a name="deploy-using-purchase-plan-parameters"></a>Satın alma planı parametrelerini kullanarak dağıtma
Görüntü için koşulları kabul ettikten sonra Abonelikteki bir VM dağıtabilirsiniz. Aşağıdaki kod parçacığında gösterildiği gibi kullanın [kümesi AzureRmVMPlan](/powershell/module/azurerm.compute/set-azurermvmplan) VM nesnesini Market planı bilgilerini ayarlamak için cmdlet. VM ağ ayarları oluşturmak ve dağıtımını tamamlamak bir tam komut dosyası için bkz: [PowerShell komut dosyası örnekleri](powershell-samples.md).

```powershell
...
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize Standard_D1

# Set the Marketplace plan information
$vmConfig = Set-AzureRmVMPlan -VM $vmConfig -Publisher "imagePlanPublisher" -Product "imagePlanProduct" -Name "imagePlanName"

$cred=Get-Credential

$vmConfig = Set-AzureRmVMOperatingSystem -Windows -VM $vmConfig -ComputerName "myVM" -Credential $cred

# Set the Marketplace image
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "imagePublisher" -Offer "imageOffer" -Skus "imageSku" -Version "imageVersion"
...
```
Ardından ağ yapılandırma nesneleriyle birlikte VM yapılandırma geçişi `New-AzureRmVM` cmdlet'i.

## <a name="next-steps"></a>Sonraki adımlar
Hızlı bir şekilde bir sanal makine oluşturmak için `New-AzureRmVM` temel görüntü bilgileri kullanarak, bkz: [PowerShell ile Windows sanal makine oluşturma](quick-create-powershell.md).

Bir PowerShell komut dosyası örneği için bkz: [tam olarak yapılandırılmış bir sanal makine oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-vm.md).


