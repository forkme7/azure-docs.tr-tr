---
title: include dosyası
description: include dosyası
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: cff737b8c79c44494cb0151d6a6281550401b26e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
## <a name="scenario"></a>Senaryo

VNet ve alt ağları oluşturma göstermek için bu belge aşağıdaki senaryoyu kullanır:

![VNet senaryosu](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

Bu senaryoda, bir VNet oluşturma **TestVNet**, ayrılmış CIDR bloğu ile **192.168.0.0./16**. VNet aşağıdaki alt ağlar içerir: 

* CIDR bloğu olarak **192.168.1.0/24** kullanan **Ön Uç**.
* CIDR bloğu olarak **192.168.2.0/24** kullanan **rka Uç**.

