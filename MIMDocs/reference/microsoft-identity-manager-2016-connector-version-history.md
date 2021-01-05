---
title: Release geschiedenis van connector versie | Microsoft Docs
description: In dit onderwerp vindt u alle versies van de connectors voor Forefront Identity Manager (FIM) en Microsoft Identity Manager (MIM)
services: active-directory
documentationcenter: ''
author: EugeneSergeev
manager: aashiman
editor: ''
reviewer: markwahl-msft
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/31/2020
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 84eae9e65a2ea65c210e026ccafa58d95c434539
ms.sourcegitcommit: 36752980300a51a0b30442ea23b9934eb8b5c752
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/31/2020
ms.locfileid: "97835309"
---
# <a name="connector-version-release-history"></a>Releasegeschiedenis van connectorversie

Connectors koppelen specifieke verbonden gegevens bronnen aan Microsoft Identity Manager (MIM) en Azure AD Connect. (In Forefront Identity Manager werden connectors aangeduid als beheer agenten.) Veel van de connectors, zoals connectors voor het inrichten van gebruikers in Active Directory, worden geleverd als onderdeel van de installatie van de MIM-synchronisatie service en het installatie pakket van Azure AD Connect. Daarnaast worden meer connectors, zoals op adreslijst servers van derden, verzonden als afzonderlijke down loads, zodat ze regel matig meer kunnen worden bijgewerkt om ondersteuning toe te voegen voor het maken van verbinding met MIM-bijgewerkte versies van doel systemen van derden.  

> [!NOTE]
> Dit onderwerp is voornamelijk op FIM-en MIM-connectors. Tenzij hieronder uitdrukkelijk wordt genoemd, worden deze connectors niet ondersteund voor installatie op Azure AD Connect. Uitgegeven connectors zijn vooraf geïnstalleerd op Azure AD Connect bij het upgraden naar de opgegeven build.


In dit onderwerp vindt u een lijst met alle versies van het pakket algemene connectors die los van MIM zijn vrijgegeven.  Zie [ondersteunde connectors in MIM 2016 SP1](../supported-management-agents.md)voor een lijst met connectors die worden ondersteund met mim.  Sommige partners hebben op deze manier hun eigen connectors gemaakt en een volledige lijst is beschikbaar in de wiki [FIM 2010 en MIM 2016: beheer agenten van partners](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-mim-2016-management-agents-from-partners.aspx).


Gerelateerde links:

