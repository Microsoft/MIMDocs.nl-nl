---
title: Het laagmodel van de PAM-omgeving | Microsoft Docs
description: Meer informatie over het laagmodel dat uw systeem scheidt op basis van de kwetsbaarheid voor risico's.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4c3b43e50403890572e77773191a821cf247269c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="tier-model-for-partitioning-administrative-privileges"></a>Laagmodel voor het partitioneren van beheerdersbevoegdheden

In moderne omgeving liggen bedreigingen overal op de loer en is het niet de vraag of, maar wanneer een aanvaller toegang krijgt tot uw systeem. Dat betekent dat interne beveiliging even belangrijk is als een krachtig perimeternetwerk. In dit artikel wordt een beveiligingsmodel beschreven dat is bedoeld om u te beschermen tegen onrechtmatige uitbreiding van toegangsrechten door activiteiten met hoge bevoegdheden af te schermen van zones met hoge risico's. Dit model biedt een goede gebruikerservaring en voldoet ook aan best practices en beveiligingsprincipes.

## <a name="elevation-of-privilege-in-active-directory-forests"></a>Onrechtmatige uitbreiding van toegangsrechten in Active Directory-forests

Gebruikers, services of toepassingsaccounts waaraan permanente beheerdersbevoegdheden voor forests van Windows Server Active Directory (AD) worden verleend, vormen een aanzienlijke risico voor de missie en zakelijke activiteiten van de organisatie. Aanvallers richten zich vaak op deze accounts omdat als een aanvaller hiermee bevoegdheden kan krijgen om verbinding te maken met andere servers of toepassingen in het domein.

Met het laagmodel worden scheiding ingesteld tussen beheerders op basis van de resources die ze beheren. Beheerders die werkstations van gebruikers beheren, worden afgescheiden van beheerders die toepassingen of bedrijfsentiteiten beheren. Meer informatie over dit model vindt u in het [referentiemateriaal over het beveiligen van bevoegde toegang](http://aka.ms/tiermodel).

## <a name="restricting-credential-exposure-with-logon-restrictions"></a>Weergave van referenties beperken met aanmeldingsbeperkingen

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
