---
title: 'Tutorial: Configurar o Jive para o provisionamento automático de usuários com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Jive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 607d538a2a2636e17265e95195000a777f162dc4
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56192329"
---
# <a name="tutorial-configure-jive-for-automatic-user-provisioning"></a>Tutorial: Configurar o Jive para provisionamento automático de usuários

O objetivo deste tutorial é mostrar as etapas que precisam ser realizadas no Jive e no Azure AD para provisionar e desprovisionar automaticamente as contas de usuário do Azure AD para o Jive.

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

*   Um locatário do Azure Active Directory.
*   Uma assinatura habilitada para logon único do Jive.
*   Uma conta de usuário no Jive com permissões de Administrador de Equipe.

## <a name="assigning-users-to-jive"></a>Atribuindo usuários ao Jive

O Azure Active Directory usa um conceito chamado "atribuições" para determinar quais usuários devem receber acesso aos aplicativos selecionados. No contexto do provisionamento automático de conta de usuário, somente os usuários e os grupos que foram "atribuídos" a um aplicativo no Azure AD serão sincronizados.

Antes de configurar e habilitar o serviço de provisionamento, você precisa decidir quais usuários e/ou grupos no Azure AD representam os usuários que precisam de acesso ao aplicativo Jive. Depois de decidir, atribua esses usuários ao aplicativo Jive seguindo estas instruções:

[Atribuir um usuário ou um grupo a um aplicativo empresarial](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a>Dicas importantes para atribuir usuários ao Jive

*   Recomendamos a atribuição de um único usuário do Azure AD ao Jive para testar a configuração de provisionamento. Outros usuários e/ou grupos podem ser atribuídos mais tarde.

*   Ao atribuir um usuário ao Jive, você deve selecionar uma função de usuário válida. A função de "Acesso Padrão" não funciona para provisionamento.

## <a name="enable-user-provisioning"></a>Habilitar o provisionamento de usuário

Esta seção explica como conectar o Azure AD à API de provisionamento de conta de usuário do Jive e como configurar o serviço de provisionamento para criar, atualizar e desabilitar contas de usuário atribuídas no Jive, com base na atribuição de usuário e de grupo do Azure AD.

> [!TIP]
> Você também pode optar por habilitar o Logon Único baseado em SAML no Jive, seguindo as instruções fornecidas no [portal do Azure](https://portal.azure.com). O logon único pode ser configurado independentemente do provisionamento automático, embora esses dois recursos sejam complementares.

### <a name="to-configure-user-account-provisioning"></a>Para configurar o provisionamento de conta de usuário:

O objetivo desta seção é descrever como habilitar o provisionamento de contas de usuário do Active Directory no Jive.
Como parte desse procedimento, é necessário fornecer um token de segurança do usuário que será necessário solicitar em Jive.com.

1. No [Portal do Azure](https://portal.azure.com), navegue até a seção **Azure Active Directory > Aplicativos Empresariais > Todos os aplicativos**.

1. Se você já tiver configurado o Jive para logon único, pesquise a instância do Jive usando o campo de pesquisa. Caso contrário, selecione **Adicionar** e pesquise **Jive** na galeria de aplicativos. Selecione o Jive nos resultados da pesquisa e adicione-o à lista de aplicativos.

1. Selecione a instância do Jive e, depois, a guia **Provisionamento**.

1. Defina o **Modo de Provisionamento** como **Automático**. 

    ![provisionamento](./media/jive-provisioning-tutorial/provisioning.png)

1. Na seção **Credenciais de Administrador**, forneça as seguintes definições de configuração:
   
     a. Na caixa de texto **Nome do Usuário Administrador do Jive**, digite o nome da conta do Jive que tem o perfil do **Administrador de Sistema** atribuído no Jive.com.
   
    b. Na caixa de texto **Senha do Administrador do Jive** , digite a senha desta conta.
   
    c. Na caixa de texto **URL de Locatário do Jive** , digite a URL de locatário do Jive.
      
      > [!NOTE]
      > A URL de locatário do Jive é a URL usada por sua organização para fazer logon no Jive.  
      > Normalmente, a URL tem o seguinte formato: **www.\<organização\>.jive.com**.          

1. No portal do Azure, clique em **Testar conectividade** para garantir que o Azure AD pode se conectar ao aplicativo Jive.

1. Insira o endereço de email de uma pessoa ou grupo que deve receber notificações de erro de provisionamento no campo **Email de Notificação** e marque a caixa de seleção abaixo.

1. Clique em **Salvar.**

1. Na seção Mapeamentos, selecione **Sincronizar Usuários do Azure Active Directory com o Jive.**

1. Na seção **Mapeamentos de Atributo**, examine os atributos de usuário que são sincronizados do Azure AD para o Jive. Os atributos selecionados como propriedades **Correspondentes** são usados para corresponder as contas de usuário do Jive em operações de atualização. Selecione o botão Salvar para confirmar as alterações.

1. Para habilitar o serviço de provisionamento do Azure AD no Jive, altere o **Status de Provisionamento** para **Ativado** na seção Configurações

1. Clique em **Salvar.**

Isso inicia a sincronização inicial de todos os usuários e/ou grupos atribuídos ao Jive na seção Usuários e Grupos. Observe que a sincronização inicial levará mais tempo do que as sincronizações subsequentes, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço esteja em execução. Use a seção **Detalhes de Sincronização** para monitorar o progresso e siga os links para os logs de atividade de provisionamento, que descrevem todas as ações executadas pelo serviço de provisionamento no aplicativo Jive.

Para saber mais sobre como ler os logs de provisionamento do Azure AD, consulte [Relatórios sobre o provisionamento automático de contas de usuário](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciamento do provisionamento de conta de usuário para Aplicativos Empresariais](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configurar Logon Único](jive-tutorial.md)
