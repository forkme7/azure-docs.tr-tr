---
title: Visual Studio'da bağlı hizmetler kullanarak Azure Storage ekleme | Microsoft Docs
description: Visual Studio bağlı Hizmetleri Ekle iletişim kutusunu kullanarak uygulamanıza Azure depolama ekleme
services: visual-studio-online
author: ghogen
manager: douge
assetId: 521ec044-ad4b-4828-8864-01decde2e758
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/26/2017
ms.author: ghogen
ms.openlocfilehash: 3c5d3dc1d91a6f8bb1816b2985f86ec5c4a12e63
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Visual Studio bağlantılı hizmetler kullanarak Azure depolama ekleme
Visual Studio ile aşağıdakilerden herhangi birini Azure depolama birimine kullanarak bağlayabilirsiniz **bağlı Hizmetleri Ekle** iletişim:

- C# bulut hizmeti
- .NET arka uç mobil hizmeti
- ASP.NET Web sitesi veya hizmeti
- ASP.NET Core hizmeti
- Azure Web işi hizmeti 

Bağlantılı hizmeti işlevselliği tüm gerekli referanslar ve bağlantı kodu projenize ekler ve yapılandırma dosyalarına uygun şekilde değiştirir. 

Tamamlandıktan sonra **bağlı Hizmetleri Ekle** sıraları, blob storage ile çalışmaya başlamak için gerekli olan adımları ayrıntılı belgeler görüntüler ve tabloları otomatik olarak iletişim.

## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Azure depolama bağlantılı Hizmetler iletişim kutusunu kullanarak bağlan
1. Projenizi Visual Studio'da açın

1. İçinde **Çözüm Gezgini**, sağ tıklatın **bağlantılı Hizmetler** düğümünü ve bağlam menüsünden ve select **bağlı hizmet Ekle**.
   
    ![Azure eklemek bağlı hizmet](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. İçinde **bağlantılı Hizmetler** sayfasında, **Azure Storage ile bulut depolama**.
   
    ![Azure depolama ekleme](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. İçinde **Azure Storage** iletişim kutusu, varolan bir depolama hesabı seçin ve Seç **Ekle**.
   
    Bir depolama hesabı oluşturmanız gerekiyorsa, sonraki adıma gidin. Aksi takdirde, 6. adıma atlayın.
    
    ![Varolan bir depolama hesabı projeye ekleyin](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. Depolama hesabı oluşturmak için: 
   
   1. Seçin **yeni depolama hesabı oluşturma** iletişim kutusunun altındaki.

   1. Doldurmak **depolama hesabı oluştur** iletişim ve select **oluşturma**.
      
       ![Yeni Azure depolama hesabı](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. Zaman **Azure Storage** iletişim kutusu görüntülendiğinde, yeni depolama hesabı listede görüntülenir. Listede yeni depolama hesabı seçin ve Seç **Ekle**.

1. Bağlantılı hizmeti görünür altında depolama **hizmeti başvuruları** projenizin düğümü.
   
## <a name="how-your-project-is-modified"></a>Projenizi nasıl değiştirilir
İletişim kutusu bitirdikten sonra Visual Studio başvuruları ekler ve belirli yapılandırma dosyalarını değiştirir. Özel değişiklikleri proje türüne bağlıdır: 

- ASP.NET proje - [ne – ASP.NET projeleri](http://go.microsoft.com/fwlink/p/?LinkId=513126)
- ASP.NET Core proje - [ne – ASP.NET 5 projeleri](http://go.microsoft.com/fwlink/p/?LinkId=513124) 
- Bulut hizmeti projesi (web rolleri ve çalışan rolleri) - [ne – bulut hizmeti projeleri](http://go.microsoft.com/fwlink/p/?LinkId=516965)
- Web işi projesinin - [ne oldu - WebJob projeleri](visual-studio/vs-storage-webjobs-what-happened.md)

## <a name="next-steps"></a>Sonraki adımlar
- [MSDN forumu: Azure depolama](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [Microsoft Azure depolama ekibi blogu](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure Storage belgeleri](https://docs.microsoft.com/azure/storage/)
