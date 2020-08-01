---
title: Afgeschafte functies van MIM en planning voor de toekomst | Microsoft Docs
description: In dit artikel worden afgeschafte functies van MIM Identity Manager 2016 SP2 gedocumenteerd.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 7/28/2020
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: cb0159ec70e161dc47140712bd2ac039786e034d
ms.sourcegitcommit: 50cee02a7146445bd6fa361349099c7b294792d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2020
ms.locfileid: "87443543"
---
# <a name="deprecated-features"></a>Afgeschafte functies

In dit artikel worden de afgeschafte functies van Microsoft Identity Manager 2016 SP2 beschreven. Wanneer de functie nog steeds aanwezig is in Microsoft Identity Manager, wordt deze nog steeds ondersteund. Functies worden niet aanbevolen voor nieuwe implementaties, omdat deze mogelijk worden verwijderd in een toekomstige hotfix of een Service Pack versie.  Voor ontwikkel aars wordt u aangeraden geen afgeschafte functies te gebruiken in nieuwe toepassingen of oplossingen.

> [!NOTE]
>
> Voor meer informatie over de nieuwe ondersteunings opties voor MIM raadpleegt u [ondersteunings opties voor klanten van Azure AD Premium](support-update-for-azure-active-directory-premium-customers.md).

## <a name="bhold"></a>BHOLD

Micro soft raadt klanten niet aan nieuwe implementaties van de onderdelen van micro soft BHOLD Suite te starten. Bestaande implementaties van BHOLD worden nog steeds ondersteund. Azure AD biedt nu [toegangs beoordelingen](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), waarmee de functies van de BHOLD Attestation-campagne en het rechten beheer worden vervangen, waardoor de functies voor de toegangs toewijzing worden vervangen.

## <a name="service-and-portal"></a>Service en portal

| **Categorie**                | **Afgeschafte functie**              | **Opmerking**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmatische configuratie van synchronisatie | Configuratie-interface van webservice (ma-data en MV-data) | De mogelijkheid om de MIM-synchronisatie service via de MIM service-webservice te configureren, kan worden verwijderd in een toekomstige hotfix of een service pack.
|

## <a name="synchronization-service"></a>Synchronisatieservice 

De volgende MAs zijn verwijderd in MIM 2016: </br> 1. MA voor FIM-certificaat beheer </br>2. MA voor Lotus Notes</br> 3. MA voor SAP R/3 </br> De Lotus Notes-en SAP R/3-MAs zijn vervangen door nieuwe connectors. Zie [nieuwste connector versie release History & down load](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)voor meer informatie.

Het ECMA1/XMA Extensibility Framework is vervangen door ECMA 2,0. Het bijwerken van bestaande ECMA1-beheer agenten met ECMA 2.0-connectors is vereist.

| **Categorie**                | **Afgeschafte functie**              | **Opmerking**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Beheer agenten           | Connectors out-of-process uitvoeren      | De synchronisatie service roept de connector in hetzelfde proces altijd aan. Het is de verantwoordelijkheid van de connector om het andere proces te starten en te beheren. |
| Beheer agenten           | Weergave naam van partitie configureren    | Deze optie is alleen gebruikt om een alternatieve naam op te geven voor een partitie in de WMI-interfaces.                                                                                                                                                                       |
| Profielen uitvoeren                | Gecombineerde profielen                   | De gecombineerde profielen verschillen per Importeer/synchronisatie, volledige import/Delta-synchronisatie en volledige import/Sync kunnen worden verwijderd. Gebruik in plaats daarvan run-profielen met twee stappen.

> [!NOTE]
> Houd alleen gecombineerde uitvoerings profielen in omgevingen waar de prestaties worden beïnvloed door een groot aantal bestaande disconnecters.

| **Categorie**                | **Afgeschafte functie**              | **Opmerking**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Kenmerkprioriteit | Meerdere masters/gelijke prioriteit                       | Een gelijke prioriteit kan worden verwijderd. U moet in plaats daarvan hand matige prioriteit configureren. U kunt deze functie blijven gebruiken als in uw omgeving een FIM-Service beheer agent is geïmplementeerd. Deze beheer agent biedt geen hand matige prioriteit om te voor komen dat export-not-broncel voor declaratief inrichten is. |
| Regels voor samen voegen           | Toevoegen aan object type                             | Alle regels voor samen voegen moeten expliciet het object type van de tekst voor de regel die ze proberen samen te stellen.       |
| Kenmerk stromen      | Selectie van Null-waarden toestaan ongewijzigd laten            | ' Null-waarden toestaan ' wordt altijd geselecteerd, dus zorg ervoor dat u ' null-waarden toestaan ' hebt geselecteerd in uw huidige omgeving.  |
| Kenmerk stromen      | "Geen kenmerken intrekken"                            | Kenmerken worden altijd ingetrokken, wat de best practice is.  |
| Extensie voor regels      | De uitbrei ding voor het uitvoeren van een tekst-en VG-regel out-of-process | De regels voor de omgekeerde en kenmerk stroom worden uitgevoerd in hetzelfde proces als de synchronisatie-engine.       |
| Extensie voor regels      | Eigenschappen van trans actie                                | Vermijd het door geven van gegevens tussen inkomend, provisioning en uitgaande synchronisatie met deze hulpprogramma klasse.  |
| Extensie voor regels      | ExchangeUtils: Create55- \* methoden                     | De methoden voor het maken van objecten voor Exchange 5,5-servers kunnen worden verwijderd.        |
| Interface            | Mms_Metaverse                                        | Alle ClmUtils-klasse leden kunnen worden verwijderd in een toekomstige hotfix of Service Pack.   |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:

- Microsoft Identity Manager is nog nauw verwant aan diens voorganger Forefront Identity Manager. Zie [FIM 2010 R2 Documentation Roadmap](https://technet.microsoft.com/library/jj133885.aspx) (Engelstalig) als u FIM nog gebruikt of aanvullende documentatie wilt raadplegen.
- [Overwegingen voor de topologie voor het implementeren van MIM](topology-considerations.md) In dit artikel worden meerdere implementatie topologieën geïntroduceerd die u kunt implementeren.
- [Gids voor capaciteits planning](capacity-planning-guide.md) U kunt deze hand leiding samen met test omgevingen gebruiken om inzicht te krijgen in het juiste bereik voor uw implementatie.

