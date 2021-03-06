---
title: "Azure uygulama hizmeti Linux üzerinde SSH desteği | Microsoft Docs"
description: "Azure uygulama hizmeti Linux'ta ile SSH kullanma hakkında bilgi edinin."
keywords: "Azure uygulama hizmeti, web uygulaması, linux, oss"
services: app-service
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: wesmc
ms.openlocfilehash: 905c257ab40057f05081e54e8680bd818023d886
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="ssh-support-for-azure-app-service-on-linux"></a>Azure uygulama hizmeti Linux üzerinde SSH desteği

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) Ağ Hizmetleri güvenli bir şekilde kullanmak için bir şifreleme ağ protokolüdür. Ayrıca, bir sisteme uzaktan güvenli bir şekilde bir komut satırı ile oturum açma ve yönetimsel komutları uzaktan yürütme için en yaygın olarak kullanılır.

Linux üzerinde App Service, her yeni web uygulamaları için çalışma zamanı yığınını kullanılan yerleşik Docker görüntülerinin uygulama kapsayıcısı içine SSH desteği sağlar.

![Çalışma zamanı yığınları](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

Bu makalede açıklanan yapılandırma ve görüntünün bir parçası olarak SSH sunucusu dahil olmak üzere özel Docker görüntülerinizi ile SSH kullanabilirsiniz.

## <a name="making-a-client-connection"></a>Bir istemci bağlantısını yapma

Bir SSH istemci bağlantısı yapmak için ana site başlatılması gerekir.

Web uygulamanız için kaynak denetimi Yönetim (SCM) uç noktası aşağıdaki biçimi kullanarak tarayıcınıza yapıştırın:

```bash
https://<your sitename>.scm.azurewebsites.net/webssh/host
```

Önceden doğrulanmış değil, Azure aboneliğinizle bağlanmak için kimlik doğrulaması için gereklidir.

![SSH bağlantısı](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)

## <a name="ssh-support-with-custom-docker-images"></a>Özel Docker görüntülerle SSH desteği

Azure portalında kapsayıcı ve istemci arasındaki SSH iletişimi desteklemek özel bir Docker görüntü için sırayla Docker görüntüsü için aşağıdaki adımları gerçekleştirin.

Azure uygulama hizmeti deposu olarak bu adımları gösterilen [örnek](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).

1. Dahil `openssh-server` yüklemesinde [ `RUN` yönerge](https://docs.docker.com/engine/reference/builder/#run) Dockerfile görüntü ve kök parolasını hesap için kümesi içinde `"Docker!"`.

    > [!NOTE]
    > Bu yapılandırma kapsayıcıya dış bağlantılar kurulmasına izin vermez. SSH yalnızca Kudu erişilebilir / yayımlama kimlik bilgileri kullanılarak kimlik doğrulaması SCM Site.

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \
        && apt-get install -y --no-install-recommends openssh-server \
        && echo "root:Docker!" | chpasswd
    ```

1. Ekleme bir [ `COPY` yönerge](https://docs.docker.com/engine/reference/builder/#copy) Dockerfile kopyalamak için bir [sshd_config](http://man.openbsd.org/sshd_config) dosya */vb./ssh/* dizin. Yapılandırma dosyanızı Azure App Service GitHub deposuna sshd_config dosyasında dayanmalıdır [burada](https://github.com/Azure-App-Service/node/blob/master/8.2.1/sshd_config).

    > [!NOTE]
    > *Sshd_config* dosyası şunları içermelidir veya bağlantı başarısız olur: 
    > * `Ciphers` aşağıdakilerden en az birini içermelidir: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs` aşağıdakilerden en az birini içermelidir: `hmac-sha1,hmac-sha1-96`.

    ```docker
    COPY sshd_config /etc/ssh/
    ```

1. Bağlantı noktası 2222 dahil [ `EXPOSE` yönerge](https://docs.docker.com/engine/reference/builder/#expose) Dockerfile için. Kök parolası biliniyor olsa da, 2222 numaralı bağlantı noktasına İnternet üzerinden erişilemez. Bir iç yalnızca bağlantı noktası erişilebilir yalnızca özel bir sanal ağ köprüsü ağının kapsayıcılara tarafından değil.

    ```docker
    EXPOSE 2222 80
    ```

1. Bir kabuk betiği'ni kullanarak SSH hizmetini başlatmak emin olun (örneğe bakın [init_container.sh](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh)).

    ```bash
    #!/bin/bash
    service ssh start
    ```

Dockerfile kullanan [ `ENTRYPOINT` yönerge](https://docs.docker.com/engine/reference/builder/#entrypoint) komut dosyasını çalıştırmak için.

    ```docker
    COPY startup /opt/startup
    ...
    RUN chmod 755 /opt/startup/init_container.sh
    ...
    ENTRYPOINT ["/opt/startup/init_container.sh"]
    ```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında sorular ve sorunları nakledebilirsiniz [Azure Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

Kapsayıcıları için Web uygulaması hakkında daha fazla bilgi için bkz:

* [Kapsayıcılar için Web App’e yönelik özel Docker görüntüsü kullanma](quickstart-docker-go.md)
* [Linux üzerinde Azure App Service’te .NET Core Kullanma](quickstart-dotnetcore.md)
* [Linux üzerinde Azure App Service’te Ruby Kullanma](quickstart-ruby.md)
* [Kapsayıcılar için Azure App Service Web App SSS](app-service-linux-faq.md)