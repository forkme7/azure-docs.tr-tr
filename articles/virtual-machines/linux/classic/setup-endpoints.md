---
title: Klasik bir Linux VM üzerindeki uç noktaları ayarlama | Microsoft Docs
description: Azure'da bir Linux sanal makineyle iletişim sağlamak için Azure portalında bir Linux VM için uç noktaları ayarlama hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: f3749738-1109-4a1d-8635-40e4bd220e91
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: 03cb90dcdcc7a7407898ddd8cbc30f1af0833676
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Bir Linux Klasik sanal makinede Azure uç noktaları kurma
Azure'da Klasik dağıtım modeli kullanarak oluşturduğunuz tüm Linux sanal makineleri otomatik olarak bir özel ağ kanalı diğer sanal makinelerle aynı bulut hizmetinde veya sanal ağ üzerinden iletişim kurabilir. Ancak, Internet veya diğer sanal ağlardaki bilgisayarlara bir sanal makineye gelen ağ trafiğini yönlendirmek için uç noktalar gerektirir. Bu makalede ayrıca kullanılabilir [Windows sanal makineleri](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

İçinde **Resource Manager** dağıtım modeli, uç noktalar kullanılarak yapılandırılmış olan **ağ güvenlik grupları (Nsg'ler)**. Daha fazla bilgi için bkz: [bağlantı noktalarını ve uç noktaları açmadan](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure portalında bir Linux sanal makine oluşturduğunuzda, bir uç nokta için güvenli Kabuk (SSH) genellikle sizin için otomatik olarak oluşturulur. Sanal makine oluşturulurken veya daha sonra gerektikçe ek uç noktaları yapılandırabilirsiniz.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Kullanarak bir VM uç noktası oluşturabilirsiniz [Azure komut satırı arabirimi](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Çalıştırma **azure vm uç noktası oluşturma** komutu.
* Resource Manager dağıtım modelinde bir sanal makine oluşturduysanız, Azure CLI Resource Manager moduna kullanabileceğiniz [ağ güvenlik grupları oluşturma](../../../virtual-network/tutorial-filter-network-traffic-cli.md) VM denetim trafiği için.
