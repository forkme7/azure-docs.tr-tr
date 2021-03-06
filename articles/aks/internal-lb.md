---
title: Bir Azure kapsayıcı hizmeti (AKS) iç yük dengeleyici oluşturma
description: Bir iç yük dengeleyici Azure kapsayıcı hizmeti (AKS) kullanın.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 3/29/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7b9ecdb5364f7c0f5bb68ce693e53bc2c5327337
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="use-an-internal-load-balancer-with-azure-container-service-aks"></a>Azure kapsayıcı hizmeti (AKS) bir iç yük dengeleyici kullanın

İç Yük Dengeleme Kubernetes hizmet Kubernetes kümesi olarak aynı sanal ağda çalışan uygulamalar için erişilebilir hale getirir. Bu belge, Azure kapsayıcı hizmeti (AKS) bir iç yük dengeleyici oluşturma ayrıntıları.

## <a name="create-internal-load-balancer"></a>İç yük dengeleyici oluşturma

Bir iç yük dengeleyici oluşturmak için hizmet türü ile bir hizmet bildirimi yapı `LoadBalancer` ve `azure-load-balancer-internal` aşağıdaki örnekte görüldüğü gibi ek açıklama.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Uygulama dağıtıldıktan sonra bir Azure yük dengeleyici oluşturulur ve AKS kümesi olarak aynı sanal ağda kullanılabilir. 

![AKS iç yük dengeleyici görüntüsü](media/internal-lb/internal-lb.png)

Ne zaman hizmeti alma ayrıntıları, IP adresi `EXTERNAL-IP` sütun iç yük dengeleyicisi IP adresidir. 

```console
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   10s
```

## <a name="specify-an-ip-address"></a>Bir IP adresi belirtin

İç yük dengeleyici ile belirli bir IP adresi kullanmak istiyorsanız, ekleme `loadBalancerIP` yük dengeleyici spec özelliğine. Belirtilen IP adresi AKS küme aynı alt ağda bulunmaları gerekir ve zaten kaynak atanmamaış olmalıdır.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Hizmet Ayrıntıları alırken, IP adresi `EXTERNAL-IP` belirtilen IP adresi yansıtmalıdır. 

```console
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="delete-the-load-balancer"></a>Yük Dengeleyici Sil

İç yük dengeleyicisi kullanarak tüm hizmetlerde sildikten sonra Yük Dengeleyici de silinir.

## <a name="next-steps"></a>Sonraki adımlar

Adresinde Kubernetes hizmetleri hakkında daha fazla bilgi [Kubernetes Hizmetleri belgeleri][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/