---
title: "Visual Studio yükleme ve Azure yığınına bağlanma | Microsoft Docs"
description: "Visual Studio'yu yüklemek ve Azure yığınına bağlanmak için gereken adımları öğrenin"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 5eae2553af1e41ace878a2f8f2c1a1656c63a7c4
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Visual Studio'yu yüklemek ve Azure yığınına bağlanın

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Geliştir ve Dağıt Azure Resource Manager için Visual Studio'yu kullanın [şablonları](azure-stack-arm-templates.md) Azure yığınında. Bu makalede açıklanan adımları Visual Studio'yu yüklemek için herhangi birinden kullanabileceğiniz [Azure yığın Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ya da istemciden aracılığıyla bağlıysanız, bir Windows tabanlı dış [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). Bu adımları Visual Studio 2015 Community Edition yeni bir yüklemesini gerçekleştirin. Daha fazla bilgi edinin [bir arada bulunma](https://msdn.microsoft.com/library/ms246609.aspx) diğer Visual Studio sürümleri arasında.

## <a name="install-visual-studio"></a>Visual Studio yükleme
1. İndirme ve çalıştırma [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).             
2. Arama **Microsoft Azure SDK - 2.9.6 ile Visual Studio Community 2015**, tıklatın **Ekle**, ve **yükleme**.

    ![Ekran görüntüsü, Webpı yükleme adımları](./media/azure-stack-install-visual-studio/image1.png) 

3. Kaldırma **Microsoft Azure PowerShell** Azure SDK'ın bir parçası olarak yüklenir.

    ![Azure PowerShell Ekle/Kaldır programlar arabiriminin ekran görüntüsü](./media/azure-stack-install-visual-studio/image2.png) 

4. [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)

5. Yükleme tamamlandıktan sonra işletim sistemini yeniden başlatın.

## <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

1. Visual Studio'yu başlatın.

2. Gelen **Görünüm** menüsünde, select **Cloud Explorer**.

3. Yeni bölmesinde seçin **hesabı Ekle** ve Azure Active Directory kimlik bilgileriyle oturum açın.  
    ![Cloud Explorer ekran görüntüsü bir kez oturum açmış ve Azure yığınına bağlı](./media/azure-stack-install-visual-studio/image6.png)

Oturum açtıktan sonra [şablonlarını dağıtma](azure-stack-deploy-template-visual-studio.md) veya kendi şablonlarınızı oluşturmak için kullanılabilir kaynak türleri ve kaynak grupları göz atın.  

## <a name="next-steps"></a>Sonraki Adımlar

 - [Şablonları geliştirmek için Azure yığını](azure-stack-develop-templates.md)
