---
title: Entender as entradas para o Azure Stream Analytics
description: Este artigo descreve o conceito de entradas em um trabalho do Azure Stream Analytics, comparando o streaming de entrada para entrada de dados de referência.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: 408a77dd5409f8604a059d3bc7f37ffe1e3d6ba2
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57855360"
---
# <a name="understand-inputs-for-azure-stream-analytics"></a>Entender as entradas para o Azure Stream Analytics

Os trabalhos do Azure Stream Analytics se conectam a uma ou mais entradas de dados. Cada entrada define uma conexão com uma fonte de dados existente. O Stream Analytics aceita entrada de dados de vários tipos de fontes de eventos, incluindo Hubs de Eventos, Hub IoT e armazenamento de Blob. As entradas são referenciadas por nome na consulta SQL de streaming que você grava para cada trabalho. Na consulta, você pode unir várias entradas para mesclar dados ou comparar dados de streaming com uma pesquisa de dados de referência e passar os resultados para saídas. 

O Stream Analytics tem integração de primeira classe de três tipos de recursos como entradas:
- [Hubs de eventos do Azure](https://azure.microsoft.com/services/event-hubs/)
- [Hub IoT do Azure](https://azure.microsoft.com/services/iot-hub/) 
- [Armazenamento de Blobs do Azure](https://azure.microsoft.com/services/storage/blobs/) 

Esses recursos de entrada podem ser da mesma assinatura do Azure que o trabalho do Stream Analytics ou de uma assinatura diferente.

Você pode usar o [portal do Azure](stream-analytics-quick-create-portal.md#configure-job-input), [do Azure PowerShell](https://docs.microsoft.com/powershell/module/az.streamanalytics/New-azStreamAnalyticsInput), [.NET API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions), [API REST](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-input), e [Visual Studio](stream-analytics-tools-for-visual-studio-install.md)para criar, editar e testar entradas de trabalho do Stream Analytics.

## <a name="stream-and-reference-inputs"></a>Entradas de referência e de transmissão
Como os dados são enviados a uma fonte de dados, eles são consumidos pelo trabalho do Stream Analytics e processados em tempo real. As entradas são divididas em dois tipos: entradas de fluxo de dados e entradas de dados de referência.

### <a name="data-stream-input"></a>Entrada de fluxo de dados
Um fluxo de dados é uma sequência ilimitada de eventos ao longo do tempo. Os trabalhos do Stream Analytics devem conter pelo menos uma fonte de entrada de fluxo de dados. O Armazenamento de Blobs, os Hubs de Eventos e o Hub IoT têm suporte como fontes de entrada de fluxo de dados. Os Hubs de Eventos são usados para coletar fluxos de eventos de vários dispositivos e serviços. Esses fluxos podem incluir os feeds de atividades de mídia social, informações de mercado de ações ou dados de sensores. Os Hubs IoT são otimizados para coletar dados de dispositivos conectados em cenários da Internet das Coisas (IoT).  O Armazenamento de Blobs pode ser usado como uma fonte de entrada para ingerir dados em massa como uma transmissão, tais como arquivos de log.  

Para obter mais informações sobre entradas de dados de streaming, consulte [Dados de streaming como entrada no Stream Analytics](stream-analytics-define-inputs.md)

### <a name="reference-data-input"></a>Entrada de dados de referência
O Stream Analytics também dá suporte à entrada conhecida como *dados de referência*. Dados de referência são totalmente estáticos ou são alterados lentamente. Normalmente, eles são usados para executar correlação e pesquisas. Por exemplo, você pode unir dados na entrada de fluxo de dados a dados nos dados de referência, assim como você realizaria uma junção SQL para pesquisar valores estáticos. O Armazenamento de Blob do Azure é a única fonte de entrada com suporte para dados de referência. Os blobs de origem de dados de referência têm um limite de até 300 MB, dependendo da complexidade da consulta e das Unidades de fluxo alocadas.

Para obter mais informações sobre entradas de dados de referência, consulte [Usando dados de referência para pesquisas no Stream Analytics](stream-analytics-use-reference-data.md)

## <a name="next-steps"></a>Próximas etapas
> [!div class="nextstepaction"]
> [Início Rápido: Criar um trabalho do Stream Analytics usando o portal do Azure](stream-analytics-quick-create-portal.md)
