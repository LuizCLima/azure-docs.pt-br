---
title: Auditoria no SQL Data Warehouse do Azure | Microsoft Docs
description: Saiba mais sobre auditoria e como configurar a auditoria no SQL Data Warehouse do Azure.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/11/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 85693ec6aa67dc69cd65aae8e66e66e2118672ef
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57898475"
---
# <a name="auditing-in-azure-sql-data-warehouse"></a>Auditoria no Azure SQL Data Warehouse

Saiba mais sobre auditoria e como configurar a auditoria no SQL Data Warehouse do Azure.

## <a name="what-is-auditing"></a>O que é auditoria?
A auditoria do SQL Data Warehouse permite registrar eventos no banco de dados em um log de auditoria na sua Conta de Armazenamento do Azure. A auditoria pode ajudar você a manter uma conformidade regulatória, a entender a atividade do banco de dados e a obter informações sobre discrepâncias e anomalias que poderiam indicar preocupações de negócios ou suspeitas de violações de segurança.

As ferramentas de auditoria permitem e facilitam a adoção de padrões de conformidade, mas não garantem a conformidade. Para obter mais informações sobre os programas Azure que oferecem suporte à conformidade com os padrões, consulte o [Azure Trust Center](https://azure.microsoft.com/support/trust-center/compliance/).

## <a id="subheading-1"></a>Fundamentos da Auditoria
A auditoria do banco de dados do SQL Data Warehouse permite que você:

* **Retenha** uma trilha de auditoria dos eventos selecionados. Definir categorias de ações de banco de dados a ser auditadas.
* **Relate** sobre a atividade do banco de dados. Utilizar relatórios pré-configurados e um painel para iniciar rapidamente um relatório de atividades e eventos.
* **Analise** relatórios. Encontrar eventos suspeitos, atividades incomuns e tendências.

Logs de auditoria são armazenados na sua conta de armazenamento do Azure. Você pode definir um período de retenção para logs de auditoria.


## <a id="subheading-4"></a>Definir a política de auditoria no nível do servidor versus no nível do banco de dados

Uma política de auditoria pode ser definida para um banco de dados específico ou como uma política padrão de servidor:

* Uma política de servidor **se aplica a todos os bancos de dados existentes e recém-criados** no servidor.

* Se a opção *auditoria de blob do servidor estiver habilitada*, ela *sempre será aplicada ao banco de dados*. O banco de dados será auditado, independentemente das configurações de auditoria do banco de dados.

* A habilitação da auditoria no banco de dados, além de sua habilitação no servidor, *não* substitui nem altera as configurações de auditoria de blob do servidor. Ambas as auditorias existirão lado a lado. Em outras palavras, o banco de dados é auditado duas vezes em paralelo: uma vez pela política de servidor e outra, pela política de banco de dados.

> [!NOTE]
> Recomendamos habilitar **somente a auditoria de blob no nível do servidor** e deixar a auditoria no nível do banco de dados desabilitada para todos os bancos de dados.
> Evite habilitar a auditoria de servidor e a auditoria do banco de dados juntas, a menos que:
> * Você deseja usar outra *conta de armazenamento* ou outro *período de retenção* para um banco de dados específico.
> * Você deseja auditar tipos de evento ou categorias de um banco de dados específico diferentes do restante dos bancos de dados no servidor. Por exemplo, talvez você tenha inserções de tabela que precisam ser auditadas somente em um banco de dados específico.
> * Você queira usar a Detecção de ameaças, que só tem suporte atualmente com a auditoria de nível de banco de dados.

> [!IMPORTANT]
>Habilitar a auditoria em um Azure SQL Data Warehouse ou em um servidor que tenha um Azure SQL Data Warehouse **fará com que o Data Warehouse seja retomado**, mesmo que ele estivesse em pausa anteriormente. **Não se esqueça de pausar o Data Warehouse novamente após a habilitação da auditoria**.

## <a id="subheading-5"></a>Configurar a auditoria de nível de servidor para todos os bancos de dados

Uma política de auditoria de servidor se aplica a **todos os bancos de dados existentes e recém-criados** no servidor.

A seção a seguir descreve a configuração de auditoria usando o Portal do Azure.

1. Vá para o [Portal do Azure](https://portal.azure.com).
2. Vá para o **SQL Server** que você deseja auditar (importante: verifique se você está exibindo o SQL Server, e não um banco de dados/DW específico). No menu **Segurança**, selecione **Auditoria e Detecção de ameaças**.

    ![Painel de navegação][6]
4. Na folha *Auditoria e Detecção de ameaças*, selecione **ATIVAR** em **Auditoria**. Esta política de auditoria se aplicará a todos os bancos de dados existentes e recém-criados nesse servidor.

    ![Painel de navegação][7]
5. Para abrir a folha **Armazenamento de Logs de Auditoria**, selecione **Detalhes de Armazenamento**. Selecione ou crie a conta de armazenamento do Azure em que os logs serão salvos e, depois, selecione o período de retenção (os logs antigos serão excluídos). Em seguida, clique em **OK**.

    ![Painel de navegação][8]

    > [!IMPORTANT]
    > Os logs de auditoria são gravados nos **Blobs de Acréscimo** em um armazenamento de Blob do Azure em sua assinatura do Azure.
    >
    > - Há suporte para todos os tipos de armazenamento (v1, v2, blob).
    > - Há suporte para todas as configurações de replicação de armazenamento.
    > - Atualmente, **não há suporte** para o **Armazenamento Premium**.
    > - Atualmente, o **armazenamento na VNet** **não tem suporte**.
    > - Atualmente, **não há suporte** para o **Armazenamento atrás de um firewall**

8. Clique em **Salvar**.



## <a id="subheading-2"></a>Configurar a auditoria de nível de banco de dados para um banco de dados individual

Uma política de auditoria pode ser definida para um banco de dados específico ou como uma política padrão de servidor.

É altamente recomendável usar a auditoria de nível de servidor e não a auditoria de nível de banco de dados, conforme descrito em [Definir nível de servidor versus política de auditoria de nível de banco de dados](#subheading-4)

Antes de configurar a auditoria, verifique se você está usando um ["Cliente de nível inferior"](sql-data-warehouse-auditing-downlevel-clients.md).


1. Inicie o [Portal do Azure](https://portal.azure.com).
2. Acesse a folha **Configurações** para o SQL Data Warehouse que deseja auditar. Selecione **Auditoria e Detecção de Ameaças**.

    ![][1]
3. Em seguida, habilite a auditoria clicando no botão **ON** .

    ![][3]
4. No painel de configuração de auditoria, selecione **DETALHES DE ARMAZENAMENTO** para abrir o painel Armazenamento de Logs de Auditoria. Selecione a conta de armazenamento do Azure para os logs e período de retenção.
    >[!TIP]
    >Use a mesma conta de armazenamento para todos os bancos de dados auditados para aproveitar ao máximo os modelos de relatórios pré-configurados.

    ![][4]

5. Clique no botão **OK** para salvar a configuração de detalhes do armazenamento.
6. Em **REGISTRO EM LOG POR EVENTO**, clique em **SUCESSO** e **FALHA** para registrar todos os eventos ou escolha categorias de evento individuais.
7. Se você estiver configurando a Auditoria para um banco de dados, talvez seja necessário alterar a cadeia de conexão do cliente para garantir que os dados de auditoria sejam capturados corretamente. Verifique o tópico [Modificar o FQDN do servidor na cadeia de conexão](sql-data-warehouse-auditing-downlevel-clients.md) para ver conexões de cliente de nível inferior.
8. Clique em **OK**.

## <a id="subheading-3"></a>Analisar os logs e relatórios de auditoria

### <a name="server-level-policy-audit-logs"></a>Logs de auditoria de política de nível de servidor
Os logs de auditoria de nível do servidor são gravados nos **Blobs Acrescentados** em um armazenamento de Blob do Azure em sua assinatura do Azure. Eles são salvos como uma coleção de arquivos de blob em um contêiner chamado **sqldbauditlogs**.

Para obter mais detalhes sobre a hierarquia da pasta de armazenamento, as convenções de nomenclatura e o formato do log, consulte a [Referência de formato do log de auditoria de blob](https://go.microsoft.com/fwlink/?linkid=829599).

Há vários métodos que podem ser usados para exibir os logs de auditoria de blob:

* Use a opção **Mesclar Arquivos de Auditoria** no SQL Server Management Studio (a partir do SSMS 17):
    1. No menu do SSMS, selecione **Arquivo** > **Abrir** > **Mesclar Arquivos de Auditoria**.

    2. A caixa de diálogo **Adicionar Arquivos de Auditoria** será aberta. Selecione uma das opções **Adicionar**, escolha se deseja mesclar arquivos de auditoria de um disco local ou importá-los do Armazenamento do Azure. Você deve fornecer os detalhes do Armazenamento do Azure e a chave de conta.

    3. Depois que todos os arquivos a serem mesclados forem adicionados, clique em **OK** para concluir a operação de mesclagem.

    4. O arquivo mesclado é aberto no SSMS, no qual você pode exibi-lo e analisá-lo, bem como exportá-lo para um arquivo XEL ou CSV ou para uma tabela.

* Use o [aplicativo de sincronização](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) que criamos. Ele é executado no Azure e do log analytics de utiliza APIs públicas para enviar por push SQL logs de auditoria nos logs do Azure Monitor. O aplicativo de sincronização envia os logs de auditoria do SQL para os logs do Azure Monitor para consumo por meio do painel de análise de log.

* Use o Power BI. Você pode exibir e analisar dados do log de auditoria no Power BI. Saiba mais sobre o [Power BI e acesse um modelo para download](https://blogs.msdn.microsoft.com/azuresqldbsupport/20../../sql-azure-blob-auditing-basic-power-bi-dashboard/).

* Baixe os arquivos de log do contêiner de Azure Storage Blob por meio do portal ou usando uma ferramenta como o [Gerenciador de Armazenamento do Azure](https://storageexplorer.com/).
    * Depois de baixar um arquivo de log localmente, clique duas vezes no arquivo para abrir, exibir e analisar os logs no SSMS.
    * Baixe também vários arquivos simultaneamente por meio do Gerenciador de Armazenamento do Azure. Clique com o botão direito do mouse em uma subpasta específica e selecione **Salvar como** para salvar em uma pasta local.

* Métodos adicionais:
   * Depois de baixar vários arquivos ou uma subpasta que contém arquivos de log, você pode mesclá-los localmente, conforme descrito nas instruções de Arquivos de Auditoria de Mesclagem do SSMS indicadas anteriormente.

   * Exiba os logs de auditoria de blob de forma programática:

     * Use a biblioteca C# do [Leitor de Eventos Estendidos](https://blogs.msdn.microsoft.com/extended_events/20../../introducing-the-extended-events-reader/).
     * [Consulte Arquivos de Eventos Estendidos usando o PowerShell](https://sqlscope.wordpress.com/20../../reading-extended-event-files-using-client-side-tools-only/).



<br>

### <a name="database-level-policy-audit-logs"></a>Logs de auditoria de política de nível de banco de dados
Os logs de auditoria de nível de banco de dados são agregados em uma coleção de Tabelas de Armazenamento com um prefixo **SQLDBAuditLogs** na conta de armazenamento do Azure que você escolheu durante a instalação. Você pode ver os arquivos de log usando uma ferramenta como o [Gerenciador de Armazenamento do Azure](https://azurestorageexplorer.codeplex.com).

Um modelo de relatório do painel pré-configurado está disponível como uma planilha do [Excel para download](https://go.microsoft.com/fwlink/?LinkId=403540) para ajudar você a analisar rapidamente os dados de log. Para utilizar o modelo em seus logs de auditoria, você precisará do Excel 2013 ou mais recente e do Power Query, que você pode [baixar aqui](https://www.microsoft.com/download/details.aspx?id=39379).

O modelo possui dados de amostra fictícios, e você pode configurar o Power Query para importar seu log de auditoria diretamente da sua conta de armazenamento do Azure.

## <a id="subheading-4"></a>Regeneração de chave de armazenamento
Em produção, você provavelmente atualizará suas chaves de armazenamento periodicamente. Ao atualizar suas chaves, é necessário salvar a política. O processo é o seguinte:

1. No painel de configurações de auditoria, que está descrito na seção anterior de instalação de auditoria, altere a **Chave de acesso de armazenamento** de *Primária* para *Secundária* e  **SALVE**.

   ![][4]
2. Acesse o painel de configurações de armazenamento e **regenere** a *Chave de Acesso Primária*.
3. Volte para o painel de configurações de auditoria,
4. altere a **Chave de Acesso de Armazenamento** de *Secundária* para *Primária* e pressione **SALVAR**.
4. Volte para a interface do usuário de armazenamento e **regenere** a *Chave de Acesso Secundária* (como preparação para o próximo ciclo de atualização de chaves.

## <a id="subheading-5"></a>Automação (PowerShell/API REST)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Você também pode configurar a auditoria no SQL Data Warehouse do Azure usando as seguintes ferramentas de automação:

* **Cmdlets do PowerShell**:

<!-- None of the following links exist anymore 3-12-2019
   * [Get-AzSqlDatabaseAuditingPolicy](/powershell/module/az.sql/get-azsqldatabaseauditingpolicy)
   * [Get-AzSqlServerAuditingPolicy](/powershell/module/az.sql/Get-azSqlServerAuditingPolicy)
   * [Remove-AzSqlDatabaseAuditing](/powershell/module/az.sql/Remove-azSqlDatabaseAuditing)
   * [Remove-AzSqlServerAuditing](/powershell/module/az.sql/Remove-azSqlServerAuditing)
   * [Set-AzSqlDatabaseAuditingPolicy](/powershell/module/az.sql/Set-azSqlDatabaseAuditingPolicy)
   * [Set-AzSqlServerAuditingPolicy](/powershell/module/az.sql/Set-azSqlServerAuditingPolicy)
   * [Use-AzSqlServerAuditingPolicy](/powershell/module/az.sql/Use-azSqlServerAuditingPolicy) -->

   * [Get-AzSqlDatabaseAuditing](/powershell/module/az.sql/get-azsqldatabaseauditing)
   * [Set-AzSqlDatabaseAuditing](/powershell/module/az.sql/set-azsqldatabaseauditing)
   * [Get-AzSqlServerAuditing](/powershell/module/az.sql/get-azsqlserverauditing)
   * [Set-AzSqlServerAuditing](/powershell/module/az.sql/set-azsqlserverauditing)

## <a name="downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>Suporte de versão anterior a clientes para auditoria e máscara de dados dinâmicos
A auditoria funciona com clientes SQL que oferecem suporte ao redirecionamento de TDS.

Qualquer cliente que implemente o protocolo TDS 7.4 também deve dar suporte a redirecionamento. As exceções incluem o JDBC 4.0, no qual o recurso de redirecionamento não tem suporte completo, e o Tedious para Node.JS, no qual o redirecionamento não foi implementado.

Para "Clientes de nível inferior" que oferecem suporte a protocolo TDS versão 7.3 e inferior, modifique o FQDN do servidor na cadeia de conexão da seguinte maneira:

- FQDN original do servidor na cadeia de conexão: <*nome do servidor*>.database.windows.net
- FQDN do servidor modificado na cadeia de conexão: <*nome do servidor*>.database.**secure**.windows.net

Uma lista parcial de "Clientes de versão anterior" inclui:

* .NET 4.0 e inferior,
* ODBC 10.0 e inferior.
* JDBC (embora o JDBC dê suporte ao TDS 7.4, o recurso de redirecionamento de TDS não recebe suporte total)
* Tedious (para o Node.JS)

**Comentário:** a modificação do FQDN do servidor acima pode ser útil também para aplicar uma política de Auditoria no Nível do SQL Server sem a necessidade de uma etapa de configuração em cada banco de dados (redução temporária).     




<!--Anchors-->
[Database Auditing basics]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Define server-level vs. database-level auditing policy]: #subheading-4
[Set up server-level auditing for all databases]: #subheading-5

<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png
[6]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_1_overview.png
[7]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_2_enable.png
[8]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_3_storage.png
