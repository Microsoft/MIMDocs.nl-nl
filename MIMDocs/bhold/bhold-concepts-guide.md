---
title: Handleiding voor Microsoft BHOLD-Suite concepten | Microsoft Docs
description: Ga aan de slag met de MIM 2016-onderdelen en installeer en configureer de synchronisatieservice.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/14/2017
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 32bd77140cf70047eaa02d363a1348e73783f87a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358836"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Handleiding voor Microsoft BHOLD-Suite concepten

Microsoft Identity Manager 2016 (MIM) kunnen organisaties voor het beheren van de hele levenscyclus van gebruikers-id's en hun bijbehorende-referenties. Het kan worden geconfigureerd voor identiteiten synchroniseren, centraal beheren van certificaten en wachtwoorden en inrichten van gebruikers in heterogene systemen. Met MIM, kunnen IT-organisaties definiëren en automatiseren van de processen die worden gebruikt voor het beheren van identiteiten van het maken van naar buiten gebruik stellen.

Deze mogelijkheden van MIM uitbreidt BHOLD-Suite van Microsoft door toe te voegen op rollen gebaseerd toegangsbeheer. BHOLD kan organisaties voor het definiëren van gebruikersrollen en om toegang tot gevoelige gegevens en toepassingen te beheren. De toegang is op basis van wat geschikt is voor deze rollen is. BHOLD-Suite bevat services en hulpprogramma's die het modelleren van de rolrelaties binnen de organisatie te vereenvoudigen. BHOLD worden deze rollen toegewezen rechten en te controleren dat de definities van gebruikersrollen en de bijbehorende rechten correct worden toegepast op gebruikers. Deze mogelijkheden zijn volledig geïntegreerd met MIM, biedt een naadloze ervaring voor eindgebruikers en IT-medewerkers hetzelfde.

Deze handleiding helpt u inzicht in de werking van BHOLD-Suite met MIM en bevat informatie over de volgende onderwerpen:

- Op rollen gebaseerd toegangsbeheer
- Attestation
- Analytics
- Rapporten
- Access Management-Connector
- MIM-integratie

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

De meest gebruikte methode voor het beheren van de gebruikerstoegang tot gegevens en toepassingen is via discretionary access control (DAC). In de meest voorkomende implementaties heeft elk aanzienlijke object een geïdentificeerde eigenaar. Heeft de mogelijkheid om toegang te verlenen of weigeren van toegang tot het object naar anderen op basis van individuele identiteit of het lidmaatschap van de eigenaar. DAC-gegevens in de praktijk meestal resulteert in een breed spectrum van beveiligingsgroepen, sommige die overeenkomen met organisatiestructuur, anderen die staan voor functionele groeperingen (zoals taaktypen of projecttoewijzingen) en anderen die bestaan uit makeshift verzamelingen van gebruikers en apparaten die zijn gekoppeld voor meer tijdelijke doeleinden. Wanneer organisaties groeit, wordt het lidmaatschap van deze groepen steeds moeilijker te beheren. Bijvoorbeeld, als een werknemer worden overgedragen vanuit een project naar een andere, moeten de groepen die worden gebruikt voor het beheren van toegang tot de activa projecten handmatig worden bijgewerkt. In dergelijke gevallen is het niet ongewoon is voor fouten optreden, fouten die kunnen project beveiligings- of productiviteit belemmeren.

MIM bevat functies waarmee u dit probleem verhelpen door te geven van geautomatiseerde controle over lidmaatschap van groep en distributie. Dit wordt echter niet de intrinsieke complexiteit van delende groepen die niet noodzakelijkerwijs gerelateerd zijn aan elkaar gestructureerd adres.

Een manier om aanzienlijk verkorten deze toename is door het implementeren van op rollen gebaseerd toegangsbeheer (RBAC). RBAC is DAC niet verplaatsen.  RBAC is gebaseerd op DAC door te geven van een framework voor het classificeren van gebruikers en IT-resources. Hiermee kunt u de relatie en de toegangsrechten die van toepassing op basis van deze classificatie zijn expliciete maken. Bijvoorbeeld door toe te wijzen aan een gebruiker de functie van de kenmerken die de gebruikers opgeven en projecttoewijzingen, de gebruiker toegang tot hulpprogramma's die nodig zijn voor de taak en de gegevens die de gebruiker nodig heeft om bij te dragen naar een bepaald project van de gebruiker kunnen worden verleend. Wanneer de gebruiker wordt ervan uitgegaan dat een andere taak en de ander projecttoewijzingen wijzigen van de kenmerken die de functie van de gebruiker opgeeft en projecten blokkeert automatisch de toegang tot de resources die alleen vereist voor de vorige positie van gebruikers.

Omdat rollen kunnen worden opgenomen in de rollen in een hiërarchische manier (bijvoorbeeld de rollen van de Verkoopmanager en verkoopmedewerker kunnen worden opgenomen in de algemene functie van de verkoop), kunt u eenvoudig juiste rechten heeft voor specifieke rollen toewijzen en nog steeds bieden juiste rechten voor iedereen die ook de ruimere rol. Bijvoorbeeld in een ziekenhuis alle medisch personeel kunnen worden verleend om de records van een patiënt te bekijken, maar alleen artsen (een subfunctie van de rol van medische) kunnen worden verleend aan de voorschriften voor de patiënt invoeren. Gebruikers die behoren tot de administratieve rollen kunnen op dezelfde manier de geen toegang tot patiëntendossiers, met uitzondering van facturering clerks (een subfunctie van de administratieve rol), die kunnen worden verleend tot deze gedeelten van een patiënt-records die nodig zijn voor rekening van de patiënt voor services geleverd door het ziekenhuis.

