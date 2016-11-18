---
title: PAM implementeren en configureren | Microsoft Docs
description: Het overzicht voor het installeren van MIM en de configuratie voor Privileged Access Management.
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: a081b49ca8d0de7ce7d5f7385e5a652b09b722c3


---

# <a name="configure-the-mim-environment-for-privileged-access-management"></a>De MIM-omgeving voor Privileged Access Management configureren
Er zijn zeven stappen die u moet voltooien om de omgeving in stellen voor forest-overschrijdende toegang, Active Directory en Microsoft Identity Manager te installeren en configureren en een Just-In-Time toegangsaanvraag te demonstreren.

Deze stappen zijn zo opgezet dat u helemaal opnieuw begint met het maken van een testomgeving. Als u PAM op een bestaande omgeving toepast, kunt u uw eigen domeincontrollers of gebruikersaccounts gebruiken in plaats van nieuwe items te maken die overeenkomen met de voorbeelden.

1.  Bereid de *CORPDC*-server voor als een domeincontroller en *CORPWKSTN* als een lidwerkstation.

2.  Bereid de *PRIVDC*server voor als een domeincontroller.

3.  Bereid de *PAMSRV*-server voor in het *PRIV*forest.

4.  Installeer de MIM-onderdelen op *PAMSRV* en de cmdlets op een lidwerkstation van het *CONTOSO*-forest en bereid ze voor Privileged Access Management voor.

5.  Stel een vertrouwensrelatie in tussen het *PRIV*- en *CONTOSO*-forest.

6.  Bereid bevoorrechte beveiligingsgroepen met toegang tot beveiligde resources en ledenaccounts voor Just-In-Time Privileged Access Management voor.

7.  Demonstreer het aanvragen, ontvangen en gebruiken van bevoorrechte toegang met verhoogde bevoegdheid tot een beveiligde resource.

>[!div class="step-by-step"]
[Start »](step-1-prepare-corp-domain.md)



<!--HONumber=Nov16_HO2-->


