---
title: Microsoft Identity Manager 2016 SP1 terminologie | Microsoft Docs
description: Uitgebreide lijst met termen waarnaar wordt verwezen in Microsoft Identity Manager 2016 SP1.
keywords: Terminologie
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/28/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: fimguy
ms.suite: ems
ms.openlocfilehash: 6e6af5be6c091be74c7162ef9c960c89a3302ba2
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492562"
---
# <a name="microsoft-identity-manager-2016-sp1-terminology"></a>Microsoft Identity Manager 2016 SP1 terminologie

Dit document bevat een uitgebreide lijst met termen waarnaar wordt verwezen in Microsoft Identity Manager 2016 SP1 en hoger.

## <a name="a"></a>A

**Active Directory groeps validatie** : een procedure die is GEÏMPLEMENTEERD in MIM 2016 en die zorgt voor de uniekheid van de account naam van een groep binnen een domein dat is opgeslagen in Active Directory.

**actie werk stroom** : een werk stroom die een actie uitvoert. Dit omvat het verzenden van een e-mail melding en het door voeren van wijzigingen in de MIM-service database.

**activiteit** : een werk stroom activiteit is de Basic buil ding block van Windows Workflow Foundation (WF)-werk stromen. Het maakt gebruik van de logica die tijdens de ontwerp fase en de uitvoerings tijd is gestart tijdens het bouwen en uitvoeren van werk stromen.

**activiteiten-assembly** : A. DLL of een. EXE-bestand met een .NET-assembly die de logica implementeert voor een werk stroom activiteit.

**anker** : een of meer unieke kenmerken van een object type die niet worden gewijzigd en vertegenwoordigt een object in de gekoppelde gegevens bron waaraan het object voor de connector ruimte is gekoppeld (bijvoorbeeld een werknemers nummer of een gebruikers-id).

**goed keuring** : een goed keuring is een goedkeurings punt voor werk stromen dat kan worden gebruikt voor het verkrijgen van autorisatie van een persoon voordat deze in de werk stroom doorgaat.

**goedkeurings-e-mail** : als een aanvraag moet worden goedgekeurd voordat deze wordt doorgevoerd, wordt een goedkeurings-e-mail verzonden naar geïdentificeerde goed keurders.

**goedkeurings aanvraag** : een aanvraag waarvoor een goed keuring is vereist. Bijvoorbeeld een e-mail bericht dat is verzonden door MIM 2016 naar een goed keurder als onderdeel van de verwerking van een goedkeurings activiteit.

**goedkeurings antwoord** : een antwoord op een goedkeurings aanvraag. Het bevat informatie over of de aanvraag is goedgekeurd of niet. Bijvoorbeeld een e-mail bericht dat is verzonden vanaf de MIM-invoeg toepassing voor Outlook in antwoord op een goedkeurings aanvraag.

**zoekmap met goed keuringen** : de zoek mappen die zijn gemaakt door de MIM-invoeg toepassing voor Outlook, waarmee de gebruiker een manier krijgt om goed keuringen in behandeling en voltooid te bekijken, en updates van goedkeurings aanvragen.

**goedkeurings drempel** : het aantal positieve goedkeurings berichten dat nodig is om de verwerking van een aanvraag voort te zetten.

**fiatteur** : de persoon die de aanvraag moet goed keuren om door te gaan naar de volgende fase. Ze ontvangen berichten over goedkeurings aanvragen als MIM-invoeg toepassing voor Outlook wordt gebruikt. Zie ook de vermelding ' escalatie goed keurder '.

**kenmerk stroom** : Hiermee definieert u de richting van welke kenmerk waarden tussen de MIM-service en andere externe systemen stromen.

**verificatie activiteit** : een werk stroom activiteit waarmee de identiteit van een gebruiker wordt gevalideerd. Bijvoorbeeld: wacht woord opnieuw instellen poort en Smart card-verificatie poort. Zie ook de vermelding voor ' QA-poort ' en ' vergrendelings poort '.

**verificatie controle** : een dialoog venster waarin de gebruiker een reactie moet geven om te verifiëren bij MIM 2016. Vragen die gebruikers bijvoorbeeld moeten beantwoorden om hun wacht woord opnieuw in te stellen.

**verificatie controle activiteit** : een Windows Workflow Foundation activiteit die wordt gebruikt om een uitdaging te configureren die wordt verleend aan een gebruiker om te verifiëren bij MIM 2016.

**autorisatie werk stroom** : een werk stroom met activiteiten die moeten worden voltooid voordat de aanvraag wordt doorgevoerd in de data base. Voor beelden zijn gegevens validatie en goed keuring.
<br/>

## <a name="c"></a>C

**registratie kenmerk wissen** : met dit kenmerk wordt de registratie die is gekoppeld aan een verificatie werk stroom gewist. In het geval van een vraag-en antwoord vraag worden bijvoorbeeld antwoorden opgeslagen in MIM 2016 in de vorm van registratie gegevens. Wanneer het selectie vakje registratie wissen is ingeschakeld en een werk stroom wordt opgeslagen, worden de registratie gegevens verwijderd, zodat gebruikers zich opnieuw kunnen registreren.

**berekend lid (of lid)** : een alleen-lezen set van resources die worden berekend op basis van de combi natie van hand matig beheerde leden en een filter.

**connector** : een object in de connector ruimte dat een object vertegenwoordigt in een verbonden gegevens bron en dat momenteel is gekoppeld aan een object in het omgekeerde door middel van vooraf gedefinieerde regels. De map bevat connector-objecten om kenmerk waarden te synchroniseren tussen een verbonden gegevens bron en het omgekeerde.

**connector filter** : een regel die u gebruikt om te voor komen dat verbindings ruimte-objecten worden gekoppeld aan een tekst object.

**connector ruimte** : een faserings gebied dat representaties van geselecteerde objecten en kenmerken in een verbonden gegevens bron bevat. Een connector-ruimte object is een object in de connector ruimte dat is gemaakt door een gegevens import van de verbonden gegevens bron of door gebruik te maken van regels binnen MIM om nieuwe objecten te creëren in verschillende verbonden gegevens bronnen. Deze objecten bevatten kenmerk waarden die kunnen worden geïmporteerd of geëxporteerd uit overeenkomende objecten in de verbonden gegevens bron.

**aantal XPath** : XPath-expressie die een numerieke waarde retourneert die tussen haakjes na de weergave naam van de resource wordt weer gegeven.

**lid op basis van criteria** : een alleen-lezen set van resources die worden berekend op basis van de combi natie van statische groeps leden en een filter.

**op criteria gebaseerd lidmaatschap** : een groep waarin het lidmaatschap van de groep wordt bepaald door een filter. Zie ook de vermelding voor ' statisch lidmaatschap '.

**lid van meerdere forests** : een lid van een beveiligings groep waarvan het gebruikers account in een ander forest dan het groeps account is.

**berekening van groep tussen forests** : een out-of-Box-activiteit die wordt geplaatst tussen forest-leden van een groep in de FSP-set (Foreign Security Principal) die is gekoppeld aan het forest waarin de groep zich bevindt.


