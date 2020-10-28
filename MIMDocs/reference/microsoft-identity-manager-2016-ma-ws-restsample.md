---
title: REST API App Service-voor beeld van web service-connector | Microsoft Docs
description: Hulp bij het implementeren van een voor beeld van een JSON-server in azure
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/28/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: deb743fcbe4bdd155c1b0c4a31e24af0e8da8a4e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758917"
---
# <a name="web-service-connector-rest-api-app-service-sample"></a>REST API App Service-voor beeld van web service-connector

Deze implementatie handleiding helpt u bij het implementeren van de voor beeld-REST JSON-server naar Azure. U kunt dit voor beeld gebruiken om u te helpen bij de configuratie en het memorandum van de web service-connector.

- Voor het voor beeld is micro soft Visual Studio 2017 vereist.
- De NMP-pakketten (native module pad) in het voor beeld gebruiken deze [JSON-server](https://github.com/typicode/JSON-server) op github.
- Down load de [voorbeeld code](https://github.com/fimguy/SAMPLEREST) van github en implementeer de voorbeeld code in azure app service.

## <a name="deploy-the-json-server-sample"></a>Het JSON server-voor beeld implementeren

1. Nadat de code is gedownload, opent u het oplossings bestand in [Visual Studio 2017](https://www.visualstudio.com/downloads/).

2. Implementeer de oplossing door het project te selecteren en vervolgens met de rechter muisknop te klikken en **publiceren** te selecteren:

    ![De oplossing publiceren](media/microsoft-identity-manager-2016-ma-ws-restsample/publish-project.png)

3. Selecteer de App Service die u voor de implementatie wilt gebruiken:

    ![Selecteer de App Service](media/microsoft-identity-manager-2016-ma-ws-restsample/app-service.png)

4. Selecteer een bestaande resource groep of maak een nieuwe resource groep:

    ![Een resource groep selecteren](media/microsoft-identity-manager-2016-ma-ws-restsample/resource-group.png)

5. Gebruik de bestaande namen voor de App Service en selecteer **maken** :

    ![De App Service maken](media/microsoft-identity-manager-2016-ma-ws-restsample/create.png)

    De App Service wordt gemaakt.

6. Als u de App Service wilt publiceren, selecteert u **publiceren** :

    ![De App Service publiceren](media/microsoft-identity-manager-2016-ma-ws-restsample/publish.png)

7. Nadat de App Service is gepubliceerd, wordt het REST API voor beeld en de bijbehorende website gestart in de standaard browser:

    ![Voor beeld REST API en website](media/microsoft-identity-manager-2016-ma-ws-restsample/sample-rest-api.png)

U kunt de implementatie nu configureren, zoals beschreven in de [hand leiding](microsoft-identity-manager-2016-ma-ws-restgeneric.md)voor de rest-implementatie.


## <a name="modify-the-sample"></a>Het voor beeld wijzigen

Als u de JSON-gegevens en het REST API-voor beeld wilt wijzigen, brengt u de wijzigingen aan in de **db.JSin** het bestand en werkt u de implementatie bij:

![De db.JSbijwerken voor het bestand](media/microsoft-identity-manager-2016-ma-ws-restsample/db-json.png)


## <a name="next-steps"></a>Volgende stappen

- [Overzicht van de algemene webservice-connector](microsoft-identity-manager-2016-ma-ws.md)
- [Installeer het hulp programma voor configuratie van webservices](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP-implementatie handleiding](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Hand leiding voor REST-implementatie](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuratie van de web service-MA](microsoft-identity-manager-2016-ma-ws-maconfig.md)
