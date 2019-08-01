---
title: Hand leiding micro soft BHOLD Suite-concepten | Microsoft Docs
description: Ga aan de slag met de MIM 2016-onderdelen en installeer en configureer de synchronisatieservice.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/14/2017
ms.topic: conceptual
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 3749b74fd867601ee05f8e45d273ad2de9144b5b
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701429"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Hand leiding micro soft BHOLD Suite-concepten

Met Microsoft Identity Manager 2016 (MIM) kunnen organisaties de volledige levens cyclus van gebruikers identiteiten en hun bijbehorende referenties beheren. Het kan worden geconfigureerd voor het synchroniseren van identiteiten, het centraal beheren van certificaten en wacht woorden en het inrichten van gebruikers op heterogene systemen. Met MIM kunnen IT-organisaties de processen definiëren en automatiseren die worden gebruikt voor het beheren van identiteiten voor het maken van een pensioen.

Micro soft BHOLDe Suite breidt deze mogelijkheden van MIM uit door op rollen gebaseerd toegangs beheer toe te voegen. BHOLD stelt organisaties in staat om gebruikers rollen te definiëren en de toegang tot gevoelige gegevens en toepassingen te beheren. De toegang is gebaseerd op wat geschikt is voor deze rollen. BHOLD Suite bevat services en hulpprogram ma's waarmee het model leren van de functie relaties binnen de organisatie wordt vereenvoudigd. BHOLD wijst die rollen toe aan rechten en controleert of de roldefinities en de bijbehorende rechten correct worden toegepast op gebruikers. Deze mogelijkheden zijn volledig geïntegreerd met MIM en bieden een naadloze ervaring voor eind gebruikers en IT-mede werkers.

Deze hand leiding helpt u te begrijpen hoe BHOLD Suite werkt met MIM en bevat de volgende onderwerpen:

- Op rollen gebaseerd toegangsbeheer
- Attestation
- Analyse
- Rapporten
- Toegangs beheer connector
- MIM-integratie

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

De meest voorkomende methode voor het beheren van gebruikers toegang tot gegevens en toepassingen is via DAC (Discretionary Access Control). In de meeste voorkomende implementaties heeft elk significant object een geïdentificeerde eigenaar. De eigenaar heeft de mogelijkheid om toegang tot het object toe te kennen of te weigeren aan anderen op basis van individuele identiteit of groepslid maatschap. In de praktijk resulteert DAC doorgaans in een verdwaald van beveiligings groepen, wat een organisatie structuur weerspiegelt, anderen die functionele groeperingen vertegenwoordigen (zoals taak typen of project toewijzingen), en andere die bestaan uit makeshift-verzamelingen van gebruikers en apparaten die zijn gekoppeld voor meer tijdelijke doel einden. Naarmate organisaties groeien, is lidmaatschap van deze groepen steeds moeilijker te beheren. Als een werk nemer bijvoorbeeld van het ene naar het andere project wordt overgezet, moeten de groepen die worden gebruikt om de toegang tot de project activa te beheren, hand matig worden bijgewerkt. In dergelijke gevallen is het niet ongebruikelijk dat fouten optreden, fouten die de project beveiliging of productiviteit kunnen belemmeren.

MIM bevat functies waarmee u dit probleem kunt verhelpen door automatische controle te bieden over het lidmaatschap van groepen en distributie lijsten. Dit biedt echter geen oplossing voor de intrinsieke complexiteit van groepen die niet noodzakelijkerwijs aan elkaar zijn gerelateerd op een gestructureerde manier.

Eén manier om deze toename aanzienlijk te reduceren is door op rollen gebaseerd toegangs beheer (RBAC) te implementeren. Met RBAC wordt DAC niet overgeplaatst.  RBAC bouwt voort op DAC door een Framework te bieden voor het classificeren van gebruikers en IT-resources. Zo kunt u de relatie en de toegangs rechten die van toepassing zijn op basis van die classificatie, expliciet maken. Als u bijvoorbeeld toewijst aan een gebruikers kenmerk waarmee de gebruikers titel en project toewijzingen van de gebruiker worden opgegeven, kan gebruikers toegang krijgen tot de hulpprogram ma's die nodig zijn voor de taak en gegevens van de gebruiker die de gebruiker aan een bepaald project moet bijdragen. Wanneer de gebruiker een andere taak en verschillende Project toewijzingen aanneemt, wordt de toegang tot de resources die nodig zijn voor de vorige positie van de gebruikers automatisch geblokkeerd door de kenmerken die de functie van de gebruiker en de projecten opgeven.

Omdat rollen op een hiërarchische manier kunnen worden opgenomen in rollen, (bijvoorbeeld de rollen van verkoop Manager en vertegenwoordiger kunnen worden opgenomen in de algemenere rol van verkoop), is het eenvoudig om de juiste rechten toe te wijzen aan specifieke rollen en nog steeds te bieden de juiste rechten voor iedereen die de uitgebreidere rol delen. In een zieken huis kan alle medische mede werkers bijvoorbeeld het recht krijgen om een patiënten-record weer te geven, maar alleen artsen (een subrol van de medische rol) kunnen het recht krijgen om voor schriften voor de patiënt in te voeren. Op dezelfde manier kunnen gebruikers die deel uitmaken van de rol van de betrokkene, toegang krijgen tot patiënten-records, met uitzonde ring van facturerings medewerkers (een subrol van de rol van patiënten), die toegang kunnen krijgen tot die delen van een patiënt records die vereist zijn voor de betaling van de patiënt voor services van het zieken huis.

