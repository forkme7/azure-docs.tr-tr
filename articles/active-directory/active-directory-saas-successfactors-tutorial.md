---
title: "Öğretici: Azure Active Directory Tümleştirme ile SuccessFactors | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile SuccessFactors arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: b9545599b4ac02927c38931777a3cee623a4e940
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Öğretici: Azure Active Directory Tümleştirme SuccessFactors ile

Bu öğreticide, Azure Active Directory (Azure AD) ile SuccessFactors tümleştirmek öğrenin.

SuccessFactors Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SuccessFactors erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için SuccessFactors (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme SuccessFactors ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SuccessFactors çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SuccessFactors ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-successfactors-from-the-gallery"></a>Galeriden SuccessFactors ekleme
Azure AD SuccessFactors tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SuccessFactors eklemeniz gerekir.

**Galeriden SuccessFactors eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SuccessFactors**seçin **SuccessFactors** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde SuccessFactors](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı SuccessFactors sınayın.

Tekli çalışmaya oturum için Azure AD SuccessFactors karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SuccessFactors ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SuccessFactors içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma SuccessFactors ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SuccessFactors test kullanıcısı oluşturma](#create-a-successfactors-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı SuccessFactors sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SuccessFactors uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile SuccessFactors yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SuccessFactors** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_samlbase.png)

3. Üzerinde **SuccessFactors etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SuccessFactors etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.eu`|

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://www.successfactors.com/<companyname>`|
    | `https://www.successfactors.com`|
    | `https://<companyname>.successfactors.eu`|
    | `https://www.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://hcm4preview.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.eu`|
    | `https://www.successfactors.cn`|
    | `https://www.successfactors.cn/<companyname>`|

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.successfactors.com`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.successfactors.eu`|
    | `https://<companyname>.sapsf.eu`|
    | `https://<companyname>.sapsf.eu/<companyname>`|
    | `https://<companyname>.sapsf.cn`|
    | `https://<companyname>.sapsf.cn/<companyname>`|
         
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [SuccessFactors istemci destek ekibi](https://www.successfactors.com/en_us/support.html) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-successfactors-tutorial/tutorial_general_400.png)
    
6. Üzerinde **SuccessFactors yapılandırma** 'yi tıklatın **yapılandırma SuccessFactors** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_configure.png) 

7. Farklı web tarayıcısı penceresinde oturum açın, **SuccessFactors Yönetici portalı** yönetici olarak.
    
8. Ziyaret **uygulama güvenliği** ve yerel **tek oturum açma özelliğini**. 

9. Herhangi bir değer yerleştirin **sıfırlama belirteci** tıklatıp **Kaydet belirteci** SAML SSO'yu etkinleştirmek için.
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma][11]

    > [!NOTE] 
    > Bu değer, açık/kapalı anahtar kullanılır. Herhangi bir değer kaydettiyseniz, SAML SSO açık'tır. Boş bir değer kaydettiyseniz SAML SSO Kapalı'dır.

10. Yerel ekran görüntüsü aşağıda ve aşağıdaki eylemleri gerçekleştirin:
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma][12]
   
    a. Seçin **SAML v2 SSO** radyo düğmesi
   
    b. Ayarlama **SAML sunduğundan taraf adı**(örneğin, SAML veren + şirket adı).
   
    c. İçinde **veren URL'si** metin kutusuna, Yapıştır **SAML varlık kimliği** Azure portalından kopyaladığınız değeri.
   
    d. Seçin **yanıt (müşteri oluşturulan/IDP/AP)** olarak **zorunlu imza gerektir**.
   
    e. Seçin **etkin** olarak **etkinleştirmek SAML bayrağı**.
   
    f. Seçin **Hayır** olarak **oturum açma isteği imza (BT oluşturulan/SP/RP)**.
   
    g. Seçin **tarayıcı/Post profili** olarak **SAML profili**.
   
    h. Seçin **Hayır** olarak **sertifika geçerli süresi zorunlu**.
   
    ı. Azure Portalı'ndan indirilen sertifika dosyasının içeriğini kopyalayın ve ardından yapıştırın **SAML doğrulama sertifikası** metin kutusu.

    > [!NOTE] 
    > Sertifika içeriği sahip başlamalıdır sertifika ve bitiş sertifika etiketler.

11. SAML V2'ye gidin ve ardından aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma][13]
   
    a. Seçin **Evet** olarak **destek SP tarafından başlatılan genel oturum kapatma**.
   
    b. İçinde **genel oturum kapatma hizmeti URL'si (LogoutRequest hedef)** metin kutusuna, Yapıştır **Sign-Out URL** kopyaladığınız değeri form Azure portalı.
   
    c. Seçin **Hayır** olarak **sp tüm NameID öğesi şifrelemek gerekir gerektiren**.
   
    d. Seçin **belirtilmeyen** olarak **NameID biçimi**.
   
    e. Seçin **Evet** olarak **etkinleştir sp tarafından başlatılan oturum açma (AuthnRequest)**.
   
    f. İçinde **şirket çapında veren olarak gönderme isteği** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri.

12. Oturum açma kullanıcı adları yapmak istiyorsanız bu adımları uygulamadan büyük küçük harfe duyarlı.
   
    ![Çoklu oturum açmayı yapılandırın][29]
    
    a. Ziyaret **şirket ayarları**(yakınında alt).
   
    b. yakın onay kutusunu seçin **etkinleştirmek olmayan durumda duyarlı kullanıcıadı**.
   
    c.Click **kaydetmek**.
   
    > [!NOTE] 
    > Bunu etkinleştirmek çalışırsanız, yinelenen bir SAML oturum açma adı oluşturur sistem denetler. Örneğin, müşterinin kullanıcı adları Kullanıcı1 hem Kullanıcı1 varsa. Büyük küçük harfe duyarlılığın hemen alma bu yinelemeleri yapar. Sistem, bir hata iletisi verir ve özelliğini etkinleştirmez. Farklı yazıldığından biçimde bir kullanıcı adlarını değiştirmek müşteri gerekir.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-successfactors-test-user"></a>SuccessFactors test kullanıcısı oluşturma

Azure AD kullanıcıları için SuccessFactors oturum açmak etkinleştirmek için bunların SuccessFactors sağlanmalıdır.  
SuccessFactors söz konusu olduğunda, sağlama bir el ile bir görevdir.

SuccessFactors içinde oluşturulan kullanıcıların almak için başvurmanız gerekir [SuccessFactors destek ekibi](https://www.successfactors.com/en_us/support.html).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta SuccessFactors için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**SuccessFactors için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SuccessFactors**.

    ![Uygulamalar listesinde SuccessFactors bağlantı](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SuccessFactors parçasında tıklattığınızda, otomatik olarak SuccessFactors uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png

[100]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_203.png

