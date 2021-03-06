---
title: Configurar Postman para chamadas de API de REST dos Serviços de Mídia do Azure
description: Saiba como configurar Postman para chamadas de API de REST dos Serviços de Mídia.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 5d0b5a57f3fe587a06a102c958b17dbf2a73225c
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58805135"
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Configurar Postman para chamadas de API de REST dos Serviços de Mídia  

> [!NOTE]
> Não estão sendo adicionados novos recursos ou funcionalidades aos Serviços de Mídia v2. <br/>Confira a versão mais recente, [Serviços de Mídia v3](https://docs.microsoft.com/azure/media-services/latest/). Consulte também [diretrizes de migração da v2 para v3](../latest/migrate-from-v2-to-v3.md)

Este tutorial mostra como configurar **Postman** para que ele possa ser usado para chamar APIs de REST dos Serviços de Mídia do Azure (AMS). O tutorial mostra como importar arquivos do ambiente e da coleção no **Postman**. A coleção contém definições agrupadas de solicitações HTTP que chamam as APIs de REST dos Serviços de Mídia do Azure (AMS). O arquivo de ambiente contém variáveis que são usadas pela coleção.

Este ambiente e a coleção são usados em artigos que mostram como realizar várias tarefas com APIs de REST dos Serviços de Mídia do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- Instale o cliente REST do [Postman](https://www.getpostman.com/) para executar as APIs de REST mostradas em alguns dos tutoriais do REST AMS. 

    Estamos usando o **Postman** mas qualquer ferramenta REST seria adequada. As outras alternativas são: **Visual Studio Code** com o plug-in do REST ou **Telerik Fiddler**. 

## <a name="configure-the-environment"></a>Configure o ambiente 

1. Crie um arquivo. JSON contendo as variáveis de ambiente usadas nos tutoriais AMS. Nomeie o arquivo (por exemplo, **AzureMediaServices.postman_environment.json**). Abra o arquivo e cole o código que define o ambiente do Postman nesta [listagem de código](postman-environment.md). 
2. Abra o aplicativo **Postman**.
3. À direita da tela, selecione a opção **Gerenciar ambiente**.

    ![Carregar um arquivo](./media/media-services-rest-upload-files/postman-create-env.png)
4. Na caixa de diálogo **Gerenciar ambiente**, clique em **Importar**.
5. Navegue e selecione o arquivo **AzureMediaServices.postman_environment.json**.
6. O ambiente **AzureMedia** é adicionado.
7. Feche a caixa de diálogo.
8. Selecione o ambiente do **AzureMedia**.

    ![Carregar um arquivo](./media/media-services-rest-upload-files/postman-choose-env.png)

## <a name="configure-the-collection"></a>Configurar a coleção

1. Crie um arquivo. JSON contendo a coleção do **Postman** com todas as operações que são necessárias para carregar um arquivo nos serviços de mídia. Nomeie o arquivo (por exemplo, **AzureMediaServicesOperations.postman_collection.json**). Abra o arquivo e cole o código que define a coleção do **Postman** nesta [listagem de código](postman-collection.md).
2. Clique em **Importar** para importar o arquivo de coleção.
3. Escolha o arquivo **AzureMediaServicesOperations.postman_collection.json**.

    ![Carregar um arquivo](./media/media-services-rest-upload-files/postman-import-collection.png)

## <a name="next-steps"></a>Próximas etapas

Confira o artigo sobre como [carregar ativos](media-services-rest-upload-files.md).  