**aangepaste expressie** : de beschrijvende taal die wordt gebruikt voor het definiëren van functies of kenmerk stromen in de geavanceerde modus.
<br/>

## <a name="d"></a>D

**doelset (of doel bron definitie na aanvraag)** : een set waarin een resource wordt verplaatst vanwege een aanvraag die de kenmerken van de bron wijzigt.

**standaard activiteit voor groeps validatie** : een out-of-Box-werk stroom activiteit die bepaalt of een aanvraag voor groeps beheer de MIM 2016-of Active Directory configuratie of het beleid zou schenden.

**disconnecter** : een object in de connector ruimte dat een object vertegenwoordigt in een verbonden gegevens bron en dat momenteel niet is gekoppeld aan een object in de tekst.

**disconnecter-objecten** : er zijn drie soorten disconnecters-objecten: ontconnecters, expliciete ontkoppelingen en gefilterde ontsluitingen.

**weergave naam** : een kenmerk van een resource dat wordt weer gegeven in een gebruikers interface om deze resource te identificeren. Een waarde die in een weergave naam wordt gebruikt, moet ondubbelzinnig en leesbaar zijn. Het is belang rijk om weergave naam op te geven als u de resource wilt gebruiken in verschillende MIM Portal-besturings elementen, zoals resource kiezer.

**distributie groep** : een verzameling resources die meestal gebruikers en andere groepen zijn die tegelijkertijd kunnen worden verzonden door e-mail te verzenden naar het postvak voor de groep.

**domein configuratie** : een configuratie bron die wordt gebruikt om Active Directory domeinen te model leren.

domeingebonden **groep** : een groep met domeingebonden bereik is een Active Directory groep die resources binnen een bepaald domein beveiligt en die leden van dat forest of een vertrouwde forest kunnen bevatten.

**bestand voor neerzetten** : een bestand met een bestands extensie is een XML-logboek bestand dat een potentieel-of import bewerking vertegenwoordigt.

**dynamische kenmerk waarde** : de waarde van een kenmerk dat wordt berekend op basis van andere kenmerken. Zo wordt bijvoorbeeld een naam kenmerk berekend door de opgegeven naam en de achternaam samen te voegen.

**dynamische groep** : een groep waarvan het lidmaatschap automatisch wordt bepaald en bijgewerkt door MIM 2016 door ervoor te zorgen dat de groep alle resources bevat (zoals personen, groepen, computers) die binnen de voor waarden vallen die worden uitgedrukt met XPath.
<br/>

## <a name="e"></a>E

**inventarisatie** : een lijst met resources die worden geretourneerd door de MIM 2016-service.

**escalatie** : als er binnen de opgegeven tijd geen goed keuring is voltooid, wordt de goed keuring geëscaleerd en worden extra goed keurders toegevoegd aan de goed keuring.

**escalatie fiatteur** : de gebruiker die de berichten van de goedkeurings aanvraag ontvangt als de goed keurders niet reageren. Zie ook de vermelding ' goed keurder '.

**expliciete connector** : een object in de connector ruimte dat is gekoppeld aan een object in de tekst en kan niet worden losgekoppeld door een connector filter. Een expliciete connector kan alleen hand matig worden gemaakt met de koppelings functie en kan alleen worden losgekoppeld door het inrichten of door middel van een koppeling.

**expliciete disconnecter** : een object in de connector ruimte dat niet is gekoppeld aan een object in de omgekeerde tekst en kan alleen worden gekoppeld met behulp van Joiner. Een object wordt een expliciete disconnectr door de verbinding met het object hand matig te verbreken met behulp van de koppeling.

**exporteren** : exporteren is het proces van het pushen van wijzigingen in de verbonden gegevens bronnen. Een export is altijd een Delta bewerking op basis van de laatste geslaagde invoer. MIM verwerkt wijzigingen exporteren, maar verwerkt de wijzigingen in de connector ruimte niet totdat de wijzigingen zijn bevestigd door een nieuwe import bewerking. Afhankelijk van het type beheer agent dat u gebruikt, kunnen de wijzigingen die door een export worden geïmplementeerd, het kenmerk, het object of het waarde-niveau hebben.

**Export kenmerk stroom** : het proces van het omzetten van de kenmerken van een object van de tekst van het omgekeerde naar de connector ruimte. Dit proces kan bestaan uit een-op-een-toewijzingen, het Toep assen van regel uitbreidingen om kenmerken te wijzigen of statische kenmerk waarden in te stellen. Geëxporteerde kenmerken worden klaargezet in de connector ruimte voor de volgende export naar de verbonden gegevens bron.

**Extensible Assertion Markup Language (XAML)** : een op XML gebaseerde taal waarin werk stroom definities worden weer gegeven.

**bereik filter voor extern systeem** : bepaalt welke resources u identificeert en filtert van een bronmap op basis van een bepaalde voor waarde.

**bron type voor externe systeem** : dit is het bron type in het externe systeem waarnaar de MIM 2016-resources zijn verbonden.

vlag voor het maken van een **externe systeem bron** : een para meter van een synchronisatie regel die aangeeft of een resource moet worden gemaakt in de connector ruimte als deze is gebaseerd op de relatie criteria die in het externe systeem zijn opgenomen. Zie MIM 2016-markering voor het maken van resources. <!-- Not sure what this is, should this be a term? -->

**bereik van het externe systeem** : een para meter van een synchronisatie regel met een filter waarmee resources worden gepresenteerd op het externe systeem waarop de regel van toepassing is.
<br/>

## <a name="f"></a>F

**filter** : een expressie met filter voorwaarden. Een filter komt overeen met een resource als elk van de filter voorwaarden in het filter overeenkomt met die van de resource. In MIM 2016 gebruikt filter de syntaxis XPath.

**gefilterde disconnecter** : een object in de connector ruimte dat kan worden toegevoegd of wordt geprojecteerd aan een object in het omgekeerde op basis van connector filter regels in de bijbehorende beheer agent.

**FIM-beheer agent** : een beheer agent die synchroniseert tussen de MIM 2016-service en de MIM 2016-synchronisatie service.

**FIM/MIM-client service voor wachtwoord herstel** : dit is de proxy service die zich op de computer van de eind gebruiker bevindt die communiceert met de MIM 2016-server.

**Extensies voor FIM/MIM-wacht woord opnieuw instellen** : Hiermee wordt verwezen naar de code die zich op de computer van de eind gebruiker bevindt die de functionaliteit van Windows-aanmelding uitbreidt om selfservice voor wachtwoord herstel in te stellen.

**Vlag voor het maken van FIM/MIM-resources** : een para meter van een synchronisatie regel waarmee wordt aangegeven of een resource in de MIM 2016-data base moet worden gemaakt als op basis van de relatie criteria de resources niet bestaan. Zie ook de vermelding voor de vlag ' externe systeem bronnen maken '.

**functie** : een onderdeel dat kan worden opgenomen in een synchronisatie regel of een werk stroom definitie voor het verwerken van gegevens waarden.
<br/>

## <a name="g"></a>G

**poort** : een werk stroom activiteit die wordt gebruikt in de verificatie fase van aanvraag verwerking. Zie ook de vermelding voor ' QA-poort ' en ' vergrendelings poort '.

