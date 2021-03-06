---
title: 'Depurar o Apache Hadoop: exibir logs e interpretar mensagens de erro – Azure HDInsight'
description: Saiba quais são as mensagens de erro que você pode receber ao administrar o HDInsight usando o PowerShell, e as etapas que podem ser executadas para recuperação.
services: hdinsight
ms.reviewer: jasonh
author: ashishthaps
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: a035789af08aa4c0d877a06295d9bd6fdedf6844
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449496"
---
# <a name="analyze-apache-hadoop-logs"></a>Analisar logs do Apache Hadoop

Cada cluster Apache Hadoop no Azure HDInsight tem uma conta de armazenamento do Azure usada como o sistema de arquivos padrão. Essa conta de armazenamento é conhecida como a Conta de armazenamento padrão. O cluster usa o Armazenamento de Tabelas do Azure e o Armazenamento de Blobs na conta de armazenamento padrão para armazenar seus logs.  Para descobrir a conta de armazenamento padrão para o cluster, confira [Manage Apache Hadoop clusters in HDInsight](../hdinsight-administer-use-portal-linux.md#find-the-storage-accounts) (Gerenciar clusters do Apache Hadoop no HDInsight). Os logs são mantidos na Conta de armazenamento mesmo após a exclusão do cluster.

## <a name="logs-written-to-azure-tables"></a>Logs gravados em Tabelas do Azure

Os logs gravados em Tabelas do Azure fornecem informações sobre o que está acontecendo com um cluster HDInsight.

Quando você cria um cluster HDInsight, seis tabelas são criadas automaticamente para clusters baseados em Linux no Armazenamento de Tabelas padrão:

* hdinsightagentlog
* syslog
* daemonlog
* hadoopservicelog
* ambariserverlog
* ambariagentlog

Os nomes de arquivo da tabela são **u<ClusterName>DDMonYYYYatHHMMSSsss<TableName>**.

Essas tabelas contêm os seguintes campos:

* ClusterDnsName
* ComponentName
* EventTimestamp
* Host
* MALoggingHash
* Mensagem
* N
* PreciseTimeStamp
* Função
* RowIndex
* Locatário
* TIMESTAMP
* TraceLevel

### <a name="tools-for-accessing-the-logs"></a>Ferramentas para acessar os logs
Há muitas ferramentas disponíveis para acesso dos dados nessas tabelas:

* Visual Studio
* Gerenciador de Armazenamento do Azure
* Power Query para Excel

#### <a name="use-power-query-for-excel"></a>Usar o Power Query para Excel
Power Query pode ser instalado do [Microsoft Power Query para Excel](https://www.microsoft.com/en-us/download/details.aspx?id=39379). Consulte a página de download para saber os requisitos do sistema.

**Use o Power Query para abrir e analisar o log do serviço**

1. Abra o **Microsoft Excel**.
2. No menu **Power Query**, clique em **No Azure**, em seguida, clique em **No armazenamento de Tabelas do Microsoft Azure**.
   
    ![HDInsight Hadoop Excel PowerQuery abrir o Armazenamento de Tabelas do Azure](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-open.png)
3. Insira o nome de conta de armazenamento, o nome curto ou o FQDN.
4. Insira a chave da conta de armazenamento. Você verá uma lista de tabelas:
   
    ![Logs do HDInsight Hadoop armazenados no Armazenamento de Tabelas do Azure](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-table-names.png)
5. Clique com o botão direito na tabela hadoopservicelog no painel **Navigator** e selecione **Editar**. Você deverá ver quatro colunas. Opcionalmente, exclua as colunas **Chave de Partição**, **Chave de Linha** e **Carimbo de Data/Hora** selecionando-as e clicando em **Remover Colunas** nas opções na faixa de opções.
6. Clique no ícone de expansão na coluna Conteúdo para escolher as colunas que você deseja importar para a planilha do Excel. Para esta demonstração, escolhi TraceLevel e ComponentName: ela pode oferecer informações básicas sobre quais componentes apresentaram problemas.
   
    ![Logs do HDInsight Hadoop escolher colunas](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-filter.png)
7. Clique em **OK** para importar os dados.
8. Selecione as colunas **NívelRastreamento**, Função e **NomeComponente**, e clique no controle **Agrupar Por** na faixa de opções.
9. Clique em **OK** na caixa de diálogo Agrupar por
10. Clique em **Aplicar e Fechar**.

Agora você pode usar o Excel para filtrar e classificar conforme o necessário. Convém incluir outras colunas (por exemplo, Mensagem) para analisar problemas quando eles ocorrerem. Porém, a seleção e agrupamento das colunas descritas acima fornece uma imagem decente do que está acontecendo com os serviços do Hadoop. A mesma ideia pode ser aplicada às tabelas setuplog e hadoopinstalllog.

#### <a name="use-visual-studio"></a>Usar o Virtual Studio
**Usar o Virtual Studio**

1. Abra o Visual Studio.
2. No menu **Exibir**, clique em **Gerenciador de Nuvem**. Ou simplesmente clique em **CTRL+\, CTRL+X**.
3. No **Gerenciador de Nuvem**, selecione **Tipos de Recurso**.  A outra opção disponível é **Grupos de Recursos**.
4. Expanda as **Contas de Armazenamento**, a conta de armazenamento padrão do cluster, em seguida, **Tabelas**.
5. Clique duas vezes em **hadoopservicelog**.
6. Adicione um filtro. Por exemplo: 
   
        TraceLevel eq 'ERROR'
   
    ![Logs do HDInsight Hadoop escolher colunas](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-visual-studio-filter.png)
   
    Para saber mais sobre como construir filtros, consulte [Construir cadeias de caracteres de filtro para o Designer de Tabela](../../vs-azure-tools-table-designer-construct-filter-strings.md).

## <a name="logs-written-to-azure-blob-storage"></a>Logs gravados no Armazenamento de Blobs do Azure
Os logs gravados em Tabelas do Azure fornecem informações sobre o que está acontecendo com um cluster HDInsight. No entanto, essas tabelas não fornecem logs no nível da tarefa, que podem ser úteis para a análise mais detalhada dos problemas, quando eles ocorrerem. Para fornecer esse nível de detalhes, os clusters HDInsight são configurados para gravar logs de tarefas em sua conta de Armazenamento de Blobs para qualquer trabalho enviado por meio do Templeton. Na prática, isso significa os trabalhos enviados usando os cmdlets do Microsoft Azure PowerShell ou as APIs de envio de trabalho do .NET, não os trabalhos enviados por meio de acesso ao cluster por RDP/linha de comando. 

Para exibir os logs, confira [Acessar logs do aplicativo Apache Hadoop YARN no HDInsight baseado em Linux](../hdinsight-hadoop-access-yarn-app-logs-linux.md).


Para saber mais sobre os logs de aplicativos, confira [Simplifying user-logs management and access in Apache Hadoop YARN](https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/) (Como simplificar o gerenciamento de logs do usuário e o acesso no Apache Hadoop YARN).


## <a name="view-cluster-health-and-job-logs"></a>Exibir logs de trabalho e integridade do cluster
### <a name="access-the-ambari-ui"></a>Acessar a interface do usuário do Ambari
No portal do Azure, clique em um nome de cluster HDInsight para abrir o painel do cluster. No painel do cluster, clique em **Painel**.

![Iniciar painel do cluster](./media/apache-hadoop-debug-jobs/hdi-debug-launch-dashboard.png)


### <a name="access-the-yarn-ui"></a>Acessar a interface do usuário do Yarn
No portal do Azure, clique em um nome de cluster HDInsight para abrir o painel do cluster. No painel do cluster, clique em **Painel**. Quando solicitado, insira as credenciais de administrador do cluster. Em Ambari, selecione **YARN** da lista de serviços à esquerda da página. Na página que aparece, selecione **Links rápidos**, em seguida, selecione a entrada de nó principal ativo e o Gerenciador de Recursos de interface do usuário.

Você pode usar a interface do usuário do YARN para fazer o seguinte:

* **Obter o status do cluster**. No painel esquerdo, expanda **Cluster** e clique em **Sobre**. Isso apresenta detalhes de status do cluster como memória alocada total, núcleos usados, o estado do gerenciador de recursos de cluster, versão do cluster, e assim por adiante.
  
    ![Iniciar painel do cluster](./media/apache-hadoop-debug-jobs/hdi-debug-yarn-cluster-state.png)
* **Obtenha o status do nó**. No painel esquerdo, expanda **Cluster** e clique em **Nós**. Isso lista todos os nós no cluster, o endereço HTTP de cada nó, os recursos alocados para cada nó, etc.
* **Monitore o status do trabalho**. No painel esquerdo, expanda **Cluster**, em seguida, clique em **Aplicativos** para listar todos os trabalhos no cluster. Se você quiser examinar os trabalhos em um estado específico (como novo, enviado, em execução, etc.), clique no link apropriado em **Aplicativos**. Você pode seguir clicando no nome do trabalho para saber mais detalhes sobre ele, como saída, logs, etc.

### <a name="access-the-hbase-ui"></a>Acessar a interface do usuário do HBase
No portal do Azure, clique em um nome de cluster HBase do HDInsight para abrir o painel do cluster. No painel do cluster, clique em **Painel**. Quando solicitado, insira as credenciais de administrador do cluster. No Ambari, selecione HBase na lista de serviços. Selecione **Links rápidos** no topo da página, aponte para o link de nó ativo do Zookeeper e, em seguida, clique em Interface do usuário mestre HBase.

## <a name="hdinsight-error-codes"></a>Códigos de erro do HDInsight
As mensagens de erro detalhadas nesta seção são fornecidas para ajudar os usuários do Hadoop no Azure HDInsight a entenderem possíveis condições de erro que podem encontrar ao administrar o serviço usando o Azure PowerShell e para avisá-los sobre as etapas que podem ser executadas para se recuperar do erro.

Algumas dessas mensagens de erro também podem ser vistas no portal do Azure ao gerenciar clusters HDInsight. Mas outras mensagens de erro que você pode encontrar são menos granulares devido às restrições nas ações corretivas possíveis neste contexto. Outras mensagens de erro são fornecidas nos contextos onde a atenuação é óbvia. 

### <a id="AtLeastOneSqlMetastoreMustBeProvided"></a>AtLeastOneSqlMetastoreMustBeProvided
* **Descrição**: forneça os detalhes do Banco de Dados SQL do Azure para pelo menos um componente para usar as configurações personalizadas para as metastores do Hive e do Oozie.
* **Atenuação**: o usuário precisa fornecer um metastore válido do SQL Azure e repetir a solicitação.  

### <a id="AzureRegionNotSupported"></a>AzureRegionNotSupported
* **Descrição**: não foi possível criar o cluster na região *nameOfYourRegion*. Use uma região do HDInsight válida e repita a solicitação.
* **Atenuação**: o cliente deve criar a região do cluster compatível com ele no momento: Sudeste Asiático, Europa Ocidental, Europa Setentrional, Leste dos EUA ou Oeste dos EUA.  

### <a id="ClusterContainerRecordNotFound"></a>ClusterContainerRecordNotFound
* **Descrição**: o servidor não pôde localizar o registro do cluster solicitado.  
* **Atenuação**: Repita a operação.

### <a id="ClusterDnsNameInvalidReservedWord"></a>ClusterDnsNameInvalidReservedWord
* **Descrição**: o nome DNS do cluster *yourDnsName* é inválido. O nome deve começar e terminar com caracteres alfanuméricos e pode conter apenas o caractere especial '-'  
* **Atenuação**: verifique se você usou um nome DNS válido para seu cluster que comece e termine com alfanuméricos e que não contenha caracteres especiais além do traço '-' e, em seguida, repita a operação.

### <a id="ClusterNameUnavailable"></a>ClusterNameUnavailable
* **Descrição**: o nome do cluster *yourClusterName* não está disponível. Escolha outro nome.  
* **Atenuação**: o usuário deve especificar um clustername exclusivo e que não exista e repetir. Se o usuário estiver usando o Portal, a interface do usuário irá notificá-lo se um nome de cluster já estiver sendo usado durante as etapas de criação.

### <a id="ClusterPasswordInvalid"></a>ClusterPasswordInvalid
* **Descrição**: a senha do cluster é inválida. A senha deve ter, pelo menos, 10 caracteres e deve conter, no mínimo, um número, uma letra maiúscula, uma letra minúscula e um caractere especial sem espaços e não pode contar o nome do usuário.  
* **Atenuação**: forneça uma senha válida do cluster e repita a operação.

### <a id="ClusterUserNameInvalid"></a>ClusterUserNameInvalid
* **Descrição**: o nome de usuário do cluster é inválido. Verifique se o nome do usuário não contém caracteres especiais ou espaços.  
* **Atenuação**: forneça um nome de usuário válido do cluster e repita a operação.

### <a id="ClusterUserNameInvalidReservedWord"></a>ClusterUserNameInvalidReservedWord
* **Descrição**: o nome DNS do cluster *yourDnsClusterName* é inválido. O nome deve começar e terminar com caracteres alfanuméricos e pode conter apenas o caractere especial '-'  
* **Atenuação**: forneça um nome de usuário válido do cluster DNS e repita a operação.

### <a id="ContainerNameMisMatchWithDnsName"></a>ContainerNameMisMatchWithDnsName
* **Descrição**: o nome do contêiner no URI *yourcontainerURI* e o nome DNS *yourDnsName* no corpo da solicitação devem ser iguais.  
* **Atenuação**: verifique se o nome do contêiner e o seu nome DNS são os mesmos e repita a operação.

### <a id="DataNodeDefinitionNotFound"></a>DataNodeDefinitionNotFound
* **Descrição**: configuração do cluster inválida. Não é possível encontrar nenhuma definição de nó de dados no tamanho do nó.  
* **Atenuação**: Repita a operação.

### <a id="DeploymentDeletionFailure"></a>DeploymentDeletionFailure
* **Descrição**: falha na exclusão da implantação do cluster  
* **Atenuação**: repita a operação de exclusão.

### <a id="DnsMappingNotFound"></a>DnsMappingNotFound
* **Descrição**: Erro de configuração de serviço. Informações de mapeamento de DNS necessárias não encontradas.  
* **Atenuação**: exclua e crie um cluster.

### <a id="DuplicateClusterContainerRequest"></a>DuplicateClusterContainerRequest
* **Descrição**: tentativa de criação de contêiner de cluster duplicado. Existe um registro para *nameOfYourContainer* mas as Etags não coincidem.
* **Atenuação**: forneça um nome exclusivo para o contêiner e repita a operação de criação.

### <a id="DuplicateClusterInHostedService"></a>DuplicateClusterInHostedService
* **Descrição**: o serviço hospedado *nameOfYourHostedService* já contém um cluster. Um serviço hospedado não pode conter vários clusters  
* **Atenuação**: hospede o cluster em outro serviço hospedado.

### <a id="FailureToUpdateDeploymentStatus"></a>FailureToUpdateDeploymentStatus
* **Descrição**: o servidor não pôde atualizar o estado da implantação do cluster.  
* **Atenuação**: Repita a operação. Se isso acontecer várias vezes, entre em contato com o CSS.

### <a id="HdiRestoreClusterAltered"></a>HdiRestoreClusterAltered
* **Descrição**: o cluster *yourClusterName* foi excluído como parte da manutenção. Recrie o cluster.
* **Atenuação**: recrie o cluster.

### <a id="HeadNodeConfigNotFound"></a>HeadNodeConfigNotFound
* **Descrição**: configuração do cluster inválida. Configuração de nó de cabeçalho necessária não encontrada nos tamanhos de nós.
* **Atenuação**: Repita a operação.

### <a id="HostedServiceCreationFailure"></a>HostedServiceCreationFailure
* **Descrição**: não é possível criar o serviço hospedado *nameOfYourHostedService*. Tente novamente a solicitação.  
* **Atenuação**: Tente novamente a solicitação.

### <a id="HostedServiceHasProductionDeployment"></a>HostedServiceHasProductionDeployment
* **Descrição**: o serviço hospedado *nameOfYourHostedService* já contém uma implantação em produção. Um serviço hospedado não pode conter várias implantações em produção. Tente novamente a solicitação com um nome de cluster diferente.
* **Atenuação**: use outro nome de cluster e repita a solicitação.

### <a id="HostedServiceNotFound"></a>HostedServiceNotFound
* **Descrição**: o serviço hospedado *nameOfYourHostedService* do cluster não pôde ser encontrado.  
* **Atenuação**: se o cluster estiver em estado de erro, exclua-o e tente novamente.

### <a id="HostedServiceWithNoDeployment"></a>HostedServiceWithNoDeployment
* **Descrição**: o serviço hospedado *nameOfYourHostedService* não tem uma implantação associada.  
* **Atenuação**: se o cluster estiver em estado de erro, exclua-o e tente novamente.

### <a id="InsufficientResourcesCores"></a>InsufficientResourcesCores
* **Descrição**: uma SubscriptionId *yourSubscriptionId* não tem núcleos restantes para criar o cluster *yourClusterName*. Obrigatório: *resourcesRequired*, Disponível: *resourcesAvailable*.  
* **Atenuação**: libere recursos em sua assinatura ou aumente os recursos disponíveis para a assinatura e tente criar o cluster novamente.

### <a id="InsufficientResourcesHostedServices"></a>InsufficientResourcesHostedServices
* **Descrição**: a ID da Assinatura *yourSubscriptionId* não tem uma cota para um novo HostedService criar o cluster *yourClusterName*.  
* **Atenuação**: libere recursos em sua assinatura ou aumente os recursos disponíveis para a assinatura e tente criar o cluster novamente.

### <a id="InternalErrorRetryRequest"></a>InternalErrorRetryRequest
* **Descrição**: O servidor encontrou um erro interno. Tente novamente a solicitação.  
* **Atenuação**: Tente novamente a solicitação.

### <a id="InvalidAzureStorageLocation"></a>InvalidAzureStorageLocation
* **Descrição**: o local de armazenamento do Azure *dataRegionName* não é um local válido. Verifique se a região está correta e tente novamente a solicitação.
* **Atenuação**: selecione um local de armazenamento que dê suporte ao HDInsight, verifique se o cluster está colocalizado e repita a operação.

### <a id="InvalidNodeSizeForDataNode"></a>InvalidNodeSizeForDataNode
* **Descrição**: o tamanho da VM é inválido para os nós de dados. Somente o tamanho 'VM Grande' tem suporte para todos os nós de dados.  
* **Atenuação**: especifique o tamanho de nó com suporte para o nó de dados e repita a operação.

### <a id="InvalidNodeSizeForHeadNode"></a>InvalidNodeSizeForHeadNode
* **Descrição**: o tamanho da VM é inválido para o nó de cabeçalho. Apenas o tamanho 'VM Extragrande' tem suporte para o nó de cabeçalho.  
* **Atenuação**: especifique o tamanho de nó com suporte para o nó de cabeçalho e repita a operação

### <a id="InvalidRightsForDeploymentDeletion"></a>InvalidRightsForDeploymentDeletion
* **Descrição**: ID da Assinatura *yourSubscriptionId* sendo usada não tem permissões suficientes para executar a operação de exclusão do cluster *yourClusterName*.  
* **Atenuação**: se o cluster estiver em estado de erro, remova-o e tente novamente.  

### <a id="InvalidStorageAccountBlobContainerName"></a>InvalidStorageAccountBlobContainerName
* **Descrição**: o nome do contêiner de blob da conta de armazenamento externa *yourContainerName* é inválido. Verifique se o nome começa com uma letra e contém somente letras minúsculas, números e hífen.  
* **Atenuação**: especifique um nome de contêiner de blob da conta de armazenamento válido e repita a operação.

### <a id="InvalidStorageAccountConfigurationSecretKey"></a>InvalidStorageAccountConfigurationSecretKey
* **Descrição**: a configuração da conta de armazenamento externa *yourStorageAccountName* é necessária para que os detalhes da chave secreta sejam definidos.  
* **Atenuação**: especifique uma chave secreta válida para a conta de armazenamento e repita a operação.

### <a id="InvalidVersionHeaderFormat"></a>InvalidVersionHeaderFormat
* **Descrição**: a versão do cabeçalho *yourVersionHeader* não está no formato válido de aaaa-mm-dd.  
* **Atenuação**: especifique um formato válido para a versão do cabeçalho e repita a solicitação.

### <a id="MoreThanOneHeadNode"></a>MoreThanOneHeadNode
* **Descrição**: configuração do cluster inválida. Encontrada mais de uma configuração de nó de cabeçalho.  
* **Atenuação**: edite a configuração para que apenas um nó de cabeçalho seja especificado.

### <a id="OperationTimedOutRetryRequest"></a>OperationTimedOutRetryRequest
* **Descrição**: a operação não pôde ser concluída dentro do tempo permitido ou do número máximo de tentativas possível. Tente novamente a solicitação.  
* **Atenuação**: Tente novamente a solicitação.

### <a id="ParameterNullOrEmpty"></a>ParameterNullOrEmpty
* **Descrição**: o parâmetro *yourParameterName* não pode ser nulo ou vazio.  
* **Atenuação**: especifique um valor válido para o parâmetro.

### <a id="PreClusterCreationValidationFailure"></a>PreClusterCreationValidationFailure
* **Descrição**: uma ou mais das entradas se solicitação de criação de cluster não são válidas. Verifique se os valores de entrada estão corretos e tente novamente a solicitação.  
* **Atenuação**: Verifique se os valores de entrada estão corretos e tente novamente a solicitação.

### <a id="RegionCapabilityNotAvailable"></a>RegionCapabilityNotAvailable
* **Descrição**: a capacidade da região não está disponível para a região *yourRegionName* e a ID da Assinatura *yourSubscriptionId*.  
* **Atenuação**: especifique uma região que dê suporte a clusters HDInsight. As regiões com suporte público são: Sudeste Asiático, Europa Ocidental, Europa Setentrional, Leste dos EUA ou Oeste dos EUA.

### <a id="StorageAccountNotColocated"></a>StorageAccountNotColocated
* **Descrição**: a conta de armazenamento *yourStorageAccountName* está na região *currentRegionName*. Deveria ser igual à região do cluster *yourClusterRegionName*.  
* **Atenuação**: especifique uma conta de armazenamento na mesma região em que seu cluster está ou, se os seus dados já estiverem na conta de armazenamento, crie um cluster na mesma região da conta de armazenamento existente. Se você estiver usando o Portal, a interface do usuário irá notificá-lo sobre esse problema com antecedência.

### <a id="SubscriptionIdNotActive"></a>SubscriptionIdNotActive
* **Descrição**: a ID da assinatura *yourSubscriptionId* fornecida não está ativa.  
* **Atenuação**: reative sua assinatura ou obtenha uma nova assinatura válida.

### <a id="SubscriptionIdNotFound"></a>SubscriptionIdNotFound
* **Descrição**: a ID da assinatura *yourSubscriptionId* não pôde ser encontrada.  
* **Atenuação**: verifique se sua ID de assinatura é válida e repita a operação.

### <a id="UnableToResolveDNS"></a>UnableToResolveDNS
* **Descrição**: não é possível resolver o *yourDnsUrl* do DNS. Verifique se foi fornecida a URL totalmente qualificada do ponto de extremidade do blob.  
* **Atenuação**: forneça uma URL de blob válida. A URL DEVE ser totalmente válida, inclusive começando com *http://* e terminando em *.com*.

### <a id="UnableToVerifyLocationOfResource"></a>UnableToVerifyLocationOfResource
* **Descrição**: não é possível verificar a localização do recurso *yourDnsUrl*. Verifique se foi fornecida a URL totalmente qualificada do ponto de extremidade do blob.  
* **Atenuação**: forneça uma URL de blob válida. A URL DEVE ser totalmente válida, inclusive começando com *http://* e terminando em *.com*.

### <a id="VersionCapabilityNotAvailable"></a>VersionCapabilityNotAvailable
* **Descrição**: a capacidade de versão não está disponível para a versão *specifiedVersion* e a ID da Assinatura *yourSubscriptionId*.  
* **Atenuação**: escolha uma versão que esteja disponível e repita a operação.

### <a id="VersionNotSupported"></a>VersionNotSupported
* **Descrição**: não há suporte para a versão *specifiedVersion*.
* **Atenuação**: escolha uma versão que com suporte e repita a operação.

### <a id="VersionNotSupportedInRegion"></a>VersionNotSupportedInRegion
* **Descrição**: a versão *specifiedVersion* não está disponível na região do Azure *specifiedRegion*.  
* **Atenuação**: escolha uma versão com suporte na região especificada e repita a operação.

### <a id="WasbAccountConfigNotFound"></a>WasbAccountConfigNotFound
* **Descrição**: configuração do cluster inválida. Configuração da conta WASB necessária não encontrada em contas externas.  
* **Atenuação**: verifique se a conta existe e está corretamente especificada na configuração e repita a operação.

## <a name="next-steps"></a>Próximas etapas

* [Habilitar despejos de heap para serviços do Apache Hadoop no HDInsight baseado em Linux](../hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [Gerenciar clusters HDInsight usando a interface do usuário da Web do Apache Ambari](../hdinsight-hadoop-manage-ambari.md)
