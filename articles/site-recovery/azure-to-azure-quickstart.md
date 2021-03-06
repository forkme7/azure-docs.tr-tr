---
title: Bir Azure VM’yi başka bir Azure bölgesine çoğaltma (Önizleme)
description: Bu hızlı başlangıç, bir Azure bölgesindeki Azure VM’yi başka bir bölgeye çoğaltmak için gerekli olan adımları sağlar.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: quickstart
ms.date: 04/08/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: a317d54b56f72373d99af35b806cb231c2ef962e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="replicate-an-azure-vm-to-another-azure-region-preview"></a>Bir Azure VM’yi başka bir Azure bölgesine çoğaltma (Önizleme)

[Azure Site Recovery](site-recovery-overview.md) hizmeti, planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu hızlı başlangıç, bir Azure VM’nin farklı bir Azure bölgesine nasıl çoğaltılacağını açıklar.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-replication-for-the-azure-vm"></a>Azure VM için çoğaltmayı etkinleştirme

1. Azure portalında, **Sanal makineler** seçeneğine tıklayın ve çoğaltmak istediğiniz VM’yi seçin.

2. **Ayarlar** kısmında, **Olağanüstü durum kurtarma (önizleme)** seçeneğine tıklayın.
3. **Olağanüstü durum kurtarmayı yapılandır** > **Hedef bölge** bölümünde, çoğaltmak istediğiniz hedef bölgeyi seçin.
4. Bu Hızlı Başlangıç için, diğer varsayılan ayarları kabul edin.
5. **Çoğaltmayı etkinleştir**’e tıklayın. Bu, sanal makineye yönelik çoğaltmayı etkinleştirmek için bir iş başlatır.

    ![çoğaltmayı etkinleştir](media/azure-to-azure-quickstart/enable-replication1.png)



## <a name="verify-settings"></a>Ayarları doğrulama

Çoğaltma işlemi bittikten sonra, çoğaltma durumunu denetleyebilir, çoğaltma ayarlarını değiştirebilir ve dağıtımı test edebilirsiniz.

1. VM menüsünde, **Olağanüstü durum kurtarma (önizleme)** seçeneğine tıklayın.
2. Çoğaltma durumunu, oluşturulan kurtarma noktalarını ve haritadaki kaynak ve hedef bölgelerini doğrulayabilirsiniz.

   ![Çoğaltma durumu](media/azure-to-azure-quickstart/replication-status.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Çoğaltma işlemini devre dışı bıraktığınızda, birincil bölgedeki VM çoğaltmayı durdurur:

- Kaynak çoğaltma ayarları otomatik olarak temizlenir.
- VM’nin Site Recovery faturalaması da ayrıca durur.

Şu adımlara göre çoğaltmayı durdurun:

1. VM’yi seçin.
2. **Olağanüstü durum kurtarma (önizleme)** bölümünde, **Diğer** seçeneğine tıklayın.
3. **Çoğaltmayı Devre Dışı Bırak** seçeneğine tıklayın.

   ![Çoğaltmayı devre dışı bırakma](media/azure-to-azure-quickstart/disable2-replication.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, tek bir VM’yi ikincil bir bölgeye çoğalttınız.

> [!div class="nextstepaction"]
> [Azure VM’leri için olağanüstü durum kurtarmayı yapılandır](azure-to-azure-tutorial-enable-replication.md)
