---
title: Azure kaynak erişim atamalarını görüntüleyin | Microsoft Docs
description: Görüntüleme ve tüm rol tabanlı erişim denetimi atamaları herhangi bir kullanıcı veya grup Azure portalında yönetme
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: jeffsta
ms.assetid: e6f9e657-8ee3-4eec-a21c-78fe1b52a005
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: rolyon
ms.openlocfilehash: 35838ed0f86dc4e063c943087c88d19195df72fd
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal"></a>Kullanıcılar ve gruplar Azure portalında için erişim atamalarını görüntüle
> [!div class="op_single_selector"]
> * [Kullanıcı veya gruba göre erişimi yönetme](role-assignments-users.md)
> * [Kaynağa göre erişimi yönetme](role-assignments-portal.md)

Rol tabanlı erişim denetimi ile (RBAC) Azure Active Directory'de (Azure AD), Azure kaynaklarınıza erişimi yönetebilirsiniz. 

RBAC ile atanmış erişim izinleri kısıtla iki yolla olduğundan ayrıntılıdır:

* **Kapsam:** RBAC rolü atamalarını kapsamlı bir belirli aboneliğe, kaynak grubu ya da kaynak. Tek bir kaynağa erişim verilen bir kullanıcı aynı abonelik alanındaki herhangi bir kaynağa erişemez.
* **Rol:** atama kapsamında erişim daraltıldığı rol atama tarafından bile daha fazla. Rol sahibi gibi üst düzey ya da sanal makine okuyucu gibi belirli olabilir.

Roller yalnızca gelen abonelik, kaynak grubu veya atama kapsamı kaynak içinde atanabilir. Ancak, belirli bir kullanıcı veya grup için tüm erişim atamalarını tek bir yerde görüntüleyebilirsiniz. Her abonelik 2000 rol atamalarında kadar olabilir. 

Hakkında daha fazla bilgi almak [Azure aboneliği kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](role-assignments-portal.md).

## <a name="view-access-assignments"></a>Erişim atamalarını görüntüle
Tek bir kullanıcı veya grup erişimi atamaları bakmak için Azure Active Directory'de başlatmak [Azure portal](http://portal.azure.com).

1. **Azure Active Directory**'yi seçin. Bu seçenek, gezinti listenizde görünür durumda değilse, seçin **tüm hizmetleri** ve Bul aşağıya doğru kaydırma **Azure Active Directory**.
2. Seçin **kullanıcılar ve gruplar**ve sonra da **tüm kullanıcılar** veya **tüm grupları**. Bu örnekte, bireysel kullanıcılar odaklanın.
    ![Kullanıcıları ve grupları Azure Active Directory'de - ekran görüntüsü yönetin](./media/role-assignments-users/rbac_users_groups.png)
3. Kullanıcı adı veya kullanıcı adı tarafından arayın.
4. Kullanıcı dikey penceresinde **Azure kaynakları**'nı seçin. Söz konusu kullanıcının tüm erişim atamaları görüntülenir.

### <a name="read-permissions-to-view-assignments"></a>Atamalarını görüntülemek için Okuma izinleri
Bu sayfa yalnızca okuma iznine sahip olması için erişim atamalarını gösterir. Örneğin, abonelik A okuma erişimine sahip olması ve bir kullanıcının atamaları denetlemek için Azure kaynaklarını dikey penceresine gidin. Bir abonelik için kendi erişim atamalarını görebilirsiniz, ancak kendisi de B. abonelikte erişim atamaları olduğunu göremez

## <a name="delete-access-assignments"></a>Erişim atamalarını silin
Bu dikey pencereden bir kullanıcı veya grup için atanmış erişim atamalarını silebilirsiniz. Erişim atama üst gruptan devralınan, kaynak veya abonelik gidin ve atama var. yönetmek gerekir.

1. Bir kullanıcı veya grup için tüm erişim atamalarını listesinden silmek istediğinizi seçin.
2. Seçin **kaldırmak** ve ardından **Evet** onaylamak için.
    ![Erişim atamayı - ekran görüntüsü](./media/role-assignments-users/delete_assignment.png)

## <a name="next-steps"></a>Sonraki adımlar

* Rol tabanlı erişim denetimi ile çalışmaya başlama [Azure aboneliği kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](role-assignments-portal.md)
* Bkz. [RBAC yerleşik rolleri](built-in-roles.md)

