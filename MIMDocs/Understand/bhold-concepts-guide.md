---
title: Microsoft BHOLD-Suite concepten handleiding | Microsoft Docs
description: Ga aan de slag met de MIM 2016-onderdelen en installeer en configureer de synchronisatieservice.
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/06/2017
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 0db8c2ebaee204c929dc7345ac6858fff6b0993b
ms.sourcegitcommit: f29f02fa8437fa55e86afd7b0b99a36d2306b96b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/08/2017
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Microsoft BHOLD-Suite concepten handleiding

Microsoft Identity Manager 2016 (MIM) kunnen organisaties voor het beheren van de hele levenscyclus van de identiteit van gebruikers en hun bijbehorende referenties. Deze kan worden geconfigureerd voor het synchroniseren van identiteiten, centraal certificaten en wachtwoorden beheren en gebruikers hebt ingericht in heterogene systemen. Met MIM, kunnen IT-organisaties definiëren en de processen voor het beheren van identiteiten van maken naar buiten gebruik stellen automatiseren.

Deze mogelijkheden van MIM breidt BHOLD-Suite van Microsoft door toegangsbeheer op basis van rollen toe te voegen. BHOLD kan organisaties gebruikersrollen definiëren en beheren van toegang tot gevoelige gegevens en toepassingen. De toegang is op basis van wat geschikt is voor deze rollen. BHOLD-Suite omvat de services en hulpprogramma's de modellering van de rolrelaties binnen de organisatie vereenvoudigen. BHOLD wijst deze rollen rechten, en om te controleren of de definities van gebruikersrollen en de gekoppelde rechten correct worden toegepast op gebruikers. Deze mogelijkheden zijn volledig geïntegreerd met MIM, biedt een naadloze ervaring voor eindgebruikers en IT-personeel die met.

In deze handleiding vindt u informatie over de werking van BHOLD-Suite met MIM en de volgende onderwerpen behandeld:

- Op rollen gebaseerd toegangsbeheer
- Attestation
- Analytics
- Rapporten
- Access Management-Connector
- MIM-integratie

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

De meest voorkomende methode voor het beheren van de gebruikerstoegang tot gegevens en toepassingen is via discretionary access control (DAC). In de meest voorkomende implementaties heeft elke aanzienlijke object een geïdentificeerde eigenaar. De eigenaar heeft de mogelijkheid om toegang te verlenen of weigeren van toegang tot het object naar anderen op basis van afzonderlijke identiteit of groepslidmaatschap. In de praktijk meestal DAC resulteert in een breed spectrum van beveiligingsgroepen, sommige die overeenkomen met organisatiestructuur anderen die functionele groeperingen (zoals taaktypen of projecttoewijzingen vertegenwoordigen) en anderen die bestaan uit makeshift verzamelingen van gebruikers en apparaten die zijn gekoppeld aan meer tijdelijke doeleinden. Omdat organisaties toenemen, is lidmaatschap van deze groepen wordt steeds moeilijker te beheren. Bijvoorbeeld, als een werknemer is opgehaald uit een project naar een andere, moeten de groepen die worden gebruikt voor toegang tot de activa projecten handmatig worden bijgewerkt. In dergelijke gevallen is het niet ongewoon is voor fouten optreden, fouten kunnen project beveiligings- of productiviteit belemmeren.

MIM bevat functies waarmee u dit probleem verhelpen door geautomatiseerde controle over lidmaatschap van een groep en distribueren. Dit kan echter niet de intrinsieke complexiteit van delende groepen die niet noodzakelijkerwijs gerelateerd zijn aan elkaar gestructureerd omgaan.

Een manier om deze komst significant verminderen is door het implementeren van op rollen gebaseerde toegangsbeheer (RBAC). RBAC vervangt DAC komt niet.  RBAC is gebaseerd op DAC doordat een framework voor het classificeren van gebruikers en IT-bronnen. Hiermee kunt u expliciete maken hun relatie en de toegangsrechten die geschikt zijn afhankelijk van deze classificatie zijn. Bijvoorbeeld door toe te wijzen aan een gebruiker de functie van de kenmerken die de gebruikers opgeven en projecttoewijzingen, de gebruiker toegang tot programma's die nodig zijn voor de taak en de gegevens die de gebruiker moet bijdragen aan een bepaald project van de gebruiker kunnen worden verleend. Wanneer de gebruiker wordt ervan uitgegaan dat een andere taak en andere projecttoewijzingen wijzigen van de kenmerken die de functie van de gebruiker opgeeft en projecten automatisch toegang tot de resources die zijn alleen vereist voor de vorige positie van de gebruikers worden geblokkeerd.

Omdat de rollen kunnen worden opgenomen in de rollen in een hiërarchische (bijvoorbeeld de rollen van Verkoopmanager en verkoopmedewerker kunnen worden opgenomen in de algemene functie van de verkoop), is het eenvoudig om juiste rechten toewijzen aan specifieke rollen en nog steeds te bieden juiste rechten voor iedereen die ook de ruimere rol. Bijvoorbeeld in een ziekenhuis alle medische medewerkers kunnen worden verleend om een patiënten records te bekijken, maar alleen artsen (een subfunctie van de rol medische) kunnen worden verleend voorschriften voor de geduld invoeren. Gebruikers die horen bij de administratieve rol kunnen op dezelfde manier de geen toegang tot patiëntrecords, met uitzondering van facturering clerks (een subfunctie van de administratieve rol), die toegang tot deze delen van een patiënten records die nodig zijn voor de geduld voor services in rekening kunnen worden verleend geleverd door het ziekenhuis.

