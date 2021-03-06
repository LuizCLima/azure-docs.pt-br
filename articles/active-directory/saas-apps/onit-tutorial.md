---
title: 'Tutorial: Integração do Azure Active Directory com o Onit | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Onit.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bc479a28-8fcd-493f-ac53-681975a5149c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/13/2019
ms.author: jeedes
ms.openlocfilehash: 56b3e42a65eb84ef6ee53b4ba16e5fafc4473405
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59270210"
---
# <a name="tutorial-azure-active-directory-integration-with-onit"></a>Tutorial: Integração do Azure Active Directory com o Onit

Neste tutorial, você aprenderá a integrar o Onit ao Azure AD (Azure Active Directory).
A integração do Onit ao Azure AD oferece os seguintes benefícios:

* No Azure AD, é possível controlar quem tem acesso ao Onit.
* Você pode permitir que os usuários entrem automaticamente no Onit (logon único) com a conta do Azure AD.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Onit, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/)
* Uma assinatura do Onit habilitada para logon único

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Onit é compatível com o SSO iniciado por **IdP**

## <a name="adding-onit-from-the-gallery"></a>Adicionar o Onit da galeria

Para configurar a integração do Onit ao Azure AD, você precisará adicionar o Onit à sua lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o Onit por meio da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Onit**, selecione **Onit** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

     ![Onit na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Onit, usando um usuário de teste chamado **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Onit.

Para configurar e testar o logon único do Azure AD com o Onit, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o Logon Único do Onit](#configure-onit-single-sign-on)** – para definir as configurações de logon único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Onit](#create-onit-test-user)** – para que haja um equivalente de Brenda Fernandes no Onit que esteja vinculado à representação do usuário no Azure AD.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Azure AD com o Onit, execute as seguintes etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração do aplicativo **Onit**, selecione **Logon único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na seção **Configuração básica de SAML**, realize as seguintes etapas:

    ![Informações de logon único de Domínio e URLs do Onit](common/sp-identifier.png)

     a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<sub-domain>.onit.com`

    b. Na caixa de texto **Identificador (ID da Entidade)**, digite uma URL usando o seguinte padrão: `https://<sub-domain>.onit.com`

    > [!NOTE]
    > Esses valores não são reais. Atualize esses valores com a URL de Entrada e o Identificador reais. Contate a [equipe de suporte ao cliente do Onit](https://www.onit.com/support) para obter esses valores. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

5. O aplicativo Onit espera as declarações SAML em um formato específico, o que exige que você adicione mapeamentos de atributo personalizados à configuração de atributos do token SAML. A captura de tela a seguir mostra a lista de atributos padrão. Clique no ícone **Editar** para abrir a caixa de diálogo **Atributos de Usuário** .

    ![image](common/edit-attribute.png)

6. Além do indicado acima, o aplicativo Onit espera que mais alguns atributos sejam passados novamente na resposta SAML. Na seção **Declarações de Usuário** da caixa de diálogo **Atributos de Usuário**, execute as seguintes etapas para adicionar o atributo de token SAML, conforme mostrado na tabela abaixo:

    | NOME | Atributo de Origem|
    | ---------------| --------------- |
    | email | user.mail |

     a. Clique em **Adicionar nova reivindicação** para abrir a caixa de diálogo **Gerenciar declarações de usuários**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.

    c. Deixe o **Namespace** em branco.

    d. Escolha Origem como **Atributo**.

    e. Na lista **Atributo de origem**, digite o valor do atributo mostrado para essa linha.

    f. Clique em **Ok**

    g. Clique em **Save** (Salvar).

7. Na seção **Certificado de Autenticação SAML**, clique no botão **Editar** para abrir a caixa de diálogo **Certificado de Autenticação SAML**.

    ![Editar o Certificado de Autenticação SAML](common/edit-certificate.png)

8. Na seção **Certificado de Autenticação SAML**, copie a **Impressão Digital** e salve-a no computador.

    ![Copiar o valor da Impressão Digital](common/copy-thumbprint.png)

9. Na seção **Configurar o Onit**, copie as URLs apropriadas, de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure AD

    c. URL de logoff

### <a name="configure-onit-single-sign-on"></a>Configurar o logon único do Onit

1. Em outra janela do navegador da Web, faça logon em seu site de empresa Onit como um administrador.

2. No menu na parte superior, clique em **Administração**.
   
    ![Administração](./media/onit-tutorial/IC791174.png "Administração")

3. Clique em **Editar Empresa**.
   
    ![Editar Corporação](./media/onit-tutorial/IC791175.png "Editar Corporação")
   
4. Clique na guia **Segurança** .
    
    ![Editar Informações da Empresa](./media/onit-tutorial/IC791176.png "Editar Informações da Empresa")

5. Na guia **Segurança** , realize as seguintes etapas:

    ![Logon Único](./media/onit-tutorial/IC791177.png "Logon Único")

     a. Como **Estratégia de Autenticação**, selecione **Logn Único e Senha**.
    
    b. Na caixa de texto **URL de Destino de IdP**, cole o valor da **URL de Logon** copiado do portal do Azure.

    c. Na caixa de texto **URL de logoff de IdP**, cole o valor da **URL de logoff** copiado do portal do Azure.

    d. Na caixa de texto **Impressão Digital do Certificado IdP (SHA1)**, cole o valor da **Impressão Digital** do certificado copiado do Portal do Azure.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD 

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory**, selecione **Usuários** e, em seguida, **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![Botão Novo usuário](common/new-user.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![A caixa de diálogo Usuário](common/user-properties.png)

    a. No campo **Nome**, insira **BrendaFernandes**.
  
    b. No campo **Nome de usuário**, digite **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao Onit.

1. No portal do Azure, selecione **Aplicativos Empresariais**, selecione **Todos os aplicativos** e, em seguida, selecione **Onit**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Onit**.

    ![O link do Onit na lista de Aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função**, escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-onit-test-user"></a>Criar um usuário de teste do Onit

Para permitir que os usuários do AD do Azure façam logon no Onit, eles devem ser provisionados no Onit. No caso do Onit, o provisionamento é uma tarefa manual.

**Para configurar o provisionamento de usuários, execute as seguintes etapas:**

1. Faça logon em seu site de empresa do **Onit** como administrador.

2. Clique em **Adicionar Usuário**.
   
    ![Administração](./media/onit-tutorial/IC791180.png "Administração")

3. Na página do diálogo **Adicionar usuário** , realize as seguintes etapas:
   
    ![Adicionar Usuário](./media/onit-tutorial/IC791181.png "Adicionar Usuário")
   
     a. Digite o **Nome** e **Endereço de Email** de uma conta válida do Azure AD que você deseja provisionar nas caixas de texto relacionadas.

    b. Clique em **Criar**.    
   
    > [!NOTE]
    > O titular da conta do Azure Active Directory recebe um email e segue um link para confirmar sua conta antes que ela se torne ativa.

### <a name="test-single-sign-on"></a>Testar logon único 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Onit no Painel de Acesso, você será conectado automaticamente ao Onit, para o qual configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

