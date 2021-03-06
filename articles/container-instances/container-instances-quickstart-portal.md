---
title: Início Rápido – implantar um contêiner do Docker em Instâncias de Contêiner do Azure – portal
description: Neste início rápido, você usa o portal do Azure para implantar rapidamente um aplicativo Web em contêineres que é executado em uma instância de contêiner do Azure isolada
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: f4d232d4d6043ede3979db67e5cd35130d931bef
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369438"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-portal"></a>Início Rápido: Implantar uma instância de contêiner no Azure usando o portal do Azure

Use as Instâncias de Contêiner do Azure para executar contêineres do Docker sem servidor no Azure de maneira simples e rápida. Implante um aplicativo em uma instância de contêiner sob demanda quando você não precisa de uma plataforma de orquestração de contêiner completa como o Serviço de Kubernetes do Azure.

Neste início rápido, você usará o portal do Azure para implantar um contêiner do Docker isolado e disponibilizar o respectivo aplicativo com um FQDN (nome de domínio totalmente qualificado). Após definir algumas configurações e implantar o contêiner, você poderá navegar até o aplicativo em execução:

![Aplicativos implantados nas Instâncias de Contêiner do Azure exibidos no navegador][aci-portal-07]

## <a name="sign-in-to-azure"></a>Entrar no Azure

Entre no Portal do Azure em https://portal.azure.com.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita][azure-free-account] antes de começar.

## <a name="create-a-container-instance"></a>Criar uma instância de contêiner

Selecione **Criar um recurso** > **Contêineres** > **Instâncias de Contêiner**.

![Começar a criar uma nova instância de contêiner no Portal do Azure][aci-portal-01]

Insira os seguintes valores nas caixas de texto **Nome do contêiner**, **Imagem de contêiner** e **Grupo de recursos**. Deixe os outros valores com seus padrões e selecione **OK**.

* Nome do contêiner: `mycontainer`
* Imagem de contêiner: `mcr.microsoft.com/azuredocs/aci-helloworld`
* Grupo de recursos: **Criar novo** > `myResourceGroup`

![Configurando as definições básicas para uma nova instância de contêiner no Portal do Azure][aci-portal-03]

Para este início rápido, deixe a configuração padrão **Público** para implantar a imagem `aci-helloworld` da Microsoft pública. Esta imagem empacota um pequeno aplicativo Web escrito no Node.js que veicula a uma página HTML estática.

Em **Configuração**, especifique um **rótulo do nome DNS** para seu contêiner. O nome precisa ser exclusivo dentro da região do Azure em que você cria a instância de contêiner. Seu contêiner estará publicamente acessível em `<dns-name-label>.<region>.azurecontainer.io`. Se você receber uma mensagem de erro “Rótulo de nome DNS não disponível”, tente usar um rótulo de nome DNS diferente.

Deixe as outras configurações em **Configuração** com seus padrões e selecione **OK** para validar a configuração.

![Configurando uma nova instância de contêiner no Portal do Azure][aci-portal-04]

Quando a validação for concluída, um resumo das configurações de seu contêiner será exibido. Selecione **OK** para enviar sua solicitação de implantação de contêiner.

![Resumo das configurações para uma nova instância de contêiner no Portal do Azure][aci-portal-05]

Quando a implantação é iniciada, aparece uma notificação indicando que a implantação está em andamento. Outra notificação será exibida quando o grupo de contêiner tiver sido implantado.

![Progresso da criação de uma nova instância de contêiner no Portal do Azure][aci-portal-08]

Abra a visão geral do grupo de contêiner navegando até **Grupos de Recursos** > **myResourceGroup** > **mycontainer**. Anote o **FQDN** (nome de domínio totalmente qualificado) da instância do contêiner, bem como seu **Status**.

![Visão geral do grupo de contêineres no Portal do Azure][aci-portal-06]

Uma vez seu **Status** é *Executando*, navegue para o FQDN do contêiner em seu navegador.

![Os aplicativos implantados usando Instâncias de Contêiner do Azure são exibidos no navegador][aci-portal-07]

Parabéns! Ao configuras apenas algumas configurações, você implantou um aplicativo publicamente acessível em Instâncias de Contêiner do Azure.

## <a name="view-container-logs"></a>Exibir logs do contêiner

Exibir os logs para uma instância de contêiner é útil ao solucionar problemas com o contêiner ou o aplicativo que é executado.

Para exibir os logs do contêiner, em **Configurações**, selecione **Contêineres** e, em seguida, **Logs**. Você verá a solicitação HTTP GET gerada quando você exibiu o aplicativo em seu navegador.

![Logs de contêiner no portal do Azure][aci-portal-11]

## <a name="clean-up-resources"></a>Limpar recursos

Quando tiver terminado com o contêiner, selecione **Visão geral** para a instância de contêiner *mycontainer*, em seguida, selecione **Excluir**.

![Excluir a instância de contêiner no Portal do Azure][aci-portal-09]

Escolha **Sim** quando a caixa de diálogo de confirmação aparecer.

![Confirmação de exclusão de uma instância de contêiner no Portal do Azure][aci-portal-10]

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você criou uma instância de contêiner do Azure com base em uma imagem da Microsoft pública. Se você quiser criar uma imagem de contêiner e implantá-la usando um Registro de Contêiner do Azure privado, prossiga para o tutorial das Instâncias de Contêiner do Azure.

> [!div class="nextstepaction"]
> [Tutorial sobre Instâncias de Contêiner do Azure](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png
[aci-portal-11]: ./media/container-instances-quickstart-portal/qs-portal-11.png

<!-- LINKS - External -->
[azure-free-account]: https://azure.microsoft.com/free/