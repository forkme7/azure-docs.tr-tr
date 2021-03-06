---
title: Azure Service Fabric uygulama parolaları yönetme | Microsoft Docs
description: Service Fabric uygulaması gizli değerleri güvenli öğrenin.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: vturecek
ms.openlocfilehash: 931667509a9aa5e898cd01ad26ff046e30acd3fe
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-secrets-in-service-fabric-applications"></a>Service Fabric uygulamaları gizli anahtarları Yönet
Bu kılavuz, Service Fabric uygulaması parolalarında yönetme adım adım anlatılmaktadır. Gizli depolama bağlantı dizeleri, parolalar veya düz metin olarak işleneceğini olmayan diğer değerleri gibi herhangi bir önemli bilgi olabilir.

[Azure anahtar kasası] [ key-vault-get-started] burada Azure Service Fabric kümeleri yüklü sertifikaları almak için bir yol ve sertifikalar için bir güvenli depolama konumu olarak kullanılır. Azure'a dağıtma değil, anahtar kasası Service Fabric uygulamaları parolalarında yönetmek üzere kullanmak gerekmez. Ancak, *kullanarak* gizli bir uygulamada herhangi bir yerde barındırılan bir kümeye dağıtılacak uygulamalar izin vermek için platform belirsiz bulut. 