Een extra voor deel van RBAC is de mogelijkheid om schei ding van taken (SoD) te definiëren en af te dwingen. Hiermee kan een organisatie combi Naties definiëren van rollen die machtigingen verlenen die niet door dezelfde gebruiker mogen worden bewaard, zodat aan een bepaalde gebruiker geen rollen kunnen worden toegewezen waarmee de gebruiker een betaling kan initiëren en bijvoorbeeld een betaling kan autoriseren. RBAC biedt de mogelijkheid om dergelijk beleid automatisch af te dwingen in plaats van de daad werkelijke implementatie van het beleid te evalueren op basis van per gebruiker.

### <a name="bhold-role-model-objects"></a>BHOLD

Met BHOLD Suite kunt u rollen binnen uw organisatie opgeven en organiseren, gebruikers aan rollen toewijzen en de juiste machtigingen toewijzen aan rollen. Deze structuur wordt een rollen model genoemd en bevat en verbindt vijf typen objecten: 

- Organisatie-eenheden
- Gebruikers
- Rollen
- Machtigingen
- Toepassingen

#### <a name="organizational-units"></a>Organisatie-eenheden

Organisatie-eenheden (OrgUnits) zijn de belangrijkste middelen waarmee gebruikers worden georganiseerd in het BHOLD-rollen model. Elke gebruiker moet tot ten minste één OrgUnit behoren. (Als een gebruiker wordt verwijderd uit de laatste organisatie-eenheid in BHOLD, wordt de gegevens record van de gebruiker verwijderd uit de BHOLD-data base.)

> [!Important]
> Organisatie-eenheden in het BHOLD-functie model mogen niet worden verward met organisatie-eenheden in Active Directory Domain Services (AD DS). De structuur van organisatie-eenheden in BHOLD is doorgaans gebaseerd op de organisatie en het beleid van uw bedrijf, niet aan de vereisten van uw netwerk infrastructuur.

Hoewel dit niet vereist is, zijn de organisatie-eenheden in de meeste gevallen gestructureerd in BHOLD om de hiërarchische structuur van de daad werkelijke organisatie te vertegenwoordigen, zoals hieronder wordt weer gegeven:

![](media/bhold-concepts-guide/org-chart.png)

In dit voor beeld zou het Role model de eenheid organizationalganizatinal voor het bedrijf als geheel (vertegenwoordigd door de president, omdat de president geen deel uitmaakt van een mororganizationalganizatinal-eenheid) of de organisatie-eenheid BHOLD root (die altijd exists) kan hiervoor worden gebruikt. OrgUnits die de bedrijfs divisies van de vice-presidenten vertegenwoordigen, worden in de organisatie-eenheid van het bedrijf geplaatst. Vervolgens worden organisatie-eenheden die overeenkomen met de marketing-en verkoop directeur toegevoegd aan de organisatie-eenheden marketing en verkoop, en organisatie-eenheden die de regionale verkoop managers vertegenwoordigen, worden in de organisatie-eenheid geplaatst voor de verkoop Manager Oost-regio. Verkoop Associates, die geen andere gebruikers beheren, zouden lid zijn van de organisatie-eenheid van de regio West verkoop Manager. Houd er rekening mee dat gebruikers lid kunnen zijn van een organisatie-eenheid op elk niveau. Zo is een administratief mede werker, die geen Manager is en rapporten rechtstreeks aan een vice-president, een lid is van de organisatie-eenheid van de vice-president.

Naast de organisatie structuur kunnen organisatie-eenheden ook worden gebruikt om gebruikers en andere organisatie-eenheden te groeperen op basis van functionele criteria, zoals voor projecten of specialisatie. In het volgende diagram ziet u hoe organisatie-eenheden worden gebruikt om de verkoop Associates te groeperen op basis van klant type:

![](media/bhold-concepts-guide/org-chart-02.png)

In dit voor beeld zou elke verkoop medewerker behoren tot twee organisatie-eenheden: een die de plaats van de koppeling vertegenwoordigt in de beheer structuur van de organisatie, en één van de klanten basis van de partner (retail of zakelijk). Elke organisatie-eenheid kan worden toegewezen aan verschillende rollen die op hun beurt kunnen worden toegewezen aan verschillende machtigingen voor toegang tot de IT-resources van de organisatie. Daarnaast kunnen rollen worden overgenomen van bovenliggende organisatie-eenheden, waardoor het proces van het door geven van rollen aan gebruikers wordt vereenvoudigd. Aan de andere kant kunnen specifieke rollen voor komen dat ze worden overgenomen, zodat een specifieke rol alleen wordt gekoppeld aan de juiste organisatie-eenheden.

OrgUnits kan in BHOLD Suite worden gemaakt met behulp van de BHOLD core-webportal of met behulp van de model Generator van het BHOLD.

