---
title: Concluir uma revisão de acesso de aplicativos - Azure Active Directory ou grupos | Microsoft Docs
description: Saiba como concluir uma revisão de acesso de membros do grupo ou o acesso de aplicativo em revisões de acesso do Active Directory do Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/02/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4265a7e08eab079e55ce91b27142ec3e55b3f3e9
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58579590"
---
# <a name="complete-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Concluir uma revisão de acesso de grupos ou revisões de acesso de aplicativos no Azure AD

Os administradores podem usar o Azure Active Directory (Azure AD) para [criar uma análise de acesso](create-access-review.md) para membros do grupo ou os usuários atribuídos a um aplicativo. O Azure AD envia automaticamente, para os revisores, um email solicitando que analisem o acesso. Se um usuário não recebeu um email, você pode enviar para ele as instruções em [revisar o acesso a grupos ou aplicativos](perform-access-review.md). (Observe que os convidados que forem designados como revisores, mas não aceitarem o convite, não receberão um email das revisões de acesso, pois eles devem primeiro aceitar um convite antes da revisão.) Depois que o período de análise de acesso tiver acabado ou se um administrador interromper a análise de acesso, siga as etapas deste artigo para ver e aplicar os resultados.

## <a name="view-an-access-review-in-the-azure-portal"></a>Exibir uma análise de acesso no Portal do Azure

1. Vá para a [página de revisões de acesso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), selecione **Programas** e escolha o programa que contém o controle de análise de acesso.

2. Selecione **Gerenciar** e selecione o controle de análise de acesso. Se houver muitos controles no programa, você poderá filtrar os controles de um tipo específico e classificar pelo status. Você também pode pesquisar pelo nome do controle da análise de acesso ou pelo nome de exibição do proprietário que o criou. 

## <a name="stop-a-review-that-hasnt-finished"></a>Parar uma análise que não foi concluída

Se a análise não tiver atingido a data de término agendada, um administrador poderá selecionar **Parar** para finalizar a análise mais cedo. Depois que a análise for interrompida, os usuários não poderão ser analisados. Não é possível reiniciar uma análise depois de ter sido interrompida.

## <a name="apply-the-changes"></a>Aplicar as alterações 

Depois que uma revisão de acesso for concluída, porque ela atingiu a data de término ou um administrador a interrompeu manualmente e a aplicação automática não foi configurada para a revisão, você poderá selecionar**Aplicar** para aplicar manualmente as alterações. O resultado da análise é implementado ao atualizar o grupo ou o aplicativo. Se o acesso do usuário tiver sido negado na análise, quando o administrador selecionar esta opção, o Azure AD removerá sua atribuição de associação ou aplicativo. 

Depois que uma revisão de acesso for concluída e a aplicação automática tiver sido configurada, o status da revisão será alterado de Concluído para estados intermediários e, finalmente, será alterado para o estado Aplicado. É necessário esperar que os usuários negados, se houver algum, sejam removidos da associação do grupo de recursos ou da atribuição de aplicativo em alguns minutos.

Uma revisão de aplicação automática configurada ou selecionar **Aplicar** não afeta um grupo originado em um diretório local ou em um grupo dinâmico. Se você quiser alterar um grupo que se origina localmente, baixe os resultados e aplique essas alterações para a representação do grupo neste diretório.

## <a name="download-the-results-of-the-review"></a>Baixar os resultados da análise

Para recuperar os resultados da análise, selecione **Aprovações** e, em seguida, selecione **Baixar**. O arquivo CSV resultante pode ser visualizado no Excel ou em outros programas que abrem arquivos CSV codificados em UTF-8.

## <a name="optional-delete-a-review"></a>Opcional: Excluir uma revisão
Caso não esteja mais interessado na análise, você poderá excluí-la. Selecione **Excluir** para remover a análise do Azure AD.

> [!IMPORTANT]
> Não há nenhum aviso antes da exclusão, portanto, certifique-se de que deseja excluir a análise.
> 
> 

## <a name="next-steps"></a>Próximas etapas

- [Gerenciar o acesso do usuário com revisões de acesso ao Azure AD](manage-user-access-with-access-reviews.md)
- [Gerenciar o acesso de convidado com revisões de acesso do Azure AD](manage-guest-access-with-access-reviews.md)
- [Gerenciar os programas e os controles para análises de acesso do Azure AD](manage-programs-controls.md)
- [Criar uma revisão de acesso de grupos ou aplicativos](create-access-review.md)
- [Criar uma revisão de acesso de usuários em uma função administrativa do Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md)
