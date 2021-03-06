---
title: SQL Server nas notas de versão VM do Azure | Microsoft Docs
description: Aprenda sobre os novos recursos e aprimoramentos do SQL Server em uma VM do Azure
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
manager: craigg
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 2/13/2019
ms.openlocfilehash: 23e072369aa8ac6ca6ada5ec185df1a8d7e03c5b
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59282535"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>Notas de versão do SQL Server no Virtual Machine do Azure

O Azure permite implantar uma máquina virtual com uma imagem do SQL Server incorporada. Este artigo resume os novos recursos e melhorias nas versões recentes do [SQL Server em máquinas virtuais do Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). O artigo também lista importantes atualizações de conteúdo que não estão diretamente vinculadas à versão, mas foram publicadas no mesmo período. Para encontrar melhorias em outros serviços do Azure, confira [Atualizações de serviço](https://azure.microsoft.com/updates)

## <a name="march-2019"></a>Março de 2019

### <a name="service-improvements"></a>Melhorias nos serviços

| Melhorias nos serviços | Detalhes |
| --- | --- |
| **Capacidade de suporte de imagem personalizada** | Agora você pode instalar o [extensão SQL IaaS](virtual-machines-windows-sql-server-agent-extension.md#installation) às imagens do sistema operacional e o SQL personalizadas, que oferece a funcionalidade limitada do [licenciamento flexível](virtual-machines-windows-sql-ahb.md). Ao registrar sua imagem personalizada com o provedor de recursos do SQL, especifique o tipo de licença como 'AHUB' como caso contrário, o registro falhará.  | 
| **Capacidade de suporte da instância nomeada** | Você pode utilizar o [extensão SQL IaaS](virtual-machines-windows-sql-server-agent-extension.md#installation) com uma instância nomeada, se a instância padrão tiver sido desinstalada corretamente. | 
| **Aprimoramento do Portal** | A experiência do portal do Azure para implantar uma VM do SQL Server foi remodelada para melhorar a usabilidade. Para obter mais informações, consulte o resumo [Quickstart](quickstart-sql-vm-create-portal.md) e mais completa [instruções](virtual-machines-windows-portal-sql-server-provision.md) guia para implantar uma VM do SQL Server. |
| &nbsp; | &nbsp; |

### <a name="documentation-improvements"></a>Melhorias na documentação

Nenhum


## <a name="february-2019"></a>fevereiro de 2019

### <a name="service-improvements"></a>Melhorias nos serviços

| Melhorias nos serviços | Detalhes |
| --- | --- |
| **Melhoria do Portal** | Agora é possível alterar o modelo de licenciamento de uma VM do SQL Server de pré-pago para trazer-your-própria licença usando o [portal do Azure](virtual-machines-windows-sql-ahb.md#with-the-azure-portal-1).|
|**Simplificação de implantação do grupo de disponibilidade com a CLI de VM do Azure SQL** | Agora é mais fácil do que nunca para implantar um grupo de disponibilidade em uma VM do SQL Server no Azure. [CLI de VM do Azure SQL](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) permite que você crie o ouvinte do WSFC, o ILB e o grupo de disponibilidade, tudo a partir da linha de comando e, em tempo recorde! Para obter mais informações, consulte [usar CLI de VM de SQL do Azure para configurar o grupo de disponibilidade Always On do SQL Server em uma VM do Azure](virtual-machines-windows-sql-availability-group-cli.md). | 
| &nbsp; | &nbsp; |


## <a name="december-2018"></a>Dezembro de 2018

| Melhorias nos serviços | Detalhes |
| --- | --- |
| **Novo provedor de recursos de grupo para cluster SQL** | Um novo provedor de recursos (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups) que define os metadados do Cluster de Failover do Windows. Ingressar em uma VM do SQL Server para o *SqlVirtualMachineGroups* inicializa o serviço de Cluster de Failover do Windows e ingressa a VM no cluster.  |
|**Automatizar a configuração de uma implantação de grupo de disponibilidade com modelos de início rápido do Azure** |Agora é possível criar o Cluster de Failover do Windows, ingressar VMs do SQL Server a ele, criar o ouvinte e configurar o Internal Load Balancer com dois modelos de Início Rápido do Azure. Para obter mais informações, consulte [usar modelo de início rápido do Azure para configurar o grupo de disponibilidade Always On do SQL Server em uma VM do Azure](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Registro do provedor de recursos automática de VM de SQL** | VMs do SQL Server implantadas depois deste mês são automaticamente registradas com o novo provedor de recursos do SQL Server. As VMs do SQL Server implantadas antes deste mês ainda precisarão ser registradas manualmente. Para obter mais informações, consulte [registrar VM existente do SQL com o provedor de recursos de VM do SQL](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>Novembro de 2018

| Melhorias nos serviços | Detalhes |
| --- | --- |
| **Novo provedor de recursos de VM do SQL** |  Um novo provedor de recursos para VMs do SQL Server (Microsoft.SqlVirtualMachine) que fornece um melhor gerenciamento da VM do SQL Server. Para obter mais informações sobre como registrar sua VM, consulte [Registrar a VM do SQL existente com o novo provedor de recursos](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider). |
|**Alternar o modelo de licenciamento** |Agora você pode alternar entre o modelo de pagamento por uso e o de trazer sua própria licença para sua VM do SQL usando a CLI do Azure ou o PowerShell do Azure. Para saber mais, confira [Como alterar o modelo de licenciamento de uma VM do SQL](virtual-machines-windows-sql-ahb.md). | 
| &nbsp; | &nbsp; |


## <a name="additional-resources"></a>Recursos adicionais

**VMs do Windows**:

* [Visão geral do SQL Server em uma VM do Windows](virtual-machines-windows-sql-server-iaas-overview.md).
* [Provisionar um servidor SQL Windows VM](virtual-machines-windows-portal-sql-server-provision.md)
* [Migrar um banco de dados para SQL Server em uma VM do Azure](virtual-machines-windows-migrate-sql.md)
* [Alta disponibilidade e recuperação de desastres para SQL Server em máquinas virtuais do Azure](virtual-machines-windows-sql-high-availability-dr.md)
* [Práticas recomendadas relacionadas ao desempenho para o SQL Server em máquinas virtuais do Azure](virtual-machines-windows-sql-performance.md)
* [Estratégias de Desenvolvimento e Padrões de Aplicativo para o SQL Server em Máquinas Virtuais do Azure](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**VMs Linux**:

* [Visão geral do SQL Server em uma VM do Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Provisionar uma máquina Virtual do SQL Server Linux](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [FAQ (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [SQL Server na documentação do Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
