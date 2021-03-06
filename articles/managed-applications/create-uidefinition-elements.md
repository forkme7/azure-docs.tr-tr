---
title: Azure UI tanım öğesi oluşturun | Microsoft Docs
description: Azure portal UI tanımlarında oluşturulurken kullanılacak öğelerini açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: d6f96d4aa66839518023b4d567caf1ff839a29fb
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="createuidefinition-elements"></a>CreateUiDefinition öğeleri
Bu makalede, şema ve tüm desteklenen bir CreateUiDefinition öğelerinin özelliklerini açıklar. Öğelerin çoğu için şemayı aşağıdaki gibidir:

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "my value",
  "toolTip": "Provide a descriptive name.",
  "constraints": {},
  "options": {},
  "visible": true
}
```

| Özellik | Gerekli | Açıklama |
| -------- | -------- | ----------- |
| ad | Evet | Belirli bir öğenin örneği başvurmak için kullanılan bir iç tanımlayıcı. Öğe adı en yaygın kullanımı yer `outputs`, burada belirtilen öğelerinin çıkış değerleri şablon parametrelerinin eşlenir. Ayrıca, bir öğeye çıkış değeri bağlamak için kullanabileceğiniz `defaultValue` başka bir öğenin. |
| type | Evet | Öğe için işlemek için kullanıcı Arabirimi denetim. Desteklenen türlerinin listesi için bkz: [öğeleri](#elements). |
| Etiket | Evet | Öğenin görüntüleme metni. Değer, birden çok dizeyi içeren bir nesne olabilir şekilde birden çok etiketleri, bazı öğe türleri içerir. |
| defaultValue | Hayır | Öğesinin varsayılan değeri. Bazı öğe türleri karmaşık varsayılan değerleri desteğini değer bir nesne olabilir. |
| toolTip | Hayır | Öğenin araç ipucu görüntülenecek metin. Benzer şekilde `label`, bazı öğeler birden çok araç ipucu dizeleri destekler. Markdown söz dizimini kullanarak satır içi bağlantı katıştırılabilir.
| Kısıtlamaları | Hayır | Öğeyi doğrulama davranışını özelleştirmek için kullanılan bir veya daha fazla özellikleri. Kısıtlamalar için desteklenen özellikler öğesi türüne göre değişir. Bazı öğe türleri doğrulama davranışını özelleştirmesini desteklemiyor ve bu nedenle hiçbir kısıtlamaları özelliğine sahiptir. |
| seçenekler | Hayır | Öğe davranışını özelleştiren ek özellikler. Benzer şekilde `constraints`, desteklenen özellikler öğesi türüne göre farklılık gösterir. |
| Görünür | Hayır | Öğenin görüntülenip görüntülenmeyeceğini belirtir. Varsa `true`, öğesi ve ilgili alt öğeleri görüntülenir. Varsayılan değer `true`. Kullanım [mantıksal işlevleri](create-uidefinition-functions.md#logical-functions) dinamik olarak bu özelliğin değerini denetlemek için.

## <a name="elements"></a>Öğeleri

Belge Şeması, bir kullanıcı Arabirimi örnek her öğe içeriyor için öğe (genellikle ilgili doğrulama ve desteklenen özelleştirme) ve örnek çıktı davranışını açıklamalar.

- [Microsoft.Common.DropDown](microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](microsoft-common-fileupload.md)
- [Microsoft.Common.OptionsGroup](microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](microsoft-common-section.md)
- [Microsoft.Common.TextBox](microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](microsoft-compute-usernametextbox.md)
- [Microsoft.Network.PublicIpAddressCombo](microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Sonraki adımlar
UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
