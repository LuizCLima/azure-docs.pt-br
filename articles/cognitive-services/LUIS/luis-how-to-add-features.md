---
title: Listas de frase
titleSuffix: Language Understanding - Azure Cognitive Services
description: Use o LUIS (Serviço Inteligente de Reconhecimento Vocal) para adicionar recursos de aplicativos que podem melhorar a detecção ou previsão de intenções e entidades que as categorias e padrões
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/16/2019
ms.author: diberry
ms.openlocfilehash: 0723c3730ca0ae6325d828fbb5f41698cb807dd3
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56874878"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>Usar listas de frases para melhorar o sinal da lista de palavras

É possível adicionar recursos ao aplicativo de LUIS para melhorar a precisão. Os recursos ajudam o LUIS fornecendo dicas de que certas palavras e frases fazem parte de um vocabulário do domínio do aplicativo. 

Uma [lista de frases](luis-concept-feature.md) inclui um grupo de valores (palavras ou frases) que pertencem à mesma classe e devem ser tratadas de maneira semelhante (por exemplo, nomes de cidades ou produtos). O que o LUIS aprende sobre um deles é aplicado automaticamente aos outros também. Essa lista não é uma entidade de lista (correspondências exatas de texto) de palavras correspondentes.

Uma lista de frase adiciona ao vocabulário do domínio de aplicativo como um segundo sinal ao LUIS sobre essas palavras.

## <a name="add-phrase-list"></a>Adicionar lista de frases

O LUIS permite até 10 listas de frase por aplicativo. 

1. Abra o aplicativo, clicando no nome do aplicativo na página **Meus Aplicativos**, clique em **Compilar** e, em seguida, clique em **Listas de frases** no painel esquerdo do seu aplicativo. 

2. Na página **Lista de frases**, clique em **Criar nova lista de frases**. 
 
3. Na caixa de diálogo **Adicionar Lista de Frases**, digite "Cidades" como o nome da lista de frases. Na caixa de **Valor**, digite os valores da lista de frases. É possível digitar um valor por vez ou um conjunto de valores separados por vírgulas e, em seguida, pressionar **Enter**.

    ![Adicionar lista de frases Cidades](./media/luis-add-features/add-phrase-list-cities.png)

4. O LUIS pode propor valores relacionados para adicionar à lista de frases. Clique em **Recomendado** para obter um grupo de valores propostos relacionados semanticamente com o(s) valor(es) adicionado(s). É possível clicar em qualquer um dos valores propostos ou clicar em **Adicionar Todos** para adicionar a todos.

    ![Valores propostos à lista de frases – adicionar todos](./media/luis-add-features/related-values.png)

5. Clique em **Esses valores são intercambiáveis**, se os valores da lista de frases adicionados forem alternativas que podem ser usadas de forma intercambiável.

    ![Valores propostos à lista de frases – caixa selecionar de modo intercambiável](./media/luis-add-features/interchangeable.png)

6. Clique em **Salvar**. A lista de frases "Cidades" é adicionada à página **Listas de Frases**.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> É possível excluir ou desativar uma lista de frases na barra de ferramentas contextual na página **Listas de frases**.

## <a name="next-steps"></a>Próximas etapas

Após adicionar, editar, excluir ou desativar uma lista de frases, [treine e teste o aplicativo](luis-interactive-test.md) novamente para verificar se o desempenho melhora.
