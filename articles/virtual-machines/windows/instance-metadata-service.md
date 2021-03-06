---
title: Serviço de Metadados de Instância do Azure | Microsoft Docs
description: A Interface RESTful para obter informações sobre a de computação, a rede e os eventos de manutenção futura da VM do Windows.
services: virtual-machines-windows
documentationcenter: ''
author: KumariSupriya
manager: harijayms
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/28/2019
ms.author: sukumari
ms.reviewer: azmetadata
ms.openlocfilehash: cc2a78dd547681a4b20fea14cd8cd7f4fd9c2df5
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58847975"
---
# <a name="azure-instance-metadata-service"></a>Serviço de Metadados de Instância do Azure

O Serviço de metadados de instância do Azure fornece informações sobre instâncias da máquina virtual em execução que podem ser usadas para gerenciar e configurar suas máquinas virtuais.
Isso inclui informações como SKU, configuração de rede e eventos de manutenção futura. Para obter mais informações sobre o tipo de informação que está disponível, consulte [categorias de metadados](#instance-metadata-data-categories).

O serviço de metadados de instância do Azure é um ponto de extremidade REST disponível para todas as máquinas virtuais de IaaS criadas por meio [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).
O ponto de extremidade está disponível em um endereço IP não roteável conhecido (`169.254.169.254`) que pode ser acessado somente de dentro da máquina virtual.

> [!IMPORTANT]
> Esse serviço está **disponível geralmente** em todas as regiões do Azure.  Regularmente, ele recebe atualizações para expor informações novas sobre instâncias de máquina virtual. Esta página reflete as atualizadas [categorias de dados](#instance-metadata-data-categories) disponíveis.

## <a name="service-availability"></a>Disponibilidade do serviço

Esse serviço está disponível nas regiões do Azure disponíveis para o público geral. Nem todas as versões de API podem estar disponíveis em todas as regiões do Azure.

Regiões                                        | Disponibilidade?                                 | Versões com suporte
-----------------------------------------------|-----------------------------------------------|-----------------
[Todas as regiões globais do Azure disponíveis](https://azure.microsoft.com/regions/)     | Disponível   | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01
[Azure Governamental](https://azure.microsoft.com/overview/clouds/government/)              | Disponível | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01
[Azure China:](https://www.azure.cn/)                                                           | Disponível | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01
[Azure Alemanha](https://azure.microsoft.com/overview/clouds/germany/)                    | Disponível | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01

A tabela é atualizada quando há atualizações de serviço e/ou novas versões com suporte ficam disponíveis.

Para testar o serviço de metadados de instância, crie uma VM do [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) ou [portal do Azure](https://portal.azure.com) nas regiões acima e siga os exemplos abaixo.

## <a name="usage"></a>Uso

### <a name="versioning"></a>Controle de versão

O Serviço de Metadados de Instância tem controle de versão. As versões são obrigatórias e a versão atual no Azure Global é `2018-10-01`. As atuais versões compatíveis são (2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01)

Conforme versões mais recentes são adicionadas, as versões mais antigas ainda podem ser acessadas para fins de compatibilidade se os scripts tiverem dependências de formatos de dados específicos.

Quando nenhuma versão for especificada, um erro é retornado com uma lista das versões mais recentes com suporte.

> [!NOTE]
> A resposta é uma cadeia de caracteres JSON. Todas as respostas de exemplo a seguir são estilos de formatação para facilitar a leitura.

**Solicitação**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance"
```

**Resposta**

```json
{
    "error": "Bad request. api-version was not specified in the request. For more information refer to aka.ms/azureimds",
    "newest-versions": [
        "2018-10-01",
        "2018-04-02",
        "2018-02-01"
    ]
}
```

### <a name="using-headers"></a>Uso de cabeçalhos

Ao consultar o Serviço de Metadados você deverá fornecer o cabeçalho `Metadata: true` para garantir que a solicitação não seja redirecionada de forma involuntária.

### <a name="retrieving-metadata"></a>Configurando e recuperando os metadados

Os metadados de instância estão disponíveis para a execução de máquinas virtuais criadas/gerenciadas usando o [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Acessar todas as categorias de dados para uma instância de máquina virtual usando a seguinte solicitação:

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01"
```

> [!NOTE]
> Todas as consultas de metadados de instância diferenciam maiúsculas de minúsculas.

### <a name="data-output"></a>Saída de dados

Por padrão, o serviço de metadados de instância retorna dados em formato JSON (`Content-Type: application/json`). No entanto, diferentes APIs retornam dados em formatos diferentes, se solicitado.
A tabela a seguir é uma referência de outros formatos de dados que pode oferecer suporte a APIs.

API | Formato de dados padrão | Outros formatos
--------|---------------------|--------------
/instance | json | texto
/scheduledevents | json | Nenhum
/attested | json | Nenhum

Para acessar um formato de resposta não padrão, especifique o formato solicitado como um parâmetro de cadeia de caracteres de consulta na solicitação. Por exemplo: 

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01&format=text"
```

> [!NOTE]
> Para nós folha a `format=json` não funciona. Para essas consultas `format=text` precisa ser especificado explicitamente se o formato padrão é json.

### <a name="security"></a>Segurança

O ponto de extremidade de metadados de instância está acessível somente dentro da instância de máquina virtual em execução em um endereço IP não roteável. Além disso, qualquer solicitação com `X-Forwarded-For`Cabeçalho é rejeitada pelo serviço.
As solicitações também devem conter um `Metadata: true` cabeçalho para garantir que a solicitação real tenha sido desejada diretamente e não faça parte de um redirecionamento não intencional.

### <a name="error"></a>Erro

Se houver um elemento de dados não encontrado ou solicitações malformadas, o serviço de metadados da instância retornará o erro de HTTP padrão. Por exemplo:

Código de status HTTP | Motivo
----------------|-------
200 OK |
400 Solicitação Inválida | Faltando `Metadata: true` cabeçalho ou tem o formato ao consultar um nó folha
404 Não Encontrado | O elemento solicitado não existe
405 método não permitido | Somente as solicitações `GET` e `POST` são suportadas
429 Número excessivo de solicitações | A API atualmente suporta um máximo de 5 consultas por segundo
500 Erro do serviço     | Aguarde um pouco e tente novamente

### <a name="examples"></a>Exemplos

> [!NOTE]
> Todas as respostas de API são cadeias de caracteres JSON. Todas as respostas de exemplo a seguir são estilos de formatação para facilitar a leitura.

#### <a name="retrieving-network-information"></a>Recuperação das informações de rede

**Solicitação**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-08-01"
```

**Resposta**

> [!NOTE]
> A resposta é uma cadeia de caracteres JSON. Todas as respostas de exemplo a seguir são estilos de formatação para facilitar a leitura.

```json
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="retrieving-public-ip-address"></a>Recuperação do endereço IP público

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-08-01&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a>Recuperação de todos os metadados para uma instância

**Solicitação**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2018-10-01"
```

**Resposta**

> [!NOTE]
> A resposta é uma cadeia de caracteres JSON. Todas as respostas de exemplo a seguir são estilos de formatação para facilitar a leitura.

```json
{
  "compute": {
    "azEnvironment": "AZUREPUBLICCLOUD",
    "location": "westus",
    "name": "jubilee",
    "offer": "Windows-10",
    "osType": "Windows",
    "placementGroupId": "",
    "plan": {
        "name": "",
        "product": "",
        "publisher": ""
    },
    "platformFaultDomain": "1",
    "platformUpdateDomain": "1",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "MicrosoftWindowsDesktop",
    "resourceGroupName": "myrg",
    "sku": "rs4-pro",
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "Department:IT;Environment:Prod;Role:WorkerRole",
    "version": "17134.345.59",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_D1",
    "zone": "1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.2.5",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.2.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3A36DDED"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>Recuperação de metadados na máquina virtual do Windows

**Solicitação**

Os metadados de instância podem ser recuperados no Windows por meio do utilitário do `curl` programa:

```bash
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2018-10-01 | select -ExpandProperty Content
```

Ou através do `Invoke-RestMethod` cmdlet do PowerShell:

```powershell

Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2018-10-01 -Method get
```

**Resposta**

> [!NOTE]
> A resposta é uma cadeia de caracteres JSON. Todas as respostas de exemplo a seguir são estilos de formatação para facilitar a leitura.

```json
{
  "compute": {
    "azEnvironment": "AZUREPUBLICCLOUD",
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "placementGroupId": "",
    "plan": {
        "name": "",
        "product": "",
        "publisher": ""
    },
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "MicrosoftSQLServer",
    "resourceGroupName": "myrg",
    "sku": "Enterprise",
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "Department:IT;Environment:Test;Role:WebRole",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmScaleSetName": "",
    "vmSize": "Standard_DS2",
    "zone": ""
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a>Categorias de dados de metadados de instância

As seguintes categorias de dados estão disponíveis por meio do serviço de metadados da instância:

Dados | DESCRIÇÃO | Versão introduzida
-----|-------------|-----------------------
azEnvironment | Ambiente do Azure onde a VM está em execução em | 01-10-2018
location | Região do Azure na qual a máquina virtual está sendo executada | 2017-04-02
Nome | Nome da VM | 2017-04-02
oferta | Oferece informações para a imagem VM. Esse valor só está presente para as imagens implantadas na Galeria de imagens do Azure. | 2017-04-02
publicador | Publicador da imagem da máquina virtual | 2017-04-02
sku | SKU específica para a imagem da máquina virtual | 2017-04-02
version | Versão da imagem da máquina virtual | 2017-04-02
osType | Linux ou Windows | 2017-04-02
platformUpdateDomain |  [Domínio de atualização](manage-availability.md) no qual a máquina virtual está sendo executada | 2017-04-02
platformFaultDomain | [Domínio de falha](manage-availability.md) no qual a máquina virtual está sendo executada | 2017-04-02
vmId | [Identificador exclusivo](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) para a máquina virtual | 2017-04-02
vmSize | [Tamanho da VM](sizes.md) | 2017-04-02
subscriptionId | Assinatura do Azure para a Máquina Virtual | 2017-08-01
marcas | [Marcas](../../azure-resource-manager/resource-group-using-tags.md) para a sua Máquina Virtual  | 2017-08-01
resourceGroupName | [Grupo de recursos](../../azure-resource-manager/resource-group-overview.md) para a sua Máquina Virtual | 2017-08-01
placementGroupId | [Grupo de Posicionamento](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) do conjunto de dimensionamento da sua Máquina Virtual | 2017-08-01
plan | [Planejar](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#plan) para uma VM no seu uma imagem do Marketplace do Azure, contém o nome, o produto e o publicador | 2018-04-02
provider | Provedor da VM | 01-10-2018
publicKeys | [Coleção de chaves públicas](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#sshpublickey) atribuído à VM e caminhos | 2018-04-02
vmScaleSetName | [Nome do conjunto de dimensionamento de máquinas virtuais](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) do seu conjunto de dimensionamento de máquinas virtuais | 2017-12-01
zona | [Zona de Disponibilidade](../../availability-zones/az-overview.md) da máquina virtual | 2017-12-01
IPv4/privateIpAddress | Endereço IPv4 local da máquina virtual | 2017-04-02
IPv4/privateIpAddress | Endereço IPv4 local da máquina virtual | 2017-04-02
subnet/address | Endereço sub-rede da máquina virtual | 2017-04-02
subnet/prefix | Prefixo de sub-rede, exemplo 24 | 2017-04-02
ipv6/ipAddress | Endereço IPv6 local da máquina virtual | 2017-04-02
macAddress | Endereço mac da máquina virtual | 2017-04-02
scheduledevents | Consulte [Eventos agendados](scheduled-events.md) | 2017-08-01
identidade | Identidades gerenciadas para recursos do Azure. Veja [adquirir um token de acesso](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) | 01-02-2018
atestados | Confira [Dados Atestados](#attested-data) | 01-10-2018

## <a name="attested-data"></a>Dados Atestados

Os Metadados de Instância respondem ao ponto de extremidade http em 169.254.169.254. Parte do cenário atendido pelo Serviço de Metadados de Instância é fornecer garantias de que os dados respondidos estejam vindo do Azure. Assinamos parte dessas informações para que as imagens do Marketplace saibam que são suas as imagens em execução no Azure.

### <a name="example-attested-data"></a>Exemplo de Dados Atestados

 > [!NOTE]
> Todas as respostas de API são cadeias de caracteres JSON. As respostas de exemplo a seguir têm estilos de formatação para facilitar a leitura.

 **Solicitação**

 ```bash
curl -H Metadata:true "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890"

```

A versão da API é um campo obrigatório e a versão com suporte para dados atestados é 2018-10-01.
Nonce é uma cadeia de caracteres de 10 dígitos opcional fornecida. A nonce pode ser usada para rastrear a solicitação e, se não for fornecida, o carimbo de data/hora atual em UTC será retornado na cadeia de caracteres de resposta codificada.

 **Resposta**

> [!NOTE]
> A resposta é uma cadeia de caracteres JSON. Todas as respostas de exemplo a seguir são estilos de formatação para facilitar a leitura.

 ```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

> O blob de assinatura é uma versão assinada do [pkcs7](https://aka.ms/pkcs7) do documento. Ele contém o certificado usado na assinatura, juntamente com os detalhes da VM, como vmId, nonce, timeStamp para a criação e a expiração do documento e informações sobre a imagem do plano. As informações do plano são preenchidas apenas para as imagens do Azure Marketplace. O certificado pode ser extraído da resposta e usado para validar que a resposta é válida e proveniente do Azure.

#### <a name="retrieving-attested-metadata-in-windows-virtual-machine"></a>Recuperar metadados atestados na Máquina Virtual do Windows

 **Solicitação**

Os metadados de instância podem ser recuperados no Windows por meio do utilitário do Powershell`curl`:

 ```bash
curl -H @{'Metadata'='true'} "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890" | select -ExpandProperty Content
```

 Ou por meio de `Invoke-RestMethod` cmdlet:

 ```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890" -Method get
```

A versão da API é um campo obrigatório e a versão com suporte para dados atestados é 2018-10-01.
Nonce é uma cadeia de caracteres de 10 dígitos opcional fornecida. A nonce pode ser usada para rastrear a solicitação e, se não for fornecida, o carimbo de data/hora atual em UTC será retornado na cadeia de caracteres de resposta codificada.

 **Resposta**

> [!NOTE]
> A resposta é uma cadeia de caracteres JSON. Todas as respostas de exemplo a seguir são estilos de formatação para facilitar a leitura.

 ```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

> O blob de assinatura é uma versão assinada do [pkcs7](https://aka.ms/pkcs7) do documento. Ele contém o certificado usado na assinatura, juntamente com os detalhes da VM, como vmId, nonce, timeStamp para a criação e a expiração do documento e informações sobre a imagem do plano. As informações do plano são preenchidas apenas para as imagens do Azure Marketplace. O certificado pode ser extraído da resposta e usado para validar que a resposta é válida e proveniente do Azure.

## <a name="example-scenarios-for-usage"></a>Cenários de exemplo para uso  

### <a name="tracking-vm-running-on-azure"></a>VM de controle em execução no Azure

Como provedor de serviço, você talvez precise controlar o número de máquinas virtuais que executam o seu software ou ter agentes que precisam controlar a exclusividade da máquina virtual. Para obter uma ID exclusiva de uma máquina virtual, use o `vmId` campo do serviço de metadados de instância.

**Solicitação**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**Resposta**

```text
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Posicionamento de contêineres, partições de dados com base em domínio de falha/atualização 

Para determinados cenários nos quais o posicionamento de réplicas diferentes é de vital importância. Por exemplo, o [posicionamento de réplica de HDFS](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) ou o posicionamento do contêiner por meio de um [organizador](https://kubernetes.io/docs/user-guide/node-selection/) podem exigir que você conheça os `platformFaultDomain` e `platformUpdateDomain` nas quais a máquina virtual está sendo executada.
Também é possível usar as [Zonas de Disponibilidade](../../availability-zones/az-overview.md) das instâncias para tomar essas decisões.
Você pode consultar esses dados diretamente por meio de serviço de metadados.

**Solicitação**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text"
```

**Resposta**

```text
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a>Obtendo mais informações sobre a máquina virtual durante a ocorrência de suporte

Como provedor de serviços, você poderá receber uma chamada de suporte na qual gostaria de ter mais informações sobre a máquina virtual. Pedir ao cliente para informar os metadados de computação pode fornecer informações básicas para o profissional de suporte saber o tipo de VM no Azure. 

**Solicitação**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-08-01"
```

**Resposta**

> [!NOTE]
> A resposta é uma cadeia de caracteres JSON. Todas as respostas de exemplo a seguir são estilos de formatação para facilitar a leitura.

```json
{
  "compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }
}
```

### <a name="getting-azure-environment-where-the-vm-is-running"></a>Obter o Ambiente do Azure em que a VM está em execução

O Azure tem várias nuvens soberanas, como o [Azure Governamental](https://azure.microsoft.com/overview/clouds/government/). Às vezes, é necessário que o Ambiente do Azure tome algumas decisões de tempo de execução. O exemplo a seguir mostra como você pode gerar tal comportamento.

**Solicitação**
```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/azEnvironment?api-version=2018-10-01&format=text"
```

**Resposta**
```bash
AZUREPUBLICCLOUD
```

### <a name="getting-the-tags-for-the-vm"></a>Obtendo as marcas para a VM

As marcas podem foram aplicadas à sua VM do Azure para organizá-los logicamente em uma taxonomia. As marcas atribuídas a uma VM podem ser recuperadas usando-se a solicitação abaixo.

**Solicitação**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/tags?api-version=2018-10-01&format=text"
```

**Resposta**

```text
Department:IT;Environment:Test;Role:WebRole
```

### <a name="validating-that-the-vm-is-running-in-azure"></a>Validar que a VM está em execução no Azure

 Os fornecedores do Marketplace desejam atestar que seu software está licenciado para ser executado somente no Azure. É necessário haver uma maneira de detectar se alguém copia o VHD localmente. Ao chamar o Serviço de Metadados de Instância, os fornecedores do Marketplace podem obter dados assinados que garantam resposta apenas do Azure.
**Solicitação**
> [!NOTE]
> Requer que o jq seja instalado.

 ```bash
  # Get the signature
   curl  --silent -H Metadata:True http://169.254.169.254/metadata/attested/document?api-version=2018-10-01 | jq -r '.["signature"]' > signature
  # Decode the signature
  base64 -d signature > decodedsignature
  #Get PKCS7 format
  openssl pkcs7 -in decodedsignature -inform DER -out sign.pk7
  # Get Public key out of pkc7
  openssl pkcs7 -in decodedsignature -inform DER  -print_certs -out signer.pem
  #Get the intermediate certificate
  wget -q -O intermediate.cer "$(openssl x509 -in signer.pem -text -noout | grep " CA Issuers -" | awk -FURI: '{print $2}')"
  openssl x509 -inform der -in intermediate.cer -out intermediate.pem
  #Verify the contents
  openssl smime -verify -in sign.pk7 -inform pem -noverify
 ```

 **Resposta**

```json
Verification successful
{"nonce":"20181128-001617",
  "plan":
    {
     "name":"",
     "product":"",
     "publisher":""
    },
"timeStamp":
  {
    "createdOn":"11/28/18 00:16:17 -0000",
    "expiresOn":"11/28/18 06:16:17 -0000"
  },
"vmId":"d3e0e374-fda6-4649-bbc9-7f20dc379f34"
}
```

Dados | DESCRIÇÃO
-----|------------
nonce | Cadeia de caracteres opcional fornecida pelo usuário com a solicitação. Se nenhuma nonce tiver sido fornecida na solicitação, o carimbo de data/hora UTC atual será retornado
plan | O [Plano](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#plan) de uma VM em uma imagem do Azure Marketplace, contém nome, produto e editor
timestamp/createdOn | O carimbo de data/hora em que o primeiro documento assinado foi criado
timestamp/expiresOn | O carimbo de data/hora em que o documento assinado expira
vmId |  [Identificador exclusivo](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) para a máquina virtual

#### <a name="verifying-the-signature"></a>Verificar a assinatura

Depois de obter a assinatura acima, você pode verificar se a assinatura é da Microsoft. Também é possível verificar o certificado intermediário e a cadeia de certificados.

> [!NOTE]
> Os certificados para a nuvem Pública e a nuvem soberana serão diferentes.

 Regiões | Certificado
---------|-----------------
[Todas as regiões globais do Azure disponíveis](https://azure.microsoft.com/regions/)     | metadata.azure.com
[Azure Governamental](https://azure.microsoft.com/overview/clouds/government/)              | metadata.azure.us
[Azure China:](https://www.azure.cn/)                                                           | metadata.azure.cn
[Azure Alemanha](https://azure.microsoft.com/overview/clouds/germany/)                    | metadata.microsoftazure.de

```bash

# Verify the subject name for the main certificate
openssl x509 -noout -subject -in signer.pem
# Verify the issuer for the main certificate
openssl x509 -noout -issuer -in signer.pem
#Validate the subject name for intermediate certificate
openssl x509 -noout -subject -in intermediate.pem
# Verify the issuer for the intermediate certificate
openssl x509 -noout -issuer -in intermediate.pem
# Verify the certificate chain
openssl verify -verbose -CAfile /etc/ssl/certs/Baltimore_CyberTrust_Root.pem -untrusted intermediate.pem signer.pem
```

### <a name="failover-clustering-in-windows-server"></a>Clustering de failover do Windows Server

Para determinados cenários, ao consultar o Serviço de Metadados de Instância com clustering de failover, é necessário adicionar uma rota à tabela de roteamento.

1. Abra uma prompt de comando com privilégios de administrador.

2. Execute o seguinte comando e anote o endereço da Interface de rede de destino (`0.0.0.0`) na tabela de rotas do IPv4.

```bat
route print
```

> [!NOTE] 
> A seguinte saída de exemplo de uma VM do Windows Server com o cluster de failover habilitado contém apenas a tabela de rotas IPv4 para manter a simplicidade.

```bat
IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0         10.0.1.1        10.0.1.10    266
         10.0.1.0  255.255.255.192         On-link         10.0.1.10    266
        10.0.1.10  255.255.255.255         On-link         10.0.1.10    266
        10.0.1.15  255.255.255.255         On-link         10.0.1.10    266
        10.0.1.63  255.255.255.255         On-link         10.0.1.10    266
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      169.254.0.0      255.255.0.0         On-link     169.254.1.156    271
    169.254.1.156  255.255.255.255         On-link     169.254.1.156    271
  169.254.255.255  255.255.255.255         On-link     169.254.1.156    271
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link     169.254.1.156    271
        224.0.0.0        240.0.0.0         On-link         10.0.1.10    266
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     169.254.1.156    271
  255.255.255.255  255.255.255.255         On-link         10.0.1.10    266
```

1. Execute o seguinte comando e anote o endereço da Interface de rede de destino (`0.0.0.0`) que é o (`10.0.1.10`) neste exemplo.

```bat
route add 169.254.169.254/32 10.0.1.10 metric 1 -p
```

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-the-vm"></a>Exemplos de como chamar o serviço de metadados usando diferentes idiomas dentro da VM 

Linguagem | Exemplo
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb
Go  | https://github.com/Microsoft/azureimds/blob/master/imdssample.go
Python   | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
C++      | https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp
C#       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
JavaScript | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
PowerShell | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh
Perl       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.pl
Java       | https://github.com/Microsoft/azureimds/blob/master/imdssample.java
Visual Basic | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.vb
Puppet | https://github.com/keirans/azuremetadata

## <a name="faq"></a>Perguntas frequentes

1. Estou recebendo o erro `400 Bad Request, Required metadata header not specified`. O que isso significa?
   * O serviço de metadados de instância exige que o cabeçalho `Metadata: true` seja passado na solicitação. Passar o cabeçalho na chamada de REST permite acessar o serviço de metadados de instância.
2. Por que não estou obtendo informações de computação para minha máquina virtual?
   * Atualmente o serviço de metadados de instância suporta apenas instâncias criadas com o Gerenciador de recursos do Azure. No futuro, o suporte para VMs de serviço de nuvem pode ser adicionado.
3. Criei minha máquina virtual com o Azure Resource Manager há algum tempo. Por que não consigo ver as informações de metadados de computação?
   * Para todas as máquinas virtuais criadas depois de setembro de 2016, adicione uma [marca](../../azure-resource-manager/resource-group-using-tags.md) para começar a ver os metadados de computação. Para máquinas virtuais mais antigas (criadas antes de setembro de 2016), adicione ou remova extensões ou dados de discos à máquina virtual para atualizar os metadados.
4. Não estou vendo todos os dados preenchidos para a nova versão
   * Para todas as máquinas virtuais criadas depois de setembro de 2016, adicione uma [marca](../../azure-resource-manager/resource-group-using-tags.md) para começar a ver os metadados de computação. Para máquinas virtuais mais antigas (criadas antes de setembro de 2016), adicione ou remova extensões ou dados de discos à máquina virtual para atualizar os metadados.
5. Por que estou recebendo o erro `500 Internal Server Error`?
   * Repita a solicitação com base no sistema de retirada exponencial. Se o problema persistir, contate o suporte do Azure.
6. Onde posso publicar comentários/perguntas adicionais?
   * Envie seus comentários em https://feedback.azure.com.
7. Isso funcionaria para Instância do Conjunto de Dimensionamento da Máquina Virtual?
   * Sim, o serviço de metadados está disponível para instâncias de conjunto de escala.
8. Como posso obter suporte para o serviço?
   * Para obter suporte para o serviço, crie um problema de suporte no portal do Azure para a VM na qual você não consegue obter resposta de metadados após várias tentativas.
9. Eu recebo uma solicitação com tempo limite para minha chamada ao serviço?
   * Chamadas de metadados devem ser feitas do endereço IP principal atribuído à placa de rede da VM, além disso, caso você tenha alterado lá suas rotas devem ser uma rota para o endereço de 169.254.0.0/16 fora de sua placa de rede.
10. Atualizei minhas marcas no conjunto de dimensionamento de máquinas virtuais, mas elas não são exibidas nas instâncias, ao contrário das VMs.
    * Atualmente, para marcas de ScaleSets são exibidas apenas na VM em um reiníco/ao refazer imagem/ou uma alteração de disco na instância.

    ![Serviço de Metadados de Instância](./media/instance-metadata-service/InstanceMetadata-support.png)

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre [Eventos Agendados](scheduled-events.md)