#### <a name="users"></a>Gebruikers

Zoals hierboven vermeld, moet elke gebruiker tot ten minste één organisatie-eenheid (OrgUnit) behoren. Omdat organisatie-eenheden het hoofd mechanisme zijn voor het koppelen van een gebruiker met rollen, in het meren deel van organisaties van een bepaalde gebruiker tot meerdere OrgUnits, zodat het eenvoudiger is om rollen met die gebruiker te koppelen. In sommige gevallen kan het echter nodig zijn om een rol te koppelen aan een gebruiker gescheiden van een OrgUnits waartoe de gebruiker behoort. Daarom kan een gebruiker rechtstreeks aan een rol worden toegewezen en kunnen ze rollen ophalen van de OrgUnits waartoe de gebruiker behoort.

Wanneer een gebruiker in de organisatie niet actief is (bijvoorbeeld als er geen medische verlof is), kan de gebruiker worden onderbroken, waardoor alle machtigingen van de gebruiker worden ingetrokken zonder dat de gebruiker uit het functie model wordt verwijderd. Wanneer de gebruiker terugkeert naar het recht, kan deze opnieuw worden geactiveerd. Hiermee worden alle machtigingen hersteld die door de rollen van de gebruiker worden verleend.

Objecten voor gebruikers kunnen afzonderlijk worden gemaakt in BHOLD via de BHOLD-basis-webportal, of ze kunnen bulksgewijs worden geïmporteerd met behulp van BHOLD model generator, of via de Access Management-connector met de FIM-synchronisatie service om gebruikers gegevens te importeren uit dergelijke bronnen als Active Directory Domain Services of human resources-toepassingen.

Gebruikers kunnen rechtstreeks in BHOLD worden gemaakt zonder de FIM-synchronisatie service te gebruiken. Dit kan handig zijn bij het model leren van rollen in een preproductie of test omgeving. U kunt ook externe gebruikers (zoals werk nemers van een toeleverancier) in staat stellen rollen toe te wijzen en zodoende toegang te krijgen tot IT-resources zonder dat ze worden toegevoegd aan de data base van de werk nemer. deze gebruikers kunnen echter de BHOLD self-service functies niet gebruiken.

#### <a name="roles"></a>Rollen

Zoals eerder vermeld, onder het RBAC-model (op rollen gebaseerd toegangs beheer) zijn machtigingen gekoppeld aan rollen in plaats van afzonderlijke gebruikers. Dit maakt het mogelijk om elke gebruiker de benodigde machtigingen te geven voor het uitvoeren van de taken van de gebruiker door de rollen van de gebruiker te wijzigen in plaats van de gebruikers machtigingen afzonderlijk toe te kennen of te weigeren. Als gevolg hiervan is de toewijzing van machtigingen niet langer vereist voor deelname aan de IT-afdeling, maar kan ook worden uitgevoerd als onderdeel van het beheer van het bedrijf. Een rol kan machtigingen verzamelen om toegang te krijgen tot verschillende systemen, hetzij rechtstreeks, hetzij via het gebruik van subrollen, waardoor de IT-betrokkenheid voor het beheer van gebruikers machtigingen nog verder wordt beperkt.

Het is belang rijk te weten dat rollen een functie zijn van het RBAC-model zelf. rollen worden doorgaans niet ingericht voor doel toepassingen. Hierdoor kan RBAC worden gebruikt in combi natie met bestaande toepassingen die niet zijn ontworpen om rollen te gebruiken of om de roldefinities te wijzigen, maar moet voldoen aan de behoeften van veranderende bedrijfs modellen zonder dat u de toepassingen zelf hoeft te wijzigen. Als een doel toepassing is ontworpen voor het gebruik van rollen, kunt u in het BHOLD met overeenkomstige toepassings rollen rollen koppelen door de toepassingsspecifieke rollen als machtigingen te behandelen.

In BHOLD kunt u een rol toewijzen aan een gebruiker, voornamelijk door middel van twee mechanismen:

- Door een rol toe te wijzen aan een organisatie-eenheid (organisatie-eenheid) waarvan de gebruiker lid is
- Door een rol rechtstreeks toe te wijzen aan een gebruiker

Een rol die is toegewezen aan een bovenliggende organisatie-eenheid, optioneel kan worden overgenomen door de organisatie-eenheden van die leden. Wanneer een rol wordt toegewezen aan of overgenomen door een organisatie-eenheid, kan deze worden aangeduid als een doel matige of voorgestelde rol. Als het een effectief rol is, krijgen alle gebruikers in de organisatie-eenheid de rol. Als het een voorgestelde rol is, moet deze worden geactiveerd voor elke organisatie-eenheid van de gebruiker of organisatie, zodat deze van toepassing is op de leden van die gebruiker. Dit maakt het mogelijk om gebruikers een subset van de rollen toe te wijzen die aan een organisatie-eenheid zijn gekoppeld, in plaats van alle rollen van de organisatie-eenheid automatisch toe te wijzen aan alle leden. Daarnaast kunnen rollen een begin-en eind datum krijgen en kunnen limieten worden geplaatst op het percentage gebruikers binnen een organisatie-eenheid waarvoor een rol effectief kan zijn.

