---
title: Azure Data Factory varlıklarını adlandırma kuralları | Microsoft Docs
description: Data Factory varlıklarını için adlandırma kurallarını açıklar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 0311c2e2223c60861d2b6c87bfa7747dc079856d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - adlandırma kuralları
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [adlandırma kurallarını sürüm 2 veri fabrikasında](../naming-rules.md).

Aşağıdaki tabloda için Data Factory yapıtlarının adlandırma kuralları sağlar.

| Ad | Ad benzersizliği | Doğrulama denetimleri |
|:--- |:--- |:--- |
| Data Factory |Microsoft Azure arasında benzersiz. Adları büyük küçük harf duyarsız, diğer bir deyişle, `MyDF` ve `mydf` aynı veri fabrikası bakın. |<ul><li>Her veri fabrikası tam olarak bir Azure aboneliğine bağlıdır.</li><li>Nesne adları bir harf veya sayı ile başlamalı ve yalnızca harf, rakam ve tire (-) karakterini içerebilir.</li><li>Her tire (-) karakterinden hemen önünde ve bir harf veya sayı gelmelidir gerekir. Kapsayıcı adlarında art arda tirelere izin verilmez.</li><li>Adı 3-63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlı hizmetler/tablolar/işlem hatları |Veri Fabrikası'nda ile benzersiz. Adları büyük/küçük harfe duyarsızdır. |<ul><li>Bir tablo adı karakter sayısı: 260.</li><li>Nesne adları bir harf, sayı veya alt çizgi (_) ile başlamalıdır.</li><li>Şu karakterler kullanılamaz: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul> |
| Kaynak Grubu |Microsoft Azure arasında benzersiz. Adları büyük/küçük harfe duyarsızdır. |<ul><li>En fazla karakter sayısı: 1000.</li><li>Ad harf, rakam ve şu karakterleri içerebilir: "-", "_",","ve"."</li></ul> |

