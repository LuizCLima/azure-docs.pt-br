---
title: Transparent Data Encryption no SQL Data Warehouse (T-SQL) | Microsoft Docs
description: TDE (Transparent Data Encryption) no SQL Data Warehouse (T-SQL)
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 7908e6e0927357446ea45e16b7c44adb83ec1fd3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57892431"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Introdução ao Transparent Data Encryption (TDE)
> [!div class="op_single_selector"]
> * [Visão Geral da Segurança](sql-data-warehouse-overview-manage-security.md)
> * [Autenticação](sql-data-warehouse-authentication.md)
> * [Criptografia (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Criptografia (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permissions"></a>Permissões Necessárias
Para habilitar a TDE (Transparent Data Encryption), você deve ser um administrador ou um membro da função dbmanager.

## <a name="enabling-encryption"></a>Habilitando a criptografia
Siga estas etapas para habilitar a TDE para um SQL Data Warehouse:

1. Conecte ao banco de dados *mestre* no servidor que está hospedando o banco de dados que usa um logon de administrador ou de um membro da função **dbmanager** no banco de dados mestre
2. Execute a instrução a seguir para criptografar o banco de dados.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Desabilitando a criptografia
Siga estas etapas para desabilitar a TDE para um SQL Data Warehouse:

1. Conecte-se ao banco de dados *mestre* que usa um logon de administrador ou de um membro da função **dbmanager** no banco de dados mestre
2. Execute a instrução a seguir para criptografar o banco de dados.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Um SQL Data Warehouse em pausa deve ser retomado antes das mudanças nas configurações da TDE.
> 
> 

## <a name="verifying-encryption"></a>Verificando a criptografia
Para verificar o status de criptografia para um SQL Data Warehouse, siga estas etapas:

1. Conecte-se ao banco de dados *mestre* ou de instância que usa um logon de administrador ou de um membro da função **dbmanager** no banco de dados mestre
2. Execute a instrução a seguir para criptografar o banco de dados.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Um resultado de ```1``` indica um banco de dados criptografado, ```0``` indica um banco de dados não criptografado.

## <a name="encryption-dmvs"></a>DMVs de criptografia
* [sys.databases][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: https://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
