---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 12/10/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 91889971e1ab8a9ea8341f6bc57735d973ea0e89
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58115556"
---
## <a name="launch-azure-cloud-shell"></a>Iniciar o Azure Cloud Shell

O Azure Cloud Shell é um shell interativo grátis que pode ser usado para executar as etapas neste artigo. Ele tem ferramentas do Azure instaladas e configuradas para usar com sua conta. 

Para abrir o Cloud Shell, basta selecionar **Experimentar** no canto superior direito de um bloco de código. Você também pode iniciar o Cloud Shell em uma guia separada do navegador indo até [https://shell.azure.com/powershell](https://shell.azure.com/powershell). Selecione **Copiar** para copiar os blocos de código, cole o código no Cloud Shell e depois pressione Enter para executá-lo.


## <a name="preview-register-the-feature"></a>Visualização: Registrar o recurso

As Galerias de Imagens Compartilhadas estão na versão prévia, mas você precisa registrar o recurso antes que possa usá-lo. Para registrar o recurso de Galerias de Imagens Compartilhadas:

```azurepowershell-interactive
Register-AzProviderFeature `
   -FeatureName GalleryPreview `
   -ProviderNamespace Microsoft.Compute
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="get-the-managed-image"></a>Obter a imagem gerenciada

Você pode ver uma lista de imagens que estão disponíveis em um grupo de recursos usando [Get-AzImage](https://docs.microsoft.com/powershell/module/az.compute/get-azimage). Depois que você souber o nome da imagem e em qual grupo de recursos ela está, poderá usar `Get-AzImage` novamente para obter o objeto de imagem e armazená-lo em uma variável para uso posterior. Este exemplo obtém uma imagem chamada *myImage* do grupo de recursos "myResourceGroup" e o atribui à variável *$managedImage*. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```

## <a name="create-an-image-gallery"></a>Criar uma galeria de imagens 

Uma galeria de imagens é o principal recurso usado para habilitar o compartilhamento de imagens. Os nomes das galerias devem ser exclusivos dentro de sua assinatura. Crie uma galeria de imagens usando [New-AzGallery](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery). O exemplo a seguir cria uma galeria chamada *myGallery* no grupo de recursos *myGalleryRG*.

```azurepowershell-interactive
$resourceGroup = New-AzResourceGroup `
   -Name 'myGalleryRG' `
   -Location 'West Central US'  
$gallery = New-AzGallery `
   -GalleryName 'myGallery' `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -Description 'Shared Image Gallery for my organization'  
```
   
## <a name="create-an-image-definition"></a>Criar uma definição de imagem 

Crie a definição de imagem da galeria usando [New-AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). Neste exemplo, a imagem da galeria é denominada *myGalleryImage*.

```azurepowershell-interactive
$galleryImage = New-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $gallery.Location `
   -Name 'myImageDefinition' `
   -OsState generalized `
   -OsType Windows `
   -Publisher 'myPublisher' `
   -Offer 'myOffer' `
   -Sku 'mySKU'
```
### <a name="using-publisher-offer-and-sku"></a>Usando o Editor, oferta e SKU 
Para os clientes a planejar como implementar imagens compartilhadas, **em uma versão futura**, você poderá usar seu pessoal definido **-Publisher**, **-oferecem** e **- Sku** valores para localizar e especificar uma definição de imagem, criar uma VM usando a versão mais recente da imagem de correspondência de definição de imagem. Por exemplo, aqui estão três definições de imagem e seus valores:

|Definição de imagem|Publicador|Oferta|Sku|
|---|---|---|---|
|myImage1|myPublisher|myOffer|mySku|
|myImage2|myPublisher|standardOffer|mySku|
|myImage3|Testando|standardOffer|testSku|

Todos os três têm conjuntos exclusivos de valores. Você pode ter versões de imagem que compartilham um ou dois, mas não todos os três valores. **Em uma versão futura**, você poderá combinar esses valores para solicitar a versão mais recente de uma imagem específica. **Isso não funcionar na versão atual**, mas estarão disponíveis no futuro. Quando lançada, usando a sintaxe a seguir deve ser usado para definir a imagem de origem como *myImage1* da tabela acima.

```powershell
$vmConfig = Set-AzVMSourceImage `
   -VM $vmConfig `
   -PublisherName myPublisher `
   -Offer myOffer `
   -Skus mySku 
```

Isso é semelhante a como você pode especificar atualmente os uso Editor, oferta e SKU para [imagens do Azure Marketplace](../articles/virtual-machines/windows/cli-ps-findimage.md) para obter a versão mais recente de uma imagem do Marketplace. Com isso em mente, cada definição de imagem deve ter um conjunto exclusivo desses valores.  

## <a name="create-an-image-version"></a>Criar uma versão de imagem

Crie uma versão de imagem a partir de uma imagem gerenciada usando [New-AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). Neste exemplo, a versão da imagem é *1.0.0* e ela é replicado para os datacenters *Centro-Oeste dos EUA* e *Centro-Sul dos EUA*.


```azurepowershell-interactive
$region1 = @{Name='South Central US';ReplicaCount=1}
$region2 = @{Name='West Central US';ReplicaCount=2}
$targetRegions = @($region1,$region2)
$job = $imageVersion = New-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryImageVersionName '1.0.0' `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -TargetRegion $targetRegions  `
   -Source $managedImage.Id.ToString() `
   -PublishingProfileEndOfLifeDate '2020-01-01' `
   -asJob 
```

Pode levar um tempo para replicar a imagem para todas as regiões de destino, portanto, criamos um trabalho para podermos acompanhar o progresso. Para ver o andamento do trabalho, digite `$job.State`.

```azurepowershell-interactive
$job.State
```

