---
title: Azure Linux VM boyutları - GPU | Microsoft Docs
description: Listeleri farklı GPU Azure Linux sanal makineler için kullanılabilir boyutları en iyi duruma getirilmiş. Vcpu, veri diskleri ve NIC yanı sıra bu serideki boyutları için depolama üretilen iş ve ağ bant sayısı hakkında bilgi listeler.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jonbeck
ms.openlocfilehash: c4704dd461ae96600fa812fdfe8d9b0e59e93d72
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>Sanal makine boyutlarını GPU en iyi duruma getirilmiş

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

Sürücü yükleme ve doğrulama adımları için bkz [N-serisi sürücü kurulumu Linux için](n-series-driver-setup.md).

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* X yüklemek döndürmemelidir sunucu veya kullanan diğer sistemler `Nouveau` Ubuntu NC vm'lerde sürücü. NVIDIA GPU sürücüleri yüklemeden önce devre dışı bırakmanız gerekir `Nouveau` sürücü.  

## <a name="other-sizes"></a>Diğer boyutlara
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [Yüksek performanslı işlem](sizes-hpc.md)

## <a name="next-steps"></a>Sonraki adımlar
Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları üzerinde işlem performans karşılaştırmanıza yardımcı olur.