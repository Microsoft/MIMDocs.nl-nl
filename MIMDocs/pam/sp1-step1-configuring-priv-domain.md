---
title: "Stap 1 Het privédomein configureren"
description: Het CORP-domein voorbereiden met bestaande of nieuwe identiteiten die worden beheerd door Privileged Identity Manager via scripts
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 24e91ed2f51206b03bec505fc0d28d25128d2c94
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# Stap 1 Het privédomein configureren
<a id="step-1-configuring-the-priv-domain" class="xliff"></a>

>[!div class="step-by-step"]
[Stap 2 »](sp1-step2-configuring-corp-domain.md)

1. Meld u aan bij PRIVC als administrator
  * Als dit een PRIV-omgeving is, meldt u zich aan bij de CORPDC
2. Voer PowerShell uit als Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecteer menuoptie 1 (PRIV Forest Configuration)


De serviceaccounts die zijn vereist voor het beheer van SQL/SharePoint en MIM worden automatisch gemaakt al ze al niet bestaan in het domein. U wordt gevraagd de wachtwoorden voor het maken van deze service-accounts tijdens het uitvoeren van het script in te voeren.
Als het PRIV-domein Windows Server 2016 is waarbij het functionele niveau is ingesteld op Windows Server 2016 Technical Preview 5, wordt u gevraagd om de optionele functie van Active Directory, Privileged Access Management, die vereist wordt door PAM, in te schakelen. Bevestig de optie Ja om door te gaan.
Voor functionele niveaus onder Windows Server 2016 kunt u de waarschuwing negeren dat extra configuratie niet wordt uitgevoerd. U moet PAMDeployment.ps1 en de PAM-forest-configuratie opnieuw uitvoeren zodra de administrator het functionele niveau verhoogt naar Windows Server 2016.

>[!NOTE]
>De volgende stappen zijn niet vereist voor PRIVOnly-configuraties

Kopieer de SIDs.txt die wordt gegenereerd in $env: SYSTEMDRIVE\PAM naar de vergelijkbare map op de CORPDC. Dit wordt vereist door de CORPDC om machtigingen voor PRIV-gebruikers in te stellen om CORP-gebruikerseigenschappen te lezen.
Nadat het script is voltooid, wordt u gevraagd om de computer opnieuw op te starten om de wijzigingen van kracht te laten worden.

>[!div class="step-by-step"]
[Stap 2 »](sp1-step2-configuring-corp-domain.md)
