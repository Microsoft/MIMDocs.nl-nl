---
title: MIM-functies en Planning voor de toekomst afgeschaft | Microsoft Docs
description: In dit artikel worden afgeschaft functies van de MIM Identity Manager 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/28/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fcf9ec8387761b6f154a95d6100ef54a12d4caf8
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358424"
---
# <a name="deprecated-features"></a>Afgeschafte functies

In dit artikel beschrijft de afgeschafte functies van Microsoft Identity Manager 2016 SP1. Wanneer de functie nog steeds aanwezig in Microsoft Identity Manager is, is het nog steeds ondersteund. Functies worden niet aanbevolen voor nieuwe implementaties, zoals ze in een functieversie kunnen worden verwijderd.  Voor ontwikkelaars, wordt aangeraden niet met behulp van de afgeschafte functies in de nieuwe toepassingen of oplossingen.

> [!NOTE]
> Functies en functionaliteiten in de Microsoft Identity Manager SP1 hebt verwijderd, worden aangeduid met **. <br>
> Voor meer informatie over de ondersteuning voor [levenscyclus voor Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

Klanten beginnen nieuwe implementaties van de Microsoft BHOLD-Suite-onderdelen wordt niet aanbevolen. Bestaande implementaties van BHOLD blijven worden ondersteund. Nu Azure AD biedt [toegangsbeoordelingen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) die enkele van de functies van BHOLD attestation campagne vervangen.

## <a name="certificate-management"></a>Certificaatbeheer 

| **Categorie**                | **Afgeschafte functies**              | **Vervanging en opmerkingen**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Beheeragents | ** FIM Certificate Management | FIM Certificate Management-Agent is verwijderd uit MIM 2016.                                                             |

## <a name="service-and-portal"></a>Service en portal

| **Categorie**                | **Afgeschafte functies**              | **Vervanging en opmerkingen**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmatische configuratie | Web Service configuratie-interface (ma-gegevens en mv-gegevens) | De mogelijkheid om te configureren van de FIM-synchronisatieservice via FIM service-webservice wordt verwijderd in een volgende versie.
|

## <a name="synchronization-service"></a>Synchronisatieservice 

| **Categorie**                | **Afgeschafte functies**              | **Vervanging en opmerkingen**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmatische configuratie | Web Service configuratie-interface | De mogelijkheid om te configureren van de FIM-synchronisatieservice via de FIM-service wordt verwijderd in een volgende versie.                                                          |
| Beheeragents           | Ingebouwde MAs                        | De volgende MAs zijn verwijderd in MIM 2016: </br> 1. ** MA voor FIM Certificate Management </br>2. ** MA voor Lotus Notes</br> 3. ** MA voor SAP R/3 </br> Lotus Notes en SAP R/3 MAs zijn vervangen door nieuwe versies. Zie voor meer informatie, [nieuwste versiegeschiedenis van Connector & downloaden](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Beheeragents           | ECMA1                               | Het uitbreidingsframework ECMA1/XMA is vervangen door de ECMA 2.0. Bestaande ECMA1 management agents bijwerken met ECMA2.0 connectors is vereist.                                                                                                                                          |
| Beheeragents           | Actieve Connectors out-of-proc      | Deze functie wordt niet vervangen. De synchronisatieservice wordt altijd de connector in hetzelfde proces aanroepen. Het is de verantwoordelijkheid van de connector om te starten en beheren van de ander proces. |
| Beheeragents           | Weergavenaam van de partitie configureren    | Deze functie wordt niet vervangen. Deze optie is alleen gebruikt voor een alternatieve naam voor een partitie in de WMI-interfaces.                                                                                                                                                                       |
| Profielen uitvoeren                | Gecombineerde profielen                   | De gecombineerde profielen importeren/Deltasynchronisatie, volledige import-/ Deltasynchronisatie en volledige import/synchronisatie wordt verwijderd. U moet in plaats daarvan uitvoeringsprofielen met twee stappen gebruiken. 

> [!NOTE]
> U moet de gecombineerde uitvoeringsprofielen alleen in omgevingen waar de prestaties zouden worden beïnvloed door een groot aantal bestaande disconnectors bewaren.


| **Categorie**                | **Afgeschafte functies**              | **Vervanging en opmerkingen**           |
|--------|-------|---|    
| Kenmerkprioriteit | Multi-beheersing/gelijk prioriteit                       | Dezelfde prioriteit worden verwijderd. Er is geen vervanging voor deze functie. In plaats daarvan moet u handmatig prioriteit configureren. U kunt deze functie wilt gebruiken als uw omgeving een FIM-servicebeheeragent geïmplementeerd heeft. Deze beheeragent biedt geen handmatige prioriteit om te voorkomen dat de export-niet-prioriteit voor declaratieve inrichting. |
| Regels toevoegen           | Neem deel aan op 'Alle' objecttype                             | Deze functie wordt niet vervangen. Alle regels voor lid worden van moeten expliciet definiëren voor het metaverse-objecttype die probeert om lid te maken.       |
| Kenmerkstromen      | Hef de selectie van 'null-waarden toestaan' voor de geëxporteerde waarden            | Deze functie wordt niet vervangen. 'Null-waarden toestaan' worden altijd geselecteerd. U moet ervoor zorgen dat u 'Null-waarden toestaan hebt' in uw huidige omgeving hebt geselecteerd.  |
| Kenmerkstromen      | "Kenmerken niet intrekken"                            | Deze functie wordt niet vervangen. Kenmerken wordt altijd worden ingetrokken, is de aanbevolen procedure.  |
| Uitbreiding van regels      | Extensie out-of-proc met metaverse en ma regels | Deze functie wordt niet vervangen. De regels voor de metaverse en het kenmerk wordt uitgevoerd in hetzelfde proces als de synchronisatie-engine.       |
| Uitbreiding van regels      | Eigenschappen van transactie                                | Deze functie wordt niet vervangen. U kunt het doorgeven van gegevens tussen inricht, inkomende en uitgaande synchronisatie met behulp van deze hulpprogrammaklasse dient te vermijden.  |
| Uitbreiding van regels      | ExchangeUtils: Create55\* methoden                     | De methoden voor het maken van objecten voor Exchange 5.5-servers worden verwijderd.        |
| Interface            | Mms_Metaverse                                        | Alle leden van de klasse ClmUtils wordt verwijderd in een volgende versie.   |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:

- Microsoft Identity Manager is nog nauw verwant aan diens voorganger Forefront Identity Manager. Zie [FIM 2010 R2 Documentation Roadmap](https://technet.microsoft.com/library/jj133885.aspx) (Engelstalig) als u FIM nog gebruikt of aanvullende documentatie wilt raadplegen.
- [Aandachtspunten voor topologie voor het implementeren van MIM](topology-considerations.md) in dit artikel bevat meerdere implementatietopologieën die u implementeren overwegen kunt.
- [Handleiding voor capaciteitsplanning](capacity-planning-guide.md) kunt u deze handleiding, samen met testomgevingen, om te begrijpen omvang die geschikt is voor uw implementatie.
