---
title: "Öğretici: Azure Active Directory Tümleştirme ile Vodeclic | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Vodeclic arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
<<<<<<< HEAD
manager: femila
=======
manager: mtillman
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.reviewer: joflore
ms.assetid: d77a0f53-e3a3-445e-ab3e-119cef6e2e1d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: jeedes
<<<<<<< HEAD
ms.openlocfilehash: 8d1a32c0d8e7c41a06a8a3a677251df25de833b8
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
=======
ms.openlocfilehash: 940c7bb5040fb91a03b01dc43ee07d52e3d4e63b
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
---
# <a name="tutorial-azure-active-directory-integration-with-vodeclic"></a>Öğretici: Azure Active Directory Tümleştirme Vodeclic ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Vodeclic tümleştirmek öğrenin.

Vodeclic Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Vodeclic erişimi, Azure AD'de kontrol edebilirsiniz.
<<<<<<< HEAD
- Otomatik olarak için Vodeclic (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).
=======
- Otomatik olarak Vodeclic (çoklu oturum açma veya SSO) ile Azure AD hesaplarına oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Vodeclic ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
<<<<<<< HEAD
- Bir Vodeclic çoklu oturum açma etkin abonelik
=======
- Vodeclic SSO etkin bir abonelik
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

<<<<<<< HEAD
Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).
=======
Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa [bir aylık ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Vodeclic ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

<<<<<<< HEAD
## <a name="adding-vodeclic-from-the-gallery"></a>Galeriden Vodeclic ekleme
Azure AD Vodeclic tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Vodeclic eklemeniz gerekir.

**Galeriden Vodeclic eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Vodeclic**seçin **Vodeclic** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.
=======
## <a name="add-vodeclic-from-the-gallery"></a>Galeriden Vodeclic Ekle
Azure AD Vodeclic tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Vodeclic eklemeniz gerekir.

**Galeriden Vodeclic eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üstündeki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Vodeclic**. Seçin **Vodeclic** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için düğmesi.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

    ![Sonuçlar listesinde Vodeclic](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

<<<<<<< HEAD
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Vodeclic sınayın.

Tekli çalışmaya oturum için Azure AD Vodeclic karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Vodeclic ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Vodeclic içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Vodeclic ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Vodeclic test kullanıcısı oluşturma](#create-a-vodeclic-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Vodeclic sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.
=======
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Vodeclic ile test etme

Tekli çalışmaya oturum için Azure AD için bir kullanıcı Azure AD'de Vodeclic karşılık gelen kullanıcı olan bilmek ister. Diğer bir deyişle, Vodeclic içinde bir Azure AD kullanıcısının ve ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

Vodeclic içinde değere vermek **kullanıcıadı** aynı değer olarak **kullanıcı adı** Azure AD'de. Şimdi iki kullanıcılar arasında bağlantı kurulduktan.

Yapılandırma ve Azure AD çoklu oturum açma Vodeclic ile test etmek için aşağıdaki yapı taşları tamamlayın:

1. [Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on) bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Vodeclic test kullanıcısı oluşturma](#create-a-vodeclic-test-user) karşılık gelen Britta Simon, kullanıcının Azure AD gösterimini bağlı Vodeclic sağlamak için.
4. [Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user) Britta Azure AD çoklu oturum açma kullanmak Simon etkinleştirmek için.
5. [Test çoklu oturum açma](#test-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Vodeclic uygulamanızda yapılandırın.

<<<<<<< HEAD
**Azure AD çoklu oturum açma ile Vodeclic yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Vodeclic** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_samlbase.png)

3. Üzerinde **Vodeclic etki alanı ve URL'leri** bölümünde, uygulama tarafından başlatılan IDP modunda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Vodeclic etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.lms.vodeclic.net/auth/saml`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.lms.vodeclic.net/auth/saml/callback`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Vodeclic etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.lms.vodeclic.net/auth/saml`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Vodeclic istemci destek ekibi](mailto:hotline@vodeclic.com) bu değerleri almak için.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-vodeclic-tutorial/tutorial_general_400.png)
    
7. Çoklu oturum açma yapılandırmak için **Vodeclic** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [Vodeclic destek ekibi](mailto:hotline@vodeclic.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
=======
**Azure AD çoklu oturum açma ile Vodeclic yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında üzerinde **Vodeclic** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** iletişim kutusunda **çoklu oturum açma modu**seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_samlbase.png)

3. Uygulamada yapılandırmak istiyorsanız, **IDP** modunda başlatılan **Vodeclic etki alanı ve URL'leri** bölümünde, aşağıdaki adımları uygulayın:

    ![Vodeclic etki alanı ve oturum açma URL'leri tek bilgi](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_url.png)

    a. İçinde **tanımlayıcısı** kutusunda, aşağıdaki desende bir URL yazın:`https://<companyname>.lms.vodeclic.net/auth/saml`

    b. İçinde **yanıt URL'si** kutusunda, aşağıdaki desende bir URL yazın:`https://<companyname>.lms.vodeclic.net/auth/saml/callback`

4. Uygulamada yapılandırmak istiyorsanız, **SP** başlatılan modu, select **Göster Gelişmiş URL ayarları** onay kutusunu işaretleyin ve aşağıdaki adımları uygulamanız:

    ![Vodeclic etki alanı ve oturum açma URL'leri tek bilgi](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_url1.png)

    İçinde **oturum açma URL'si** kutusunda, aşağıdaki desende bir URL yazın:`https://<companyname>.lms.vodeclic.net/auth/saml`
     
    > [!NOTE] 
    > Bu değerleri gerçek değil. Gerçek tanımlayıcısı ile bu değerleri güncelleştirmek URL'si ve oturum açma URL'si yanıtlayın. Kişi [Vodeclic istemci destek ekibi](mailto:hotline@vodeclic.com) bu değerleri almak için.

5. İçinde **SAML imzalama sertifikası** bölümünde, select **meta veri XML**. Ardından meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_certificate.png) 

6. **Kaydet**’i seçin.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-vodeclic-tutorial/tutorial_general_400.png)
    
7. Çoklu oturum açma yapılandırmak için **Vodeclic** tarafı, indirilen Gönder **meta veri XML** için [Vodeclic destek ekibi](mailto:hotline@vodeclic.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com) uygulaması kuruluyor sırada. Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesinde ve katıştırılmış erişim belgeleri etraflıca **yapılandırma** alt bölüm. Daha fazla bilgiyi embedded belgeler özelliği hakkında [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

<<<<<<< HEAD
**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
=======
**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları uygulayın:**

1. Azure portalında sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**. Ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

<<<<<<< HEAD
    d. **Oluştur**'a tıklayın.
=======
    d. **Oluştur**’u seçin.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
 
### <a name="create-a-vodeclic-test-user"></a>Vodeclic test kullanıcısı oluşturma

Bu bölümde, Vodeclic içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Vodeclic destek ekibi](mailto:hotline@vodeclic.com) Vodeclic platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

> [!NOTE]
<<<<<<< HEAD
> Uygulama dağıtımı gereksinimi makine Güvenilenler listesine al gerekebilir göre ve söz konusu ortak IP adresiniz paylaşmaya gereksinim [Vodeclic destek ekibi](mailto:hotline@vodeclic.com).
=======
> Uygulama gereksinimlerine göre makine Güvenilenler listesine al gerekebilir. Ortak IP adresi ile paylaşmak, gerçekleşmesi gerekir [Vodeclic destek ekibi](mailto:hotline@vodeclic.com).
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Vodeclic için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

<<<<<<< HEAD
**Vodeclic için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.
=======
**Vodeclic için Britta Simon atamak için aşağıdaki adımları uygulayın:**

1. Azure Portalı'ndaki uygulamaları görünümünü açın ve dizin görünümüne gidin. Ardından, Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Vodeclic**.

    ![Uygulamalar listesinde Vodeclic bağlantı](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_app.png)  

<<<<<<< HEAD
3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Vodeclic parçasında tıklattığınızda, otomatik olarak Vodeclic uygulamanıza açan.
=======
3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** içinde **eklemek atama** iletişim kutusu.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listesi.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **seçin** düğmesi.

7. İçinde **eklemek atama** iletişim kutusunda **atamak** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim panelinde Vodeclic döşeme seçtiğinizde, otomatik olarak Vodeclic uygulamanıza oturum.

>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_203.png