**groep nesten** : een veld van een groeps definitie waarmee wordt opgegeven of de groep andere groepen bevat als leden van de huidige groep.

**groeps bereik** : een veld van een groeps definitie, een of ' local ', ' Global ' of ' Universal '. Zie Active Directory groeps bereik voor meer informatie.
<br/>

## <a name="i"></a>I

**importeren** : het proces waarbij de gekoppelde gegevens bron objecten worden verplaatst naar de connector ruimte voor het maken, wijzigen, verwijderen of verifiëren. Een import kan een volledige of een Delta bewerking zijn. Voor een volledige import vraagt MIM alle aangewezen objecten aan bij de verbonden gegevens bron en worden alle tijdelijke objecten verwijderd waarvoor een bijbehorend object niet is ontvangen tijdens deze import bewerking. Als gevolg hiervan is deze stap voor het uitvoeren van een profiel handig voor het opschonen van de staging-objecten in de connector ruimte. De objecten die zijn ontvangen van de verbonden gegevens bron, worden klaargezet in de connector ruimte. Voor de Delta-import om de gewenste resultaten te bieden, moet de verbonden gegevens bron een vorm van een water merk implementeren. De verbonden gegevens bron gebruikt het water merk om aan te geven wanneer de meest recente wijzigingen in een object zijn opgetreden. MIM leest het water merk om te bepalen wat moet worden meegenomen in de Delta-import. Een voor beeld is het Active Directory USN.

**Import kenmerk stroom (IAF)** : kenmerk stroom importeren is het proces van het importeren van een kenmerk uit de connector ruimte naar de omgekeerde tekst. Dit proces kan ertoe leiden dat er een-op-een-kenmerk toewijzingen worden toegepast met behulp van regel uitbreidingen om kenmerken te wijzigen of statische kenmerken in te stellen.

**afbeeldings-URL** : URL voor een afbeeldings bestand dat moet worden weer gegeven in de gebruikers interface van de MIM 2016-Portal.

**eerste stroom** : een eerste stroom is een stroom voor kenmerk waarden die slechts eenmaal wordt toegepast wanneer de resource voor de eerste keer wordt gemaakt. Dat wil zeggen dat er alleen een eerste wacht woord wordt gemaakt wanneer u voor de eerste keer een account maakt.

**interactieve werk stroom** : een werk stroom die een reactie vereist van de gebruiker die een wijziging aanvraagt, zoals voor het uitvoeren van aanvullende verificatie controles.
<br/>

## <a name="j"></a>J

**samen voegen** : een koppeling is het proces van het koppelen van een connector Space-object met een bestaand omgekeerd object. Kenmerk waarden stroomt alleen tussen gekoppelde objecten.

**aanvraag voor deelname aan groep** : een aanvraag om een gebruiker toe te voegen aan een groep.
<br/>

## <a name="l"></a>L

**vergren deling** : een configuratie-instelling voor een persoons bron in de MIM 2016-data base waarmee deze persoon kan worden geverifieerd bij MIM 2016 of het opnieuw instellen van wacht woorden kan worden uitgevoerd.

**vergrendelings poort** : een werk stroom activiteit in de verificatie fase van aanvraag verwerking voor het vergren delen van een gebruiker die niet kan worden geverifieerd. Zie ook de vermelding voor ' vergren delen ' en ' QA-poort '.

**drempel waarde voor vergren deling** : dit is een besturings element integer waarmee het aantal keren wordt aangegeven dat een gebruiker de verificatie werk stroom niet kan volt ooien voordat ze worden vergrendeld voor de vergrendelings duur. De standaard instelling voor deze waarde is 3. De ondergrens is 0 en de maximum limiet is 99.

**vergrendelings duur** : dit is een besturings element integer waarmee de duur in minuten van de gebruiker wordt vergrendeld na het aanduiden van de vergrendelings drempel.  De standaard instelling voor dit is 15 minuten.  De ondergrens voor deze instelling is 1 en de maximum limiet is 9999. De bovengrens stelt de beheerder in staat de bovengrens in te stellen op een waarde van meer dan één dag.

**aantal vergrendelings drempels vóór permanente vergren deling** : dit is een besturings element integer waarmee de beheerder een numerieke waarde kan configureren voor het aantal keren dat een gebruiker de vergrendelings drempel kan aanraken voordat deze permanent wordt vergrendeld.  Permanente vergren deling impliceert dat de gebruiker moet worden ontgrendeld door de systeem beheerder. Deze waarde is standaard ingesteld op 3. Het bereik voor deze instelling ligt tussen 1 en 99.
<br/>

## <a name="m"></a>M

**beheer agent** : een beheer agent (MA) verbindt een specifieke verbonden gegevens bron met de werkmap. Het is verantwoordelijk voor het verplaatsen van gegevens van de verbonden gegevens bron naar MIM en de regels waarmee de geschiktheid van de identiteits gegevens kan worden bepaald om deel te nemen aan MIM en andere verbonden gegevens bronnen. Wanneer de gegevens in de werkmap worden gewijzigd, kan de beheer agent de gegevens exporteren naar de verbonden gegevens bronnen om te zorgen dat de verbonden gegevens bron is gesynchroniseerd met de gegevens in MIM. Er wordt een beheer agent gebruikt in plaats van een connector op basis van een agent.

**Management Policy rule (MPR)** : beheer beleidsregels (beheer beleidsregels) bieden een mechanisme voor het model leren van bedrijfs verwerkings regels voor inkomende aanvragen op de MIM 2016-server. Ze bepalen de machtigingen voor het aanvragen van bewerkingen op MIM 2016-resources samen met de werk stromen die worden geactiveerd door deze aanvragen.
   
   Er zijn twee soorten beheer beleidsregels:
   
- Aanvraag beheer beleidsregels: machtigingen verlenen en werk stromen uitvoeren (aangeroepen voordat een aangevraagde bewerking is uitgevoerd).
- Overgangs beheer beleidsregels instellen: alleen werk stromen (reactie op een toegepaste status wijziging) uitvoeren.
   
  De belangrijkste ontwerp objecten voor beheer beleidsregels zijn:
   
- Model machtigingen
- Toewijzingen voor model werk stromen
- Model overgangen
- Reflexieve definities model leren
- Niet-geverifieerde gebruikers toegang model leren
- Het model leren van tijdelijke beleids regels
- Model filter machtigingen

**hand matig beheerd lid** : het lidmaatschap van de groep of set die bestaat uit een hand matig geselecteerde lijst van gebruikers, groepen of andere resources.

Meta- **tekst** : het centrale gegevens archief dat door MIM wordt gebruikt om de geaggregeerde identiteits gegevens op te nemen uit meerdere verbonden gegevens bronnen, met één globale, geïntegreerde weer gave van alle gecombineerde objecten. De tekst van de inverse mag niet worden gebruikt als tabel of weer gave van geaggregeerde identiteits gegevens voor een andere toepassing dan MIM, omdat beschadiging kan resulteren.

**bewaakt postvak** : een postvak dat door MIM 2016-service monitors wordt gebruikt om goed keuring te ontvangen en e-mail berichten op te vragen bij de MIM-invoeg toepassing voor Outlook.
<br/>