* [Nieuwste connectors downloaden](https://go.microsoft.com/fwlink/?LinkId=717495)
* Naslag documentatie voor [algemene LDAP-connectors](microsoft-identity-manager-2016-connector-genericldap.md)
* Naslag documentatie voor [algemene SQL-connectors](microsoft-identity-manager-2016-connector-genericsql.md)
* Naslag documentatie voor [Web Services-connectors](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws)
* Naslag documentatie voor [Power shell-connector](microsoft-identity-manager-2016-connector-powershell.md)
* Referentie documentatie voor [Lotus Domino-connector](microsoft-identity-manager-2016-connector-domino.md)
* Naslag documentatie voor [share point-gebruikers profiel archief-connector](https://go.microsoft.com/fwlink/?LinkID=331344)

## <a name="1113470-december-2020"></a>1.1.1347.0 (december 2020)
### <a name="fixed-issues"></a>Opgeloste problemen
- Graph-connector
  - Er is een probleem opgelost met een connector die onjuiste B2B-uitnodigingen verzendt bij het maken van een e-mail groep of contact persoon

## <a name="1113460-november-2020"></a>1.1.1346.0 (november 2020)
### <a name="fixed-issues"></a>Opgeloste problemen
- Graph-connector
  - Er is een probleem opgelost met een beschadiging van de lokale connector cache waardoor uitvoer fouten van Delta-import worden veroorzaakt
  - Er is een probleem opgelost met dubbele vermeldingen die zijn gerapporteerd door de connector tijdens de volledige import uitvoering, waardoor detectie fouten optreden
  - Er is een probleem opgelost met een onjuiste import van complexe gegevens typen, bijvoorbeeld *employeeOrgData*
- Algemene SQL-connector
  - Er is een probleem opgelost met een SQL Native verificatie fout vanwege een DSN-connection string eigenschap *TrustedConnection* ingesteld op *False* 
- Algemene LDAP-connector
  - Er is een probleem opgelost met het verwerken van *OpenLDAP* *accessLog* -vermeldingen bij het importeren van verschillen waardoor onjuiste groepslid maatschappen worden gewijzigd en andere fouten

## <a name="1113020-september-2020"></a>1.1.1302.0 (september 2020)
### <a name="fixed-issues"></a>Opgeloste problemen
- Graph-connector
  - Er is een probleem opgelost met het vernieuwen van het schema voor */beta* -eind punt

## <a name="1113010-august-2020"></a>1.1.1301.0 (augustus 2020)
### <a name="fixed-issues"></a>Opgeloste problemen
- Graph-connector
  - Er is een fout opgelost met mislukte import pogingen vanwege een lege *User type* -kenmerk waarde
  - Er is een fout opgelost met de connector cache van Delta-import fouten die niet leesbaar is vanwege detectie fouten
  - De gerapporteerde time-outs van de service en proxy kunnen automatisch opnieuw worden geprobeerd
  - De schema detectie voor het */beta* -eind punt is bijgewerkt 
### <a name="enhancements"></a>Verbeteringen 
- Graph-connector
  - Ondersteuning toegevoegd voor lees-en schrijf waarden van aangepaste Directory-extensie kenmerken
  - Ondersteuning toegevoegd voor het lezen van *Service-Principal* leden van groepen in het */beta* -eind punt 
  - Prestatie verbeteringen voor Delta-import uitvoeringen in verband met schema detectie
  - Graph connector kan nu externe leden van een gebruiker uitnodigen

  > [!NOTE]
  > Als u de uitnodiging voor het maken van een gast in Build 1.1.1170.0 van de connector hebt gebruikt, moet u de synchronisatie regels bijwerken met de volgende logica:

  - Uitgaande stromen
    - Er wordt een gebruiker uitgenodigd bij het exporteren van de gebruiker en de export bevat een *mail-* kenmerk, maar geen *userPrincipalName-kenmerk*.  Als de *userPrincipalName* is opgegeven, wordt er een gebruiker gemaakt in plaats van uitgenodigd
    - Het kenmerk *User type* definieert alleen of een gebruiker een *lid* wordt of een *gast* (de standaard instelling is een *lid* als dit niet is ingesteld)
  - Binnenkomende stromen
    - *UserPrincipalName* kenmerk waarden van externe gebruikers worden weer gegeven als is

## <a name="1111700-april-2020"></a>1.1.1170.0 (2020 april)
### <a name="fixed-issues"></a>Opgeloste problemen
- Algemene SQL-connector
   - Er is een fout opgelost met het op query's gebaseerde export strategie en kenmerken voor de kenmerking met meerdere waarden
- Lotus Notes-Connector
   - Groepen uit secundaire Notes-adres boeken worden niet langer verwijderd door het *AdminP* -proces. De bewerking direct verwijderen wordt nu gebruikt
- Algemene LDAP-connector
   - Er is een fout opgelost met kenmerken van LDAP-Directory bewerkingen, bijvoorbeeld *pwdUpdateTime*, niet zichtbaar in schema
### <a name="enhancements"></a>Verbeteringen 
- Graph-connector   
   - Upn's van externe gast gebruikers worden niet meer weer gegeven als-is. in plaats daarvan worden ze weer gegeven in connector ruimte om te zien als e-mail berichten
   - Ondersteuning toegevoegd voor B2B-gast gebruikers inrichten

   Als u B2B-gast gebruikers wilt uitnodigen, moet u het volgende doen:
   - Machtigingen verlenen om gasten uit te nodigen voor uw Azure AD-toepassing die is gekoppeld aan Graph connector
   - De sectie connector configuratie volt ooien voor het uitnodigen van externe gebruikers: Stel de omleidings-URL voor uitnodigen (verplicht) in en kies of u e-mail berichten wilt verzenden
   - Stel de verplichte kenmerken in uw regel voor uitgaande synchronisatie in:
     - ' Gast ' =>*User type* (alleen eerste stroom)
     - extern e-mail adres =>*userPrincipalName*
     - CustomExpression ("CN =" + csObjectID + ", OBJECT = User") =>*DN* (alleen eerste stroom)
     - csObjectID =>*id* (alleen eerste stroom)

## <a name="1111300-february-2020"></a>1.1.1130.0 (februari 2020)
### <a name="fixed-issues"></a>Opgeloste problemen
- Graph-connector
   - Exporteren mislukt niet meer wanneer u een lid probeert toe te voegen aan een groep die al is toegevoegd
   - de ondersteuning voor virtuele kenmerken van export_password is verholpen. u hoeft de kenmerk stroom van het wacht woord niet meer in te stellen
   - Vaste export stroom van teken reeks kenmerken met meerdere waarden
   - Er zijn verschillende bugs opgelost die van invloed zijn op Delta-import
   - Verschillende mogelijke datum-en tijd notatie fouten opgelost
- Algemene SQL-connector
   - Verschillende fouten van de gebruikers interface zijn opgelost
   - Verwerkte onjuiste verwijzingen verwerken tijdens Delta-import
   - Een bug met SQL Wijzigingen bijhouden Delta-import strategie en tabellen met meerdere waarden opgelost om de wijzigingen in het groepslid maatschap correct te importeren
   - Er is een fout opgelost met kenmerk waarden die niet worden gewist bij het exporteren
   - Er is een fout opgelost met het laatste element van het verwijzings kenmerk met meerdere waarden dat niet wordt verwijderd bij het exporteren
   - Een bug met schema vernieuwing opgelost, waardoor referentie kenmerken worden ingesteld op teken reeksen
   - Er is een fout opgelost met de waarden van de opgeslagen procedure parameters afgekapt tot 397 bytes
   - Er is een fout opgelost met Oracle-tabellen en-weer gaven schema detectie is hoofdletter gevoelig
- Lotus Notes-Connector
   - Prestaties verbeterd bij het importeren van groeps leden
   - Volledige import mislukt niet langer met ' null-referentie fouten '
   - Fout tijdens het verwijderen van een notitie postvak als ACL is ingesteld
   - Lege groeps namen veroorzaken geen Delta-import fouten
   - Er is een fout opgelost met niet-afdruk bare tekens die zijn achtergebleven in kenmerken na het verwijderen van teken reeks waarden
- Algemene LDAP-connector
   - Bij het importeren van verschillen wordt letterlijke waarde vervangen niet meer weer gegeven als er geen waarde is ingesteld in de bron-wijzigingen logboek
### <a name="enhancements"></a>Verbeteringen 
- Graph-connector
   - Er is ondersteuning toegevoegd voor soevereine Clouds en de mogelijkheid om aanmeldings-en grafiek bron-Url's te configureren
   - Niet-ondersteunde kenmerken worden gefilterd en verborgen in connector eigenschappen na schema detectie

## <a name="4418001-july-2019"></a>4.4.1800.1 (juli 2019)
### <a name="enhancements"></a>Opties
- Connector voor share point-gebruikers profiel archief
   - Er is ondersteuning toegevoegd voor share Point server 2019. De connector is beschikbaar als down load vanuit het [micro soft Download centrum](https://go.microsoft.com/fwlink/?linkid=279713) 

## <a name="119530-june-2019"></a>1.1.953.0 (juni 2019)

### <a name="fixed-issues"></a>Opgeloste problemen:
- Algemene LDAP-connector
   - Delta-import mislukt niet meer als het veld wijzigingen leeg is in de map Oracle Internet
- Algemene SQL-connector
   - Er is een probleem opgelost met de aangepaste strategie voor het importeren van SQL-query's met start index-en EndIndex-para meters waardoor alleen de eerste pagina wordt geïmporteerd
   - Er is een probleem opgelost met de import strategie Table/View bij het lezen van MS SQL waardoor alleen de eerste pagina wordt geïmporteerd
- Graph-connector:
   - De contact personen van de organisatie worden nu goed verwerkt, de e-mail ontbreekt niet meer
   - Er is een probleem met het importeren en exporteren van beheer kenmerken opgelost. De connector mislukt niet meer wanneer de Manager leeg is. De waarde Manager wordt op de juiste wijze bijgewerkt in azure AD
   - Er is een probleem opgelost met een Delta-import van lege waarden
   - Er is een probleem opgelost met een vastgelopen connector bij het importeren van Delta wanneer het lokale cache bestand is beschadigd
   - Er zijn verschillende problemen opgelost met een onjuiste export van lege waarden of wanneer alleen een kenmerk waarde is gewijzigd
   - Connector verwerkt nu het verwijderen/toevoegen van bewerkingen voor hetzelfde kenmerk tijdens het uitvoeren van de export.
   - Er zijn verschillende problemen opgelost met langlopende import bewerkingen en uitvoer wanneer toegangs tokens verlopen tijdens de uitvoering van de connector. Connector vernieuwt nu de toegangs tokens, indien nodig zonder fouten
   - Er is een probleem opgelost met het laatste lid van een groep dat niet wordt verwijderd
### <a name="enhancements"></a>Opties
- Algemene SQL-connector
   - de commandTimeout-para meter van een gegevens lezer is ingesteld op overeenkomen met de time-out van de connector. Als u langlopende query's hebt die meer dan 30 seconden duren, kunt u de parameter waarde ' opdracht-timeout ' verhogen in het gedeelte ' connectiviteit '
- Graph-connector: 
   - De volledige import strategie voor groepslid maatschappen met meerdere threads is toegevoegd om de import prestaties te verbeteren. Delta-import blijft een bewerking met één thread
   - Er is ondersteuning toegevoegd voor complexe schema typen die kenmerken als OnPremisesExtentionAttributes. * nu beschikbaar zijn
   - Er is ondersteuning toegevoegd voor het kenmerk export_password om te voor komen dat niet-herstelde fouten worden geïmporteerd en het oorspronkelijke wacht woord niet in de connector ruimte wordt weer gegeven. Gedrag is vergelijkbaar met andere ECMA2-connectors
   - Er is een handler toegevoegd ter ondersteuning van het beperken van HTTP-aanvragen. Wanneer Azure AD replica te veel aanvragen van een client ontvangt, reageert deze mogelijk met Retry-After instructie. De connector wordt onderbroken en opnieuw in plaats van mislukt
   - Het Delta-import profiel wordt niet meer gestart als er query filters zijn gedefinieerd. Als u alleen specifieke objecten uit Azure AD wilt importeren, bijvoorbeeld gebruikers met een achternaam die begint met een *, wordt de functionaliteit voor Delta-import geblokkeerd


## <a name="119130-january-2019"></a>1.1.913.0 \( januari 2019\)

### <a name="fixed-issues"></a>Opgeloste problemen:

* Algemene SQL:
    * Er is een regressie fout opgelost in de algemene SQL-connector waarbij de eerste 5000-objecten alleen werden gelezen.
* Graph-connector:
    * Er is een probleem opgelost waarbij de Graph API-connector geen lees-of schrijf bewerking kan uitvoeren voor een Tenant of een nieuwe connector maakt wanneer de optie Beta is geselecteerd voor de verbinding.

### <a name="enhancements"></a>Opties

* Graph-connector:
    * Stel het TLS 1,2-protocol in als de standaard waarde voor de Graph-connector.
* Algemene SQL-connector:
    * De algemene SQL-connector is getest met Oracle 12c en Oracle 18c.

## <a name="119110"></a>1.1.911.0

> [!NOTE]  
> Deze connector build-versie is alleen bedoeld voor gebruik in Azure AD Connect-implementaties en is niet voor gebruik in MIM-implementaties.
> 
> Vergeleken met de vorige connector release, bevat het geen verbeteringen of updates voor MIM-klanten.

-  Hiermee werkt u de niet-standaard connectors (bijvoorbeeld generieke LDAP-connector en algemene SQL-connector) bij Azure AD Connect.  

## <a name="118610"></a>1.1.861.0

### <a name="fixed-issues"></a>Opgeloste problemen:
* Titels wijzigen voor alle connector instellingen van Forefront naar micro soft

* Lotus Notes: 
    * Fouten in COM met Lotus geven soms geen fouten weer
* Algemeen LDAP:
    * Gldap extra-symbool voor DI met PING Directory server
* Algemene webservices:
    * Fout bij het exporteren van JSON-parsering
* Algemene SQL:
    * Binair kenmerk exporteren
    * Object typen kunnen geen subtekenreeksen van elkaar zijn
    * Wijzigingen in de tabel met meerdere waarden worden niet bijgehouden in de werking van ' Delta-import ', als ' Delta strategie ' Wijzigingen bijhouden is
* Graph connector (open bare preview)
    * Fout bij verwijderen van groep
    * User-Agent bijwerken naar http-header
    * De connector valideert de client-ID en het client geheim niet
    * De naam van de Tenant moet worden bijgesneden

### <a name="enhancements"></a>Opties
* Lotus Notes: * de mogelijkheid toevoegen om de time-out te verhogen via de gebruikers interface
* Graph connector (open bare preview) * het wachtwoord kenmerk wordt gefilterd bij het importeren om te voor komen dat de export niet opnieuw is geïmporteerd.
    * Voeg ondersteuning toe van $filter query parameter: beperkt tot bewerkingen met alle filters die in de Delta query werken, werkt ook in de connector * is bijgewerkt om nextLink direct te gebruiken in plaats van Skip token te [](https://developer.microsoft.com/en-us/graph/docs/concepts/paging) extra heren voor paginerings gegevens

## <a name="118300"></a>1.1.830.0

### <a name="fixed-issues"></a>Opgeloste problemen:
* ConnectorsLog System. Diagnostics. EventLogInternal. InternalWriteEvent (bericht: een apparaat dat aan het systeem is gekoppeld, werkt niet)
* In deze versie van connectors moet u bindings omleiding bijwerken van 3.3.0.0-4.1.3.0 naar 4.1.4.0 in miiserver.exe.config
* Algemene webservices:
    * Het omgezette geldige JSON-antwoord kan niet worden opgeslagen in het configuratie hulpprogramma
* Algemene SQL:
    * Bij het exporteren wordt altijd alleen een bijwerk query gegenereerd voor de bewerking van het verwijderen. Toegevoegd voor het genereren van een Verwijder query
    * De SQL-query, waarmee objecten worden opgehaald voor de bewerking van Delta-import, als ' Delta strategie ' is ingesteld op ' Wijzigingen bijhouden ' is opgelost. In deze implementatie bekende beperking: Delta-import met de modus Wijzigingen bijhouden houdt geen wijzigingen in kenmerken met meerdere waarden bij
    * Mogelijkheid tot het genereren van een Verwijder query voor case, wanneer het nodig is om de laatste waarde van het kenmerk met meerdere waarden te verwijderen en deze rij bevat geen andere gegevens behalve de waarde die moet worden verwijderd.
    * Verwerking van System. ArgumentException bij het implementeren van uitvoer parameters per SP
    * Onjuiste query voor het uitvoeren van de bewerking voor het exporteren naar het veld met het type varbinary (max)
    * Probleem met parameterList-variabele is twee maal geïnitialiseerd (in de functies ExportAttributes en GetQueryForMultiValue)


## <a name="116490-aadconnect-116490"></a>1.1.649.0 (AADConnect 1.1.649.0)

### <a name="fixed-issues"></a>Opgeloste problemen:

* Lotus Notes:
  * Optie aangepaste certificeert filteren
  * Bij het importeren van de klasse ImportOperations is de definitie van de bewerkingen die kunnen worden uitgevoerd in de modus views en in de zoek modus opgelost.
* Algemeen LDAP:
  * OpenLDAP Directory gebruikt DN als anker in plaats van entryUUI. Nieuwe optie voor GLDAP-connector, waarmee anker kan worden gewijzigd
* Algemene SQL:
  * Vast veld voor exporteren naar, met het type varbinary (max).
  * Bij het toevoegen van binaire gegevens van een gegevens bron aan een CSEntry-object, is de functie DataTypeConversion mislukt op nul bytes. De functie Fixed DataTypeConversion van de klasse CSEntryOperationBase.




### <a name="enhancements"></a>Opties

* Algemene SQL:
  * De mogelijkheid om de modus voor het uitvoeren van een opgeslagen procedure met benoemde para meters of niet met de naam, te configureren, wordt toegevoegd in een configuratie venster van de algemene SQL Management-Agent in de pagina globale para meters. In de pagina globale para meters is er een selectie vakje met het label ' gebruik benoemde para meters voor het uitvoeren van een opgeslagen procedure ', die verantwoordelijk is voor de modus voor het uitvoeren van een opgeslagen procedure met benoemde para meters.
    * De mogelijkheid om een opgeslagen procedure met benoemde para meters uit te voeren, werkt momenteel alleen voor data bases van IBM DB2 en MSSQL. Voor data bases die Oracle en MySQL gebruiken, werkt deze methode niet: 
      * De SQL-syntaxis van MySQL biedt geen ondersteuning voor benoemde para meters in opgeslagen procedures.
      * Het ODBC-stuur programma voor de Oracle ondersteunt geen benoemde para meters voor benoemde para meters in opgeslagen procedures.

## <a name="116040-aadconnect-116140"></a>1.1.604.0 (AADConnect 1.1.614.0)


### <a name="fixed-issues"></a>Opgeloste problemen:

* Algemene webservices:
  * Er is een probleem opgelost waardoor een SOAP-project niet kan worden gemaakt wanneer er twee of meer eind punten zijn.
* Algemene SQL:
  * Bij het importeren van de GSQL was de tijd niet correct geconverteerd, wanneer deze wordt opgeslagen in de connector ruimte. De standaard notatie voor de datum en tijd voor de connector ruimte van de GSQL is gewijzigd van JJJJ-MM-DD uu: mm: ssZ naar JJJJ-MM-DD uu: mm: ssZ.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Opgeloste problemen:

* Algemene webservices:
  * Het hulp programma WSconfig is niet op de juiste wijze geconverteerd van de JSON-matrix van de voorbeeld aanvraag voor de REST service methode. Dit veroorzaakte problemen met serialisatie van deze JSON-matrix voor de REST-aanvraag.
  * Het configuratie hulpprogramma voor de webservice-connector biedt geen ondersteuning voor het gebruik van spatie symbolen in JSON-kenmerk namen 
    * Een vervangings patroon kan hand matig worden toegevoegd aan het WSConfigTool.exe.config bestand, bijvoorbeeld  ```<appSettings> <add key="JSONSpaceNamePattern" value="__" /> </appSettings>```
      > [!NOTE]
      > De JSONSpaceNamePattern-sleutel is vereist voor het exporteren van de volgende fout melding wordt weer gegeven: bericht: de lege naam is niet geldig. 

* Lotus Notes:
  * Als de optie **aangepaste certificeerers toestaan voor organisatie/organisatie-eenheden** is uitgeschakeld, mislukt de connector tijdens het exporteren (update) nadat de export stroom alle kenmerken zijn geëxporteerd naar Domino, maar op het moment van het exporteren van een KeyNotFoundException wordt geretourneerd naar de synchronisatie. 
    * Dit gebeurt omdat de naamswijziging mislukt wanneer wordt geprobeerd DN (kenmerk van de gebruikers naam) te wijzigen door een van de onderstaande kenmerken te wijzigen:  
      - LastName
      - FirstName
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - OE
      - altcommonname

  * Wanneer de optie **aangepaste certificeerers toestaan voor organisatie/organisatie-eenheden** is ingeschakeld, maar de vereiste certificeert nog steeds leeg zijn, wordt KeyNotFoundException uitgevoerd.

### <a name="enhancements"></a>Opties

* Algemene SQL:
  * **Scenario: opnieuw ontwerpen geïmplementeerd:** "*"-functie
  * **Beschrijving van oplossing:** De methode voor [het verwerken van verwijzings kenmerken met meerdere waarden](microsoft-identity-manager-2016-connector-genericsql.md)is gewijzigd.


### <a name="fixed-issues"></a>Opgeloste problemen:

* Algemene webservices:
  * Kan de server configuratie niet importeren als de WebService-connector aanwezig is
  * De WebService-connector werkt niet met meerdere webservices

* Algemene SQL:
  * Er worden geen object typen vermeld voor het kenmerk met één waarde waarnaar wordt verwezen
  * Delta-import Wijzigingen bijhouden-strategie object wordt verwijderd wanneer de waarde wordt verwijderd uit de tabel met meerdere waarden
  * OverflowException in GSQL-connector met DB2 op als/400

Lotus
  * Optie toegevoegd om Enable\Disable te zoeken voordat de GlobalParameters-pagina wordt geopend

## <a name="114430"></a>1.1.443.0

Uitgebracht: 2017 maart

### <a name="enhancements"></a>Verbeteringen 

* Algemene SQL:</br>
  **Scenario symptomen:**  Het is een bekende beperking met de SQL-connector waarbij alleen een verwijzing naar een object type is toegestaan en kruis verwijzingen met leden zijn vereist. </br>
  **Beschrijving van oplossing:** In de optie verwerkings stap voor verwijzingen zijn ' * ' geselecteerd, worden alle combi Naties van object typen teruggestuurd naar de synchronisatie-engine.

> [!Important]
> - Hiermee worden veel tijdelijke aanduidingen gemaakt
> - Het is vereist om ervoor te zorgen dat de naamgeving unieke typen Kruis objecten is.


* Algemeen LDAP:</br>
  **Scenario:** Wanneer slechts enkele containers in een specifieke partitie zijn geselecteerd, wordt de zoek opdracht nog uitgevoerd in de hele partitie. Specifiek wordt gefilterd op de synchronisatie service, maar niet door MA, waardoor de prestaties kunnen afnemen. </br>

  **Beschrijving van oplossing:** De code van de GLDAP-connector is gewijzigd, zodat het mogelijk is alle containers en zoek objecten in elk item te doorzoeken, in plaats van te zoeken in de hele partitie.


* Lotus Domino:

  **Scenario:** Ondersteuning voor het verwijderen van Domino mail voor een persoon tijdens het exporteren. </br>
  **Oplossing:** Configureer bare ondersteuning bij het verwijderen van e-mail voor het verwijderen van een persoon tijdens het exporteren.

### <a name="fixed-issues"></a>Opgeloste problemen:
* Algemene webservices:
  * Wanneer u de service-URL in standaard-SAP WSconfig-projecten wijzigt via het hulp programma voor configuratie van de WebService, treedt de volgende fout op: kan een deel van het pad niet vinden

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* Algemeen LDAP:
  * GLDAP-connector ziet niet alle kenmerken in AD LDS
  * Wizard wordt onderbroken wanneer er geen UPN-kenmerken worden gedetecteerd vanuit het LDAP-adreslijst schema
  * Delta-import bewerking mislukt met detectie fouten die niet aanwezig zijn tijdens volledige import bewerking, wanneer het kenmerk object Class niet is geselecteerd
  * Op de configuratie pagina voor het configureren van partities en hiërarchieën worden geen objecten weer gegeven die gelijk zijn aan de partitie voor nieuwe servers in het algemeen  
  LDAP-MA. Er zijn alleen objecten uit de RootDSE-partitie weer gegeven.


* Algemene SQL:
  * Oplossing voor het algemene SQL-water merk met een kenmerk met meerdere waarden dat geen fout heeft geïmporteerd
  * Bij het exporteren van deleted\added-waarden van het kenmerk met meerdere waarden, worden ze niet deleted\added in de gegevens bron.  


* Lotus Notes:
  * Een specifiek veld "volledige naam" wordt echter in de tekst correct weer gegeven. Als u exporteert naar notities, is de waarde voor het kenmerk null of leeg.
  * Oplossing voor dubbele Certificeerer fout
  * Wanneer het object zonder gegevens is geselecteerd op de Lotus Domino-connector met andere objecten, ontvangen we de detectie fout tijdens het uitvoeren van een volledige import bewerking.
  * Wanneer er Delta-import wordt uitgevoerd op de Lotus Domino-connector, wordt aan het einde van de uitvoering van de Microsoft.IdentityManagement.MA.LotusDomino.Service.exe-service soms een toepassings fout geretourneerd.
  * Groepslid maatschap werkt algemeen prima en blijft behouden, behalve wanneer het exporteren wordt uitgevoerd om een gebruiker uit het lidmaatschap te verwijderen, maar de gebruiker wordt niet daad werkelijk verwijderd uit het lidmaatschap van Lotus Notes.
  * De mogelijkheid om de modus van exporteren te kiezen als ' item onderaan toevoegen ' is toegevoegd in de configuratie-GUI van Lotus MA om nieuwe items onderaan toe te voegen tijdens het exporteren voor kenmerken met meerdere waarden.
  * Connector voegt de benodigde logica toe om het bestand te verwijderen uit de e-mailmap en de ID-kluis.
  * Lidmaatschap verwijderen werkt niet voor cross-NAB-lid.
  * De waarden moeten worden verwijderd uit het kenmerk met meerdere waarden

## <a name="111170"></a>1.1.117.0
Uitgebracht: 2016 maart

**Nieuwe connector**  
Initiële versie van de [algemene SQL-connector](microsoft-identity-manager-2016-connector-genericsql.md).

**Nieuwe functies:**

* Algemene LDAP-connector:
  * Er is ondersteuning toegevoegd voor Delta-import met Isode.
* Web Services-connector:
  * De csEntryChangeResult-activiteit en de setImportErrorCode-activiteit zijn bijgewerkt zodat object niveau fouten terug kunnen worden geretourneerd naar de synchronisatie-engine.
  * De SAP6-en SAP6User-sjablonen zijn bijgewerkt om de fout functionaliteit voor het nieuwe object niveau te gebruiken.
* Lotus Domino-connector:
  * Voor het exporteren moet u één certificeerer per adres boek hebben. U kunt nu hetzelfde wacht woord voor alle certificeerers gebruiken om het beheer te vereenvoudigen.

**Opgeloste problemen:**

* Algemene LDAP-connector:
  * Voor IBM Tivoli DS zijn sommige referentie kenmerken niet correct gedetecteerd.
  * Voor Open LDAP tijdens een Delta-import zijn witruimte aan het begin en het einde van teken reeksen afgekapt.
  * Voor Novell en NetIQ is een export die een object verplaatst tussen Ou's/containers en op hetzelfde moment de naam van het object heeft gewijzigd.
* Web Services-connector:
  * Als de webservice meerdere eind punten voor dezelfde binding heeft, heeft de connector deze eind punten niet juist gedetecteerd.
* Lotus Domino-connector:
  * Het exporteren van het kenmerk fullName naar een mail-in data base werkt niet.
  * Een export waarbij zowel een toegevoegd als een verwijderd lid uit een groep alleen de toegevoegde leden heeft geëxporteerd.
  * Als een Notes-document ongeldig is (het kenmerk isValid ingesteld op false), mislukt de connector.

## <a name="older-releases"></a>Oudere versies
Vóór 2016 maart zijn de connectors uitgebracht als ondersteunings onderwerpen.

**Algemene LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, 2015 september
* [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, 2015 maart
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, 2015 januari
* [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, 2014 september
* [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, 2014 maart

**Webservices**

* [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, 2014 september

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, 2014 september

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, 2015 september
* [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, 2015 maart
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, 2014 augustus
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003, 2014 februari  
* [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, 2013 oktober
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, 2013 augustus

## <a name="troubleshooting"></a>Problemen oplossen 

> [!NOTE]
> Bij het bijwerken van Microsoft Identity Manager-of AADConnect met gebruik van een van de ECMA2-connectors. 

U moet de connector definitie vernieuwen bij een upgrade naar Match of u ontvangt de volgende fout melding in het gebeurtenis logboek van de toepassing start to Report Warning log ID 6947: "assembly-versie in AAD connector Configuration (" X. X. XXX. X ") is vroeger dan de werkelijke versie (X. X. XXX. X") van "C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll".

De definitie vernieuwen:
* De eigenschappen voor de connector instantie openen
* Klik op het tabblad verbinding/verbinding maken
  * Voer het wacht woord voor het Connector account in
* Klik op elk van de eigenschappen tabbladen, op zijn beurt
  * Als dit type connector een tabblad partities heeft, met een knop Vernieuwen, klikt u op de knop vernieuwen terwijl u op dat tabblad
* Nadat alle tabbladen met eigenschappen zijn geopend, klikt u op de knop OK om de wijzigingen op te slaan.

## <a name="other-connectors"></a>Andere connectors

Naast de bovenstaande connectors, worden connectors voor share point en een verouderde connector voor Windows Azure Active Directory ook afzonderlijk van MIM gedistribueerd.

### <a name="sharepoint-user-profile"></a>Share point-gebruikers profiel

Met de Forefront Identity Manager connector voor share point-gebruikers profiel opslag kunt u identiteits gegevens synchroniseren met de gebruikers profiel opslag in share point 2013 en share point 2016.   Versie 4.3.2430.0 van deze connector, gepubliceerd 12/19/2016 kan worden gedownload van het [micro soft Download centrum](https://www.microsoft.com/en-us/download/details.aspx?id=41164).

Meer informatie over deze connector vindt u in het [hotfix-combinatie pakket](https://support.microsoft.com/en-us/help/3156030/hotfix-rollup-build-4-3-2201-0-is-available-for-forefront-identity-man) en instructies voor het [gebruik van een voor beeld van een MIM-oplossing in share Point server 2016](https://docs.microsoft.com/SharePoint/administration/use-a-sample-mim-solution-in-sharepoint-server-2016).

### <a name="forefront-identity-manager-connector-for-windows-azure-active-directory-legacy-connector"></a>Forefront Identity Manager-connector voor Windows Azure Active Directory (verouderde connector)

De Azure AD-connector voor FIM was een vroege technologie voor het synchroniseren van identiteits gegevens naar Azure Active Directory. De Azure AD-connector voor FIM, versie 1.0.6635.0069 vanaf 19 februari 2014, is in functie blok keren. [Er worden](https://www.microsoft.com/en-us/download/details.aspx?id=41166) geen updates ontvangen, maar deze worden nog steeds ondersteund. De oplossing voor het gebruik van FIM en de Azure AD-connector is vervangen door [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect). Micro soft raadt u aan om geen nieuwe implementatie met deze connector te starten.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [algemene documentatie over LDAP-connectors](microsoft-identity-manager-2016-connector-genericldap.md) vindt u meer informatie over de naslag documentatie over[algemene SQL-connectors](microsoft-identity-manager-2016-connector-genericsql.md) meer informatie over de naslag documentatie voor [Web Services-connectors](microsoft-identity-manager-2016-ma-ws.md) meer informatie over de referentie documentatie voor de [Power shell](microsoft-identity-manager-2016-connector-powershell.md) -connector voor meer informatie over de naslag documentatie over de [Lotus Domino-connector](microsoft-identity-manager-2016-connector-domino.md)
