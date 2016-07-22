---
title: De MIM-omgeving voor Privileged Access Management configureren | Microsoft Identity Manager
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: 9cf126d898c93faf89d7119136cce4e4963bb63d
ms.openlocfilehash: c9f2cf2ba1f42ea1513ae38d8089839d85ae5553


---

# De MIM-omgeving voor Privileged Access Management configureren
Er zijn zeven stappen die u moet voltooien om de omgeving in stellen voor forest-overschrijdende toegang, Active Directory en Microsoft Identity Manager te installeren en configureren en een Just-In-Time toegangsaanvraag te demonstreren.

Deze stappen zijn zo opgezet dat u helemaal opnieuw begint met het maken van een testomgeving. Als u PAM op een bestaande omgeving toepast, kunt u uw eigen domeincontrollers of gebruikersaccounts gebruiken in plaats van nieuwe items te maken die overeenkomen met de voorbeelden.

1.  Bereid de *CORPDC*-server voor als een domeincontroller en *CORPWKSTN* als een lidwerkstation.

2.  Bereid de *PRIVDC*server voor als een domeincontroller.

3.  Bereid de *PAMSRV*-server voor in het *PRIV*forest.

4.  Installeer de MIM-onderdelen op *PAMSRV* en de cmdlets op een lidwerkstation van het *CONTOSO*-forest en bereid ze voor Privileged Access Management voor.

5.  Stel een vertrouwensrelatie in tussen het *PRIV*- en *CONTOSO*-forest.

6.  Bereid bevoorrechte beveiligingsgroepen met toegang tot beveiligde resources en ledenaccounts voor Just-In-Time Privileged Access Management voor.

7.  Demonstreer het aanvragen, ontvangen en gebruiken van bevoorrechte toegang met verhoogde bevoegdheid tot een beveiligde resource.

>[!div class="step-by-step"] [Start »](step-1-prepare-corp-domain.md)



<!--HONumber=Jun16_HO3-->


