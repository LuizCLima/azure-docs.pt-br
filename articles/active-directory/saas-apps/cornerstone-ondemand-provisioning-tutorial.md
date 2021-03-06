---
title: 'Tutorial: Configurar o Cornerstone OnDemand para o provisionamento automático de usuário com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para provisionar e desprovisionar automaticamente contas de usuário para Cornerstone OnDemand.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d17a3c81784d56c6fcad7c7608559abf732882a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59274086"
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Tutorial: Configurar o Cornerstone OnDemand para o provisionamento automático de usuário

O objetivo deste tutorial é demonstrar as etapas a serem executadas na Cornerstone OnDemand e no Microsoft Azure AD (Azure Active Directory) para configurar o Microsoft Azure AD para provisionar e desprovisionar automaticamente usuários e/ou grupos para a Cornerstone OnDemand.

> [!NOTE]
> Este tutorial descreve um conector compilado na parte superior do Serviço de Provisionamento de Usuário do Microsoft Azure AD. Para detalhes importantes sobre o que esse serviço faz, como funciona e as perguntas frequentes, consulte [Automatizar o provisionamento e desprovisionamento de usuários para aplicativos SaaS com o Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial pressupõe que você já tem os seguintes pré-requisitos:

* Um locatário do Azure AD
* Um locatário do Cornerstone OnDemand
* Uma conta de usuário na Cornerstone OnDemand com permissões de administrador

> [!NOTE]
> A integração de provisionamento do Microsoft Azure AD baseia-se no [Serviço da Web da Cornerstone OnDemand](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf), que está disponível para equipes da Cornerstone OnDemand.

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Adicionando Cornerstone OnDemand da galeria

Antes de configurar a Cornerstone OnDemand para provisionamento automático de usuário com Microsoft Azure AD, será necessário adicionar a Cornerstone OnDemand da galeria de aplicativos do Microsoft Azure AD na sua lista de aplicativos SaaS gerenciados.

**Para adicionar a Cornerstone OnDemand da galeria de aplicativos do Microsoft Azure AD, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Cornerstone OnDemand**, selecione **Cornerstone OnDemand** no painel de resultados e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Cornerstone OnDemand na lista de resultados](common/search-new-app.png)

## <a name="assigning-users-to-cornerstone-ondemand"></a>Atribuir usuários para Cornerstone OnDemand

O Azure Active Directory usa um conceito chamado "atribuições" para determinar quais usuários devem receber acesso aos aplicativos selecionados. No contexto do provisionamento automático de usuário, somente os usuários e/ou grupos que foram atribuídos a um aplicativo no Microsoft Azure AD são sincronizados.

Antes de configurar e habilitar o provisionamento automático de usuário, será necessário decidir quais usuários e/ou grupos no Microsoft Azure AD precisarão acessar a Cornerstone OnDemand. Uma vez decidido, será possível atribuir esses usuários e/ou grupos à Cornerstone OnDemand seguindo estas instruções:

