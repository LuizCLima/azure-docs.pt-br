---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: 845b27b8efd66de87ec08e0b5b81bcc332dffdfb
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "58113868"
---
### <a name="prerequisites"></a>Pré-requisitos
* Uma conta do Twilio
* Um número de telefone Twilio verificado que possa receber SMS
* Um número de telefone Twilio verificado que possa enviar SMS

> [!NOTE]
> Se você estiver usando uma conta de avaliação do Twilio, só poderá enviar SMS para números de telefone **verificados**.  
> 
> 

Antes de usar sua conta do Twilio em um aplicativo lógico, você deve autorizar o aplicativo lógico a se conectar à sua conta do Twilio. Felizmente, você pode fazer isso de forma fácil usando seu aplicativo lógico no Portal do Azure. 

Aqui estão as etapas para autorizar seu aplicativo lógico a se conectar à sua conta do Twilio:

1. Para criar uma conexão com o Twilio, no designer do aplicativo lógico, selecione **Mostrar APIs gerenciadas da Microsoft** na lista suspensa e, em seguida, insira *Twilio* na caixa de pesquisa. Selecione o gatilho ou ação que gostaria de usar:   
   ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Se você não tiver criado quaisquer conexões Twilio antes, será solicitado a fornecer suas credenciais do Twilio. Essas credenciais serão usadas para autorizar seu aplicativo lógico a se conectar aos dados da sua conta do Twilio e usá-los:  
   ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. Você precisará da **ID da conta do Twilio** e do **token de acesso do Twilio** no painel do Twilio, então entre em sua conta do Twilio e pegue essas duas informações:  
   ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio e aplicativos lógicos usam nomes diferentes para identificar essas duas informações. Você deve mapeá-los para a caixa de diálogo de aplicativos lógicos da seguinte maneira: ![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Selecione o botão **Criar conexão**:  
   ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Observe que a conexão foi criada e agora você pode continuar com as outras etapas no seu aplicativo lógico:   
   ![](./media/connectors-create-api-twilio/twilio-5.png)