## <a name="n"></a>N

**meldings activiteit** : een werk stroom activiteit binnen de actie fase van aanvraag verwerking waarbij MIM 2016 e-mail berichten verzendt naar een of meer gebruikers om hen op de hoogte te stellen van de aanvraag.

**meldings bericht** : een e-mail bericht dat wordt verzonden door een meldings activiteit. Zie ook de vermelding ' meldings activiteit '.
<br/>

## <a name="o"></a>O

**ObjectID (ResourceID)** : een kenmerk dat een GLOBALLY Unique Identifier (GUID) bevat die door MIM 2016 aan elke resource wordt toegewezen wanneer deze wordt gemaakt. Dit wordt ook wel een resource-ID genoemd.

**object-id** : een volg orde van getallen die wordt gebruikt als een id voor een veld in een X. 509-digitaal certificaat of voor een kenmerk type of object klasse in een op LDAP gebaseerde adreslijst service. Object-id's worden meestal toegewezen door software leveranciers en standaard instellingen.

**bewerkings type** : het bewerkings type is aangevraagd voor de resource die wordt beheerd door MIM 2016 via de webservice. Dit omvat het maken en verwijderen van resources en het lezen en wijzigen van bron kenmerken. Daarnaast kunt u met behulp van Add-/Remove-bewerkingen verdere controle Toep assen op de wijzigings bewerking om alleen de toevoeging van waarden voor kenmerken of het verwijderen ervan te beheren.

**operator** : een element van een filter waarmee een vergelijking of een andere relatie tussen gegevens waarden wordt opgegeven.

**oorspronkelijke set (of doel bron definitie vóór aanvraag)** : een set waarbij een resource vóór een wijziging in de kenmerken van de bron hoort.
<br/>

## <a name="p"></a>P

**para meter** : bij het inrichten van nieuwe resources is het soms mogelijk om kenmerk waarden van een externe bron, zoals een gebruiker, op te geven. Een kenmerk waarde wordt door gegeven als para meter om een nieuwe resource te kunnen maken.

**partitie** : een logisch gegevens volume in de connector ruimte. Een beheer agent kan een of meer partities maken om gegevens logisch in afzonderlijke logische groeperingen te verdelen. Elk gegevens volume wordt afzonderlijk verwerkt tijdens de synchronisatie.

**wacht woord opnieuw instellen** : een procedure waarmee het wacht woord van een gebruiker kan worden gewijzigd in een bekende waarde, in situaties waarin de gebruiker het wacht woord verg eten of kwijtraakt. Zie ook de vermelding voor "registratie".

**fase** : elke aanvraag voor het maken, bijwerken of verwijderen van resources wordt verwerkt via drie werk stroom fasen. In de verificatie fase kunnen aanvullende verificatie controles van de gebruiker die de aanvraag heeft ingediend worden uitgevoerd. In de autorisatie fase worden alle benodigde goed keuringen verzameld. In de actie fase worden de activiteiten uitgevoerd nadat de aanvraag voor het wijzigen van de resource is doorgevoerd.

**tijdelijke objecten** : objecten in de connector ruimte die een enkel niveau van de hiërarchie van de verbonden gegevens bron vertegenwoordigen. Als u bijvoorbeeld objecten wilt synchroniseren met een Active Directory-forest, moet u de containers importeren waaruit het pad voor de Active Directory-objecten is opgebouwd. In het voor beeld, CN = MikeDan, OE = users, DC = micro soft, DC = com, worden tijdelijke objecten voor plaatsaanduidingen gemaakt voor DC = com en DC = micro soft, DC = com. Een object met een tijdelijke aanduiding kan ook een object vertegenwoordigen in de verbonden gegevens bron waarnaar de waarde van een geïmporteerd referentie kenmerk verwijst (bijvoorbeeld het object waarnaar het Manager-kenmerk verwijst in een gebruikers object). Tijdelijke objecten bevatten geen kenmerk waarden en kunnen niet worden gekoppeld aan de tekst.

**Beleids beheer** : beleids beheer in MIM 2016 wordt mogelijk gemaakt door een share point-console voor het ontwerpen en afdwingen van beleid. Met uitbreid bare op Windows Workflow Foundation gebaseerde werk stromen kunnen gebruikers beleid voor identiteits beheer definiëren, automatiseren en afdwingen. Beleids beheer omvat ook heterogene identiteits synchronisatie en consistentie die wordt behaald door integratie van een breed scala aan netwerk besturingssystemen, e-mail, data bases, mappen, toepassingen en bestanden voor bestands toegang.

**beleids update (of uitvoeren op beleids update)** : als een werk stroom opnieuw moet worden uitgevoerd als gevolg van een wijziging in de sets of beheer beleidsregels die ernaar verwijzen, wordt de vlag voor beleids updates gebruikt om dit aan te geven.

**prioriteit** : een volg orde van de synchronisatie regels.

**Principal set** : een set die in beheer beleidsregel wordt gebruikt om de set resources op te geven die de evaluatie van de beheer beleids regel initieert.

**Principal ingesteld ten opzichte van resource** : dit is een reflexieve eigenschap. De waarde ervan wordt gedefinieerd in termen van een van de resource-eigenschappen. Het wordt gebruikt voor het definiëren van regels voor dynamisch beheer waarvan de voor waarden worden geëvalueerd in de context van elke doel resource die wordt verwerkt.

**projectie** : het proces van het maken van een object in de tekst die is gebaseerd op projectie regels en dat object vervolgens automatisch koppelen aan een bestaand object in de connector ruimte.

**inrichten** : het proces van het maken, wijzigen van de naam en het opheffen van de inrichting van objecten in vooraf bepaalde connector ruimten op basis van wijzigingen in een object in de tekst. Inrichtings regels kunnen worden ingesteld om te worden aangeroepen wanneer een omgekeerd object wordt gewijzigd. Met deze regels kunnen acties op object niveau worden uitgevoerd, zoals het maken van een nieuw connector-ruimte object of het verbreken van bestaande connector-ruimte objecten die zijn gekoppeld aan het omgekeerde object.
<br/>

## <a name="q"></a>Q

**QA-Gate** : een werk stroom activiteit in een verificatie fase waarin de aanvragende gebruiker antwoorden moet geven op een of meer vooraf vastgestelde vragen. Deze activiteit wordt doorgaans gebruikt bij het opnieuw instellen van wacht woorden, om de gebruiker te vragen hun identiteit te bewijzen door de gebruiker te voorzien van een aantal vooraf bepaalde vragen waarvoor alleen die gebruiker weet dat de gebruiker het juiste antwoord moet opgeven. Zie ook de vermelding ' poort vergren delen '.

**QA-uitdaging** : een uitdaging waarbij de gebruiker een reeks vragen moet beantwoorden om zich te kunnen verifiëren bij MIM 2016.
<br/>

## <a name="r"></a>R

**instellingen voor wille keurige wacht woord** : een instelling die het aantal tekens bepaalt dat vereist is voor het instellen van een wacht woord in de externe map.

**verwijzings kenmerk type** : een kenmerk type waarin de waarden van het kenmerk zijn de kenmerk waarden van de ObjectID (Globally Unique Identifiers) van andere resources in MIM 2016.

