---
title: Como usar o kubectl com espaços de desenvolvimento do Azure
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: conceptual
description: Desenvolvimento rápido de Kubernetes com contêineres e microsserviços no Azure
keywords: 'Docker, Kubernetes, Azure, AKS, serviço Kubernetes do Azure, contêineres, Helm, malha de serviço, o roteamento de malha do serviço, kubectl, k8s '
ms.openlocfilehash: 0dfe88966deeb4dcf0196aa1f1584a06794b36a7
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57773258"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Usar kubectl com um Azure Dev Space

É possível acessar o cluster do Kubernetes em um Azure Dev Space e usar ferramentas do Kubernetes existentes, como `kubectl`.

Executar o comando `az aks use-dev-spaces` adicionará automaticamente um contexto de configuração `kubectl`, portanto, o kubectl já deverá estar conectado ao cluster do Kubernetes do Azure Dev Spaces. Exemplos:
- Confirme o contexto atual: `kubectl config current-context`
- Listar todos os contextos disponíveis: `kubectl config get-contexts`. 
- Alterar contexto: `kubectl config use-context <context-name>`
- Exiba o painel do Kubernetes: execute `kubectl proxy` e, em seguida, abra o navegador no endereço que este comando emitir (acrescente `/ui` à URL para navegar até o painel do Kubernetes).
- Liste os serviços em execução no Azure Dev Spaces padrão do Azure nomeado *padrão*: `kubectl get services --namespace=default`