Een bijkomend voordeel van RBAC is de mogelijkheid om te definiëren en afdwingen van scheiding van functies (beplanten). Dit kan een organisatie voor het definiëren van combinaties van functies voor het verlenen van machtigingen die niet door dezelfde gebruiker moeten worden bewaard, zodat de functies waarmee de gebruiker een betaling te initiëren en machtigen van een betaling, bijvoorbeeld door een bepaalde gebruiker kan niet worden toegewezen. RBAC biedt de mogelijkheid om af te dwingen dergelijk beleid automatisch plaats om te evalueren van de effectieve implementatie van het beleid op basis van een gebruiker.

### <a name="bhold-role-model-objects"></a>BHOLD-modelobjecten rol

U kunt opgeven en functies binnen uw organisatie, de gebruikers toewijzen aan rollen en de juiste machtigingen toewijzen aan rollen te organiseren met BHOLD-Suite. Deze structuur is een functie-model genoemd, en deze bevat en verbindt u vijf typen objecten: 

- Organisatie-eenheden
- Users
- Rollen
- Machtigingen
- Toepassingen

#### <a name="organizational-units"></a>Organisatie-eenheden

Organisatie-eenheden (OrgUnits) zijn de belangrijkste middelen waarmee gebruikers zijn ingedeeld in het model van de BHOLD-functie. Elke gebruiker moet behoren tot ten minste één OrgUnit. (Zelfs wanneer een gebruiker wordt verwijderd uit de laatste organisatie-eenheid in BHOLD, record van de gegevens van de gebruiker wordt verwijderd uit de BHOLD-database.)

>[!Important]
Organisatie-eenheden in het model van de rol BHOLD mag geen verward met organisatie-eenheden in Active Directory Domain Services (AD DS). Normaal gesproken is de structuur van de organisatie-eenheid in BHOLD gebaseerd op de organisatie en het beleid van uw bedrijf, niet de vereisten van uw netwerkinfrastructuur.

Hoewel dit niet vereist is, in de meeste gevallen zijn organisatie-eenheden geordend in BHOLD vertegenwoordigt de hiërarchische structuur van de werkelijke organisatie, zoals hieronder:

![](media/bhold-concepts-guide/org-chart.png)

In dit voorbeeld zou het model van de functie organizationalganizatinal-eenheid voor het bedrijf als geheel (vertegenwoordigd door de directeur, omdat de directeur geen deel uit van een eenheid mororganizationalganizatinal maakt) of de organisatie-eenheid van BHOLD-hoofdmap (die altijd aanwezig) kan worden gebruikt voor dit doel. OrgUnits de zakelijke afdelingen leiding van de voorzitters vice die zouden worden geplaatst in het bedrijf, organisatie-eenheid. Volgende, organisatie-eenheden die overeenkomt met de marketing- en bestuur zou worden toegevoegd aan de marketing- en organisatie-eenheden en organisatie-eenheden voor de regionale verkoopmanagers zouden worden geplaatst in de organisatie-eenheid voor de Oost regio Verkoopmanager. De verkoopmedewerkers, die andere gebruikers niet beheert, zou leden van de organisatie-eenheid van de verkoopmanager van Oost regio worden gemaakt. Houd er rekening mee dat gebruikers kunnen leden van een organisatie-eenheid op elk niveau. Bijvoorbeeld, een administratief medewerker die zich niet in een manager en rapporten rechtstreeks naar een onderdirecteur, zou een lid zijn van de vice-directeur organisatie-eenheid.

Naast de organisatiestructuur voor, worden organisatie-eenheden ook gebruikt om gebruikers en de andere organisatie-eenheden volgens functionele criteria zoals groepen voor projecten of specialisatie. Het volgende diagram toont hoe de organisatie-eenheden moeten worden gebruikt voor het groeperen van verkoopmedewerkers volgens het klanttype:

![](media/bhold-concepts-guide/org-chart-02.png)

In dit voorbeeld wordt elke verkoop koppelen aan twee organisatie-eenheden zouden behoren: één voor het koppelen plaats in de organisatie beleidsstructuur en één voor het koppelen van de klant base (retail- of bedrijfsnetwerk). Elke organisatie-eenheid verschillende rollen die op zijn beurt kunnen andere machtigingen voor toegang tot de organisatie worden toegewezen kan worden toegewezen IT-bronnen. Bovendien kunnen rollen worden overgenomen van bovenliggend organisatie-eenheden, het proces van het doorgeven van rollen om gebruikers te vereenvoudigen. Aan de andere kant worden specifieke rollen voorkomen dat wordt overgenomen, ervoor te zorgen dat een specifieke rol wordt alleen gekoppeld met de juiste organisatie-eenheden.

