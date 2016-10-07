---
title: "Stap 1 Het privédomein configureren"
description: Het CORP-domein voorbereiden met bestaande of nieuwe identiteiten die worden beheerd door Privileged Identity Manager via scripts
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 37ac600701ed9d90790e557d2f282be45eed43b4


---
# Stap 1 Het privédomein configureren

1. Meld u aan bij PRIVC als administrator
  * Als dit een PRIV-omgeving is, meldt u zich aan bij de CORPDC
2. Voer PowerShell uit als Administrator
3. cd $env: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecteer menuoptie 1 (PRIV Forest Configuration)


De serviceaccounts die zijn vereist voor het beheer van SQL/SharePoint en MIM worden automatisch gemaakt al ze al niet bestaan in het domein. U wordt gevraagd de wachtwoorden voor het maken van deze service-accounts tijdens het uitvoeren van het script in te voeren.
Als het PRIV-domein Windows Server 2016 is waarbij het functionele niveau is ingesteld op Windows Server 2016 Technical Preview 5, wordt u gevraagd om de optionele functie van Active Directory, Privileged Access Management, die vereist wordt door PAM, in te schakelen. Bevestig de optie Ja om door te gaan.
Voor functionele niveaus onder Windows Server 2016 kunt u de waarschuwing negeren dat extra configuratie niet wordt uitgevoerd. U moet PAMDeployment.ps1 en de PAM-forest-configuratie opnieuw uitvoeren zodra de administrator het functionele niveau verhoogt naar Windows Server 2016.

>[!Note] De volgende stappen zijn niet vereist voor PRIVOnly-configuraties

Kopieer de SIDs.txt die wordt gegenereerd in $env: SYSTEMDRIVE\PAM naar de vergelijkbare map op de CORPDC. Dit wordt vereist door de CORPDC om machtigingen voor PRIV-gebruikers in te stellen om CORP-gebruikerseigenschappen te lezen.
Nadat het script is voltooid, wordt u gevraagd om de computer opnieuw op te starten om de wijzigingen van kracht te laten worden.



<!--HONumber=Sep16_HO4-->


