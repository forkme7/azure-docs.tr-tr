---
title: Azure Data Factory kullanarak bir FTP sunucusundan veri kopyalama | Microsoft Docs
description: Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak bir FTP sunucusundan bir desteklenen havuz veri deposuna kopyalamak öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jingwang
ms.openlocfilehash: d382d1198d84555035d348ac66881c24249c4d95
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="copy-data-from-ftp-server-by-using-azure-data-factory"></a>Azure Data Factory kullanarak FTP sunucusundan veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-ftp-connector.md)
> * [Sürüm 2 - Önizleme](connector-ftp.md)

Bu makalede kopya etkinliği Azure Data Factory'de bir FTP sunucusundan veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 FTP Bağlayıcısı](v1/data-factory-ftp-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna FTP sunucusundan veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu FTP bağlayıcı destekler:

- Dosya kopyalarken kullanarak **temel** veya **anonim** kimlik doğrulaması.
- Dosyaları olarak kopyalama- ya da dosyalarıyla ayrıştırma [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli FTP tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikleri, bağlantılı FTP hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Ftp_sunucusu**. | Evet |
| konak | Adını veya FTP sunucusunun IP adresini belirtin. | Evet |
| port | FTP sunucusunun dinlediği bağlantı noktasını belirtin.<br/>İzin verilen değerler: tamsayı, varsayılan değer: **21**. | Hayır |
| enableSsl | FTP SSL/TLS kanalı üzerinden kullanılıp kullanılmayacağını belirtin.<br/>İzin verilen değerler: **true** (varsayılan), **false**. | Hayır |
| enableServerCertificateValidation | FTP SSL/TLS kanalı üzerinden kullanırken sunucu SSL sertifika doğrulamasını etkinleştirmek bu seçeneği belirtin.<br/>İzin verilen değerler: **true** (varsayılan), **false**. | Hayır |
| authenticationType | Kimlik doğrulama türünü belirtin.<br/>İzin verilen değerler: **temel**, **anonim** | Evet |
| Kullanıcı adı | FTP sunucusuna erişimi olan kullanıcı belirtin. | Hayır |
| password | (Kullanıcı adı) kullanıcının parolasını belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Hayır |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek 1: Anonim kimlik doğrulamasını kullanma**

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "<ftp server>",
            "port": 21,
            "enableSsl": true,
            "enableServerCertificateValidation": true,
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: Temel kimlik doğrulaması kullanma**

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "<ftp server>",
            "port": 21,
            "enableSsl": true,
            "enableServerCertificateValidation": true,
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde, FTP veri kümesi tarafından desteklenen özellikler listesini sağlar.

FTP'den verileri kopyalamak için kümesine tür özelliği ayarlamak **FileShare**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **dosya paylaşımı** |Evet |
| folderPath | Klasör yolu. Örneğin: klasör/alt / |Evet |
| fileName | **Adı veya joker karakter filtresini** belirtilen "folderPath" altında dosyalar için. Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm dosyaları işaret eder. <br/><br/>Filtre için joker karakter verilir: `*` (birden çok karakter) ve `?` (tek bir karakter).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"` |Hayır |
| Biçimi | İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın.<br/><br/>Belirli bir biçime sahip dosyaları ayrıştırma istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**,  **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Json biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquet biçimi](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopyalama senaryosu) |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Daha fazla bilgi için bkz: [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyler: **Optimal** ve **en hızlı**. |Hayır |
| useBinaryTransfer | İkili aktarım modunu kullanıp kullanmayacağınızı belirtin. İkili mod (varsayılan) ve ASCII yanlış true değerlerdir. |Hayır |

>[!TIP]
>Bir klasörü altındaki tüm dosyaları kopyalamak için belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip tek bir dosya kopyalamak için belirtin **folderPath** klasörü bölümüyle ve **fileName** dosya adına sahip.<br>Bir klasör altındaki dosyalar kümesini kopyalamak için belirtin **folderPath** klasörü bölümüyle ve **fileName** joker karakter Filtresi ile.

>[!NOTE]
>Dosya filtresi için "fileFilter" özelliğini kullanıyorsanız, hala olarak desteklenmektedir-"ileride dosyaadı" eklenen yeni filtre özellikten yararlanabilmek için önerilen, açıkken.

**Örnek:**

```json
{
    "name": "FTPDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<FTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "myfile.csv.gz",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, FTP kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="ftp-as-source"></a>Kaynak olarak FTP

FTP'den verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **FileSystemSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **FileSystemSource** |Evet |
| Özyinelemeli | Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. Özyinelemeli true ve havuz için ayarlandığında Not dosya tabanlı depolama, boş klasör/alt-folder havuz kopyalanır ve oluşturulan olmaz.<br/>İzin verilen değerler: **true** (varsayılan), **false** | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<FTP input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).