OrgUnits kunnen worden gemaakt in BHOLD-Suite met behulp van de Core BHOLD-web-portal of met behulp van de Generator BHOLD-Model.

#### <a name="users"></a>Users

Zoals eerder vermeld, moet elke gebruiker behoren tot ten minste één organisatie-eenheid (OrgUnit). Omdat de organisatie-eenheden zijn de belangrijkste mechanisme voor het koppelen van een gebruiker met de rol, in de meeste organisaties behoort een opgegeven gebruiker tot meerdere OrgUnits gemakkelijker functies koppelen aan deze gebruiker. In sommige gevallen kan echter het mogelijk nodig voor een rol koppelen aan een gebruiker naast elke OrgUnits die de gebruiker behoort. Hierdoor kan worden een gebruiker toegewezen rechtstreeks aan een rol, evenals het installeren van functies van de OrgUnits die de gebruiker behoort.

Wanneer een gebruiker is niet actief zijn binnen de organisatie (even weg medische laat bijvoorbeeld), kan de gebruiker worden onderbroken, die alle machtigingen van de gebruiker zonder dat de gebruiker verwijderd uit het model van de functie trekt. Bij het naar het recht wordt geretourneerd, de gebruiker kan opnieuw worden geactiveerd, waarmee u herstelt de machtigingen die zijn verleend door rollen van de gebruiker.

Objecten voor gebruikers kunnen afzonderlijk worden gemaakt in BHOLD via de webportal van BHOLD-Core, of ze kunnen worden geïmporteerd in bulk met behulp van BHOLD Model Generator of door de Access Management-Connector voor het importeren van gebruikersgegevens uit met de FIM-synchronisatieservice bronnen zoals Active Directory Domain Services of human resources-toepassingen.

Gebruikers kunnen rechtstreeks in BHOLD zonder gebruik van de FIM-synchronisatieservice worden gemaakt. Dit kan nuttig zijn wanneer het modelleren van rollen in een preproductieverzameling of testomgeving. U kunt ook toestaan dat externe gebruikers (zoals werknemers van een uitbesteding) worden toegewezen rollen en dus toegang te krijgen tot de IT-bronnen niet zijn toegevoegd aan de werknemersdatabase. Deze gebruikers wordt echter niet worden de BHOLD selfservice-functies kunnen gebruiken.

#### <a name="roles"></a>Rollen

Zoals eerder is vermeld, onder de toegang op basis van rollen-model voor toegangsbeheer (RBAC), zijn machtigingen zijn gekoppeld aan rollen in plaats van afzonderlijke gebruikers. Hierdoor kan elke gebruiker de vereiste machtigingen voor het vervullen van de gebruiker door het wijzigen van de rollen van de gebruiker in plaats van afzonderlijk verlenen of weigeren van de gebruikersmachtigingen geven. Als gevolg hiervan de toewijzing van de machtigingen van IT-afdeling deelname niet langer vereist, maar in plaats daarvan kan worden uitgevoerd als onderdeel van het beheer van het bedrijf. Een rol aggregeert machtigingen voor toegang tot andere systemen, direct of via subfuncties, waardoor de noodzaak van IT betrokkenheid bij het beheren van gebruikersmachtigingen.

Het is belangrijk te weten dat rollen zijn van een functie van het RBAC-model zelf. rollen zijn doorgaans niet ingericht doel van toepassingen. Deze kan RBAC moet worden gebruikt samen met bestaande toepassingen die niet zijn ontworpen rollen gebruiken of kunt u de serverfunctie definities worden de behoeften van bedrijfsmodellen zonder te wijzigen van de toepassingen zelf te wijzigen. Als een doeltoepassing is ontworpen voor gebruik van functies, klikt u vervolgens kunt u bijbehorende rollen in het model BHOLD-rol met de bijbehorende rollen van de toepassing door de toepassing-specifieke rollen te behandelen als machtigingen.

In BHOLD, kunt u een rol toewijzen aan een gebruiker voornamelijk via twee mechanismen:

- Door een rol toewijzen aan een organisatie-eenheid (organisatie-eenheid), waarvan de gebruiker lid is
- Door een rol toewijst rechtstreeks aan een gebruiker

Een rol die is toegewezen aan een organisatie-eenheid van de bovenliggende eventueel kan worden overgenomen door de organisatie-eenheden lid. Wanneer een rol is toegewezen aan of worden overgenomen door een organisatie-eenheid, kan deze worden aangewezen als een rol voor een effectieve of voorgesteld. Als het een effectieve rol, worden alle gebruikers in de organisatie-eenheid de rol toegewezen. Als het een voorgestelde rol, moet deze worden geactiveerd voor elke organisatie-eenheid gebruiker of een lid te worden van kracht voor die gebruiker of leden van de organisatie-eenheid. Hierdoor kunnen gebruikers toe te wijzen een subset van de functies die zijn gekoppeld aan een organisatie-eenheid, in plaats van alle functies van de organisatie-eenheid automatisch toewijzen aan alle leden. Bovendien rollen kunnen de opgegeven start- en einddatums en limieten van het percentage van de gebruikers binnen een organisatie-eenheid waarvoor een rol mogelijk effectief kunnen worden geplaatst.