In het volgende diagram ziet u hoe aan een afzonderlijke gebruiker een rol kan worden toegewezen in BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

In dit diagram wordt rol A toegewezen aan een organisatie-eenheid als een overneem bare rol, en wordt deze dus overgenomen door de organisatie-eenheden van de leden en alle gebruikers binnen die organisatie-eenheden. Rol B wordt toegewezen als een voorgestelde rol voor een organisatie-eenheid. Het moet worden geactiveerd voordat een gebruiker in de organisatie-eenheid kan worden gemachtigd met de machtigingen van de rol. Role C is een effectief rol, zodat de machtigingen direct van toepassing zijn op alle gebruikers in de organisatie-eenheid. Rol D is rechtstreeks gekoppeld aan de gebruiker en de bijbehorende machtigingen zijn dus direct van toepassing op die gebruiker.

Daarnaast kan een rol worden geactiveerd voor een gebruiker op basis van de kenmerken van een gebruiker. Zie autorisatie op basis van kenmerken voor meer informatie.

#### <a name="permissions"></a>Machtigingen

Een machtiging in BHOLD correspondeert met een autorisatie die is geïmporteerd uit een doel toepassing. Dat wil zeggen, wanneer BHOLD is geconfigureerd om te werken met een toepassing, een lijst met autorisaties ontvangt die BHOLD kan koppelen aan rollen. Als Active Directory Domain Services (AD DS) bijvoorbeeld als een toepassing aan BHOLD wordt toegevoegd, ontvangt deze een lijst met beveiligings groepen die, als BHOLD-machtigingen, kunnen worden gekoppeld aan rollen in BHOLD.

Machtigingen zijn specifiek voor toepassingen. BHOLD biedt een enkele, uniforme weer gave van machtigingen zodat machtigingen kunnen worden gekoppeld aan rollen, zonder dat rollen beheerders de implementatie details van de machtigingen moeten begrijpen. In de praktijk kunnen verschillende systemen een machtiging anders afdwingen. De toepassingsspecifieke connector van de FIM-synchronisatie service naar de toepassing bepaalt hoe machtigings wijzigingen voor een gebruiker aan die toepassing worden gegeven. 

#### <a name="applications"></a>Toepassingen

BHOLD implementeert een methode voor het Toep assen van op rollen gebaseerd toegangs beheer (RBAC) naar externe toepassingen. Dat wil zeggen dat wanneer BHOLD wordt ingericht met gebruikers en machtigingen van een toepassing, BHOLD deze machtigingen aan gebruikers kan koppelen door rollen aan de gebruikers toe te wijzen en vervolgens de machtigingen te koppelen aan de rollen. Het achtergrond proces van de toepassing kan vervolgens de juiste machtigingen toewijzen aan de gebruikers op basis van de rol/machtigingen toewijzing in BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Het BHOLD Suite-rollen model ontwikkelen

Om u te helpen uw Role model te ontwikkelen, biedt BHOLD Suite model generator, een hulp programma dat gemakkelijk te gebruiken en uitgebreid is.

Voordat u model generator gebruikt, moet u een reeks bestanden maken waarmee de objecten worden gedefinieerd die door model generator worden gebruikt om het roltype samen te stellen. Zie technische documentatie voor micro soft BHOLD Suite voor meer informatie over het maken van deze bestanden.

De eerste stap bij het gebruik van de BHOLD-model Generator is het importeren van deze bestanden om de basis bouwstenen te laden in model generator. Wanneer de bestanden zijn geladen, kunt u criteria opgeven die door model generator worden gebruikt voor het maken van verschillende klassen rollen:

- Lidmaatschaps rollen die aan een gebruiker zijn toegewezen op basis van de OrgUnits (organisatie-eenheden) waartoe de gebruiker behoort
- Kenmerk rollen die aan een gebruiker zijn toegewezen op basis van de kenmerken van de gebruiker in de BHOLD-data base
- Voorgestelde rollen die zijn gekoppeld aan een organisatie-eenheid, maar moeten worden geactiveerd voor specifieke gebruikers
- Eigendoms rollen die een gebruikers controle over organisatie-eenheden en rollen verlenen waarvoor een eigenaar niet is opgegeven in de geïmporteerde bestanden

> [!Important]
> Wanneer u bestanden uploadt, schakelt u het selectie vakje **bestaand model behouden** alleen in test omgevingen in. In productie omgevingen moet u model generator gebruiken om het eerste roltype te maken. U kunt dit niet gebruiken om een bestaand roltype in de BHOLD-data base te wijzigen.

Nadat model Generator deze rollen in het-roltype heeft gemaakt, kunt u het Role Model exporteren naar de BHOLD-data base in de vorm van een XML-bestand.

### <a name="advanced-bhold-features"></a>Geavanceerde BHOLD-functies

In de vorige secties zijn de basis functies van het toegangs beheer op basis van rollen (RBAC) in BHOLD beschreven. In deze sectie vindt u een overzicht van de aanvullende functies in BHOLD die verbeterde beveiliging en flexibiliteit kunnen bieden voor de implementatie van RBAC van uw organisatie. In deze sectie vindt u overzichten van de volgende BHOLD-functies:

