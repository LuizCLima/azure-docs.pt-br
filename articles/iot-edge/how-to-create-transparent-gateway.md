---
title: Criar um dispositivo de gateway transparente - Azure IoT Edge | Microsoft Docs
description: Usar um dispositivo Azure IoT Edge como gateway transparente que possa processar informações de dispositivos downstream
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/29/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 9d67a87b182758e37c9e379a8f96a6540797ce3e
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58482939"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Configurar um dispositivo IoT Edge para agir como gateway transparente

Este artigo fornece instruções detalhadas para configurar dispositivos IoT Edge para funcionar como gateway transparente para que outros dispositivos se comuniquem com o Hub IoT. Neste artigo, o termo *gateway do IoT Edge* se refere a um dispositivo de IoT Edge usado como um gateway transparente. Para obter informações mais detalhadas, consulte [Como um dispositivo IoT Edge pode ser usado como um gateway](./iot-edge-as-gateway.md), o que fornece uma visão geral conceitual.

>[!NOTE]
>No momento:
> * Se o gateway está desconectado do Hub IoT, os dispositivos downstream não podem se autenticar no gateway.
> * Os dispositivos habilitados para o Edge não podem se conectar com gateways do IoT Edge. 
> * Dispositivos downstream não podem usar o upload de arquivo.

Para um dispositivo funcionar como gateway, ele precisa ser capaz de se conectar com segurança aos dispositivos downstream. Azure IoT Edge permite que você use a infraestrutura de chave pública (PKI) para configurar conexões seguras entre dispositivos. Neste caso, nós estamos permitindo um dispositivo downstream conectar-se a um dispositivo IoT Edge atuando como um gateway transparente. Para manter a segurança razoável, o dispositivo downstream deve confirmar a identidade do dispositivo Edge como você deseja apenas os dispositivos que se conectar a seus gateways e não um gateway potencialmente mal-intencionados.

