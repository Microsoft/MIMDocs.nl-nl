---
title: Handleiding voor capaciteitsplanning | Microsoft Docs
description: Met de informatie in deze handleiding leert u de variabelen te begrijpen die u in aanmerking moet nemen voordat u MIM 2016 implementeert, inclusief de belastingsniveaus en beleidsbeslissingen.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 15eb35d01ed5c5c6e125c45f238bb2f7a7c564d7
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042114"
---
# <a name="capacity-planning-guide"></a>Handleiding voor capaciteitsplanning

Met Microsoft Identity Manager (MIM) kunt u gebruikersaccounts maken, bijwerken en verwijderen binnen uw organisatie. Dit biedt eindgebruikers ook de mogelijkheid om de selfservicefuncties van hun eigen account te beheren. Zelfs in een kleine omgeving kunnen al deze acties behoorlijk wat tijd vergen.

Voordat u aan de slag gaat met MIM kunt u deze handleiding, samen met testomgevingen, gebruiken om inzicht te krijgen in het bereik van uw implementatie. Dit artikel begeleidt u bij veel voorkomende factoren die u in aanmerking moet nemen. Omdat elke implementatie echter uniek is, is het testen van uw scenario's in een testomgeving nog steeds de beste manier om te bepalen wat de juiste servers, hardware of topologieën voor uw behoeften zijn.

Als u nog niet bekend bent met MIM 2016 en de bijbehorende onderdelen, kunt u het beste informatie over [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md) lezen voor u doorgaat.

## <a name="overview"></a>Overzicht

Er zijn een aantal factoren die van invloed kunnen zijn op de totale capaciteit en prestaties van uw Microsoft Identity Manager-implementatie:

- De manieren waarop u de MIM-onderdelen (topologie) fysiek implementeert.
- De hardware waarop deze onderdelen worden uitgevoerd.
- Het aantal en de complexiteit van de MIM-beleids configuratie objecten zijn belang rijke factoren om rekening mee te houden bij het plannen van capaciteit.
- De verwachte schaal van de implementatie en de verwachte belasting zijn doorgaans meer duidelijkere factoren die van invloed zijn op de prestaties en capaciteit.

De belangrijkste factoren die van invloed zijn op de capaciteit en prestaties van een MIM 2016-implementatie, worden in de volgende tabel behandeld:

