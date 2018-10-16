---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: Bekijk het proces om gebruikers te maken in AD DS met Microsoft Identity Manager 2016
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 032f3cb68214dcdb7008b4b771a8b66c05ebe890
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333969"
---
# <a name="how-do-i-provision-users-to-ad-ds"></a>Hoe kan ik gebruikers inrichten voor AD DS?

Van toepassing op: Microsoft Identity Manager 2016 SP1 (MIM)

Een basisvereiste voor een identiteitsbeheersysteem is de mogelijkheid resources te kunnen inrichten voor een extern systeem.

Deze handleiding beschrijft de belangrijkste bouwstenen die betrokken zijn bij het inrichten van gebruikers vanuit Microsoft® Identity Manager (MIM) 2016 voor Active Directory® Domain Services (AD DS). Daarnaast leest u hier hoe u kunt controleren of uw scenario werkt zoals verwacht, welke mogelijkheden er zijn voor het beheren van Active Directory-gebruikers met MIM 2016, en vindt u aanvullende informatiebronnen.

## <a name="before-you-begin"></a>Voordat u begint


In deze sectie vindt u informatie over de reikwijdte van dit document. In het algemeen zijn gebruikshandleidingen bedoeld voor lezers die al enige basiservaring hebben met het synchroniseren van objecten met MIM, zoals behandeld in de aanverwante [handleidingen Aan de slag](http://go.microsoft.com/FWLink/p/?LinkId=190486).

### <a name="audience"></a>Doelgroep


Deze handleiding is bedoeld voor IT-professionals die al een basiskennis hebben van de werking van het MIM-synchronisatieproces en die praktische ervaring en conceptuele kennis over specifieke scenario's willen opdoen.

### <a name="prerequisite-knowledge"></a>Vereiste voorkennis


In dit document wordt ervan uitgegaan dat u een exemplaar van MIM kunt uitvoeren en dat u ervaring hebt met het configureren van eenvoudige synchronisatiescenario's, zoals beschreven in de volgende documenten:

-   [Inleiding tot inkomende synchronisatie](http://go.microsoft.com/FWLink/p/?LinkId=189652)

-   [Inleiding tot uitgaande synchronisatie](http://go.microsoft.com/FWLink/p/?LinkId=189653)

De inhoud van dit document dient als een uitbreiding op deze inleidende documenten.

### <a name="scope"></a>Bereik


Het scenario dat in dit document wordt beschreven is vereenvoudigd om tegemoet te komen aan de vereisten van een technische basisomgeving. Het document is erop gericht de lezer kennis bij te brengen over de hier besproken concepten en technologieën.

Aan de hand hiervan kunt u een oplossing ontwikkelen waarbij u groepen in AD DS gaat beheren met behulp van MIM.

### <a name="time-requirements"></a>Tijdvereisten


Het kost 90 tot 120 minuten om de procedures in dit document te voltooien.

Bij deze schatting wordt ervan uitgegaan dat de testomgeving reeds is geconfigureerd en opgesteld.

### <a name="getting-support"></a>Ondersteuning


Als u vragen hebt over de inhoud van dit document, of als u algemene feedback wilt geven en bespreken, kunt u een bericht sturen naar het [Forefront Identity Manager 2010 forum](http://go.microsoft.com/FWLink/p/?LinkId=189654) (forum van Forefront Identity Manager 2010).

## <a name="scenario-description"></a>Scenariobeschrijving


Het fictieve bedrijf Fabrikam is van plan MIM te gebruiken voor het beheer van de gebruikersaccounts in AD DS van de organisatie. Als onderdeel van dit proces moet Fabrikam gebruikers inrichten voor AD DS. Voor het testen van de initiële opstelling heeft Fabrikam een technische basisomgeving opgezet, bestaande uit MIM en AD DS.
In deze omgeving test Fabrikam een scenario dat bestaat uit een gebruiker die handmatig in de MIM-portal is aangemaakt. In dit scenario wordt de gebruiker als ingeschakelde gebruiker voorzien van een vooraf gedefinieerd wachtwoord voor AD DS.

## <a name="scenario-design"></a>Scenario-ontwerp


Voor het gebruik van deze handleiding hebt u drie architectuurcomponenten nodig:

-   Active Directory-domeincontroller

-   Een computer waarop een FIM-synchronisatieservice wordt uitgevoerd
-   Een computer waarop een FIM-portal wordt uitgevoerd

In de volgende illustratie wordt deze omgeving verduidelijkt.

![Vereiste omgeving](media/how-provision-users-adds/image001.png)


U kunt alle componenten op één computer uitvoeren.

> [!NOTE]
> Zie de [FIM Installation Guide](http://go.microsoft.com/FWLink/p/?LinkId=165845) (FIM-installatiehandleiding) voor meer informatie over het instellen van MIM.

## <a name="scenario-components-list"></a>Lijst met componenten voor het scenario


In de volgende lijst vindt u de componenten die onderdeel uitmaken van het scenario in deze handleiding.

| ![Organisatie-eenheid](media/how-provision-users-adds/image005.jpg)   | Organisatie-eenheid                | MIM-objecten: een organisatie-eenheid (OE) die wordt gebruikt als doel voor de ingerichte gebruikers.                                                       |
|----------------------------------------|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| ![Gebruikersaccounts](media/how-provision-users-adds/image006.jpg)   | Gebruikersaccounts                      | &#183; **ADMA**: Active Directory-gebruikersaccount met voldoende rechten om verbinding te maken met AD DS.<br/> &#183; **FIMMA**: Active Directory-gebruikersaccount met voldoende rechten om verbinding te maken met MIM.
                                                                 |
| ![Beheeragents en uitvoerprofielen](media/how-provision-users-adds/image007.jpg)  | Beheeragents en uitvoerprofielen | &#183; **Fabrikam ADMA**: beheeragent die gegevens uitwisselt met AD DS. <br/> &#183; Fabrikam FIMMA: beheeragent die gegevens uitwisselt met MIM.                                                                                 |
| ![Synchronisatieregels](media/how-provision-users-adds/image008.jpg)  | Synchronisatieregels              | Regel voor uitgaande synchronisatie van Fabrikam-groep: regel voor uitgaande synchronisatie die gebruikers inricht voor AD DS.                                     |
| ![Sets](media/how-provision-users-adds/image009.jpg)   | Sets                               | Alle contractanten: set met dynamisch lidmaatschap voor alle objecten met een EmployeeType-kenmerkwaarde van Contractant.                                |
| ![Werkstromen](media/how-provision-users-adds/image010.jpg)  | Werkstromen                          | AD-inrichtingswerkstroom: werkstroom om de MIM-gebruiker binnen het bereik te brengen van de AD-regel voor uitgaande synchronisatie.                                |
| ![Regels voor Informatiebeheerbeleid](media/how-provision-users-adds/image011.jpg)   | Regels voor Informatiebeheerbeleid            | Beheerbeleidsregel voor AD-inrichting: beheerbeleidsregel (MPR) die wordt getriggerd als een resource lid wordt van de set Alle contractanten. |
| ![MIM-gebruikers](media/how-provision-users-adds/image012.jpg) | MIM-gebruikers                          | Julia Steen: MIM-gebruiker die u inricht voor AD DS.                                                                                             |



## <a name="scenario-steps"></a>Scenariostappen


Het scenario dat in deze handleiding wordt beschreven, bestaat uit de bouwstenen die in de volgende afbeelding worden getoond.

![Scenariostappen](media/how-provision-users-adds/image013.png)


## <a name="configuring-the-external-systems"></a>Externe systemen configureren


In deze sectie vindt u instructies voor de resources die u moet maken en die buiten de MIM-omgeving aanwezig zijn.

### <a name="step-1-create-the-ou"></a>Stap 1: de OE maken


De OE dient als container voor de ingerichte voorbeeldgebruiker. Zie [Create a New Organizational Unit](http://go.microsoft.com/FWLink/p/?LinkId=189655) (Een nieuwe organisatie-eenheid maken) voor meer informatie.

Maak in de AD DS een OE met de naam MIMObjects.

### <a name="step-2-create-the-active-directory-user-accounts"></a>Stap 2: Active Directory-gebruikersaccounts maken

Voor het scenario in deze handleiding hebt u twee Active Directory-gebruikersaccounts nodig:

- **ADMA**: gebruikt door de Active Directory-beheeragent.

- **FIMMA**: gebruikt door de FIM-servicebeheeragent.

In beide gevallen volstaan normale gebruikersaccounts. Verderop in dit document vindt u meer informatie over de specifieke vereisten voor beide accounts. Zie [Create a New User Account](http://go.microsoft.com/FWLink/p/?LinkId=189656) (Een nieuw gebruikersaccount maken) voor meer informatie over het maken van gebruikers.


## <a name="configuring-the-fim-synchronization-service"></a>De FIM-synchronisatieservice configureren


Voor de configuratiestappen in dit document, moet u FIM Synchronization Service Manager starten.

### <a name="creating-the-management-agents"></a>De beheeragents maken

Voor het scenario in deze handleiding moet u twee beheeragents maken:

-   **Fabrikam ADMA**: beheeragent voor AD DS.

-   **Fabrikam FIMMA**: beheeragent voor de FIM-servicebeheeragent.

### <a name="step-3-create-the-fabrikam-adma-management-agent"></a>Stap 3: de Fabrikam ADMA-beheeragent maken

Als u een beheeragent configureert voor AD DS, moet u een account opgeven dat wordt gebruikt door de beheeragent in de gegevensuitwisseling met AD DS. Gebruik een normaal gebruikersaccount. Maar als u gegevens importeert vanuit AD DS, moet het account rechten hebben om wijzigingen vanuit het DirSync-besturingselement te peilen. Als u de beheeragent gegevens naar AD DS wilt laten exporteren, moet u het account voldoende rechten geven op de doel-OE's. Zie [Configuring the ADMA Account](http://go.microsoft.com/FWLink/p/?LinkId=189657) (Het ADMA-account configureren) voor meer informatie.

Als u een gebruiker in AD DS wilt maken, dient u de DN van het object uit te laten stromen. Daarnaast is het een goed gebruik om de voornaam, achternaam en weergavenaam te laten stromen, zodat u zeker weet dat uw objecten te vinden zijn.

In AD DS maken gebruikers nog steeds gebruik van het kenmerk sAMAccountName om zich aan te melden bij de adreslijstservice. Als u geen waarde voor dit kenmerk opgeeft, genereert de adreslijstservice er een willekeurige waarde voor. De willekeurige waarden zijn echter niet gebruikersvriendelijk. Daarom maakt een gebruikersvriendelijke versie van dit kenmerk gewoonlijk deel uit van een export naar AD DS. Gebruikers kunnen zich pas aanmelden bij AD DS, als u een wachtwoord opneemt dat is gemaakt met behulp van het kenmerk unicodePwd in uw exportlogica.

> [!Note]
> Zorg ervoor dat de waarde die u opgeeft als unicodePwd in overeenstemming is met het wachtwoordbeleid van de doel-AD DS.

Als u een wachtwoord instelt voor AD DS-accounts, moet u ook een account maken als een ingeschakeld account. Stel hiervoor het kenmerk userAccountControl in. Zie [Using FIM to Enable or Disable Accounts in Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189658) (FIM gebruiken voor het in- of uitschakelen van accounts in Active Directory) voor meer informatie over het kenmerk userAccountControl.

In de volgende tabel worden de belangrijkste scenariospecifieke instellingen vermeld die u moet configureren.

| Ontwerppagina voor beheeragenten                          | Configuration                                                  |
|---------------------------------------------------------|----------------------------------------------------------------|
| Beheeragent maken                                 | 1. **Beheeragent voor:** AD DS  <br/> 2.  **Naam:** Fabrikam ADMA |
| Verbinding maken met een Active Directory-forest                      | 1. **Selecteer mappartities:** “DC=Fabrikam,DC=com”   <br/>   2. Klik op **Containers** om het dialoogvenster **Select Containers** te openen om te controleren dat **MIMObjects** de enige geselecteerde OE is.        |
| Objecttypen selecteren                                     | Naast de reeds geselecteerde objecttypen selecteert u **gebruiker.** |
| Kenmerken selecteren                                       | 1. Klik op **Alles weergeven.** <br/>   2. Selecteer de volgende kenmerken: <br/> &nbsp;&nbsp;&nbsp;&#176; **displayName** <br/> &nbsp;&nbsp;&nbsp;&#176; **givenName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **sn** <br/> &nbsp;&nbsp;&nbsp;&#176;  **SamAccountName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **unicodePwd** <br/> &nbsp;&nbsp;&nbsp;&#176;  **userAccountControl**     

Zie de volgende onderwerpen in de Help voor meer informatie:
- Een beheeragent maken
- Verbinding maken met een Active Directory-forest
- De beheeragent voor Active Directory gebruiken
- Mappartities configureren

> [!Note]
> Zorg dat u een importkenmerkstroomregel hebt geconfigureerd voor het kenmerk ExpectedRulesList.

### <a name="step-4-create-the-fabrikam-fimma-management-agent"></a>Stap 4: de Fabrikam FIMMA-beheeragent maken

Als u een FIM-servicebeheeragent configureert, moet u een account opgeven dat wordt gebruikt door de beheeragent in de gegevensuitwisseling met de FIM-service.

Gebruik een normaal gebruikersaccount. Het account moet hetzelfde zijn als het account dat u tijdens de installatie van MIM hebt opgegeven. Zie [Using Windows PowerShell to Do a FIMMA Account Configuration Quick Test](http://go.microsoft.com/FWLink/p/?LinkId=189659) (Windows PowerShell gebruiken voor het uitvoeren van een snelle test van de FIMMA-accountconfiguratie) voor een script dat u kunt gebruiken om de naam vast te stellen van het FIMMA-account dat u tijdens de installatie hebt opgegeven en om te testen of het account nog geldig is.

In de volgende tabel worden de belangrijkste scenariospecifieke instellingen vermeld die u moet configureren. Maak de beheeragent op basis van de informatie in de onderstaande tabel.  

| Ontwerppagina voor beheeragenten | Configuration |
|------------|------------------------------------|
| Beheeragent maken | 1. **Beheeragent voor:** FIM-servicebeheerAgent <br/> 2. **Naam:** Fabrikam FIMMA |
| Verbinding maken met database     | Gebruik de volgende instellingen: <br/> &#183; **Server:** localhost <br/> &#183; **Database:** FIMService <br/> &#183;**Basisadres FIM-Service:** http://localhost:5725 <br/> <br/> Geef de informatie op over het account dat u voor deze beheeragent hebt gemaakt. |
| Objecttypen selecteren                                     | Naast de reeds geselecteerde objecttypen selecteert u **Persoon.**   |
| Objecttypetoewijzingen configureren                          | Voeg naast de al bestaande objecttypetoewijzingen een toewijzing toe voor het **Gegevensbronobjecttype** Persoon in het **Metaverseobjecttype** Persoon. |
| Kenmerkstroom configureren                                | Voeg naast de al bestaande kenmerkstroomtoewijzingen de volgende kenmerkstroomtoewijzingen toe: <br/><br/> ![Kenmerkstroom](media/how-provision-users-adds/image018.jpg) |




Zie de volgende onderwerpen in de Help voor meer informatie:
-   Een beheeragent maken

-   Verbinding maken met een Active Directory-database

-   De beheeragent voor Active Directory gebruiken

-   Mappartities configureren

> [!NOTE]
>  Zorg dat u een importkenmerkstroomregel hebt geconfigureerd voor het kenmerk ExpectedRulesList.

### <a name="step-5-create-the-run-profiles"></a>Stap 5: de uitvoerprofielen maken

In de volgende tabel worden de uitvoerprofielen vermeld die u moet maken voor het scenario in deze handleiding.

| Beheeragent  | Uitvoerprofiel     |
|-------------------|--------------------------------------|
| Fabrikam ADMA     | 1. Volledig importbewerking  <br/> 2. Volledige synchronisatie <br/> 3. Delta-import <br/> 4. Deltasynchronisatie <br/> 5. Exporteren                                                                    |
| Fabrikam FIMMA   | 1. Volledig importbewerking <br> 2. Volledige synchronisatie <br/> 3. Delta-Import <br/> 4. Deltasynchronisatie <br/> 5. Exporteren|                                                                                                                                                                                   

Maak aan de hand van de vorige tabel uitvoerprofielen voor elke beheeragent.


> [!Note]
> Zie Create a Management Agent Run Profile (Een uitvoerprofiel voor beheeragenten maken) in MIM Help voor meer informatie.                                                                                                                  
> 
> 
> [!Important]
>  Controleer of het inrichten in uw omgeving is ingeschakeld. U kunt dit doen door het uitvoeren van het script, met behulp van Windows PowerShell voor het inrichten van inschakelen (http://go.microsoft.com/FWLink/p/?LinkId=189660).


## <a name="configuring-the-fim-service"></a>De FIM-service configureren


Voor het scenario in deze handleiding moet u een inrichtingsbeleid voor inrichten configureren, zoals in de volgende afbeelding wordt getoond.

![Beleid inrichten](media/how-provision-users-adds/image019.png)

Het doel van dit inrichtingsbeleid is om groepen binnen het bereik te brengen van de regel voor uitgaande synchronisatie van Active Directory-gebruikers. Als u de resource binnen het bereik van de synchronisatieregel hebt gebracht, schakelt u de synchronisatie-engine in om uw resource volgens uw configuratie in te richten voor AD DS.

Voor het configureren van de FIM-Service, gaat u in Windows naar http://localhost/identitymanagement. Als u het inrichtingsbeleid wilt maken, gaat u op de pagina van de MIM-portal naar de gerelateerde pagina's van de sectie Beheer. Verifieer de configuratie door het script uit te voeren in [Using Windows PowerShell to document your provisioning policy configuration](http://go.microsoft.com/FWLink/p/?LinkId=189661) (Windows PowerShell gebruiken om de configuratie voor het inrichtingsbeleid te documenteren).

### <a name="step-6-create-the-synchronization-rule"></a>Stap 6: de synchronisatieregel maken

In de volgende tabellen wordt de configuratie getoond van de vereiste inrichtingssynchronisatieregel van Fabrikam. Maak de synchronisatieregel aan de hand van de gegevens in de volgende tabellen.

| Configuratie van de synchronisatieregel                                                                         |                                                                             |                                                           
|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|
| Naam                                                                                                       | Regel voor uitgaande synchronisatie van Active Directory-gebruiker                         |                                                          
| Beschrijving                                                                                               |                                                                             |                                                           
| Prioriteit                                                                                                | 2                                                                           |                                                           
| Richting gegevensstroom   | Uitgaand             |       
| Afhankelijkheid       |         |                                         


| Bereik |                                                                             |                                                           
|--------|-------|
| Resourcetype voor de metaverse | persoon |                                                         
| Extern systeem                   |Fabrikam ADMA                                                               |                                                       
| Resourcetype voor het externe systeem                                                                              | gebruiker      



| Relatie ||
|------------|---------|
| Resource maken in extern systeem                                                                         | True                                                                        |                                                           
| Ongedaan maken van inrichting inschakelen                                                                                      | False                                                                       |                                                           

| Criteria voor relatie                                                                                      | |
|------------|----------|
| ILM-kenmerk     | Kenmerk van de gegevensbron                                                       |
| Kenmerk van de gegevensbron         | sAMAccountName    |

| Initiële uitgaande kenmerkstromen        | |                                                             |
|-------------------|---------------------- |---------------|
| Null-waarden toestaan                 | Bestemming                                                                 | Bron                                                    |
| onjuist                       | dn                                                                          | \+("CN=",displayName,",OU=MIMObjects,DC=fabrikam,DC=com") |
| onjuist                       | userAccountControl                                                          | **Constant:** 512                                         |
| onjuist                                                                     | unicodePwd                    | Constant: P\@\$\$W0rd                                    |

| Permanente uitgaande kenmerkstromen  |                                                                     |                                                           |
|--------------------------------------|---------------------------------------------------------------------|-----------------------------------------------------------|
| Null-waarden toestaan                                                                                                | Bestemming                                                                 | Bron                                                    |
| onjuist                                                                                                      | sAMAccountName                                                              | accountName                                               |
| onjuist                                                                                                      | displayName                                                                 | displayName                                               |
| onjuist                                                                                                      | givenName                                                                   | firstName                                                 |
| onjuist                                                                                                      | sn                                                                          | lastName                                                  |



> [!NOTE]
>  Belangrijk: Verifieer dat u Alleen initiële stroom hebt geselecteerd voor de kenmerkstroom met de DN als doel.                                                                          

### <a name="step-7-create-the-workflow"></a>Stap 7: De werkstroom maken

Het doel van de AD-inrichtingswerkstroom is om de inrichtingssynchronisatieregel van Fabrikam aan een resource toe te voegen. In de volgende tabellen wordt de configuratie vermeld.  Maak een werkstroom aan de hand van de gegevens in de onderstaande tabellen.

| Werkstroomconfiguratie               |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Naam                                 | Inrichtingswerkstroom van Active Directory-gebruiker                     |
| Beschrijving                          |                                                                 |
| Werkstroomtype                        | Actie                                                          |
| Uitvoeren bij beleidsupdates                 | False                                                           |

| Synchronisatieregel                 |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Naam                                 | Regel voor uitgaande synchronisatie van Active Directory-gebruiker             |
| Actie                               | Toevoegen                                                             |




### <a name="step-8-create-the-mpr"></a>Stap 8: de MPR maken

De vereiste MPR is van het type Setovergang en wordt getriggerd wanneer een resource lid wordt van de set Alle contractanten. In de volgende tabellen wordt de configuratie vermeld.  Maak een MPR aan de hand van de gegevens in de onderstaande tabellen.

| MPR-configuratie                    |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Naam                                 | Beheerbeleidsregel voor inrichting van AD-gebruikers                 |
| Beschrijving                          |                                                             |
| Type                                 | Setovergang                                              |
| Verleent toestemmingen                   | False                                                       |
| Uitgeschakeld                             | False                                                       |

| Definitie overgang                |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Overgangstype                      | Overgang in                                               |
| Overgangsset                       | Alle contractanten                                             |

| Beleidswerkstromen                     |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Type                                 | Actie                                                      |
| Weergavenaam                         | Inrichtingswerkstroom van Active Directory-gebruiker                 |




## <a name="initializing-your-environment"></a>De omgeving initialiseren


De doelstelling van de initialisatiefase is als volgt:

-   Breng de synchronisatieregel over naar de metaverse.

-   Breng de Active Directory-structuur over naar het Active Directory-connectorgebied.

### <a name="step-9-run-the-run-profiles"></a>Stap 9: de uitvoerprofielen uitvoeren

In de volgende tabellen worden de uitvoerprofielen vermeld die onderdeel zijn van de initialisatiefase.  Voer de uitvoerprofielen uit aan de hand van de onderstaande tabel.

| Uitvoeren                                                                                                           | Beheeragent                                      | Uitvoerprofiel          |
|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|----------------------|
| 1                                                                                                             | Fabrikam FIMMA                                        | Volledig importbewerking          |
| 2                                                                                                             |                                                       | Volledige synchronisatie |
| 3                                                                                                             |                                                       | Exporteren               |
| 4                                                                                                             |                                                       | Delta-import         |
|                                                                                                               |                                                       |                      |
| 5                                                                                                             | Fabrikam ADMA                                         | Volledig importbewerking          |
| 6                                                                                                             |                                                       | Volledige synchronisatie |



> [!NOTE]
> Controleer of de regel voor uitgaande synchronisatie is geprojecteerd in de metaverse.

## <a name="testing-the-configuration"></a>De configuratie testen


Het doel van deze sectie is het testen van de feitelijke configuratie. Voor het testen van de configuratie dient u het volgende te doen:

1.  Maak een voorbeeldgebruiker in de FIM-portal.

2.  Controleer de vereisten voor het inrichten van de voorbeeldgebruiker.

3.  Richt de voorbeeldgebruiker in voor AD DS.

4.  Controleer of de gebruiker aanwezig is in AD DS.

### <a name="step-10-create-a-sample-user-in-mim"></a>Stap 10: een voorbeeldgebruiker maken in MIM


In de volgende tabel worden de eigenschappen van de voorbeeldgebruiker vermeld. Maak een voorbeeldgebruiker aan de hand van de gegevens in de onderstaande tabel.

| Kenmerk                              | Waarde                                                          |
|----------------------------------------|----------------------------------------------------------------|
| Voornaam                             | Julia                                                         |
| Achternaam                              | Steen                                                          |
| Weergavenaam                           | Julia Steen                                                   |
| Accountnaam                           | JSteen                                                         |
| Domein                                 | Fabrikam                                                       |
| Werknemerstype                          | Contractant                                                     |



### <a name="verify-the-provisioning-requisites-of-the-sample-user"></a>De vereisten controleren voor het inrichten van de voorbeeldgebruiker


Als u de voorbeeldgebruiker wilt inrichten voor AD DS, moet aan twee voorwaarden worden voldaan:

1.  De gebruiker moet lid zijn van de set Alle contractanten.

2.  De gebruiker van de set moet binnen het bereik zijn van de regel voor uitgaande synchronisatie.

### <a name="step-11-verify-the-user-is-a-member-of-all-contractors"></a>Stap 11: controleren of de gebruiker lid is van Alle contractanten

Als u wilt controleren of de gebruiker lid is van de set Alle contractanten, opent u de set en klikt u op Leden weergeven.

![Controleren of de gebruiker lid is van alle contractanten](media/how-provision-users-adds/image022.jpg)


### <a name="step-12-verify-the-user-is-in-the-scope-of-the-outbound-synchronization-rule"></a>Stap 12: controleren of de gebruiker binnen het bereik is van de regel voor uitgaande synchronisatie

Als u wilt controleren of de gebruiker binnen het bereik is van de regel voor uitgaande synchronisatie, opent u de eigenschappenpagina van de gebruiker en controleert u het kenmerk Lijst met verwachte regels op het tabblad Inrichten. Het kenmerk Lijst met verwachte regels moet de AD-gebruiker vermelden

Regel voor uitgaande synchronisatie. In de volgende schermopname wordt een voorbeeld getoond van het kenmerk Lijst met verwachte regels.

![Status van synchronisatieregel](media/how-provision-users-adds/image023.jpg)

Op dit punt in het proces is de status van de synchronisatieregel In wachtrij. Dat betekent dat de synchronisatieregel nog niet op de gebruiker wordt toegepast.



### <a name="step-13-synchronize-the-sample-group"></a>Stap 13: de voorbeeldgroep synchroniseren


Voordat u met de eerste synchronisatiecyclus voor een testobject begint, dient u de verwachte toestand van het object te volgen na het uitvoeren van een uitvoerprofiel in een testschema. Het testschema moet naast de algemene toestand van het object (gemaakt, bijgewerkt of verwijderd) ook de kenmerkwaarden bevatten die u verwacht.
Gebruik het testschema om de verwachtingen voor het testschema te controleren. Als na een stap de verwachte resultaten niet worden geretourneerd, gaat u pas met de volgende stap door als u de afwijking tussen het verwachte resultaat en het feitelijke resultaat hebt opgelost.

U kunt de synchronisatiestatistieken gebruiken als een eerste indicatie bij het controleren van uw verwachtingen. Als u bijvoorbeeld verwacht dat nieuwe objecten gefaseerd in een connectorgebied worden binnengebracht, maar de importstatistieken retourneren geen 'toevoegingen', dan is er duidelijk iets in de omgeving dat niet volgens verwachting werkt.

![Synchronisatiestatistieken](media/how-provision-users-adds/image024.jpg)

Hoewel de synchronisatiestatistieken een eerste indicatie kunnen zijn van het feit of de synchronisatie volgens de verwachting functioneert, moet u de functies Search Connector Space en Metaverse Search van Synchronization Service Manager gebruiken om de verwachte kenmerkwaarden te controleren.

Ga als volgt te werk om de gebruiker met AD DS te synchroniseren:

1.  Importeer de gebruiker in het FIM MA-connectorgebied.

2.  Projecteer de gebruiker in de metaverse.

3.  Richt de gebruiker in voor het Active Directory-connectorgebied.

4.  Exporteer statusgegevens naar FIM.

5.  Exporteer de gebruiker naar AD DS.

6.  Controleer of de gebruiker is gemaakt.

Voer de volgende uitvoerprofielen uit als u deze taken wilt verrichten.

| Beheeragent | Uitvoerprofiel  |
|------------------|--------------|
| Fabrikam FIMMA   | 1. Delta-import <br/> 2. Deltasynchronisatie <br/> 3. Exporteren <br/> 4. Delta-import |
| Fabrikam FIMMA   | 1. Exporteren <br/> 2. Delta-Import       |


Na het importeren van de FIM-servicedatabase, Britta Simon en het object ExpectedRuleEntry, dat koppelingen Julia op de regel voor uitgaande synchronisatie van AD-gebruiker gefaseerd binnengebracht in het Fabrikam FIMMA-connectorgebied. Bij het controleren van Julia in het connectorgebied controleert, naast de kenmerkwaarden die u hebt geconfigureerd in de FIM-Portal, vindt u ook een geldige verwijzing naar het object verwachte-Regelvermelding. In de volgende schermopname ziet u hier een voorbeeld van.

![Eigenschappen van het object van het connectorgebied](media/how-provision-users-adds/image025.jpg)

Het doel van de deltasynchronisatie die op Fabrikam FIMMA wordt uitgevoerd, bestaat uit het uitvoeren van een aantal bewerkingen:

-   Projectie: het nieuwe gebruikersobject en het gerelateerde object Verwachte-regelvermelding worden in de metaverse geprojecteerd.

-   Inrichting: het recent geprojecteerde object, Julia Steen, wordt ingericht in het connectorgebied van Fabrikam ADMA.

-   Exportkenmerkstromen: bij beide beheeragents treden exportkenmerkstromen op. Op de Fabrikam ADMA wordt het recent ingerichte object, Julia Steen, ingevuld met de nieuwe kenmerkwaarden. Op de Fabrikam IMMA worden het bestaande object, Julia Steen, en het gerelateerde object, ExpectedRuleEntry, bijgewerkt met kenmerkwaarden die het resultaat zijn van de projectie.

![Synchronisatiestatistieken](media/how-provision-users-adds/image026.jpg)

Zoals reeds aangegeven door de synchronisatiestatistieken, heeft er een inrichtingsactiviteit plaatsgevonden voor het connectorgebied van Fabrikam ADMA. Als de eigenschappen van het metaverseobject van Julia Steen worden bekeken, ziet u dat deze activiteit het resultaat is van het feit dat het kenmerk ExpectedRulesList is ingevuld met een geldige verwijzing.

![eigenschappen van het metaverseobject](media/how-provision-users-adds/image027.jpg)

Tijdens de volgende exportbewerking op Fabrikam FIMMA wordt de synchronisatieregelstatus van Julia Steen bijgewerkt van In wachtrij naar Toegepast. Dit geeft aan dat de regel voor uitgaande synchronisatie nu actief is voor het object in de metaverse.

![Regel voor toegepaste synchronisatie](media/how-provision-users-adds/image028.jpg)

Aangezien er een nieuw object voor het ADMA-connectorgebied is ingericht, staat er één exportbewerking in de wachtrij om aan deze beheeragent te worden toegevoegd (Add). Door een voor dit doel gemaakt script te gebruiken, ziet u één gerapporteerde exportbewerking in de wachtrij om te worden toegevoegd (Add). Zie [Using Windows PowerShell to Display the Export State of a Management Agent](http://go.microsoft.com/FWLink/p/?LinkId=189664) (Windows PowerShell gebruiken om de exportstatus van een beheeragent weer te geven) als u het script wilt gebruiken.

![Exportbewerkingen voor beheeragent in wachtrij](media/how-provision-users-adds/image029.jpg)

In FIM moet elke exportbewerking worden gevolgd door een delta-importbewerking om de exportbewerking te voltooien. De delta-importbewerking die u uitvoert nadat een exportbewerking is uitgevoerd, staat bekend als een bevestigende importbewerking. Bevestigende importbewerkingen zijn vereist om de FIM-synchronisatieservice in te schakelen voor het maken van passende bijwerkvereisten tijdens opvolgende synchronisaties.


Voer het uitvoerprofiel uit volgens de instructies in deze sectie.

> [!IMPORTANT]
> Elk uitvoerprofiel moet zonder fouten worden voltooid.

### <a name="step-14-verify-the-provisioned-user-in-ad-ds"></a>Stap 14: de ingerichte gebruiker in AD DS inrichten

Open de OE FIMObjects om te controleren dat de voorbeeldgebruiker is ingericht voor AD DS. Julia Steen moet zich in de OE FIMObjects bevinden.

![de gebruiker verifiëren in de OE FIMObjects](media/how-provision-users-adds/image033.jpg)

<a name="summary"></a>Samenvatting
=======

Het doel van dit document is u een inleiding te geven tot de belangrijkste bouwstenen voor het synchroniseren van een gebruiker in MIM met AD DS. Voor de initiële test moet u beginnen met het minimaal aantal vereiste kenmerken om een taak te voltooien. U voegt meer kenmerken aan het scenario toe als de algemene stappen werken zoals verwacht. Door de complexiteit tot een minimum te beperken, wordt het oplossen van problemen vereenvoudigd.

Als u de configuratie test, zult u waarschijnlijk nieuwe testobjecten verwijderen en opnieuw maken. Voor objecten met een

ingevuld kenmerk ExpectedRulesList, kan dit resulteren in zwevende ERE-objecten.
Zie [A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment](http://go.microsoft.com/FWLink/p/?LinkId=189667) (Een methode voor het verwijderen van zwevende ExpectedRuleEntry-objecten uit uw omgeving) voor een beschrijving van het verwijderen van deze objecten uit uw testomgeving.

In een typisch synchronisatiescenario met AD DS als synchronisatiedoel, is MIM niet gezaghebbend voor alle kenmerken van een object. Als u bijvoorbeeld gebruikersobjecten in AD DS beheert door middel van FIM, moeten ten minste het domein en de objectSID-kenmerken worden bijgedragen door de AD DS-beheeragent.
De accountnaam, het domein en de objectSID-kenmerken zijn vereist als u een gebruiker in staat wil stellen zich aan te melden bij de FIM-portal. Als u deze kenmerken vanuit AD DS wilt invullen, is er een aanvullende regel voor inkomende synchronisatie voor het AD DS-connectorgebied vereist. Als u objecten met meerdere bronnen voor kenmerkwaarden beheert, moet u ervoor zorgen dat de prioriteit van de kenmerkstroom correct wordt geconfigureerd. Als de prioriteit van de kenmerkstroom niet correct wordt geconfigureerd, blokkeert de synchronisatie-engine het invullen van kenmerkwaarden. In het artikel [Prioriteit kenmerkstroom](http://go.microsoft.com/FWLink/p/?LinkId=189675) vindt u meer informatie over prioriteit van de kenmerkstroom.

<a name="see-also"></a>Zie ook
=========

<a name="other-resources"></a>Andere resources
---------------

[Using FIM to Enable or Disable Accounts in Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189670) (FIM gebruiken voor het in- of uitschakelen van accounts in Active Directory)

[About Reference Attributes](http://go.microsoft.com/FWLink/p/?LinkId=189671) (Verwijzingskenmerken)

[How Can I Manage My FIM MA Account](http://go.microsoft.com/FWLink/p/?LinkId=189672) (Mijn FIM MA-account beheren)

[Detecting Nonauthoritative Accounts – Part 1: Envisioning](http://go.microsoft.com/FWLink/p/?LinkId=189673) (Niet-gezaghebbende accounts detecteren. Deel 1: planning)

[The Poor Man’s Version of a Connector Detection Mechanism](http://go.microsoft.com/FWLink/p/?LinkId=189674) (De uitgeklede versie van een mechanisme voor het detecteren van connectors)

[Configuring the ADMA Account](http://go.microsoft.com/FWLink/p/?LinkId=189657) (Het ADMA-account configureren)

[A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment](http://go.microsoft.com/FWLink/p/?LinkId=189667) (Een methode voor het verwijderen van zwevende ExpectedRuleEntry-objecten uit uw omgeving)

[About Attribute Flow Precedence](http://go.microsoft.com/FWLink/p/?LinkId=189675) (Prioriteit kenmerkstroom)

[About Exports](http://go.microsoft.com/FWLink/p/?LinkId=189676) (Exportbewerkingen)