## <a name="obtain-a-data-encipherment-certificate"></a>Veri şifreleme sertifikası alın
Veri şifreleme sertifikası kesinlikle şifreleme için kullanılır ve şifre çözme yapılandırmasının bir hizmetin Settings.xml değerleri ve kimlik doğrulaması için kullanılan veya şifre metin imzalama değil. Sertifikanın aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika bir özel anahtar içermelidir.
* Sertifikanın bir kişisel bilgi değişimi (.pfx) dosyasına dışarı aktarılabilir olarak, anahtar değişimi için oluşturulmuş olması gerekir.
* Sertifika anahtar kullanımı veri şifreleme (10) içermelidir ve sunucu kimlik doğrulaması veya istemci kimlik doğrulaması içermemesi gerekir. 
  
  Örneğin, PowerShell kullanarak otomatik olarak imzalanan sertifika oluşturulurken `KeyUsage` bayrağını ayarlayın, `DataEncipherment`:
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Kümenizdeki sertifikayı yükleme
Bu sertifika, kümedeki her düğümde yüklenmelidir. Bu çalışma zamanında bir hizmetin Settings.xml depolanan değerleri şifresini çözmek için kullanılır. Bkz: [Azure Kaynak Yöneticisi'ni kullanarak bir küme oluşturmak nasıl] [ service-fabric-cluster-creation-via-arm] kurulum yönergeleri. 

## <a name="encrypt-application-secrets"></a>Uygulama parolaları şifrelemek
Bir uygulama dağıtırken, sertifika gizli değerlerle şifrelemek ve bir hizmetin Settings.xml yapılandırma dosyasına Ekle. Service Fabric SDK yerleşik gizli şifreleme ve şifre çözme işlevi vardır. Gizli değerleri yerleşik anda şifrelenmiş şifresi ve program aracılığıyla hizmet kodunda okuyun. 

Aşağıdaki PowerShell komutunu bir gizli anahtarı şifrelemek için kullanılır. Bu komut, yalnızca değeri şifreler; Mevcut **değil** şifre metnin imzalayın. Ciphertext gizli değerleri için üretmek için kümenizdeki yüklü aynı şifreleme sertifikası kullanmanız gerekir:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

Sonuçta elde edilen base 64 kodlu bir dize hem gizli ciphertext yanı sıra, şifrelemek için kullanılan sertifikayla ilgili bilgileri içerir.  Base-64 kodlu bir dize hizmetinizin Settings.xml yapılandırma dosyasıyla parametresinde eklenebilir `IsEncrypted` özniteliğini `true`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a>Uygulama örnekleri uygulama parolaları Ekle
İdeal olarak, farklı ortamlar için dağıtım olarak otomatik olmalıdır. Bu, gizli şifreleme bir yapı ortamında gerçekleştirme ve uygulama örnekleri oluşturulurken parametre olarak şifrelenmiş parolalar sağlama gerçekleştirilebilir.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Geçersiz kılınabilir parametreler Settings.xml kullanın
Settings.xml yapılandırma dosyası uygulama oluşturma sırasında sağlanan geçersiz kılınabilir parametrelere izin verir. Kullanım `MustOverride` parametresi için bir değer sağlamak yerine özniteliği:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

Settings.xml değerleri geçersiz kılmak için ApplicationManifest.xml hizmeti için bir geçersiz kılma parametresi bildirin:

```xml
<ApplicationManifest ... >
  <Parameters>
    <Parameter Name="MySecret" DefaultValue="" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides>
      <ConfigOverride Name="Config">
        <Settings>
          <Section Name="MySettings">
            <Parameter Name="MySecret" Value="[MySecret]" IsEncrypted="true" />
          </Section>
        </Settings>
      </ConfigOverride>
    </ConfigOverrides>
  </ServiceManifestImport>
 ```

Değer olarak belirtilebilir şimdi bir *uygulama parametresi* uygulamasının bir örneğini oluştururken. Uygulama örneğini oluşturmak komut dosyasıyla PowerShell kullanarak veya bir derleme işlemindeki kolay tümleştirme için C# ile yazılmış.

PowerShell kullanarak, parametre için sağlanan `New-ServiceFabricApplication` olarak komutunu bir [karma tablosu](https://technet.microsoft.com/library/ee692803.aspx):

```powershell
PS C:\Users\vturecek> New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

C# kullanarak, uygulama parametreleri belirtilen bir `ApplicationDescription` olarak bir `NameValueCollection`:

```csharp
FabricClient fabricClient = new FabricClient();

NameValueCollection applicationParameters = new NameValueCollection();
applicationParameters["MySecret"] = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=";

ApplicationDescription applicationDescription = new ApplicationDescription(
    applicationName: new Uri("fabric:/MyApp"),
    applicationTypeName: "MyAppType",
    applicationTypeVersion: "1.0.0",
    applicationParameters: applicationParameters)
);

await fabricClient.ApplicationManager.CreateApplicationAsync(applicationDescription);
```

## <a name="decrypt-secrets-from-service-code"></a>Hizmet kodundan gizli şifresini çözme
Gizli anahtarı şifrelemek için kullanılan şifreleme sertifikası ile şifresini çözerek Settings.xml dışında şifrelenmiş değerler okuyabilir. Service Fabric Hizmetleri'nde altında ağ hizmeti varsayılan olarak Windows üzerinde çalışan ve bazı ek kurulum olmadan düğümde yüklü olan sertifikalar erişiminiz yok.

Ne zaman emin ağ hizmeti yapmanıza gerek veri şifreleme sertifikasını kullanarak veya hangi kullanıcı hizmet hesabı altında çalışan sertifikanın özel anahtarı erişebilir. Service Fabric, bunu yapmak için yapılandırırsanız erişim hizmetiniz için otomatik olarak verme işleyecek. Bu yapılandırma, kullanıcılar ve sertifikalar için güvenlik ilkelerini tanımlayarak ApplicationManifest.xml yapılabilir. Aşağıdaki örnekte, ağ hizmeti hesabının, parmak izi tarafından tanımlanan bir sertifika okuma erişimi verilir:

```xml
<ApplicationManifest … >
    <Principals>
        <Users>
            <User Name="Service1" AccountType="NetworkService" />
        </Users>
    </Principals>
  <Policies>
    <SecurityAccessPolicies>
      <SecurityAccessPolicy GrantRights=”Read” PrincipalRef="Service1" ResourceRef="MyCert" ResourceType="Certificate"/>
    </SecurityAccessPolicies>
  </Policies>
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```

> [!NOTE]
> Sertifika parmak izi sertifika deposu ek bileşeninden Windows kopyalarken görünmeyen bir karakter parmak izi dizenin başında yerleştirilir. Bu görünmez karakter çalışırken bir sertifika parmak izi tarafından bulmak için bu nedenle bu ek karakter silmek istediğinizden emin bir hata neden olabilir.
> 
> 

### <a name="use-application-secrets-in-service-code"></a>Uygulama parolaları hizmetini kullanma
Kolay olan değerleri şifresini çözmek için bir yapılandırma paketi Settings.xml yapılandırma değerlerini erişmek için API sağlar `IsEncrypted` özniteliği kümesine `true`. Şifrelenmiş metin şifreleme için kullanılan sertifikayla ilgili bilgileri içerdiğinden, el ile Sertifika Bul gerekmez. Sertifika yalnızca hizmet üzerinde çalıştığı düğüm üzerinde yüklü olması gerekir. Basit Arama `DecryptValue()` yöntemi özgün gizli değer almak için:

```csharp
ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");
SecureString mySecretValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue()
```

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi edinmek [uygulama ve hizmet güvenliği](service-fabric-application-and-service-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-and-service-manifests.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-secret-management/overview.png
