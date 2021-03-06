---
title: Azure Otomasyonu kaynak gruplarıyla kaldırılmasını otomatik hale getirme
description: Bir Azure Otomasyonu senaryosunun, aboneliğinizdeki tüm kaynak gruplarını kaldırmaya yönelik runbook’lar içeren PowerShell İş Akışı sürümü.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 1d54e03c1b5518dece4e11d76593b12fe83dc8c2
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-automation-scenario---automate-removal-of-resource-groups"></a>Azure Otomasyonu senaryosu - kaynak gruplarının kaldırılmasını otomatik hale getirme
Birçok müşteri birden fazla kaynak grubu oluşturur. Bazıları üretim uygulamalarını yönetmek için, bazıları ise geliştirme, test ve hazırlık ortamları olarak kullanılabilir. Bu kaynakların dağıtımının otomatik hale getirilmesi bir özelliktir, ancak bir kaynak grubunun tek bir düğme tıklanarak kullanımdan kaldırılması da başka bir özelliktir. Bu ortak yönetim görevini Azure Otomasyonu'nu kullanarak basit hale getirebilirsiniz. Bu özellik, örneğin MSDN veya Microsoft İş Ortağı Ağı Bulut Temel Bileşenleri programı gibi bir üye teklifi üzerinden harcama limitine sahip bir Azure aboneliği ile çalışıyorsanız yararlı olur.

Bu senaryo bir PowerShell runbook'u temel alır ve aboneliğinizden belirttiğiniz bir veya daha fazla kaynak grubunu kaldırmak için tasarlanmıştır. Runbook’un varsayılan ayarı, devam etmeden önce test etmektir. Bu ayar, kaynak grubunu bu yordamı tamamlamak için hazır olmadan yanlışlıkla silmenizi önler.   

## <a name="getting-the-scenario"></a>Senaryoyu alma
Bu senaryo, [PowerShell Galerisi](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript)’nden indirebileceğiniz PowerShell runbook’u içerir. Doğrudan Azure portal’daki [Runbook Galerisi](automation-runbook-gallery.md)’nden de indirebilirsiniz.<br><br>

| Runbook | Açıklama |
| --- | --- |
| Remove-ResourceGroup |Bir veya daha fazla Azure kaynak grubunu ve ilişkili kaynakları abonelikten kaldırır. |

<br>
Bu runbook için aşağıdaki giriş parametreleri tanımlanmıştır:

| Parametre | Açıklama |
| --- | --- |
| NameFilter (Gerekli) |Silmeyi amaçladığınız kaynak gruplarını sınırlandırmak için bir ad filtresi belirtir. Virgülle ayrılmış bir liste kullanarak birden çok değer geçirebilirsiniz.<br>Filtre büyük küçük harfe duyarlı değildir ve dizeyi içeren herhangi bir kaynak grubu ile eşleşir. |
| PreviewMode (İsteğe bağlı) |Hangi kaynak gruplarının silineceğini görmek için runbook’u yürütür, ancak herhangi bir işlem yapmaz.<br>Varsayılan değer, runbook’a geçirilen bir veya daha fazla kaynak grubunun yanlışlıkla silinmesini önlemeye yardımcı olmak üzere **true** olarak ayarlanmıştır. |

## <a name="install-and-configure-this-scenario"></a>Bu senaryoyu yükleme ve yapılandırma
### <a name="prerequisites"></a>Önkoşullar
Bu runbook [Azure Farklı Çalıştır hesabı](automation-sec-configure-azure-runas-account.md) kullanarak kimlik doğrulaması yapar.    

### <a name="install-and-publish-the-runbooks"></a>Runbook yükleme ve yayımlama
Runbook’u indirdikten sonra [Runbook’ları içeri aktarma yordamları](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation) içindeki yordamı kullanarak içeri aktarabilirsiniz. Runbook’u Otomasyon hesabınıza başarıyla içeri aktarıldıktan sonra yayımlayın.

## <a name="using-the-runbook"></a>Runbook’u kullanma
Aşağıdaki adımlar, bu runbook ve nasıl çalıştığı ile aşina Yardım yürütülmesini size yol. Aslında kaynak grubunun silinmesi bu örnekte, runbook test ediyorsunuz.  

1. Azure portal’da Otomasyon hesabınızı açın ve **Runbook'lar** seçeneğine tıklayın.
2. **Remove-ResourceGroup** runbook’unu seçin ve **Başlat**’a tıklayın.
3. Runbook'u başlattığınızda **Runbook'u Başlat** sayfası açılır ve parametreleri yapılandırabilirsiniz. Test kullanabilirsiniz ve hiçbir zarar yanlışlıkla sildiyseniz neden olur, aboneliğinizde kaynak gruplarının adlarını girin.

   > [!NOTE]
   > Seçili kaynak gruplarının yanlışlıkla silinmesini önlemek için **Previewmode**’un **true** olarak ayarlandığından emin olun. Bu runbook, bu runbook'un çalıştığı Otomasyon hesabını içeren kaynak grubunu kaldırmaz.  
   >
   >
1. Tüm parametre değerlerini yapılandırdıktan sonra **Tamam**’a tıklarsanız runbook yürütme kuyruğuna alınır.  

Ayrıntılarını görüntülemek için **Kaldır ResourceGroup** runbook işi Azure portalında altında **kaynak**seçin **işleri** runbook. Seçin, görüntülemek istediğiniz iş. İş özetinde, giriş parametreleri ve çıkış akışına ek olarak işe ilişkin genel bilgiler ve gerçekleşen özel durumlar gösterilir.<br> ![Remove-ResourceGroup runbook iş durumu](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

**İş Özeti** bölümünde çıkış, uyarı ve hata akışlarından iletiler bulunur. Runbook yürütmeyle ilgili ayrıntılı sonuçları görüntülemek için **Çıkış**’ı seçin.<br> ![Remove-ResourceGroup runbook çıkış sonuçları](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## <a name="next-steps"></a>Sonraki adımlar
* Kendi runbook’unuzu oluşturmaya başlamak için bkz. [Azure Otomasyonu’nda runbook oluşturma veya içeri aktarma](automation-creating-importing-runbook.md).
* PowerShell İş Akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell İş Akışı runbook uygulamam](automation-first-runbook-textual.md).
