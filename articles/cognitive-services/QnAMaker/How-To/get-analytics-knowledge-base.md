---
title: Análise na base de dados de conhecimento
titleSuffix: Azure Cognitive Services
description: O QnA Maker armazena todos os logs de chat e outros dados telemétricos, se você habilitou o App Insights durante a criação do serviço do QnA Maker. Execute os exemplos de consultas para obter seus logs de bate-papo do App Insights.
services: cognitive-services
author: tulasim88
manager: nitinme
displayName: chat history, history, chat logs, logs
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/04/2019
ms.author: tulasim
ms.openlocfilehash: 09369e760c654e7e56067e6da02bb772bffa7400
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960829"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Obter a análise em sua base de dados de conhecimento

O QnA Maker armazena todos os logs de bate-papo e outros dados de telemetria, se você tiver habilitado o App Insights durante a [criação do seu serviço QnA Maker](./set-up-qnamaker-service-azure.md). Execute os exemplos de consultas para obter seus logs de bate-papo do App Insights.

1. Vá até o recurso do App Insights.

    ![Selecione o seu recurso do Application Insights](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. Selecione **Análise**. Uma nova janela será aberta onde você pode consultar os dados de telemetria do QnA Maker.

    ![Selecionar análise](../media/qnamaker-how-to-analytics-kb/analytics.png)

3. Cole a consulta a seguir e execute-a.

    ```query
        requests
        | where url endswith "generateAnswer"
        | project timestamp, id, name, resultCode, duration
        | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
        | join kind= inner (
        traces | extend id = operation_ParentId
        ) on id
        | extend question = tostring(customDimensions['Question'])
        | extend answer = tostring(customDimensions['Answer'])
        | project KbId, timestamp, resultCode, duration, question, answer
    ```

    Clique em **Executar** para executar a consulta.

    ![Executar consulta](../media/qnamaker-how-to-analytics-kb/run-query.png)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Executar consultas para outras análises em sua base de dados de conhecimento do QnA Maker

### <a name="total-90-day-traffic"></a>Tráfego total de 90 dias

```query
    //Total Traffic
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by bin(timestamp, 1d), KbId
```

### <a name="total-question-traffic-in-a-given-time-period"></a>Tráfego total de perguntas em um determinado período de tempo

```query
    //Total Question Traffic in a given time period
    let startDate = todatetime('2018-02-18');
    let endDate = todatetime('2018-03-12');
    requests
    | where timestamp <= endDate and timestamp >=startDate
    | where url endswith "generateAnswer" and name startswith "POST" 
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by KbId
```

### <a name="user-traffic"></a>Tráfego de usuários

```query
    //User Traffic
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId 
    ) on id
    | extend UserId = tostring(customDimensions['UserId'])
    | summarize ChatCount=count() by bin(timestamp, 1d), UserId, KbId
```

### <a name="latency-distribution-of-questions"></a>Distribuição de latência das perguntas

```query
    //Latency distribution of questions
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | project timestamp, id, name, resultCode, performanceBucket, KbId
    | summarize count() by performanceBucket, KbId
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Gerenciar chaves](./key-management.md)