Een extra voordeel van RBAC is de mogelijkheid om te definiëren en afdwingen van scheiding van functies (beplanten). Dit kan een organisatie voor het definiëren van combinaties van functies die het verlenen van machtigingen die moeten niet worden ondergebracht door dezelfde gebruiker, zodat de functies waarmee de gebruiker een betaling starten en om een betaling, bijvoorbeeld door een bepaalde gebruiker kan niet worden toegewezen. RBAC biedt de mogelijkheid om af te dwingen dergelijk beleid automatisch in plaats van dat voor de evaluatie van de daadwerkelijke implementatie van het beleid op basis van per gebruiker.

### <a name="bhold-role-model-objects"></a>BHOLD-modelobjecten rol

U kunt opgeven en organiseren van rollen binnen uw organisatie, gebruikers toewijzen aan rollen, en de juiste machtigingen toewijzen aan rollen met BHOLD-Suite. Deze structuur het model van een functie wordt aangeroepen, en het bevat en verbinding maakt vijf typen objecten: 

- Organisatie-eenheden
- Users
- Rollen
- Machtigingen
- Toepassingen

#### <a name="organizational-units"></a>Organisatie-eenheden

Organisatie-eenheden (OrgUnits) zijn de belangrijkste middelen waarmee gebruikers zijn ingedeeld in het model BHOLD-rol. Elke gebruiker moet behoren tot ten minste één OrgUnit. (Zelfs wanneer een gebruiker wordt verwijderd uit de laatste organisatie-eenheid in BHOLD, record van de gegevens van de gebruiker is verwijderd uit de BHOLD-database.)

> [!Important]
> Organisatie-eenheden in het model van de rol van BHOLD mag niet worden verward met organisatie-eenheden in Active Directory Domain Services (AD DS). Normaal gesproken is de structuur van organisatie-eenheid in BHOLD gebaseerd op de organisatie en het beleid van uw bedrijf, niet de vereisten van uw netwerkinfrastructuur.

Hoewel het niet vereist is, in de meeste gevallen zijn organisatie-eenheden gestructureerd in BHOLD om weer te geven van de hiërarchische structuur van de werkelijke organisatie, die vergelijkbaar is met het volgende:

![](media/bhold-concepts-guide/org-chart.png)

In dit voorbeeld, zou het model van de functie organizationalganizatinal-eenheid voor het bedrijf als geheel (vertegenwoordigd door de president, omdat de president geen deel uit van een eenheid mororganizationalganizatinal maakt) of de organisatie-eenheid van BHOLD-hoofdmap (die altijd Er bestaat) kan worden gebruikt voor dat doel. Voor de zakelijke afdelingen onder leiding van de vice president OrgUnits zouden worden geplaatst in de zakelijke organisatie-eenheid. Volgende, organisatie-eenheden die overeenkomt met de marketing- en bestuur zou worden toegevoegd aan de marketing- en organisatie-eenheden en organisatie-eenheden voor de regionale verkoopmanagers zouden worden geplaatst in de organisatie-eenheid voor de Oostelijke regio Verkoopmanager. Verkoop associates, die andere gebruikers niet beheert, zou leden van de organisatie-eenheid van de verkoopmanager voor Oost-regio worden gemaakt. Houd er rekening mee dat gebruikers lid van een organisatie-eenheid op elk niveau zijn kunnen. Bijvoorbeeld, een administratief medewerker die niet een manager en rapporten rechtstreeks naar een vice president is, zou een lid zijn van de vice president van organisatie-eenheid.

Naast de organisatiestructuur voor, worden organisatie-eenheden ook gebruikt voor het groeperen van gebruikers en andere organisatie-eenheden op basis van functionele criteria, zoals voor projecten of specialisatie. Het volgende diagram toont hoe organisatie-eenheden zouden worden gebruikt voor het groeperen van verkoopmedewerkers op basis van het klanttype:

![](media/bhold-concepts-guide/org-chart-02.png)

In dit voorbeeld elke verkoop koppelen zou deel uitmaken van twee organisatie-eenheden: één voor het koppelen van plaats in de organisatie managementstructuur en één die aangeeft van het koppelen van eindklanten (retail- of bedrijfsnetwerk). Elke organisatie-eenheid kan worden toegewezen met verschillende rollen die op zijn beurt kunnen verschillende machtigingen voor toegang tot de organisatie worden toegewezen IT-resources. Rollen kunnen bovendien worden overgenomen van bovenliggende organisatie-eenheden, het proces van het doorgeven van rollen om gebruikers te vereenvoudigen. Aan de andere kant worden specifieke rollen voorkomen dat wordt overgenomen, ervoor te zorgen dat een specifieke rol wordt alleen gekoppeld aan de juiste organisatie-eenheden.

