---
title: "Azure AD v2 ASP.NET Web sunucusu alma başlatıldı - Config | Microsoft Docs"
description: "Microsoft oturum açma Openıd Connect standardını kullanan geleneksel web tarayıcı tabanlı bir uygulama ile ASP.NET çözümünü uygulama"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 0c627802ccfba230dcde2dafffee26cb1c895791
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
## <a name="create-an-application-express"></a>(Hızlı) uygulama oluşturma
Uygulamanızı kaydetmeniz gerekir artık *Microsoft uygulama kayıt portalı*:
1. Uygulamanızı aracılığıyla kaydetme [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Uygulamanız ve e-posta için bir ad girin
3.  Kurulum destekli seçeneğinin işaretli olduğundan emin olun
4.  Uygulamanıza bir yeniden yönlendirme URL'si eklemek için yönergeleri izleyin

## <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Uygulama kayıt bilgilerinizi çözümünüze (Gelişmiş) ekleyin
Uygulamanızı kaydetmeniz gerekir artık *Microsoft uygulama kayıt portalı*:
1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app) bir uygulamayı kaydetmek için
2. Uygulamanız ve e-posta için bir ad girin 
3.  Destekli kurulumu için seçeneğinin işaretli olduğundan emin olun
4.  Tıklatın `Add Platform`sonra seçin`Web`
5.  Visual Studio'ya geri dönün ve Çözüm Gezgini'nde, projeyi seçin ve ardından (bir özellik penceresinde F4 tuşuna basın görmüyorsanız) özellikleri penceresine bakın gidin
6.  Değişiklik SSL etkin`True`
7.  SSL URL'sini kopyalayın ve bu URL yönlendirme URL'si listesine kayıt Portalı'nın listesinde yönlendirme URL'si ekleyin:<br/><br/>![Proje Özellikleri](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
8.  Aşağıdakileri ekleyin `web.config` bölümünün altında kök klasöründe yer `configuration\appSettings`:

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
Değiştir `ClientId` yalnızca kayıtlı uygulama kimliği
</li>
<li>
Değiştir `redirectUri` projenizin SSL URL'si
</li>
</ol>
<!-- End Docs -->
