---
title: Criar um Gateway de aplicativo do Azure - CLI clássica do Azure
description: Saiba como criar um Gateway de Aplicativo usando a CLI clássica do Azure no Resource Manager
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 4/15/2019
ms.author: victorh
ms.openlocfilehash: 7107f45253c4f13b3378489726bf5034e104fa30
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59608448"
---
# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Criar um gateway de aplicativo usando a CLI do Azure

O Gateway de Aplicativo do Azure é um balanceador de carga de camada 7. Ele fornece o failover e solicitações HTTP de roteamento de desempenho entre diferentes servidores, estejam eles na nuvem ou no local. Gateway de aplicativo tem os seguintes recursos de entrega de aplicativo: Balanceamento de carga HTTP, afinidade de sessão baseada em cookie e descarregamento de Secure Sockets Layer (SSL), investigações de integridade personalizadas e suporte para vários sites.

## <a name="prerequisite-install-the-azure-cli"></a>Pré-requisito: Instalar a CLI do Azure

Para executar as etapas neste artigo, você precisará [instalar a CLI do Azure](../xplat-cli-install.md) e você precisará [entrar no Azure](/cli/azure/authenticate-azure-cli). 

> [!NOTE]
> Se você não tiver uma conta do Azure, crie uma. Inscreva-se em uma [avaliação gratuita aqui](../active-directory/fundamentals/sign-up-organization.md).

## <a name="scenario"></a>Cenário

Nesse cenário, você aprenderá como criar um Gateway de Aplicativo usando o portal do Azure.

Este cenário:

* Criará um Gateway de Aplicativo com duas instâncias.
* Criar uma rede virtual chamada ContosoVNET com um bloco CIDR 10.0.0.0/16 reservado.
* Crie uma sub-rede chamada subnet01 que use 10.0.0.0/28 como bloco CIDR.

> [!NOTE]
> A configuração adicional do Gateway de Aplicativo, incluindo investigações de integridade personalizadas, endereços de pool de back-end e regras adicionais são configuradas após o Gateway de Aplicativo ser configurado e não durante a implantação inicial.

## <a name="before-you-begin"></a>Antes de começar

O Gateway de Aplicativo do Azure requer sua própria sub-rede. Ao criar uma rede virtual, certifique-se de deixar espaço de endereço suficiente para ter várias sub-redes. Depois de implantar um gateway de aplicativo a uma sub-rede, apenas gateway de aplicativos adicionais poderão ser adicionados à sub-rede.

## <a name="sign-in-to-azure"></a>Entrar no Azure

Abra o **Prompt de comando do Microsoft Azure**e entre.

```azurecli-interactive
az login
```

Depois de digitar o exemplo anterior, um código será fornecido. Navegue até https://aka.ms/devicelogin em um navegador para continuar a entrada no processo.

![cmd mostrando logon de dispositivo][1]

No navegador, digite o código recebido. Você será redirecionado para uma página de entrada.

![navegador para inserir código][2]

Depois que o código foi inserido, você estará conectado. Feche o navegador para continuar com o cenário.

![conectado com êxito][3]

## <a name="switch-to-resource-manager-mode"></a>Alternar para o modo do Resource Manager

```azurecli-interactive
azure config mode arm
```

## <a name="create-the-resource-group"></a>Criar o grupo de recursos

Antes de criar o gateway de aplicativo, um grupo de recursos é criado para conter o gateway de aplicativo. O código a seguir mostra o comando.

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Após a criação do grupo de recursos, uma rede virtual é criada para o gateway de aplicativo.  No exemplo a seguir, o espaço de endereço foi 10.0.0.0/16, conforme definido nas anotações do cenário anterior.

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a>Criar uma sub-rede

Após a criação da rede virtual, uma sub-rede é adicionada ao gateway de aplicativo.  Se você planeja usar o gateway de aplicativo com um aplicativo Web hospedado na mesma rede virtual do que o gateway de aplicativo, não se esqueça de deixar espaço suficiente para outra sub-rede.

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-the-application-gateway"></a>Criar o gateway de aplicativo

Depois que a rede virtual e a sub-rede forem criadas, os pré-requisitos para o gateway de aplicativo estarão completos. Além de um certificado. pfx exportado anteriormente e a senha do certificado são necessários para a etapa a seguir: Os endereços IP usados para o back-end são os endereços IP para o servidor de back-end. Esses valores podem ser IPs privados na rede virtual, ips públicos ou nomes de domínio totalmente qualificados para seus servidores de back-end.

```azurecli-interactive
azure network application-gateway create \
--name AdatumAppGateway \
--location eastus \
--resource-group ContosoRG \
--vnet-name ContosoVNET \
--subnet-name subnet01 \
--servers 134.170.185.46,134.170.188.221,134.170.185.50 \
--capacity 2 \
--sku-tier Standard \
--routing-rule-type Basic \
--frontend-port 80 \
--http-settings-cookie-based-affinity Enabled \
--http-settings-port 80 \
--http-settings-protocol http \
--frontend-port http \
--sku-name Standard_Medium
```

> [!NOTE]
> Para obter uma lista de parâmetros que podem ser fornecidos durante a criação, execute o seguinte comando: **azure network application-gateway create --help**.

Este exemplo cria um Gateway de Aplicativo básico com configurações padrão para o ouvinte, pool de back-end, configurações de http de back-end e regras. Você pode modificar essas configurações de acordo com sua implantação quando o provisionamento for bem-sucedido.
Se você já tiver seu aplicativo Web definido com o pool de back-end nas etapas anteriores, o balanceamento de carga começará depois que ele for criado.

## <a name="next-steps"></a>Próximos passos

Saiba como criar investigações de integridade personalizados visitando [Criar uma investigação de integridade personalizada](application-gateway-create-probe-portal.md)

Saiba como configurar o Descarregamento de SSL e levar a descriptografia SSL cara longe dos seus servidores Web visitando [Configurar Descarregamento de SSL](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png
