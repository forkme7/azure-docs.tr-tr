---
title: Ağ güvenlik grubu akış günlükleri Azure Ağ İzleyicisi ile yönetme | Microsoft Docs
description: Bu sayfa, ağ güvenlik grubu akış günlüklerine Azure Ağ İzleyicisi'ni yönetmek açıklanmaktadır
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: cb41781c5ac8fb759cecea01402c08dd716bf7d7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="manage-network-security-group-flow-logs-in-the-azure-portal"></a>Ağ güvenlik grubu akış günlükleri Azure portalında Yönet

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Ağ güvenlik grubu akış günlükleri, giriş ve çıkış IP trafiği bir ağ güvenlik grubu ile ilgili bilgileri görüntülemek sağlayan Ağ İzleyicisi özelliğidir. Bu akış günlükleri JSON biçiminde yazılmıştır ve önemli bilgiler sağlayan dahil olmak üzere: 

- Giden ve gelen akış kuralı başına temelinde.
- Akış uygulandığı NIC.
- 5-tanımlama grubu (kaynak/hedef IP, kaynak/hedef bağlantı noktası, protokol) akışı hakkında bilgi sağlar.
- Trafik izin verilen veya reddedilen hakkında bilgi.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlamak için aşağıdaki kaynaklara zaten olmalıdır:

- Varolan bir Ağ İzleyicisi. Ağ İzleyicisi oluşturmak için bkz: [bir Ağ İzleyicisi örneği oluşturmayı](network-watcher-create.md).
- Geçerli bir sanal makine ile varolan bir kaynak grubu. Bir sanal makineye sahip değilseniz, bkz. Create bir [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) veya [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) sanal makine.

## <a name="register-insights-provider"></a>Öngörüler sağlayıcısını Kaydet

Başarılı bir şekilde çalışması için günlük akışı **Microsoft.ınsights** sağlayıcı kaydedilmelidir. Sağlayıcıyı kaydetmek için aşağıdaki adımları uygulayın: 

1. Git **abonelikleri**ve akış günlükleri etkinleştirmek istediğiniz aboneliği seçin. 
2. Üzerinde **abonelik** dikey penceresinde, select **kaynak sağlayıcıları**. 
3. Ara sağlayıcılar listesi ve doğrulayın **Microsoft.ınsights** sağlayıcı kayıtlı. Aksi takdirde, ardından **kaydetmek**.

![Görünüm sağlayıcıları][providers]

## <a name="enable-flow-logs"></a>Akış günlükleri etkinleştir

Bu adımları bir ağ güvenlik grubu akışı açtığında etkinleştirme işlemi uygulayın.

### <a name="step-1"></a>1. Adım

Ağ İzleyicisi örneğine gidin ve ardından **NSG akış günlükleri**.

![Akış günlükleri genel bakış][1]

### <a name="step-2"></a>2. Adım

Bir ağ güvenlik grubu listeden seçin.

![Akış günlükleri genel bakış][2]

### <a name="step-3"></a>3. Adım 

Üzerinde **akışı günlüklerinin ayarları** dikey penceresinde ayarlanmış durumu **üzerinde**ve ardından bir depolama hesabı yapılandırın. Sahip mevcut bir depolama hesabını seçin **tüm ağları** (varsayılan) seçili altında **güvenlik duvarları ve sanal ağlar**altında **ayarları** depolama hesabı için. Bir depolama hesabı seçtikten sonra seçin **Tamam**ve ardından **kaydetmek**.

![Akış günlükleri genel bakış][3]

## <a name="download-flow-logs"></a>Akış günlükleri indirmek

Akış günlükleri, bir depolama hesabında kaydedilir. Bunları görüntülemek için akış günlüklerinizi indirin.

### <a name="step-1"></a>1. Adım

Akış günlükleri indirmek için seçin **akış günlükleri yapılandırılan depolama hesaplarından indirebilirsiniz**. Bu adım hangi günlükleri indirmek için seçebileceğiniz bir depolama hesabı görünümüne alır.

![Akış günlüğü ayarları][4]

### <a name="step-2"></a>2. Adım

Doğru depolama hesabınıza gidin. Ardından **kapsayıcıları** > **Öngörüler günlük networksecuritygroupflowevent**.

![Akış günlüğü ayarları][5]

### <a name="step-3"></a>3. Adım

Akış günlüğü konuma seçin ve ardından Git **karşıdan**.

![Akış günlüğü ayarları][6]

Günlük yapısı hakkında daha fazla bilgi için [ağ güvenlik grubu akışı günlüğü genel bakış](network-watcher-nsg-flow-logging-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [NSG akış günlüklerinizi Powerbı ile görselleştirme](network-watcher-visualize-nsg-flow-logs-power-bi.md).

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png