**referentiële integriteit** : een beperking in MIM 2016 waarin een verwijzings kenmerk geen waarde kan hebben van een ObjectID van een resource die is verwijderd.

**registratie** : een procedure voor het configureren van self-service voor wachtwoord herstel voor een gebruiker. Zie ook de vermelding ' QA-Gate '.

**opnieuw registreren** : het bijwerken van de registratie voor een authenticatie vraag in MIM 2016, doorgaans vereist na een wijziging in het beheer beleid voor registratie van wacht woord opnieuw instellen.

**relatie maken** : Configuratie vlaggen van een synchronisatie regel waarmee wordt bepaald of resources automatisch in MIM 2016 of in het externe systeem moeten worden gemaakt als de resources ontbreken.

**relatie criteria** : instelling van een synchronisatie regel die wordt gebruikt voor het vergelijken van resources in MIM-server en bronnen in externe systemen.

**relatie beëindigen** : Hiermee geeft u aan of gerelateerde resources in andere externe systemen moeten worden losgekoppeld (en mogelijk verwijderd) wanneer de synchronisatie regel niet meer van toepassing is.

**aanvraag beheer** : de mogelijkheid van een gebruiker om te communiceren met verzonden aanvragen en bijbehorende werk stromen.

**beheer beleidsregel voor aanvragen (RMPR)** : een type beheer beleidsregel dat wordt geëvalueerd en toegepast op inkomende aanvragen om bewerkingen uit te voeren. RMPRS worden voornamelijk gebruikt voor het schrijven van toegangs beleids definities in MIM. Met andere woorden, het antwoord op de manier waarop de aanvraag wordt verwerkt. Wanneer u een RMPR configureert, bevindt de aanvrager zich in de set die is ontworpen voor het uitvoeren van een bewerking.

   De MIM-architectuur definieert zes verschillende bewerkingen waarvoor een RMPR kan worden gedefinieerd:
   
- Een resource maken.
- Een resource verwijderen.
- Een resource lezen.
- Voeg een waarde toe aan een kenmerk met meerdere waarden.
- Een waarde uit een kenmerk met meerdere waarden verwijderen.
- Een kenmerk met één waarde wijzigen.
   
  Wanneer u een RMPR definieert, moet u ten minste één van deze zes bewerkingen selecteren. De bewerkingen worden altijd gedefinieerd in de context van de aanvrager. Voor elke voor waarde is de definitie van een doel vereist. Een bewerking die wordt toegepast op een doel, kan resulteren in een status overgang van de doel resource. Er wordt altijd een RMPR aangeroepen voordat een aangevraagde bewerking is uitgevoerd. Als u het doel van uw voor waarde effectief wilt karakteriseren, moet u twee verschillende statussen configureren:
   
- Doel resource definitie vóór aanvraag: de status van het doel voordat de aanvraag wordt toegepast.
- Doel resource definitie na aanvraag: de status van het doel nadat de aanvraag is toegepast.
   
  Of u beide statussen moet definiëren, is afhankelijk van de bewerkingen waarvoor uw RMPR is gedefinieerd. In een Maak bewerking heeft de aangevraagde resource geen begin status. Daarom hoeft u de doel resource definitie na aanvraag alleen te configureren voor een Maak bewerking.
   
  Een lees-of verwijder bewerking veroorzaakt geen status overgang. Voor deze twee typen bewerkingen hoeft u alleen de doel bron definitie op te geven voor de aanvraag.
   
  Voor een wijzigings bewerking of voor alle andere Combi Naties van bewerkingen moet u beide statussen configureren. Dit kan dezelfde waarden hebben als er geen status overgang plaatsvindt.
   
  U kunt de gerelateerde resources uitdrukken ten opzichte van de aanvrager (zoals het eigen gebruikers object van de aanvrager, de Manager van een doel gebruiker of de eigenaar van een doel groep).
   
  De meest eenvoudige vorm van een reactie op een voor waarde is het verlenen van machtigingen voor het uitvoeren van de aangevraagde bewerking. Naast het verlenen van machtigingen kunt u ook andere bewerkingen als reactie op een voor waarde in een RMPR definiëren. In de MIM-architectuur worden deze bewerkingen gedefinieerd in de vorm van werk stromen. Op het moment dat een bepaalde RMPR wordt verwerkt, heeft het systeem mogelijk niet voldoende informatie om machtigingen te verlenen. In dit geval kunt u aanvullende verificatie-en autorisatie stappen in uw RMPR definiëren die van toepassing zijn op de persoon die een bepaalde aanvraag uitvoert. Als u bijvoorbeeld toestemming wilt geven om de aangevraagde bewerking uit te voeren, is het mogelijk dat een gebruiker hand matig moet worden gecommuniceerd om de bewerking goed te keuren.
   
  De volgende afbeelding bevat een overzicht van de volledige architectuur van een RMPR:
   
  ![RMPR-architectuur](media/mim-2016-sp1-terms/8f5054080d3f8bd9c73d93a5e3ed51f9.gif)
   
  Wanneer een nieuw aanvraag object wordt gemaakt in MIM, voert het systeem een query uit op de geconfigureerde RMPRs voor overeenkomende objecten door de aanvraag voorwaarden te vergelijken met de ingestelde voor waarden in uw beheer RMPRs. Als overeenkomende RMPRs zich bevinden, worden deze toegepast op het aanvraag object in de wachtrij. In de volgende afbeelding ziet u een overzicht van dit proces:
   
  ![Overeenkomende RMPRs zijn gevonden en toegepast op het object in de wachtrij aanvraag](media/mim-2016-sp1-terms/65bdb65aaffa55a90707ff6f7b13632d.gif)
   
  In de MIM-Portal moeten machtigingen voor bewerkingen expliciet worden verleend. Met andere woorden, tenzij wordt verleend door een RMPR, worden alle bewerkingen voor resources geweigerd. Elk aanvraag object vereist ten minste één RMPR die toestemming verleent om de gevraagde bewerking uit te voeren op een doel.

**aanvraag object** : wanneer een gebruiker een taak uitvoert in de MIM-portal of de MIM-invoeg toepassing voor Outlook, wordt deze weer gegeven als een aanvraag object. Aanvraag objecten vertegenwoordigen een handig rapportage mechanisme voor activiteiten in uw systeem.

   Elk aanvraag object heeft de volgende onderdelen:
   
- Aanvrager: een resource die vraagt om een bewerking uit te voeren.
- Bewerking: de actie die de aanvrager wil uitvoeren.
- Doel: de resource die het doel is van de aangevraagde bewerking.
   
  Een aanvraag object is logischerwijze een implementatie van de volgende instructie:
   
  `The requester attempts to perform the following operation on this target...`
      
  De volgende afbeelding bevat een overzicht van de algemene architectuur van een aanvraag object:
   
  ![Architectuur Aanvraagobject](media/mim-2016-sp1-terms/788e9b3a4fcddb78ff4a05dcb5e61192.gif)
   
  Elk aanvraag object heeft een status-eigenschap om de verwerkings status aan te geven. Voor het verwerken van aanvragen kan hand matige interactie nodig zijn om een aanvraag te volt ooien. Zo kan de eigenaar van een groep verplicht zijn om de aanvraag van een andere gebruiker hand matig goed te keuren om lid te worden van een groep. Naast hand matige interactie kunt u MIM ook zo configureren dat een specifieke aanvraag automatisch wordt verwerkt zonder menselijke interactie.
   
