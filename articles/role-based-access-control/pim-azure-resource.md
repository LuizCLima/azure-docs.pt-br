---
title: Gerenciar o acesso aos recursos do Azure com o PIM (Azure AD Privileged Identity Management)
description: Saiba como gerenciar acesso aos recursos do Azure usando PIM (Azure Active Directory Privileged Identity Management) e RBAC (controle de acesso baseado em função).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 757068034868744b408c9402b521a0e4c73950f7
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/18/2019
ms.locfileid: "56338189"
---
# <a name="manage-access-to-azure-resources-with-azure-ad-privileged-identity-management"></a>Gerenciar o acesso aos recursos do Azure com o Azure AD Privileged Identity Management

Para proteger contas privilegiadas contra ataques cibernéticos mal-intencionados, use o PIM (Azure Active Directory Privileged Identity Management) para reduzir o tempo de exposição de privilégios e aumentar a visibilidade de seu uso por meio de relatórios e alertas. O PIM faz isso limitando os usuários a apenas usar seus privilégios JIT (“Just-In-Time”) ou atribuindo privilégios por uma duração reduzida após a qual eles são revogados automaticamente. 

Agora você pode usar o PIM com o RBAC (controle de acesso baseado em função) para gerenciar, controlar e monitorar o acesso aos recursos do Azure. O PIM pode gerenciar a associação de funções internas e personalizadas para ajudá-lo a: 

- Habilitar o acesso “Just-In-Time” sob demanda aos recursos do Azure
- Expirar o acesso aos recursos automaticamente para os usuários e grupos atribuídos
- Atribuir o acesso temporário aos recursos do Azure para tarefas rápidas ou escalas de plantão
- Obtenha alertas quando novos usuários ou grupos recebem o acesso aos recursos e quando eles ativam atribuições qualificadas

Para obter mais informações, confira [O que é o Privileged Identity Management do Azure AD?](../active-directory/privileged-identity-management/pim-configure.md).