OrgUnits kunnen worden gemaakt in BHOLD-Suite met behulp van BHOLD-Core-web-portal of met behulp van de Generator van BHOLD-Model.

#### <a name="users"></a>Users

Zoals eerder vermeld, moet elke gebruiker moet behoren tot ten minste één organisatie-eenheid (OrgUnit). Omdat de organisatie-eenheden zijn de belangrijkste mechanisme voor het koppelen van een gebruiker met de rol, in de meeste organisaties behoort een bepaalde gebruiker tot meerdere OrgUnits zodat u gemakkelijk rollen koppelen aan die gebruiker. In sommige gevallen echter mogelijk een rol koppelen aan een gebruiker naast elke OrgUnits waartoe de gebruiker behoort. Als gevolg daarvan kan worden een gebruiker toegewezen rechtstreeks aan een rol, evenals het ophalen van functies van de OrgUnits waartoe de gebruiker behoort.

Wanneer een gebruiker is niet actief zijn binnen de organisatie (tijdens voor medische laat bijvoorbeeld), kan de gebruiker worden onderbroken, die trekt u machtigingen van de gebruiker zonder de gebruiker worden verwijderd uit het model van de rol. Bij naar recht wordt geretourneerd, de gebruiker kan opnieuw worden geactiveerd, waarmee u herstelt de machtigingen die zijn verleend door rollen van de gebruiker.

Objecten voor gebruikers kunnen afzonderlijk worden gemaakt in BHOLD via de webportal van BHOLD-Core, of ze kunnen worden geïmporteerd in één bulkbewerking met behulp van BHOLD-Generator voor Model, of met behulp van de Access Management-Connector met de FIM-synchronisatieservice voor het importeren van gebruikersgegevens uit bronnen zoals Active Directory Domain Services of HR-toepassingen.

Gebruikers kunnen rechtstreeks in BHOLD zonder gebruik van de FIM-synchronisatieservice worden gemaakt. Dit kan nuttig zijn bij het modelleren van rollen in een Pre-productie- of testomgeving. U kunt ook externe gebruikers (zoals de werknemers van een toeleverancier) worden toegewezen rollen en dus toegang krijgen tot IT-bronnen niet zijn toegevoegd aan de werknemersdatabase. Deze gebruikers wordt echter niet worden de BHOLD selfservice-functies kunnen gebruiken.

#### <a name="roles"></a>Rollen

Zoals eerder vermeld, onder de-op rollen gebaseerd model voor toegangsbeheer (RBAC), zijn machtigingen zijn gekoppeld aan rollen in plaats van afzonderlijke gebruikers. Dit maakt het mogelijk elke gebruiker de vereiste machtigingen voor het vervullen van de gebruiker door het wijzigen van de rollen van de gebruiker in plaats van afzonderlijk verlenen of weigeren van de gebruikersmachtigingen geven. Als gevolg hiervan de toewijzing van de machtigingen niet meer deelname van IT-afdeling vereist, maar in plaats daarvan kan worden uitgevoerd als onderdeel van het beheer van het bedrijf. Een rol kunt machtigingen voor toegang tot andere systemen, hetzij rechtstreeks, hetzij door het gebruik van subfuncties, waardoor de noodzaak van IT-rol bij het beheer van gebruikersmachtigingen samenvoegen.

Het is belangrijk te weten dat rollen een functie van de RBAC-model zelf zijn. rollen zijn doorgaans niet is ingericht voor doeltoepassingen. Deze kan RBAC moet worden gebruikt samen met bestaande toepassingen die niet zijn ontworpen om te gebruiken van rollen of te wijzigen van de rol van definities worden de behoeften van bedrijfsmodellen wijzigen zonder de toepassingen zelf wijzigen. Als een doeltoepassing is ontworpen voor gebruik van functies, klikt u vervolgens kunt u rollen in het model van de rol van BHOLD met bijbehorende rollen van de toepassing door de toepassingsspecifieke rollen te behandelen als machtigingen.

In BHOLD, kunt u een rol toewijzen aan een gebruiker hoofdzakelijk via twee mechanismen:

- Door een rol toewijzen aan een organisatie-eenheid (organisatie-eenheid) die de gebruiker lid is
- Een rol rechtstreeks aan een gebruiker toe te wijzen

Een rol die is toegewezen aan een bovenliggende organisatie-eenheid (optioneel) worden overgenomen door de organisatie-eenheden lid. Wanneer een rol is toegewezen aan of door een organisatie-eenheid overgenomen, worden deze als de rol van een doeltreffende of voorgestelde aangewezen. Als het een effectieve rol, worden alle gebruikers in de organisatie-eenheid, de rol toegewezen. Als het een voorgestelde rol, moet deze worden geactiveerd voor elke gebruiker of een lid organisatie-eenheid om te worden van kracht voor die gebruiker of leden van de organisatie-eenheid. Dit maakt het mogelijk een subset van de rollen die zijn gekoppeld aan een organisatie-eenheid, in plaats van alle functies van de organisatie-eenheid automatisch toewijzen aan alle leden van gebruikers toewijzen. Bovendien rollen kunnen de opgegeven begin- en einddatums en limieten voor het percentage van gebruikers binnen een organisatie-eenheid waarvoor een rol van kracht worden kan kunnen worden geplaatst.

