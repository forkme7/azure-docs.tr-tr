---
title: Azure Analysis Services sunucunuzun örnek tablo modeli ekleme | Microsoft Docs
description: Azure Analysis Services içinde bir örnek modeli eklemeyi öğrenin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 27353ff8c05f44b76304279e09a8a8d817041d78
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="tutorial-add-a-sample-model"></a>Öğretici: bir örnek modeli ekleme

Bu öğreticide, bir örnek Adventure Works modeli sunucunuza ekleyin. Örnek modeli öğretici modelleme Adventure Works Internet satış (1200) verileri tamamlanmış bir sürümüdür. Bir örnek modeli, model yönetim sınama, araçları ve istemci uygulamaları ile bağlanma ve model verileri sorgulamak için yararlıdır.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Bir Azure Analysis Services sunucusu
- Sunucu Yöneticisi izinleri

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-sample-model"></a>Bir örnek modeli oluşturma

1. Server **genel bakış**, tıklatın **yeni model**.

    ![Bir örnek modeli oluşturma](./media/analysis-services-create-sample-model/aas-create-sample-new-model.png)

2. İçinde **yeni model** > **bir veri kaynağını seçin**, doğrulama **örnek veri** seçilir ve ardından **Ekle**.

    ![Örnek verileri seçin](./media/analysis-services-create-sample-model/aas-create-sample-data.png)

3. İçinde **genel bakış**, doğrulama `adventureworks` örnek oluşturulur.

    ![Örnek verileri seçin](./media/analysis-services-create-sample-model/aas-create-sample-verify.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Örnek modelinizi önbellek kaynakları kullanıyor. Test etmek için örnek modelinizi kullanmıyorsanız, sunucunuzdan kaldırmanız gerekir.

> [!NOTE]
> Bu adımları nasıl SSMS kullanarak bir sunucudan bir model silineceğini açıklar. Ayrıca, Web Tasarımcı özelliği Preview sürümünü kullanarak bir model silebilirsiniz.

1. SSMS içinde > **Object Explorer**, tıklatın **Bağlan** > **Analysis Services**.

2. İçinde **sunucuya Bağlan**, sunucu adını, sonra yapıştırın **kimlik doğrulaması**, seçin **Active Directory - MFA desteğiyle Evrensel**, kullanıcı adınızı girin ve ardından **Bağlanmak**.

    ![Oturum aç](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-signin.png)

3. İçinde **Object Explorer**, sağ `adventureworks` örnek veritabanını ve ardından **silmek**.

    ![Örnek veritabanını silin](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-delete.png)

## <a name="next-steps"></a>Sonraki adımlar 

[Power BI Desktop'ta Bağlan](analysis-services-connect-pbi.md)   
[Veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md)


