---
title: Exemplo de script da CLI do Azure - Criar e enviar um trabalho | Microsoft Docs
description: O script da CLI do Azure neste tópico mostra como enviar um trabalho para uma Transformação de codificação simples usando a URL HTTPs.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/01/2019
ms.author: juliako
ms.openlocfilehash: 63d036ea4faaf7e24f337fa3956986d165c84854
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57244325"
---
# <a name="cli-example-create-and-submit-a-job"></a>Exemplo de CLI: Criar e enviar um trabalho

Nos Serviços de Mídia v3, ao enviar Trabalhos para processar seus vídeos, você precisa informar os Serviços de Mídia onde encontrar o vídeo de entrada. Uma das opções é especificar uma URL HTTP como uma entrada de trabalho (conforme mostrado neste exemplo). 

## <a name="prerequisites"></a>Pré-requisitos 

[Crie uma conta de Serviços de Mídia](../create-account-cli-how-to.md).

[!INCLUDE [media-services-cli-instructions.md](../../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Script de exemplo

Ao executar `az ams job start`, você pode definir um rótulo na saída do trabalho. O rótulo pode ser usado posteriormente para identificar a finalidade desse ativo de saída. 

- Se você atribuir um valor ao rótulo, defina ‘--output-assets’ como “assetname=label”
- Se você não atribuir um valor ao rótulo, defina ‘--output-assets’ como “assetname=”.
  Observe que "=" é adicionado ao `output-assets`. 

```azurecli
az ams job start \
  --name testJob001 \
  --transform-name testEncodingTransform \
  --base-uri 'https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/' \
  --files 'Ignite-short.mp4' \
  --output-assets testOutputAssetName= \
  -a amsaccount \
  -g amsResourceGroup 
```

Você receberá uma resposta semelhante a esta:

```
{
  "correlationData": {},
  "created": "2019-02-15T05:08:26.266104+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform/jobs/testJob001",
  "input": {
    "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
    "files": [
      "Ignite-short.mp4"
    ],
    "label": null,
    "odatatype": "#Microsoft.Media.JobInputHttp"
  },
  "lastModified": "2019-02-15T05:08:26.266104+00:00",
  "name": "testJob001",
  "outputs": [
    {
      "assetName": "testOutputAssetName",
      "error": null,
      "label": "",
      "odatatype": "#Microsoft.Media.JobOutputAsset",
      "progress": 0,
      "state": "Queued"
    }
  ],
  "priority": "Normal",
  "resourceGroup": "amsResourceGroup",
  "state": "Queued",
  "type": "Microsoft.Media/mediaservices/transforms/jobs"
}
```

## <a name="next-steps"></a>Próximas etapas

Para obter mais exemplos, consulte [exemplos de CLI do Azure](../cli-samples.md).
