---
title: Erişim raporlama - Azure RBAC | Microsoft Docs
description: Tüm değişiklikleri, Azure aboneliklerinize Son 90 gün içinde rol tabanlı erişim denetimi ile erişimi listeleyen bir rapor oluşturur.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 169ed8dd6d14d8d9d0fd49ad7306b1d4fb2c4d90
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2018
---
# <a name="create-an-access-report-for-role-based-access-control"></a>Rol tabanlı erişim denetimi için bir access raporu oluşturma
Birisi erişim izni verilir veya erişim aboneliklerinizi içinde iptal eder dilediğiniz zaman değişiklikleri Azure olayları günlüğe. Son 90 gün için tüm değişiklikleri görmek için erişim değişiklik geçmişi raporları oluşturabilirsiniz.

## <a name="create-a-report-with-azure-powershell"></a>Azure PowerShell ile bir rapor oluşturma
PowerShell'de erişim değişiklik geçmişi raporu oluşturmak için kullanın [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) komutu.

Bu komut çağırdığınızda, aşağıdakiler de dahil olmak üzere listelenen istediğiniz atamaları hangi özelliğinin belirtebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **Eylem** |Erişim izni veya iptal |
| **Arayan** |Erişim değişiklik için sorumlu sahibi |
| **PrincipalId** | Kullanıcı, Grup veya rolü atandı uygulamanın benzersiz tanıtıcısı |
| **PrincipalName** |Kullanıcı, Grup veya uygulama adı |
| **PrincipalType** |Bir kullanıcı, Grup veya uygulama atama olup |
| **RoleDefinitionId** |Verilen veya iptal rolünün GUID |
| **RoleName** |Verilen veya iptal rolü |
| **Kapsam** | Abonelik, kaynak grubu veya atama uygulandığı kaynak benzersiz tanıtıcısı | 
| **ScopeName** |Abonelik, kaynak grubu veya kaynak adı |
| **ScopeType** |Atama abonelik, kaynak grubu veya kaynak kapsamı olmasına |
| **zaman damgası** |Erişim değiştirildi saat ve tarihi |

Bu örnek komut Abonelik son yedi gün için tüm erişim değişiklikleri listeler:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog - ekran görüntüsü](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Azure CLI ile bir rapor oluşturun
Azure komut satırı arabirimi (CLI) içinde bir erişim değişiklik geçmişi raporu oluşturmak için kullanın `azure role assignment changelog list` komutu.

## <a name="export-to-a-spreadsheet"></a>Bir elektronik tabloya dışarı aktarma
Raporu kaydedin veya verileri işlemek için erişim değişikliklerini bir .csv dosyasına dışarı aktarın. Ardından, gözden geçirme için elektronik tablodaki raporunu görüntüleyebilirsiniz.

![Değişim günlüğü görüntülenebilir elektronik tablo olarak - ekran görüntüsü](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="next-steps"></a>Sonraki adımlar
* Çalışmak [Azure rbac'de özel roller](role-based-access-control-custom-roles.md)
* Nasıl yöneteceğinizi öğrenmek [powershell ile Azure RBAC](role-based-access-control-manage-access-powershell.md)

