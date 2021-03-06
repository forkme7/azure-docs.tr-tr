---
title: Azure Analysis Services Web Tasarımcısı kullanarak tablolu model oluşturma | Microsoft Docs
description: Azure portalında Web Tasarımcısını kullanarak bir Azure Analysis Services tablolu model oluşturmayı öğrenin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 4b1d4cffc3571297f2b74674156cb7f3bad7c2c8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-a-model-in-azure-portal"></a>Azure portalında bir model oluşturma

Azure portalında Azure Analysis Services web Tasarımcısı (Önizleme) özelliği oluşturmak ve tablolu modeller düzenleyin ve model veri sağ tarayıcınızda sorgulamak için hızlı ve kolay bir yol sağlar. 

Unutmayın, web Designer **Önizleme**. İşlevselliği sınırlıdır. Daha gelişmiş modeli geliştirme ve test için Visual Studio (SSDT) ve SQL Server Management Studio (SSMS) kullanmak en iyisidir.

## <a name="before-you-begin"></a>Başlamadan önce

- Bir standart veya Geliştirici katmanı Azure Analysis Services sunucusunda. Web Tasarımcısı'nı kullanarak oluşturduğunuz yeni modelleri DirectQuery, bu Katmanlar tarafından desteklenen ' dir.
- Bir Azure SQL Database, Azure SQL Data Warehouse veya Power BI Desktop (.pbix) dosyası olarak bir veri kaynağı. Power BI Desktop dosyaları destek Azure SQL Database ve Azure SQL Data Warehouse oluşturulan yeni modeller.
- Bir SQL Server hesabı ve Azure SQL Database veya Azure SQL veri ambarı veri kaynaklarına bağlanmak için parola.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="to-create-a-new-tabular-model"></a>Yeni bir tablolu model oluşturmak için

1. Sunucunuzdaki **genel bakış** > **Web Tasarımcısı**, tıklatın **açık**.

    ![Azure portalında bir model oluşturma](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. İçinde **Web Tasarımcısı** > **modelleri**, tıklatın **+ Ekle**.

    ![Azure portalında bir model oluşturma](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. İçinde **yeni model**, model adını yazın ve ardından bir veri kaynağı seçin.

    ![Azure portalında yeni model iletişim kutusu](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. İçinde **Bağlan**, bağlantı özelliklerini girin. Kullanıcı adı ve parola bir SQL Server hesabı olması gerekir.

     ![Azure portalında iletişim Bağlan](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. İçinde **tabloları ve görünümleri**, modelinize eklemek ve ardından istediğiniz tabloları seçin **oluşturma**. Bir anahtar çifti ile tablolar arasında ilişkiler otomatik olarak oluşturulur.

     ![Tabloları ve görünümleri seçin](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

Yeni modelinizi tarayıcınızda görüntülenir. Buradan, şunları yapabilirsiniz:   

- Model verileri sorgu alanları Sorgu Tasarımcısı sürüklemek ve filtreleri ekleme.
- Yeni ölçüleri tablolarda oluşturun.
- Json Düzenleyicisi'ni kullanarak model meta verilerini düzenleyin.
- Model, Visual Studio (SSDT), Power BI Desktop veya Excel'de açın.

![Tabloları ve görünümleri seçin](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> Model meta verilerini düzenleyin veya yeni ölçüleri tarayıcınızda oluşturduğunuzda, bu değişiklikleri modelinizi Azure kaydetmeyi. Ayrıca SSDT, Power BI Desktop veya Excel modelinizde üzerinde çalışıyorsanız, modelinizi eşitlenmemiş elde edebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar 
[Veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md)  
[Excel ile bağlanma](analysis-services-connect-excel.md)  