**verwerkings model voor aanvragen** : het model voor het verwerken van aanvragen in MIM bestaat uit drie hoofd fasen:

- Fase 1: authenticatie
- Fase 2: autorisatie
- Fase 3: actie
   
  Werk stromen die elk een of meer activiteiten bevatten, kunnen worden gekoppeld aan elk van deze fasen en worden uitgevoerd in de context van het uitvoeren van één aanvraag. Een aanvraag kan worden geïnitieerd vanuit één aanroep van een gebruiker naar een van de webservice-eind punten of via een gebruiker die een aanvraag in de MIM-Portal maakt.
   
  In de volgende afbeelding ziet u de relatie van de onderdelen van de aanvraag verwerking:
   
  ![Relatie tussen onderdelen van aanvraag verwerking](media/mim-2016-sp1-terms/0e57445d8b9a3216c31add80eb26493f.gif)
   
  Aanvragen worden in de volgende volg orde verwerkt:
   
- Het maken van het aanvraag object: MIM 2016 maakt een aanvraag object in reactie op een aanroep van een van de webservice-eind punten of vanwege een aanvraag die via de MIM-portal wordt gestart.
   
- MPR-evaluatie: de rechten van de aanvrager om de actie aan te vragen, worden gevalideerd en de berekening van de betreffende werk stromen wordt uitgevoerd. De aanvraag wordt gecontroleerd op basis van toewijzingen aan MPR-objecten. Als u wilt toewijzen aan een MPR, moeten alle toepasselijke velden van de MPR voor de aangevraagde bewerking overeenkomen. Dit omvat de aanvrager, bewerking, doel resource en kenmerken. Als al deze voor waarden, waaronder de betrokken kenmerken, geldt voor een inkomende aanvraag, wordt de juiste MPR vergeleken met de aanvraag. Een aanvraag moet worden toegewezen aan ten minste één MPR die de machtiging verleent als onderdeel van de definitie. Als dit het geval is, wordt de aanvraag door gegeven via de machtigings controle fase van aanvraag verwerking. Als dit niet het geval is, mislukt de aanvraag. Het systeem bepaalt ook de ingestelde overgangen die deel uitmaken van de aanvraag en zoekt alle gerelateerde set beheer beleidsregels op basis van overgang.
   
- Verificatie: MIM 2016 voert verificatie werk stromen een voor een in een niet-deterministische volg orde uit om de identiteit van de aanvrager te bevestigen.
   
- Autorisatie: MIM 2016 bevestigt de machtiging van de aanvrager om de aangevraagde bewerking uit te voeren op de resource die in de aanvraag is opgegeven. Alle afhankelijke autorisatie werk stromen worden parallel uitgevoerd, maar een aanvraag wordt niet doorgevoerd in het MIM-object archief, tenzij alle werk stromen zijn voltooid en alle zijn geslaagd.
   
- Verwerking: MIM 2016 voert de aangevraagde bewerking uit in het MIM-toepassings archief.
   
- Actie: MIM 2016 voert alle processen uit die moeten worden uitgevoerd vanwege de aangevraagde bewerking. Alle actie werk stromen worden parallel uitgevoerd. Voor lees bewerkingen zijn geen werk stromen toegepast op hun verwerking. Dit omvat de geconfigureerde werk stromen in de RMPR en de werk stromen in het beheer beleidsregels ingesteld op basis van overgang.
   
  >[!NOTE]
  >Aanvragen die door het synchronisatie account worden gestart, passeren alle verificatie-en autorisatie werk stromen die op de accounts van toepassing zijn. Alle toepasselijke actie werk stromen worden toegepast.

**aanvrager** : de identiteit van de gebruiker of service die een aanvraag heeft ingediend bij MIM 2016.

**leverancieraanvraag** : een geconfigureerde verzameling gebruikers die een aanvraag kan indienen. Kan "iedereen" of een specifieke set gebruikers zijn die door een filter worden gedefinieerd.

**resource** : een exemplaar van een bepaald resource type in MIM 2016. Elke resource wordt uniek geïdentificeerd door het ObjectID-kenmerk (ResourceID).

**Weergave configuratie voor bron besturings elementen (RCDC)** : RCDCs zijn configuratie resources die worden gebruikt voor het weer geven van de gebruikers interface in het resource besturings element (RC) voor het ontwerpen van een specifiek resource type in MIM 2016.

**huidige set van resource** : onderdeel van de definitie van de voor waarden van management policy rules (MPR). Het verzamelen van doel resources op het moment dat de aanvraag wordt ontvangen. Is van toepassing op lezen, verwijderen en bewerkings typen wijzigen.

**definitieve resourceset** : een deel van de definitie van de voor waarde in beheer beleidsregels. Het verzamelen van doel resources na het verwerken van de aanvraag. Is alleen van toepassing op het maken en wijzigen van de bewerkings typen.

**resource hiërarchie** : in een directory service is de hiërarchie van een bron vermelding het verzamelen van Directory-vermeldingen tussen de basis van een naamgevings context en die bron vermelding.

**resource bereik** : een set resources waarover een aanvraag kan worden verzonden.

**resource type** : een deel van een schema waarmee de weer gave van een resource in MIM 2016 wordt gedefinieerd.

**toewijzing van resource type** : een relatie tussen een resource type dat wordt gebruikt om een resource te vertegenwoordigen in MIM 2016 en een resource klasse die wordt gebruikt om die resource in de meta tekst aan te geven.

**rol** : een door de organisatie toegewezen beveiligings-principal die wordt gebruikt voor het beheren van toegangs rechten.

**uitbrei ding van regels** : een uitbrei ding van regels is een Dynamic-Link Library (. dll) die een gedefinieerde set regels voor het beheren van gegevens bevat. U kunt regel uitbreidingen gebruiken tijdens synchronisaties om functionaliteit uit te breiden. U kunt bijvoorbeeld een uitbreidings module gebruiken om gegevens uit twee bron kenmerken te combi neren en ze te stroom lijnen naar één doel kenmerk (bijvoorbeeld, `sn` en `givenName` naar `displayName` ).

**uitvoerings geschiedenis** : een set statistieken waarin de resultaten van één uitvoering van een beheer agent worden weer gegeven.

**profiel uitvoeren** : een uitvoerings profiel vertegenwoordigt een reeks stappen die aangeven hoe een beheer agent en configuratie-instellingen moeten worden uitgevoerd om te bepalen hoe een beheer agent wordt uitgevoerd. Een beheer agent kan meerdere uitvoerings profielen hebben, die worden opgeslagen met de beheer agent. Een uitvoerings profiel bestaat uit ten minste één stap voor het uitvoeren van een profiel.
<br/>

## <a name="s"></a>S

**zoekmap** : Zie de vermelding voor de zoekmap ' goed keuringen '.