- Kardinaliteit
- Schei ding van taken
- Context aanpas bare machtigingen
- Verificatie op basis van een kenmerk
- Flexibele kenmerk typen


#### <a name="cardinality"></a>Kardinaliteit

*Kardinaliteit* verwijst naar de implementatie van bedrijfs regels die zijn ontworpen om het aantal keren te beperken dat twee entiteiten aan elkaar kunnen worden gekoppeld. In het geval van BHOLD kunnen kardinaliteit regels worden ingesteld voor rollen, machtigingen en gebruikers.

U kunt een rol configureren om het volgende te beperken:

- Het maximum aantal gebruikers waarvoor een voorgestelde rol kan worden geactiveerd
- Het maximum aantal subrollen dat kan worden gekoppeld aan de rol
- Het maximum aantal machtigingen dat kan worden gekoppeld aan de rol

U kunt een machtiging configureren om het volgende te beperken:

- Het maximum aantal rollen dat kan worden gekoppeld aan de machtiging
- Het maximum aantal gebruikers aan wie de machtiging kan worden verleend

U kunt een gebruiker configureren om het volgende te beperken:

- Het maximum aantal rollen dat aan de gebruiker kan worden gekoppeld
- Het maximum aantal machtigingen dat kan worden toegewezen aan de gebruiker via roltoewijzingen

#### <a name="separation-of-duties"></a>Schei ding van taken

Schei ding van taken (SoD) is een bedrijfs principe waarmee wordt voor komen dat personen de mogelijkheid krijgen om acties uit te voeren die niet voor één persoon beschikbaar moeten zijn. Een werk nemer kan bijvoorbeeld geen betaling aanvragen en de betaling autoriseren. Het principe van SoD stelt organisaties in staat om een systeem van controles en saldo's te implementeren om de bloot stelling van werknemers fouten of een ernstige storing te minimaliseren.

BHOLD implementeert SoD door u incompatibele machtigingen te definiëren. Wanneer deze machtigingen zijn gedefinieerd, BHOLD afdwingt SoD door het maken van functies die zijn gekoppeld aan niet-compatibele machtigingen te voor komen, ongeacht of deze rechtstreeks of via overname zijn gekoppeld, en door te voor komen dat gebruikers meerdere rollen kunnen toewijzen, wanneer gecombineerd, worden incompatibele machtigingen verleend, opnieuw per directe toewijzing of via overname. Conflicten kunnen eventueel worden overschreven.

#### <a name="context-adaptable-permissions"></a>Context aanpas bare machtigingen

