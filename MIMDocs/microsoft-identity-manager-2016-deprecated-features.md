---
title: Afgeschafte functies van MIM en planning voor de toekomst | Microsoft Docs
description: In dit artikel worden afgeschafte functies van MIM Identity Manager 2016 SP1 gedocumenteerd.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 2/28/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 009d0e99e2da445d4df35dc9de81b297a65fe2a3
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044222"
---
# <a name="deprecated-features"></a>Afgeschafte functies

In dit artikel worden de afgeschafte functies van Microsoft Identity Manager 2016 SP1 beschreven. Wanneer de functie nog steeds aanwezig is in Microsoft Identity Manager, wordt deze nog steeds ondersteund. Functies worden niet aanbevolen voor nieuwe implementaties, omdat deze mogelijk worden verwijderd in een functie versie.  Voor ontwikkel aars wordt u aangeraden geen afgeschafte functies te gebruiken in nieuwe toepassingen of oplossingen.

> [!NOTE]
> Functies en functionaliteiten die in de Microsoft Identity Manager SP1 worden verwijderd, worden aangeduid met * *. <br>
> Voor meer informatie over de ondersteunings [levenscyclus voor Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

Micro soft raadt klanten niet aan nieuwe implementaties van de onderdelen van micro soft BHOLD Suite te starten. Bestaande implementaties van BHOLD worden nog steeds ondersteund. Azure AD biedt nu [toegangs beoordelingen](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) waarmee een aantal van de functies van de BHOLD Attestation-campagne worden vervangen.

## <a name="certificate-management"></a>Certificaatbeheer 

| **Categorie**                | **Afgeschafte functie**              | **Vervangen en opmerkingen**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Beheer agenten | \* * FIM-certificaat beheer | De agent voor FIM Certificate Management is verwijderd in MIM 2016.                                                             |

## <a name="service-and-portal"></a>Service en portal

| **Categorie**                | **Afgeschafte functie**              | **Vervangen en opmerkingen**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmatische configuratie | Configuratie-interface van webservice (ma-data en MV-data) | De mogelijkheid om de FIM-synchronisatie service via de FIM service-webservice te configureren, wordt verwijderd in een volgende versie.
|

## <a name="synchronization-service"></a>Synchronisatieservice 

| **Categorie**                | **Afgeschafte functie**              | **Vervangen en opmerkingen**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmatische configuratie | Configuratie-interface van webservice | De mogelijkheid om de FIM-synchronisatie service via de FIM-service te configureren, wordt verwijderd in een volgende versie.                                                          |
| Beheer agenten           | Ingebouwde MAs                        | De volgende MAs zijn verwijderd in MIM 2016: </br> 1. * * MA voor FIM-certificaat beheer </br>2. * * MA voor Lotus Notes</br> 3. * * MA voor SAP R/3 </br> De Lotus Notes-en SAP R/3-MAs zijn vervangen door nieuwe versies. Zie voor meer informatie [nieuwste connector versie release geschiedenis & downloaden](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Beheer agenten           | ECMA1                               | Het ECMA1/XMA Extensibility Framework is vervangen door de ECMA 2,0. Het bijwerken van bestaande ECMA1-beheer agenten met ECMA 2.0-connectors is vereist.                                                                                                                                          |
| Beheer agenten           | Connectors out-of-process uitvoeren      | Deze functie wordt niet vervangen. De synchronisatie service roept de connector in hetzelfde proces altijd aan. Het is de verantwoordelijkheid van de connector om het andere proces te starten en te beheren. |
| Beheer agenten           | Weergave naam van partitie configureren    | Deze functie wordt niet vervangen. Deze optie is alleen gebruikt om een alternatieve naam op te geven voor een partitie in de WMI-interfaces.                                                                                                                                                                       |
| Profielen uitvoeren                | Gecombineerde profielen                   | De gecombineerde profielen verschillen per Importeer/synchronisatie, volledige import/Delta-synchronisatie en volledige import/Sync worden verwijderd. U moet in plaats daarvan run-profielen met twee stappen gebruiken. 

> [!NOTE]
> Houd alleen gecombineerde uitvoerings profielen in omgevingen waar de prestaties worden beïnvloed door een groot aantal bestaande disconnecters.


| **Categorie**                | **Afgeschafte functie**              | **Vervangen en opmerkingen**           |
|--------|-------|---|    
| Kenmerk prioriteit | Meerdere masters/gelijke prioriteit                       | De gelijke prioriteit wordt verwijderd. Er is geen vervanging voor deze functie. U moet in plaats daarvan hand matige prioriteit configureren. U kunt deze functie blijven gebruiken als in uw omgeving een FIM-Service beheer agent is geïmplementeerd. Deze beheer agent biedt geen hand matige prioriteit om te voor komen dat export-not-broncel voor declaratief inrichten is. |
| Regels voor samen voegen           | Toevoegen aan object type                             | Deze functie wordt niet vervangen. Alle regels voor samen voegen moeten expliciet het object type van de tekst voor de regel die ze proberen samen te stellen.       |
| Kenmerk stromen      | Selectie van Null-waarden toestaan ongewijzigd laten            | Deze functie wordt niet vervangen. "Null-waarden toestaan" wordt altijd geselecteerd. Zorg ervoor dat u ' null-waarden toestaan ' hebt geselecteerd in uw huidige omgeving.  |
| Kenmerk stromen      | "Geen kenmerken intrekken"                            | Deze functie wordt niet vervangen. Kenmerken worden altijd ingetrokken, wat de best practice is.  |
| Extensie voor regels      | De uitbrei ding voor het uitvoeren van een tekst-en VG-regel out-of-process | Deze functie wordt niet vervangen. De regels voor de omgekeerde en kenmerk stroom worden uitgevoerd in hetzelfde proces als de synchronisatie-engine.       |
| Extensie voor regels      | Eigenschappen van trans actie                                | Deze functie wordt niet vervangen. Vermijd het door geven van gegevens tussen inkomend, provisioning en uitgaande synchronisatie met deze hulpprogramma klasse.  |
| Extensie voor regels      | ExchangeUtils: Create55\*-methoden                     | De methoden voor het maken van objecten voor Exchange 5,5-servers worden verwijderd.        |
| Interface            | Mms_Metaverse                                        | Alle leden van de ClmUtils-klasse worden verwijderd in een volgende versie.   |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:

- Microsoft Identity Manager is nog nauw verwant aan diens voorganger Forefront Identity Manager. Zie [FIM 2010 R2 Documentation Roadmap](https://technet.microsoft.com/library/jj133885.aspx) (Engelstalig) als u FIM nog gebruikt of aanvullende documentatie wilt raadplegen.
- [Overwegingen voor de topologie voor het implementeren van MIM](topology-considerations.md) In dit artikel worden meerdere implementatie topologieën geïntroduceerd die u kunt implementeren.
- [Gids voor capaciteits planning](capacity-planning-guide.md) U kunt deze hand leiding samen met test omgevingen gebruiken om inzicht te krijgen in het juiste bereik voor uw implementatie.
