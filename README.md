﻿## <a name="microsoft-open-source-code-of-conduct"></a>Microsoft açık kaynak kullanım kuralları

Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur.

Daha fazla bilgi için [Kullanım Kuralları SSS](https://opensource.microsoft.com/codeofconduct/faq/) sayfasına bakın. Başka sorularınız ya da yorumlarınız varsa bunları [opencode@microsoft.com](mailto:opencode@microsoft.com) adresine gönderebilirsiniz.

## <a name="contribute-to-azure-technical-documentation"></a>Azure teknik belgelerine katkıda bulunun
Biz topluluğumuz yanı sıra Microsoft çalışanları belgelerine takımlar dışında Katkıları Hoş Geldiniz. Nasıl katkıda kimin size bağlıdır ve değişiklikleri sıralama katkıda ister misiniz:

* **Topluluk - küçük güncelleştirmeler**: Küçük katkılar yapmak istiyorsanız makaleyi bu depoda bulabilir veya [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure) adresinde makaleye gidebilirsiniz. Daha sonra makaledeki **Düzenle** bağlantısına tıklayarak GitHub'da makalenin kaynağına gidin. Güncelleştirmelerinizi GitHub kullanıcı arabirimini kullanarak yapabilirsiniz. İsterseniz depo için çatal oluşturarak güncelleştirmeleri bu çataldan da gönderebilirsiniz. 

* **Topluluk - yeni makaleler + önemli değişiklikler**: Azure parçası iseniz topluluk ve istediğiniz yeni bir makale oluşturmak veya önemli değişiklikler göndermek Lütfen belgeleri ekibi ile bir konuşma başlatmak için bir sorun gönderin. Bir plana kabul sonra bu yeni içerik iş bir birleşimi ortak ve özel depoları taşımanıza yardımcı olmak için bir çalışan çalışmak gerekir. 

* **Çalışanlar**: bir teknik yazar, program yöneticisi olduğunuz ya da bir Azure hizmeti ve bu ürün ekibinin Geliştirici işinizi katkıda veya teknik makaleleri yazmak için ise, özel deposu (https://github.com/ kullanmanız gerekir MicrosoftDocs/azure-docs-pr). Çalışanlar Microsoft world diğer kısımlarından ortak depodaki küçük güncelleştirmeler için kullanmanız gerekir.

* **Çalışanlar**: Bir Azure hizmetinin ürün takımında çalışan teknik yazar, program yöneticisi veya yazılım geliştiriciyseniz ve işiniz bu depoya katkıda bulunmak veya teknik yazı yazmaksa çalışmanızı özel depoda yapmalısınız ([https://github.com/MicrosoftDocs/azure-docs-pr](https://github.com/MicrosoftDocs/azure-docs-pr)). Var olan bir makalede önemli değişiklikler yapacaksanız, resimler ekleyip, değiştirecek, yeni makale katkısında bulunacaksanız bu depoyu forklamalı, Git Bash yüklemeli, bir markdown editörü edinmeli ve git komutları öğrenmelisiniz. Detaylar için [İç Katkıda bulunanlar Kılavuzu](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master)'na göz atabilirsiniz.
## <a name="about-your-contributions-to-azure-content"></a>Azure içeriğine katkılarınız hakkında
### <a name="minor-corrections"></a>Küçük düzeltmeler
Bu depodaki belge veya kod örnekleri için yaptığınız küçük düzeltmeler veya açıklamalar [docs.microsoft.com Kullanım Koşulları](https://docs.microsoft.com/legal/termsofuse) kapsamındadır.

### <a name="larger-submissions-from-community-members"></a>Topluluk üyelerinden daha büyük gönderiler
Belge ve kod örnekleri için önemli değişiklikler içeren bir çekme isteği gönderirseniz, çevrimiçi bir katkı lisansı Sözleşmesi (CLA) göndermek için isteyen çekme isteğinde bir ileti görürsünüz. Çekme isteğinize gözden geçirebilirsiniz önce çevrimiçi formu doldurmanız gerekir.

## <a name="tools-and-setup"></a>Araçlar ve Kurulum
Topluluğa katkıda bulunanlar GitHub kullanıcı arabirimini kullanın veya katkıda depoyu çatallaştırma - daha fazla bilgi bulunur bizim [katkıda bulunanlar Kılavuzu](https://docs.microsoft.com/contribute). 

## <a name="repository-organization"></a>Depo düzeni
azure-docs deposundaki tüm içerik https://docs.microsoft.com/azure adresindeki belge düzenini takip eder. Bu depoda iki kök klasör bulunur: 

### <a name="articles"></a>\articles
*\articles* klasörü *.md* uzantılı markdown dosyaları olarak biçimlendirilmiş belge makalelerini içerir. Bu makaleler, genellikle Azure hizmetine göre gruplandırılır.

*\articles* klasörü kök dizindeki makalelerin medya dosyaları için *\media* adlı bir klasör içerir. Bu klasörün içinde her makalenin resim dosyalarını içeren alt klasörler bulunur. Hizmet klasörlerinde her bir hizmet klasöründeki makaleler için ayrı bir medya klasörü vardır. Makale resim klasörleri, *.md* dosya uzantısı dışında makale dosyaları ile aynı ada sahiptir. 

### <a name="includes"></a>\includes
Bir veya daha fazla makaleye dahil etmek üzere oluşturduğunuz yeniden kullanılabilir içerik bölümleri burada bulunur. 

## <a name="how-to-use-markdown-to-format-your-topic"></a>Konunuzu biçimlendirmek için markdown kullanma
Bu depodaki tüm makaleler GitHub’a uygun markdown kullanır. Markdown ile bilmiyorsanız, bkz:

* [Markdown temel bilgileri](https://help.github.com/articles/markdown-basics/)
* [Yazdırılabilir markdown ipucu sayfası](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

## <a name="labels"></a>Etiketler

Genel kullanıma açık azure-docs deposunda, çekme istekleri iş akışını yönetmemize yardımcı olması için çekme isteklerine otomatik etiketler atanır. Bu etiketler aynı zamanda çekme isteğinde bulunanlara isteklerinin durumu ile ilgili bilgi vermeye de olanak sağlar.

* **Yazmak için gönderilen değişiklik**: Yazar bekleyen çekme isteği bildirildi.
* **hazır birleştirme**: çekme isteği gözden geçirme ekibimiz tarafından gözden geçirme için hazır.



