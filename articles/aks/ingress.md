---
title: Giriş ile Azure kapsayıcı hizmeti (AKS) kümesi yapılandırın
description: Yükleyin ve bir Azure kapsayıcı hizmeti (AKS) küme NGINX giriş denetleyicisi yapılandırın.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/03/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: d56b27a040420d049f567ac0de9289b1e72f3ea9
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="https-ingress-on-azure-container-service-aks"></a>HTTPS giriş Azure kapsayıcı hizmeti (AKS)

Bir giriş denetleyicisi ters proxy, yapılandırılabilir trafik yönlendirme ve TLS sonlandırma Kubernetes hizmetleri sağlayan yazılım parçasıdır. Kubernetes giriş kaynakları giriş kuralları ve tek tek Kubernetes Hizmetleri için rotalar yapılandırmak için kullanılır. Bir giriş denetleyicisi ve giriş kurallarını kullanarak, tek bir dış adresi Kubernetes kümedeki birden fazla hizmet için trafiği yönlendirmek için kullanılabilir.

Bu belgede bir örnek dağıtımında kılavuzluk etmektedir [NGINX giriş denetleyicisi] [ nginx-ingress] bir Azure kapsayıcı hizmeti (AKS) kümesindeki. Ayrıca, [KUBE LEGO] [ kube-lego] proje otomatik olarak oluşturmak ve yapılandırmak için kullanılan [şimdi şifrelemek] [ lets-encrypt] sertifikalar. Son olarak, bazı uygulamalar, her biri tek bir adresi erişilebilen AKS kümedeki çalıştırılır.

## <a name="prerequisite"></a>Önkoşul

Helm CLI yükleme - Helm CLI bkz [belgelerine] [ helm-cli] yükleme yönergeleri için.

## <a name="install-an-ingress-controller"></a>Bir giriş denetleyicisi yükleme

Helm NGINX giriş denetleyicisi yüklemek için kullanın. Bkz. NGINX giriş controller [belgelerine] [ nginx-ingress] ayrıntılı dağıtım bilgileri için.

Grafik depo güncelleştirin.

```console
helm repo update
```

NGINX giriş denetleyicisi yükleyin. Bu örnek, denetleyicide yükler `kube-system` ad alanı, bu tercih ettiğiniz bir ad alanına değiştirilebilir.

```
helm install stable/nginx-ingress --namespace kube-system
```

Yükleme sırasında Azure ortak IP adresi giriş denetleyici için oluşturulur. Genel IP adresi almak için kubectl get hizmet komutunu kullanın. Hizmete atanan IP adresi için biraz zaman alabilir.

```console
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   13.82.238.45   80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>         80/TCP                       20m
```

Genel IP adresine göz atarsanız hiçbir giriş kuralları, oluşturulduğundan, NGINX giriş denetleyicileri varsayılan 404 sayfasına yönlendirilir.

![Varsayılan NGINX arka uç](media/ingress/default-back-end.png)

## <a name="configure-dns-name"></a>DNS adı yapılandırma

HTTPS sertifika kullanıldığından, giriş denetleyicileri IP adresi için bir FQDN adı yapılandırmanız gerekir. Bu örnekte, bir Azure FQDN ile Azure CLI oluşturulur. Giriş denetleyicisi FQDN ile kullanmak istediğiniz adı ve IP adresiyle komut dosyasını güncelleştirin.

```
#!/bin/bash

# Public IP address
IP="52.224.125.195"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get resource group and public ip name
RESOURCEGROUP=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[resourceGroup]" --output tsv)
PIPNAME=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[name]" --output tsv)

# Update public ip address with dns name
az network public-ip update --resource-group $RESOURCEGROUP --name  $PIPNAME --dns-name $DNSNAME
```

Giriş denetleyicisi artık FQDN üzerinden erişilebilir olması gerekir.

## <a name="install-kube-lego"></a>KUBE LEGO yükleyin

