---
title: 'Início Rápido: Lista as vozes de texto em fala, Python - serviços de fala'
titleSuffix: Azure Cognitive Services
description: Neste início rápido, você aprenderá como obter a lista completa de vozes neurais e padrão para um região/ponto de extremidade usando o Python. A lista é retornada como JSON e disponibilidade de voz varia por região.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: erhopf
ms.openlocfilehash: 66bda68b1313a7c172e273671bc3a03503d08e0d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58876573"
---
# <a name="quickstart-get-the-list-of-text-to-speech-voices-using-python"></a>Início Rápido: Obter a lista de texto em fala vozes usando Python

Neste início rápido, você aprenderá como obter a lista completa de vozes neurais e padrão para um região/ponto de extremidade usando o Python. A lista é retornada como JSON e disponibilidade de voz varia por região. Para obter uma lista de regiões com suporte, consulte [regiões](regions.md).

Este início rápido requer uma [conta de serviços Cognitivos do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com um recurso de serviços de fala. Se não tiver uma conta, você poderá usar a [avaliação gratuita](get-started.md) para obter uma chave de assinatura.

## <a name="prerequisites"></a>Pré-requisitos

Este início rápido requer:

* Python 2.7.x ou 3.x
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) ou seu editor de texto favorito
* Uma chave de assinatura do Azure para os serviços de fala

## <a name="create-a-project-and-import-required-modules"></a>Criar um projeto e importar os módulos necessários

Crie um novo projeto Python usando seu IDE ou editor favorito. Em seguida, copie esse trecho de código para seu projeto em um arquivo denominado `get-voices.py`.

```python
import requests
```

> [!NOTE]
> Se você nunca usou esses módulos, você precisará instalá-los antes de executar o programa. Para instalar esses pacotes, execute: `pip install requests`.

Solicitações é usado para solicitações HTTP para o serviço de texto em fala.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Defina a chave de assinatura e crie um prompt para o TTS

Nas próximas seções, você criará métodos para manipular a autorização, chamar a API de conversão de texto em fala e validar a resposta. Vamos começar criando uma classe. É onde colocaremos nossos métodos para troca de tokens e chamaremos a API text-to-speech.

```python
class GetVoices(object):
    def __init__(self, subscription_key):
        self.subscription_key = subscription_key
        self.access_token = None
```

A `subscription_key` é sua chave exclusiva do portal do Azure.

## <a name="get-an-access-token"></a>Obter um token de acesso

Esse ponto de extremidade requer um token de acesso para autenticação. Para obter um token de acesso, é necessária uma troca. Este exemplo troca sua chave de assinatura de serviços de fala para obter um token de acesso usando o `issueToken` ponto de extremidade.

Este exemplo pressupõe que sua assinatura dos serviços de fala é na região Oeste dos EUA. Se você estiver usando uma região diferente, atualize o valor para `fetch_token_url`. Para uma lista completa, consulte [Regiões](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Copie este código para a classe `GetVoices`:

```python
def get_token(self):
    fetch_token_url = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    headers = {
        'Ocp-Apim-Subscription-Key': self.subscription_key
    }
    response = requests.post(fetch_token_url, headers=headers)
    self.access_token = str(response.text)
```

> [!NOTE]
> Para obter mais informações sobre autenticação, consulte [Autenticar com um token de acesso](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

## <a name="make-a-request-and-save-the-response"></a>Faça uma solicitação e salve a resposta

Aqui você vai para a solicitação de compilação e salvar a lista de vozes retornadas. Primeiro, você precisa definir `base_url` e `path`. Este exemplo supõe que você esteja usando o endpoint do West US. Se o seu recurso estiver registrado em uma região diferente, atualize o `base_url`. Para obter mais informações, consulte [regiões de serviços de fala](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Em seguida, adicione os cabeçalhos necessários para a solicitação. Finalmente, você fará uma solicitação ao serviço. Se a solicitação for bem-sucedida, e um código de 200 status é retornado, a resposta é gravada em um arquivo.

Copie este código para a classe `GetVoices`:

```python
def get_voices(self):
    base_url = 'https://westus.tts.speech.microsoft.com'
    path = '/cognitiveservices/voices/list'
    get_voices_url = base_url + path
    headers = {
        'Authorization': 'Bearer ' + self.access_token
    }
    response = requests.get(get_voices_url, headers=headers)
    if response.status_code == 200:
        with open('voices.json', 'wb') as voices:
            voices.write(response.content)
            print("\nStatus code: " + str(response.status_code) + "\nvoices.json is ready to view.\n")
    else:
        print("\nStatus code: " + str(
            response.status_code) + "\nSomething went wrong. Check your subscription key and headers.\n")
```

## <a name="put-it-all-together"></a>Colocar tudo isso junto

Você está quase lá. Se a solicitação for bem-sucedida e um código de status 200 for retornado, a resposta de fala será gravada em um arquivo com registro de data e hora.

```python
if __name__ == "__main__":
    subscription_key = "YOUR_KEY_HERE"
    app = GetVoices(subscription_key)
    app.get_token()
    app.get_voices()
```

## <a name="run-the-sample-app"></a>Executar o aplicativo de exemplo

Isso é tudo, você está pronto para executar o exemplo. Na linha de comando (ou sessão de terminal), navegue até o diretório do projeto e execute:

```console
python get-voices.py
```

## <a name="clean-up-resources"></a>Limpar recursos

Remova todas as informações confidenciais do código-fonte do seu aplicativo de exemplo, como as chaves de assinatura.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Explorar exemplos do Python no GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/Python)

## <a name="see-also"></a>Consulte também

* [Referência de API de texto em fala](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Criar fontes de voz personalizada](how-to-customize-voice-font.md)
* [Grave amostras de voz para criar uma voz personalizada](record-custom-voice-samples.md)
