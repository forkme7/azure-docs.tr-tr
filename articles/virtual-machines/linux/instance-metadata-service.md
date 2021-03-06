---
title: Azure örneği meta veri hizmeti | Microsoft Docs
description: Linux VM'ın işlem, ağ ve yaklaşan Bakımı olaylar hakkında bilgi almak için rESTful arabirimi.
services: virtual-machines-linux
documentationcenter: ''
author: harijayms
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/10/2017
ms.author: harijayms
ms.openlocfilehash: 8da7573d8790bae8c54d2055fd355372625eccf7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="azure-instance-metadata-service"></a>Azure örneği meta veri hizmeti


Azure örneği meta veri hizmeti yönetmek ve sanal makinelerinizi yapılandırmak için kullanılan sanal makine örneği çalıştırma hakkında bilgi sağlar.
Bu, SKU, ağ yapılandırması ve yaklaşan Bakımı olayları gibi bilgileri içerir. Hangi tür bilgileri kullanılabilir daha fazla bilgi için bkz: [meta veri kategorileri](#instance-metadata-data-categories).

Azure'nın örnek meta veri hizmeti REST uç noktası aracılığıyla oluşturulan Iaas VM'ler için erişilebilir olan [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Uç nokta iyi bilinen yönlendirilemeyen IP adresinde mevcuttur (`169.254.169.254`), erişilebilir yalnızca VM dahilinde.

> [!IMPORTANT]
> Bu hizmet **genel olarak kullanılabilir** Azure bölgelerindeki.  Düzenli olarak, sanal makine örnekleri ilgili yeni bilgiler kullanıma sunmak için güncelleştirmeleri alır. Bu sayfayı güncel yansıtır [veri kategorilerini](#instance-metadata-data-categories) kullanılabilir.

## <a name="service-availability"></a>Hizmet kullanılabilirliği
Hizmeti genel olarak kullanılabilir Azure bölgelerde kullanılabilir. Tüm API sürümü tüm Azure bölgelerde kullanılabilir.

Bölgeler                                        | Kullanılabilirlik?                                 | Desteklenen Sürümler
-----------------------------------------------|-----------------------------------------------|-----------------
[Tüm genel olarak kullanılabilir genel Azure bölgeleri](https://azure.microsoft.com/regions/)     | Genel olarak kullanılabilir   | 2017-04-02, 2017-08-01, 2017-12-01
[Azure Devlet Kurumları](https://azure.microsoft.com/overview/clouds/government/)              | Genel olarak kullanılabilir | 2017-04-02,2017-08-01
[Azure Çin](https://www.azure.cn/)                                                           | Genel olarak kullanılabilir | 2017-04-02,2017-08-01
[Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)                    | Genel olarak kullanılabilir | 2017-04-02,2017-08-01

Bu tablo hizmet güncelleştirmeleri vardır ve yeni desteklenen sürümler kullanılabilir onayladığında veya

Örneği meta veri hizmeti denemek için bir VM'den oluşturmak [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) veya [Azure portal](http://portal.azure.com) yukarıdaki bölgelerde ve aşağıdaki örnekleri izleyin.

## <a name="usage"></a>Kullanım

### <a name="versioning"></a>Sürüm oluşturma
Örnek meta veri sürümü tutulan hizmetidir. Sürümleri zorunludur ve genel Azure üzerinde geçerli sürümü `2017-12-01`. (2017-04-02, 2017-08-01,2017-12-01) geçerli desteklenen sürümler:

> [!NOTE] 
> Önceki Önizleme sürümleri {son} API sürümü desteklenen zamanlanmış olaylar. Bu biçim artık desteklenmemektedir ve gelecekte kullanım dışı kalacaktır.

Yeni sürümler eklendikçe komut dosyalarınızı belirli veri biçimleri üzerinde bağımlılıkları varsa, daha eski sürümlerini yine de uyumluluk için erişilebilir. Ancak, önceki Önizleme sürümü (2017-03-01) hizmeti genel olarak kullanılabilir olduğunda kullanılamayabilir.

### <a name="using-headers"></a>Üst bilgileri kullanma
Örnek meta veri hizmeti sorguladığınızda başlık sağlamalısınız `Metadata: true` istek istemeden yönlendirilmeyen emin olmak için.

### <a name="retrieving-metadata"></a>Meta veri alma

Örnek meta verileri oluşturulan yönetilen/kullanarak sanal makineleri çalıştırmak için kullanılabilir [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Aşağıdaki isteği kullanarak bir sanal makine örneği için tüm veri kategorilerini erişebilirsiniz:

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01"
```

> [!NOTE] 
> Tüm örnek meta veri sorguları büyük/küçük harfe duyarlıdır.

### <a name="data-output"></a>Veri çıkışı
Varsayılan olarak, örnek meta veri hizmeti verileri JSON biçiminde döndürür (`Content-Type: application/json`). Ancak, farklı API'leri verileri farklı biçimlerde istediyseniz döndürün.
Aşağıdaki tabloda, API destekleyebilir diğer veri biçimlerini başvurudur.

API | Varsayılan veri biçimi | Diğer biçimlere
--------|---------------------|--------------
/instance | json | Metin
/scheduledevents | json | yok

Varsayılan olmayan yanıt biçimi erişmek için bir istek sorgu dizesi parametresi olarak istenen biçim belirtin. Örneğin:

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01&format=text"
```

### <a name="security"></a>Güvenlik
Örnek meta veri Hizmeti uç noktası yalnızca yönlendirilebilir olmayan bir IP adresi üzerinde çalışan sanal makine örneği içinde erişilebilir. Ayrıca, herhangi bir ile istek bir `X-Forwarded-For` üstbilgi hizmeti tarafından reddedildi.
İstekleri de içermelidir bir `Metadata: true` üstbilgi gerçek isteği doğrudan istenen ve istenmeyen yeniden yönlendirme bir parçası olduğundan emin olun. 

### <a name="error"></a>Hata
Bir veri öğesi bulunamadı veya hatalı biçimlendirilmiş bir istek varsa örneği meta veri hizmeti standart HTTP hatalarını döndürür. Örneğin:

HTTP durum kodu | Neden
----------------|-------
200 TAMAM |
400 Hatalı istek | Eksik `Metadata: true` üstbilgisi
404 Bulunamadı | İstenen öğe yok 
405 Yönteme izin verilmiyor | Yalnızca `GET` ve `POST` istekleri desteklenir
429 çok fazla istek | API şu anda en fazla saniyede 5 sorgularını destekler
500 hizmet hatası     | Bir süre sonra yeniden deneyin

### <a name="examples"></a>Örnekler

> [!NOTE] 
> Tüm API yanıtları JSON dizelerdir. Okunabilirlik için pretty yazdırılan tüm aşağıdaki örnek yanıtlar.

#### <a name="retrieving-network-information"></a>Ağ bilgileri alınıyor

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-08-01"
```

**Yanıt**

> [!NOTE] 
> Yanıtı bir JSON dizesidir. Aşağıdaki örnek yanıt okunabilirlik için pretty yazdırılmıştır.

```json
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="retrieving-public-ip-address"></a>Genel IP adresi alınıyor

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-08-01&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a>Bir örneği için tüm meta veri alma

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-12-01"
```

**Yanıt**

> [!NOTE] 
> Yanıtı bir JSON dizesidir. Aşağıdaki örnek yanıt okunabilirlik için pretty yazdırılmıştır.

```json
{
  "compute": {
    "location": "westus",
    "name": "avset2",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "placementGroupId": "",
    "platformFaultDomain": "1",
    "platformUpdateDomain": "1",
    "publisher": "Canonical",
    "resourceGroupName": "myrg",
    "sku": "16.04-LTS",
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "",
    "version": "16.04.201708030",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_D1",
    "zone": "1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.2.5",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.2.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3A36DDED"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>Meta veriler de Windows sanal makinesi alınıyor

**İstek**

Örnek meta verileri alınabilir Windows PowerShell yardımcı programı `curl`: 

```bash
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-08-01 | select -ExpandProperty Content
```

Aracılığıyla veya `Invoke-RestMethod` cmdlet:
    
```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-08-01 -Method get 
```

**Yanıt**

> [!NOTE] 
> Yanıtı bir JSON dizesidir. Aşağıdaki örnek yanıt okunabilirlik için pretty yazdırılmıştır.

```json
{
  "compute": {
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "MicrosoftSQLServer",
    "sku": "Enterprise",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmSize": "Standard_DS2"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": [
            
          ]
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a>Örnek meta veri kategorileri
Aşağıdaki veri kategorilerini örneği meta veri hizmeti kullanılabilir:

Veriler | Açıklama | Sunulan sürüm 
-----|-------------|-----------------------
location | Azure bölgesi VM çalışır durumda | 2017-04-02 
ad | VM adı | 2017-04-02
teklif | VM görüntüsü için bilgi sunar. Bu değer yalnızca Azure resmi Galerisi'nden dağıtılan görüntüleri için mevcuttur. | 2017-04-02
publisher | VM görüntüsü yayımcısı | 2017-04-02
SKU | VM görüntüsü için belirli SKU | 2017-04-02
sürüm | VM görüntüsü | 2017-04-02
osType | Linux veya Windows | 2017-04-02
platformUpdateDomain |  [Güncelleştirme etki alanı](manage-availability.md) VM'nin çalışır durumda | 2017-04-02
platformFaultDomain | [Hata etki alanı](manage-availability.md) VM'nin çalışır durumda | 2017-04-02
vmId | [Benzersiz tanımlayıcı](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) VM | 2017-04-02
vmSize | [VM boyutu](sizes.md) | 2017-04-02
subscriptionId | Sanal makine için Azure aboneliği | 2017-08-01
etiketler | [Etiketler](../../azure-resource-manager/resource-group-using-tags.md) sanal makineniz için  | 2017-08-01
resourceGroupName | [Kaynak grubu](../../azure-resource-manager/resource-group-overview.md) sanal makineniz için | 2017-08-01
placementGroupId | [Yerleştirme grup](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) , sanal makine ölçek kümesi | 2017-08-01
vmScaleSetName | [Sanal makine ScaleSet adı] (.. /.. / virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md), sanal makine ölçek kümesi | 2017-12-01
bölge | [Kullanılabilirlik bölge](../../availability-zones/az-overview.md) sanal makinenizin | 2017-12-01 
ipv4/privateIpAddress | VM yerel IPv4 adresi | 2017-04-02
ipv4/publicIpAddress | VM genel IPv4 adresi | 2017-04-02
alt ağ/adresi | VM alt ağ adresi | 2017-04-02 
alt ağ/öneki | Alt ağ öneki, örnek 24 | 2017-04-02 
ipv6/ipAddress | VM yerel IPv6 adresi | 2017-04-02 
macAddress | VM mac adresi | 2017-04-02 
scheduledevents | Bkz: [zamanlanmış olayları](scheduled-events.md) | 2017-08-01

## <a name="example-scenarios-for-usage"></a>Kullanım için örnek senaryolar  

### <a name="tracking-vm-running-on-azure"></a>Azure üzerinde çalışan VM izleme

Bir hizmet sağlayıcısı olarak yazılımınızı çalışan VM sayısı izlemek veya VM benzersizliğini izlemeniz gereken aracıların gerekebilir. Bir VM için benzersiz bir kimliği elde edebilmek için kullanmak `vmId` örneği meta veri hizmetinden alan.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**Yanıt**

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Yerleştirme kapsayıcıların veri bölümlerini arıza/güncelleştirme etki alanı tabanlı 

Belirli senaryolar, farklı veri çoğaltmaları yerleşimini prime çok önemlidir. Örneğin, [HDFS çoğaltma yerleştirme](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) veya kapsayıcı yerleştirme aracılığıyla bir [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) bilmek gerektirebilir `platformFaultDomain` ve `platformUpdateDomain` VM'nin çalışır.
Ayrıca yararlanabilirsiniz [kullanılabilirlik bölgeleri](../../availability-zones/az-overview.md) bu kararlar almak örnekleri için. Bu veri örneği meta veri hizmeti üzerinden doğrudan sorgulayabilir.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text" 
```

**Yanıt**

```
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a>Destek olayı sırasında VM hakkında daha fazla bilgi alma 

Bir hizmet sağlayıcısı olarak, burada VM hakkında daha fazla bilgiye istediğiniz bir destek çağrısı alabilirsiniz. İşlem meta verileri paylaşmak için müşteri isteyen Azure VM'de türü hakkında bilgi edinmek için profesyonel desteği için temel bilgiler sağlayabilir. 

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-08-01"
```

**Yanıt**

> [!NOTE] 
> Yanıtı bir JSON dizesidir. Aşağıdaki örnek yanıt okunabilirlik için pretty yazdırılmıştır.

```json
{
  "compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }
}
```

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-the-vm"></a>VM içindeki farklı dillerde kullanarak meta veri hizmeti çağrılırken örnekleri 

Dil | Örnek 
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb
Başlayın  | https://github.com/Microsoft/azureimds/blob/master/imdssample.go            
Python   | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
C++      | https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp
C#       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
JavaScript | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
PowerShell | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh
Perl       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.pl
Java       | https://github.com/Microsoft/azureimds/blob/master/imdssample.java
Visual Basic | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.vb
    

## <a name="faq"></a>SSS
1. Hata alıyorum `400 Bad Request, Required metadata header not specified`. Bu ne anlama geliyor?
   * Üst bilgisi örneği meta veri hizmeti gerektiriyor `Metadata: true` istekte geçirilecek. Bu üst REST çağrısı geçirme örneği meta veri hizmeti erişmesini sağlar. 
2. Neden VM'im için işlem bilgi alıyorum değil mi?
   * Şu anda örneği meta veri hizmeti, yalnızca Azure Kaynak Yöneticisi ile oluşturulan örnekleri destekler. Bulut hizmeti VM'ler eklenebilir gelecekte desteği.
3. Sanal Makinem Azure Resource Manager aracılığıyla geri bir süre oluşturdum. Bkz: değil neden ben meta veri bilgileri işlem?
   * Eylül 2016 sonra oluşturulan tüm VM'ler için ekleme bir [etiketi](../../azure-resource-manager/resource-group-using-tags.md) görmeye başlayacaksınız için meta veri işlem. (Eylül 2016 öncesinde oluşturulan) eski VM'ler için ekleme/meta verilerini yenilemek için VM uzantıları veya veri diski Kaldır.
4. 2017-08-01 yeni sürümü için doldurulmuş tüm verileri görüyorum değil
   * Eylül 2016 sonra oluşturulan tüm VM'ler için ekleme bir [etiketi](../../azure-resource-manager/resource-group-using-tags.md) görmeye başlayacaksınız için meta veri işlem. (Eylül 2016 öncesinde oluşturulan) eski VM'ler için ekleme/meta verilerini yenilemek için VM uzantıları veya veri diski Kaldır.
5. Neden iletisi alıyorum hata `500 Internal Server Error`?
   * Üstel geri alma sistem göre isteğinizi yeniden deneyin. Sorun devam ederse Azure desteğine başvurun.
6. Burada ek soruları/açıklamalar paylaşmak?
   * Yorumlarınızı gönderme http://feedback.azure.com.
7. Bu sanal makine ölçek kümesi örneği için çalışır?
   * Evet meta veri hizmeti için ölçek kümesi örnek kullanılabilir. 
8. Hizmet için nasıl destek alma?
   * Hizmeti için destek almak için uzun denemeden sonra meta veri yanıtı alabilir olduğunuz değil VM için Azure portalında bir destek sorununu oluşturma 

   ![Örnek meta verileri desteği](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [zamanlanmış olayları](scheduled-events.md)