NGINX giriş denetleyicisi TLS sonlandırma destekler. Almak ve HTTPS için sertifikaları yapılandırmak için çeşitli yollar olsa da, bu belgenin kullanımı gösterilir [KUBE LEGO][kube-lego], otomatik sağlayan [sağlar şifrelemek] [ lets-encrypt] sertifika oluşturma ve yönetim işlevselliği.

KUBE LEGO denetleyicisi yüklemek için aşağıdaki Helm yükleme komutunu kullanın. E-posta adresi kuruluşunuzun birinden ile güncelleştirin.

```
helm install stable/kube-lego \
  --set config.LEGO_EMAIL=user@contoso.com \
  --set config.LEGO_URL=https://acme-v01.api.letsencrypt.org/directory
```

KUBE LEGO yapılandırma hakkında daha fazla bilgi için bkz: [KUBE LEGO belgelerine][kube-lego].

## <a name="run-application"></a>Uygulamayı çalıştırma

Bu noktada, bir giriş denetleyicisi ve bir sertifika yönetimi çözümü yapılandırıldı. Şimdi birkaç uygulamaları AKS kümenizdeki çalıştırın.

Bu örnekte, Helm basit hello world uygulamasının birden çok örneği çalıştırmak için kullanılır.

Uygulamayı çalıştırmadan önce geliştirme sisteminizde Azure örneklerinden Helm deposunu ekleyin.

```
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

 AKS hello world grafik birlikte aşağıdaki komutu çalıştırın:

```
helm install azure-samples/aks-helloworld
```

Şimdi hello world uygulamasının ikinci bir örneğini yükleyin.

İki uygulama görsel olarak ayrı; böylece ikinci örneği için yeni bir başlık belirtin. Ayrıca, benzersiz bir hizmet adı belirtmeniz gerekir. Bu yapılandırmalar aşağıdaki komutta görülebilir.

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-ingress-route"></a>Giriş yolu oluştur

Her iki uygulamayı Kubernetes kümenizde çalışan ancak yapılandırıldı artık türünde bir hizmet olan `ClusterIP`. Bu nedenle, uygulamaları Internet'ten erişilebilir değildir. Kullanılabilir hale getirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı iki uygulamalardan biri trafiği yönlendirmek kuralları yapılandırır.

Bir dosya adı oluşturun `hello-world-ingress.yaml` ve aşağıdaki YAML kopyalayın.

Not alın adresi trafiği `https://demo-aks-ingress.eastus.cloudapp.azure.com/` adlı hizmete yönlendirilir `aks-helloworld`. Trafik adresine `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` yönlendirilir `ingress-demo` hizmet.

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/tls-acme: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Giriş kaynakla oluşturmak `kubectl apply` komutu.

```console
kubectl apply -f hello-world-ingress.yaml
```

## <a name="test-the-ingress-configuration"></a>Giriş yapılandırmayı test etme

Kubernetes giriş denetleyicinizi FQDN'sini göz atın, hello world uygulamasının görmeniz gerekir.

![Bir uygulama örneği](media/ingress/app-one.png)

Şimdi giriş denetleyicisiyle FQDN'sini Gözat `/hello-world-two` yolu, özel başlık hello world uygulamayla görmeniz gerekir.

![İki uygulama örneği](media/ingress/app-two.png)

Ayrıca bağlantı şifrelenir ve şimdi şifrelemek tarafından verilen bir sertifika kullanılır dikkat edin.

![Sertifika şifreleme sağlar](media/ingress/certificate.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede gösterilen yazılım hakkında daha fazla bilgi edinin.

- [Helm CLI][helm-cli]
- [NGINX giriş denetleyicisi][nginx-ingress]
- [KUBE-LEGO][kube-lego]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/en-us/azure/aks/kubernetes-helm#install-helm-cli
[kube-lego]: https://github.com/jetstack/kube-lego
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
