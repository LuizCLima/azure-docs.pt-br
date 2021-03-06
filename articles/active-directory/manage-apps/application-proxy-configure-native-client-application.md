---
title: Publicar aplicativos cliente nativos - Azure AD | Microsoft Docs
description: Aborda como habilitar aplicativos clientes nativos para se comunicar com o Conector de Proxy do aplicativo Azure AD para fornecer acesso remoto seguro aos seus aplicativos locais.
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
ms.date: 11/08/2018
ms.author: celested
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 21e101dee878a48cce1005d51ad5e59166b0cfa1
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56193111"
---
# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Como habilitar aplicativos clientes nativos para interagir com aplicativos de proxy

Além dos aplicativos Web, o Proxy de Aplicativo do Azure Active Directory também pode ser usado para publicar aplicativos cliente nativos configurados com a ADAL (Biblioteca de Autenticação do Azure AD). Os aplicativos cliente nativos diferem dos aplicativos Web porque eles são instalados em um dispositivo, enquanto os aplicativos Web são acessados por meio de um navegador. 

O Proxy de Aplicativo dá suporte a aplicativos cliente nativos aceitando os tokens emitidos pelo Azure AD enviados no cabeçalho. O serviço de proxy de aplicativo executa a autenticação em nome dos usuários. Essa solução não usa tokens de aplicativo para autenticação. 

![Relação entre os usuários finais, o Active Directory do Azure e os aplicativos publicados](./media/application-proxy-configure-native-client-application/richclientflow.png)

Use a Biblioteca de Autenticação do Azure AD, que trata da autenticação e dá suporte a vários ambientes de cliente, para publicar aplicativos nativos. O Proxy de Aplicativo se encaixa no [cenário de Aplicativo Nativo para API Web](../develop/native-app.md). 

Este artigo explica as quatro etapas para publicar um aplicativo nativo com o Proxy de Aplicativo e a Biblioteca de Autenticação do Azure AD. 

## <a name="step-1-publish-your-application"></a>Etapa 1: Publicar seu aplicativo
Publique seu aplicativo de proxy como faria com qualquer outro aplicativo e atribua aos usuários acesso a ele. Para saber mais, consulte [Publicar aplicativos com o Proxy de Aplicativo](application-proxy-add-on-premises-application.md).

## <a name="step-2-configure-your-application"></a>Etapa 2: Configurar seu aplicativo
Configure seu aplicativo nativo da seguinte maneira:

1. Entre no [Portal do Azure](https://portal.azure.com).
2. Navegue até **Azure Active Directory** > **Registros de aplicativo**.
3. Selecione **Novo registro de aplicativo**.
4. Especifique um nome para o aplicativo, selecione **Nativo** como o tipo de aplicativo e forneça o URI de Redirecionamento para seu aplicativo. 

   ![Criar um novo registro de aplicativo](./media/application-proxy-configure-native-client-application/create.png)
5. Selecione **Criar**.

Para obter informações mais detalhadas sobre como criar um novo registro de aplicativo, confira [Integrando aplicativos ao Azure Active Directory](../develop/quickstart-v1-integrate-apps-with-azure-ad.md).


## <a name="step-3-grant-access-to-other-applications"></a>Etapa 3: Conceder acesso a outros aplicativos
Habilite o aplicativo nativo para ser exposto a outros aplicativos no seu diretório:

1. Ainda nos **Registros de aplicativo**, selecione o novo aplicativo nativo que você acabou de criar.
2. Selecione **Permissões de API**.
3. Selecione **Adicionar uma permissão**.
4. Abra a primeira etapa, **Selecionar uma API**.
5. Use a barra de pesquisa para encontrar o aplicativo Proxy de Aplicativo que você publicou na primeira seção. Escolha esse aplicativo e clique em **Selecionar**. 

   ![Procurar o aplicativo de proxy](./media/application-proxy-configure-native-client-application/select_api.png)
6. Abra a segunda etapa, **Selecionar permissões**.
7. Use a caixa de seleção para conceder ao aplicativo nativo acesso ao aplicativo de proxy e clique em **Selecionar**.

   ![Conceder acesso ao aplicativo de proxy](./media/application-proxy-configure-native-client-application/select_perms.png)
8. Selecione **Concluído**.


## <a name="step-4-edit-the-active-directory-authentication-library"></a>Etapa 4: Editar a Biblioteca de Autenticação do Active Directory
Edite o código de aplicativo nativo no contexto de autenticação da ADAL (Biblioteca de Autenticação do Active Directory) para incluir o seguinte texto:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = await authContext.AcquireTokenAsync("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

As variáveis no exemplo de código devem ser substituídas da seguinte maneira:

* A **ID do locatário** pode ser encontrada no Portal do Azure. Navegue até **Azure Active Directory** > **Propriedades** e copie a ID de Diretório. 
* A **URL externa** é a URL de front-end que você inseriu no Aplicativo de Proxy. Para encontrar esse valor, navegue até a seção **Proxy de aplicativo** do aplicativo de proxy.
* A **ID do aplicativo** do aplicativo nativo pode ser encontrada na página **Propriedades** do aplicativo nativo.
* O **URI de Redirecionamento do aplicativo nativo** pode ser encontrado na página **URIs de Redirecionamento** do aplicativo nativo.

Quando a ADAL é editada com esses parâmetros, os usuários devem ser capazes de autenticar para aplicativos cliente nativos, mesmo quando eles estão fora da rede corporativa. 

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o fluxo do aplicativo nativo, confira [Aplicativo nativo para API Web](../develop/native-app.md)

Saiba mais sobre como configurar o [Logon único para o proxy de aplicativo](what-is-single-sign-on.md#single- sign-on-methods)
