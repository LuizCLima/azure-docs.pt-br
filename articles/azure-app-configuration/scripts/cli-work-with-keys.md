---
title: Amostra de script da CLI do Azure – Trabalhar com pares chave-valor em um repositório de configurações de aplicativo do Azure | Microsoft Docs
description: Fornece informações sobre como trabalhar com pares chave-valor em um repositório de configurações de aplicativo do Azure
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: azure-app-configuration
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 9288ea08da6335dd29e7a15a9bc871b76c1ce7e9
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57438424"
---
# <a name="work-with-key-values-in-an-azure-app-configuration-store"></a>Trabalhar com pares chave-valor em um repositório de configurações de aplicativo do Azure

Este script de exemplo cria um par chave-valor em um repositório de configurações de aplicativo do Azure, lista todos os pares chave-valor existentes, atualiza o valor da chave recém-criada e, por fim, o exclui.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se você optar por instalar e usar a CLI localmente, este artigo exigirá que seja executada a CLI do Azure versão 2.0 ou posterior. Execute `az --version` para encontrar a versão. Se você precisar instalar ou atualizar, confira [Instalar a CLI do Azure](/cli/azure/install-azure-cli).

É necessário instalar a extensão da CLI da Configuração de Aplicativo do Azure primeiro executando o seguinte comando:

        az extension add -n appconfig

## <a name="sample-script"></a>Script de exemplo

```azurecli-interactive
#!/bin/bash

appConfigName=myTestAppConfigStore
newKey="TestKey"

# Create a new key-value 
az appconfig kv set --name $appConfigName --key $newKey --value "Value 1"

# List current key-values
az appconfig kv list --name $appConfigName

# Update new key's value
az appconfig kv set --name $appConfigName --value "Value 2"

# List current key-values
az appconfig kv list --name $appConfigName

# Delete new key
az appconfig kv delete  --name $appConfigName --key $newKey

# List current key-values
az appconfig kv list --name $appConfigName
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação sobre o script

Este script usa os comandos a seguir para operar em pares chave-valor em um repositório de configurações de aplicativo. Cada comando da tabela é vinculado à documentação específica do comando.

| Comando | Observações |
|---|---|
| [az appconfig kv set](/cli/azure/ext/appconfig/appconfig) | Cria ou atualiza um par chave-valor. |
| [az appconfig kv list](/cli/azure/ext/appconfig/appconfig) | Lista os pares chave-valor em um repositório de configurações de aplicativo. |
| [az appconfig kv delete](/cli/azure/ext/appconfig/appconfig) | Exclui um par chave-valor. |

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre a CLI do Azure, veja a [documentação da CLI do Azure](/cli/azure).

Encontre mais amostras de script da CLI da Configuração de Aplicativo na [documentação da Configuração de Aplicativo do Azure](../cli-samples.md).
