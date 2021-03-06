---
title: Autenticação baseada em cabeçalho com o PingAccess para Proxy de Aplicativo do Azure AD | Microsoft Docs
description: Publique aplicativos com o PingAccess e o Proxy de Aplicativo a fim de oferecer suporte à autenticação baseada em cabeçalho.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/11/2017
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8eef15098eed8959655ae2904bf41a8c3dffc9f4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58882777"
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Autenticação baseada em cabeçalho para logon único com Proxy de Aplicativo e PingAccess

O Proxy de Aplicativo do Azure Active Directory e o PingAccess fizeram uma parceria para fornecer aos clientes do Azure Active Directory acesso a ainda mais aplicativos. O PingAccess expande as [ofertas existentes do Proxy de Aplicativo](application-proxy.md) para incluir acesso de logon único a aplicativos que usam cabeçalhos para autenticação.

## <a name="what-is-pingaccess-for-azure-ad"></a>O que é PingAccess para Azure AD?

PingAccess para Azure Active Directory é uma oferta de PingAccess que permite que você conceda acesso de usuários e logon único para aplicativos que usam cabeçalhos de autenticação. O Proxy de Aplicativo trata esses aplicativos como qualquer outro, usando o Azure AD para autenticar o acesso e, depois, passando o tráfego por meio do serviço de conector. O PingAccess fica na frente dos aplicativos e converte o token de acesso do Azure AD em um cabeçalho, para que o aplicativo receba a autenticação em um formato que consiga ler.

Os usuários não perceberão algo diferente quando entrarem para usar seus aplicativos corporativos. Eles ainda podem trabalhar de qualquer lugar e em qualquer dispositivo. 

Como os conectores do Proxy de Aplicativo direcionam o tráfego remoto a todos os aplicativos, independentemente do tipo de autenticação, eles também continuarão a balancear a carga automaticamente.

## <a name="how-do-i-get-access"></a>Como posso obter acesso?

Como esse cenário é oferecido por meio de uma parceria entre o Azure Active Directory e o PingAccess, você precisa de licenças para os dois serviços. No entanto, as assinaturas do Azure Active Directory Premium incluem uma licença básica PingAccess que abrange até 20 aplicativos. Se você precisar publicar mais de 20 aplicativos com base em cabeçalho, poderá adquirir uma licença adicional do PingAccess. 

Para obter mais informações, consulte [Edições do Active Directory do Azure](../fundamentals/active-directory-whatis.md).

## <a name="publish-your-application-in-azure"></a>Publicar seu aplicativo no Azure

