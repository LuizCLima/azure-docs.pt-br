---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2019
ms.openlocfilehash: d1c880ddc90ae3ce18dfde7e1983b45ac239de85
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58886821"
---
As configurações `Logging` gerenciam o suporte ao registro em log do ASP.NET Core para o contêiner. Você pode usar as mesmas definições de configuração e valores para o contêiner usado em um aplicativo ASP.NET Core. 

Os provedores de registro em log a seguir são suportados pelo contêiner:

|Provedor|Finalidade|
|--|--|
|[Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|O ASP.NET Core `Console` provedor de log. Todas as definições de configuração do ASP.NET Core e os valores padrão desse provedor de log são suportados.|
|[Depurar](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|O ASP.NET Core `Debug` provedor de log. Todas as definições de configuração do ASP.NET Core e os valores padrão desse provedor de log são suportados.|
|[Disco](#disk-logging)|O provedor de log do JSON. Esse provedor de log grava dados de log na montagem de saída.|

Esse comando contêiner armazena informações de registro em log no formato JSON para a montagem de saída:

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
Logging:Disk:Format=json
```

Esse comando contêiner mostra informações de depuração, prefixado com `dbug`, enquanto o contêiner está em execução:

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
Logging:Console:LogLevel:Default=Debug
```

### <a name="disk-logging"></a>Registro em log do Disk

O provedor de registro `Disk` suporta as seguintes configurações:  

| NOME | Tipo de dados | DESCRIÇÃO |
|------|-----------|-------------|
| `Format` | Cadeia de caracteres | O formato de saída para arquivos de log.<br/> **Observação:** Esse valor deve ser definido como `json` para habilitar o provedor de logs. Se esse valor for especificado sem especificar também uma montagem de saída durante a instanciação de um contêiner, ocorrerá um erro. |
| `MaxFileSize` | Número inteiro | O tamanho máximo, em megabytes (MB), de um arquivo de log. Quando o tamanho do arquivo de log atual atende ou excede esse valor, um novo arquivo de log é iniciado pelo provedor de log. Se -1 for especificado, o tamanho do arquivo de log será limitado apenas pelo tamanho máximo do arquivo, se houver, para a montagem de saída. O valor padrão é 1. |

Para obter mais informações sobre como configurar o suporte ao registro em log do ASP.NET Core, consulte [Configuração do arquivo de configurações](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1).

