---
title: O que são Reservas do Azure? | Microsoft Docs
description: Saiba mais sobre as Reservas do Azure e os preços para economizar nos custos de máquinas virtuais, de bancos de dados SQL, do Azure Cosmos DB e de outros recursos.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: banders
ms.openlocfilehash: 1349a05e1dd235c7b375335ae2c9fed16170a61f
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58649383"
---
# <a name="what-are-azure-reservations"></a>O que são Reservas do Azure?

As Reservas do Azure ajudam a economizar pagando previamente por um ano ou três anos de máquina virtual, de capacidade de computação do Banco de Dados SQL, de produtividade do Azure Cosmos DB ou de outros recursos do Azure. Pagar previamente permite que você obtenha um desconto nos recursos que você usar. As reservas podem reduzir significativamente os custos com máquinas virtuais, computação do Banco de Dados SQL, Azure Cosmos DB ou outros recursos em até 72% nos preços pagos conforme o uso. As reservas fornecem um desconto de cobrança e não afetam o estado de tempo de execução dos recursos.

Você pode comprar uma instância reservada no [portal do Azure](https://aka.ms/reservations). Para obter mais informações, consulte os seguintes artigos:

Planos de serviço:
- [Máquinas virtuais com instâncias de VM reservadas do Azure](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Capacidade reservada de recursos do BD Cosmos do Azure com o Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md)
- [Capacidade reservada de recursos de computação de banco de dados SQL com o banco de dados SQL](../sql-database/sql-database-reserved-capacity.md)

Planos de software:
- [Planos de software Red Hat das reservas do Azure](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Planos de software do SUSE das reservas do Azure](../virtual-machines/linux/prepay-suse-software-charges.md)

## <a name="why-buy-a-reservation"></a>Por que comprar uma reserva?

Se você tiver máquinas virtuais, Azure Cosmos DB ou bancos de dados SQL executadas por longos períodos de tempo, comprar uma reserva dá a opção mais econômica. Por exemplo, quando você executa continuamente quatro instâncias de um serviço sem uma reserva, você será cobrado com taxas pré-pagas. Se você comprar uma reserva para esses recursos, você obtém imediatamente o desconto de reserva. Os recursos não serão mais cobrados com base nas taxas pagas conforme o uso.

## <a name="charges-covered-by-reservation"></a>Encargos cobertos por reserva

Planos de serviço:

- Instância de Máquina Virtual Reservada: Uma reserva cobre apenas os custos de computação da máquina virtual. Não cobre encargos adicionais de software, rede e armazenamento.
- Capacidade reservada do Azure Cosmos DB: Uma reserva abrange uma taxa de transferência provisionada para os seus recursos. Ela não cobre encargos de armazenamento e rede.
- vCore reservado do Banco de Dados SQL: Apenas os custos de computação são incluídos em uma reserva. A licença é cobrada separadamente.
- Capacidade reservada do Azure Cosmos DB: Uma reserva cobre a produtividade provisionada para seus recursos, mas não cobre encargos de rede e armazenamento.

Para as máquinas virtuais do Windows e banco de Dados SQL, você pode cobrir os custos de licenciamento com o [Benefício Híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>Quem é elegível para comprar uma reserva?

Para comprar um plano, você deve ter uma função de proprietário de assinatura em um Enterprise (MS-AZR - 0017p ou MS-AZR - 0148p) ou uma assinatura pré-paga (MS-AZR - 003p ou MS-AZR - 0023P). Provedores de soluções de nuvem podem usar o portal do Azure ou [Partner Center](/partner-center/azure-reservations) para comprar reservas do Azure.

Os clientes do EA podem limitar as compras para administradores EA desabilitando o **adicionar instâncias reservadas** opção no Portal EA. Os administradores EA devem ser um proprietário da assinatura pelo menos uma assinatura de EA para comprar uma reserva. A opção é útil para empresas que querem uma equipe centralizada para comprar reservas para centros de custo diferentes. Após a compra, equipes centralizadas podem adicionar os proprietários do Centro de custo para as reservas. Proprietários, em seguida, podem definir o escopo de reserva para suas assinaturas. A equipe central não precisa ter acesso de proprietário de assinatura em que a reserva é adquirida.

Um desconto de reserva aplica-se apenas aos recursos associados aos tipos de assinatura Enterprise, Pagamento Conforme o Uso ou CSP.

## <a name="how-is-a-reservation-billed"></a>Como uma reserva é cobrada?

A reserva é cobrada no método de pagamento associado à assinatura. Se você tiver uma assinatura do Enterprise, o custo de reserva será deduzido do seu saldo de investimento. Se o saldo de investimento não cobrir o custo da reserva, você será cobrado pelo excedente. Se você tiver uma assinatura paga conforme o uso, será cobrado imediatamente do seu cartão de crédito. Se você for cobrado por fatura, verá os encargos na sua próxima fatura.

## <a name="how-reservation-discount-is-applied"></a>Como o desconto de reserva é aplicado

O desconto de reserva se aplica para o uso de recursos que correspondem aos atributos que você seleciona ao comprar a reserva. Os atributos incluem o escopo em que as VMs, os banco de dados SQL, o Azure Cosmos DB ou outros recursos correspondentes são executados. Por exemplo, caso deseje obter um desconto de reserva para quatro máquinas virtuais Standard D2 na região Oeste dos EUA, selecione a assinatura na qual as VMs estão em execução. Se as máquinas virtuais são executadas em diferentes assinaturas em sua conta ou registro, então, selecione o escopo como compartilhado. O escopo compartilhado permite que o desconto de reserva seja aplicado entre assinaturas. Você pode alterar o escopo depois de comprar uma reserva. Para obter mais informações, consulte [Gerenciar Reservas do Azure](billing-manage-reserved-vm-instance.md).

Um desconto de reserva aplica-se apenas aos recursos associados aos tipos de assinatura Enterprise, Pagamento Conforme o Uso ou CSP. Os recursos executados em uma assinatura com outros tipos de oferta não recebem o desconto de reserva.

Para entender melhor como reservas afeta sua cobrança, consulte os seguintes artigos:

Planos de serviço:

- [Compreender o desconto de Instâncias de VM Reservadas do Azure](billing-understand-vm-reservation-charges.md)
- [Compreender o desconto de reserva do Azure](billing-understand-vm-reservation-charges.md)
- [Entender o desconto de reserva do Azure Cosmos DB](billing-understand-cosmosdb-reservation-charges.md)

Planos de software:

- [Entenda o desconto de reserva do Azure e o uso do Red Hat](billing-understand-rhel-reservation-charges.md)
- [Entender o desconto de reserva do Azure e o uso do SUSE](billing-understand-suse-reservation-charges.md)

## <a name="when-the-reservation-term-expires"></a>Quando o prazo de reserva expira

No final do prazo de reserva, o desconto de cobrança expira e a máquina virtual, o Banco de Dados SQL, o Azure Cosmos DB ou outro recurso é cobrado pelo preço pago conforme o uso. As Reservas do Azure não são renovadas automaticamente. Para continuar recebendo o desconto de cobrança, você deve comprar uma nova reserva para serviços e softwares elegíveis.

## <a name="discount-applies-to-different-sizes"></a>Desconto se aplica às tamanhos diferentes

Quando você compra uma reserva, o desconto pode ser aplicado a outras instâncias com atributos que estão dentro do mesmo grupo de tamanho. Esse recurso é conhecido como a flexibilidade de tamanho de instância. A flexibilidade da cobertura de desconto depende do tipo de reserva e dos atributos que você escolhe quando compra a reserva.

Planos de serviço:

- Instâncias de VM reservadas: Quando você comprar a reserva e seleciona **otimizado para**: **flexibilidade de tamanho de instância**, a cobertura de desconto depende do tamanho da VM que você selecionar. A reserva pode ser aplicada aos tamanhos de VMs (máquinas virtuais) no mesmo grupo de série de tamanho. Para obter mais informações, confira [Flexibilidade de tamanho de máquina virtual com Instâncias de VM Reservadas](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- Capacidade reservada do Banco de Dados SQL: A cobertura de desconto depende do nível de desempenho escolhido. Para obter mais informações, confira [Entender como um desconto de reserva do Azure é aplicado](billing-understand-reservation-charges.md).
- Capacidade reservada do Azure Cosmos DB: A cobertura de desconto depende da taxa de transferência provisionada. Para obter mais informações, confira [Entender como um desconto de reserva do Azure Cosmos DB é aplicado](billing-understand-cosmosdb-reservation-charges.md).

## <a name="need-help-contact-us"></a>Precisa de ajuda? Entre em contato conosco.

Se você tiver dúvidas ou precisar de Ajuda, [criar uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas

- Comece a economizar em suas máquinas virtuais comprando uma [Instância de VM Reservada](../virtual-machines/windows/prepay-reserved-vm-instances.md), uma [capacidade reservada do Banco de Dados SQL](../sql-database/sql-database-reserved-capacity.md) ou uma [capacidade reservada do Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md).
- Saiba mais sobre as Reservas do Azure com os seguintes artigos:
    - [Gerenciar Reservas do Azure](billing-manage-reserved-vm-instance.md)
    - [Entender o uso de reserva para a sua assinatura paga conforme o uso](billing-understand-reserved-instance-usage.md)
    - [Entender o uso de reserva para seu registro de empresa](billing-understand-reserved-instance-usage-ea.md)
    - [Custos de software do Windows não estão incluídos nas reservas](billing-reserved-instance-windows-software-costs.md)
    - [Reservas do Azure no programa de CSP (Provedor de Soluções na Nuvem) do Partner Center](https://docs.microsoft.com/partner-center/azure-reservations)