Het volgende diagram illustreert hoe een afzonderlijke gebruiker kan worden toegewezen een rol in BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

In dit diagram rol A is toegewezen aan een organisatie-eenheid als een overneembare rol, en dus wordt overgenomen door de organisatie-eenheden van het lid en alle gebruikers binnen de organisatie-eenheden. Rol B is toegewezen als een voorgestelde rol voor een organisatie-eenheid. Deze moet worden geactiveerd voordat een gebruiker in de organisatie-eenheid kan worden geautoriseerd met machtigingen van de rol. Rol C is de rol van een doeltreffende, zodat de machtigingen ervan direct op alle gebruikers in de organisatie-eenheid toegepast. Rol D rechtstreeks naar de gebruiker is gekoppeld en dus de machtigingen ervan direct toegepast op die gebruiker.

Bovendien kan een rol worden geactiveerd voor een gebruiker op basis van kenmerken van een gebruiker. Zie voor meer informatie op basis van claims-kenmerk.

#### <a name="permissions"></a>Machtigingen

Een machtiging in BHOLD komt overeen met een machtiging die is geïmporteerd uit een doeltoepassing. Dat wil zeggen, wanneer BHOLD is geconfigureerd om te werken met een toepassing, ontvangt deze een lijst met machtigingen die BHOLD aan rollen koppelen kunt. Bijvoorbeeld, wanneer Active Directory Domain Services (AD DS) is toegevoegd aan BHOLD als een toepassing, ontvangt deze een lijst met beveiligingsgroepen die als BHOLD-machtigingen kan worden gekoppeld aan rollen in BHOLD.

Machtigingen zijn specifiek voor toepassingen. BHOLD biedt een geïntegreerde weergave van machtigingen zodat machtigingen gekoppeld aan rollen worden kunnen zonder rol managers om details over de implementatie van de machtigingen te begrijpen. In de praktijk kunnen verschillende systemen afdwingen voor een machtiging anders. De connector toepassingsspecifieke via de FIM-synchronisatieservice naar de toepassing bepaalt hoe wijzigingen in de machtigingen voor een gebruiker worden geleverd bij die toepassing. 

#### <a name="applications"></a>Toepassingen

BHOLD implementeert een methode voor het op rollen gebaseerd toegangsbeheer (RBAC) toepassen op externe toepassingen. Dat wil zeggen, wanneer BHOLD is ingericht met gebruikers en machtigingen van een toepassing, kunt BHOLD koppelen deze machtigingen aan gebruikers door rollen toewijzen aan de gebruikers en vervolgens de machtigingen te koppelen aan de rollen. Proces van de achtergrond van de toepassing kunt vervolgens de juiste machtigingen toewijzen aan de gebruikers op basis van de toewijzing van de rol/machtiging in BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Het ontwikkelen van het model van de rol van BHOLD-Suite

BHOLD-Suite biedt voor hulp bij het ontwikkelen van een rol model, Model Generator, een hulpprogramma dat is eenvoudig te gebruiken en uitgebreid.

Voordat u de Model-Generator gebruikt, moet u een reeks van bestanden die de objecten die Model Generator gebruikt definieert voor het maken van het model van de functie maken. Zie Microsoft BHOLD-Suite technische documentatie voor meer informatie over het maken van deze bestanden.

De eerste stap bij het gebruik van de Generator van BHOLD-Model is om deze bestanden om de elementaire bouwstenen laden in Model Generator te importeren. Wanneer de bestanden zijn geladen, kunt u de criteria die Model Generator gebruikt voor het maken van verschillende klassen van de rollen vervolgens opgeven:

- Van lidmaatschapsfuncties die zijn toegewezen aan een gebruiker op basis van de OrgUnits (organisatie-eenheden) die de gebruiker behoort
- Kenmerk-functies die zijn toegewezen aan een gebruiker op basis van de kenmerken van de gebruiker in de BHOLD-database
- Voorgestelde functies die zijn gekoppeld aan een organisatie-eenheid, maar moeten worden geactiveerd voor specifieke gebruikers
- Eigendom van functies die een controle van de gebruiker over de organisatie-eenheden en functies waarvoor een eigenaar niet is opgegeven in de geïmporteerde bestanden verlenen

> [!Important]
> Tijdens het uploaden van bestanden, selecteer de **bestaande Model behouden** selectievakje alleen in een testomgeving. In een productieomgeving, moet u Model Generator gebruiken om de eerste rol-model te maken. U kunt deze niet gebruiken voor het wijzigen van het model van een bestaande functie in de BHOLD-database.

Nadat deze rollen Model Generator in het model van de functie gemaakt, kunt u het model van de functie vervolgens exporteren naar de BHOLD-database in de vorm van een XML-bestand.

### <a name="advanced-bhold-features"></a>Geavanceerde functies van BHOLD

Vorige secties beschreven de belangrijkste functies van op rollen gebaseerd toegangsbeheer (RBAC) in BHOLD. In deze sectie geeft een overzicht van aanvullende functies in BHOLD die verbeterde beveiliging en flexibiliteit van uw organisatie uitvoering van RBAC biedt. In deze sectie vindt u overzichten van de volgende BHOLD-functies:

