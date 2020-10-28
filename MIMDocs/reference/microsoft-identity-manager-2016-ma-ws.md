---
title: Overzicht van de algemene webservice-connector | Microsoft Docs
description: Overzicht van de configuratie en vereisten voor de generic web service-connector.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: b0d4cc9ac9b9ac080038d632df0c02c4c244e3d6
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758899"
---
# <a name="overview-of-the-generic-web-service-connector"></a>Overzicht van de algemene webservice-connector

De web service connector integreert identiteiten via web service-bewerkingen met Microsoft Identity Manager (MIM) 2016 SP1. De connector vereist dat het project bestand van de webservice verbinding maakt met de juiste gegevens bron. Dit project kan worden gedownload uit het [micro soft Download centrum](https://go.microsoft.com/fwlink/?LinkID=235883) , samen met [documentatie](https://www.microsoft.com/en-us/download/details.aspx?id=29943) over het gebruik van de connector met Oracle eBusiness, Oracle People Soft en SAP. U kunt deze ook maken met behulp van het hulp programma voor configuratie van webservices.

Wanneer de MIM-synchronisatie service de web service-Connector aanroept, wordt het geconfigureerde project bestand ( **WsConfig** -bestand) geladen. Dit bestand helpt het het eind punt van de gegevens bron te herkennen dat moet worden gebruikt om een verbinding tot stand te brengen. Het bestand geeft er ook de werk stroom over die moet worden uitgevoerd om een MIM-bewerking te kunnen implementeren. Als u de geconfigureerde werk stromen wilt uitvoeren, maakt de web service-connector gebruik van de .NET 4 Workflow Foundation run time engine.

![Werkstroom](media/microsoft-identity-manager-2016-ma-ws/workflow.png)

## <a name="web-service-layers"></a>Web-Service lagen

Er worden twee hoofd lagen gebruikt voor het implementeren van de Web Service Management Agent (MA)-oplossing: 

- Hulp programma voor configuratie van webservices
- Run-time connector geïmplementeerd met werk stroom .NET 4,0

## <a name="supported-data-sources-for-web-service-discovery"></a>Ondersteunde gegevens bronnen voor detectie van webservices

Het hulp programma voor configuratie van webservices implementeert de volgende functies:

- SOAP-detectie: de beheerder kan een WSDL-pad invoeren dat wordt weer gegeven door de webservice van het doel. Detectie produceert een boom structuur van de gehoste webservices met hun binnenste eind punt (en)/Operations samen met de meta gegevens beschrijving van de bewerking. Er is geen limiet voor het aantal detectie bewerkingen dat kan worden uitgevoerd (stap voor stap). De gedetecteerde bewerkingen worden later gebruikt voor het configureren van de stroom van bewerkingen die de bewerkingen van de connector implementeren voor de gegevens bron (als import/export/wacht woord).

- REST detectie: de beheerder kan Details van de rest-service invoeren, d.w.z. service-eind punt, bronpad, methode en parameter Details. Een gebruiker kan een onbeperkt aantal rest Services toevoegen. De gegevens van de rest-services worden opgeslagen in het ```discovery.xml``` bestand ```wsconfig``` project. Ze worden later door de gebruiker gebruikt voor het configureren van de activiteit van de rest-webservice in de werk stroom.

- Configuratie van connector ruimte-schema: Hiermee kan de beheerder het ruimte-schema voor de connector configureren. De schema configuratie bevat een lijst met object typen en kenmerken voor een specifieke implementatie. De beheerder kan de object typen opgeven die worden ondersteund door de web service MA. De beheerder kan hier ook de kenmerken kiezen die deel uitmaken van het ruimte schema voor de connector.

- Configuratie van de bewerkings stroom: gebruikers interface van werk stroom ontwerper voor het configureren van de implementatie van FIM-bewerkingen (import/export/Password) per object type via beschik bare functies voor webservice-bewerkingen, zoals:

    - Toewijzing van para meters van connector ruimte naar web service-functies.
    - Toewijzing van para meters van web service-functies aan de connector ruimte.

## <a name="resources-generated-by-the-web-service-configuration-tool"></a>Bronnen die worden gegenereerd door het hulp programma voor configuratie van webservices

Het hulp programma voor configuratie van webservices genereert de benodigde resources voor het configureren van een volledig functionele web service MA, waaronder:

- Connector ruimte schema: een binair bestand dat de schema configuratie bevat. Het bestand wordt door MIM geïmporteerd via de ```Get Schema``` interface wanneer de ma is geconfigureerd met de gebruikers interface van de FIM-synchronisatie. Deze wordt vervolgens geconverteerd naar het object ECMA2-schema-indeling.

- Werk stromen: een reeks werk stroom definities. Ze worden tijdens runtime door de web service-MA gebruikt om een juiste bewerking uit te voeren.

- WCF-configuratie bestand: een configuratie bestand dat wordt geproduceerd door de detectie bewerking. Het bestand bevat de binding-en eind punt gegevens die zijn vereist om een webservice-bewerking voor de gegevens bron te kunnen aanroepen.

- Data contract-assembly: aangezien de web service-connector nu ondersteuning biedt voor zowel de SOAP-als de REST-service, worden de door de gegenereerde gegevens contracten in het generated.dll-bestand verschillend.

- SOAP-assembly: tijdens het parseren van de WSDL-invoer genereert het hulp programma voor configuratie van webservices gegevens contract typen. Dit zijn gegevens structuren die worden gebruikt door de webservice-bewerkingen om te communiceren met de externe service. Deze contract typen worden ook gebruikt om externe gegevens bron entiteiten beschikbaar te maken voor kenmerk toewijzing van object type.

- REST-assembly: tijdens het parseren van een voorbeeld aanvraag-antwoord voor de REST-webservice, genereert het configuratie hulpprogramma typen (klassen) die worden gebruikt in de werk stroom om te communiceren met de webservice via de oproep activiteit voor webservices. Elke aanvraag/antwoord wordt gedefinieerd in de eigen naam ruimte. De naam ruimte heeft een syntaxis als \< service naam \> . \< ResourceName \> . \< MethodName \> . [ Aanvraag/antwoord]. Bij het inpakken van elke aanvraag/antwoord in een afzonderlijke naam ruimte kunnen problemen worden verminderd door de naam van een dubbel type (klasse).

![Werkstroom](media/microsoft-identity-manager-2016-ma-ws/workflow2.png)

### <a name="project-file-type"></a>Project bestands type

De web service-MA wordt opgeslagen in een gecomprimeerd bestand (ZIP-indeling) met de naam die is opgegeven door de gebruiker en de bestands extensie ' WsConfig '. De bestands extensie ' WsConfig ' is geregistreerd en gekoppeld aan het hulp programma voor configuratie van webservices door Installer. Bestaande MA-projecten kunnen worden geopend, gewijzigd en opgeslagen. Ze kunnen worden opgeslagen in de map FIM-synchronisatie service-extensies of op een andere locatie. Wijzigingen die betrekking hebben op object type en kenmerken, moeten worden gesynchroniseerd op FIM-zijde.  Het configuratie hulpprogramma is een toepassing met meerdere exemplaren die is ontworpen om MA (en) te maken en te wijzigen.

## <a name="supported-security-modes"></a>Ondersteunde beveiligings modi

De REST/SOAP-webservice-toepassing kan worden beveiligd met een webserver zoals IIS. Met de toepassing kan de gebruiker de beveiligings modus selecteren, zoals wordt weer gegeven in de volgende afbeelding. De beveiligings modi omvatten Basic, Digest, Certificate, Windows of None.

![Beveiligings modi](media/microsoft-identity-manager-2016-ma-ws/security-mode.png)

## <a name="supported-data-types"></a>Ondersteunde gegevenstypen

De volgende gegevens typen worden ondersteund:

- SOAP (verouderd): het SOAP-gegevens type wordt ondersteund zoals beschreven in dit [MSDN-artikel](https://msdn.microsoft.com/library/ms995800.aspx). Ondersteuning wordt alleen geboden voor de alleen de BAPI-stack (Business Application Programming Interface). Voor beelden van SOAP-sjablonen zijn beschikbaar in het [micro soft Download centrum](https://www.microsoft.com/en-us/download/details.aspx?id=51495).
- REST (niet ODATA): een op HTTP-protocol gebaseerde connector/web.

## <a name="next-steps"></a>Volgende stappen 

- [Installeer het hulp programma voor configuratie van webservices](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP-implementatie handleiding](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Hand leiding voor REST-implementatie](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuratie van de web service-MA](microsoft-identity-manager-2016-ma-ws-maconfig.md)
