---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 10/24/2018
ms.author: cephalin
ms.openlocfilehash: e4bc749047bbf0d55fc60a699ac956421775d5b0
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "58116080"
---
No [portal do Azure](https://portal.azure.com), clique em **Grupo de recursos** > **cloud-shell-storage-\<your_region>** > **\<storage_account_name>**.

![Encontrar a conta do armazenamento do Cloud Shell](../articles/app-service/media/app-service-deploy-zip/upload-choose-storage-account.png)

Na página **Visão geral** da conta de armazenamento, selecione **Arquivos**.

Selecione o compartilhamento de arquivo gerado automaticamente e selecione **Carregar**. Esse compartilhamento de arquivos está montado no Cloud Shell como `clouddrive`.

![Encontrar o botão Carregar](../articles/app-service/media/app-service-deploy-zip/upload-select-button.png)

Clique no seletor de arquivo, selecione o arquivo ZIP e clique em **Carregar**. 

No Cloud Shell, use `ls` para verificar que você pode ver o arquivo ZIP carregado no compartilhamento `clouddrive` padrão.

```azurecli-interactive
ls clouddrive
```
