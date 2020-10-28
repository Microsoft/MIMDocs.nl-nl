---
title: Lotus Domino-connector | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de Lotus Domino-connector van micro soft kunt configureren.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: e07fd469-d862-470f-a3c6-3ed2a8d745bf
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/05/2018
ms.author: billmath
ms.openlocfilehash: 46e80bce942b32bce7b7c3fb148973c249136b3f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758791"
---
# <a name="lotus-domino-connector-technical-reference"></a>Technische documentatie voor Lotus Domino-connector
In dit artikel wordt de Lotus Domino-connector beschreven. Het artikel is van toepassing op de volgende producten:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * U moet hotfix 4.1.3671.0 of hoger [KB3092178](https://support.microsoft.com/kb/3092178)gebruiken.

Voor MIM2016 en FIM2010R2 is de connector beschikbaar als down load vanuit het [micro soft Download centrum](https://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Overzicht van de Lotus Domino-connector
Met de Lotus Domino-connector kunt u de synchronisatie service integreren met de Lotus Domino-server van IBM.

Van een perspectief op hoog niveau worden de volgende functies ondersteund door de huidige versie van de connector:

| Functie | Ondersteuning |
| --- | --- |
| Verbonden gegevens bron |Server: <li>Lotus Domino 8.5. x</li><li>Lotus Domino 9. x</li>Client:<li>Lotus Domino 8.5. x</li><li>Lotus Notes 9. x</li> |
| Scenario's |<li>Beheer van object levenscyclus</li><li>Groepsbeheer</li><li>Wachtwoordbeheer</li> |
| Bewerkingen |<li>Volledige en Delta-import</li><li>Exporteren</li><li>Wacht woord voor HTTP-wacht woord instellen en wijzigen</li> |
| Schema |<li>Persoon (zwervende gebruiker, contact persoon (personen zonder certificaat))</li><li>Groep</li><li>Resource (resource, room, online vergadering)</li><li>Mail-in data base</li><li>Dynamische detectie van kenmerken voor ondersteunde objecten</li><li>Ondersteuning voor Maxi maal 250 aangepaste certificeerers met een organisatie & organisatie-eenheden (OE)</li> |

De Lotus Domino-connector gebruikt de Lotus Notes-client om te communiceren met de Lotus Domino-server. Als gevolg van deze afhankelijkheid moet een ondersteunde Lotus Notes-client op de synchronisatie server worden geïnstalleerd. De communicatie tussen de client en de server wordt geïmplementeerd via de Interop.domino.dll-interface (Lotus Notes .NET Interop). Deze interface vereenvoudigt de communicatie tussen het Microsoft.NET-platform en de Lotus Notes-client en ondersteunt toegang tot Lotus Domino-documenten en-weer gaven. Voor het importeren van verschillen is het ook mogelijk dat de native C++-interface wordt gebruikt (afhankelijk van de geselecteerde methode voor het importeren van verschillen).

### <a name="prerequisites"></a>Vereisten
Voordat u de connector gebruikt, moet u ervoor zorgen dat u de volgende vereisten op de synchronisatie server hebt:

* Microsoft .NET 4.5.2-Framework of hoger
* De Lotus Notes-client moet worden geïnstalleerd op de synchronisatie server
* De Lotus Domino-connector vereist dat de standaard-Lotus Domino LDAP-schema database (schema. NSF) aanwezig is op de Domino-adreslijst server. Als deze niet aanwezig is, kunt u deze installeren door de LDAP-service op de Domino-server uit te voeren of opnieuw te starten.

### <a name="connected-data-source-permissions"></a>Machtigingen voor verbonden gegevens bron
Als u een van de ondersteunde taken in de Lotus Domino-connector wilt uitvoeren, moet u lid zijn van de volgende groepen:

* Volledige toegangs beheerders
* Beheerders
* Database beheerders

De volgende tabel bevat de machtigingen die voor elke bewerking zijn vereist:

| Bewerking | Toegangsrechten |
| --- | --- |
| Importeren |<li>Open bare documenten lezen</li><li> Volledige toegangs beheerder (wanneer u lid bent van de groep volledige toegang tot Administrators, hebt u automatisch de juiste toegang tot in ACL.)</li> |
| Exporteren en wacht woord instellen |Efficiënte toegang: <li>Documenten maken</li><li>Documenten verwijderen</li><li>Open bare documenten lezen</li><li>Open bare documenten schrijven</li><li>Documenten repliceren of kopiëren</li>Voor export bewerkingen hebt u ook de volgende rollen nodig: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li> |

### <a name="direct-operations-and-adminp"></a>Directe bewerkingen en AdminP
Bewerkingen gaan rechtstreeks naar de Domino-map of het AdminP-proces. In de volgende tabellen worden alle ondersteunde objecten, bewerkingen en, indien van toepassing, de gerelateerde implementatie methode vermeld:

**Primair adres boek**

| Object | Maken | Bijwerken | Verwijderen |
| --- | --- | --- | --- |
| Person |AdminP |Direct |AdminP |
| Groep |AdminP |Direct |AdminP |
| MailInDB |Direct |Direct |Direct |
| Resource |AdminP |Direct |AdminP |

**Secundair adres boek**

| Object | Maken | Bijwerken | Verwijderen |
| --- | --- | --- | --- |
| Person |N.v.t. |Direct |Direct |
| Groep |Direct |Direct |Direct |
| MailInDB |Direct |Direct |Direct |
| Resource |N.v.t. |N.v.t. |N.v.t. |

Wanneer een resource wordt gemaakt, wordt er een Notes-document gemaakt. Op dezelfde manier wordt het notitie document verwijderd wanneer een resource wordt verwijderd.

### <a name="ports-and-protocols"></a>Poorten en protocollen
IBM Lotus Notes-client-en Domino-servers communiceren met behulp van Notes Remote Procedure Call (NRPC) waarbij NRPC TCP/IP moet gebruiken. Het standaard poort nummer is 1352, maar kan worden gewijzigd door de Domino-beheerder.

### <a name="not-supported"></a>Niet ondersteund
De volgende bewerkingen worden niet ondersteund door de huidige versie van de Lotus Domino-connector:

* Verplaats postvak tussen servers.

## <a name="create-a-new-connector"></a>Een nieuwe connector maken
### <a name="client-software-installation-and-configuration"></a>Installatie en configuratie van client software
Lotus Notes moet zijn geïnstalleerd op de server **voordat** de connector wordt geïnstalleerd.

Wanneer u installeert, moet u ervoor zorgen dat u **één gebruiker installeert** . De standaard **installatie voor meerdere gebruikers** werkt niet.  
![Notes1](./media/microsoft-identity-manager-2016-connector-domino/notes1.png)

Installeer op de pagina functies alleen de vereiste Lotus Notes-functies en **eenmalige aanmelding** van de client. Er is één aanmelding vereist voordat de connector zich kan aanmelden bij de Domino-server.  
![Notes2](./media/microsoft-identity-manager-2016-connector-domino/notes2.png)

**Opmerking:** Start Lotus Notes eenmaal met een gebruiker die zich op dezelfde server bevindt als het account dat u gebruikt als het service account van de connector. Zorg er ook voor dat u de Lotus Notes-client op de-server sluit. Deze kan niet tegelijkertijd worden uitgevoerd. de connector probeert verbinding te maken met de Domino-server.

### <a name="create-connector"></a>Connector maken
Als u een Lotus Domino-connector wilt maken, selecteert u in de **synchronisatie service** **beheer agent** selecteren en **maakt** u deze. Selecteer de **Lotus Domino-connector (micro soft)** .  
![CreateConnector](./media/microsoft-identity-manager-2016-connector-domino/createconnector.png)

Als uw versie van de synchronisatie service de mogelijkheid biedt om de **architectuur** te configureren, moet u ervoor zorgen dat de connector is ingesteld op de standaard waarde om in het **proces** uit te voeren.

### <a name="connectivity"></a>Connectiviteit
Op de pagina connectiviteit moet u de naam van de Lotus Domino-server opgeven en de aanmeldings referenties opgeven.  
![Connectiviteit](./media/microsoft-identity-manager-2016-connector-domino/connectivity.png)

De eigenschap Domino server ondersteunt twee indelingen voor de server naam:

* ServerName
* Servername/mapnaam

De indeling **servername/mapnaam** is de voorkeurs indeling voor dit kenmerk, omdat deze sneller reageert wanneer de connector contact opneemt met de Domino-server.

Het gegeven bestand UserID wordt opgeslagen in de configuratie database van de synchronisatie service.

U hebt de volgende opties voor het **importeren van verschillen** :

* **Geen** . De connector voert geen Delta-import bewerkingen uit.
* **Toevoegen/bijwerken** . De connector voert Delta-toevoeg-en update bewerkingen uit. Voor verwijderen is een **volledige import** bewerking vereist. Deze bewerking maakt gebruik van .net Interop.
* **Toevoegen/bijwerken/verwijderen** . De connector voert Delta-toevoeg-, bijwerk-en verwijder bewerkingen uit. Deze bewerking maakt gebruik van de systeem eigen C++-interfaces.

In **schema opties** hebt u de volgende opties:

* **Standaard schema** . De connector detecteert het schema van de Domino-server. Deze selectie is de standaard optie.
* **DSML-schema** . Wordt alleen gebruikt als de Domino-server het schema niet weergeeft. Vervolgens kunt u een DSML-bestand met het schema maken en dit in plaats daarvan importeren. Zie [Oasis](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml)voor meer informatie over dsml.

Wanneer u op Volgende klikt, worden de para meters UserID en wacht woord configuratie gecontroleerd.

### <a name="global-parameters"></a>Globale parameters
Op de pagina globale para meters configureert u de optie tijd zone en import-en export bewerking.  
![Globale parameters](./media/microsoft-identity-manager-2016-connector-domino/globalparameters.png)

Met de para meter **tijd zone** van de Domino-server wordt de locatie van uw Domino-server gedefinieerd.

Deze configuratie optie is vereist voor de ondersteuning van **Delta-import** bewerkingen, omdat hiermee de synchronisatie service de wijzigingen van de laatste twee invoer kan bepalen.

> [!Note]
> Vanaf de update van maart 2017 wordt het scherm algemene para meters weer gegeven met de optie om de e-mail database van de gebruiker te verwijderen tijdens het verwijderen van de gebruiker.

![Postvak van de gebruiker verwijderen](./media/microsoft-identity-manager-2016-connector-domino/AdminP.png)

#### <a name="import-settings-method"></a>Instellingen importeren, methode
U hebt de volgende opties om de **volledige import bewerking uit te voeren** :

* Search
* Weer gave (aanbevolen)

De **Zoek functie** maakt gebruik van indexeren in Domino, maar het is gebruikelijk dat de indexen niet in realtime worden bijgewerkt en dat de gegevens die door de server worden geretourneerd, niet altijd juist zijn. Voor een systeem met veel wijzigingen werkt deze optie doorgaans niet goed en levert deze onwaare verwijderingen in bepaalde situaties. **Zoeken** gaat echter sneller dan **weer gave** .

**Weer gave** is de aanbevolen optie omdat deze de juiste status van de gegevens bevat. Het is iets langzamer dan zoeken.

#### <a name="creation-of-virtual-contact-objects"></a>Maken van virtuele contact objecten
Het **maken van een \_ contact object inschakelen** heeft de volgende opties:

* Geen
* Niet-verwijzings waarden
* Referentie-en niet-verwijzings waarden

In Domino kunnen referentie kenmerken veel verschillende indelingen bevatten om naar andere objecten te verwijzen. Om verschillende variaties te kunnen aanduiden, implementeert de connector \_ contact-objecten, ook wel bekend als **virtuele contact personen** (VC). Deze objecten worden gemaakt, zodat ze kunnen worden toegevoegd aan bestaande MV-objecten of als nieuwe objecten worden geprojecteerd. Op deze manier kunnen kenmerk verwijzingen behouden blijven.

Als u deze instelling inschakelt en de inhoud van een verwijzings kenmerk geen DN-indeling is, \_ wordt een contact object gemaakt. Een kenmerk member van een groep kan bijvoorbeeld SMTP-adressen bevatten. Het is ook mogelijk om korte en andere kenmerken te hebben in verwijzings kenmerken. Selecteer voor dit scenario **niet-verwijzings waarden** . Deze configuratie is de meest voorkomende instelling voor Domino-implementaties.

Als Lotus Domino is geconfigureerd voor het hebben van afzonderlijke adres boeken met verschillende DN-namen die hetzelfde object vertegenwoordigen, is het mogelijk om ook \_ contact objecten te maken voor alle referentie waarden die in een adres boek worden gevonden. Voor dit scenario selecteert u de optie **referentie en niet-verwijzings waarden** .

Als u meerdere waarden in het kenmerk **FullName** in Domino hebt, moet u ook het maken van virtuele contact personen inschakelen zodat verwijzingen kunnen worden omgezet. Dit kenmerk kan bijvoorbeeld meerdere waarden hebben na een huwelijk of echt scheiding. Schakel het selectie vakje **inschakelen... FullName heeft meerdere waarden** voor dit scenario.

Door deel te nemen aan de juiste kenmerken, \_ worden de contact objecten gekoppeld aan het mv-object.

Aan deze objecten is VC = \_ contact persoon toegevoegd.

#### <a name="import-settings-conflict-object"></a>Instellingen voor importeren, conflict object
**Conflict object uitsluiten**

In een grote Domino-implementatie is het mogelijk dat meerdere objecten dezelfde DN hebben als gevolg van replicatie problemen. In dergelijke gevallen ziet de connector twee objecten met verschillende UniversalIDs, maar dezelfde DN. Dit conflict zou ertoe leiden dat er een tijdelijk object wordt gemaakt in de connector ruimte. De connector kan de objecten die zijn geselecteerd in Domino negeren als slacht offer van de replicatie. Als u dit selectie vakje inschakelt, wordt de aanbeveling ingeschakeld.

#### <a name="export-settings"></a>Exportinstellingen
Als de optie **AdminP gebruiken voor het bijwerken van verwijzingen** is uitgeschakeld, is de selectie van verwijzings kenmerken, zoals lid, een directe oproep en wordt het AdminP-proces niet gebruikt. Gebruik deze optie alleen wanneer AdminP niet is geconfigureerd voor het onderhouden van referentiële integriteit.

#### <a name="routing-information"></a>Routerings informatie
In Domino is het mogelijk dat een verwijzings kenmerk routerings gegevens bevat die zijn Inge sloten als achtervoegsel voor de DN-naam. Het kenmerk member in een groep kan bijvoorbeeld <strong>CN = example/organization@ABC </strong>bevatten. Het achtervoegsel @ABC bevindt zich in de routerings gegevens. De routerings gegevens worden door Domino gebruikt voor het verzenden van e-mail berichten naar het juiste Domino-systeem. Dit kan een systeem zijn in een andere organisatie. In het veld routerings informatie kunt u de routerings achtervoegsels opgeven die in de organisatie worden gebruikt binnen het bereik van de connector. Als een van deze waarden is gevonden als achtervoegsel in een verwijzings kenmerk, worden de routerings gegevens verwijderd uit de verwijzing. Als het achtervoegsel van de route ring voor een verwijzings waarde niet overeenkomt met een van deze waarden die zijn opgegeven, wordt er een \_ contact object gemaakt. Deze \_ contact objecten worden gemaakt met **ro = @ <RoutingSuffix>** ingevoegd in de DN. Voor deze \_ contact objecten worden ook de volgende kenmerken toegevoegd zodat u, indien nodig, kunt deel nemen aan een echt object: \_ route naam, \_ naam contact persoon, \_ DisplayName en UniversalID.

#### <a name="additional-address-books"></a>Aanvullende adres boeken
Als er geen directory- **assistent** is geïnstalleerd, die de naam van secundaire adres boeken bevat, kunt u deze adres boeken hand matig invoeren.

#### <a name="multivalued-transformation"></a>Trans formatie met meerdere waarden
Veel kenmerken in Lotus Domino zijn meerdere waarden. De bijbehorende mailverse kenmerken zijn meestal enkele waarden. Door de import-en export bewerking te configureren, schakelt u de connector in om u te helpen bij de vereiste vertaling van de betrokken kenmerken.

**Exporteren**  
De optie voor het exporteren van bewerkingen ondersteunt twee modi:

* Item toevoegen
* Item vervangen

**Item vervangen** : wanneer u deze optie selecteert, worden de huidige waarden van het kenmerk in Domino altijd door de connector verwijderd en vervangen door de gegeven waarden. De gegeven waarde kan één waarde of meerdere waarden zijn.

Voor beeld: het kenmerk assistent van een persoons object heeft de volgende waarden:

* CN = Greg Winston/OE = contoso/O = Amerikaans continent, NAB = names. nsf
* CN = John Smith/OE = contoso/O = Amerikaans continent, NAB = names. nsf

Als een nieuwe assistent met de naam **David Alexander** is toegewezen aan dit persoons object, is het resultaat:

* CN = David Alexander/OE = contoso/O = Amerikaans continent, NAB = names. nsf

**Item toevoegen** : wanneer u deze optie selecteert, behoudt de connector de bestaande waarden van het kenmerk in Domino en worden nieuwe waarden ingevoegd boven aan de lijst met gegevens.

Voor beeld: het kenmerk assistent van een persoons object heeft de volgende waarden:

* CN = Greg Winston/OE = contoso/O = Amerikaans continent, NAB = names. nsf
* CN = John Smith/OE = contoso/O = Amerikaans continent, NAB = names. nsf

Als een nieuwe assistent met de naam **David Alexander** is toegewezen aan dit persoons object, is het resultaat:

* CN = David Alexander/OE = contoso/O = Amerikaans continent, NAB = names. nsf
* CN = Greg Winston/OE = contoso/O = Amerikaans continent, NAB = names. nsf
* CN = John Smith/OE = contoso/O = Amerikaans continent, NAB = names. nsf

**Importeren**  
De optie import bewerking ondersteunt twee modi:

* Standaard
* Meerdere waarden per waarde

**Standaard** : wanneer u de standaard optie selecteert, worden alle waarden van alle kenmerken geïmporteerd.

Meerdere waarden **voor één waarde** : wanneer u deze optie selecteert, wordt er een kenmerk met meervoudige waarden geconverteerd naar een kenmerk met één waarde. Als er meer dan één waarde bestaat, wordt de waarde boven (deze waarde is doorgaans ook de meest recent) gebruikt.

Voor beeld: het kenmerk assistent van een persoons object heeft de volgende waarden:

* CN = David Alexander/OE = contoso/O = Amerikaans continent, NAB = names. nsf
* CN = Greg Winston/OE = contoso/O = Amerikaans continent, NAB = names. nsf
* CN = John Smith/OE = contoso/O = Amerikaans continent, NAB = names. nsf

De meest recente update van dit kenmerk is **David Alexander** . Omdat de optie import bewerking is ingesteld op meerdere waarden voor een enkele waarde, importeert connector alleen **David Alexander** in de connector ruimte.

De logica voor het converteren van kenmerken met meerdere waarden in kenmerken met één waarde is niet van toepassing op het kenmerk van de groeps leden en het kenmerk persoons-FullName.

Het is ook mogelijk om transformatie regels voor importeren en exporteren te configureren voor kenmerken met meerdere waarden per kenmerk, als een uitzonde ring op de globale regel. Als u deze optie wilt configureren, voert u [object type] in. [kenmerknaam] in de **lijst met uitsluitings kenmerken importeren** en tekst vakken **met uitsluitings kenmerken exporteren** . Als u bijvoorbeeld persoon. Assistant invoert en de globale vlag is ingesteld op alle waarden importeren, wordt alleen de eerste waarde voor de assistent geïmporteerd.

#### <a name="certifiers"></a>Ondercertificeerers
Alle organisaties/organisatie-eenheden worden weer gegeven door de connector. Om objecten van de persoon naar het primaire adres boek te kunnen exporteren, is een certificeert dat het wacht woord is vereist.

Als alle certificeerers hetzelfde wacht woord hebben, kan het **wacht woord voor alle Certifers** worden gebruikt. U kunt het wacht woord hier opgeven en alleen het certificeert-bestand.

Als u alleen importeert, hoeft u geen certificeerers op te geven.

### <a name="configure-provisioning-hierarchy"></a>Inrichtings hiërarchie configureren
Wanneer u de Lotus Domino-connector configureert, slaat u deze dialoog pagina over. De Lotus Domino-connector biedt geen ondersteuning voor het inrichten van hiërarchieën.  
![Inrichtings hiërarchie](./media/microsoft-identity-manager-2016-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Partities en hiërarchieën configureren
Wanneer u partities en hiërarchieën configureert, moet u het primaire adres boek met de naam NAB = names. nsf selecteren. Naast het primaire adres boek kunt u ook secundaire adres boeken selecteren, indien aanwezig.  
![Partities](./media/microsoft-identity-manager-2016-connector-domino/partitions.png)

### <a name="select-attributes"></a>Kenmerken selecteren
Wanneer u uw kenmerken configureert, moet u alle kenmerken selecteren die worden voorafgegaan door **\_ MMS \_** . Deze kenmerken zijn vereist wanneer u nieuwe objecten inricht op Lotus Domino

![Kenmerken](./media/microsoft-identity-manager-2016-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Beheer van object levenscyclus
Deze sectie bevat een overzicht van de verschillende objecten in Domino.

### <a name="person-objects"></a>Persoons objecten
Het persoons object vertegenwoordigt gebruikers in organisatie-en organisatie-eenheden. Naast de standaard kenmerken kan de Domino-beheerder aangepaste kenmerken toevoegen aan een persoons object. Een persoons object moet mini maal alle verplichte kenmerken bevatten. Zie [Lotus Notes-eigenschappen](#lotus-notes-properties)voor een volledige lijst met verplichte kenmerken. Als u een persoons object wilt registreren, moet aan de volgende vereisten worden voldaan:

* Het adres boek (names. NSF) moet zijn gedefinieerd en het moet het primaire adres boek zijn.
* U moet de onderhanden id van de O/OE-eenheid en het wacht woord hebben om een bepaalde gebruiker in de organisatie-eenheid te registreren.
* U moet een specifieke set Lotus Notes-eigenschappen instellen voor een persoons object. Deze eigenschappen worden gebruikt voor het inrichten van het object persoon. Zie de sectie ' [Lotus Notes-eigenschappen](#lotus-notes-properties) ' verderop in dit document voor meer informatie.
* Het eerste HTTP-wacht woord voor een persoon is een kenmerk dat is ingesteld tijdens het inrichten.
* Het object person moet een van de volgende drie ondersteunde typen zijn:
  1. Normale gebruiker met een e-mail bestand en een gebruikers-id-bestand
  2. Zwervende gebruiker (een normale gebruiker die alle zwervende database bestanden bevat)
  3. Contacts (gebruiker zonder id-bestand)

Personen (met uitzonde ring van contact personen) kunnen verder worden gegroepeerd in gebruikers van de Verenigde Staten en internationale gebruikers, zoals gedefinieerd door de waarde van de \_ MMS- \_ eigenschap IDRegType. Deze personen gebruiken de Notes-client voor toegang tot Lotus Domino-servers, hebben een notitie-id en een persoons document. Als ze Notes mail gebruiken, hebben ze ook een e-mail bestand. De gebruiker moet zijn geregistreerd om actief te worden. Zie voor meer informatie:

* [Notities instellen voor gebruikers](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
* [Gebruikers registratie](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
* [Gebruikers beheren](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
* [Gebruikers naam wijzigen](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_RENAMING_A_LOTUS_INOTES_USER_STEPS.html)

Al deze bewerkingen worden uitgevoerd in Lotus Domino en vervolgens geïmporteerd in de synchronisatie service.

### <a name="resources-and-rooms"></a>Resources en ruimtes
Een resource is een ander type van een data base in Lotus Domino. Resources kunnen Vergader zalen zijn met verschillende typen apparaten, zoals projectors. Er zijn subtypen van bronnen die worden ondersteund door de Lotus Domino-connector die worden gedefinieerd door het kenmerk resource type:

| Type resource | Kenmerk van bron type |
| --- | --- |
| Room |1 |
| Resource (overige) |2 |
| Online vergadering |3 |

Het type resource object werkt alleen als het volgende is vereist:

* De resource reserverings database moet al bestaan op de verbonden Domino-server
* De site is al gedefinieerd voor de resource

De Data Base voor resource reservering bevat drie typen documenten:

* Site profiel
* Resource
* Reservering

Zie [de Data Base voor resource reserveringen instellen](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html)voor meer informatie over het instellen van de Data Base voor resource reservering.

**Resources maken, bijwerken en verwijderen**  
De bewerkingen Create, update en DELETE worden uitgevoerd door de Lotus Domino-connector in de Data Base voor resource reservering. Resources worden gemaakt als documenten in names. NSF (het primaire adres boek). Zie [resource documenten bewerken en verwijderen](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html)voor meer informatie over het bewerken en verwijderen van resources.

**Import-en export bewerking voor resources**  
De resources kunnen worden geïmporteerd in en geëxporteerd uit de synchronisatie service, zoals elk ander object type. Selecteer het object type als resource tijdens de configuratie. Voor een geslaagde export bewerking moet u details over het resource type, de Vergader database en de site naam hebben.

### <a name="mail-in-databases"></a>Mail-In-data bases
Een Mail-In data base is een Data Base die is ontworpen voor het ontvangen van e-mail berichten. Het is een Lotus Domino-postvak dat niet is gekoppeld aan een specifiek Lotus Domino-gebruikers account (dat wil zeggen dat het geen eigen ID-bestand en wacht woord heeft). Een mail-in-data base heeft een unieke gebruikers-id ("short name") die aan het bericht is gekoppeld en een eigen e-mail adres.

Als er een afzonderlijk postvak nodig is met een eigen e-mail adres dat kan worden gedeeld tussen verschillende gebruikers (bijvoorbeeld group@contoso.com ), wordt er een mail-in data base gemaakt. De toegang tot dit postvak wordt geregeld via de Access Control lijst (ACL), die de namen bevat van de Notes-gebruikers die het postvak mogen openen.

Zie de sectie ' [verplichte kenmerken](#mandatory-attributes) ' verderop in dit artikel voor een lijst met de vereiste kenmerken.

Wanneer een Data Base is ontworpen om een e-mail te ontvangen, wordt een Mail-In database document gemaakt in Lotus Domino. Dit document moet aanwezig zijn in de Domino-Directory van elke server waarop een kopie van de data base is opgeslagen. Zie [een Mail-In database document maken](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html)voor een gedetailleerde beschrijving van het maken van een data base-document.

Voordat u een Mail-In-data base maakt, moet de data base al bestaan (had moeten zijn gemaakt door de Lotus-beheerder) op de Domino-server.

### <a name="group-management"></a>Groepsbeheer
U kunt een gedetailleerd overzicht van het beheer van de Lotus Domino-groep krijgen van de volgende resources:

* [Groepen gebruiken](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
* [Een groep maken](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
* [Groepen maken en wijzigen](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
* [Groepen beheren](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
* [De naam van een groep wijzigen](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Wachtwoordbeheer
Er zijn twee soorten wacht woorden voor een geregistreerde Lotus Domino-gebruiker:

1. Gebruikers wachtwoord (opgeslagen in User.id-bestand)
2. Internet/HTTP-wacht woord

De Lotus Domino-connector ondersteunt alleen bewerkingen met het HTTP-wacht woord.

Als u wachtwoord beheer wilt uitvoeren, moet u wachtwoord beheer voor de connector inschakelen in de ontwerp functie van de beheer agent. Als u wachtwoord beheer wilt inschakelen, selecteert u **wachtwoord beheer inschakelen** op de pagina **uitbrei dingen configureren** .  
![Extensies configureren](./media/microsoft-identity-manager-2016-connector-domino/configureextensions.png)

De Lotus Domino-connector ondersteunt de volgende bewerkingen op Internet wacht woord:

* Wacht woord instellen: wacht woord instellen stelt een nieuw HTTP/Internet-wacht woord in voor de gebruiker in Domino. Het account wordt standaard ook ontgrendeld. De ontgrendelings vlag wordt weer gegeven op de WMI-interface van de synchronisatie-engine.
* Wacht woord wijzigen: in dit scenario is het mogelijk dat een gebruiker het wacht woord wil wijzigen of dat het wacht woord moet worden gewijzigd na een opgegeven tijdstip. Deze bewerking kan alleen worden uitgevoerd als het oude en het nieuwe wacht woord verplicht zijn. Zodra het nieuwe wacht woord is gewijzigd, wordt het in Lotus Domino bijgewerkt.

Zie voor meer informatie:

* [De functie Internet vergren delen gebruiken](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
* [Internet wachtwoorden beheren](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Naslag informatie
Deze sectie bevat een lijst met kenmerk beschrijvingen en kenmerk vereisten voor de Lotus Domino-connector.

### <a name="lotus-notes-properties"></a>Lotus Notes-eigenschappen
Wanneer u persoons objecten inricht in uw Lotus Domino-map, moeten uw objecten beschikken over een specifieke set eigenschappen met specifieke waarden. Deze waarden zijn alleen vereist voor Create-bewerkingen.

De volgende tabel geeft een lijst van deze eigenschappen en geeft een beschrijving van deze kenmerken.

| Eigenschap | Beschrijving |
| --- | --- |
| \_MMS_AltFullName |De alternatieve volledige naam van de gebruiker. |
| \_MMS_AltFullNameLanguage |De taal die moet worden gebruikt voor het opgeven van de alternatieve volledige naam van de gebruiker. |
| \_MMS_CertDaysToExpire |Het aantal dagen vanaf de huidige datum voordat het certificaat verloopt. Als deze niet wordt opgegeven, is de standaard datum twee jaar na de huidige datum. |
| \_MMS_Certifier |Eigenschap die de naam van de organisatie hiërarchie van de certificeert bevat. Bijvoorbeeld: OE = OrganizationUnit, O = org, C = land. |
| \_MMS_IDPath |Als de eigenschap leeg is, wordt er geen lokaal gebruikers-id-bestand gemaakt op de synchronisatie server. Als de eigenschap een bestands naam bevat, wordt er een gebruikers-ID-bestand gemaakt in de map madata. De eigenschap kan ook een volledig pad bevatten. |
| \_MMS_IDRegType |Personen kunnen worden geclassificeerd als contact personen, gebruikers van de Verenigde Staten en internationale gebruikers. De volgende tabel bevat de mogelijke waarden: <li>0: contact opnemen</li><li>1-Amerikaanse gebruiker</li><li>2-internationale gebruiker</li> |
| \_MMS_IDStoreType |De vereiste eigenschap voor Amerikaanse en internationale gebruikers. De eigenschap bevat een geheel getal dat aangeeft of de gebruikers-id is opgeslagen als bijlage in het adres boek notities of in het e-mail bestand van de persoon. Als het bestand met de gebruikers-ID een bijlage in het adres boek is, kan het optioneel worden gemaakt als een bestand met \_ MMS_IDPath. <li>Leeg archief-ID-bestand in ID-kluis, geen id-bestand (gebruikt voor contact personen).</li><li> 1-bijlage in het adres boek voor notities. De \_ eigenschap MMS_Password moet worden ingesteld voor gebruikers-id-bestanden die bijlagen zijn</li><li>2-archief-ID in het e-mail bestand van de persoon. De \_ MMS_UseAdminP moet worden ingesteld op false om het e-mail bestand te laten maken tijdens de registratie van de persoon. De \_ eigenschap MMS_Password moet worden ingesteld voor gebruikers-id-bestanden.</li> |
| \_MMS_MailQuotaSizeLimit |Het aantal mega bytes dat is toegestaan voor de data base van het e-mail bestand. |
| \_MMS_MailQuotaWarningThreshold |Het aantal mega bytes dat is toegestaan voor de data base van het e-mail bestand voordat een waarschuwing wordt gegeven. |
| \_MMS_MailTemplateName |Het e-mail sjabloon bestand dat wordt gebruikt voor het maken van het e-mail bestand van de gebruiker. Als er een sjabloon is opgegeven, wordt het e-mail bestand gemaakt met behulp van de opgegeven sjabloon. Als er geen sjabloon is opgegeven, wordt het standaard sjabloon bestand gebruikt om het bestand te maken. |
| \_MMS_OU |Optionele eigenschap die de OE-naam onder de certificeert. Deze eigenschap moet leeg zijn voor contact personen. |
| \_MMS_Password |Vereiste eigenschap voor gebruikers. De eigenschap bevat het wacht woord voor het identificatie bestand van het object. |
| \_MMS_UseAdminP |De eigenschap moet worden ingesteld op True als het e-mail bestand moet worden gemaakt door het AdminP-proces op de Domino-server (asynchroon naar het export proces). Als eigenschap is ingesteld op False, wordt het e-mail bestand gemaakt met de Domino-gebruiker (synchroon in het export proces). |

Voor een gebruiker met een bijbehorend id-bestand \_ moet de eigenschap MMS_Password een waarde bevatten. Voor toegang tot e-mail via de Lotus Notes-client moeten de eigenschappen mailserver en mailfile van een gebruiker een waarde bevatten.

De volgende eigenschappen moeten waarden bevatten om toegang te krijgen tot e-mail via een webbrowser:

* Mailfile-required-eigenschap die het pad op de Lotus Domino-server bevat waarop het e-mail bestand is opgeslagen.
* De eigenschap mailserver vereist die de naam van de Lotus Domino-server bevat. Deze waarde is de naam die moet worden gebruikt wanneer u het Lotus-e-mail bestand op de Domino-server hebt gemaakt.
* HTTPPassword: een optionele eigenschap die het wacht woord voor webtoegang voor het object bevat.

De eigenschap HTTPPassword moet een waarde bevatten om toegang te krijgen tot de Domino-server zonder mail mogelijkheid. De eigenschap mailfile en de eigenschap mailserver kunnen leeg zijn.

Met \_ MMS_ IDStoreType = 2 (archief-id in e-mail bestand) is de eigenschap mail systeem van NotesRegistrationclass ingesteld op REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Verplichte kenmerken
De Lotus Domino-connector ondersteunt hoofd zakelijk deze typen objecten (document typen):

* Groep
* Mail-In-data base
* Person
* Contact persoon (persoon zonder certificeerer)
* Resource

In deze sectie vindt u de kenmerken die verplicht zijn voor elk ondersteund object om te exporteren naar een Domino-server.

| Objecttype | Verplichte kenmerken |
| --- | --- |
| Groep |<li>ListName</li> |
| Main-In-data base |<li>FullName</li><li>Mailfile</li><li>Mailserver</li><li>Maildomein</li> |
| Person |<li>LastName</li><li>Mailfile</li><li>Korte naam</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li> |
| Contact persoon (persoon zonder certificeerer) |<li>\_MMS_IDRegType</li> |
| Resource |<li>FullName</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Site</li><li>DisplayName</li><li>Mailfile</li><li>Mailserver</li><li>Maildomein</li> |

## <a name="common-issues-and-questions"></a>Veelvoorkomende problemen en vragen
### <a name="schema-detection-does-not-work"></a>Schema detectie werkt niet
Om het schema te kunnen detecteren, moet het bestand schema. nsf aanwezig zijn op de Domino-server. Dit bestand wordt alleen weer gegeven als LDAP is geïnstalleerd op de server. Als het schema niet kan worden gedetecteerd, controleert u het volgende:

* Het File schema. NSF is aanwezig in de hoofdmap van de Domino-server
* De gebruiker heeft machtigingen om het bestand schema. NSF te bekijken.
* Het opnieuw opstarten van de LDAP-server afdwingen. Open de **Lotus Domino-console** en gebruik de opdracht **LDAP-ReloadSchema** om het schema opnieuw te laden.

### <a name="not-all-secondary-address-books-are-visible"></a>Niet alle secundaire adres boeken zijn zichtbaar
De Domino-connector is afhankelijk van de functie **Directory hulp** om de secundaire adres boeken te vinden. Als de secundaire adres boeken ontbreken, controleert u of [Directory-ondersteuning](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ABOUT_DIRECTORY_ASSISTANCE.html) is ingeschakeld en geconfigureerd op de Domino-server.

### <a name="custom-attributes-in-domino"></a>Aangepaste kenmerken in Domino
Er zijn verschillende manieren in Domino om het schema uit te breiden zodat het wordt weer gegeven als een aangepast kenmerk dat kan worden gebruikt door de connector.

**Benadering 1: Lotus Domino-schema uitbreiden**

1. Maak een kopie van de Domino-Directory sjabloon {PUBNAMES. NTF} door de volgende [stappen uit te voeren](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (u moet de standaard sjabloon voor de IBM Lotus Domino-map niet aanpassen):
2. Open het exemplaar van de Domino-adreslijst sjabloon {CONTOSO. NTF}-sjabloon die is gemaakt in Domino Designer en volg de volgende stappen:
   * Klik op gedeelde elementen en vouw subformulieren uit
   * Dubbel klik op $ {ObjectName} InheritableSchema subform (waarbij {ObjectName} de naam is van de standaard klasse Structured object, bijvoorbeeld: persoon).
   * Noem het kenmerk dat u wilt toevoegen aan schema {MyPersonAtrribute} en dat overeenkomt met dat kenmerk. Maak een veld door het menu **maken** te selecteren en vervolgens **veld** te selecteren in het menu.
   * Stel in het veld toegevoegd de eigenschappen in door het type, de stijl, de grootte, het letter type en andere gerelateerde para meters te selecteren in het veld venster Eigenschappen.
   * Behoud de standaard waarde voor het kenmerk hetzelfde als de naam die voor dat kenmerk is opgegeven (bijvoorbeeld als de kenmerk naam MyPersonAttribute is, behoud de standaard waarde met dezelfde naam).
   * Sla het subformulier $ {ObjectName} InheritableSchema op met bijgewerkte waarden.
3. Vervang de sjabloon voor de Domino-map {PUBNAMES. NTF} met de nieuwe aangepaste sjabloon {CONTOSO. NTF} door [de volgende stappen uit te voeren](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Sluit de Domino-beheerder en open de Domino-console om de LDAP-service opnieuw te starten en het LDAP-schema opnieuw te laden:
   * Plaats in de Domino-console de opdracht onder **Domino-opdracht** tekst die is ingediend om de LDAP-service opnieuw te starten- [taak LDAP opnieuw starten](https://www.ibm.com/support/knowledgecenter/SSKTMJ_10.0.1/admin/conf_startingandstoppingtheldapservice_c.html).
   * Ldap-schema gebruiken LDAP-opdracht informatie over LDAP-ReloadSchema
5. Open Domino-beheerder en selecteer personen & tabblad groepen om het toegevoegde kenmerk weer te geven in Domino toevoegen persoon.
6. Open schema. nsf op het tabblad **bestanden** en zie toegevoegd kenmerk wordt weer gegeven in de klasse dominoPerson LDAP-object.

**Benadering 2: een auxClass maken met een aangepast kenmerk en koppelen aan de object klasse**

1. Maak een kopie van de Domino-Directory sjabloon {PUBNAMES. NTF} door de volgende [stappen uit te voeren](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nooit de standaard sjabloon voor de IBM Lotus Domino-map aanpassen):
2. Open het exemplaar van de Domino-adreslijst sjabloon {CONTOSO. NTF} sjabloon die is gemaakt in Domino Designer.
3. Selecteer in het linkerdeel venster gedeelde code en vervolgens subformulieren.
4. Klik op nieuw subformulier
5. Ga als volgt te werk om de eigenschappen voor het nieuwe subformulier op te geven:
   * Klik, terwijl het nieuwe subformulier is geopend, op ontwerp subformulier eigenschappen
   * Voer naast de eigenschap naam een naam in voor de klasse hulp object, bijvoorbeeld TestSubform.
   * Behoud de eigenschap opties in subformulier invoegen... dialoog venster geselecteerd
   * Schakel de eigenschap Options ' rendering Pass through HTML ' in Notes uit.
   * Laat de andere eigenschappen hetzelfde en sluit het vak Eigenschappen van subformulier.
   * Sla het nieuwe subformulier op en sluit het.
6. Ga als volgt te werk om een veld toe te voegen om de hulp object klasse te definiëren:
   * Open het subformulier dat u hebt gemaakt.
   * Kies maken-veld.
   * Geef naast naam op het tabblad basis beginselen van het dialoog venster veld een wille keurige naam op, bijvoorbeeld: {MyPersonTestAttribute}.
   * Stel in het veld toegevoegd de eigenschappen in door het type, de stijl, de grootte, het letter type en de bijbehorende eigenschappen te selecteren.
   * Behoud de standaard waarde voor het kenmerk hetzelfde als de naam die voor dat kenmerk is opgegeven (bijvoorbeeld als de kenmerk naam MyPersonTestAttribute is, behoud de standaard waarde met dezelfde naam).
   * Sla het subformulier op met bijgewerkte waarden en voer de volgende handelingen uit:
     * Selecteer in het linkerdeel venster gedeelde code en vervolgens subformulieren
     * Selecteer het nieuwe subformulier en kies Eigenschappen ontwerpen.
     * Klik op het derde tabblad aan de linkerkant en selecteer **Dit verbod op ontwerp wijziging door geven** .
7. Open $ {ObjectName} ExtensibleSchema subform, (waarbij {ObjectName} de naam is van de standaard klasse Structured object, bijvoorbeeld – persoon).
8. Voeg een resource toe en selecteer het subformulier (dat u bijvoorbeeld TestSubform) hebt gemaakt en sla het subformulier $ {ObjectName} ExtensibleSchema op.
9. Vervang de sjabloon voor de Domino-map {PUBNAMES. NTF} met de nieuwe aangepaste sjabloon {CONTOSO. NTF} door [de volgende stappen uit te voeren](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Sluit de Domino-beheerder en open de Domino-console om de LDAP-service opnieuw te starten en het LDAP-schema opnieuw te laden:
    * Plaats in de Domino-console de opdracht onder **Domino-opdracht** tekst die is ingediend om de LDAP-service opnieuw te starten- [taak LDAP opnieuw starten](https://www.ibm.com/support/knowledgecenter/SSKTMJ_10.0.1/admin/conf_startingandstoppingtheldapservice_c.html).
    * Gebruik LDAP-opdracht geven ldap- **ReloadSchema** om het ldap-schema opnieuw te laden.
11. Open de Domino-beheerder en selecteer personen & tabblad groepen om het toegevoegde kenmerk weer te geven, wordt weer gegeven in Domino toevoegen persoon (onder andere tabblad).
12. Open schema. nsf op het tabblad **bestanden** en zie toegevoegd kenmerk wordt weer gegeven onder TestSubform LDAP hulp object klasse.

**Benadering 3: het aangepaste kenmerk toevoegen aan de klasse ExtensibleObject**

1. Open het bestand {schema. nsf} dat is geplaatst in de hoofdmap
2. Selecteer LDAP-object klassen in het menu links onder **alle schema documenten** en klik op de knop **object klasse toevoegen** :
3. Geef de LDAP-naam op in de vorm van {zzzExtensibleSchema} (waarbij zzz de naam is van de standaard klasse Structured object, bijvoorbeeld persoon). Als u bijvoorbeeld het schema voor de object klasse person wilt uitbreiden, moet u de LDAP-naam {PersonExtensibleSchema} opgeven.
4. Geef een naam op voor de bovenliggende object klasse waarvoor u het schema wilt uitbreiden. Als u bijvoorbeeld het schema voor de object klasse person wilt uitbreiden, geeft u de naam van de bovenliggende object klasse {dominoPerson} op:
5. Geef een geldige OID op die overeenkomt met de object klasse.
6. Selecteer uitgebreide/aangepaste kenmerken onder verplichte of optionele velden voor kenmerk typen volgens de vereiste:
7. Nadat u de vereiste kenmerken aan de ExtensibleObjectClass hebt toegevoegd, klikt u op **opslaan & sluiten** .
8. Er wordt een ExtensibleObjectClass gemaakt voor de respectievelijke standaard object klasse met uitgebreide kenmerken.

## <a name="troubleshooting"></a>Problemen oplossen
* Zie voor meer informatie over het inschakelen van logboek registratie voor het oplossen van problemen met de connector, het [inschakelen van etw-tracering voor connectors](https://go.microsoft.com/fwlink/?LinkId=335731).
