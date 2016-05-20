---
# required metadata

title: Handleiding voor capaciteitsplanning | Microsoft Identity Manager
description: Met de informatie in deze handleiding leert u de variabelen te begrijpen die u in aanmerking moet nemen voordat u MIM 2016 implementeert, inclusief de belastingsniveaus en beleidsbeslissingen.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Handleiding voor capaciteitsplanning

Als u klaar bent om Microsoft Identity Manager (MIM) te implementeren, gebruikt u deze handleiding en testomgevingen om uw implementatie te ontwerpen. Dit artikel begeleidt u bij veel voorkomende factoren die u in aanmerking moet nemen. Omdat elke implementatie echter uniek is, is het testen van uw scenario's in een testomgeving nog steeds de beste manier om te bepalen wat de juiste servers, hardware of topologieën voor uw behoeften zijn.

Als u nog niet bekend bent met MIM 2016 en de bijbehorende onderdelen, kunt u het beste informatie over [Microsoft Identity Manager 2016](/microsoft-identity-manager/understand-explore/microsoft-identity-manager-2016) lezen voor u doorgaat.

## Overzicht
Er zijn een verschillende variabelen die de algehele capaciteit en prestaties van uw Microsoft Identity Manager-implementatie kunnen beïnvloeden. De manieren waarop u de MIM-onderdelen (topologie) fysiek implementeert en de hardware waarop deze onderdelen worden uitgevoerd, zijn belangrijke factoren bij het bepalen van de prestaties en capaciteit die u van uw implementatie MIM kunt verwachten. Het aantal en de complexiteit van de MIM-beleidsconfiguratieobjecten zijn mogelijk minder duidelijk, maar ze vormen nog steeds een belangrijke factor waarmee u rekening moet houden bij het plannen van capaciteit. Uiteindelijk zijn de verwachte schaal van de implementatie, evenals de verwachte belasting, doorgaans duidelijkere factoren die invloed hebben op prestaties en capaciteit.

De belangrijkste factoren die van invloed zijn op de capaciteit en de prestaties die kunnen worden verwacht een MIM 2016-implementatie, worden in de volgende tabel beschreven.

