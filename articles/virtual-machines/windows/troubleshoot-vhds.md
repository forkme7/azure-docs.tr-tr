---
title: Azure Windows sanal makinelerde ekli VHD'ler sorunlarını giderme | Microsoft Docs
description: Windows sanal makineleri veya içeren bir depolama hesabını silerken sorun beklenmeyen yeniden başlatmalar gibi sorunları gidermek nasıl VHD'ler bağlı.
keywords: SSH bağlantı reddedildi, ssh hatası, azure ssh, SSH bağlantısı başarısız oldu
services: virtual-machines-windows
author: roygara
manager: jeconnoc
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 02/28/2018
ms.author: rogarana
ms.openlocfilehash: d0103d8ea608014e53f70402220b302b6da445e9
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="troubleshoot-attached-vhds-on-azure-windows-virtual-machines"></a>Azure Windows sanal makinelerde ekli VHD'ler sorun giderme

Azure sanal makineleri bağlı sanal sabit disk (VHD) işletim sistemi diski ve diğer veri disklerde kullanır. VHD'ler, bir veya daha fazla Azure Storage hesapları sayfa bloblarını olarak depolanır. Bu makalede VHD ile kaynaklanabilecek ortak sorunlarının nasıl giderileceği açıklanmaktadır. 

  * [Ekli VHD ile VM'lerin beklenmeyen yeniden başlatma]
  * [Resource Manager dağıtımında depolama silme hataları]

## <a name="you-are-experiencing-unexpected-reboots"></a>Ekli VHD ile VM'lerin beklenmeyen yeniden başlatma

Bir Azure sanal makine (VM) çok sayıda aynı depolama hesabında olan ekli VHD'ler varsa, VM başarısız olmasına neden olan bir depolama hesabı ölçeklenebilirlik hedefleri aşabilir. Depolama hesabı için dakika ölçümleri denetleyin (**TotalRequests**/**Totalıngress**/**TotalEgress**) aşan ani için bir depolama hesabı ölçeklenebilirlik hedefleri. Depolama hesabınıza azaltma oluştuysa belirleme konusunda yardım almak için "PercentThrottlingError artış bir [ölçümleri Göster]" bölümüne bakın.

Genel olarak, her tek tek giriş veya çıkış işlemi bir sanal makineden bir VHD çevrilir **Al sayfasında** veya **Put sayfa** temel sayfa blobu işlemleri. Bu nedenle, belirli bir davranışı, uygulamanızın üzerinde tek bir depolama hesabında dayalı kaç VHD'ler ayarlamak için ortamınız için tahmini IOPS kullanabilirsiniz. Microsoft, tek bir depolama hesabında 40 veya daha az disk olmasını önerir. Bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../../storage/common/storage-scalability-targets.md) depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla ayrıntı için özellikle toplam istek oranı ve toplam bant genişliğini depolama hesabı türü için kullanmakta olduğunuz.

Depolama hesabınız için ölçeklenebilirlik hedefleri aşan, etkinliğin ayrı ayrı her hesap azaltmak için birden çok depolama hesaplarındaki Vhd'lerinizi koyun.

## <a name="storage-delete-errors-in-rm"></a>Resource Manager dağıtımında depolama silme hataları

Bu bölüm, aşağıdaki hatalardan biri gerçekleştiğinde bir Azure depolama hesabı, kapsayıcı ya da Azure Resource Manager dağıtımında blob silmeye çalışırken sorun giderme kılavuzu sağlar.

>**Depolama hesabı 'StorageAccountName' silinemedi. Hata: Depolama hesabına ait yapıtlar kullanımda nedeniyle silinemiyor.**

>**# Kaldırıldığından dışında # silinemedi:<br>VHD'ler: kapsayıcıda şu anda bir kira yoktur ve istekte hiçbir kiralama kimliği belirtildi.**

>**# Dışında # BLOB'lar silinemedi:<br>BlobName.vhd: blob üzerindeki şu anda bir kira yoktur ve istekte hiçbir kiralama kimliği belirtildi.**

Azure Vm'lerinde kullanılan VHD'ler, sayfa bloblarını Azure standart veya premium storage hesabında depolanan .vhd dosyalarıdır.  Azure diskleri hakkında daha fazla bilgi bulunabilir [burada](../../virtual-machines/windows/about-disks-and-vhds.md). Azure bozulmasını önlemek için bir VM öğesine bağlı bir disk silinmesini önler. Ayrıca, kapsayıcıları ve bir VM öğesine bağlı bir sayfa blob'u olan depolama hesaplarını silinmesini önler. 

Bu hatalardan biri alırken bir depolama hesabı, kapsayıcı veya blob silmek için bir işlemdir: 
1. [Bir VM'ye bağlı BLOB'ları tanımlayın](#step-1-identify-blobs-attached-to-a-vm)
2. [Sil VM ile birlikte bağlı **işletim sistemi diski**](#step-2-delete-vm-to-detach-os-disk)
3. [Tüm detach **veri diskleri** kalan sanal makine gelen](#step-3-detach-data-disk-from-the-vm)

Bu adımları tamamladıktan sonra depolama hesabı silinirken, kapsayıcısı veya blob yeniden deneyin.

### <a name="step-1-identify-blob-attached-to-a-vm"></a>1. adım: bir VM'ye bağlı blob tanımlayın

#### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Senaryo 1: bir blob – silme ekli VM tanımlayın
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde seçin **tüm kaynakları**. Depolama hesabına altında Git **Blob hizmeti** seçin **kapsayıcıları**ve silmek için blob gidin.
3. Varsa blob **kira durumu** olan **Leased**, sonra sağ tıklatın ve seçin **Düzenle meta veri** Blob meta verileri bölmesini açmak için. 

    ![Portal ekran depolama hesabı BLOB'ları ve sağ tıklayın > "Vurgulanmış meta Düzenle"](./media/troubleshoot-vhds/utd-edit-metadata-sm.png)

4. Blob meta verileri bölmesinde denetleyin ve değeri kayıt **MicrosoftAzureCompute_VMName**. Bu değer VHD bağlı olduğu VM adıdır. (Bkz **önemli** Bu alan yoksa)
5. Blob meta verileri bölmesinde denetleyin ve değerini kaydedin **MicrosoftAzureCompute_DiskType**. Ekli disk işletim sistemi veya veri diski ise bu değeri tanımlar (bkz **önemli** Bu alan yoksa). 

     ![Depolama "Blob meta verileri" bölmesiyle açık portalı ekran görüntüsü](./media/troubleshoot-vhds/utd-blob-metadata-sm.png)

6. Blob disk türü ise **OSDisk** izleyin [2. adım: işletim sistemi diski kullanımdan çıkarmak için Sil VM](#step-2-delete-vm-to-detach-os-disk). Aksi durumda, blob disk türü ise **DataDisk** adımları [3. adım: veri diskini sanal makineden](#step-3-detach-data-disk-from-the-vm). 

> [!IMPORTANT]
> Varsa **MicrosoftAzureCompute_VMName** ve **MicrosoftAzureCompute_DiskType** görünmez blob meta verileri blob açıkça kiralanmış ve bir VM öğesine bağlı değil belirtir. Kira ilk bozmadan kiralık BLOB'lar silinemez. Kira sonu, blob üzerindeki sağ tıklatın ve seçmek için **sonu kira**. Bir VM öğesine bağlı olmayan kiralık BLOB'ları blob silinmesini engellemek, ancak kapsayıcı veya depolama hesabı silme engellemez.

#### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>2. Senaryo: bir kapsayıcı - silme VM'ler için bağlı olan tüm blob(s) kapsayıcıdaki tanımlayın
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde seçin **tüm kaynakları**. Depolama hesabına altında Git **Blob hizmeti** seçin **kapsayıcıları**, silinecek kapsayıcı bulun.
3. Kapsayıcı açmak için tıklatın ve onun içindeki blobları listesi görüntülenir. Blob türüne sahip tüm BLOB'lar tanımlamak = **sayfa blobu** ve kiralama durum = **Leased** bu listeden. İzleyin [Senaryo 1](#step-1-identify-blobs-attached-to-a-vm) her bu BLOB'lar ile ilişkilendirilmiş VM tanımlamak için.

    ![Depolama hesabı BLOB'ları ve "kira vurgulanmış durumuyla" "Leased" ile portalı ekran görüntüsü](./media/troubleshoot-vhds/utd-disks-sm.png)

4. İzleyin [2. adım](#step-2-delete-vm-to-detach-os-disk) ve [adım 3](#step-3-detach-data-disk-from-the-vm) sanal makine ile silmek için **OSDisk** ve ayırma **DataDisk**. 

#### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Senaryo 3: depolama silme hesap - VM'ler için bağlı olan tüm blob(s) depolama hesabındaki tanımlama
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde seçin **tüm kaynakları**. Depolama hesabına altında Git **Blob hizmeti** seçin **kapsayıcıları**.

    ![Depolama hesabı kapsayıcıları ve "kira vurgulanmış durumuyla" "Leased" ile portalı ekran görüntüsü](./media/troubleshoot-vhds/utd-containers-sm.png)

3. İçinde **kapsayıcıları** bölmesinde, tüm kapsayıcıları tanımlamak nerede **kira durumu** olan **Leased** ve izleyin [Senaryo 2](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) her  **Kiralanmış** kapsayıcı.
4. İzleyin [2. adım](#step-2-delete-vm-to-detach-os-disk) ve [adım 3](#step-3-detach-data-disk-from-the-vm) sanal makine ile silmek için **OSDisk** ve ayırma **DataDisk**. 

### <a name="step-2-delete-vm-to-detach-os-disk"></a>2. adım: İşletim sistemi diski kullanımdan çıkarmak için VM silme
VHD bir işletim sistemi diski ise, VHD'nin silinebilmesi için önce VM silmeniz gerekir. Başka bir eylem, bu adımları tamamladıktan sonra aynı VM ekli veri diskleri için gerekli olacaktır:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde seçin **sanal makineleri**.
3. VHD bağlı olduğu VM seçin.
4. Hiçbir şey etkin olarak sanal makinenin kullandığından emin olun ve sanal makine artık gerekir.
5. Üstündeki **sanal makine ayrıntıları** bölmesinde, **silmek**ve ardından **Evet** onaylamak için.
6. VM silinmesi gerekir, ancak VHD korunabilir. Ancak, VHD'yi bir VM'ye bağlı artık veya üzerinde bir kira sahip. Yayımlanacak kira için birkaç dakika sürebilir. Blob konumuna hem de kira serbest olduğunu doğrulamak için Gözat **Blob özellikleri** bölmesinde **kiralama durum** olmalıdır **kullanılabilir**.

### <a name="step-3-detach-data-disk-from-the-vm"></a>3. adım: VM veri diski kullanımdan çıkarın
VHD bir veri diski varsa, kira kaldırmak için VM VHD'den bağlantısını kesin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde seçin **sanal makineleri**.
3. VHD bağlı olduğu VM seçin.
4. Seçin **diskleri** üzerinde **sanal makine ayrıntıları** bölmesi.
5. Silinecek veri diski seçin VHD iliştirilir. VHD URL'si denetleyerek disk hangi blob bağlı olduğu belirleyebilirsiniz.
6. Blob konumun yolunu denetlemek için disk üzerinde tıklatarak doğrulayabilirsiniz **VHD URİ'si** alan.
7. Seçin **Düzenle** tepesindeki **diskleri** bölmesi.
8. Tıklatın **simgesi detach** silinecek veri diski.

     ![Depolama "Blob meta verileri" bölmesiyle açık portalı ekran görüntüsü](./media/troubleshoot-vhds/utd-vm-disks-edit.png)

9. **Kaydet**’i seçin. Disk şimdi sanal makineden ayrıldı ve VHD artık kiralanmış. Yayımlanacak kira için birkaç dakika sürebilir. Blob konumuna hem de kiralamasının serbest bırakıldığını doğrulamak için Gözat **Blob özellikleri** bölmesinde **kiralama durum** değeri olmalıdır **kilitli değil** veya **Kullanılabilir**.

[Ekli VHD ile VM'lerin beklenmeyen yeniden başlatma]: #you-are-experiencing-unexpected-reboots
[Resource Manager dağıtımında depolama silme hataları]: #storage-delete-errors-in-rm