**Zoek bereik** : Hiermee geeft u de eigenschappen op voor een bepaalde Zoek context die een gebruiker kan uitvoeren vanuit de MIM 2016-Portal. Een gebruiker kan bijvoorbeeld een zoek bereik selecteren in een vervolg keuzelijst voor alle gebruikers, alle distributie lijsten, ' mijn wachtende goed keuringen ', en de zoek resultaten worden beperkt tot items die voldoen aan deze criteria, naast de zoek termen die zijn opgegeven door de gebruiker.

**security descriptor** : een structuur en bijbehorende gegevens die de beveiligings gegevens voor een Beveilig bare bron bevatten. Een security descriptor identificeert de eigenaar en de primaire groep van de resource. Het kan ook een DACL bevatten waarmee de toegang tot de resource wordt beheerd, en een SACL waarmee de logboek registratie van pogingen voor toegang tot de bron wordt geregeld.

**beveiligingsprincipal: een** identiteit die wordt gebruikt voor beveiligings beheer, zoals een gebruikers account, die kan worden geverifieerd bij een service.

**beveiligings token** : een protocol element waarmee verificatie-en autorisatie gegevens worden overgebracht op basis van een referentie. In de protocollen voor webservices wordt een beveiligings token weer gegeven als een XML-element in een SOAP-header, zoals gedefinieerd door WS-Security.

**beveiligings token service** : een service die het WS-Trust-protocol implementeert om de vertrouwens relatie tussen clients en services te beheren op basis van de uitwisseling van beveiligings tokens.

**sequentiële werk stroom** : alle werk stromen in MIM 2016 worden afgeleid van de sequentiële werk stroom Windows Workflow Foundation. Het bevat meerdere werk stromen in een sequentiële volg orde.

**Service account** : een Windows-account dat is toegewezen om te worden gebruikt door een Windows-service, in plaats van te worden gebruikt door een gebruiker om zich aan te melden bij een computer systeem. Dit vertegenwoordigt het systeem account van MIM.

**Set** : een benoemde verzameling resources. Meestal worden sets gebruikt om resources te organiseren op basis van regels. Het lidmaatschap van een set is hand matig beheerd of op basis van criteria. Dit betekent dat u hand matig resources aan een set kunt toevoegen en hoe u criteria kunt definiëren waarmee automatisch resources worden toegevoegd aan een set op basis van een filter-instructie. Wanneer een resource voldoet aan de filter criteria, wordt deze automatisch toegevoegd aan de gerelateerde set.

**Beleids regel voor overgangs beheer instellen (TMPR)** : een beheer beleidsregel die wordt toegepast op wijzigingen in het lidmaatschap van een set. Stel Tmpr's Toep assen actie werk stromen in als object overgang naar of van een opgegeven set in de MPR.

   Er zijn twee soorten Tmpr's:
   
- Overgang in: een resource wordt lid van de overgangs set.
- Overstappen: een resource verlaat de overgangs.
   
   >[!NOTE]
   >Wanneer een overgangs wordt verwijderd, behandelt het systeem het verwijderen als een overgangs gebeurtenis voor de betrokken objecten.
      
  Het antwoord is een reactie op een toegepaste status wijziging. Wanneer de gerelateerde MPR wordt aangeroepen, is de voor waarde al toegepast. Dit betekent dat de betrokken resources al zijn overgezet naar of uit een overgangs. Voor Tmpr's is het doel van de reactie niet om de reactie op een aangevraagde bewerking te definiëren, maar om het antwoord op een toegepaste bewerking te definiëren. Met andere woorden, voor een MPR op basis van een overgang, is het niet van belang om de status te bereiken. Wat relevant is, zijn de gevolgen van een status wijziging.
   
  Wanneer u een set op basis van overgangen gebaseerde MPR in MIM configureert, moet u de volgende drie instellingen opgeven:
   
- Overgangs set
- Type overgang
- Beleidswerkstromen
   
  De beleids werk stromen zijn definities van de processen die moeten worden aangeroepen als reactie op de status wijziging. De meest voorkomende use cases voor beheer beleidsregels zijn het verlenen of intrekken van rechten en het inrichten en ongedaan maken van de inrichting in externe gegevens bronnen.
   
  De volgende afbeelding bevat een overzicht van de volledige architectuur van een set op basis van overgangen gebaseerd:

  ![TMPR-architectuur instellen](media/mim-2016-sp1-terms/e12154d35b48cec676f160930ff33662.gif)
  
  Beheer beleidsregels op basis van overgangen instellen worden geactiveerd door aanvragen. Wanneer een aanvraag wordt verwerkt en goedgekeurd door een RMPR, bepaalt de MIM-service ook of een goedgekeurde aanvraag resulteert in een status overgang en of een op een status overgang gebaseerde MPR die de status wijziging verwerkt, bestaat.
  
  In de volgende afbeelding ziet u een overzicht van de relatie tussen een aanvraag en een set op basis van een op overgang gebaseerde MPR:
  
  ![Relatie tussen een aanvraag en een set-TMPR](media/mim-2016-sp1-terms/426be3a3a5380720fa1eaa3e7df360df.gif)

**Sid** : een unieke waarde die wordt gebruikt om een gebruikers account, groeps account of aanmeldings sessie te identificeren.

**SOAP** : een protocol voor het uitwisselen van gestructureerde informatie tussen software onderdelen.

**synchronisatie** : het proces waarbij de geselecteerde gegevens in meerdere gegevens bronnen in overeenstemming worden bewaard. Synchronisatie vertegenwoordigt alleen bewerkingen voor objecten in MIM. Synchronisatie kan een bewerking zijn voor de volledige set gegevens waarvoor deze is gedefinieerd in een beheer agent of een Delta bewerking, conform de wijzigingen sinds de laatste bekende bewerking. De stap profiel voor het uitvoeren van een synchronisatie definieert de inkomende en uitgaande synchronisatie processen.

   De stap voor het profiel voor het uitvoeren van synchronisatie heeft twee subtypen:
   
- Deltasynchronisatie
- Volledige synchronisatie
   
  Tijdens Delta synchronisatie worden door MIM alleen geïmporteerde objecten verwerkt. Dit zijn de staging-objecten die zijn gemarkeerd als import in behandeling. Deze stap voor het uitvoeren van het profiel is handig voor het verwerken van alleen objecten waarvoor nog wijzigingen in behandeling zijn, maar die niet zijn verwerkt tijdens een eerdere synchronisatie-uitvoering.
   
  Delta synchronisatie wordt gebruikt in twee vooraf gedefinieerde run Profiles en gedraagt zich iets anders. Het eerste uitvoerings profiel is Delta synchronisatie, waarbij er geen import van een verbonden bron wordt uitgevoerd, maar alle objecten in de connector ruimte worden geëvalueerd en objecten met wachtende wijzigingen worden verwerkt. In het tweede uitvoerings profiel worden Delta-import en Delta synchronisatie gecombineerd. Met dit profiel uitvoeren worden alleen de objecten en kenmerken geïmporteerd uit de verbonden gegevens bron waarvan de waarden zijn gewijzigd sinds de laatste keer dat de beheer agent werd uitgevoerd. Beheer agent regels worden vervolgens alleen toegepast op objecten die in behandeling zijnde wijzigingen van de Delta-import. Objecten zonder wijzigingen die in behandeling zijn van die Delta-import worden niet geëvalueerd.
   
  Tijdens de volledige synchronisatie evalueert MIM synchronisatie regels en past deze toe op alle staging-objecten in een connector ruimte. Volledige synchronisatie moet worden gestart wanneer er wijzigingen zijn toegepast op de regels van een bepaalde omgeving. Afhankelijk van het aantal objecten in uw connector ruimte, kan dit een tijdrovende bewerking zijn, dus regel matig gewijzigde synchronisatie regels in uw productie omgeving moeten worden vermeden.

