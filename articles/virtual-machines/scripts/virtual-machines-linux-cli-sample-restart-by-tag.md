---
title: Azure CLI Betik Örneği - VM’leri yeniden başlatma | Microsoft Docs
description: Azure CLI Betik Örneği - VM’leri etikete ve kimliğe göre yeniden başlatma
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: a9f7cf8ba492004cb6d9e359bfb392448dfbe813
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="restart-vms"></a>VM’leri yeniden başlatma

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Bu örnekte bazı VM’leri alma ve yeniden başlatmanın birkaç yolu gösterilmektedir.

Birinci yol, kaynak grubundaki tüm VM’leri yeniden başlatır.

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

İkinci yol ise `az resouce list` kullanarak etiketlenmiş VM’leri alır ve VM olan kaynaklarla filtreleyip bu VM’leri yeniden başlatır.

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

Bu örnek bir Bash kabuğunda çalışır. Windows istemcisi üzerinde Azure CLI betikleri çalıştırma seçenekleri için bkz. [Windows’da Azure CLI çalıştırma](../windows/cli-options.md).


## <a name="sample-script"></a>Örnek betik

Örnek, üç betik içerir.
Birincisi sanal makineleri hazırlar.
Komutun hazırlanacak her bir VM’yi beklemeden döndürmesi için no-wait seçeneğini kullanır.
İkincisi VM'lerin tam olarak hazırlanmasını bekler.
Üçüncü betik hazırlanan tüm VM’leri ve sonra yalnızca etiketli VM'leri yeniden başlatır.

### <a name="provision-the-vms"></a>VM’leri hazırlama

Bu betik bir kaynak grubu oluşturur ve sonra yeniden başlatılacak üç VM oluşturur.
İki tanesi etiketlenir.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision the VMs")]

### <a name="wait"></a>Wait

Bu betik, üç VM’nin tamamı hazırlanana kadar veya bir tanesi hazırlamada başarısız olana kadar 20 saniyede bir hazırlama durumunu denetler.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for the VMs to be provisioned")]

### <a name="restart-the-vms"></a>VM’leri yeniden başlatma

Bu betik, kaynak grubundaki tüm VM’leri ve sonra yalnızca etiketli VM'leri yeniden başlatır.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Betik örneği çalıştırıldıktan sonra kaynak gruplarını, VM’leri ve ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, sanal makine, kullanılabilirlik kümesi, yük dengeleyici ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Sanal makineleri oluşturur.  |
| [az vm list](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | VM’lerin yeniden başlatılmadan önce hazırlandığından emin olmak ve sonra VM’leri yeniden başlatmak için kimliklerini almak amacıyla `--query` ile birlikte kullanılır. |
| [az resource list](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Etiket kullanan VM’lerin kimliklerini almak için `--query` ile birlikte kullanılır. |
| [az vm restart](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | VM’leri yeniden başlatır. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
