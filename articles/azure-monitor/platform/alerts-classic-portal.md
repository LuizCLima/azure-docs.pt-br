---
title: Criar, exibir e gerenciar alertas de métrica clássicos usando o Azure Monitor
description: Saiba como usar o portal do Azure, a CLI ou o Powershell para criar, exibir e gerenciar regras de alerta de métrica clássicos.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: snmuvva
ms.openlocfilehash: 4a225dbc8e84d65a6ea25f63627599e5bb7d2ced
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905437"
---
# <a name="create-view-and-manage-classic-metric-alerts-using-azure-monitor"></a>Criar, exibir e gerenciar alertas de métrica clássicos usando o Azure Monitor

Os alertas de métrica clássicos no Azure Monitor fornecem uma maneira para notificação quando uma das suas métricas ultrapassarem um limite. Os alertas de métrica clássicos são uma funcionalidade mais antiga que permite alertas somente em métricas não dimensionais. Há uma funcionalidade mais recente, chamada alertas de métrica, que tem funcionalidade aprimorada sobre alertas de métrica clássicos. Você pode aprender mais sobre a nova funcionalidade de alertas de métrica na [visão geral de alertas de métrica](../../azure-monitor/platform/alerts-metric-overview.md). Neste artigo, descreveremos como criar, exibir e gerenciar regras de alertas de métrica clássicos por meio do portal do Azure, da CLI do Azure e do Powershell.

## <a name="with-azure-portal"></a>Com o portal do Azure

1. No [portal](https://portal.azure.com/), localize o recurso que você quer monitorar e, em seguida, selecione-o.

2. Na seção**MONITORAMENTO**, selecione **Alertas (Clássico)**. O texto e o ícone podem variar ligeiramente para diferentes recursos. Se você não localizar **Alertas (Clássico)** aqui, poderá encontrá-lo em **Alertas** ou **Regras de Alerta**.

    ![Monitoramento](media/alerts-classic-portal/AlertRulesButton.png)

3. Selecione o comando **Adicionar alerta de métrica (clássico)** e preencha os campos.

    ![Adicionar alerta](media/alerts-classic-portal/AddAlertOnlyParamsPage.png)

4. **Nomeie** a regra de alerta. Em seguida, escolha uma **Descrição**, que também aparece nos emails de notificação.

5. Selecione a **Métrica** que você quer monitorar. Em seguida, escolha um valor de **Condição** e **Limite** para a métrica. Escolha também o **Período** durante o qual a regra de métrica deverá ser atendida antes de o alerta disparar. Por exemplo, se você usar o período "Pelos últimos 5 minutos" e o alerta procura por CPU acima de 80%, o alerta disparará quando a CPU estiver consistentemente acima de 80% por 5 minutos. Depois que o primeiro gatilho ocorrer, ele será disparado novamente quando a CPU ficar abaixo de 80% por 5 minutos. A medição da métrica da CPU acontece a cada minuto.

6. Selecione **Proprietários de email...** se você quiser que administradores e coadministradores recebam notificações por email quando o alerta for acionado.

7. Se você quiser enviar notificações para endereços de email adicionais quando o alerta for acionado, adicione-os no campo **Emails adicionais do administrador**. Vários emails separados com ponto e vírgula, no seguinte formato: *e-mail\@contoso.com;email2\@contoso.com*

8. Coloque um URI válido no campo **Webhook** se você quiser que ele seja chamado quando o alerta for acionado.

9. Se você usar a Automação do Azure, selecione um runbook a ser executado quando o alerta for acionado.

10. Selecione **OK** para criar o alerta.

Em alguns minutos, o alerta estará ativo e disparará conforme descrito anteriormente.

Após criar um alerta, você poderá selecioná-lo e executar uma das seguintes tarefas:

* Exibir um gráfico mostrando o limite da métrica e os valores reais do dia anterior.
* Editar ou exclui-lo.
* **Desabilitar** ou **Habilitar** se você quiser interromper temporariamente ou continuar recebendo notificações do alerta.

## <a name="with-azure-cli"></a>Com a CLI do Azure

As seções anteriores descreveram como criar, exibir e gerenciar regras de alerta de métrica usando o portal do Azure. Esta seção descreve como fazer o mesmo usando a [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) de multiplataforma. O modo mais rápido de começar a usar a CLI do Azure é com o [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest).

### <a name="get-all-classic-metric-alert-rules-in-a-resource-group"></a>Obtenha regras de alertas de métrica clássicos em um grupo de recursos

```azurecli
az monitor alert list --resource-group <group name>
```

### <a name="see-details-of-a-particular-classic-metric-alert-rule"></a>Consulte os detalhes de uma determinada regra de alerta de métrica clássico

```azurecli
az monitor alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-classic-metric-alert-rule"></a>Crie uma regra de alerta de métrica clássico

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-a-classic-metric-alert-rule"></a>Exclua uma regra de alerta de métrica clássico

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="with-powershell"></a>Com o PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Esta seção mostra como usar os comandos do PowerShell para criar, exibir e gerenciar alertas de métrica clássicos. Os exemplos neste artigo ilustram como você pode usar os cmdlets do Azure Monitor para alertas de métrica clássicos.

1. Se ainda não tiver feito isso, configure o PowerShell para ser executado no seu computador. Para saber mais, consulte [Como instalar e configurar o PowerShell](/powershell/azure/overview). Você também pode ver a lista completa de cmdlets do PowerShell do Azure Monitor em [Cmdlets do Azure Monitor (Insights)](https://docs.microsoft.com/powershell/module/az.applicationinsights).

2. Primeiro, entre em sua assinatura do Azure.

    ```powershell
    Connect-AzAccount
    ```

3. Você verá uma tela de entrada. Quando entrar, sua Conta, sua TenantID e a ID da Assinatura padrão serão exibidas. Todos os cmdlets do Azure funcionam no contexto de sua assinatura padrão. Para exibir a lista de assinaturas a que você tem acesso, use o seguinte comando:

    ```powershell
    Get-AzSubscription
    ```

4. Para alterar o contexto de trabalho para uma assinatura diferente, use o comando a seguir:

    ```powershell
    Set-AzContext -SubscriptionId <subscriptionid>
    ```

5. É possível obter todas as regras de alertas de métrica clássicos em um grupo de recursos:

    ```powershell
    Get-AzAlertRule -ResourceGroup montest
    ```

6. É possível exibir detalhes de uma regra de alerta de métrica clássico

    ```powershell
    Get-AzAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
    ```

7. É possível recuperar todo o conjunto de regras de alerta para um recurso de destino. Por exemplo, todas as regras de alerta definidas em uma VM.

    ```powershell
    Get-AzAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
    ```

8. Regras de alerta clássico não podem ser criadas por meio do PowerShell. Para criar uma regra de alerta que você precisa usar o novo ['Add-AzMetricAlertRule'](/powershell/module/az.monitor/add-azmetricalertrule) comando.

## <a name="next-steps"></a>Próximas etapas

- [Criar um alerta de métrica clássico com um modelo do Resource Manager](../../azure-monitor/platform/alerts-enable-template.md).
- [Fazer um alerta de métrica clássico notificar um sistema diferente do Azure usando um webhook](../../azure-monitor/platform/alerts-webhooks.md).
