---
title: Gerenciar retenção de backup de longo prazo do Banco de Dados SQL do Azure | Microsoft Docs
description: Saiba como armazenar backups automatizados no armazenamento do SQL do Azure e como restaurá-los
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 04/17/2019
ms.openlocfilehash: 255f118d6dc6873364c2f8d4569e23c3e54ea83e
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680512"
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Gerenciar a retenção de backup de longo prazo do Banco de Dados SQL do Azure

No Banco de Dados SQL do Azure, é possível configurar um banco de dados individual ou em pool com uma política LTR de [retenção de backup em longo prazo](sql-database-long-term-retention.md) para reter backups automaticamente no armazenamento de Blobs do Azure por até 10 anos. Em seguida, você pode recuperar um banco de dados usando esses backups pelo Portal do Azure ou o PowerShell.

> [!IMPORTANT]
> A [Instância Gerenciada do Banco de Dados SQL do Azure](sql-database-managed-instance.md) não dá suporte à retenção de backup em longo prazo.

## <a name="use-the-azure-portal-to-configure-long-term-retention-policies-and-restore-backups"></a>Usar o Portal do Azure para configurar políticas de retenção de longo prazo e restaurar backups

As seções a seguir mostram como usar o Portal do Azure para configurar a retenção de longo prazo, exibir os backups em retenção de longo prazo e restaurar backups da retenção de longo prazo.

### <a name="configure-long-term-retention-policies"></a>Configurar políticas de retenção de longo prazo

Você pode configurar o Banco de Dados SQL para [reter backups automatizados](sql-database-long-term-retention.md) por um período maior que o período de retenção da camada de serviços. 

