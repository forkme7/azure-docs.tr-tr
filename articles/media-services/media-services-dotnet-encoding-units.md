---
title: "Medya kodlama birimleri - Azure ekleyerek işleme ölçeğini |  Microsoft Docs"
description: "Bilgi nasıl .NET ile kodlama birimleri eklemek"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2017
ms.author: juliako;milangada;
ms.openlocfilehash: 89203a9499b3624faf41b63f4ea6e7bd29f3f0c9
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="how-to-scale-encoding-with-net-sdk"></a>.NET SDK ile kodlama ölçeklendirme
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-scale-media-processing.md)
> * [.NET](media-services-dotnet-encoding-units.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

> [!NOTE]
> Java SDK'ın en son sürümünü almak ve Java ile geliştirmeye başlamak için bkz. [Media Services için Java istemcisi SDK’sı ile çalışmaya başlama](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Media Services için en yeni PHP SDK'sını indirmek üzere, [Packagist deposunda](https://packagist.org/packages/microsoft/windowsazure#v0.5.7) Microsoft/WindowAzure paketinin 0.5.7 sürümünü arayın.  

## <a name="overview"></a>Genel Bakış
> [!IMPORTANT]
> Gözden geçirdiğinizden emin olun [genel bakış](media-services-scale-media-processing-overview.md) medyayı işleme ölçeklendirme hakkında daha fazla bilgi almak için.
> 
> 

Ayrılmış birim türü ve .NET SDK kullanarak ayrılan birimler kodlama sayısını değiştirmek için aşağıdakileri yapın:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a>Bir destek bileti açmadan

Varsayılan olarak 10 S2 veya S3 medya ayrılmış birimleri (MRU) veya 25 S1 MRU ve 5 isteğe bağlı akışa ayrılan birimler için her Media Services hesabı ölçeklendirebilirsiniz. Bir destek bileti açılarak daha yüksek bir sınır isteyebilir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

