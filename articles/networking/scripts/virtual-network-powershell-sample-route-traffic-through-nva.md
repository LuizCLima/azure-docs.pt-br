---
title: Exemplo de script do Azure PowerShell – Rotear o tráfego por meio de uma solução de virtualização de rede | Microsoft Docs
description: Exemplo de script do Azure PowerShell – Rotear o tráfego por meio de uma solução de virtualização de rede de firewall.solução de virtualização.
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: b596431a8d492316c7bacf65c9c0d5805030e621
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56650846"
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Rotear o tráfego por meio de uma solução de virtualização de rede

Este exemplo de script cria uma rede virtual com sub-redes de front-end e back-end. Ele também cria uma VM com o encaminhamento de IP habilitado para rotear o tráfego entre as duas sub-redes. Depois de executar o script, você pode implantar o software de rede, como um aplicativo de firewall, à VM.

Se necessário, instale o Azure PowerShell usando a instrução encontrada no [guia do Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) e, em seguida, execute `Connect-AzAccount` para criar uma conexão com o Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Limpar a implantação 

Execute o comando a seguir para remover o grupo de recursos, a VM e todos os recursos relacionados.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a>Explicação sobre o script

Este script usa os comandos a seguir para criar um grupo de recursos, uma rede virtual e grupos de segurança de rede. Cada comando na tabela redireciona para a documentação específica do comando.

| Comando | Observações |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)  | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Cria uma rede virtual do Azure e uma sub-rede front-end. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Cria as sub-redes back-end e DMZ. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Cria um endereço IP público para acessar a VM da Internet. |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Cria uma interface de rede virtual e habilita o encaminhamento IP nela. |
| [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Cria um grupo de segurança de rede (NSG). |
| [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) | Cria regras de NSG que permitem as portas HTTP e HTTPS de entrada para a VM. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)| Associa os NSGs e tabelas de rotas às sub-redes. |
| [New-AzRouteTable](/powershell/module/az.network/new-azroutetable)| Cria uma tabela de rotas com todas as rotas. |
| [New-AzRouteConfig](/powershell/module/az.network/new-azrouteconfig)| Cria as rotas para rotear o tráfego entre sub-redes e a Internet por meio da VM. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Cria uma máquina virtual e anexa o NIC a ela. Este comando também especifica a imagem de máquina virtual a ser usada e as credenciais administrativas. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup)  | Exclui um grupo de recursos e todos os seus recursos contidos nele. |

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre o Azure PowerShell, confira a [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Exemplos adicionais de script de PowerShell de rede podem ser encontrados na [Documentação de visão geral da rede do Azure](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