1. No portal do Azure, selecione o servidor SQL e clique em **Gerenciar Backups**. Na guia **Configurar políticas**, *marque a caixa de seleção do banco de dados no qual deseja definir ou modificar políticas de retenção de backup de longo prazo*. Se a caixa de seleção ao lado do banco de dados não estiver marcada, as alterações da política não se aplicarão ao banco de dados.  

   ![link gerenciar backups](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. No painel **Configurar políticas**, selecione se deseja reter backups semanais, mensais ou anuais e especifique o período de retenção para cada um. 

   ![configurar políticas](./media/sql-database-long-term-retention/ltr-configure-policies.png)

3. Quando concluir, clique em **Aplicar**.

> [!IMPORTANT]
> Quando você habilita uma política de retenção de backup de longo prazo, pode levar até 7 dias para o primeiro backup se torne visível e disponível para restauração. Para obter detalhes sobre o cadance de backup do LTR, consulte [retenção de longo prazo](sql-database-long-term-retention.md).

### <a name="view-backups-and-restore-from-a-backup-using-azure-portal"></a>Exibir os backups e restaurar de um backup usando o Portal do Azure

Exiba os backups que são mantidos para um banco de dados específico com uma política LTR e restaure a partir desses backups. 

1. No portal do Azure, selecione o servidor SQL e clique em **Gerenciar Backups**. Na guia **Backups disponíveis**, selecione o banco de dados para o qual você deseja ver os backups disponíveis.

   ![selecionar banco de dados](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

3. No painel **Backups disponíveis**, examine os backups disponíveis. 

   ![exibir backups](./media/sql-database-long-term-retention/ltr-available-backups.png)

4. Selecione o backup do qual você deseja restaurar e, em seguida, especifique o novo nome do banco de dados.

   ![restaurar](./media/sql-database-long-term-retention/ltr-restore.png)

5. Clique em **OK** para restaurar seu banco de dados a partir do backup no armazenamento do SQL do Azure para o novo banco de dados.

6. Na barra de ferramentas, clique no ícone de notificação para exibir o status do trabalho de restauração.

   ![progresso do trabalho de restauração](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Quando o trabalho de restauração for concluído, abra a página **Bancos de dados SQL** para exibir o banco de dados recém-restaurado.

> [!NOTE]
> A partir daqui, você pode conectar o banco de dados restaurado usando o SQL Server Management Studio para executar as tarefas necessárias, tais como, [extrair um pouco de dados do banco de dados restaurado para copiar para o banco de dados existente ou excluir o banco de dados existente e renomear o banco de dados restaurado com o nome do banco de dados existente](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="use-powershell-to-configure-long-term-retention-policies-and-restore-backups"></a>Usar o PowerShell para configurar políticas de retenção de longo prazo e restaurar backups

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> O módulo do PowerShell do Azure Resource Manager ainda é compatível com o banco de dados SQL, mas todo o desenvolvimento futuro é para o módulo Az.Sql. Para esses cmdlets, consulte [azurerm. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Os argumentos para os comandos no módulo Az e nos módulos AzureRm são substancialmente idênticos.

As seções a seguir mostram como usar o PowerShell para configurar a retenção de backup de longo prazo, exibir backups no armazenamento do SQL do Azure e restaurar a partir de um backup no armazenamento do SQL do Azure.


### <a name="rbac-roles-to-manage-long-term-retention"></a>Funções RBAC para gerenciar retenção de longo prazo

Para gerenciar backups de LTR, será necessário ser 
- Proprietário da assinatura ou ter
- Função Colaborador do SQL Server no escopo da **Assinatura** ou
- Função Colaborador do Banco de Dados SQL no escopo da **Assinatura**

Se for necessário um controle mais granular, você poderá criar funções personalizadas do RBAC e atribuí-las no escopo da **Assinatura**. 

Para **Get-AzSqlDatabaseLongTermRetentionBackup** e **AzSqlDatabase restauração** a função deve ter as seguintes permissões:

Microsoft.Sql/locations/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read
 
Para **AzSqlDatabaseLongTermRetentionBackup remover** a função precisa ter as seguintes permissões:

Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete


### <a name="create-an-ltr-policy"></a>Criar uma política LTR

```powershell
# Get the SQL server 
# $subId = “{subscription-id}”
# $serverName = “{server-name}”
# $resourceGroup = “{resource-group-name}” 
# $dbName = ”{database-name}”

Connect-AzAccount
Select-AzSubscription -SubscriptionId $subId

# get the server
$server = Get-AzSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W 

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetention = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>Exibir políticas LTR
Este exemplo mostra como listar as políticas LTR dentro de um servidor

```powershell
# Get all LTR policies within a server
$ltrPolicies = Get-AzSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | Get-AzSqlDatabaseLongTermRetentionPolicy -Current 

# Get the LTR policy of a specific database 
$ltrPolicies = Get-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName  -ResourceGroupName $resourceGroup -Current
```
### <a name="clear-an-ltr-policy"></a>Limpar uma política LTR
Este exemplo mostra como limpar uma política LTR de um banco de dados

```powershell
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

### <a name="view-ltr-backups"></a>Exibir backups LTR

Este exemplo mostra como listar os backups LTR dentro de um servidor. 

```powershell
# Get the list of all LTR backups in a specific Azure region 
# The backups are grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest
# backup first.  
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location 

# Get the list of LTR backups from the Azure region under 
# the named server. 
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName

# Get the LTR backups for a specific database from the Azure region under the named server 
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -DatabaseName $dbName

# List LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -DatabaseState Live

# Only list the latest LTR backup for each database 
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -OnlyLatestPerDatabase
```

### <a name="delete-ltr-backups"></a>Excluir backups LTR

Este exemplo mostra como excluir um backup LTR da lista de backups.

```powershell
# remove the earliest backup 
$ltrBackup = $ltrBackups[0]
Remove-AzSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```
> [!IMPORTANT]
> A exclusão do backup LTR é irreversível. Você pode configurar notificações sobre cada exclusão no Azure Monitor filtrando a operação "Exclui um backup de retenção de longo prazo". O log de atividades contém informações sobre quem fez a solicitação e quando. Confira [Criar alertas do log de atividades](../azure-monitor/platform/alerts-activity-log.md) para obter instruções detalhadas.
>

### <a name="restore-from-ltr-backups"></a>Restaurar a partir de backups LTR
Este exemplo mostra como restaurar a partir de um backup LTR. Observe que esta interface não foi alterada, mas o parâmetro de identificação do recurso agora requer a identificação do recurso do backup LTR. 

```powershell
# Restore LTR backup as an S3 database
Restore-AzSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup -TargetDatabaseName $dbName -ServiceObjectiveName S3
```

> [!NOTE]
> A partir daqui, você pode conectar o banco de dados restaurado usando o SQL Server Management Studio para executar as tarefas necessárias, tais como, extrair um pouco de dados do banco de dados restaurado para copiar para o banco de dados existente ou excluir o banco de dados existente e renomear o banco de dados restaurado com o nome do banco de dados existente. Confira [recuperação pontual](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Próximos passos

- Para saber mais sobre backups automáticos gerados pelo serviço, veja [backups automáticos](sql-database-automated-backups.md)
- Para saber mais sobre a retenção de backup de longo prazo, consulte [retenção de backup de longo prazo](sql-database-long-term-retention.md)
