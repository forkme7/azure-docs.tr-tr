---
title: "Nasıl yapılır Azure Service Fabric Hizmetleri için ortam değişkenleri belirtin | Microsoft Docs"
description: "Service Fabric uygulamaları için ortam değişkenleri kullanmayı gösterir"
documentationcenter: .net
author: mikkelhegn
manager: markfuss
editor: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: d487eeadde9f9a45549763863f8fe5b06b2945a4
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>Service Fabric Hizmetleri için ortam değişkenleri belirtme

Bu makalede, Service Fabric bir hizmet veya kapsayıcı için ortam değişkenleri belirtme gösterilmektedir.

## <a name="procedure-for-specifying-environment-variables-for-services"></a>Hizmetler için ortam değişkenleri belirtmek için yordamı

Bu örnekte, bir ortam değişkeni için bir kapsayıcı ayarlayın. Makale, zaten sahip olduğunuz bir uygulama ve hizmet bildirimi varsayar.

1. ServiceManifest.xml dosyasını açın.
1. İçinde `CodePackage` öğesi, yeni bir ekleme `EnvironmentVariables` öğesi ve bir `EnvironmentVariable` her ortam değişkeni için öğesi.

    ```xml
      <CodePackage Name="MyCode" Version="CodeVersion1">
      ...
        <EnvironmentVariables>
          <EnvironmentVariable Name="MyEnvVariable" Value="DeafultValue"/>
          <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentVariables>
      </CodePackage>
    ```

    Ortam değişkenleri, uygulama bildiriminde geçersiz kılınabilir.

1. Uygulama bildirimini ortam değişkenleri geçersiz kılmak için kullanın `EnvironmentOverrides` öğesi.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede açıklanan kavramları bazıları hakkında daha fazla bilgi için bkz: [birden çok ortamları makaleler için uygulamaları yönetmek](service-fabric-manage-multiple-environment-app-configuration.md).

Visual Studio'da kullanılabilir olan diğer uygulama yönetim özellikleri hakkında daha fazla bilgi için bkz: [Visual Studio'da, Service Fabric uygulamaları yönetmek](service-fabric-manage-application-in-visual-studio.md).