- de kardinaliteit
- Scheiding van functies
- Context flexibel machtigingen
- Op kenmerken gebaseerde autorisatie
- Flexibele kenmerktypen


#### <a name="cardinality"></a>de kardinaliteit

*De kardinaliteit* verwijst naar de implementatie van bedrijfsregels die zijn ontworpen om te beperken van het aantal keren dat twee entiteiten kunnen zijn gerelateerd aan elkaar worden verbonden. In het geval van BHOLD, kunnen de kardinaliteit van regels voor rollen en machtigingen voor gebruikers worden gemaakt.

U kunt een rol om te beperken het volgende configureren:

- Het maximum aantal gebruikers waarvoor een voorgestelde rol kan worden geactiveerd
- Het maximum aantal subfuncties die kunnen worden gekoppeld aan de rol
- Het maximum aantal machtigingen die kunnen worden gekoppeld aan de rol

U kunt een machtiging voor het beperken van de volgende configureren:

- Het maximum aantal rollen die kunnen worden gekoppeld aan de machtiging
- Het maximum aantal gebruikers die de machtiging kunnen worden verleend

U kunt een gebruiker om te beperken het volgende configureren:

- Het maximum aantal rollen die kunnen worden gekoppeld aan de gebruiker
- Het maximum aantal machtigingen die kunnen worden toegewezen aan de gebruiker door de roltoewijzingen

#### <a name="separation-of-duties"></a>Scheiding van functies

Scheiding van functies (beplanten) is een business-principe waarmee wordt geprobeerd om te voorkomen dat gebruikers de mogelijkheid voor het uitvoeren van acties die niet beschikbaar is voor één persoon toegang. Een werknemer moet bijvoorbeeld niet kunnen aanvragen van een betaling en autorisatie van de betaling. Het principe van beplanten kan organisaties voor het implementeren van een systeem van controle- en balansniveau hun blootstelling aan het risico van werknemer fout of fouten minimaliseren.

BHOLD implementeert beplanten doordat u kunt niet-compatibele machtigingen definiëren. Wanneer deze machtigingen zijn gedefinieerd, BHOLD beplanten afgedwongen door te voorkomen dat het maken van de rollen die zijn gekoppeld aan machtigingen voor een niet-compatibele, ongeacht of ze rechtstreeks zijn gekoppeld of via overname, en door te voorkomen dat gebruikers uit te maken van meerdere rollen dat toegewezen bij gecombineerd, zou niet-compatibele machtigingen verlenen opnieuw door directe toewijzing of door overname. (Optioneel) kunnen veroorzaakt een conflict worden genegeerd.

#### <a name="context-adaptable-permissions"></a>Context flexibel machtigingen

