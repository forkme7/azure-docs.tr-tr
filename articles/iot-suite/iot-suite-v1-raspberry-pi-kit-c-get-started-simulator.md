---
title: "Azure IOT paketi C ile sanal telemetriyi kullanarak Raspberry Pi'yi bağlanma | Microsoft Docs"
description: "Microsoft Azure IOT Starter Kit Böğürtlenli pi 3 kullanın ve Azure IOT paketi. Uzaktan izleme çözümüne Raspberry Pi'yi bağlanmak için kullanım C buluta sanal telemetriyi göndermek ve çözüm panodan çağrılan yöntemler yanıt verir."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 19b16bff7874a03578b8766668c431553da0d875
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-simulated-telemetry-using-c"></a>Raspberry Pi 3 Uzaktan izleme çözümüne bağlama ve C kullanarak sanal telemetriyi Gönder

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Bu öğretici Raspberry Pi 3 buluta göndermek için sıcaklık ve nem veri benzetimini yapmak için nasıl kullanılacağını gösterir. Öğretici kullanır:

- Raspbian işletim sistemi, C programlama dili ve C için Microsoft Azure IOT SDK'sı bir örnek aygıt uygulamak için.
- IOT paketi Uzaktan izleme çözümü bulut tabanlı arka uç önceden yapılandırılmış.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-v1-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> Uzaktan izleme çözümü Azure aboneliğinizde Azure Hizmetleri kümesi sağlar. Dağıtım gerçek Kurumsal Mimarisi yansıtır. Gereksiz Azure tüketim ücretleri önlemek için kendisiyle tamamladığınızda azureiotsuite.com önceden yapılandırılmış çözüm örneğiniz silin. Önceden yapılandırılmış çözümü yeniden gerekiyorsa, kolayca yeniden oluşturabilirsiniz. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz: [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri tanıtım amacıyla][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-the-sample"></a>İndirme ve örnek yapılandırma

Şimdi, indirin ve Uzaktan izleme istemci uygulaması, Raspberry Pi'yi yapılandırın.

### <a name="clone-the-repositories"></a>Depoları kopyalama

Zaten yapmadıysanız, bir terminal, Pi üzerinde aşağıdaki komutları çalıştırarak gerekli depoları kopyalama:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>Güncelleştirme cihaz bağlantı dizesi

Örnek kaynak dosyasında açma **nano** Düzenleyicisi aşağıdaki komutu kullanarak:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/remote_monitoring/remote_monitoring.c
```

Aşağıdaki satırları bulun:

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

Aygıt ve oluşturulan ve bu öğreticinin başlangıcında kaydedilen IOT hub'ı bilgi yer tutucu değerlerini değiştirin. Değişikliklerinizi kaydetmek (**Ctrl-O**, **Enter**) ve düzenleyiciden çıkın (**Ctrl-X**).

## <a name="build-the-sample"></a>Örnek oluşturma

Önkoşul, bir terminal Raspberry Pi'yi üzerinde aşağıdaki komutları çalıştırarak, C için Microsoft Azure IOT cihaz SDK yükleyin:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

Bu gibi durumlarda, güncelleştirilmiş örnek çözümü şimdi Raspberry Pi'yi oluşturabilirsiniz:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
```

Örnek program Raspberry Pi'yi şimdi çalıştırabilirsiniz. Aşağıdaki komutu girin:

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

Aşağıdaki örnek çıkış Raspberry Pi'yi komut satırına bakın çıkış örneğidir:

![Böğürtlenli Pi uygulamadan çıktı][img-raspberry-output]

Tuşuna **Ctrl-C** herhangi bir zamanda programı'ndan çıkmak için.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [Azure IOT Geliştirme Merkezi](https://azure.microsoft.com/develop/iot/) daha fazla örnekleri ve Azure IOT belgeler.

[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-simulator/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
