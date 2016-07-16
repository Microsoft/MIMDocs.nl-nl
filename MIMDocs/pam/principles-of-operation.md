---
title: Inzicht in de PAM-onderdelen | Microsoft Identity Manager
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: a6bdf1b947ee3ebc4c9e89e74b2912697ebf1f60
ms.openlocfilehash: 49f47050703095d402a1514342baf4e928f66c70


---

# Inzicht in de PAM-onderdelen

Met Privileged Access Management wordt beheerderstoegang gescheiden gehouden van dagelijkse gebruikersaccounts. Deze oplossing is afhankelijk van parallelle forests:

- *CORP*: uw algemene zakelijke forest dat een of meer domeinen omvat. Hoewel u meerdere CORP-forests kunt hebben, wordt in de voorbeelden in deze artikelen voor het gemak één forest met één domein gebruikt.  
- *PRIV*: een toegewezen forest dat specifiek voor dit PAM-scenario is gemaakt. Dit forest bevat één domein voor bevoorrechte groepen en accounts die uit een of meer CORP-domeinen afkomstig zijn via een schaduwkopie.

De MIM-oplossing zoals geconfigureerd voor PAM omvat de volgende onderdelen:  

- **MIM-service**: hiermee wordt bedrijfslogica geïmplementeerd voor bewerkingen voor identiteits- en toegangsbeheer, zoals bevoorrecht accountbeheer en de verwerking van aanvragen voor uitbreiding.   
- **MIM-portal**: een SharePoint-portal, gehost door SharePoint 2013, voor administratorbeheer en een gebruikersinterface voor configuratie.
- **MIM-servicedatabase**: opgeslagen in SQL Server 2012 of 2014 en bevat identiteitsgegevens en metagegevens die nodig zijn voor de MIM-service.
- **PAM-controleservice** en **PAM Component-service**: twee services waarmee de levenscyclus van bevoorrechte accounts wordt beheerd en waarmee ondersteuning wordt geboden aan de PRIV AD bij de levenscyclus voor groeplidmaatschap.
- **PowerShell-cmdlets**: voor het vullen van de MIM-service en PRIV AD met gebruikers en groepen die overeenkomen met de gebruikers en groepen in het CORP-forest voor PAM-beheerders en voor eindgebruikers die JIT-gebruikt aanvragen van de bevoegdheden op een beheerdersaccount.
- **PAM REST API en voorbeeldportal**: voor ontwikkelaars die MIM integreren in het PAM-scenario met aangepaste clients voor uitbreiding, zonder dat PowerShell of SOAP moet worden gebruiken. Het gebruik van de REST API wordt weergegeven met een voorbeeldwebtoepassing.

Eenmaal geïnstalleerd en geconfigureerd, is elke groep die is gemaakt in de migratieprocedure in het PRIV-forest een schaduwbeveiligingsgroep gebaseerd op SIDHistory (of in een latere update met Windows Server vNext, een Foreign Principal Group) die overeenkomt met de SID-groep in het oorspronkelijke CORP-forest. Wanneer er daarnaast met de MIM-service leden worden toegevoegd aan deze groepen in het PRIV-forest, krijgen deze lidmaatschappen een beperking op basis van tijd.

Wanneer een gebruiker een uitbreiding aanvraagt via de PowerShell-cmdlets en de aanvraag wordt goedgekeurd, wordt het account met de MIM-service vervolgens toegevoegd aan een groep in het PRIV-forest. Wanneer de gebruiker zich aanmeldt met het bevoorrechte account, bevat het Kerberos-token een SID (Security Identifier) die identiek is aan de SID van de groep in het CORP-forest. Omdat het CORP-forest is geconfigureerd om het PRIV-forest te vertrouwen, wordt het account met verhoogde bevoegdheid weergegeven dat wordt gebruikt voor toegang tot een resource in het CORP-forest. Een resource die wordt gecontroleerd met de Kerberos-groepslidmaatschappen, moet deze lid zijn van de beveiligingsgroepen van die resource. Dit wordt opgegeven via forest-overschrijdende Kerberos-verificatie.

Bovendien geldt voor deze lidmaatschappen een tijdslimiet zodat na een vooraf geconfigureerd interval het beheeraccount van de gebruiker niet langer deel uitmaakt van de groep in het PRIV-forest. Als gevolg hiervan kan dat account niet langer worden gebruikt voor toegang tot extra resources.



<!--HONumber=Jun16_HO3-->


