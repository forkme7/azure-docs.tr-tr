---
title: Azure Site Recovery Azure VM'ler için çoğaltma yapılandırma | Microsoft Docs
description: Bu makalede, başka bir Site Kurtarma'yı kullanarak bir Azure bölgesinden Azure VM'ler için çoğaltma yapılandırma açıklar.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/09/2018
ms.author: asgang
ms.openlocfilehash: e5947242295a9c57b1c73e202c061d222cd0842f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Başka bir Azure bölgesine çoğaltma Azure sanal makineler


>[!NOTE]
>
> Azure sanal makineler için Site Recovery çoğaltma şu anda önizlemede değil.

Bu makalede, bir Azure bölgesinden diğerine Azure VM'ler, çoğaltma etkinleştir açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bu senaryo için Site Recovery önceden ayarladığınız açıklandığı gibi varsayar [Azure için Azure Öğreticisi](azure-to-azure-tutorial-enable-replication.md). Artık ön hazırlıklı ve kurtarma Hizmetleri kasası oluşturulmuş olduğundan emin olun.



## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Çoğaltmayı etkinleştirin. Bu yordam, Birincil Azure bölgesi Doğu Asya, ve ikincil bölge Güneydoğu Asya olduğunu varsayar.

1. Kasaya tıklayın **+ Çoğalt**.
2. Aşağıdaki alanları dikkat edin:
    - **Kaynak**: Bu durumda VM'ler başlangıç noktasını **Azure**.
    - **Kaynak konumu**: sanal makinelerinizi korumak istediğiniz Azure bölgesi. Bu çizim için kaynak konum 'Doğu Asya' değil.
    - **Dağıtım modeli**: kaynak makinelerin Azure dağıtım modeli.
    - **Kaynak grubu**: kaynak sanal makinelerinizi ait olduğu kaynak grubu. Seçilen kaynak grubundaki tüm sanal makineleri, sonraki adımda koruma için listelenir.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

3. İçinde **sanal makineleri > sanal makine Seç**, tıklayın ve çoğaltmak istediğiniz her bir VM seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Ardından **Tamam**.
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. İçinde **ayarları**, isteğe bağlı olarak hedef site ayarları yapılandırabilirsiniz:

    - **Hedef konum**: Burada, kaynak sanal makine verilerinizi çoğaltılabilir konumu. Seçili makineler konumuna bağlı olarak, Site Recovery, uygun hedef bölgelerin listesini sağlar. Kurtarma Hizmetleri kasası konumu olarak aynı hedef konumu tutmanızı öneririz.
    - **Hedef kaynak grubu**: çoğaltılan sanal makineleriniz için tüm ait kaynak grubu. Varsayılan olarak Azure Site Recovery "asr" sonekine sahip adla hedef bölgede yeni bir kaynak grubu oluşturur. Azure Site Recovery tarafından önceden oluşturulmuş kaynak grubu mevcut olmaması durumunda, yeniden kullanılır. Aşağıdaki bölümde gösterildiği gibi özelleştirmek seçebilirsiniz.
    - **Hedef sanal ağ**: varsayılan olarak, Site Recovery yeni bir sanal ağ hedef bölgede "asr" sonekine sahip adıyla oluşturur. Bu kaynak ağınıza eşlenen ve gelecekteki tüm koruma için kullanılır. [Daha fazla bilgi edinin](site-recovery-network-mapping-azure-to-azure.md) ağ eşlemesi hakkında.
    - **(VM kullanmaz, kaynak diskleri yönetiliyorsa) depolama hesapları hedef**: varsayılan olarak, Site Recovery kaynak VM depolama yapılandırmanızı mimicking yeni bir hedef depolama hesabı oluşturur. Depolama hesabı zaten mevcut olmaması durumunda, yeniden kullanılır.
    - **Çoğaltma yönetilen diskleri (kaynağınız VM yönetilen diskleri kullanıyorsa)**: Site Recovery disk kaynağının VM yönetilen olarak kaynak VM'ın yönetilen diskleri aynı depolama türünü (standart veya premium) ile yansıtmak üzere hedef bölgede yeni yönetilen yinelemenin diskleri oluşturur.
    - **Depolama hesapları önbelleğe**: Site Recovery önbellek depolama kaynağı bölgede adlı ek depolama alanı hesabı gerekiyor. Kaynak sanal makinelerin gerçekleştiği tüm değişiklikleri izlenen ve bu hedef konumuna çoğaltma önce önbellek depolama hesabına gönderilir.
    - **Kullanılabilirlik kümesi**: varsayılan olarak, Azure Site Recovery hedef bölgede kümesi "asr" sonekine sahip adı ile yeni bir kullanılabilirlik oluşturur. Kullanılabilirlik kümesi zaten Azure Site Recovery tarafından oluşturulan mevcut olmaması durumunda, yeniden kullanılır.
    - **Çoğaltma İlkesi**: kurtarma noktası bekletme geçmişi ve uygulama tutarlılığı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Azure Site Recovery ' 24 saattir kurtarma noktası bekletme ve ' 60 dakika uygulama tutarlılığı anlık görüntü sıklığı için varsayılan ayarlarla yeni bir çoğaltma ilkesi oluşturur.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

Site Recovery tarafından kullanılan varsayılan hedef ayarlarını değiştirebilirsiniz.

1. Tıklatın **Özelleştir:** varsayılan ayarlarını değiştirmek için:
    - İçinde **hedef kaynak grubu**, aboneliğin hedef konumda tüm kaynak gruplarının listeden kaynak grubunu seçin.
    - İçinde **hedef sanal ağ**, hedef konumdaki tüm sanal ağ listesinden bir ağ seçin.
    - İçinde **kullanılabilirlik kümesi**, kullanılabilirlik kümesi'nin kaynak bölgesinde bir parçası olup olmadıklarını varsa VM, kullanılabilirlik kümesi ayarları ekleyebilirsiniz.
    - İçinde **hedef depolama hesapları**, kullanmak istediğiniz hesabı seçin.

        ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/customize.PNG)

2. Tıklatın **hedef kaynak oluşturma** > **çoğaltmayı etkinleştirme**.
3. Sanal makineleri çoğaltma için etkinleştirildikten sonra altında VM sağlık durumunu kontrol edebilirsiniz **çoğaltılan öğeler**

>[!NOTE]
>İlk çoğaltma sırasında durum, ilerleme durumu yenilemek için biraz zaman alabilir. Tıklatın **yenileme** en son durumunu almak için düğmesi.
>

# <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) yük devretme testi çalıştırma hakkında.
