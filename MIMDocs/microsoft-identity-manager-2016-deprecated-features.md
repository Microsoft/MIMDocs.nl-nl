---
title: MIM afgeschafte functies en plannen voor de toekomst | Microsoft Docs
description: Dit artikel worden afgeschafte functies van de MIM-Identity Manager 2016 SP1.
keywords: ''
author: barclayn
ms.author: davidste
manager: mbaldwin
ms.date: 2/28/2018
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 50f7b135ce0d5a46ea08068a7658b229759d2b50
ms.sourcegitcommit: 24bb3e82f55971696bdefa6c240f1a27f856e110
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="deprecated-features"></a>Afgeschafte functies

In dit artikel beschrijft de afgeschafte functies van Microsoft Identity Manager 2016 SP1. Wanneer de functie nog steeds aanwezig is in Microsoft Identity Manager is, is het nog steeds ondersteund. Functies worden niet aanbevolen voor nieuwe implementaties als ze in een versie van de functie kunnen worden verwijderd.  Voor ontwikkelaars, wordt aangeraden geen gebruik van afgeschafte functies in de nieuwe toepassingen of oplossingen.

>[!NOTE]
Functies en functionaliteiten in de Microsoft Identity Manager SP1 hebt verwijderd, worden aangeduid met **. <br>
Voor meer informatie over de ondersteuning [levenscyclus voor Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

Klanten beginnen nieuwe implementaties van de Microsoft BHOLD-Suite-onderdelen wordt niet aanbevolen. Bestaande implementaties van BHOLD blijven worden ondersteund. Nu Azure AD levert [toegang tot beoordelingen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) die enkele van de functies van BHOLD attestation campagne vervangen.

## <a name="certificate-management"></a>Certificaatbeheer 
| **Categorie**                | **Afgeschafte functie**              | **Vervangen en -opmerkingen**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Beheeragents | ** FIM Certificate Management | FIM Certificate Management Agent is verwijderd uit MIM 2016.                                                             |

## <a name="service-and-portal"></a>Service en portal

| **Categorie**                | **Afgeschafte functie**              | **Vervangen en -opmerkingen**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmatische configuratie | Webservice-configuratie-interface (ma-gegevens en mv-gegevens) | De mogelijkheid voor het configureren van de FIM-synchronisatieservice via FIM service-webservice wordt verwijderd in een volgende versie.
|

## <a name="synchronization-service"></a>Synchronisatieservice 

| **Categorie**                | **Afgeschafte functie**              | **Vervangen en -opmerkingen**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmatische configuratie | Webservice-configuratie-interface | De mogelijkheid voor het configureren van de FIM-synchronisatieservice via de FIM-service wordt verwijderd in een volgende versie.                                                          |
| Beheeragents           | Ingebouwde MAs                        | De volgende MAs zijn in MIM 2016 verwijderd: </br> 1. ** MA voor FIM Certificate Management </br>2. ** MA voor Lotus Notes</br> 3. ** MA voor SAP R/3 </br> Lotus Notes en SAP R/3 MAs zijn vervangen door nieuwe versies. Zie voor meer informatie [nieuwste Connector versiegeschiedenis van Release en downloaden](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Beheeragents           | ECMA1                               | Het ECMA1/XMA uitbreidbaarheid-framework is vervangen door de ECMA 2.0. Bijwerken van bestaande ECMA1 beheeragents met ECMA2.0 connectors is vereist.                                                                                                                                          |
| Beheeragents           | Connectors out-of-proc uitgevoerd      | Deze functie wordt niet vervangen. De synchronisatieservice belt altijd de connector in hetzelfde proces. Het is de verantwoordelijkheid van de connector starten en het andere proces beheren. |
| Beheeragents           | Weergavenaam van de partitie configureren    | Deze functie wordt niet vervangen. Deze optie is alleen gebruikt voor een alternatieve naam voor een partitie in de WMI-interfaces.                                                                                                                                                                       |
| Profielen uitvoeren                | Gecombineerde profielen                   | De gecombineerde profielen importeren/Deltasynchronisatie, volledige import-/ Deltasynchronisatie en volledige import/synchronisatie wordt verwijderd. U moet gebruiken uitvoeringsprofielen met twee stappen. 

>[!NOTE]
U moet de gecombineerde uitvoeringsprofielen alleen in omgevingen waar de prestaties zou worden beïnvloed door een groot aantal bestaande disconnectors bewaren.


| **Categorie**                | **Afgeschafte functie**              | **Vervangen en -opmerkingen**           |
|--------|-------|---|    
| Kenmerkprioriteit | Multi-kennis/gelijke prioriteit                       | Dezelfde prioriteit worden verwijderd. Er is geen vervanging voor deze functie. In plaats daarvan moet u handmatig prioriteit configureren. U kunt blijven deze functie gebruiken als uw omgeving een beheeragent FIM-Service is geïmplementeerd heeft. Deze beheeragent biedt geen handmatige voorrang om te voorkomen dat de export niet prioriteit voor declaratieve inrichting. |
| Regels toevoegen           | Deelnemen aan op '' objecttype                             | Deze functie wordt niet vervangen. Alle join-regels moeten expliciet definiëren voor het metaverse-objecttype dat ze probeert om lid te maken.       |
| Kenmerkstromen      | Schakel 'null-waarden toestaan' voor geëxporteerde waarden            | Deze functie wordt niet vervangen. 'Null-waarden toestaan' worden altijd geselecteerd. Zorg ervoor dat u "null-waarden toestaan hebt' geselecteerd in uw huidige omgeving.  |
| Kenmerkstromen      | "Kenmerken niet intrekken"                            | Deze functie wordt niet vervangen. Kenmerken wordt altijd worden ingetrokken, is de aanbevolen procedure.  |
| Regeluitbreiding      | Extensie out-of-proc met metaverse en ma regels | Deze functie wordt niet vervangen. De metaverse en het kenmerk stroomregels wordt in hetzelfde proces als de synchronisatie-engine uitgevoerd.       |
| Regeluitbreiding      | Transactie-eigenschappen                                | Deze functie wordt niet vervangen. Vermijd het doorgeven van gegevens tussen binnenkomende, inrichting en uitgaande synchronisatie met behulp van deze hulpprogrammaklasse.  |
| Regeluitbreiding      | ExchangeUtils: Create55\* methoden                     | De methoden voor het maken van objecten voor Exchange 5.5-servers worden verwijderd.        |
| Interface            | Mms_Metaverse                                        | Alle leden van de klasse ClmUtils wordt verwijderd in een volgende versie.   |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:

- Microsoft Identity Manager is nog nauw verwant aan diens voorganger Forefront Identity Manager. Zie [FIM 2010 R2 Documentation Roadmap](https://technet.microsoft.com/library/jj133885.aspx) (Engelstalig) als u FIM nog gebruikt of aanvullende documentatie wilt raadplegen.
- [Aandachtspunten voor topologie voor het implementeren van MIM](topology-considerations.md) in dit artikel bevat meerdere implementatietopologieën die u implementeren overwegen kunt.
- [Handleiding voor capaciteitsplanning](capacity-planning-guide.md) kunt u deze handleiding en testomgevingen om te begrijpen van het juiste bereik voor uw implementatie.