* [Atribuir um usuário ou um grupo a um aplicativo empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Dicas importantes para atribuir usuários à Cornerstone OnDemand

* É recomendável que um único usuário do Microsoft Azure AD seja atribuído à Cornerstone OnDemand para testar a configuração de provisionamento automático de usuário. Outros usuários e/ou grupos podem ser atribuídos mais tarde.

* Ao atribuir um usuário à Cornerstone OnDemand, você deverá selecionar qualquer função específica do aplicativo válida (se disponível) na caixa de diálogo de atribuição. Usuários com a função **Acesso padrão** são excluídos do provisionamento.

## <a name="configuring-automatic-user-provisioning-to-cornerstone-ondemand"></a>Configurar o provisionamento automático de usuário para a Cornerstone OnDemand

Esta seção guia-o através das etapas para configurar o serviço de provisionamento do Microsoft Azure AD para criar, atualizar e desabilitar usuários e/ou grupos na Cornerstone OnDemand com base em atribuições de usuário e/ou grupo no Microsoft Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-cornerstone-ondemand-in-azure-ad"></a>Para configurar o provisionamento automático de usuário para a Cornerstone OnDemand no Microsoft Azure AD:

1. Entrar para o [portal do Azure](https://portal.azure.com) e selecione **aplicativos empresariais**, selecione **todos os aplicativos**, em seguida, selecione **Cornerstone OnDemand**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Cornerstone OnDemand**.

    ![O link do Cornerstone OnDemand na lista de aplicativos](common/all-applications.png)

3. Selecione a guia **Provisionamento**.

    ![Provisionamento da Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Defina o **Modo de Provisionamento** como **Automático**.

    ![Provisionamento da Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. Na seção **Credenciais de Administrador**, insira o **Nome de Usuário do Administrador**, **Senha de Administrador** e **Domínio** da conta da Cornerstone OnDemand.

    * No campo **Nome de Usuário do Administrador**, preencha o nome de usuário/domínio da conta de administrador no Locatário da Cornerstone OnDemand. Exemplo: contoso\admin.

    * No campo **Senha de Administrador**, preencha a senha correspondente ao nome de usuário do administrador.

    * No campo **Domínio**, preencha a URL do serviço da Web do locatário Cornerstone OnDemand. Exemplo: O serviço está localizado em `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`; para a Contoso, o domínio é `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`. Para obter mais informações sobre como recuperar a URL do serviço Web, consulte [aqui](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_Web_Services_-_User-OU_Technical_Specification_v20160222.pdf).

6. Ao preencher os campos mostrados na Etapa 5, clique em **Testar Conectividade** para garantir que o Microsoft Azure AD possa conectar-se à Cornerstone OnDemand. Se a conexão falhar, certifique-se de que a conta da Cornerstone OnDemand tenha permissões de administrador e tente novamente.

    ![Provisionamento da Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/TestConnection.png)

7. No campo **Notificação por Email**, insira o endereço de email de uma pessoa ou grupo que deverá receber as notificações de erro de provisionamento e selecione a caixa de seleção - **Enviar uma notificação por email quando ocorrer uma falha**.

    ![Provisionamento da Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/EmailNotification.png)

8. Clique em **Salvar**.

9. Na seção **Mapeamentos**, selecione **Sincronizar usuários do Azure Active Directory com a Cornerstone OnDemand**.

    ![Provisionamento da Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserMapping.png)

10. Revise os atributos de usuário que são sincronizados do Microsoft Azure AD com a Cornerstone OnDemand na seção **Mapeamento de Atributos**. Os atributos selecionados como propriedades **Correspondentes** são utilizados para corresponder as contas de usuários na Cornerstone OnDemand para operações de atualização. Selecione o botão **Salvar** para confirmar as alterações.

    ![Provisionamento da Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserMappingAttributes.png)

11. Para configurar filtros de escopo, consulte as seguintes instruções fornecidas no [tutorial do Filtro de Escopo](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

12. Para habilitar o serviço de provisionamento do Microsoft Azure AD para a Cornerstone OnDemand, altere o **Status de Provisionamento** para **Ativado** na seção **Configurações**.

    ![Provisionamento da Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningStatus.png)

13. Defina os usuários e/ou grupos a serem provisionados para a Cornerstone OnDemand, escolhendo os valores desejados na seção **Escopo** nas **Configurações**.

    ![Provisionamento da Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/SyncScope.png)

14. Quando estiver pronto para provisionar, clique em **Salvar**.

    ![Provisionamento da Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/Save.png)

Essa operação inicia a sincronização inicial de todos os usuários e/ou grupos definidos no **Escopo** na seção **Configurações**. Observe que a sincronização inicial levará mais tempo do que as sincronizações subsequentes, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço de provisionamento do Microsoft Azure Active Directory esteja em execução. Use a seção **Detalhes de Sincronização** para monitorar o progresso e siga os links para os relatórios de atividade de provisionamento, que descrevem todas as ações executadas pelo serviço de provisionamento do Microsoft Azure AD na Cornerstone OnDemand.

Para saber mais sobre como ler os logs de provisionamento do Azure AD, consulte [Relatórios sobre o provisionamento automático de contas de usuário](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitações do conector

* O atributo Cornerstone OnDemand **posição** espera um valor que corresponde às funções no portal da Cornerstone OnDemand. A lista de valores válidos de **Posição** podem ser obtidos navegando até **Editar Registro de Usuário > Estrutura organizacional > Posição** no portal da Cornerstone OnDemand.

    ![Usuário de Edição de Provisionamento da Cornerstone OnDemand ](./media/cornerstone-ondemand-provisioning-tutorial/UserEdit.png) ![Provisionamento de Posicionamento da Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserPosition.png) ![Lista de Posições de Provisionamento Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/PostionId.png)

## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciamento do provisionamento de conta de usuário para Aplicativos Empresariais](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Próximas etapas

* [Saiba como fazer revisão de logs e obter relatórios sobre atividade de provisionamento](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_03.png
