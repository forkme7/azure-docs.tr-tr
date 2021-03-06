---
title: "Şablonları Azure yığını için geliştirme | Microsoft Docs"
description: "Azure yığın şablon en iyi yöntemleri öğrenin"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 8a5bc713-6f51-49c8-aeed-6ced0145e07b
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: bbd6cc68f1c16d48380cf498d6b089abe923e95a
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="azure-resource-manager-template-considerations"></a>Azure Resource Manager şablonu konuları

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Uygulamanızı geliştirirken şablon taşınabilirlik Azure ve Azure yığın arasında olmak önemlidir. Bu makalede Azure Resource Manager geliştirme için Değerlendirmeler sağlar [şablonları](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf), uygulama ve test dağıtımınızda Azure Azure yığın ortam erişim olmadan prototip olabilir.

## <a name="resource-provider-availability"></a>Kaynak sağlayıcı kullanılabilirliği
Dağıtmayı planlayan şablon yalnızca Azure yığınında önizlemede veya kullanılabilir zaten Microsoft Azure hizmetlerini kullanmanız gerekir.

## <a name="public-namespaces"></a>Ortak ad alanları
Azure yığını, veri merkezinizde barındırıldığından, farklı hizmet uç noktası ad alanlarını Azure genel bulutunda vardır. Sonuç olarak, Azure yığınına dağıtmaya çalıştığınızda sabit kodlanmış ortak uç noktalardan Azure Resource Manager şablonları başarısız. Bunun yerine, kullanabileceğiniz *başvuru* ve *birleştirme* işlevi dinamik olarak değerlerine göre hizmet uç noktası oluşturmak için dağıtım sırasında kaynak Sağlayıcısı'ndan alınan. Örneğin, belirtme yerine *blob.core.windows.net* şablonunuzda, almak [primaryEndpoints.blob](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/101-simple-windows-vm/azuredeploy.json#L201) dinamik olarak ayarlamak için *osDisk.URI* uç noktası:

     "osDisk": {"name": "osdisk","vhd": {"uri": 
     "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2015-06-15').primaryEndpoints.blob, variables('vmStorageAccountContainerName'),
      '/',variables('OSDiskName'),'.vhd')]"}}

## <a name="api-versioning"></a>API sürümü oluşturma
Azure hizmet sürümleri, Azure ve Azure yığın arasında farklılık gösterebilir. Her kaynak sunulan yetenekleri tanımlayan apiVersion özniteliği gerektirir. Azure yığınında API sürümü uyumluluğundan emin olmak için her kaynak sağlayıcısı için geçerli API sürümleri şunlardır:

| Kaynak Sağlayıcısı | apiVersion |
| --- | --- |
| İşlem |`'2015-06-15'` |
| Ağ |`'2015-06-15'`, `'2015-05-01-preview'` |
| Depolama |`'2016-01-01'`, `'2015-06-15'`, `'2015-05-01-preview'` |
| KeyVault | `'2015-06-01'` |
| App Service |`'2015-08-01'` |

## <a name="template-functions"></a>Şablon işlevleri
Azure Resource Manager [işlevleri](../../azure-resource-manager/resource-group-template-functions.md) dinamik şablonları oluşturmak için gerekli özellikleri sağlar. Örnek olarak, gibi görevler için işlevleri kullanabilirsiniz:

* Birleştirme veya dizeleri kırpma 
* Diğer kaynaklardan başvuru değeri
* Birden çok örneği dağıtma kaynakları yineleme 

Bu işlevler Azure yığınında kullanılabilir değil:

* Atla
* Al

## <a name="resource-location"></a>Kaynak konumu
Azure Resource Manager şablonları konum özniteliği dağıtım sırasında kaynaklara yerleştirmek için kullanın. Azure'da, Batı ABD veya Güney Amerika gibi bir bölgesi Konumlar bakın. Azure yığınında konumları Azure yığın, veri merkezinizde olduğundan farklıdır. Şablonları Azure ve Azure yığın arasında aktarılabilir olduğundan emin olmak için kaynakların dağıtırken kaynak grubu konumu başvuruda bulunmalıdır. Kullanarak bunu yapabilirsiniz `[resourceGroup().Location]` tüm kaynakları kaynak grubu konumu devral emin olmak için. Aşağıdaki alıntı bir depolama hesabını dağıtırken bu işlevini kullanarak, bir örnek verilmiştir:

    "resources": [
    {
      "name": "[variables('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "[variables('apiVersionStorage')]",
      "location": "[resourceGroup().location]",
      "comments": "This storage account is used to store the VM disks",
      "properties": {
      "accountType": "Standard_GRS"
      }
    }
    ]

## <a name="next-steps"></a>Sonraki adımlar
* [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
* [Azure CLI ile şablonlarını dağıtma](azure-stack-deploy-template-command-line.md)
* [Şablonları Visual Studio ile dağıtma](azure-stack-deploy-template-visual-studio.md)

