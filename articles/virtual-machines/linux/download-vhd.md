---
title: Linux VHD Azure'dan karşıdan | Microsoft Docs
description: Azure CLI ve Azure portalını kullanarak bir Linux VHD indirin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: cynthn
ms.openlocfilehash: d62fe702e42929fde2bb2a0dee4ea2cc8621ff99
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="download-a-linux-vhd-from-azure"></a>Azure'dan Linux VHD indirin

Bu makalede, nasıl yükleneceği hakkında bilgi edineceksiniz bir [Linux sanal sabit disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Azure CLI ve Azure portalını kullanarak Azure dosyasından. 

Sanal makineler (VM'ler) Azure kullanımda [diskleri](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bir işletim sistemini, uygulamaları ve verileri depolamak için bir yer olarak. Tüm Azure VM'ler en az iki disk – bir Windows işletim sistemi diski ve geçici bir diski var. İşletim sistemi diski başlangıçta görüntüden oluşturulur ve hem işletim sistemi diski ve görüntünün VHD'leri bir Azure depolama hesabında depolanır. Sanal makineler ayrıca VHD'ler olarak da depolanan bir veya daha fazla veri diski olabilir.

Zaten yapmadıysanız, yükleme [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="stop-the-vm"></a>VM’yi durdurma

Çalışan bir VM bağlıysa VHD Azure'dan indirilemiyor. Bir VHD yüklemek için VM durdurmanız gerekir. Bir VHD'yi kullanılabilir olarak kullanmak istiyorsanız bir [görüntü](tutorial-custom-images.md) diğer VM'ler ile yeni diskler oluşturmak için yetkisini kaldırma ve dosyada bulunan işletim sistemini genelleştirir ve VM'yi durdurun gerekir. VHD'yi yeni bir örneğini bir var olan VM veya veri diski için disk olarak kullanmak için yalnızca durdurun ve VM ayırması gerekir.

VHD diğer sanal makineleri oluşturmak için bir resim olarak kullanmak için aşağıdaki adımları tamamlayın:

1. SSH, hesap adını ve VM genel IP adresi bağlanmak ve bu yetkisini kaldırma için kullanın. Ortak IP adresiyle bulabilirsiniz [az ağ ortak IP Göster](https://docs.microsoft.com/en-us/cli/azure/network/public-ip#az-network-public-ip-show). + Kullanıcı parametresi son sağlanan kullanıcı hesabının da kaldırır. Hesap kimlik bilgilerini VM Fırında pişirme bu bırakın + kullanıcı parametresi. Aşağıdaki örnek, son sağlanan kullanıcı hesabını kaldırır:

    ```bash
    ssh azureuser@<publicIpAddress>
    sudo waagent -deprovision+user -force
    exit 
    ```

2. Azure hesabınızda oturum açın [az oturum açma](https://docs.microsoft.com/cli/azure/reference-index#az_login).
3. Durdurun ve VM serbest bırakma.

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. VM genelleştirin. 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

VHD'yi yeni bir örneğini bir var olan VM veya veri diski için disk olarak kullanmak için aşağıdaki adımları tamamlayın:

1.  [Azure Portal](https://portal.azure.com/) oturum açın.
2.  Hub menüsünde, **Virtual Machines**’e tıklayın.
3.  VM listeden seçin.
4.  VM için dikey penceresinde **durdurmak**.

    ![VM'yi Durdur](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>SAS URL oluştur

VHD dosyasını indirmek için oluşturmak gereken bir [paylaşılan erişim imzası (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL. URL oluşturulduğunda, sona erme süresi URL atanır.

1.  VM için dikey pencerenin menüsünde **diskleri**.
2.  VM için işletim sistemi diski seçin ve ardından **verme**.
3.  Tıklatın **URL'yi oluşturmak**.

    ![URL'yi oluşturmak](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a>VHD indirin

1.  Oluşturulan URL altında VHD dosyasını yükle'yi tıklatın.

    ![VHD indirin](./media/download-vhd/export-download.png)

2.  ' İ tıklatmanız gerekir **kaydetmek** karşıdan yüklemeyi başlatmak için tarayıcıda. VHD dosyası için varsayılan ad *abcd*.

    ![Tarayıcıda Kaydet'e tıklayın.](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [karşıya yükleyin ve Azure CLI 2.0 ile özel diskten bir Linux VM oluşturma](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- [Azure diskleri Azure CLI yönetmek](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

