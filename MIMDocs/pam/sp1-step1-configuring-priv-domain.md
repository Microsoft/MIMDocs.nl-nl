---
title: Stap 1 Het privédomein configureren
description: Het CORP-domein voorbereiden met bestaande of nieuwe identiteiten die worden beheerd door Privileged Identity Manager via scripts
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: c1092df1c1fe43551dfde8bbe4f0e77cf46ee981
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518203"
---
# <a name="step-1-configuring-the-priv-domain"></a>Stap 1 Het privédomein configureren

> [!div class="step-by-step"]
> [Stap 2 »](sp1-step2-configuring-corp-domain.md)

1. Meld u aan bij PRIVC als administrator
   * Als dit een PRIV-omgeving is, meldt u zich aan bij de CORPDC
2. Voer PowerShell uit als Administrator
3. cd $env: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecteer menuoptie 1 (PRIV Forest Configuration)


De serviceaccounts die zijn vereist voor het beheer van SQL/SharePoint en MIM worden automatisch gemaakt al ze al niet bestaan in het domein. U wordt gevraagd de wachtwoorden voor het maken van deze service-accounts tijdens het uitvoeren van het script in te voeren.
Als het PRIV-domein Windows Server 2016 is waarbij het functionele niveau is ingesteld op Windows Server 2016 Technical Preview 5, wordt u gevraagd om de optionele functie van Active Directory, Privileged Access Management, die vereist wordt door PAM, in te schakelen. Bevestig de optie Ja om door te gaan.
Voor functionele niveaus onder Windows Server 2016 kunt u de waarschuwing negeren dat extra configuratie niet wordt uitgevoerd. U moet PAMDeployment.ps1 en de PAM-forest-configuratie opnieuw uitvoeren zodra de administrator het functionele niveau verhoogt naar Windows Server 2016.

>[!NOTE]
>De volgende stappen zijn niet vereist voor PRIVOnly-configuraties

Kopieer de SIDs.txt die wordt gegenereerd in $env: SYSTEMDRIVE\PAM naar de vergelijkbare map op de CORPDC. Dit wordt vereist door de CORPDC om machtigingen voor PRIV-gebruikers in te stellen om CORP-gebruikerseigenschappen te lezen.
Nadat het script is voltooid, wordt u gevraagd om de computer opnieuw op te starten om de wijzigingen van kracht te laten worden.

> [!div class="step-by-step"]
> [Stap 2 »](sp1-step2-configuring-corp-domain.md)