Um dispositivo downstream pode ser qualquer aplicativo ou plataforma que tenha uma identidade criada com o serviço de nuvem do [Hub IoT do Azure](https://docs.microsoft.com/azure/iot-hub). Em muitos casos, esses aplicativos usam o [SDK do dispositivo IoT do Azure](../iot-hub/iot-hub-devguide-sdks.md). Para todos os fins práticos, um dispositivo downstream pode até ser um aplicativo em execução no próprio dispositivo de gateway IoT Edge. 

Você pode criar qualquer infraestrutura de certificado que permite a relação de confiança necessária para sua topologia de dispositivo/gateway. Neste artigo, supomos a mesma configuração de certificado que você usaria para habilitar a [segurança X.509 da CA](../iot-hub/iot-hub-x509ca-overview.md) no Hub IoT, que envolve um certificado CA X.509 associado a um hub IoT específico (o proprietário do hub IoT CA) e uma série de certificados assinados com essa CA e uma CA para o dispositivo Azure Data Box Edge.

![Configuração do certificado de gateway](./media/how-to-create-transparent-gateway/gateway-setup.png)

O gateway apresenta o seu certificado AC de dispositivo Edge para o dispositivo downstream durante a iniciação da conexão. O dispositivo downstream verifica se que o certificado de AC de dispositivo Edge é assinado pelo certificado de autoridade de certificação de proprietário. Esse processo permite que o dispositivo downstream confirmar que o gateway vem de uma fonte confiável.

As etapas a seguir o orientará no processo de criação de certificados e instalá-los nos lugares certos.

## <a name="prerequisites"></a>Pré-requisitos

Um dispositivo do IoT Edge para configurar como gateway. Você pode usar seu computador de desenvolvimento ou uma máquina virtual como dispositivo do IoT Edge seguindo as etapas para os seguintes sistemas operacionais:
* [Windows](./how-to-install-iot-edge-windows.md)
* [Linux x64](./how-to-install-iot-edge-linux.md)
* [Linux ARM32](./how-to-install-iot-edge-linux-arm.md)

Você pode usar qualquer computador para gerar os certificados e, em seguida, copiá-los para seu dispositivo IoT Edge.

>[!NOTE]
>O "nome do gateway" usado para criar os certificados nessa instrução, precisa ser o mesmo nome usado como nome de host em seu arquivo config. YAML de IoT Edge e como GatewayHostName na cadeia de conexão do dispositivo downstream. O "nome do gateway" precisa ser resolvido para um endereço IP, usando DNS ou uma entrada do arquivo host. A comunicação baseada no protocolo usado (MQTTS:8883 / AMQPS:5671 / HTTPS:433) deve ser possível entre o dispositivo downstream e o transparant do IoT Edge. Se um firewall estiver entre os dois, a respectiva porta precisa ser aberta.

## <a name="generate-certificates-with-windows"></a>Gerar certificados com o Windows

Use as etapas nesta seção para gerar certificados de teste em um dispositivo do Windows. Você pode gerar os certificados no próprio dispositivo IoT Edge ou usar um computador separado e copiar os certificados finais para qualquer dispositivo IoT Edge executando qualquer sistema operacional compatível. 

Os certificados gerados nesta seção são somente para fins de teste. 

### <a name="install-openssl"></a>Instalar o OpenSSL

Instale o OpenSSL para Windows no computador que você está usando para gerar os certificados. Há muitas maneiras de instalar o OpenSSL:

   >[!NOTE]
   >Se já tiver o OpenSSL instalado em seu dispositivo Windows, você pode ignorar esta etapa, mas verifique se openssl.exe está disponível na variável de ambiente PATH.

* **Mais fácil:** Faça o download e instale todos os [binários do OpenSSL de terceiros](https://wiki.openssl.org/index.php/Binaries), por exemplo, [este projeto no SourceForge](https://sourceforge.net/projects/openssl/). Adicione o caminho completo para openssl.exe à variável de ambiente PATH. 
   
* **Recomendados:** Faça o download do código-fonte do OpenSSL e construa os binários em sua máquina sozinho ou por meio do [vcpkg](https://github.com/Microsoft/vcpkg). As instruções a seguir usam vcpkg para baixar o código-fonte, compilação e instalar o OpenSSL em seu computador Windows com etapas simples.

   1. Navegue para um diretório onde você deseja instalar vcpkg. Chamaremos esse diretório de *\<VCPKGDIR>*. Siga as instruções para fazer o download e instalar o [vcpkg](https://github.com/Microsoft/vcpkg).
   
   2. Quando o vcpkg estiver instalado, em um prompt do powershell, execute o seguinte comando para instalar o pacote OpenSSL para Windows x64. Normalmente, a instalação leva cerca de 5 minutos para concluir.

      ```powershell
      .\vcpkg install openssl:x64-windows
      ```
   3. Adicione `<VCPKGDIR>\installed\x64-windows\tools\openssl` à variável de ambiente PATH para que o arquivo openssl.exe fique disponível para invocação.

### <a name="prepare-creation-scripts"></a>Preparar scripts de criação

O SDK do dispositivo IoT do Azure para C contém scripts que você pode usar para gerar certificados de teste. Nesta seção, você pode clonar o SDK e configurar o PowerShell.

1. Abra uma janela do PowerShell no modo de administrador. 

2. Clone o repositório git que contém scripts para gerar certificados de não produção. Esses scripts ajudam você a criar os certificados necessários para configurar um gateway transparente. Use o comando `git clone` ou [faça o download do ZIP](https://github.com/Azure/azure-iot-sdk-c/archive/master.zip). 

   ```powershell
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

3. Navegue até o diretório no qual você deseja trabalhar. Chamaremos esse diretório de *\<WRKDIR>*.  Todos os arquivos serão criados neste diretório.

4. Copie os arquivos de configuração e script em seu diretório de trabalho. 

   ```powershell
   copy <path>\azure-iot-sdk-c\tools\CACertificates\*.cnf .
   copy <path>\azure-iot-sdk-c\tools\CACertificates\ca-certs.ps1 .
   ```

   Se você fez o download do SDK como ZIP, o nome da pasta será `azure-iot-sdk-c-master` e o restante do caminho será o mesmo. 

5. Configure a variável de ambiente OPENSSL_CONF para usar o arquivo de configuração openssl_root_ca.cnf.

    ```powershell
    $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
    ```

6. Habilite o PowerShell para executar os scripts.

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

7. Coloque as funções, usadas por scripts, no namespace global do PowerShell.
   
   ```powershell
   . .\ca-certs.ps1
   ```

8. Verifique se o OpenSSL foi instalado corretamente e certifique-se de não haver colisões de nome com certificados existentes. Se houver problemas, o script deve descrever como corrigi-los em seu sistema.

   ```powershell
   Test-CACertsPrerequisites
   ```

### <a name="create-certificates"></a>Criar certificados

Nesta seção, você criará três certificados, conectando-os, em seguida, em uma cadeia. A colocação dos certificados em um arquivo de cadeia permite que você instale-os facilmente no seu dispositivo de gateway do IoT Edge e qualquer o dispositivo downstream.  

1. Crie o Certificado de Autoridade de Certificação do proprietário e faça com que assine um certificado intermediário. Todos os certificados são colocados em *\<WRKDIR>*.

      ```powershell
      New-CACertsCertChain rsa
      ```

2. Crie uma chave privada e o Certificado de Autoridade de Certificação de dispositivo Edge com o comando a seguir. Dê um nome para o dispositivo de gateway, que será usado para nomear os arquivos e durante a geração do certificado. 

   ```powershell
   New-CACertsEdgeDevice "<gateway name>"
   ```

3. Crie uma cadeia de Certificado de Autoridade de Certificação de certificação do proprietário, o certificado intermediário e Certificado de Autoridade de Certificação de dispositivo Edge com o comando a seguir. 

   ```powershell
   Write-CACertsCertificatesForEdgeDevice "<gateway name>"
   ```

   O script cria os seguintes certificados e chaves:
   * `<WRKDIR>\certs\new-edge-device.*`
   * `<WRKDIR>\private\new-edge-device.key.pem`
   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

## <a name="generate-certificates-with-linux"></a>Gerar certificados com o Linux

Use as etapas nesta seção para gerar certificados de teste em um dispositivo do Linux. Você pode gerar os certificados no próprio dispositivo IoT Edge ou usar um computador separado e copiar os certificados finais para qualquer dispositivo IoT Edge executando qualquer sistema operacional compatível. 

### <a name="prepare-creation-scripts"></a>Preparar scripts de criação

1. Clone o repositório git que contém scripts para gerar certificados de não produção. Esses scripts ajudam você a criar os certificados necessários para configurar um gateway transparente. 

   ```bash
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

2. Navegue até o diretório no qual você deseja trabalhar. Chamaremos esse diretório de *\<WRKDIR>*.  Todos os arquivos serão criados neste diretório.
  
3. Copie os arquivos de script e de configuração em seu diretório de trabalho.

   ```bash
   cp <path>/azure-iot-sdk-c/tools/CACertificates/*.cnf .
   cp <path>/azure-iot-sdk-c/tools/CACertificates/certGen.sh .
   ```

4. Configure o OpenSSL para gerar certificados usando o script fornecido. 

   ```bash
   chmod 700 certGen.sh 
   ```

### <a name="create-certificates"></a>Criar certificados

Nesta seção, você criará três certificados, conectando-os, em seguida, em uma cadeia. A colocação dos certificados em um arquivo de cadeia permite que você instale-os facilmente no seu dispositivo de gateway do IoT Edge e qualquer o dispositivo downstream.  

1. Crie o certificado de AC do proprietário e um certificado intermediário. Esses certificados são colocados em *\<WRKDIR>*.

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   O script cria os seguintes certificados e chaves:
   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>/certs/azure-iot-test-only.intermediate.cert.pem`
   * `<WRKDIR>/private/azure-iot-test-only.root.ca.key.pem`
   * `<WRKDIR>/private/azure-iot-test-only.intermediate.key.pem`

2. Crie uma chave privada e o Certificado de Autoridade de Certificação de dispositivo Edge com o comando a seguir. Dê um nome para o dispositivo de gateway, que será usado para nomear os arquivos e durante a geração do certificado. 

   ```bash
   ./certGen.sh create_edge_device_certificate "<gateway name>"
   ```

   O script cria os seguintes certificados e chaves:
   * `<WRKDIR>/certs/new-edge-device.*`
   * `<WRKDIR>/private/new-edge-device.key.pem`

3. Crie uma cadeia de certificados chamada **new-edge-device-full-chain.cert.pem** do certificado de Autoridade de Certificação do proprietário, o certificado intermediário e certificado de Autoridade de Certificação do dispositivo Edge.

   ```bash
   cat ./certs/new-edge-device.cert.pem ./certs/azure-iot-test-only.intermediate.cert.pem ./certs/azure-iot-test-only.root.ca.cert.pem > ./certs/new-edge-device-full-chain.cert.pem
   ```

## <a name="install-certificates-on-the-gateway"></a>Instalar certificados no gateway

Agora que você fez uma cadeia de certificados, precisa instalá-la no dispositivo de gateway do IoT Edge e configurar o tempo de execução do IoT Edge para referenciar os novos certificados. 

1. Copie os seguintes arquivos de *\<WRKDIR >*. Salve-os em qualquer lugar no seu dispositivo IoT Edge. Vamos nos referir ao diretório de destino em seu dispositivo IoT Edge como *\<CERTDIR>*. 

   Se você gerou os certificados no próprio dispositivo Edge, você pode ignorar esta etapa e usar o caminho para o diretório de trabalho.

   * Certificado de AC de dispositivo - `<WRKDIR>\certs\new-edge-device-full-chain.cert.pem`
   * Chave privada de AC do dispositivo - `<WRKDIR>\private\new-edge-device.key.pem`
   * AC do proprietário - `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

2. Abra o artigo de configuração do daemon de segurança do IoT Edge. 

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

3. Defina as propriedades do **certificado** no arquivo config.yaml para o caminho onde você colocou os arquivos de certificado e de chave no dispositivo IoT Edge.

```yaml
certificates:
  device_ca_cert: "<CERTDIR>\\certs\\new-edge-device-full-chain.cert.pem"
  device_ca_pk: "<CERTDIR>\\private\\new-edge-device.key.pem"
  trusted_ca_certs: "<CERTDIR>\\certs\\azure-iot-test-only.root.ca.cert.pem"
```

## <a name="deploy-edgehub-to-the-gateway"></a>Implantar EdgeHub para o gateway

Ao instalar o IoT Edge em um dispositivo, apenas um módulo de sistema é iniciado automaticamente: o agente do Edge. Para que seu dispositivo funcione como gateway, você precisa de ambos os módulos do sistema. Se você não tiver implantado nenhum módulo para seu dispositivo de gateway antes, crie uma implantação do seu dispositivo para iniciar o segundo módulo de sistema, o hub do Edge. A implantação ficará vazia porque você não adicionou nenhum módulo no assistente, mas ambos os módulos do sistema são implantados. 

Você pode verificar quais módulos estão em execução em um dispositivo com o comando `iotedge list`.

1. No portal do Azure, navegue para o hub IoT.

2. Vá para **IoT Edge** e selecione o dispositivo Edge IoT que você deseja usar como um gateway.

3. Selecione **Definir Módulos**.

4. Selecione **Avançar**.

5. Na página **Especificar rotas**, você deve ter uma rota padrão que envie todas as mensagens de todos os módulos para o Hub IoT. Caso contrário, adicione o código a seguir e selecione **Avançar**.

   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```

6. Na página **Revisar modelo**, selecione **Enviar**.

## <a name="route-messages-from-downstream-devices"></a>Rotear de mensagens de dispositivos downstream
O tempo de execução de IoT Edge pode rotear as mensagens enviadas dos dispositivos downstream assim como as mensagens enviadas pelos módulos. Isso permite que você execute uma análise em um módulo em execução no gateway antes de enviar dados para a nuvem. 

Atualmente, a maneira como você roteia as mensagens enviadas pelos dispositivos downstream é diferenciando-as de mensagens enviadas pelos módulos. Todas as mensagens enviadas por módulos contêm uma propriedade de sistema chamada **connectionModuleId**, mas as mensagens enviadas pelos dispositivos downstream não. Você pode usar a cláusula WHERE da rota para excluir todas as mensagens que contêm essa propriedade do sistema. 

A rota abaixo seria usada para enviar mensagens de um dispositivo downstream para um nome de módulo `ai_insights`.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", 
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" 
    } 
}
```

Para saber mais sobre o roteamento de mensagens, consulte [Implantar módulos e estabelecer rotas](./module-composition.md#declare-routes).

[!INCLUDE [iot-edge-extended-ofline-preview](../../includes/iot-edge-extended-offline-preview.md)]

## <a name="next-steps"></a>Próximas etapas

Agora que você tem um dispositivo IoT Edge funcionando como gateway transparente, você precisará configurar seus dispositivos downstream para confiar no gateway e enviar mensagens para ele. Para obter mais informações, consulte [Cnectar um dispositivo downstream a um gateway do Azure IoT Edge](how-to-connect-downstream-device.md).
