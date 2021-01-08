---
title: MIM 2016 configureren voor Privileged Access Management | Microsoft Docs
description: Het overzicht voor het installeren van MIM en de configuratie voor Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 64ed3184ee38102f488c61bf0b41bc0fec34b049
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010604"
---
# <a name="configure-the-mim-environment-for-privileged-access-management"></a>De MIM-omgeving voor Privileged Access Management configureren

Er zijn zeven stappen die u moet voltooien om de omgeving in stellen voor forest-overschrijdende toegang, Active Directory en Microsoft Identity Manager te installeren en configureren en een Just-In-Time toegangsaanvraag te demonstreren.

Deze stappen zijn zo opgezet dat u helemaal opnieuw begint met het maken van een testomgeving. Als u PAM toepast op een bestaande omgeving, kunt u uw eigen domein controllers of gebruikers accounts voor het *CONTOSO* -domein gebruiken, in plaats van nieuwe te maken die overeenkomen met de voor beelden.

1. Bereid de *CORPDC*-server voor als een domeincontroller en *CORPWKSTN* als een lidwerkstation.

2. Bereid de *PRIVDC* server voor als een domeincontroller.

3.  Bereid de *PAMSRV*-server voor in het *PRIV* forest.

4.  Installeer de MIM-onderdelen op *PAMSRV* en de cmdlets op een lidwerkstation van het *CONTOSO*-forest en bereid ze voor Privileged Access Management voor.

5.  Stel een vertrouwensrelatie in tussen het *PRIV*- en *CONTOSO*-forest.

6.  Bereid bevoorrechte beveiligingsgroepen met toegang tot beveiligde resources en ledenaccounts voor Just-In-Time Privileged Access Management voor.

7.  Demonstreer het aanvragen, ontvangen en gebruiken van bevoorrechte toegang met verhoogde bevoegdheid tot een beveiligde resource.

> [!div class="step-by-step"]
> [Start »](step-1-prepare-corp-domain.md)
