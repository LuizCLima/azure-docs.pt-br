---
title: Criar um balanceador de carga interno no Azure Kubernetes Service (AKS)
description: Saiba como criar e usar um balanceador de carga interno para expor seus serviços com o AKS (Serviço de Kubernetes do Azure).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 03/04/2019
ms.author: iainfou
ms.openlocfilehash: a26eab83f567a46f613e3bfda95fd99aba2b79c0
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57404307"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Usar um balanceador de carga interno com o AKS (Serviço de Kubernetes do Azure)

Para restringir o acesso a seus aplicativos no Serviço de Kubernetes do Azure (AKS), você pode criar e usar um balanceador de carga interno. Um balanceador de carga interno torna um serviço do Kubernetes acessível somente a aplicativos em execução na mesma rede virtual que o cluster do Kubernetes. Este artigo mostra como criar e usar um balanceador de carga interno com o AKS (Serviço de Kubernetes do Azure).

> [!NOTE]
> O Balanceador de carga do Azure está disponível em dois SKUs - *Básico* e *Padrão*. O AKS atualmente dá suporte a *básica* SKU. Se quiser usar o SKU *Standard*, você poderá usar [acs-engine][aks-engine] de upstream. Para obter mais informações, consulte [Comparação de SKU do balanceador de carga do Azure][azure-lb-comparison].

## <a name="before-you-begin"></a>Antes de começar

Este artigo considera que já existe um cluster do AKS. Se você precisar de um cluster do AKS, confira o guia de início rápido do AKS [Usando a CLI do Azure][aks-quickstart-cli] ou [Usando o portal do Azure][aks-quickstart-portal].

Você também precisa da CLI do Azure versão 2.0.59 ou posterior instalado e configurado. Execute  `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, confira  [Instalar a CLI do Azure][install-azure-cli].

## <a name="create-an-internal-load-balancer"></a>Criar um balanceador de carga interno

Para criar um balanceador de carga interno, crie um manifesto de serviço denominado `internal-lb.yaml` com o tipo de serviço *LoadBalancer* e a anotação *azure-load-balancer-internal*, conforme mostrado no exemplo a seguir:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

Implantar o balanceador de carga interno usando o [kubectl aplicar] kubectl-aplicar] e especifique o nome do seu manifesto YAML:

```console
kubectl apply -f internal-lb.yaml
```

Um balanceador de carga do Azure é criado no grupo de recursos de nó e conectado à mesma rede virtual que o cluster do AKS.

Quando você visualiza os detalhes do serviço, o endereço IP do balanceador de carga interno é mostrado na coluna *EXTERNAL-IP*. Nesse contexto, *externo* em relação à interface externa do balanceador de carga, não é que ele receba um endereço IP público, externo. Pode levar um minuto ou dois para o endereço IP ser alterado na *\<pendente\>* para um real endereço IP interno, conforme mostrado no exemplo a seguir:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   2m
```

## <a name="specify-an-ip-address"></a>Especificar um endereço IP

Se você quiser usar um endereço IP específico com o balanceador de carga interno, adicione a propriedade *loadBalancerIP* ao manifesto YAML do balanceador de carga. O endereço IP especificado deve residir na mesma sub-rede que o cluster do AKS e não deve ser atribuído a um recurso.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: internal-app
```

Quando implantado e você exibir os detalhes do serviço, o endereço IP na *EXTERNAL-IP* coluna reflete a seu endereço IP especificado:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="use-private-networks"></a>Usar redes privadas

Quando você cria seu cluster do AKS, é possível especificar as configurações de rede avançadas. Essa abordagem permite implantar o cluster na rede virtual e nas sub-redes existentes do Azure. Um cenário é implantar o cluster do AKS em uma rede privada conectada ao seu ambiente local e executar serviços acessíveis apenas internamente. Para obter mais informações, consulte configurar suas próprias sub-redes da rede virtual com [Kubenet] [ use-kubenet] ou [CNI do Azure][advanced-networking].

Não é necessário realizar nenhuma alteração nas etapas anteriores para implantar um balanceador de carga interno em um cluster do AKS que usa uma rede privada. O balanceador de carga é criado no mesmo grupo de recursos que seu cluster do AKS, mas conectado à sua rede virtual e à sub-rede privadas, conforme mostrado no exemplo a seguir:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.1.15.188   10.0.0.35     80:31669/TCP   1m
```

> [!NOTE]
> Talvez seja necessário conceder à entidade de serviço do seu cluster do AKS a função *Colaborador de rede* para o grupo de recursos no qual seus recursos de rede virtual do Azure são implantados. Exiba a entidade de serviço com [az aks show][az-aks-show], como `az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId"`. Para criar uma atribuição de função, use o comando [az role assignment create][az-role-assignment-create].

## <a name="specify-a-different-subnet"></a>Especificar uma sub-rede diferente

Para especificar uma sub-rede para o balanceador de carga, adicione a anotação *azure-load-balancer-internal-subnet* ao seu serviço. A sub-rede especificada deve estar na mesma rede virtual que seu cluster do AKS. Quando implantado, endereço *EXTERNAL-IP* do balanceador de carga faz parte da sub-rede especificada.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "apps-subnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

## <a name="delete-the-load-balancer"></a>Excluir o balanceador de carga

Quando todos os serviços que usam o balanceador de carga interno são excluídos, o próprio balanceador de carga também é excluído.

Você também pode excluir diretamente um serviço como qualquer recurso do Kubernetes, como `kubectl delete service internal-app`, que também exclui o balanceador de carga do Azure subjacente.

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre os serviços do Kubernetes na [documentação dos serviços do Kubernetes][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[use-kubenet]: configure-kubenet.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli