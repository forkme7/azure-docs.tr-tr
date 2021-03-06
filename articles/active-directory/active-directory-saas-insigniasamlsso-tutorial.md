---
title: "Öğretici: Azure Active Directory Tümleştirme Insignia SAML SSO | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Insignia SAML SSO arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 828c981c-c3dd-4eb2-8699-0f732baa43f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: jeedes
ms.openlocfilehash: 9d7d780e212a409c1b868b675e190c4a03788ab1
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-insignia-saml-sso"></a>Öğretici: Azure Active Directory Tümleştirme Insignia SAML SSO

Bu öğreticide, Azure Active Directory (Azure AD) ile Insignia SAML SSO tümleştirmek öğrenin.

Insignia SAML SSO Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Insignia SAML SSO erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak için Insignia SAML SSO (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Insignia SAML SSO ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Insignia SAML SSO çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Insignia SAML SSO ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-insignia-saml-sso-from-the-gallery"></a>Galeriden Insignia SAML SSO ekleme
Azure AD Insignia SAML SSO tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Insignia SAML SSO eklemeniz gerekir.

**Galeriden Insignia SAML SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Insignia SAML SSO**seçin **Insignia SAML SSO** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde insignia SAML SSO](./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_insigniasamlsso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Insignia SAML SSO ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Insignia SAML SSO bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Insignia SAML SSO arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Insignia SAML SSO atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma Insignia SAML SSO sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Insignia SAML SSO test kullanıcısı oluşturma](#create-an-insignia-saml-sso-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Insignia SAML SSO sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Insignia SAML SSO uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Insignia SAML SSO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Insignia SAML SSO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_insigniasamlsso_samlbase.png)

3. Üzerinde **Insignia SAML SSO etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![İnsignia SAML SSO etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_insigniasamlsso_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://<customername>.insigniails.com/ils` |
    | `https://<customername>.insigniails.com/` |
    | `https://<customername>.insigniailsusa.com/ ` |

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<customername>.insigniailsusa.com/<uniqueid>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Insignia SAML SSO istemci destek ekibi](http://www.insigniasoftware.com/insignia/Techsupport.aspx) bu değerleri almak için. 
 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_insigniasamlsso_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_general_400.png)

6. Üzerinde **Insignia SAML SSO Yapılandırması** 'yi tıklatın **Insignia SAML SSO yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![İnsignia SAML SSO yapılandırma](./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_insigniasamlsso_configure.png) 

7. Çoklu oturum açma yapılandırmak için **Insignia SAML SSO** yan, indirilen göndermek için ihtiyacınız **sertifika (Base64)**, **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** için [İnsignia SAML SSO destek ekibi](http://www.insigniasoftware.com/insignia/Techsupport.aspx). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-insigniasamlsso-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-insigniasamlsso-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-insigniasamlsso-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-insigniasamlsso-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-an-insignia-saml-sso-test-user"></a>Bir Insignia SAML SSO test kullanıcısı oluşturma

Bu bölümde, Britta Simon Insignia kitaplığı sistemde adlı bir kullanıcı oluşturun. Çalışmak [Insignia kitaplığı sistem destek ekibi](http://www.insigniasoftware.com/insignia/Techsupport.aspx) Insignia kitaplığı sistem platform kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Insignia SAML SSO erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Insignia SAML SSO atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Insignia SAML SSO**.

    ![Uygulamalar listesinde Insignia SAML SSO bağlantı](./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_insigniasamlsso_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Insignia SAML SSO parçasında tıklattığınızda, otomatik olarak Insignia SAML SSO uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-insigniasamlsso-tutorial/tutorial_general_203.png