**synchronisatie filter** : een filter om te voor komen dat bronnen in de tekst worden overgedragen naar de MIM 2016-data base.

**synchronisatie regel** : een regel voor het stroom lijnen van resource gegevens tussen de MIM-server (inclusief de MIM-synchronisatie-engine) en het verbonden externe systeem.
<br/>

## <a name="t"></a>T

**tijdelijk beleid** : een set Transition-MPR die is gebonden aan een tijdelijke set. Het beleid wordt toegepast op het verstrijken van de tijd, omdat objecten worden overgezet naar en van de set op basis van de definitie van de tijdelijke set.

**Tijdelijke set** : een type van een set-object dat is gebaseerd op relatieve datums. Tijdelijke sets bieden een mechanisme waarmee het proces van overgang naar of van een set volledig kan worden geautomatiseerd op basis van het verstrijken van de tijd. Zo kan een tijdelijke set worden gedefinieerd voor alle groepen die vandaag een week verlopen. Het systeem evalueert de objecten in het systeem automatisch en voegt deze dagelijks aan deze set toe. Met andere voor beelden kunt u dynamische definities van een tijd verwijzing toestaan, zoals een filter dat is gebaseerd op x dagen vanaf vandaag.

**tijdgebonden gebeurtenis** : een overgangs gebeurtenis die optreedt nadat een geconfigureerd tijds interval is verstreken of een specifieke datum en tijd is bereikt.

**time-out** : een periode waarin MIM 2016 wacht op goedkeurings antwoorden totdat een activiteit wordt geëscaleerd.

**transitieset** : een set die wordt gebruikt in een definitie van een beleids regel voor het instellen van een overgangs beheer. Het beleid wordt toegepast op de wijzigingen in het set-lidmaatschap, waarmee objecten de set kunnen invoeren of verlaten, afhankelijk van de configuratie van de TMPR.
<br/>

## <a name="u"></a>U

**Ontgrendelde groep** : een groep waarin het lidmaatschap van de groep kan worden gewijzigd door andere gebruikers dan de eigenaar van de groep.

**universele groep** : een groep met een universeel bereik is een Active Directory groep die leden van een bepaald forest kan bevatten. Aan een universele groep kunnen machtigingen worden toegewezen in elk domein of forest. Distributie lijsten hebben doorgaans een universeel bereik.  Een beveiligings groep met een universeel bereik kan bronnen in hetzelfde forest beveiligen.

**update aanvraag** : een aanvraag om de kenmerken van een resource te wijzigen.

**gebruiks trefwoord** : een gebruiks trefwoord wordt gebruikt om te bepalen welke zoekbereiken worden weer gegeven voor een specifieke pagina in de gebruikers interface van de portal. Elke lijst weergave pagina in de gebruikers interface bevat nul of meer gebruiks trefwoorden en de gebruikers interface voor die pagina bevat alle zoekbereiken die overeenkomende tref woorden bevatten. Bij het ontwerpen van zoekbereiken kunnen klanten nul of meer tref woorden opgeven per zoek bereik, om aan te passen welke zoekbereiken worden weer gegeven voor een bepaalde pagina in de gebruikers interface. Het wordt ook gebruikt om te bepalen welke start pagina resource en navigatiebalk bron worden weer gegeven voor welke groep gebruikers. Het wordt ook gebruikt in schema beheer om schema-elementen te beveiligen en te labelen die nodig zijn voor verschillende onderdelen van MIM.
<br/>

## <a name="w"></a>W

**webportal** : een gebruikers interface die wordt geïmplementeerd door een software toepassing via een onderdeel van een webserver, zoals IIS.

**webservice** : een protocol interface naar een service die is geïmplementeerd met behulp van een HTTP-protocol.

**werk stroom** : een werk stroom is een set van elementaire eenheden die activiteiten worden genoemd en die worden opgeslagen als een model dat een echt proces beschrijft. Werk stromen bieden een manier om de volg orde en de afhankelijke relaties tussen werk items te beschrijven. Deze werk stroomt het model van begin tot eind en de activiteiten kunnen worden uitgevoerd door personen of systeem functies. Met andere woorden, als een reactie op een aanvraag complexe verwerking vereist, worden de stappen in een werk stroom object ingekapseld. Werk stromen zijn optionele onderdelen en zijn nauw verbonden met beheer beleidsregels. Werk stromen definiëren een activiteit of activiteiten die moeten worden uitgevoerd tijdens de verwerking van een MPR. MIM installeert diverse standaard werk stromen die kunnen worden gebruikt als is of als basis voor een aangepaste werk stroom.

   Voor beelden van werk stroom activiteiten zijn:

- Een geautomatiseerd e-mail bericht verzenden om goed keuring te aanvragen.
- De kenmerken beperken die een gebruiker kan zien tijdens een aangepaste zoek opdracht.
- Validatie van een nieuwe groep met AD DS of MIM-richt lijnen.
- Het object wordt toegevoegd aan of verwijderd uit het bereik van een synchronisatie regel.
   
  Om alle verwerkings vereisten in uw omgeving op te lossen, definieert de MIM-architectuur drie typen werk stromen:
   
- Verificatie: voert aanvullende validatie van de gebruikers-id uit voordat u doorgaat met de aanvraag
- Autorisatie: valideert een aanvraag via een reeks activiteiten, zoals het verkrijgen van de vereiste externe goed keuring voordat een aanvraag wordt verwerkt.
- Actie: Hiermee worden alle verdere activiteiten verwerkt nadat de oorspronkelijke aanvraag is voltooid.
   
  Deze drie werk stromen vormen een onderdeel van het verwerkings model van de aanvraag.

**werk stroom definitie** : de definitie van de werk stroom wordt opgeslagen in de XOML-indeling die is gedefinieerd door de Windows Workflow Foundation (WF). Hiermee definieert u de activiteiten, de para meters voor de activiteiten en de volg orde waarin ze moeten worden uitgevoerd.

**Workflow Designer** : de ontwerp tijd ervaring voor het bouwen van werk stromen.

**werk stroom-host** : het Server onderdeel dat betrekking heeft op het uitvoeren van werk stromen. In MIM 2016 is de MIM 2016-service de host van de werk stroom.

**werk stroom exemplaar** : een actief exemplaar van een werk stroom definitie als effect van een aanvraag.

**werk stroom beheer** : een MIM 2016-functie die betrekking heeft op het ontwerpen van werk stromen, het uitvoeren ervan en het beheer ervan. Werk stroom beheer bestaat uit de Workflow Designer, aanvraag beheer en de host van de werk stroom.