| Ontwerpfactor | Aandachtspunten |
| ------------- | -------------- |
| Topologie | De distributie van de MIM-services op de computers in het netwerk. |
| Hardware | De fysieke hardware of eventuele gevirtualiseerde hardwarespecificaties die u voor elk MIM-onderdeel uitvoert. Dit omvat CPU, geheugen, netwerkadapter en de hardeschijfconfiguratie. |
| MIM-beleidsconfiguratieobjecten | Het aantal en type MIM-beleidsconfiguratieobjecten, inclusief sets, beheerbeleidsregels (MPR's) en werkstromen. |
| Schaal | Het aantal gebruikers, groepen, berekende groepen en aangepaste objecttypen, zoals computers die worden beheerd door MIM 2016. Houd tevens rekening met de complexiteit van dynamische groepen en het nesten van groepen. |
| Werklast | De frequentie van gebruik. Hoe vaak verwacht u bijvoorbeeld dat nieuwe groepen of gebruikers worden gemaakt, wachtwoorden opnieuw worden ingesteld of de portal wordt bezocht in een bepaalde periode. Houd er rekening mee dat de belasting in de loop van een uur, dag, week of jaar kan verschillen. Afhankelijk van het onderdeel kunt u kiezen voor een ontwerp voor piekbelasting of gemiddelde belasting. |


## Microsoft Identity Manager-onderdelen hosten

De onderdelen van Microsoft Identity Manager hoeven zich niet op dezelfde computer bevinden. Nadenken over deze onderdelen en de fysieke of virtuele machines die als host optreden, vormt een belangrijk onderdeel van de capaciteitsplanning.

Hardwarefactoren kunnen invloed hebben op de prestaties van de MIM-omgeving. Bijvoorbeeld:
- Wat is de fysieke-schijfconfiguratie voor de computer met de SQL-Database voor de MIM 2016-service? Het aantal aandrijfassen die gezamenlijk de schijfconfiguratie vormen en de distributie van logboek- en gegevensbestanden kunnen de prestaties van het systeem aanzienlijk beïnvloeden.

Denk ook na over de externe factoren in uw configuratie. Bijvoorbeeld:
- Als u een SAN gebruikt als configuratie van de MIM 2016-servicedatabase, moet u ook rekening houden met de andere programma’s die de SAN delen. Deze toepassingen kunnen invloed hebben op prestaties van de database als ze gebruikmaken van de gedeelde schijfresources op de SAN.


## Gebruikers en groepen
Het aantal gebruikers en groepen in uw omgeving is een typisch onderwerp dat u in aanmerking moet nemen wanneer u nadenkt over het schalen van een implementatie. Er zijn echter enkele andere gerelateerde overwegingen waarmee u in uw planning tevens rekening moet houden.

- Kunnen gebruikers groepen maken? Als dit het geval is, moet u inschatten hoe het maken van nieuwe groepen door gebruikers de groei van groepen in uw omgeving beïnvloedt.

- Worden er dynamische groepen geïmplementeerd? Bereken hoeveel en welke typen dynamische groepen in uw omgeving worden verwacht.


## Verwachte belastingsniveaus
U moet ook rekening houden met het type belasting dat op de MIM-onderdelen wordt geplaatst. Deze informatie kan waarschijnlijk worden geschat door te kijken naar huidige programma’s in uw omgeving. U moet onder andere rekening houden met de volgende relevante vragen:

- Hoe vaak verwacht u een aanvraag om lid te worden van een groep of deze te verlaten?

- Hoe vaak verwacht u dat een gebruiker een statische of dynamische groep gaat maken?

- Hoeveel niet door gebruikers geïnitieerde bewerkingen verwacht u, zoals de synchronisatie van wijzigingen in externe systemen? Zorg ervoor dat u rekening houdt met een belasting die wordt gegenereerd door het synchroniseren van identiteitsgegevens met externe systemen.

- Wat voor soort scenario's wilt u implementeren? Verschillende scenario's dragen bij aan verschillende belastingpatronen. Clientcomputers waarop de MIM 2016-client is geïnstalleerd, valideren bijvoorbeeld periodiek of registratie is vereist bij het aanmelden, waardoor de systeembelasting wordt verhoogd.

- Verwacht u grote verschillen in de belastingsniveaus, variërend van een normale belasting tot piekbelasting? Zo worden er na een vakantie doorgaans veel wachtwoorden opnieuw ingesteld. Zorg ervoor dat u uw systeemonderhoud en synchronisatieplanningen buiten de verwachte gebruikspieken plant. Als u nadenkt over capaciteitsplanning, moet u rekening houden met perioden van piekbelasting.


## Beleidsconfiguratieobjecten

Microsoft Identity Manager-beleidsconfiguratieobjecten omvatten MPR’s, sets, werkstromen en synchronisatieregels voor een bepaalde implementatie. MIM-implementaties zijn uniek voor elke klant omdat de beleidsconfiguratie verandert, afhankelijk van de behoeften van elke implementatie. Belangrijke prestatieoverwegingen voor MIM-beleidsconfiguratieobjecten zijn onder andere:

- **Sets** Elke bewerking in het systeem moet worden geëvalueerd op basis van bestaande setlidmaatschappen en -updates die wijzigingen in het setlidmaatschap veroorzaken. Zo mag een eenvoudige wijziging, zoals een wijziging in het nummer van het kantoor van een gebruiker, geen grote gevolgen hebben. Andere wijzigingen kunnen echter een trapsgewijze invloed, zoals de wijziging van een manager die van invloed kan zijn op meerdere objecten op meerdere niveaus.

- **Beheerbeleidsregels** Met de beheerbeleidsregels (MPR’s) worden de toegangsbeheersregel beheerd en werkstromen geactiveerd. Terwijl u beheerbeleidsregels maakt, merkt u mogelijk dat u het aantal sets moet verhogen om verschillende objecttransitietoestanden te kunnen vastleggen. Deze extra sets activeren mogelijk extra werkstromen, waarbij elke werkstroom wordt toegewezen aan unieke aanvragen in het systeem. Dit wordt vervolgens een extra onderwerp dat u in aanmerking moet nemen bij het plannen van capaciteit.

MIM-beleidsconfiguratie omvat tevens het nemen van beslissingen over het inrichten in uw omgeving. Zorg ervoor dat u over de volgende zaken nadenkt:

- Omvat uw implementatie de inrichting van afwijkende beveiligingsprincipes voor meerdere AD DS-forests (Active Directory Domain Services)? Hierdoor worden meer werkstromen en aanvragen gegenereerd, wat leidt tot extra belasting van het systeem.

- Gebruikt u inrichting zonder code? Zo ja, dan is dit van invloed op het aantal verwachte regelvermeldingen en de bijbehorende aanvragen en werkstromen in het systeem.


## Zie ook
- [Aandachtspunten voor topologie voor een MIM-implementatie](topology-considerations.md)
- De downloadbare [handleiding voor planningscapaciteit van Forefront Identity Manager (FIM) 2010 ](http://go.microsoft.com/fwlink/?LinkId=200180) biedt meer informatie over een testbuild en prestatietestresultaten.


<!--HONumber=Apr16_HO4-->


