---
title: Excluir um grupo - Active Directory do Azure | Microsoft Docs
description: Instruções sobre como criar um grupo usando o Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: d9543908aafbb4ecd8f642f766f656f780706a36
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207204"
---
# <a name="delete-a-group-using-azure-active-directory"></a>Excluir um grupo usando o Microsoft Azure Active Directory
Você pode excluir um grupo do Azure Active Directory (Microsoft Azure Active Directory) para qualquer número de motivos, mas normalmente, será porque você:

- atribuiu incorretamente o **Tipo de grupo** à opção errada.

- criou o grupo errado ou um grupo duplicado por engano. 

- não precisa mais do grupo.

## <a name="to-delete-a-group"></a>Para excluir um grupo
1. Faça login no [portal do Azure](https://portal.azure.com) usando uma conta de administrador global para o diretório.

2. Selecione **Active Directory do Azure** e, em seguida, selecione **Grupos**.

3. Na página **Grupos - Todos os grupos**, pesquise e selecione o grupo que você deseja excluir. Para essas etapas, usaremos **política de MDM - Leste**.

    ![Grupos - Todos os grupos, o nome do grupo destacado](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. Na página **MDM policy - East Overview**, selecione **Delete**.

    O grupo é excluído do seu locatário do Azure Active Directory.

    ![Política de MDM - página Visão Geral Leste, opção de exclusão destacada](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>Próximas etapas

- Se você excluir um grupo por engano, poderá criá-lo novamente. Para mais informações, consulte [Como criar um grupo básico e adicionar membros](active-directory-groups-create-azure-portal.md).

- Se você excluir um grupo do Office 365 por engano, você poderá restaurá-lo. Para obter mais informações, consulte [restaurar um grupo do Office 365 excluído](../users-groups-roles/groups-restore-deleted.md).