Este artigo destina-se a pessoas que estão publicando um aplicativo com esse cenário pela primeira vez. Ele explica como começar com o Aplicativo e o PingAccess, além das etapas de publicação. Se você já tiver configurado os dois serviços, mas desejar uma atualização sobre as etapas de publicação, poderá ignorar a instalação do conector e ir para [Adicionar seu aplicativo ao Azure AD com o Proxy de Aplicativo](#add-your-app-to-azure-ad-with-application-proxy).

>[!NOTE]
>Como esse cenário é uma parceria entre o Azure AD e o PingAccess, algumas das instruções estão no site do Ping Identity.

### <a name="install-an-application-proxy-connector"></a>Instalar um conector do Proxy de Aplicativo

Se você já tiver o Proxy de Aplicativo habilitado, além de um conector instalado, poderá ignorar essa seção e ir para [Adicionar seu aplicativo ao Azure AD com o Proxy de Aplicativo](#add-your-app-to-azure-ad-with-application-proxy).

O conector do Proxy de Aplicativo é um serviço do Windows Server que direciona o tráfego de seus funcionários remotos para seus aplicativos publicados. Para obter informações mais detalhadas, confira [Habilitar o Proxy de Aplicativo no Portal do Azure](application-proxy-add-on-premises-application.md).

1. Entrar para o [portal do Azure](https://portal.azure.com) como um administrador do aplicativo.
2. Selecione **Azure Active Directory** > **Proxy de aplicativo** .
3. Selecione **Baixar Conector** para iniciar o download do conector do Proxy de Aplicativo. Siga as instruções de instalação.

   ![Habilitar o Proxy de Aplicativo e baixar o conector](./media/application-proxy-configure-single-sign-on-with-ping-access/install-connector.png)

4. O download do conector deve habilitar automaticamente o Proxy de Aplicativo para seu diretório, mas se isso não acontecer, selecione **Habilitar Proxy de Aplicativo**.


### <a name="add-your-app-to-azure-ad-with-application-proxy"></a>Adicionar o aplicativo ao Azure AD com o Proxy de Aplicativo

Há duas ações necessárias no portal do Azure. Primeiro, você precisa publicar seu aplicativo com o Proxy de Aplicativo. Depois, colete algumas informações sobre o aplicativo a ser usado durante as etapas do PingAccess.

Siga estas etapas para publicar seu aplicativo. Para um passo a passo mais detalhado das etapas 1 a 8, veja [Publicar aplicativos usando o Proxy de Aplicativo do Azure AD](application-proxy-add-on-premises-application.md).

1. Se você não fez isso na última seção, entrar para o [portal do Azure](https://portal.azure.com) como um administrador do aplicativo.
2. Selecione **Azure Active Directory** > **Aplicativos empresariais**.
3. Selecione **Adicionar** na parte superior da folha.
4. Selecione **Aplicativo local**.
5. Preencha os campos obrigatórios com informações sobre seu novo aplicativo. Use as diretrizes a seguir para as configurações:
   - **URL interna**: normalmente, você fornece a URL que levará você até a página de entrada do aplicativo quando você estiver na rede corporativa. Para esse cenário, o conector precisa tratar o proxy do PingAccess como a página inicial do aplicativo. Use este formato: `https://<host name of your PA server>:<port>`. A porta é a 3000 por padrão, mas você pode configurá-la no PingAccess.

     > [!WARNING]
     > Para este tipo de SSO, a URL interna deve usar https e não pode usar o http.

   - **Método de pré-autenticação**: Azure Active Directory
   - **Traduzir URL nos cabeçalhos**: Não 

   >[!NOTE]
   >Se este for seu primeiro aplicativo, use a porta 3000 para iniciar e retorne para atualizar essa configuração se alterar a configuração de PingAccess. Se esse for o seu segundo aplicativo ou posterior, isso será necessário para combinar o Ouvinte configurado no PingAccess. Saiba mais sobre [ouvintes no PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).

6. Selecione **Adicionar** na parte inferior da folha. Seu aplicativo é adicionado e abre o menu de início rápido.
7. No menu de início rápido, selecione **Atribuir um usuário para teste** e adicione pelo menos um usuário para o aplicativo. Verifique se essa conta de teste tem acesso ao aplicativo local.
8. Selecione **Atribuir** para salvar a atribuição do usuário de teste.
9. Na folha de gerenciamento do aplicativo, selecione **Logon único**.
10. Escolha **Logon baseado em cabeçalho** no menu suspenso. Clique em **Salvar**.

    >[!TIP]
    >Se esta for a primeira vez que você usa o logon único com base em cabeçalhos, será necessário instalar o PingAccess. Para certificar-se de que sua assinatura do Azure seja automaticamente associada à sua instalação do PingAccess, use o link nesta página de logon único para baixar o PingAccess. Você pode abrir o site de download agora ou voltar a esta página mais tarde. 

    ![Selecione o logon com base no cabeçalho](./media/application-proxy-configure-single-sign-on-with-ping-access/sso-header.PNG)

11. Feche a folha Aplicativos empresariais ou role a tela totalmente para a esquerda para retornar ao menu do Azure Active Directory.
12. Selecione **Registros do Aplicativo**.

    ![Selecionar Registros de aplicativo](./media/application-proxy-configure-single-sign-on-with-ping-access/app-registrations.png)

13. Selecione o aplicativo que você acabou de adicionar e, depois, **URLs de Resposta**.

    ![Selecionar URLs de Resposta](./media/application-proxy-configure-single-sign-on-with-ping-access/reply-urls.png)

14. Verifique se a URL externa que você atribuiu ao seu aplicativo na etapa 5 está na lista de URLs de Resposta. Se não estiver, adicione-a agora.
15. Na folha de configurações do aplicativo, selecione **Permissões necessárias**.

    ![Selecionar Permissões necessárias](./media/application-proxy-configure-single-sign-on-with-ping-access/required-permissions.png)

16. Selecione **Adicionar**. Para a API, escolha **Windows Azure Active Directory**, depois, **Selecionar**. Para as permissões, escolha **Leitura e gravação em todos os aplicativos** e **Entrar e ler o perfil do usuário**, depois **Selecionar** e **Concluído**.  

    ![Selecionar permissões](./media/application-proxy-configure-single-sign-on-with-ping-access/select-permissions.png)

17. Conceda permissões antes de fechar a tela de permissões. 
    ![Conceder Permissões](./media/application-proxy-configure-single-sign-on-with-ping-access/grantperms.png)

### <a name="collect-information-for-the-pingaccess-steps"></a>Coletar informações sobre as etapas do PingAccess

1. Na folha de configurações de seu aplicativo, selecione **Propriedades**. 

   ![Selecionar Propriedades](./media/application-proxy-configure-single-sign-on-with-ping-access/properties.png)

2. Salve o valor **ID do Aplicativo**. Isso é usado para a ID do cliente quando você configura o PingAccess.
3. Na folha de configurações, selecione **Chaves**.

   ![Selecionar Chaves](./media/application-proxy-configure-single-sign-on-with-ping-access/Keys.png)

4. Crie uma chave inserindo uma descrição de chave e escolhendo uma data de validade no menu suspenso.
5. Clique em **Salvar**. Um GUID será exibido no campo **Valor**.

   Salve esse valor agora, pois você não poderá vê-lo novamente depois de fechar esta janela.

   ![Criar uma nova chave](./media/application-proxy-configure-single-sign-on-with-ping-access/create-keys.png)

6. Feche a folha Registros de aplicativo empresariais ou role a tela totalmente para a esquerda para retornar ao menu do Azure Active Directory.
7. Selecione **Propriedades**.
8. Salve o GUID em **ID de Diretório**.

### <a name="optional---update-graphapi-to-send-custom-fields"></a>Opcional - Atualizar a API do Graph para enviar campos personalizados

Para obter uma lista de tokens de segurança que o Azure AD envia para autenticação, confira [Referência de token do Azure AD](../develop/v1-id-and-access-tokens.md). Se você precisar de uma declaração personalizada que envia outros tokens, use Explorador do Graph ou o manifesto do aplicativo no Portal do Azure para definir o campo *acceptMappedClaims* do aplicativo como **True**.    

Este exemplo usa o Explorador do Graph:

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```
Este exemplo usa o [portal do Azure](https://portal.azure.com) para atualizar o campo *acceptedMappedClaims*:
1. Entrar para o [portal do Azure](https://portal.azure.com) como um administrador do aplicativo.
2. Selecione **Azure Active Directory** > **Registros de aplicativo**.
3. Selecione seu aplicativo > **Manifesto**.
4. Selecione **Editar**, procure o campo *acceptedMappedClaims* e altere o valor para **true**.
![Manifesto de aplicativo](./media/application-proxy-configure-single-sign-on-with-ping-access/application-proxy-ping-access-manifest.PNG)
1. Clique em **Salvar**.

>[!NOTE]
>Para usar uma declaração personalizada, você também deve ter uma política personalizada definida e atribuída ao aplicativo.  Essa política deve incluir todos os atributos personalizados necessários.
>
>Atribuição e definição de política podem ser feitas por meio do PowerShell, do Azure AD Graph Explorer ou do MS Graph.  Se você estiver fazendo isso no PowerShell, você talvez precise usar pela primeira vez `New-AzureADPolicy` e, em seguida, atribuí-lo para o aplicativo com `Set-AzureADServicePrincipalPolicy`.  Para obter mais informações, consulte a [Documentação da política do Azure AD](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment).

### <a name="optional---use-a-custom-claim"></a>Opcional – use uma declaração personalizada
Para fazer seu aplicativo usar uma declaração personalizada e incluir campos adicionais, crie também uma [política de mapeamento de declarações personalizadas e atribuída ao aplicativo](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment).

## <a name="download-pingaccess-and-configure-your-app"></a>Baixar o PingAccess e configurar seu aplicativo

Agora que você concluiu todas as etapas de instalação do Azure Active Directory, será possível prosseguir para a configuração do PingAccess. 

As etapas detalhadas para a parte do PingAccess deste cenário continuam na documentação de Identidade de Ping, [Configurar o PingAccess para o Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).

Essas etapas orientarão você durante o processo de obtenção de uma conta do PingAccess, se você ainda não tiver uma, instalação do servidor do PingAccess e criação de uma conexão do Provedor do Azure AD OIDC com a ID de Diretório que você copiou do Portal do Azure. Depois, use os valores de ID do Aplicativo e Chave para criar uma Sessão da Web no PingAccess. Em seguida, configure o mapeamento de identidade e crie um host virtual, site e aplicativo.

### <a name="test-your-app"></a>Testar seu aplicativo

Depois de concluir todas essas etapas, seu aplicativo estará pronto para execução. Para testá-lo, abra um navegador e navegue até a URL externa que você criou quando publicou o aplicativo no Azure. Entre com a conta de teste que você atribuiu ao aplicativo.

## <a name="next-steps"></a>Próximas etapas

- [Configurar o PingAccess para Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [Como o Proxy de Aplicativo do Azure AD fornece logon único?](application-proxy-single-sign-on.md)
- [Solucionar problemas de Proxy de Aplicativo](application-proxy-troubleshoot.md)
