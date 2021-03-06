---
title: Recursos com suporte para alertas de métrica no Azure Monitor
description: Referência sobre métricas e logs de suporte para alertas de métrica no Azure Monitor
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 0dd8d7c1e004472d230337b72d55ac7ced905b41
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59490921"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Recursos com suporte para alertas de métrica no Azure Monitor

Agora, o Azure Monitor dá suporte a um [novo tipo de alerta de métrica](../../azure-monitor/platform/alerts-overview.md), que tem vantagens significativas sobre os [alertas de métrica clássicos](../../azure-monitor/platform/alerts-classic.overview.md) mais antigos. As métricas estão disponíveis para uma [ampla lista de serviços do Azure](../../azure-monitor/platform/metrics-supported.md). Os alertas mais recentes dão suporte a um subconjunto (cada vez maior) de tipos de recurso. Este artigo lista esse subconjunto.

Você também pode usar os alertas de métrica mais recentes sobre dados de log populares, armazenados em um espaço de trabalho do Log Analytics extraído como métricas. Para saber mais, exiba [Alertas de Métrica para Logs](../../azure-monitor/platform/alerts-metric-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLI, suporte REST
Atualmente, é possível criar alertas de métrica mais novos apenas no portal do Azure, [API REST](https://docs.microsoft.com/rest/api/monitor/metricalerts/) ou [Modelos do Gerenciador de Recursos](../../azure-monitor/platform/alerts-metric-create-templates.md). Suporte para configurar os alertas mais recentes usando o PowerShell e a CLI do Azure versões 2.0 e superiores em breve.

## <a name="metrics-and-dimensions-supported"></a>Medidas e dimensões com suporte
Os alertas de métrica mais recentes fornecem suporte a alertas para métricas que usam dimensões. É possível usar dimensões para filtrar sua métrica para o nível certo. Todas as métricas compatíveis, juntamente com as dimensões aplicáveis, podem ser exploradas e visualizadas no [Azure Monitor – Metrics Explorer](../../azure-monitor/platform/metrics-charts.md).

Esta é a lista completa das origens métricas do Azure Monitor com suporte pelos alertas mais recentes:

|Tipo de recurso  |Dimensões compatíveis  | Métricas disponíveis|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Sim        | [Gerenciamento da API](../../azure-monitor/platform/metrics-supported.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Sim   | [Contas de Automação](../../azure-monitor/platform/metrics-supported.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/D| [Contas do Lote](../../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/D     |[Cache Redis do Azure](../../azure-monitor/platform/metrics-supported.md#microsoftcacheredis)|
|Microsoft.CognitiveServices/accounts     |    N/D     | [Serviços Cognitivos](../../azure-monitor/platform/metrics-supported.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines     |    N/D     | [Máquinas virtuais](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/D      |[Conjuntos de dimensionamento de máquinas virtuais](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Sim| [Grupos de contêineres](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.ContainerService/managedClusters | Sim | [Clusters gerenciados](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerservicemanagedclusters)|
|Microsoft.DataFactory/datafactories| Sim| [Data Factories V1](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Sim     |[Data Factories V2](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/D      |[Banco de dados para MySQL](../../azure-monitor/platform/metrics-supported.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/D     | [Banco de dados para PostgreSQL](../../azure-monitor/platform/metrics-supported.md#microsoftdbforpostgresqlservers)|
|Microsoft.Devices/IotHubs    | N/D     |[Métricas do Hub IoT](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesiothubs)
|Microsoft.Devices/provisioningServices    | Sim     |[Métricas DPS](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesprovisioningservices)
|Microsoft.EventHub/namespaces     |  Sim      |[Hubs de Evento](../../azure-monitor/platform/metrics-supported.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Não  | [Vaults](../../azure-monitor/platform/metrics-supported.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     N/D    |[Aplicativos Lógicos](../../azure-monitor/platform/metrics-supported.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/D     | [Gateways do Aplicativo](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/dnsZones | N/D| [Zonas DNS](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkdnszones) |
|Microsoft.Network/expressRouteCircuits | N/D |  [Circuitos do ExpressRoute](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/loadBalancers (apenas para SKUs Standard)| Sim| [Balanceadores de carga](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  N/D       |[Endereços IP Públicos](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkpublicipaddresses)|
|Microsoft.Network/trafficManagerProfiles | Sim | [Perfis do Gerenciador de Tráfego](../../azure-monitor/platform/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.OperationalInsights/workspaces| Sim|[Workspaces do Log Analytics](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces)|
|Microsoft.PowerBIDedicated/capacities | N/D | [Capacidades](../../azure-monitor/platform/metrics-supported.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices     |   N/D      |[Serviços de pesquisa](../../azure-monitor/platform/metrics-supported.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Sim       |[Barramento de Serviço](../../azure-monitor/platform/metrics-supported.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Sim     | [Contas de Armazenamento](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Sim    | [Serviços de blob](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsblobservices), [Serviços de arquivo](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsfileservices), [Serviços de fila](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsqueueservices) e [Serviços de tabela](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/D       | [Stream Analytics](../../azure-monitor/platform/metrics-supported.md#microsoftstreamanalyticsstreamingjobs)|
| Microsoft.Web/serverfarms | Sim | [Planos do Serviço de Aplicativo](../../azure-monitor/platform/metrics-supported.md#microsoftwebserverfarms)  |
| Microsoft.Web/sites | Sim | [Serviços de Aplicativos](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-excluding-functions) e [Funções](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-functions)|
| Microsoft.Web/sites/slots | Sim | [Slots do Serviço de Aplicativo](../../azure-monitor/platform/metrics-supported.md#microsoftwebsitesslots)|


## <a name="payload-schema"></a>Esquema de conteúdo

A operação POST contém a carga JSON e esquema a seguir para todos os alertas de métrica mais recentes quando o [grupo de ações ](../../azure-monitor/platform/action-groups.md) adequadamente configurado é usado:

```json
{
  "schemaId": "AzureMonitorMetricAlert",
  "data": {
    "version": "2.0",
    "status": "Activated",
    "context": {
      "timestamp": "2018-02-28T10:44:10.1714014Z",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
      "name": "StorageCheck",
      "description": "",
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "condition": {
        "windowSize": "PT5M",
        "allOf": [
          {
            "metricName": "Transactions",
            "dimensions": [
              {
                "name": "AccountResourceId",
                "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
              },
              {
                "name": "GeoType",
                "value": "Primary"
              }
            ],
            "operator": "GreaterThan",
            "threshold": "0",
            "timeAggregation": "PT5M",
            "metricValue": 1
          }
        ]
      },
      "subscriptionId": "00000000-0000-0000-0000-000000000000",
      "resourceGroupName": "Contoso",
      "resourceName": "diag500",
      "resourceType": "Microsoft.Storage/storageAccounts",
      "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
      "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
    },
    "properties": {
      "key1": "value1",
      "key2": "value2"
    }
  }
}
```

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre a nova [Experiência de alertas](../../azure-monitor/platform/alerts-overview.md).
* Saiba mais sobre os [alertas de log no Azure](../../azure-monitor/platform/alerts-unified-log.md).
* Saiba mais sobre os [Alertas no Azure](../../azure-monitor/platform/alerts-overview.md).