Als u machtigingen die worden automatisch op basis van een objectkenmerk aangepast kunnen maakt, kunt u het totale aantal machtigingen die u hebt voor het beheren van verminderen. Context flexibel machtigingen (CAP's) kunnen u een formule definiëren als een machtiging-kenmerk dat Hiermee wijzigt u hoe de machtiging wordt toegepast door de toepassing die is gekoppeld met de machtiging. Bijvoorbeeld, kunt u een formule die wijzigt de toegangsmachtigingen voor een map (door middel van een beveiligingsgroep die is gekoppeld aan de toegangsbeheerlijst van de map) op basis van of een gebruiker deel uitmaakt van een organisatie-eenheid (organisatie-eenheid) met permanente of werknemers-contract. Als de gebruiker van een organisatie-eenheid wordt verplaatst naar een andere, de nieuwe machtigingenset wordt automatisch toegepast en de oude machtiging is gedeactiveerd. 

De CAP-formule kunt de waarden van kenmerken die zijn toegepast voor toepassingen, machtigingen, organisatie-eenheden en gebruikers van een query.

#### <a name="attribute-based-authorization"></a>Op kenmerken gebaseerde autorisatie

Een manier om te bepalen of een rol die is gekoppeld aan een organisatie-eenheid (organisatie-eenheid) wordt geactiveerd voor een bepaalde gebruiker in de organisatie-eenheid is het gebruik van op kenmerken gebaseerde autorisatie (ABA). U kunt automatisch een rol activeren met behulp van ABA, alleen wanneer aan bepaalde regels op basis van kenmerken van een gebruiker wordt voldaan. U kunt bijvoorbeeld een rol koppelen aan een organisatie-eenheid die actief zijn voor een gebruiker wordt alleen als de functie van de gebruiker overeenkomt met de functie in de regel ABA. Dit elimineert de noodzaak een voorgestelde rol voor een gebruiker handmatig activeren. In plaats daarvan kan een rol worden geactiveerd voor alle gebruikers in een organisatie-eenheid die beschikken over een kenmerkwaarde die voldoet aan van de rol ABA-regel. Regels kunnen worden gecombineerd, zodat een rol wordt alleen geactiveerd als de kenmerken van een gebruiker voldoet aan alle ABA-regels die worden opgegeven voor de rol.

Het is belangrijk te weten dat de resultaten van ABA regel tests worden beperkt door de van Kardinaliteitsinstellingen. Als de kardinaliteitsinstelling van de van een regel bepaalt niet meer dan twee gebruikers een rol kunnen worden toegewezen, en als een regel voor ABA zouden anders een rol voor vier gebruikers activeert, wordt bijvoorbeeld de rol geactiveerd alleen voor de eerste twee gebruikers die de ABA-test slaagt.

#### <a name="flexible-attribute-types"></a>Flexibele kenmerktypen

Het systeem van kenmerken in BHOLD is maximaal worden uitgebreid. U kunt nieuwe kenmerktypen voor dergelijke objecten definiëren als gebruikers, organisatie-eenheden (organisatie-eenheden) en rollen. Kenmerken kunnen worden gedefinieerd als u wilt dat de waarden die gehele getallen, Booleaanse waarde zijn (Ja/Nee), uit alfanumerieke tekens, datum, tijd en e-mailadressen. Kenmerken kunnen worden opgegeven als enkele waarden of een lijst met waarden.

## <a name="attestation"></a>Attestation

De BHOLD-Suite voorziet in hulpprogramma's die u gebruiken kunt om te controleren dat afzonderlijke gebruikers gemachtigd om uit te voeren hun zakelijke taken hebt ontvangen. De beheerder kan de opgegeven door de module BHOLD-Attestation-portal gebruiken voor het ontwerpen van een beheren het attestation-proces.

Het attestation-proces wordt uitgevoerd met behulp van campagnes in welke campagne stewards de gelegenheid krijgt en betekent om te controleren dat de gebruikers waarvoor ze verantwoordelijk zijn juiste toegang hebben tot BHOLD-beheerde toepassingen en de juiste machtigingen in deze toepassingen. De eigenaar van een campagne is aangewezen om te overzien van de campagne en om ervoor te zorgen dat de campagne correct wordt verricht. Een campagne kan worden gemaakt voor het eenmalig of volgens een terugkerend basis optreden.

De steward voor een campagne worden normaal gesproken een manager die wordt bevestigen voor de toegangsrechten van gebruikers die behoren tot een of meer organisatie-eenheden waarvoor de manager verantwoordelijk is. Stewards automatisch kan worden geselecteerd voor de gebruikers in een campagne op basis van gebruikerskenmerken wordt beoordeeld of de stewards voor een campagne kunnen worden gedefinieerd door ze in een bestand dat elke gebruiker in de campagne wordt beoordeeld op een steward-kaarten weer te geven.

Wanneer een campagne begint, wordt BHOLD verzendt een e-mailbericht naar de campagne stewards en de eigenaar en vervolgens verzendt periodieke herinneringen voor het onderhouden van de voortgang van de campagne. Stewards worden doorgestuurd naar een campagne-portal waar ze een lijst van de gebruikers waarvoor ze verantwoordelijk zijn en de functies die zijn toegewezen aan deze gebruikers kunnen zien. De stewards kunnen vervolgens controleren of ze verantwoordelijk voor elk van de vermelde gebruikers zijn en goedkeuren of weigeren van de toegangsrechten van elk van de vermelde gebruikers.

De portal campagne eigenaren ook gebruiken om de voortgang van de campagne te controleren en campagneactiviteiten worden vastgelegd zodat eigenaars van de campagne de acties die zijn uitgevoerd in de loop van de campagne kunnen analyseren.

## <a name="analytics"></a>Analytics

Een van de belangrijke overwegingen bij het implementeren van een uitgebreide toegang op basis van rechten (RBAC) besturingssysteem is de balans tussen strikt toegangsbeheer beheren en voorkomen van onnodige (of, slechter, onverwachte) barrières voor toegang tot. De inspanningen om het saldo vaak onderhouden resulteert in een access-control-structuur die zo complex is of onverwachte interacties tussen beleidsregels zijn bijna onvermijdelijk.

Om die reden is het belangrijk dat u voor het analyseren van de gevolgen van de access-control-beleid voordat ze daadwerkelijk erin worden geplaatst. De Analytics-module van BHOLD-Suite biedt u de mogelijkheid om uit te voeren van deze analyse door te laten u bij ontwikkelen van regels die staan voor uw beleid en vervolgens weer van de gebruikers waarvan de machtigingen voldoen of veroorzaken een conflict met de regel. Op basis van deze analyse, kunt u het beleid aanpassen of rollen en machtigingen om te voorkomen van conflicten met het beleid wijzigen.

De portal voor BHOLD-analyse biedt u de mogelijkheid om samen te stellen rulesets die bestaan uit een of meer regels die u maakt als u wilt testen van een bepaald beleid of de groep van het beleid. Een regel bestaat uit de volgende belangrijke onderdelen:

- Een titel en beschrijving waarmee u kunt identificeren en op de regel beschrijven
- Een status die aangeeft of de regel gereed voor controle is, wordt gecontroleerd of goedgekeurd
- Een element ingesteld (zoals gebruikers of machtigingen) die de regel is ontworpen om te testen
- Optionele subset filters die expressies die u gebruiken kunt om te selecteren van een juiste subgroep van het element dat zijn moet worden onderzocht
- Een of meer regel filters die zijn expressies die staan voor het beleid wordt getest.

Een regel kunt een van de volgende sets van element testen:

- Users
- Organisatie-eenheden
- Rollen
- Machtigingen
- Toepassingen
- Accounts

Het volgende diagram ziet u een eenvoudige regel bestaat uit twee subset regels en twee regels:

![](media/bhold-concepts-guide/rules.png)

Let op het verschil in het effect van een subsetfilter mislukt en werkt het uitvoeren van een regel filter: werkt het uitvoeren van een subsetfilter een elementobject verwijdert uit het testen door de volgende filters, terwijl werkt het uitvoeren van een filter regel zorgt ervoor dat het object worden gerapporteerd als niet compatibel zijn. Alleen de objecten die de subset filters en alle filters van de regel doorgeven worden als compatibel gerapporteerd.

Elk filter bestaat uit een type, een operator (dit is afhankelijke van type), een sleutel (een van de elementen) en een waarde op basis waarvan de sleutel wordt getest door de operator. Het volgende filter zou bijvoorbeeld controleren of het aantal gebruikers in een subset element groter is dan 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Type:**   | Aantal   |
| **Sleutel:**  | Users  |
| **Operator**  | >  |
| **Waarde:** | 10 |

De filters regels kunnen worden van de drie typen en gebruik operators die specifiek zijn voor hun type, zoals wordt aangegeven:

- Kenmerk
  - < en >
  - = en! =
  - **bevat**
  - **Bevat niet**
- Aantal
  - < en >
  - = en! =
- Beperkende
  - **Moet bevatten en moet beschikken over al**
  - **Geen en kan niet alle**
  - **Kan alleen een en alleen voor alle hebben kan**
  - **Exclusief alle en uitsluitend hebben alle**

> [!Note]
> Beperkende filters kunnen u de aangegeven operators gebruiken voor het testen van een sleutel aan de hand van meerdere waarden.

Als u testen van de implementatie van een scheiding van taken (beplanten)-beleid met de mededeling wilt dat er geen gebruiker die de aanvraag betaling machtiging heeft ook goedkeuren betaling toestemming hebben, kan u bijvoorbeeld een regel als volgt maken:

|   |  |
|---|--|
|Naam:| Betaling beplanten testen|
|Element:| Users|
|Subsetfilter:| Een andere machtiging betaling aanvragen hebben|
|Filter in de regel: | Kan niet alle machtigingen goedkeuren betaling hebben|

Wanneer u deze regel wordt uitgevoerd, wordt de BHOLD-analyse-module weergegeven van het aantal gebruikers in de geselecteerde subset (het aantal gebruikers met de machtiging voor betaling aanvragen), het aantal gebruikers die aan de regel voldoen en het aantal gebruikers die niet aan de regel voldoen. U kunt vervolgens de niet-compatibele gebruikers weergeven, zodat u kunt de inconsistentie oplossen.

Naast de resultaten weer te geven, kunt u het rapport ook opslaan als een door komma's gescheiden waarden (CSV) of een XML-bestand waarmee u kunt de resultaten later analyseren. U kunt ook de resulterende lijst wilt weergeven van extra informatie waarmee u een beter begrip van de impact kunt aanpassen. Bijvoorbeeld, als u gebruikers testen wilt, kunt u weergegeven (of rapport) de accounts van de compatibele of niet-compatibele gebruikers, zodat u kunt zien welke toepassingen betrokken zijn.

Omdat een regel meerdere filters bevatten kan, kunt u filters om te testen of een combinatie van voorwaarden bestaan. Het resultaat is het product van een test en Booleaanse van alle filters, maar u kunt opgeven dat een test of van de combinatie van filter worden uitgevoerd.

Bijvoorbeeld, als uw bedrijfsbeleid managers de machtiging betalingsmethode wijzigen of de machtiging goedkeuren betaling vereist, kan u test voldoet aan het beleid door een regel als volgt uit:


|  |  |
|--|--|
|Naam: | Betaling beplanten Test wijzigen|
|Element: | Users |
|Subsetfilter: | Een rol Manager hebben|
| Regel filters: |Moet een andere machtiging betalingsmethode wijzigen </br> Een andere machtiging goedkeuren betaling moet hebben|

Standaard worden elke gebruiker die een manager die de betalingsmethode wijzigen de machtiging betaling aanvragen en gerapporteerd als compatibel. Het beleid is echter vereist dat een manager beschikken over een machtiging niet per se beide. Als u wilt testen werkelijke voldoet aan het beleid, moet u de of Booleaanse operator met de regel gebruiken om te bepalen of er geen beheerders die niet beschikken over de machtiging van de betalingsmethode wijzigen of de machtiging goedkeuren betaling.

In tegenstelling tot andere operators, de **uitsluitend hebt** en de **uitsluitend beschikt over alle** operators wijzen op naleving voor objecten die anders zouden worden uitgesloten door een subsetfilter. Als u bijvoorbeeld voor het testen van een beleid dat alle beheerders (en alleen beheerders) de machtiging goedkeuren beoordelingen hebben, kunt u een regel als volgt:

|  |  |
|--|--|
|Naam: | Beoordeling goedkeuring testen|
|Element: | Users|
| Subsetfilter: | Een rol Manager hebben
|Filter in de regel: | Exclusief hebt geen machtiging goedkeuren beoordelingen|

Deze regel wordt als compatibel gebruikers die beheerders zijn en de machtiging goedkeuren beoordelingen en gebruikers die zijn niet managers en bij wie niet de machtiging goedkeuren beoordelingen zijn. Daarentegen worden managers die niet over deze machtiging en de gebruikers die geen managers, maar die gemachtigd gerapporteerd als niet compatibel zijn.

Zoals eerder is aangegeven, kunt u regels combineren in een ruleset, waardoor het gemakkelijker voor u om te categoriseren en regels om te voldoen aan uw zakelijke vereisten beheren is.

U kunt ook een set algemene filters die, wanneer dit is ingeschakeld, zijn van toepassing op elke regel die wordt getest. Als u vaak uitsluiten van een bepaalde subset met records wilt bij het testen van de regels in verschillende rulesets, kunt u algemene filters die u kunt inschakelen of uitschakelen, indien nodig.

## <a name="reporting"></a>Rapporten

De rapportage van BHOLD-module biedt u de mogelijkheid om informatie in het model van de functie via een groot aantal rapporten weer te geven. De rapportage van BHOLD-module biedt een uitgebreide set met ingebouwde rapporten, plus het bevat een wizard waarmee u gebruiken kunt om eenvoudige en geavanceerde aangepaste rapporten te maken. Wanneer u een rapport hebt uitgevoerd, kunt u onmiddellijk de resultaten weer te geven of de resultaten opslaan in een Microsoft Excel (.xlsx)-bestand. Als u wilt weergeven van dit bestand met behulp van Microsoft Excel 2000, Microsoft Excel 2002 of Microsoft Excel 2003, kunt u downloaden en installeren van de Microsoft Office-compatibiliteitspakket voor Word, Excel en PowerPoint-bestandsindelingen.


Kunt u de rapportage van BHOLD-module voornamelijk produceren van rapporten die zijn gebaseerd op de huidige rol informatie. Gegenereerd rapporten voor controle wijzigingen in gegevens van identiteit, heeft Forefront Identity Manager 2010 R2 een rapportagefunctionaliteit voor de FIM-Service die in de System Center Service Manager-datawarehouse is geïmplementeerd. Zie voor meer informatie over FIM reporting, de Forefront Identity Manager 2010 en Forefront Identity Manager 2010 R2-documentatie in de technische bibliotheek voor Forefront Identity Management.

Categorieën wordt gedekt door de ingebouwde rapporten omvatten het volgende:

- Administration
- Attestation
- besturingselementen
- Inkomende Access Control
- Logboekregistratie
- Model
- statistieken
- Werkstroom

U kunt rapporten maken en toe te voegen aan deze categorieën, of kunt u uw eigen categorieën waarin u aangepaste en ingebouwde rapporten kunt plaatsen.

Als u een rapport maken, doorloopt de wizard u de volgende parameters opgeven:

- Identificatie-informatie, inclusief de naam, beschrijving, categorie, gebruik en doelgroep
- Velden die moeten worden weergegeven in het rapport
- Query's die welke items opgeven moeten worden geanalyseerd
- Volgorde waarin rijen worden gesorteerd
- Velden die worden gebruikt voor het rapport in de secties opsplitsen
- Filters te verfijnen van de elementen die worden geretourneerd in het rapport

In elke fase van de wizard, kunt u het rapport bekijken als u deze tot nu toe hebt gedefinieerd en deze opslaan als u geen hoeft aanvullende parameters opgeeft. U kunt ook terug verplaatsen naar de vorige stappen om parameters die u eerder in de wizard hebt opgegeven te wijzigen.

## <a name="access-management-connector"></a>Access Management-Connector

De module BHOLD-Suite Access Management-Connector biedt ondersteuning voor initiële en lopende synchronisatie van gegevens in BHOLD. De Access Management-Connector werkt met de FIM-synchronisatieservice om gegevens tussen de kern van BHOLD-database, de MIM-metaverse en doeltoepassingen en identity stores te verplaatsen.

Eerdere versies van BHOLD vereist meerdere MAs voor het beheren van gegevensoverdracht tussen MIM en tussenliggende BHOLD-databasetabellen. In de SP1 BHOLD-Suite kunt de Access Management-Connector u beheeragents (MAs) definiëren in MIM waarmee de overdracht van gegevens tussen BHOLD en MIM.

## <a name="mim-integration"></a>MIM-integratie

Een belangrijk en krachtige functie van Forefront Identity Manager 2010 en Forefront Identity Manager 2010 R2 is de self-service portal waarmee eindgebruikers kunnen hun informatie-id en het lidmaatschap weergeven en beheren. De MIM-Portal met de selfservicerol beheer een uitbreiding voor MIM-integratie. Bijvoorbeeld met behulp van de BHOLD-functies in de MIM-Portal, een gebruiker kan de roltoewijzing aanvragen en actieve rollen kunt weergeven en aanvragen in behandeling. Aanvullende mogelijkheden kunnen worden verleend aan gedelegeerde beheerders, zoals de mogelijkheid om de roltoewijzing van de aanvraag voor andere gebruikers.

Het is belangrijk te weten dat de BHOLD-extensies voor de MIM-Portal selfservicerol en werkstroombeheer ondersteunen en rapportage. Andere functies van BHOLD-beheer, evenals de attestation, worden geleverd door de webportals van de BHOLD-modules, die afzonderlijk worden gehost in de MIM-Portal.

## <a name="next-steps"></a>Volgende stappen

- [Handleiding voor BHOLD-installatie](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