Het volgende diagram illustreert hoe een afzonderlijke gebruiker kan worden toegewezen een rol in BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

In dit diagram rol A is toegewezen aan een organisatie-eenheid als een overneembare rol, en dus wordt overgenomen door de organisatie-eenheden lid en voor alle gebruikers binnen deze organisatie-eenheden. Functie B is toegewezen als een voorgestelde rol voor een organisatie-eenheid. Worden moet geactiveerd voordat een gebruiker in de organisatie-eenheid met de machtigingen van de rol kan worden geautoriseerd. Rol C is een effectieve rol, zodat de machtigingen ervan onmiddellijk toe te op alle gebruikers in de organisatie-eenheid passen. Rol D rechtstreeks aan de gebruiker is gekoppeld en dus de machtigingen ervan onmiddellijk toepassen voor die gebruiker.

Bovendien kan een rol worden geactiveerd voor een gebruiker op basis van de kenmerken van een gebruiker. Zie voor meer informatie, op basis van een kenmerk.

#### <a name="permissions"></a>Machtigingen

Een machtiging in BHOLD overeenkomt met een machtiging die is geïmporteerd uit een doeltoepassing. Dat wil zeggen, wanneer BHOLD is geconfigureerd voor gebruik met een toepassing, ontvangt deze een lijst met machtigingen die BHOLD aan rollen koppelen kunt. Bijvoorbeeld, wanneer Active Directory Domain Services (AD DS) is toegevoegd aan BHOLD als een toepassing, ontvangt deze een lijst met beveiligingsgroepen die als BHOLD-machtigingen kunnen worden gekoppeld aan rollen in BHOLD.

Machtigingen zijn specifiek voor toepassingen. BHOLD biedt een geïntegreerde weergave van machtigingen zodat machtigingen zonder rol managers om te begrijpen details over de implementatie van de machtigingen kunnen worden gekoppeld aan rollen. In de praktijk verschillende systemen worden mogelijk toegepast een machtiging anders. De toepassingsspecifieke connector in vanuit de FIM-synchronisatieservice tot de toepassing bepaalt hoe wijzigingen in de machtigingen voor een gebruiker worden geleverd bij die toepassing. 

#### <a name="applications"></a>Toepassingen

BHOLD implementeert een methode voor het toepassen van op rollen gebaseerde toegangsbeheer (RBAC) op externe toepassingen. Dat wil zeggen, wanneer BHOLD is ingericht met gebruikers en machtigingen van een toepassing, kunt BHOLD koppelen deze machtigingen aan gebruikers door rollen toewijzen aan de gebruikers en vervolgens de machtigingen te koppelen aan de rollen. Proces van de achtergrond van de toepassing kunt vervolgens de juiste machtigingen toewijzen aan haar gebruikers op basis van de toewijzing van de rol/machtiging in BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>De rol BHOLD-Suite model te ontwikkelen

BHOLD-Suite biedt om u te helpen bij het ontwikkelen van uw model rol, Model Generator, een hulpprogramma dat is eenvoudig te gebruiken en uitgebreide.

Voordat u Model Generator gebruiken, moet u een reeks bestanden waarmee de objecten die Model Generator wordt gebruikt om het model van de functie samen te stellen. Zie Microsoft BHOLD-Suite technische documentatie voor informatie over het maken van deze bestanden.

De eerste stap bij het gebruik van de Generator BHOLD-Model is om deze bestanden voor het laden van de bouwstenen in Model Generator te importeren. Wanneer de bestanden zijn geladen, kunt u criteria die Model Generator maakt diverse klassen van rollen opgeven:

- Van lidmaatschapsfuncties die zijn toegewezen aan een gebruiker op basis van de OrgUnits die de gebruiker behoort (organisatie-eenheden)
- Kenmerk-functies die zijn toegewezen aan een gebruiker op basis van de kenmerken van de gebruiker in de BHOLD-database
- Voorgestelde functies die zijn gekoppeld aan een organisatie-eenheid, maar moeten worden geactiveerd voor specifieke gebruikers
- Eigendom-functies die een controle van de gebruiker over de organisatie-eenheden en functies waarvoor een eigenaar niet is opgegeven in de geïmporteerde bestanden verlenen

>[!Important]
Tijdens het uploaden van bestanden, selecteer de **bestaande Model behouden** selectievakje alleen in een testomgeving. In een productieomgeving, moet u Model Generator gebruiken om de eerste rol-model te maken. U kunt deze niet gebruiken om te wijzigen van een bestaand model van de rol in de BHOLD-database.

Nadat u deze rollen Model Generator maakt in het model van de functie, kunt u vervolgens het model van de functie met de BHOLD-database in de vorm van een XML-bestand exporteren.

### <a name="advanced-bhold-features"></a>Geavanceerde BHOLD-functies

Vorige secties beschreven de basisfuncties van op rollen gebaseerde toegangsbeheer (RBAC) in BHOLD. Deze sectie bevat een overzicht van aanvullende functies in BHOLD die verbeterde beveiliging en flexibiliteit om de implementatie van uw organisatie van RBAC te kunnen bieden. Deze sectie biedt overzichten van de volgende BHOLD-functies:

