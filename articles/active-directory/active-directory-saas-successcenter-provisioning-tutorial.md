---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı dönüm OnDemand yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlamak ve kullanıcı hesaplarına dönüm OnDemand sağlanmasını için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant
ms.openlocfilehash: 8aed9bdcdd54f2f0ef3cd81f979635e5716f0b5f
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2018
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Öğretici: Dönüm OnDemand otomatik kullanıcı sağlamayı yapılandırın


Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya dönüm OnDemand gruplarına sağlanmasını dönüm OnDemand ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi için adımları göstermektir.


> [!NOTE]
> Bu öğretici Azure AD kullanıcı sağlama hizmeti üstünde oluşturulmuş bir bağlayıcı açıklar. Bu hizmet ne yaptığını, nasıl çalıştığı ve sık sorulan sorular önemli ayrıntılar için bkz: [otomatikleştirmek kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına etkinleştirmektir](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide verilen senaryoda aşağıdaki sahip olduğunuz varsayılmaktadır:

*   Azure AD kiracısı
*   Dönüm OnDemand Kiracı
*   Dönüm OnDemand yönetici izinlerine sahip bir kullanıcı hesabı


> [!NOTE]
> Azure AD tümleştirme sağlama dayanan [dönüm OnDemand Webservice](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf), dönüm OnDemand takıma kullanılabilir olduğu.

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Galeriden dönüm OnDemand ekleme
Azure AD ile otomatik kullanıcı sağlamayı dönüm OnDemand yapılandırmadan önce Azure AD uygulama Galeriden yönetilen SaaS uygulamaları listenize dönüm OnDemand eklemeniz gerekir.

**Azure AD uygulama Galeriden dönüm OnDemand eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panelde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölümü][2]
    
3. Dönüm OnDemand eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **dönüm OnDemand**.

    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/AppSearch.png)

5. Sonuçlar panelinde seçin **dönüm OnDemand**ve ardından **Ekle** dönüm OnDemand SaaS uygulamaları listenize eklemek için düğmeyi.

    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/AppSearchResults.png)

    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cornerstone-ondemand"></a>Kullanıcıları dönüm OnDemand atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik kullanıcı sağlamayı bağlamında, yalnızca kullanıcı gruplarına ve/veya "Azure AD'de bir uygulamaya atanmış" eşitlenir. 

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları dönüm OnDemand erişmeniz karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek dönüm OnDemand için bu kullanıcılara ve/veya grupları atayabilirsiniz:

*   [Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Kullanıcıların dönüm OnDemand atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının yapılandırma sağlama otomatik kullanıcı test etmek için dönüm OnDemand atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Kullanıcı dönüm OnDemand atarken, (varsa) geçerli bir uygulamaya özgü rol atama iletişim kutusunda seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-cornerstone-ondemand"></a>Dönüm OnDemand otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılara ve/veya dönüm kullanıcı ve/veya grup atamaları üzerinde Azure AD içinde dayalı OnDemand gruplarında devre dışı bırakmak için hizmet sağlama Azure AD yapılandırma adımlarını size kılavuzluk eder.


### <a name="to-configure-automatic-user-provisioning-for-cornerstone-ondemand-in-azure-ad"></a>Azure AD'de dönüm OnDemand için otomatik kullanıcı sağlamayı yapılandırmak için:


1. Oturum [Azure portal](https://portal.azure.com) ve **Azure Active Directory > Kurumsal uygulamalar > tüm uygulamaları**.

2. Dönüm OnDemand SaaS uygulamaları listesinden seçin.
 
    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/Successcenter2.png)

3. Seçin **sağlama** sekmesi.

    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/ProvisioningTab.png)

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/ProvisioningCredentials.png)

5. Altında **yönetici kimlik bilgileri** bölümü, giriş **yönetici kullanıcı adı**, **yönetici parolası**, ve **etki alanı** dönüm OnDemand's, hesabı.

    *   İçinde **yönetici kullanıcı adı** alanında, doldurmak dönüm OnDemand Kiracı yönetici hesabı kullanıcı adı. Örnek: yönetici

    *   İçinde **yönetici parolası** alanında, yönetici kullanıcı adı için karşılık gelen parola doldurur.

    *   İçinde **etki alanı** alan, dönüm Web hizmeti URL'sini doldurur. Örnek: Hizmet bulunur `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`, Contoso etki alanı için `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`.

6. Adım 5'te gösterilen alanları doldurma sırasında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD dönüm OnDemand bağlanabilir. Bağlantı başarısız olursa dönüm OnDemand hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/TestConnection.png)

7. İçinde **bildirim e-posta** alan, bir kişi veya grubun ve sağlama hata bildirimleri almak onay e-posta adresini girin **birhataoluşursa,bire-postabildirimigönder**.

    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/EmailNotification.png)

8. **Kaydet**’e tıklayın.

9. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory Kullanıcıları dönüm OnDemand**.

    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/UserMapping.png)

10. Azure AD'den dönüm OnDemand eşitlenir kullanıcı öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri dönüm OnDemand kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın.

    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/UserMappingAttributes.png)

11. Kapsam filtrelerini yapılandırmak için sağlanan aşağıdaki yönergelerine bakın [Scoping filtre Öğreticisi](./active-directory-saas-scoping-filters.md).

12. Azure AD hizmeti dönüm OnDemand için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/ProvisioningStatus.png)

13. Kullanıcılara ve/veya istediğiniz grupları dönüm OnDemand sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/SyncScope.png)

14. Sağlamak için hazır olduğunuzda tıklatın **kaydetmek**.

    ![OnDemand dönüm sağlama](./media/active-directory-saas-successcenter-provisioning-tutorial/Save.png) 


Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlanan gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada hizmet sağlama Azure AD çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve dönüm OnDemand hizmette sağlama Azure AD tarafından gerçekleştirilen tüm eylemler açıklanmaktadır Etkinlik Raporu sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](./active-directory-saas-provisioning-reporting.md).
## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-successcenter-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successcenter-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successcenter-provisioning-tutorial/tutorial_general_03.png
