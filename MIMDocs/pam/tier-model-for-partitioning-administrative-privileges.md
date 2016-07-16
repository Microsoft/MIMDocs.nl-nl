---
title: Laagmodel voor het partitioneren van beheerdersbevoegdheden | Microsoft Identity Manager
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9e5f51d5ca731b3564b8262db0f4cddeb850231a
ms.openlocfilehash: 509c05bbda5f0a0b936518fb023000771c45d4f7


---

# Laagmodel voor het partitioneren van beheerdersbevoegdheden

In moderne omgeving liggen bedreigingen overal op de loer en is het niet de vraag of, maar wanneer een aanvaller toegang krijgt tot uw systeem. Dat betekent dat interne beveiliging even belangrijk is als een krachtig perimeternetwerk. In dit artikel wordt een beveiligingsmodel beschreven dat is bedoeld om u te beschermen tegen onrechtmatige uitbreiding van toegangsrechten door activiteiten met hoge bevoegdheden af te schermen van zones met hoge risico's. Dit model biedt een goede gebruikerservaring en voldoet ook aan best practices en beveiligingsprincipes.

## Onrechtmatige uitbreiding van toegangsrechten in Active Directory-forests

Gebruikers, services of toepassingsaccounts waaraan permanente beheerdersbevoegdheden voor forests van Windows Server Active Directory (AD) worden verleend, vormen een aanzienlijke risico voor de missie en zakelijke activiteiten van de organisatie. Aanvallers richten zich vaak op deze accounts omdat als een aanvaller hiermee bevoegdheden kan krijgen om verbinding te maken met andere servers of toepassingen in het domein.

Met het laagmodel worden scheiding ingesteld tussen beheerders op basis van de resources die ze beheren. Beheerders die werkstations van gebruikers beheren, worden afgescheiden van beheerders die toepassingen of bedrijfsentiteiten beheren. Meer informatie over dit model vindt u in het [referentiemateriaal over het beveiligen van bevoegde toegang](http://aka.ms/tiermodel).

## Weergave van referenties beperken met aanmeldingsbeperkingen

Het verminderen van het risico op diefstal van referenties voor beheerdersaccounts omvat doorgaans een hervorming van administratieve procedures om de kwetsbaarheid voor aanvallen te verminderen. Ten eerste wordt organisaties aangeraden:

- Het aantal hosts te beperken waarop beheerdersreferenties worden weergegeven.
- Bevoegdheden voor rollen tot een minimum te beperken.
- Ervoor te zorgen dat administratieve taken niet worden uitgevoerd op hosts die worden gebruikt voor standaardgebruikersactiviteiten (bijvoorbeeld e-mailen en internetten).

De volgende stap is het implementeren van aanmeldingsbeperkingen en het inschakelen van processen en procedures om te voldoen aan de vereisten voor het laagmodel. In het ideale geval moet weergave van referenties worden teruggebracht tot de minimale bevoegdheden die nodig zijn voor de rol binnen elke laag.

Aanmeldingsbeperkingen moeten worden geïmplementeerd om ervoor te zorgen dat maximaal bevoorrechte accounts geen toegang hebben tot minder veilige resources. Bijvoorbeeld:

- Domeinbeheerders (laag 0) kunnen zich niet aanmelden bij bedrijfsservers (laag 1) en standaardwerkstations van gebruikers (laag 2).
- Serverbeheerders (laag 1) kunnen zich niet aanmelden niet bij standaardwerkstations van gebruikers (laag 2).

>[!NOTE] 
> Serverbeheerders moeten niet worden opgenomen in de domeinbeheerdersgroep. Personeelsleden die verantwoordelijk zijn voor het beheren van domeincontrollers en bedrijfsservers, moeten afzonderlijke accounts krijgen.

Aanmeldingsbeperkingen kunnen worden afgedwongen met:

- Beperkingen voor aanmeldingsrechten van het groepbeleid, met inbegrip van:  
    - Toegang tot deze computer vanaf het netwerk weigeren  
    - Aanmelden als batchtaak weigeren  
    - Aanmelden als service weigeren  
    - Lokaal aanmelden weigeren  
    - Aanmelden via instellingen voor Extern bureaublad weigeren  
- Verificatiebeleidsregels en silo's bij gebruik van Windows Server 2012 of hoger
- Selectieve verificatie als het account zich in een toegewezen beheerforest bevindt

In het volgende artikel [Planning a bastion environment](planning-bastion-environment.md) (Een bastionomgeving plannen) wordt beschreven hoe u een toegewezen beheerforest voor Microsoft Identity Manager kunt toevoegen om beheerdersaccounts in te stellen.



<!--HONumber=Jun16_HO5-->


