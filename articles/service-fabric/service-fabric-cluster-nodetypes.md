---
title: Azure Service Fabric düğüm türleri ve sanal makine ölçekleme kümeleri | Microsoft Docs
description: Örneği veya küme düğümünde bir ölçek uzaktan bağlanma ayarlamak ve nasıl Azure Service Fabric düğüm türleri için sanal makine ölçek ilişkili ayarlar öğrenin.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: chackdan
ms.openlocfilehash: ce3b3e6f3c7ee0c7198c27ed0ca0e3610d26f143
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Service Fabric düğüm türleri ve sanal makine ölçekleme kümeleri
[Sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets) Azure hesaplama kaynağı olan. Ölçek kümeleri dağıtmak ve sanal makinelerin bir koleksiyon kümesi olarak yönetmek için kullanabilirsiniz. Bir Azure Service Fabric kümesi tanımladığınız her düğüm türü ayrı bir ölçek büyütme ayarlar.  Ölçek her sanal makinede yüklü Service Fabric çalışma zamanını ayarlayın. Bağımsız olarak her düğüm türü yukarı veya aşağı ölçeklendirmek, her küme düğümünde çalışan işletim sistemi SKU'su değişiklik kümesi farklı bağlantı noktalarının açık ve farklı kapasite ölçümlerini kullanın.

Aşağıdaki şekil, ön uç ve arka uç adlı iki düğüm türü olan bir küme gösterir. Her düğüm türü beş düğüm yok.

![İki düğüm türü olan bir küme][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Sanal makine ölçek kümesi örneklerinin düğümlerine eşleme
Yukarıdaki şekilde gösterildiği gibi ölçek kümesi örneklerinin 0 örneğinizi ve 1 ile artırın. Numaralandırma düğüm adı yansıtılır. Örneğin, düğüm BackEnd_0 0 arka uç ölçek kümesi örneğidir. Bu belirli ölçek kümesini BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 ve BackEnd_4 adlı beş örneğe sahip.

Ölçek ayarlama ölçeklendirdiğinizde yeni bir örneği oluşturulur. Yeni ölçek kümesi örnek adı genellikle adına ve sonraki örnek numarasını ölçek kümesi olur. Bizim örneğimizde, BackEnd_5 değil.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Harita ölçek düğüm türleri için yük dengeleyicisi ayarlayın ve kümeleri ölçeklendirme
Azure portalında kümenizi dağıtılan veya örnek Azure Resource Manager şablonu kullanılan tüm kaynaklar bir kaynak grubu altında listelenir. Her ölçek kümesi veya düğüm türü için yük Dengeleyiciler görebilirsiniz. Yük dengeleyicisi adı aşağıdaki biçimi kullanır: **LB -&lt;düğüm türü adı&gt;**. LB-sfcluster4doc-0, aşağıdaki çizimde gösterildiği gibi örneğidir:

![Kaynaklar][Resources]


## <a name="next-steps"></a>Sonraki adımlar
* Bkz: ["her yere dağıtma" özelliği ve Azure tarafından yönetilen kümeleriyle karşılaştırma genel bakış](service-fabric-deploy-anywhere.md).
* Hakkında bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
* [Uzaktan bağlantı](service-fabric-cluster-remote-connect-to-azure-cluster-node.md) belirli ölçek kümesi örneği
* [RDP bağlantı noktası aralığı değerleri güncelleştirmek](./scripts/service-fabric-powershell-change-rdp-port-range.md) , VM'ler dağıtımdan sonra küme
* [Yönetici kullanıcı adı ve parola değiştirme](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) küme VM'ler için

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
