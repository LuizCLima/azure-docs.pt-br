---
title: Esquemas de acompanhamento personalizados para mensagens B2B – Aplicativos Lógicos do Azure | Microsoft Docs
description: Criar esquemas de acompanhamento personalizados que monitorem mensagens B2B nas contas de integração para os Aplicativos Lógicos do Azure com o Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.date: 01/27/2017
ms.openlocfilehash: f919e9a7cca210fa5920bcc6bed05a9a41fba8bf
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57192377"
---
# <a name="create-custom-tracking-schemas-that-monitor-end-to-end-workflows-in-azure-logic-apps"></a>Criar esquemas de acompanhamento personalizados que monitorem fluxos de trabalho de ponta a ponta nos Aplicativos Lógicos do Azure

Há um acompanhamento interno que você pode habilitar para diferentes partes do fluxo de trabalho entre empresas, como acompanhamento de mensagens AS2 ou X12. Quando você cria fluxos de trabalho que incluem um aplicativo lógico, o BizTalk Server, o SQL Server ou qualquer outra camada, é possível habilitar o acompanhamento personalizado que registra eventos desde o início até o fim do fluxo de trabalho. 

Este artigo fornece código personalizado que você pode usar nas camadas fora do aplicativo lógico. 

## <a name="custom-tracking-schema"></a>Esquema de controle personalizado

```json
{
   "sourceType": "",
   "source": {
      "workflow": {
         "systemId": ""
      },
      "runInstance": {
         "runId": ""
      },
      "operation": {
         "operationName": "",
         "repeatItemScopeName": "",
         "repeatItemIndex": "",
         "trackingId": "",
         "correlationId": "",
         "clientRequestId": ""
      }
   },
   "events": [
      {
         "eventLevel": "",
         "eventTime": "",
         "recordType": "",
         "record": {                
         }
      }
   ]
}
```

| Propriedade | Type | DESCRIÇÃO |
| --- | --- | --- |
| sourceType |   | O tipo da fonte de execução. Os valores permitidos são **Microsoft.Logic/workflows** e **personalizado**. (Obrigatório) |
| Fonte |   | Se o tipo de fonte for **Microsoft.Logic/workflows**, as informações de origem precisarão seguir este esquema. Se o tipo de origem for **personalizado**, o esquema será um JToken. (Obrigatório) |
| systemId | Cadeia de caracteres | ID do sistema de aplicativo lógico. (Obrigatório) |
| runId | Cadeia de caracteres | ID de execução do aplicativo lógico. (Obrigatório) |
| operationName | Cadeia de caracteres | O nome da operação (por exemplo, ação ou gatilho). (Obrigatório) |
| repeatItemScopeName | Cadeia de caracteres | Repita o nome do item se a ação estiver dentro de um loop `foreach`/`until`. (Obrigatório) |
| repeatItemIndex | Número inteiro | Se a ação está ou não dentro de um loop `foreach`/`until`. Indica o índice de itens repetidos. (Obrigatório) |
| trackingId | Cadeia de caracteres | A ID de acompanhamento para correlacionar as mensagens. (Opcional) |
| correlationId | Cadeia de caracteres | A ID de correlação para correlacionar as mensagens. (Opcional) |
| clientRequestId | Cadeia de caracteres | O cliente pode populá-la para correlacionar mensagens. (Opcional) |
| eventLevel |   | Nível do evento. (Obrigatório) |
| eventTime |   | A hora do evento no formato UTC AAAA-MM-DDTHH:MM:SS.00000Z. (Obrigatório) |
| recordType |   | Tipo do registro de acompanhamento. O valor permitido é **personalizado**. (Obrigatório) |
| record |   | Tipo de registro personalizado. O formato permitido é JToken. (Obrigatório) |
||||

## <a name="b2b-protocol-tracking-schemas"></a>Esquemas de acompanhamento do protocolo B2B

Para obter informações sobre esquemas de acompanhamento do protocolo B2B, veja:

* [Esquemas de acompanhamento de AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [Esquemas de acompanhamento de X12](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre o [monitoramento de mensagens B2B](logic-apps-monitor-b2b-message.md)
* Saiba mais sobre [rastreando mensagens B2B nos logs do Azure Monitor](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)