| Ontwerpfactor | Overwegingen |
| ------------- | -------------- |
| Topologie | De distributie van de MIM-services op de computers in het netwerk. |
| Hardware | De fysieke hardware (fysiek of virtueel) voor elk MIM-onderdeel, inclusief CPU, geheugen, netwerk adapter en configuratie van harde schijven. |
| MIM-beleidsconfiguratieobjecten | Het aantal en type MIM-beleidsconfiguratieobjecten, inclusief sets, beheerbeleidsregels (MPR's) en werkstromen. |
| Schaal | De gebruikers, groepen, berekende groepen en aangepaste object typen die worden beheerd door MIM 2016. Houd tevens rekening met de complexiteit van dynamische groepen en het nesten van groepen. |
| Werklast | De frequentie van gebruik. Bewerkingen, zoals het maken van nieuwe groepen of gebruikers, het opnieuw instellen van wacht woorden of portal bezoeken per minuut of uur. Houd er rekening mee dat de belasting in de loop van een uur, dag, week of jaar kan verschillen. Afhankelijk van het onderdeel kunt u kiezen voor een ontwerp voor piekbelasting of gemiddelde belasting. |

## <a name="hosting-microsoft-identity-manager-components"></a>Microsoft Identity Manager-onderdelen hosten

De onderdelen van Microsoft Identity Manager hoeven zich niet op dezelfde computer bevinden. Nadenken over deze onderdelen en de fysieke of virtuele machines die als host optreden, vormt een belangrijk onderdeel van de capaciteitsplanning.

Hardwarefactoren kunnen invloed hebben op de prestaties van de MIM-omgeving. Bijvoorbeeld:

- Wat is de fysieke-schijfconfiguratie voor de computer met de SQL-Database voor de MIM 2016-service? Het aantal aandrijfassen die gezamenlijk de schijfconfiguratie vormen en de distributie van logboek- en gegevensbestanden kunnen de prestaties van het systeem aanzienlijk beïnvloeden.

Denk ook na over de externe factoren in uw configuratie. Bijvoorbeeld:

- Als u een SAN gebruikt als configuratie van de MIM 2016-servicedatabase, moet u ook rekening houden met de andere programma’s die de SAN delen. Deze toepassingen kunnen invloed hebben op prestaties van de database als ze gebruikmaken van de gedeelde schijfresources op de SAN.

## <a name="users-and-groups"></a>Gebruikers en groepen

Het aantal gebruikers en groepen in uw omgeving is een typisch onderwerp dat u in aanmerking moet nemen wanneer u nadenkt over het schalen van een implementatie. Er zijn echter enkele andere gerelateerde overwegingen waarmee u in uw planning tevens rekening moet houden.

- Kunnen gebruikers groepen maken? Als dit het geval is, moet u inschatten hoe het maken van nieuwe groepen door gebruikers de groei van groepen in uw omgeving beïnvloedt.

- Worden er dynamische groepen geïmplementeerd? Bereken hoeveel en welke typen dynamische groepen in uw omgeving worden verwacht.

## <a name="expected-load-levels"></a>Verwachte belastingsniveaus

U moet ook rekening houden met het type belasting dat op de MIM-onderdelen wordt geplaatst. U moet een schatting maken van de belasting door de huidige toepassingen in uw omgeving te bekijken. U moet onder andere rekening houden met de volgende relevante vragen:

- Hoe vaak verwacht u een aanvraag om lid te worden van een groep of deze te verlaten?

- Hoe vaak verwacht u dat een gebruiker een statische of dynamische groep gaat maken?

- Hoeveel niet door gebruikers geïnitieerde bewerkingen verwacht u, zoals de synchronisatie van wijzigingen in externe systemen? Zorg ervoor dat u rekening houdt met een belasting die wordt gegenereerd door het synchroniseren van identiteitsgegevens met externe systemen.

- Wat voor soort scenario's wilt u implementeren? Verschillende scenario's dragen bij aan verschillende laad patronen. Client computers waarop de MIM 2016-client is geïnstalleerd, valideren bijvoorbeeld periodiek of registratie is vereist bij het aanmelden.

- Verwacht u grote verschillen in de belastingsniveaus, variërend van een normale belasting tot piekbelasting? Het is bijvoorbeeld mogelijk dat veel wacht woorden opnieuw worden ingesteld na een kerst periode. Zorg ervoor dat u uw systeemonderhoud en synchronisatieplanningen buiten de verwachte gebruikspieken plant. Als u nadenkt over capaciteitsplanning, moet u rekening houden met perioden van piekbelasting.

## <a name="policy-configuration-objects"></a>Beleidsconfiguratieobjecten

MIM-beleids configuratie objecten omvatten de beheer beleidsregels, sets, werk stromen en synchronisatie regels voor een implementatie. MIM-implementaties zijn uniek voor elke klant omdat de beleidsconfiguratie verandert, afhankelijk van de behoeften van elke implementatie. Belang rijke prestatie overwegingen zijn onder andere de volgende MIM-beleids configuratie objecten:

- **Sets** Elke bewerking in het systeem moet worden geëvalueerd op basis van bestaande setlidmaatschappen en -updates die wijzigingen in het setlidmaatschap veroorzaken. Een wijziging in het gebouw nummer van het kantoor van een individu kan bijvoorbeeld geen grote invloed hebben. Andere wijzigingen kunnen echter een trapsgewijze invloed, zoals de wijziging van een manager die van invloed kan zijn op meerdere objecten op meerdere niveaus.

- **Beheerbeleidsregels** Met de beheerbeleidsregels (MPR’s) worden de toegangsbeheersregel beheerd en werkstromen geactiveerd. Door het maken van beheer beleidsregels kan de nood zaak worden verg root om het aantal sets te verhogen zodat u verschillende object overgangs statussen kunt vastleggen. Deze extra sets activeren mogelijk extra werkstromen, waarbij elke werkstroom wordt toegewezen aan unieke aanvragen in het systeem. Dit wordt vervolgens een extra onderwerp dat u in aanmerking moet nemen bij het plannen van capaciteit.

MIM-beleidsconfiguratie omvat tevens het nemen van beslissingen over het inrichten in uw omgeving. Zorg ervoor dat u over de volgende zaken nadenkt:

- Omvat uw implementatie de inrichting van afwijkende beveiligingsprincipes voor meerdere AD DS-forests (Active Directory Domain Services)? Hierdoor worden meer werkstromen en aanvragen gegenereerd, wat leidt tot extra belasting van het systeem.

- Gebruikt u inrichting zonder code? Zo ja, dan is dit van invloed op het aantal verwachte regelvermeldingen en de bijbehorende aanvragen en werkstromen in het systeem.

## <a name="next-steps"></a>Volgende stappen

- [Aandachtspunten voor topologie voor een MIM-implementatie](topology-considerations.md)
- De Download bare [hand leiding voor de capaciteits planning van Forefront Identity Manager (FIM) 2010](https://www.microsoft.com/en-us/download/details.aspx?id=7437) gaat meer details over een test resultaat en prestatie test resultaten.