- Kardinaliteit
- Scheiding van functies
- Context aanpasbare machtigingen
- Verificatie op basis van kenmerken
- Flexibele kenmerktypen


#### <a name="cardinality"></a>Kardinaliteit

*Kardinaliteit* verwijst naar de implementatie van bedrijfsregels die zijn ontworpen om te beperken het aantal keren twee entiteiten kunnen zijn gerelateerd aan elkaar. In het geval van BHOLD, kunnen de kardinaliteit van regels voor rollen, machtigingen en gebruikers worden gemaakt.

U kunt een rol als u wilt beperken, het volgende configureren:

- Het maximum aantal gebruikers waarvoor een voorgestelde rol kan worden geactiveerd
- Het maximum aantal subfuncties die kunnen worden gekoppeld aan de rol
- Het maximum aantal machtigingen die kunnen worden gekoppeld aan de rol

U kunt een machtiging voor het beperken van de volgende configureren:

- Het maximum aantal rollen die kunnen worden gekoppeld aan de machtiging
- Het maximum aantal gebruikers die de machtiging kunnen worden verleend

U kunt een gebruiker om te beperken, het volgende configureren:

- Het maximum aantal rollen die kunnen worden gekoppeld aan de gebruiker
- Het maximum aantal machtigingen die kunnen worden toegewezen aan de gebruiker via roltoewijzingen

#### <a name="separation-of-duties"></a>Scheiding van functies

Scheiding van functies (beplanten) is een zakelijke principe waarmee wordt geprobeerd te voorkomen dat personen de mogelijkheid voor het uitvoeren van acties die niet beschikbaar is voor één persoon krijgen. Een werknemer moet bijvoorbeeld niet mogelijk om aan te vragen een betaling en machtigen van de betaling. Het principe van beplanten kan organisaties voor het implementeren van een systeem van controle- en balansniveau hun blootstelling aan het risico van de werknemer fout of fouten minimaliseren.

BHOLD implementeert beplanten doordat u incompatibel machtigingen definiëren. Wanneer deze machtigingen zijn gedefinieerd, BHOLD beplanten wordt afgedwongen door te voorkomen dat het maken van rollen die zijn gekoppeld aan niet-compatibele machtigingen of ze rechtstreeks zijn gekoppeld of door overname en door te voorkomen dat gebruikers niet kan worden toegewezen meerdere functies die bij gecombineerd, incompatibel machtigingen wilt verlenen opnieuw door rechtstreekse toewijzing toe of door overname. Conflicten kunnen eventueel worden genegeerd.

#### <a name="context-adaptable-permissions"></a>Context aanpasbare machtigingen

