---
title: 'Öğretici: Azure Active Directory Tümleştirme ile CylancePROTECT | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile CylancePROTECT arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ea392d8c-c8aa-4475-99d0-b08524ef0f3a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: jeedes
ms.openlocfilehash: a4b8cbbe3d75702f38b5060957aff9f5c30e1daa
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="tutorial-azure-active-directory-integration-with-cylanceprotect"></a>Öğretici: Azure Active Directory Tümleştirme CylancePROTECT ile

Bu öğreticide, Azure Active Directory (Azure AD) ile CylancePROTECT tümleştirmek öğrenin.

CylancePROTECT Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- CylancePROTECT erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için CylancePROTECT (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme CylancePROTECT ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir CylancePROTECT çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden CylancePROTECT ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-cylanceprotect-from-the-gallery"></a>Galeriden CylancePROTECT ekleme
Azure AD CylancePROTECT tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden CylancePROTECT eklemeniz gerekir.

**Galeriden CylancePROTECT eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **CylancePROTECT**seçin **CylancePROTECT** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde CylancePROTECT](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_cylanceprotect_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı CylancePROTECT sınayın.

Tekli çalışmaya oturum için Azure AD CylancePROTECT karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının CylancePROTECT ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma CylancePROTECT ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[CylancePROTECT test kullanıcısı oluşturma](#create-a-cylanceprotect-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı CylancePROTECT sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma CylancePROTECT uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile CylancePROTECT yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **CylancePROTECT** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_cylanceprotect_samlbase.png)

3. Üzerinde **CylancePROTECT etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![CylancePROTECT etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_cylanceprotect_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:
    
    | Bölge | URL değeri |
    |----------|---------|
    | Asya Pasifik Kuzeydoğu (APNE1)| ` https://login-apne1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Asya Pasifik Güneydoğu (AU) | `https://login-au.cylance.com/EnterpriseLogin/ConsumeSaml` |
    | Avrupa Orta (EUC1)|`https://login-euc1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Kuzey Amerika|`https://login.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Güney Amerika (SAE1)|`https://login-sae1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    
    b. İçinde **yanıt URL'si** metin kutusuna, URL'yi yazın:
    
    | Bölge | URL değeri |
    |----------|---------|
    | Asya Pasifik Kuzeydoğu (APNE1)|`https://login-apne1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Asya Pasifik Güneydoğu (AU)|`https://login-au.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Avrupa Orta (EUC1)|`https://login-euc1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Kuzey Amerika|`https://login.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Güney Amerika (SAE1)|`https://login-sae1.cylance.com/EnterpriseLogin/ConsumeSaml`|

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve kimliğin sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_cylanceprotect_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_400.png)

6. Üzerinde **CylancePROTECT yapılandırma** 'yi tıklatın **yapılandırma CylancePROTECT** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![CylancePROTECT yapılandırma](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_cylanceprotect_configure.png) 

7. Çoklu oturum açma yapılandırmak için **CylancePROTECT** yan, indirilen göndermek için ihtiyacınız **Certificate(Base64), Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** konsol yöneticisine. Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-cylanceprotect-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-cylanceprotect-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-cylanceprotect-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-cylanceprotect-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-cylanceprotect-test-user"></a>CylancePROTECT test kullanıcısı oluşturma

Bu bölümde, CylancePROTECT içinde Britta Simon adlı bir kullanıcı oluşturun. CylancePROTECT platform kullanıcıları eklemek için Yönetici Konsolu ile çalışır. Azure Active Directory hesap sahibi bir e-posta alır ve onu etkinleştirilmeden önce kendi hesabı onaylamak için bağlantıyı izleyin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta CylancePROTECT için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**CylancePROTECT için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **CylancePROTECT**.

    ![Uygulamalar listesinde CylancePROTECT bağlantı](./media/active-directory-saas-cylanceprotect-tutorial/tutorial_cylanceprotect_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli CylancePROTECT parçasında tıklattığınızda, otomatik olarak CylancePROTECT uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cylanceprotect-tutorial/tutorial_general_203.png

