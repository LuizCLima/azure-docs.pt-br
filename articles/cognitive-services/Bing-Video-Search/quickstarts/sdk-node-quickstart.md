---
title: 'Início Rápido: Pesquisar vídeos usando o SDK de Pesquisa de Vídeo do Bing para Node.js'
titleSuffix: Azure Cognitive Services
description: Use este guia de início rápido para enviar solicitações de pesquisa de vídeo usando o SDK de Pesquisa de Vídeo do Bing para Node.js
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: rosh
ms.openlocfilehash: ffd03a900ec304a930dfc64e7860e28e0374da55
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863748"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-nodejs"></a>Início rápido: Executar uma pesquisa de vídeo com o SDK de Pesquisa de Vídeo do Bing para Node.js

Use este início rápido para começar a pesquisar notícias com o SDK de Pesquisa de Vídeo do Bing para Node.js. Embora a Pesquisa de Vídeo do Bing tenha uma API REST compatível com a maioria das linguagens de programação, o SDK fornece uma maneira fácil de integrar o serviço aos seus aplicativos. O código-fonte desta amostra pode ser encontrado no [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js). Ele contém mais recursos e anotações.

## <a name="prerequisites"></a>Pré-requisitos

- [Node.js](https://www.nodejs.org/)

Para configurar o aplicativo de console usando o SDK da Pesquisa de Vídeo do Bing:
* Execute `npm install ms-rest-azure` no seu ambiente de desenvolvimento.
* Execute `npm install azure-cognitiveservices-videosearch` no seu ambiente de desenvolvimento.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Criar e inicializar o aplicativo

1. Crie um novo arquivo JavaScript em seu IDE ou editor favorito e adicione uma instrução `require()` ao SDK de Pesquisa de Vídeo do Bing e o módulo `CognitiveServicesCredentials`. Criar uma variável para a chave de assinatura. 
    
    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
    ```

2. Crie uma instância de `CognitiveServicesCredentials` com sua chave. Em seguida, use-a para criar uma instância do cliente de pesquisa de vídeo.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let client = new VideoSearchAPIClient(credentials);
    ```

## <a name="send-the-search-request"></a>Enviar a solicitação de pesquisa

1. Use `client.videosOperations.search()` para enviar uma solicitação de pesquisa para a API de Pesquisa de Vídeo do Bing. Quando os resultados da pesquisa forem retornados, use `.then()` para registrar o resultado em log.
    
    ```javascript
    client.videosOperations.search('Interstellar Trailer').then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

<!-- Remove until the response can be replace with a sanitized version.
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'VideoObjectElementType'

![Video results](media/video-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Criar um aplicativo Web de página única](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Consulte também 

* [O que é a API da Pesquisa de Vídeo do Bing?](../overview.md)
* [Exemplos de SDK do .NET dos Serviços Cognitivos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)