Door de machtigingen die kunnen worden automatisch gewijzigd op basis van een objectkenmerk maakt, kunt u het totale aantal machtigingen die u hebt voor het beheren van reduceren. Context aanpasbare machtigingen (CAP's) kunnen u een formule te definiëren als een machtiging-kenmerk op dat hoe de machtiging wordt toegepast door de toepassing die is gekoppeld aan de machtiging wijzigt. Bijvoorbeeld, kunt u een formule waarvan de wijzigingen de toegangsmachtigingen voor een map (via een beveiligingsgroep die is gekoppeld met de map access control list) op basis van of een gebruiker behoort tot een organisatie-eenheid (organisatie-eenheid) met fulltime- of samenvouwen van werknemers. Als de gebruiker naar een andere wordt verplaatst van de ene organisatie-eenheid, de nieuwe machtiging wordt automatisch toegepast en de oude machtiging is gedeactiveerd. 

De formule CAP kunt opvragen van de waarden van kenmerken die zijn toegepast voor toepassingen, machtigingen, organisatie-eenheden en gebruikers.

#### <a name="attribute-based-authorization"></a>Verificatie op basis van kenmerken

Een manier om te bepalen of een rol die is gekoppeld aan een organisatie-eenheid (organisatie-eenheid) is geactiveerd voor een bepaalde gebruiker in de organisatie-eenheid is het gebruik van kenmerk gebaseerde verificatie (ABA). U kunt automatisch een rol activeren met behulp van ABA alleen wanneer aan bepaalde regels op basis van de kenmerken van een gebruiker wordt voldaan. U kunt bijvoorbeeld een rol koppelen aan een organisatie-eenheid die actief is voor een gebruiker wordt alleen als de functie van de gebruiker overeenkomt met de functie in de ABA-regel. Hierdoor is de noodzaak om handmatig te activeren een voorgestelde rol voor een gebruiker. In plaats daarvan kan een rol worden geactiveerd voor alle gebruikers in een organisatie-eenheid die een kenmerkwaarde die voldoet aan de rol ABA regel hebben. Regels kunnen worden gecombineerd, zodat een rol wordt alleen geactiveerd wanneer een gebruiker kenmerken voldoen aan alle ABA regels opgegeven voor de rol.

Het is belangrijk te weten dat de resultaten van ABA regel tests worden beperkt door de van Kardinaliteitsinstellingen. Bijvoorbeeld, wordt als de kardinaliteitsinstelling van de van een regel bepaalt niet meer dan twee gebruikers een rol kunnen worden toegewezen, en als een regel ABA zou een rol voor vier gebruikers anders activeren, de rol geactiveerd alleen voor de eerste twee gebruikers die de test ABA doorstaan.

#### <a name="flexible-attribute-types"></a>Flexibele kenmerktypen

Het systeem van kenmerken in BHOLD is maximaal worden uitgebreid. U kunt nieuwe typen van de kenmerken voor dergelijke objecten definiëren als gebruikers, organisatie-eenheden (organisatie-eenheden) en rollen. Kenmerken kunnen worden gedefinieerd als u wilt dat de waarden die gehele getallen, Booleaanse waarde (Ja/Nee), alfanumerieke, datum, tijd en e-mailadressen. Kenmerken kunnen worden opgegeven als één waarde of een lijst met waarden.

## <a name="attestation"></a>Attestation

BHOLD-Suite biedt hulpprogramma's die u gebruiken kunt om te controleren dat afzonderlijke gebruikers machtigingen hebben gekregen juiste hun zakelijke taken uitvoeren. De beheerder kan de opgegeven door de module BHOLD Attestation portal gebruiken voor het ontwerpen van een beheren de attestation-proces.

De attestation-proces wordt uitgevoerd met behulp van de campagnes in welke campagne stewards de gelegenheid krijgt en betekent om te controleren of de gebruikers waarvoor ze verantwoordelijk zijn relevante toegang heeft tot BHOLD-beheerde toepassingen en de juiste machtigingen in deze toepassingen. De eigenaar van een campagne is aangewezen voor de campagne productactiveringsprogramma beheert en om ervoor te zorgen dat de campagne correct wordt verricht. Een campagne kan worden gemaakt voor het eenmalig zijn of op periodieke basis.

De wereldburgers voor een campagne worden normaal gesproken een manager die wordt behaald na de toegangsrechten van gebruikers die behoren tot een of meer organisatie-eenheden waarvoor de manager verantwoordelijk is. Stewards automatisch kan worden geselecteerd voor de gebruikers in een campagne op basis van gebruikerskenmerken wordt aangetoond of de stewards voor een campagne kunnen worden gedefinieerd door deze in een bestand dat elke gebruiker in de campagne wordt Attestation naar een wereldburgers wordt toegewezen.

Wanneer een campagne wordt gestart, wordt BHOLD een melding e-mailbericht verzendt naar de campagne stewards en de eigenaar en stuurt vervolgens regelmatig herinneringen waarmee ze worden uitgevoerd in de campagne onderhouden. Stewards worden omgeleid naar een campagne-portal waar ze een lijst van de gebruikers waarvoor ze verantwoordelijk zijn en de functies die zijn toegewezen aan deze gebruikers kunnen zien. De stewards kunnen vervolgens controleren of ze verantwoordelijk voor elk van de vermelde gebruikers zijn en goedkeuren of weigeren van de toegangsrechten van elk van de vermelde gebruikers.

De portal campagne eigenaren ook gebruiken om de voortgang van de campagne te controleren en campagneactiviteiten worden vastgelegd zodat eigenaars van de campagne de acties die zijn uitgevoerd in de loop van de campagne kunnen analyseren.

## <a name="analytics"></a>Analytics

Een van de belangrijke overwegingen bij het implementeren van een systeem van uitgebreide toegang op basis van rechten toegangsbeheer (RBAC) is het saldo tussen strikt toegangsbeheer onderhouden en te vermijden onnodige (of, slechter in, onverwachte) barrières voor toegang tot. De inspanning om dit saldo vaak onderhouden resulteert in een access control-structuur die zo complex is of onverwachte interacties tussen beleidsregels zijn bijna onvermijdelijk.

Daarom is het belangrijk om te kunnen analyseren van de gevolgen van het beleid voor toegangsbeheer voordat ze daadwerkelijk worden geplaatst in plaats. De module Analytics van BHOLD-Suite biedt u de mogelijkheid om uit te voeren deze analyse door zodat u kunt regels die staan voor uw beleid ontwikkelen en vervolgens de gebruikers waarvan de machtigingen in overeenstemming zijn of een conflict met de regel. Op basis van deze analyse, kunt u het beleid aanpassen of rollen en machtigingen eventuele conflicten met het beleid wijzigen.

De portal BHOLD-analyse biedt u de mogelijkheid om samen te stellen rulesets die bestaan uit een of meer regels die u maakt voor een bepaald beleid of de groep van het beleid testen. Een regel bestaat uit de volgende belangrijke onderdelen:

- Een titel en beschrijving op die u kunt identificeren en beschrijven de regel
- Een status die aangeeft of de regel gereed voor een overzicht is, wordt gecontroleerd of goedgekeurd
- Een element worden ingesteld (zoals gebruikers- of machtigingen) die de regel is ontworpen om te testen
- Optionele subset filters voor expressies die u gebruiken kunt om te selecteren van een juiste subgroep van het element dat moet worden onderzocht
- Een of meer regel filters zijn expressies met daarin het beleid wordt getest.

Een regel kan een van de volgende sets van element kunt testen:

- Users
- Organisatie-eenheden
- Rollen
- Machtigingen
- Toepassingen
- Accounts

Het volgende diagram wordt een eenvoudige regel die bestaan uit twee subset en twee filterregels:

![](media/bhold-concepts-guide/rules.png)

Noteer het verschil in het effect van het mislukken van een subsetfilter en een filter regel mislukt: mislukt een subsetfilter een elementobject verwijdert uit het testen door de volgende filters, terwijl een filter regel mislukt zorgt ervoor het object dat dat moet worden gerapporteerd als niet compatibel zijn. Alleen objecten die alle subset-filters en alle filters voor de regel doorgeven worden gerapporteerd als zijnde compatibel.

Elk filter bestaat uit een type, een operator (dat type afhankelijke), een sleutel (een van de elementen) en een waarde op basis waarvan de sleutel wordt getest door de operator. Het volgende filter zou bijvoorbeeld controleren of het aantal gebruikers in een subset van het element groter is dan 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Type:**   | Aantal   |
| **Sleutel:**  | Users  |
| **Operator**  | >  |
| **Waarde:** | 10 |

De filters regels kunnen zijn drie soorten en gebruiken van operators die specifiek zijn voor hun type, zoals wordt aangegeven:

- Kenmerk
  - < en >
  - = en! =
  - **Bevat**
  - **Bevat geen**
- Aantal
  - < en >
  - = en! =
- Beperkende
  - **Moet een hebben en moet alle**
  - **Geen en kan niet alle**
  - **Kan slechts een hebben en alleen voor alle hebben kunnen**
  - **Uitsluitend hebben alle en uitsluitend alle**

>[!Note]
Beperkende filters kunnen de aangegeven operators gebruiken voor het testen van een sleutel op basis van een verzameling met meerdere waarden.

Als u testen van de implementatie van een scheiding van rechten (beplanten)-beleid met de mededeling wilt dat er geen gebruiker die de aanvraag betaling machtiging heeft ook betaling goedkeuren toestemming hebben, kunt u bijvoorbeeld een regel als volgt opgeven.

|   |  |
|---|--|
|Naam:| Betaling beplanten Test|
|Element:| Users|
|Subsetfilter:| Een andere machtiging betaling aanvragen hebben|
|Regel filter: | Kan niet alle machtigingen goedkeuren betaling hebben|

Wanneer u deze regel wordt uitgevoerd, wordt de module BHOLD-analyse weergegeven van het aantal gebruikers in de geselecteerde subset (het aantal gebruikers met de machtiging betaling aanvragen), het aantal gebruikers die aan de regel voldoen en het aantal gebruikers die niet aan de regel voldoen. Vervolgens kunt u de niet-compatibele gebruikers weergeven, zodat u kunt de inconsistentie corrigeren.

Naast de resultaten weer te geven, kunt u ook het rapport opslaan als een door komma's gescheiden waarden (CSV) of het XML-bestand kunt u later de resultaten te analyseren. U kunt ook de resulterende lijst wilt weergeven van aanvullende informatie waarmee u de impact beter te begrijpen kunt aanpassen. Bijvoorbeeld, als u gebruikers testen wilt, kunt u weergeven (of rapporteren) de accounts van compatibele of niet-compatibele gebruikers zodat u kunt zien welke toepassingen betrokken zijn.

Omdat een regel meerdere filters bevatten kan, kunt u filters om te testen of een bepaalde combinatie van voorwaarden bestaan. Standaard wordt het resultaat is het product van een test en Booleaanse van alle filters, maar u kunt opgeven dat een test of van de combinatie filter worden uitgevoerd.

Bijvoorbeeld, als uw bedrijfsbeleid managers vereist om de machtiging betaling wijzigen of de machtiging goedkeuren betaling, kan u test naleving van het beleid door het construeren van een regel als volgt:


|  |  |
|--|--|
|Naam: | Betaling beplanten Test wijzigen|
|Element: | Users |
|Subsetfilter: | Een rol Manager hebben|
| Filters voor regel: |Gemachtigd bent een betaling wijzigen </br> Moet een machtiging goedkeuren betaling hebben|

Standaard worden elke gebruiker die een manager die de betalingsmethode wijzigen de machtiging betaling aanvragen en gerapporteerd als zijnde compatibel. Het beleid is echter vereist dat een manager gemachtigd beide, niet beide. Als u wilt testen werkelijke naleving van het beleid, moet u de of Booleaanse operator met de regel gebruiken om te bepalen of er zijn geen beheerders die niet over de machtiging betaling wijzigen of de machtiging goedkeuren betaling.

In tegenstelling tot andere operators de **uitsluitend hebt** en de **uitsluitend beschikken over alle** operators wijzen op naleving voor objecten die anders zou worden uitgesloten door een subsetfilter. Als u bijvoorbeeld een beleid dat u alle managers (en alleen managers) de machtiging goedkeuren beoordelingen hebben testen, kunt u een regel als volgt opgeven.

|  |  |
|--|--|
|Naam: | Bekijk goedkeuring testen|
|Element: | Users|
| Subsetfilter: | Een rol Manager hebben
|Regel filter: | Uitsluitend hebt machtiging goedkeuren beoordelingen|

Deze regel wordt gemeld als compatibel met gebruikers die managers zijn en hebben de machtiging goedkeuren beoordelingen en gebruikers die niet managers en wie bent niet gemachtigd de beoordelingen goedkeuren. Als u daarentegen worden managers die beschikt niet over deze machtiging en gebruikers die geen managers maar die gerapporteerd als niet compatibel zijn.

Zoals eerder is aangegeven, kunt u regels combineren in een ruleset waardoor het gemakkelijker om te categoriseren en regels om te voldoen aan uw bedrijfsvereisten beheren is.

Ook kunt u een set van globale filters die, wanneer dit is ingeschakeld, van toepassing op elke regel die wordt getest. Als u vaak uitsluiten van een specifieke subset van records wilt bij het testen van de regels in andere rulesets, kunt u algemene filters die u kunt inschakelen of uitschakelen, indien nodig.

## <a name="reporting"></a>Rapporten

De rapportage van BHOLD-module biedt u informatie weergeven in het model van de functie via diverse rapporten. De rapportage van BHOLD-module biedt een uitgebreide verzameling van ingebouwde rapporten, plus het bevat een wizard waarmee u gebruiken kunt om gewone en geavanceerde aangepaste rapporten te maken. Wanneer u een rapport uitvoert, kunt u onmiddellijk de resultaten weer te geven of opslaan van de resultaten in een Microsoft Excel (.xlsx). Als u wilt weergeven van dit bestand met behulp van Microsoft Excel 2000, Microsoft Excel 2002 of Microsoft Excel 2003, kunt u downloaden en installeren van Microsoft Office Compatibility Pack voor Word, Excel en PowerPoint-bestandsindelingen.


Kunt u de BHOLD rapportagemodule voornamelijk produceren van rapporten die zijn gebaseerd op de huidige rol informatie. Rapporten voor controle wijzigingen in gegevens van identiteit gegenereerd, heeft Forefront Identity Manager 2010 R2 een rapportagemogelijkheid voor de FIM-Service die is geïmplementeerd in de System Center Service Manager-datawarehouse. Zie voor meer informatie over FIM reporting de Forefront Identity Manager 2010 en Forefront Identity Manager 2010 R2-documentatie in de technische bibliotheek voor Forefront Identity Management.

Categorieën vallen de ingebouwde rapporten omvatten het volgende:

- Administration
- Attestation
- Besturingselementen
- Inkomende toegangsbeheer
- Logboekregistratie
- Model
- Statistieken
- Werkstroom

U kunt rapporten maken en toe te voegen aan deze categorieën, of kunt u uw eigen categorieën waarin u aangepaste en ingebouwde rapporten kunt plaatsen.

Als u een rapport maken, begeleidt de wizard u bij de volgende parameters opgeven:

- Identificerende informatie, zoals naam, beschrijving, categorie, gebruik en doelgroep
- Velden worden weergegeven in het rapport
- Query's die welke items opgeven moeten worden geanalyseerd
- Volgorde waarin rijen worden gesorteerd
- Velden gebruikt voor het rapport in secties opsplitsen
- Filters voor het verfijnen van de elementen die worden geretourneerd in het rapport

In elk stadium van de wizard kunt u het rapport bekijken als u deze tot nu toe hebt gedefinieerd en deze opslaan als u geen hoeft aanvullende parameters opgeeft. U kunt ook terug verplaatsen naar de vorige stappen om te wijzigen van de parameters die u eerder in de wizard hebt opgegeven.

## <a name="access-management-connector"></a>Access Management-Connector

De module BHOLD-Suite Access Management-Connector ondersteunt de initiële en lopende synchronisatie van gegevens in BHOLD. De Access Management-Connector werkt met de FIM-synchronisatieservice om gegevens tussen de Core BHOLD-database, de MIM-metaverse en doeltoepassingen en identiteitsopslag te verplaatsen.

Vorige versies van BHOLD vereist meerdere MAs waarmee de gegevensoverdracht tussen MIM en tussenliggende BHOLD-databasetabellen. In BHOLD-Suite SP1 kunt de Access Management-Connector u beheeragents (MAs) definiëren in MIM waarmee de overdracht van gegevens tussen BHOLD en MIM.

## <a name="mim-integration"></a>MIM-integratie

Een belangrijk en krachtige functie van Forefront Identity Manager 2010 en Forefront Identity Manager 2010 R2 is de self-service portal waarmee eindgebruikers kunnen hun identiteit en lidmaatschap informatie weergeven en beheren. MIM-integratie breidt de MIM-Portal met selfservice rollen beheren. Bijvoorbeeld via de BHOLD-functies in de MIM-Portal een gebruiker kan aanvragen roltoewijzing en actieve rollen kunt weergeven en aanvragen in behandeling. Aanvullende mogelijkheden kunnen worden toegekend aan gedelegeerde beheerders, zoals de mogelijkheid om te aanvraag roltoewijzingen voor andere gebruikers.

Het is belangrijk te weten dat de BHOLD-extensies voor de MIM-Portal functie selfservice en werkstroombeheer ondersteunen en rapportage. Andere functies voor het beheer van BHOLD, evenals de Attestation-bewerking, worden geleverd door de webportals BHOLD-modules die worden gehost, afzonderlijk van de MIM-Portal.

## <a name="next-steps"></a>Volgende stappen
