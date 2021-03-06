---
title: Criar um contêiner do serviço de nuvem com o PowerShell | Microsoft Docs
description: Este artigo explica como criar um contêiner do serviço de nuvem com o PowerShell. O contêiner hospeda funções da Web e de trabalho.
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: ''
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: 771f93edfee8f7b48fb7d0d2c98419f9427f6338
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793519"
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Usar um comando do Azure PowerShell para criar um contêiner de serviço de nuvem vazio

Este artigo explica como criar rapidamente um contêiner de Serviços de Nuvem usando cmdlets do Azure PowerShell. Siga as etapas abaixo:

1. Instale o cmdlet do Microsoft Azure PowerShell da página [Baixar o Azure PowerShell](https://aka.ms/webpi-azps) .
2. Abra o prompt de comando do PowerShell.
3. Use [Add-AzureAccount](/powershell/module/servicemanagement/azure/add-azureaccount?view=azuresmps-4.0.0) para entrar.

   > [!NOTE]
   > Para obter mais instruções sobre a instalação do cmdlet do Azure PowerShell e conectar-se à sua assinatura do Azure, consulte [Como instalar e configurar o Azure PowerShell](/powershell/azure/overview).
   >
   >
4. Use **New-AzureService** para criar um contêiner de serviço de nuvem do Azure vazio.

   ```
   New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```

5. Siga este exemplo para invocar o cmdlet:

   ```powershell
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Para obter mais informações sobre como criar o serviço de nuvem do Azure, execute:

```powershell
Get-help New-AzureService
```

### <a name="next-steps"></a>Próximas etapas

* Para gerenciar a implantação do serviço de nuvem, consulte os comandos [Get-AzureService](/powershell/module/servicemanagement/azure/Get-AzureService?view=azuresmps-4.0.0), [Remove-AzureService](/powershell/module/servicemanagement/azure/Remove-AzureService?view=azuresmps-4.0.0) e [Set-AzureService](/powershell/module/servicemanagement/azure/set-azureservice?view=azuresmps-4.0.0). Você também pode consultar [Como configurar os serviços de nuvem](cloud-services-how-to-configure-portal.md) para saber mais.
* Para publicar o seu projeto de serviço de nuvem para Azure, consulte o exemplo de código **PublishCloudService.ps1** do [repositório de serviços de nuvem arquivado](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Scripts/cloud-services-continuous-delivery).