Door machtigingen te maken die automatisch kunnen worden gewijzigd op basis van een object kenmerk, kunt u het totale aantal machtigingen beperken dat u wilt beheren. Met context aanpas bare machtigingen (Cap's) kunt u een formule definiëren als een machtigings kenmerk dat wijzigt hoe de machtiging wordt toegepast door de toepassing die is gekoppeld aan de machtiging. U kunt bijvoorbeeld een formule maken die de toegangs machtiging wijzigt in een bestandsmap (via een beveiligings groep die is gekoppeld aan de toegangs beheer lijst van de map), op basis van het feit of een gebruiker lid is van een organisatie-eenheid (organisatie-eenheid) die fulltime-of contract werk nemers. Als de gebruiker van de ene organisatie-eenheid naar de andere wordt verplaatst, wordt de nieuwe machtiging automatisch toegepast en wordt de oude machtiging gedeactiveerd. 

De CAP-formule kan een query uitvoeren op de waarden van kenmerken die zijn toegepast op toepassingen, machtigingen, organisatie-eenheden en gebruikers.

#### <a name="attribute-based-authorization"></a>Verificatie op basis van een kenmerk

Een manier om te bepalen of een rol die is gekoppeld aan een organisatie-eenheid (organisatie-eenheid) is geactiveerd voor een bepaalde gebruiker in de organisatie-eenheid, is het gebruik van op kenmerken gebaseerde autorisatie (ABA). Door ABA te gebruiken, kunt u automatisch een rol activeren wanneer aan bepaalde regels op basis van de kenmerken van een gebruiker wordt voldaan. U kunt bijvoorbeeld een rol koppelen aan een organisatie-eenheid die alleen actief wordt voor een gebruiker als de functie van de gebruiker overeenkomt met de naam van de functie in de ABA-regel. Dit elimineert de nood zaak om een voorgestelde rol voor een gebruiker niet hand matig te activeren. In plaats daarvan kan een rol worden geactiveerd voor alle gebruikers in een organisatie-eenheid met een kenmerk waarde die voldoet aan de ABA-regel van de rol. Regels kunnen worden gecombineerd, zodat een rol alleen wordt geactiveerd wanneer de kenmerken van een gebruiker voldoen aan alle ABA-regels die zijn opgegeven voor de rol.

Het is belang rijk te weten dat de resultaten van ABA regel tests worden beperkt door instellingen voor kardinaliteit. Als de instelling voor kardinaliteit van een regel bijvoorbeeld aangeeft dat er niet meer dan twee gebruikers een rol kunnen worden toegewezen, en als een ABA-regel anders een rol zou activeren voor vier gebruikers, wordt de rol alleen geactiveerd voor de eerste twee gebruikers die de ABA-test door geven.

#### <a name="flexible-attribute-types"></a>Flexibele kenmerk typen

Het systeem met kenmerken in BHOLD is zeer uitbreidbaar. U kunt nieuwe kenmerk typen voor dergelijke objecten definiëren als gebruikers, organisatie-eenheden (organisatie-eenheden) en rollen. Kenmerken kunnen worden gedefinieerd voor waarden die een geheel getal zijn, een Booleaanse waarde (Ja/Nee), alfanumerieke tekens, datum, tijd en e-mail adressen. Kenmerken kunnen worden opgegeven als één waarde of als een lijst met waarden.

## <a name="attestation"></a>Attestation

De BHOLD Suite bevat hulpprogram ma's die u kunt gebruiken om te controleren of afzonderlijke gebruikers de juiste machtigingen hebben gekregen om hun bedrijfs taken uit te voeren. De beheerder kan de portal van de BHOLD Attestation-module gebruiken om een beheer van het Attestation-proces te ontwerpen.

Het Attestation-proces wordt uitgevoerd door middel van campagnes waarbij campagne-BHOLD de mogelijkheid krijgen om te controleren of de gebruikers voor wie ze verantwoordelijk zijn, de juiste toegang hebben tot door beheerde toepassingen en de juiste machtigingen binnen deze toepassingen. Een campagne-eigenaar is aangewezen om de campagne te controleren en ervoor te zorgen dat de campagne correct wordt uitgevoerd. Een campagne kan worden gemaakt om één keer of op een terugkerende basis te worden uitgevoerd.

Normaal gesp roken is het voor een campagne een manager waarmee de toegangs rechten worden verklaard van gebruikers die deel uitmaken van een of meer organisatie-eenheden waarvoor de manager verantwoordelijk is. Er kunnen automatisch een module worden geselecteerd voor de gebruikers die worden verklaard in een campagne op basis van gebruikers kenmerken, of de-consoles voor een campagne kunnen worden gedefinieerd door ze in een bestand te vermelden, waarbij elke gebruiker wordt toegewezen aan een.

Wanneer een campagne begint, stuurt BHOLD een e-mail meldings bericht naar de campagne-en-eigenaar en verzendt deze vervolgens periodieke herinneringen om hen te helpen bij het onderhouden van de voortgang in de campagne. Er worden in een campagne Portal een lijst weer geven met de gebruikers waarvoor ze verantwoordelijk zijn en de rollen die aan deze gebruikers zijn toegewezen. U kunt vervolgens bevestigen of ze verantwoordelijk zijn voor elk van de vermelde gebruikers en de toegangs rechten van elk van de vermelde gebruikers goed keuren of weigeren.

De portal campagne eigenaren ook gebruiken om de voortgang van de campagne te controleren en campagneactiviteiten worden vastgelegd zodat eigenaars van de campagne de acties die zijn uitgevoerd in de loop van de campagne kunnen analyseren.

## <a name="analytics"></a>Analyse

Een van de belang rijke overwegingen bij het implementeren van een uitgebreid RBAC-systeem (Rights Access Control) is het evenwicht tussen het beheren van strikte toegangs beheer en het voor komen van onnodige (of, verslechterde, onverwachte) obstakels voor toegang. De inspanningen om dit saldo te hand haven, hebben vaak een toegangs beheer structuur die zo complex is dat onverwachte interacties tussen beleids regels bijna onvermijdbaar zijn.

Daarom is het belang rijk dat u de gevolgen van beleid voor toegangs beheer kunt analyseren voordat ze daad werkelijk worden geplaatst. De analyse module van BHOLD Suite biedt u de mogelijkheid om deze analyse uit te voeren door regels te ontwikkelen die uw beleid vertegenwoordigen en vervolgens de gebruikers te laten zien waarvan de machtigingen voldoen aan of conflicteren met de regel. Op basis van deze analyse kunt u het beleid aanpassen of rollen en machtigingen wijzigen om conflicten met het beleid op te lossen.

De BHOLD Analytics Portal biedt u de mogelijkheid om Rule sets te maken die bestaan uit een of meer regels die u maakt om een bepaald beleid of groep beleids regels te testen. Een regel bestaat uit de volgende hoofd onderdelen:

- Een titel en beschrijving waarmee u de regel kunt identificeren en beschrijven
- Een status die aangeeft of de regel gereed is voor controle, wordt gecontroleerd of goedgekeurd
- Een verzameling elementen (zoals gebruikers of machtigingen) die de regel is ontworpen om te testen
- Optionele subset filters die expressies zijn die u kunt gebruiken om een geschikte subgroep te selecteren van het element dat moet worden onderzocht
- Een of meer regel filters die expressies vertegenwoordigen voor het beleid dat wordt getest.

Een regel kan een van de volgende element sets testen:

- Gebruikers
- Organisatie-eenheden
- Rollen
- Machtigingen
- Toepassingen
- Accounts

In het volgende diagram ziet u een eenvoudige regel die bestaat uit twee deel verzamelings regels en twee filter regels:

![](media/bhold-concepts-guide/rules.png)

Let op het verschil in het effect van het mislukken van een subset filter en het mislukken van een regel filter: Als u een subset filter uitschakelt, wordt een object element verwijderd uit het testen op volgende filters, terwijl bij het mislukken van een regel filter wordt aangegeven dat het object niet aan het beleid voldoet. Alleen de objecten die alle subset filters door geven en alle regel filters worden als compatibel gerapporteerd.

Elk filter bestaat uit een type, een operator (die van type is afhankelijk), een sleutel (een van de elementen) en een waarde waarmee de sleutel door de operator wordt getest. Met het volgende filter wordt bijvoorbeeld getest of het aantal gebruikers in een element deel uitmaakt van meer dan 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Type:**   | Aantal   |
| **Prestatie**  | Gebruikers  |
| **Operator**  | >  |
| **Waarde:** | 10 |

De regel filters kunnen uit drie typen bestaan en Opera tors die specifiek zijn voor het type, zoals aangegeven:

- Kenmerk
  - < en >
  - = en! =
  - **Daarin**
  - **Bevat niet**
- Aantal
  - < en >
  - = en! =
- Beperkende
  - **Moet alle hebben en moeten hebben**
  - **Kan geen van beide bevatten**
  - **Kan alleen alle hebben en mag alleen**
  - **Alleen voor alle en enige**

> [!Note]
> Beperkend filters kunnen de aangegeven Opera tors gebruiken om een sleutel te testen op basis van een set met meerdere waarden.

Als u bijvoorbeeld wilt testen van de implementatie van een SoD-beleid (schei ding van taken) dat aangeeft dat geen enkele gebruiker met een machtiging voor aanvraag betaling een machtiging voor het goed keuren van betalingen heeft, kunt u een regel als volgt maken:

|   |  |
|---|--|
|Naam:| Test betalings SoD|
|ElementName| Gebruikers|
|Subset-filter:| Een betaling met toestemming aanvragen|
|Regel filter: | Kan geen machtiging voor betaling goed keuren|

Wanneer u deze regel uitvoert, wordt in de BHOLD Analytics-module het aantal gebruikers in de geselecteerde subset weer gegeven (het aantal gebruikers met de machtiging voor het aanvragen van een aanvraag), het aantal gebruikers dat voldoet aan de regel en het aantal gebruikers dat niet aan de regel voldoet. U kunt de niet-compatibele gebruikers vervolgens weer geven zodat u de inconsistentie kunt corrigeren.

Naast het weer geven van de resultaten kunt u het rapport ook opslaan als een door komma's gescheiden waarde (CSV) of XML-bestand, zodat u de resultaten op een later tijdstip kan analyseren. U kunt ook het resulterende rapport aanpassen om aanvullende informatie weer te geven die u kan helpen beter inzicht te krijgen in de gevolgen. Als u bijvoorbeeld gebruikers test, kunt u de accounts van de compatibele of niet-compatibele gebruikers weer geven (of rapporteren), zodat u kunt zien welke toepassingen bij u betrokken zijn.

Omdat een regel meerdere filters kan bevatten, kunt u filters verbinden om te testen of er een bepaalde combi natie van voor waarden bestaat. Het resultaat is standaard het product van een Boole-test van alle filters, maar u kunt opgeven dat de filter combinatie moet worden uitgevoerd.

Als uw bedrijfs beleid bijvoorbeeld beheerders nodig heeft om de machtiging voor het wijzigen van de betaling of het goed keuren van de betaling te hebben, kunt u de naleving van het beleid testen door een regel te maken zoals in het volgende voor beeld:


|  |  |
|--|--|
|Naam: | Betalings SoDe test wijzigen|
|ElementName | Gebruikers |
|Subset-filter: | Een rollen beheerder hebben|
| Regel filters: |U moet een machtiging hebben om de betaling te wijzigen </br> U moet een machtiging hebben om de betaling goed te keuren|

Standaard worden gebruikers die een manager zijn en die de machtiging voor het wijzigen van de betaling hebben, gerapporteerd als compatibel. Het beleid vereist echter dat een manager over beide machtigingen beschikt, niet noodzakelijkerwijs beide. Als u de werkelijke naleving van het beleid wilt testen, moet u de operator of Boolean met de regel gebruiken om te bepalen of er managers zijn die de machtiging voor het wijzigen van de betaling of voor het goed keuren van de betaling niet hebben.

In tegens telling tot andere opera tors **hebben** **alle** Opera tors alleen de compatibiliteit voor objecten die anders zouden worden uitgesloten door een subset filter. Als u bijvoorbeeld een beleid wilt testen dat alle managers (en alleen managers) de machtiging beoordeling goed keuren hebben, kunt u als volgt een regel maken:

|  |  |
|--|--|
|Naam: | Goedkeurings test controleren|
|ElementName | Gebruikers|
| Subset-filter: | Een rollen beheerder hebben
|Regel filter: | Zonder goed keuring van toestemming|

Deze regel meldt zich als compatibele gebruikers die beheerders zijn en die de machtiging goed keuren voor controle hebben en gebruikers die geen beheerder zijn en die geen machtiging voor goed keuring hebben. Beheerders die deze machtiging niet hebben en gebruikers die geen beheerders rechten hebben, maar wel die machtiging hebben gerapporteerd als niet-compatibel.

Zoals eerder is vermeld, kunt u regels combi neren in een ruleSet, waardoor het eenvoudiger wordt om regels te categoriseren en beheren om te voldoen aan uw bedrijfs vereisten.

U kunt ook een set globale filters definiëren die, indien ingeschakeld, van toepassing zijn op elke regel die wordt getest. Als u regel matig een bepaalde subset records wilt uitsluiten bij het testen van regels in verschillende regel sets, kunt u globale filters opgeven die u zo nodig kunt in-of uitschakelen.

## <a name="reporting"></a>Rapporten

De BHOLD-rapportage module biedt u de mogelijkheid om gegevens in het-roltype te bekijken via verschillende rapporten. De BHOLD-rapportage module biedt een uitgebreide set ingebouwde rapporten, plus het bevat een wizard die u kunt gebruiken voor het maken van eenvoudige en geavanceerde aangepaste rapporten. Wanneer u een rapport uitvoert, kunt u de resultaten direct weer geven of de resultaten opslaan in een micro soft Excel-bestand (. XLSX). Als u dit bestand wilt weer geven met behulp van micro soft Excel 2000, micro soft Excel 2002 of micro soft Excel 2003, kunt u het Microsoft Office Compatibility Pack voor Word-, Excel-en Power Point-bestands indelingen downloaden en installeren.


U gebruikt de BHOLD-rapportage module meestal om rapporten te maken die zijn gebaseerd op de huidige informatie over de rol. Voor gegenereerde rapporten voor het controleren van de identiteits gegevens heeft Forefront Identity Manager 2010 R2 een rapportage mogelijkheid voor de FIM-service die in de System Center Service Manager Data Warehouse is geïmplementeerd. Zie de documentatie voor Forefront Identity Manager 2010 en Forefront Identity Manager 2010 R2 in de technische bibliotheek voor Forefront Identity Management voor meer informatie over FIM-rapportage.

De volgende categorieën zijn opgenomen in de ingebouwde rapporten:

- Administration
- Attestation
- Besturingselementen
- Access Control naar binnen
- Logboekregistratie
- Model
- Autoriteiten
- Werkstroom

U kunt rapporten maken en toevoegen aan deze categorieën, maar u kunt ook uw eigen categorieën definiëren waarin u aangepaste en ingebouwde rapporten kunt plaatsen.

Wanneer u een rapport maakt, wordt u door de wizard stapsgewijs door de volgende para meters geleverd:

- Identificatie gegevens, zoals naam, beschrijving, categorie, gebruik en doel groep
- Velden die in het rapport moeten worden weer gegeven
- Query's die opgeven welke items moeten worden geanalyseerd
- Volg orde waarin rijen moeten worden gesorteerd
- Velden die worden gebruikt om het rapport in secties te splitsen
- Filters om de geretourneerde elementen in het rapport te verfijnen

In elke fase van de wizard kunt u een voor beeld van het rapport bekijken zoals u dit tot nu toe hebt gedefinieerd en dit bestand opslaat als u geen aanvullende para meters hoeft op te geven. U kunt ook teruggaan naar de vorige stappen om para meters die u eerder in de wizard hebt opgegeven, te wijzigen.

## <a name="access-management-connector"></a>Toegangs beheer connector

De BHOLD Suite Access Management connector-module ondersteunt zowel initiële als actieve synchronisatie van gegevens in BHOLD. De Access Management-connector werkt met de FIM-synchronisatie service om gegevens te verplaatsen tussen de BHOLD Core-Data Base, het MIM-mailverse en de doel toepassingen en identiteits archieven.

Eerdere versies van BHOLD hebben meerdere MAs nodig om de gegevens stroom tussen MIM-en tussenliggende BHOLD-database tabellen te beheren. In BHOLD Suite SP1 kunt u met de Access Management-connector beheer agenten (MAs) in MIM definiëren die rechtstreeks gegevens overdracht tussen BHOLD en MIM bieden.

## <a name="mim-integration"></a>MIM-integratie

Een belang rijke en krachtige functie van Forefront Identity Manager 2010 en Forefront Identity Manager 2010 R2 is de Self-Service Portal waarmee eind gebruikers hun identiteits-en lidmaatschaps gegevens kunnen weer geven en beheren. MIM-integratie breidt de MIM-Portal uit met Self-Service Role Management. Met de BHOLD-functies in de MIM-Portal kan een gebruiker bijvoorbeeld roltoewijzing aanvragen en actieve rollen en in behandeling zijnde aanvragen weer geven. Er kunnen aanvullende mogelijkheden worden verleend aan gedelegeerde beheerders, zoals de mogelijkheid om roltoewijzingen aan te vragen voor andere gebruikers.

Het is belang rijk te weten dat de BHOLD-uitbrei dingen van de MIM-Portal selfservice rollen en werk stroom beheer ondersteunen en rapportage. Andere BHOLD-beheer functies, evenals Attestation, worden aangeboden door de Webportals van de BHOLD-modules, die onafhankelijk van de MIM-Portal worden gehost.

## <a name="next-steps"></a>Volgende stappen

- [Installatie handleiding voor BHOLD](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
