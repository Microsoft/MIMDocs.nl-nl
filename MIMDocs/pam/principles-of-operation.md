---
title: PAM-onderdelen | Microsoft Docs
description: Privileged Access Management deelt sommige onderdelen met MIM en heeft daarnaast een aantal eigen onderdelen. Meer informatie over hoe deze onderdelen samenwerken.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e3e248092f674a031e255eb368681ab1b6dc21df
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010690"
---
# <a name="understand-the-components-of-mim-pam"></a>Meer informatie over de onderdelen van de MIM PAM

Privileged Access Management houdt beheerders toegang gescheiden van dagelijkse gebruikers accounts met behulp van een afzonderlijke forest.

> [!NOTE]
> De PAM-benadering die door MIM wordt verstrekt, is bedoeld om te worden gebruikt in een aangepaste architectuur voor geïsoleerde omgevingen waar Internet toegang niet beschikbaar is, waarbij deze configuratie vereist is voor de regelgeving, of in zeer belang rijke, geïsoleerde omgevingen zoals offline onderzoek laboratoria en niet-verbonden operationele technologie of omgevingen voor gegevens verzameling. Als uw Active Directory deel uitmaakt van een omgeving met Internet verbinding, raadpleegt u [privileged Access beveiligen](/security/compass/overview) voor meer informatie over waar u moet beginnen.

 Deze oplossing is afhankelijk van parallelle forests:

- *CORP*: uw algemene zakelijke forest dat een of meer domeinen omvat. Hoewel u meerdere CORP-forests kunt hebben, wordt in de voorbeelden in deze artikelen voor het gemak één forest met één domein gebruikt.  
- *PRIV*: een toegewezen forest dat specifiek voor dit PAM-scenario is gemaakt. Dit forest bevat één domein voor bevoorrechte groepen en accounts die uit een of meer CORP-domeinen afkomstig zijn via een schaduwkopie.

De MIM-oplossing zoals geconfigureerd voor PAM omvat de volgende onderdelen:  

- **MIM-service**: hiermee wordt bedrijfslogica geïmplementeerd voor bewerkingen voor identiteits- en toegangsbeheer, zoals bevoorrecht accountbeheer en de verwerking van aanvragen voor uitbreiding.
- **MIM-Portal**: een portal op basis van share point, die wordt gehost door share point 2013 of hoger, die een beheer-en configuratie gebruikersinterface biedt voor beheerders.
- **MIM-service database**: opgeslagen in SQL Server 2012 of hoger en bevat identiteits gegevens en meta gegevens die vereist zijn voor de MIM-service.
- **PAM-controleservice** en **PAM Component-service**: twee services waarmee de levenscyclus van bevoorrechte accounts wordt beheerd en waarmee ondersteuning wordt geboden aan de PRIV AD bij de levenscyclus voor groeplidmaatschap.
- **PowerShell-cmdlets**: voor het vullen van de MIM-service en PRIV AD met gebruikers en groepen die overeenkomen met de gebruikers en groepen in het CORP-forest voor PAM-beheerders en voor eindgebruikers die JIT-gebruikt aanvragen van de bevoegdheden op een beheerdersaccount.
- **PAM REST API en voorbeeldportal**: voor ontwikkelaars die MIM integreren in het PAM-scenario met aangepaste clients voor uitbreiding, zonder dat PowerShell of SOAP moet worden gebruiken. Het gebruik van de REST API wordt weergegeven met een voorbeeldwebtoepassing.

Eenmaal geïnstalleerd en geconfigureerd, is elke groep die is gemaakt door de migratie procedure in het PRIV-forest een afwijkende Principal-groep die de groep in het oorspronkelijke CORP-forest spiegelt. De groep Foreign principal bevat gebruikers die lid zijn van deze groep met dezelfde SID in hun Kerberos-token als de SID van de groep in het CORP-forest. Wanneer er daarnaast met de MIM-service leden worden toegevoegd aan deze groepen in het PRIV-forest, krijgen deze lidmaatschappen een beperking op basis van tijd.

Wanneer een gebruiker een uitbreiding aanvraagt via de PowerShell-cmdlets en de aanvraag wordt goedgekeurd, wordt het account met de MIM-service vervolgens toegevoegd aan een groep in het PRIV-forest. Wanneer de gebruiker zich aanmeldt met het bevoorrechte account, bevat het Kerberos-token een SID (Security Identifier) die identiek is aan de SID van de groep in het CORP-forest. Omdat het CORP-forest is geconfigureerd om het PRIV-forest te vertrouwen, wordt het account met verhoogde bevoegdheid weergegeven dat wordt gebruikt voor toegang tot een resource in het CORP-forest. Een resource die wordt gecontroleerd met de Kerberos-groepslidmaatschappen, moet deze lid zijn van de beveiligingsgroepen van die resource. Dit wordt opgegeven via forest-overschrijdende Kerberos-verificatie.

Bovendien geldt voor deze lidmaatschappen een tijdslimiet zodat na een vooraf geconfigureerd interval het beheeraccount van de gebruiker niet langer deel uitmaakt van de groep in het PRIV-forest. Als gevolg hiervan kan dat account niet langer worden gebruikt voor toegang tot extra resources.
