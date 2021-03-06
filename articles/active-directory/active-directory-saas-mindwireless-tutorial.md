---
title: 'Öğretici: Azure Active Directory Tümleştirme ile mindWireless | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile mindWireless arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bd00a339-27c9-4904-b66f-a95bf597ac3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: jeedes
ms.openlocfilehash: 1c08b0fe24e2545a07e3335cc0e0ae87a2c0540c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="tutorial-azure-active-directory-integration-with-mindwireless"></a>Öğretici: Azure Active Directory Tümleştirme mindWireless ile

Bu öğreticide, Azure Active Directory (Azure AD) ile mindWireless tümleştirmek öğrenin.

MindWireless Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- MindWireless erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Azure AD hesaplarına sahip (çoklu oturum açma) mindWireless için açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme mindWireless ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir mindWireless çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden mindWireless ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-mindwireless-from-the-gallery"></a>Galeriden mindWireless ekleme
Azure AD mindWireless tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden mindWireless eklemeniz gerekir.

**Galeriden mindWireless eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **mindWireless**seçin **mindWireless** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde mindWireless](./media/active-directory-saas-mindwireless-tutorial/tutorial_mindwireless_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı mindWireless sınayın.

Tekli çalışmaya oturum için Azure AD mindWireless karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının mindWireless ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma mindWireless ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[MindWireless test kullanıcısı oluşturma](#create-a-mindwireless-test-user)**  - bir Britta Simon karşılık gelen kullanıcı Azure AD gösterimini bağlı mindWireless içinde olması.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma mindWireless uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile mindWireless yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **mindWireless** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-mindwireless-tutorial/tutorial_mindwireless_samlbase.png)

3. Üzerinde **mindWireless etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![mindWireless etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-mindwireless-tutorial/tutorial_mindwireless_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.mwsmart.com/`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.mwsmart.com/SAML/AssertionConsumerService.aspx`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [mindWireless destek ekibi](mailto:sdulloor@mindwireless.com) bu değerleri almak için.

4. MindWireless uygulaması SAML onaylar SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde bekliyor.

5. Aşağıdaki ekran görüntüsü için bir örneği gösterir. Talep adı her zaman olması **çalışan kimliği** ve hangisinin biz eşlenen kullanıcı EmployeeID içeren user.employeeid değer. Eşleşen kullanıcıya Azure AD'den mindWireless üzerinde EmployeeID burada yapılır, ancak Ayrıca uygulama ayarlarınıza göre farklı bir değere eşleme. Çalışabileceğiniz [mindWireless destek ekibi](mailto:sdulloor@mindwireless.com) önce bir kullanıcının doğru tanıtıcısı kullanın ve bu değeri ile eşlemek için **çalışan kimliği** talep.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-mindwireless-tutorial/tutorial_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri | Namespace değeri |
    | -------------- | --------------- | ----------------|
    | Çalışan Kimliği | User.employeeid | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-mindwireless-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-mindwireless-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. İçinde **Namespace** metin kutusuna, ilgili satır için gösterilen ad alanı değeri yazın.
    
    e. **Tamam**’a tıklayın.
    
7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-mindwireless-tutorial/tutorial_mindwireless_certificate.png) 

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-mindwireless-tutorial/tutorial_general_400.png)

9. Üzerinde **mindWireless yapılandırma** 'yi tıklatın **mindWireless yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![mindWireless yapılandırma](./media/active-directory-saas-mindwireless-tutorial/tutorial_mindwireless_configure.png) 

10. Çoklu oturum açma yapılandırmak için **mindWireless** yan, indirilen göndermek için ihtiyacınız **Certificate(Base64), SAML çoklu oturum açma hizmet URL'si**, ve **SAML varlık kimliği** için [mindWireless destek ekibi](mailto:sdulloor@mindwireless.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-mindwireless-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-mindwireless-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-mindwireless-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-mindwireless-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-mindwireless-test-user"></a>MindWireless test kullanıcısı oluşturma

Bu bölümde, mindWireless içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [mindWireless destek ekibi](mailto:sdulloor@mindwireless.com) mindWireless platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta mindWireless erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**MindWireless için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **mindWireless**.

    ![Uygulamalar listesinde mindWireless bağlantı](./media/active-directory-saas-mindwireless-tutorial/tutorial_mindwireless_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli mindWireless parçasında tıklattığınızda, otomatik olarak mindWireless uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mindwireless-tutorial/tutorial_general_203.png

