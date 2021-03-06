---
title: "Benzetimli Uzaktan izleme çözümü - Azure cihaz davranışını | Microsoft Docs"
description: "Bu makalede Uzaktan izleme çözümünde bir sanal cihaz davranışını tanımlamak için JavaScript kullanmayı açıklar."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/29/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: e5846893166c3e65b75e84d02849c2b8ab78e079
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="implement-the-device-model-behavior"></a>Cihaz modeli davranışına uygulama

Makaleyi [aygıt model şeması anlamak](iot-suite-remote-monitoring-device-schema.md) bir sanal cihaz modeli tanımlayan şema açıklanmaktadır. Bu makale, bir sanal cihaz davranışını uygulayan iki tür JavaScript dosyası için adlandırılır:

- **Durum** JavaScript dosyaları, aygıtın iç durumunu güncelleştirmek için sabit aralıklarla çalıştırın.
- **Yöntem** JavaScript dosyaları çözüm aygıtta bir yöntemi çağırır oluşturulduğunda çalışan.

Bu makalede, bilgi nasıl yapılır:

>[!div class="checklist"]
> * Bir sanal cihaz durumunu denetle
> * Nasıl bir sanal cihaz reponds yöntemine çağrı Uzaktan izleme çözümü tanımlayın
> * Komut dosyalarınızı hata ayıklama

## <a name="state-behavior"></a>Durum davranışı

[Benzetimi](iot-suite-remote-monitoring-device-schema.md#simulation) aygıt model şeması bölümünü tanımlayan bir sanal cihaz iç durum:

- `InitialState`cihaz durumu nesnenin tüm özelliklerinin ilk değerleri tanımlar.
- `Script`Aygıt durumunu güncelleştirmek için bir zamanlamaya göre çalışan bir JavaScript dosyası tanımlar.

Aşağıdaki örnek, bir sanal Soğutucu cihaz için cihaz durumu nesnesinin tanımı gösterir:

```json
"Simulation": {
  "InitialState": {
    "online": true,
    "temperature": 75.0,
    "temperature_unit": "F",
    "humidity": 70.0,
    "humidity_unit": "%",
    "pressure": 150.0,
    "pressure_unit": "psig",
    "simulation_state": "normal_pressure"
  },
  "Script": {
    "Type": "javascript",
    "Path": "chiller-01-state.js",
    "Interval": "00:00:05"
  }
}
```

Tanımlanan sanal cihaz durumu `InitialState` bölümünde, bellekte benzetimi hizmeti tarafından tutulur. Durum bilgisi girdi olarak geçirilen `main` tanımlanan işlevi **Soğutucu 01 state.js**. Bu örnekte benzetimi hizmetin çalıştığı **Soğutucu 01 state.js** beş saniyede dosya. Komut dosyası, sanal cihaz durumunu değiştirebilirsiniz.

Aşağıdaki tipik bir özetini gösterir `main` işlevi:

```javascript
function main(context, previousState) {

  // Use the previous device state to
  // generate the new device state
  // returned by the function.

  return state;
}
```

`context` Parametresi aşağıdaki özelliklere sahiptir:

- `currentTime`biçiminde bir dize olarak`yyyy-MM-dd'T'HH:mm:sszzz`
- `deviceId`, örneğin`Simulated.Chiller.123`
- `deviceModel`, örneğin`Chiller`

`state` Parametresi aygıt benzetimi hizmeti tarafından tutulan gibi cihaz durumunu içerir. Bu değer `state` önceki çağrısı tarafından döndürülen nesne `main`.

Aşağıdaki örnek, tipik bir uyarlamasını gösterir `main` benzetimi hizmeti tarafından tutulan cihaz durumu işlemek için yöntem:

```javascript
// Default state
var state = {
  online: true,
  temperature: 75.0,
  temperature_unit: "F",
  humidity: 70.0,
  humidity_unit: "%",
  pressure: 150.0,
  pressure_unit: "psig",
  simulation_state: "normal_pressure"
};

function restoreState(previousState) {
  // If the previous state is null, force a default state
  if (previousState !== undefined && previousState !== null) {
      state = previousState;
  } else {
      log("Using default state");
  }
}

function main(context, previousState) {

  restoreState(previousState);

  // Update state

  return state;
}
```

Aşağıdaki örnekte gösterildiği nasıl `main` yöntemi zaman içinde değişir telemetri değerleri benzetimini:

```javascript
/**
 * Simple formula generating a random value
 * around the average and between min and max
 */
function vary(avg, percentage, min, max) {
  var value = avg * (1 + ((percentage / 100) * (2 * Math.random() - 1)));
  value = Math.max(value, min);
  value = Math.min(value, max);
  return value;
}


function main(context, previousState) {

    restoreState(previousState);

    // 75F +/- 5%,  Min 25F, Max 100F
    state.temperature = vary(75, 5, 25, 100);

    // 70% +/- 5%,  Min 2%, Max 99%
    state.humidity = vary(70, 5, 2, 99);

    log("Simulation state: " + state.simulation_state);
    if (state.simulation_state === "high_pressure") {
        // 250 psig +/- 25%,  Min 50 psig, Max 300 psig
        state.pressure = vary(250, 25, 50, 300);
    } else {
        // 150 psig +/- 10%,  Min 50 psig, Max 300 psig
        state.pressure = vary(150, 10, 50, 300);
    }

    return state;
}
```

Tam görüntüleyebilirsiniz [Soğutucu 01 state.js](https://github.com/Azure/device-simulation-dotnet/blob/master/Services/data/devicemodels/scripts/chiller-01-state.js) github'da.

## <a name="method-behavior"></a>Yöntem davranışı

[CloudToDeviceMethods](iot-suite-remote-monitoring-device-schema.md#cloudtodevicemethods) aygıt model şeması bölümünü tanımlayan bir sanal cihaz yanıt verir yöntemleri.

Aşağıdaki örnek, bir sanal Soğutucu aygıt tarafından desteklenen yöntemlerin listesi gösterir:

```json
"CloudToDeviceMethods": {
  "Reboot": {
    "Type": "javascript",
    "Path": "Reboot-method.js"
  },
  "FirmwareUpdate": {
    "Type": "javascript",
    "Path": "FirmwareUpdate-method.js"
  },
  "EmergencyValveRelease": {
    "Type": "javascript",
    "Path": "EmergencyValveRelease-method.js"
  },
  "IncreasePressure": {
    "Type": "javascript",
    "Path": "IncreasePressure-method.js"
  }
}
```

Her yöntemin yöntemi davranışını uygulayan ilişkili bir JavaScript dosyası vardır.

Tanımlanan sanal cihaz durumu `InitialState` bölüm şeması, bellekte benzetimi hizmeti tarafından tutulur. Durum bilgisi girdi olarak geçirilen `main` yöntemi çağrıldığında JavaScript dosyasında tanımlanan işlevi. Komut dosyası, sanal cihaz durumunu değiştirebilirsiniz.

Aşağıdaki tipik bir özetini gösterir `main` işlevi:

```javascript
function main(context, previousState) {

}
```

`context` Parametresi aşağıdaki özelliklere sahiptir:

- `currentTime`biçiminde bir dize olarak`yyyy-MM-dd'T'HH:mm:sszzz`
- `deviceId`, örneğin`Simulated.Chiller.123`
- `deviceModel`, örneğin`Chiller`

`state` Parametresi aygıt benzetimi hizmeti tarafından tutulan gibi cihaz durumunu içerir.

Yöntem davranışını uygulamaya yardımcı olmak için kullanabileceğiniz iki genel işlevler şunlardır:

- `updateState`benzetim hizmeti tarafından tutulan durumunu güncelleştirmek için.
- `sleep`uzun süre çalışan bir görev benzetimini yapmak için yürütme duraklatmak için.

Aşağıdaki örnek, bir kısaltılmış gösterir **IncreasePressure method.js** benzetimli Soğutucu cihazlar tarafından kullanılan komut dosyası:

```javascript
function main(context, previousState) {

    log("Starting 'Increase Pressure' method simulation (5 seconds)");

    // Pause the simulation and change the simulation mode so that the
    // temperature will fluctuate at ~250 when it resumes
    var state = {
        simulation_state: "high_pressure",
        CalculateRandomizedTelemetry: false
    };
    updateState(state);

    // Increase
    state.pressure = 210;
    updateState(state);
    log("Pressure increased to " + state.pressure);
    sleep(1000);

    // Increase
    state.pressure = 250;
    updateState(state);
    log("Pressure increased to " + state.pressure);
    sleep(1000);

    // Resume the simulation
    state.CalculateRandomizedTelemetry = true;
    updateState(state);

    log("'Increase Pressure' method simulation completed");
}
```

## <a name="debugging-script-files"></a>Komut dosyası hata ayıklama

Durum ve yöntemi komut dosyalarını çalıştırmak için aygıt benzetimi hizmeti tarafından kullanılan Javascript yorumlayıcı bir hata ayıklayıcısı eklemeniz mümkün değil. Ancak, bilgi hizmeti günlüğüne oturum açabilir. Yerleşik `log()` işlevi izlemek ve işlev yürütme hata ayıklama bilgileri kaydetmenize olanak sağlar.

Yorumlayıcı başarısız olursa bir sözdizimi hatası ise ve yazan bir `Jint.Runtime.JavaScriptException` hizmet günlüğü girişine.

[Sanal bir cihaz oluşturma](iot-suite-remote-monitoring-test.md) makale aygıtı benzetimi hizmeti yerel olarak çalıştırmak nasıl gösterir. Hizmet yerel olarak çalışan buluta dağıtmadan önce sanal cihazlarınızın hata ayıklamak kolaylaştırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, kendi özel sanal cihaz modeli davranışını tanımlamak açıklanmıştır. Bu makalede gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bir sanal cihaz durumunu denetle
> * Nasıl bir sanal cihaz reponds yöntemine çağrı Uzaktan izleme çözümü tanımlayın
> * Komut dosyalarınızı hata ayıklama

Bir sanal cihaz davranışını belirtmek öğrendiniz artık, önerilen sonraki adıma edinmektir nasıl [sanal bir cihaz oluşturma](iot-suite-remote-monitoring-test.md).

Uzaktan izleme çözümü hakkında daha fazla Geliştirici bilgi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->