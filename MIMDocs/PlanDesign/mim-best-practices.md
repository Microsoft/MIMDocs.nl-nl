---
title: Aanbevolen procedures voor Microsoft Identity Manager 2016| Microsoft Docs
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/11/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: a0d00c7e5d99e43d3fb0b3011a3851f7194bfdf2
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="microsoft-identity-manager-2016-best-practices"></a>Aanbevolen procedures voor Microsoft Identity Manager 2016

In dit onderwerp worden de aanbevolen procedures beschreven voor het implementeren van en werken met Microsoft Identity Manager 2016 (MIM)

## <a name="sql-setup"></a>Installatie van SQL
>[!NOTE]
In de volgende aanbevelingen voor het instellen van een server met SQL wordt uitgegaan van een SQL-exemplaar toegewezen aan de FIMService en een SQL-exemplaar toegewezen aan de FIMSynchronizationService-database. Als u de FIMService in een geconsolideerde omgeving uitvoert, brengt u wijzigingen aan die geschikt zijn voor uw configuratie.

Configuratie van de SQL-server (Structured Query Language) is essentieel voor optimale systeemprestaties. Het bereiken van optimale MIM-prestaties in grootschalige implementaties is afhankelijk van de toepassing van aanbevolen procedures voor een server waarop SQL wordt uitgevoerd. Zie de volgende onderwerpen voor meer informatie over aanbevolen procedures voor SQL:

-   [Top 10 van aanbevolen procedures voor opslag](http://go.microsoft.com/fwlink/?LinkID=183663)

-   [Tempdb-prestaties optimaliseren](http://go.microsoft.com/fwlink/?LinkID=188267)

-   [Aanbevolen procedures voor SQL-servers](http://go.microsoft.com/fwlink/?LinkID=188268)

-   [Indexen opnieuw indelen en opbouwen](http://go.microsoft.com/fwlink/?LinkID=188269)

### <a name="presize-data-and-log-files"></a>Vooraf de grootte bepalen van gegevens en logboekbestanden

Vertrouw niet op autogrow. Beheer in plaats daarvan de groei van deze bestanden handmatig. U kunt autogrow uit veiligheidsoverwegingen aan laten staan, maar u moet de groei van de gegevensbestanden proactief beheren. Zie de [FIM Capacity Planning Guide (Handleiding voor FIM-capaciteitsplanning)](http://go.microsoft.com/fwlink/?LinkID=185246) voor voorbeeldgrootten van de MIM-database.

### <a name="to-presize-sql-data-and-log-files"></a>Vooraf de grootte van SQL-gegevens en -logboekbestanden bepalen

1.  Start SQL Server Management Studio.

2.  Ga naar de database FIMService, klik met de rechtermuisknop op FIMService en klik vervolgens op Eigenschappen.

3.  Op de pagina Bestanden breidt u de databasebestanden naar de vereiste grootte uit.

### <a name="isolate-log-from-data-files"></a>Logboekbestanden van gegevensbestanden isoleren

Volg de aanbevolen procedures voor SQL-servers voor het opslaan van de transactie- en gegevenslogboekbestanden voor de databases op afzonderlijke fysieke schijven.

Aanvullende tempdb-bestanden maken

Voor optimale prestaties wordt aangeraden één gegevensbestand per CPU-kern in het tempdb-bestand te maken.

### <a name="to-create-additional-tempdb-files"></a>Aanvullende tempdb-bestanden maken

1.  Start SQL Server Management Studio.

2.  Ga naar de database tempdb in Systeemdatabases, klik met de rechtermuisknop op tempdb en klik vervolgens op Eigenschappen.

3.  Maak één gegevensbestand voor elke CPU-kern op de pagina Bestanden. Sla de tempdb-gegevensbestanden en -logboekbestanden naar verschillende stations en aandrijfassen op.

### <a name="ensure-adequate-space-for-log-files"></a>Zorgen voor voldoende ruimte voor de logboekbestanden

Het is belangrijk dat u de schijfvereisten van uw herstelmodel weet. De modus voor eenvoudig herstel is mogelijk geschikt voor de initiële systeembelasting om het gebruik van schijfruimte te beperken, maar de gegevens die zijn gemaakt na de meest recente back-up worden blootgesteld aan gegevensverlies. Wanneer u de volledig-herstelmodus gebruikt, moet u schijfgebruik beheren via back-ups. Dit houdt onder meer in dat u regelmatig back-ups moet maken van het transactielogboek om intensief gebruik van de schijfruimte te voorkomen. Zie [Recovery Model Overview (Overzicht van herstelmodellen)](http://go.microsoft.com/fwlink/?LinkID=185370) voor meer informatie.

### <a name="limit-sql-server-memory"></a>SQL-servergeheugen beperken

Afhankelijk van de hoeveelheid geheugen op de SQL-server en als u de SQL-server deelt met andere services (dat wil zeggen MIM 2016-service en MIM 2016-synchronisatieservice), wilt u mogelijk het geheugengebruik van SQL beperken. U kunt dit doen met de volgende procedure.

1.  Start SQL Server Enterprise Manager.

2.  Selecteer Nieuwe query.

3.  Voer de volgende query uit:

  ```SQL
  USE master

  EXEC sp_configure 'show advanced options', 1

  RECONFIGURE WITH OVERRIDE

  USE master

  EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
  WITH OVERRIDE
  ```

  In dit voorbeeld wordt de SQL-server opnieuw geconfigureerd voor het gebruik van niet meer dan 12 gigabyte (GB) aan geheugen.

4.  Controleer de instelling met de volgende query:

  ```SQL
  USE master

  EXEC sp_configure 'max server memory (MB)'--- verify the setting

  USE master

  EXEC sp_configure 'show advanced options', 0

  RECONFIGURE WITH OVERRIDE
  ```

### <a name="backup-and-recovery-configuration"></a>Back-up en herstel configureren

Over het algemeen moet u databaseback-ups uitvoeren in overeenstemming met het back-upbeleid van uw organisatie. Als er geen incrementele logboekback-ups zijn gepland, moet de database worden ingesteld op de eenvoudige herstelmodus. Zorg dat u de gevolgen begrijpt van de verschillende herstelmodellen voordat u uw back-upstrategie implementeert, evenals de schijfruimtevereisten voor deze modellen. Voor het volledig-herstelmodel moet u regelmatig back-ups maken om intensief gebruik van de schijfruimte te voorkomen. Zie [Recovery Model Overview (Overzicht van herstelmodellen)](http://go.microsoft.com/fwlink/?LinkID=185370) en [FIM 2010 Backup and Restore Guide (Handleiding voor back-up en herstel)](http://go.microsoft.com/fwlink/?LinkID=165864).

## <a name="create-a-backup-administrator-account-for-the-fimservice-after-installation"></a>Een Administrator-back-upaccount voor de FIMService maken na de installatie


>[!IMPORTANT]
Leden van de groep FIMService Administrators hebben unieke machtigingen die essentieel zijn voor de werking van uw FIM-implementatie. Als u zich niet kunt aanmelden als onderdeel van de groep Administrators, is de enige oplossing een eerdere back-up van het systeem terug te zetten. Om het risico op deze situatie te beperken, is het raadzaam dat u andere gebruikers aan de groep FIM-administrators toevoegt als onderdeel van uw configuratie na de installatie.

## <a name="fim-service"></a>FIM-service


### <a name="configuring-fim-service-service-exchange-mailbox"></a>Het Exchange-postvak van de FIM-service configureren

Hier volgen de aanbevolen procedures voor het configureren van Microsoft Exchange Server voor het serviceaccount van de MIM 2016-service.

- Configureer het serviceaccount zo dat dit alleen e-mail van interne e-mailadressen kan accepteren. Het postvak van het serviceaccount mag nooit e-mail kunnen ontvangen van externe SMTP-servers.

#### <a name="to-configure-the-service-account"></a>Het serviceaccount configureren

1.  Selecteer het **serviceaccount van de FIM-service** in de Exchange Management Console.

2.  Selecteer Eigenschappen, Instellingen e-mailstroom en selecteer vervolgens **Mail Delivery Restrictions**.

3.  Selecteer **Vereisen dat alle afzenders worden geverifieerd**.

Zie [Beperkingen voor de bezorging van e-mail configureren](http://go.microsoft.com/fwlink/?LinkID=183625) voor meer informatie.

-   Configureer het serviceaccount zodanig dat e-mailberichten groter dan 1 MB worden geweigerd. Volg de aanbevolen procedure voor [het configureren van limieten voor de berichtgrootte](http://go.microsoft.com/fwlink/?LinkID=183626) voor een postvak of een openbare map met e-mail.

-   Stel een postvakopslaglimiet van 5 GB voor het serviceaccount in. Voor optimale resultaten volgt u de aanbevolen procedures [Opslagquota voor een postvak configureren](http://go.microsoft.com/fwlink/?LinkID=156929).

## <a name="mim-portal"></a>MIM-portal


### <a name="disable-sharepoint-indexing"></a>Indexeren van SharePoint uitschakelen

Het is raadzaam om het indexeren van Microsoft Office SharePoint® uit te schakelen. Er zijn geen documenten die moeten worden geïndexeerd en indexering veroorzaakt veel fouten in logboekvermeldingen en mogelijke prestatieproblemen met FIM 2010. Indexeren van SharePoint uitschakelen

1.  Klik op Start op de server die als host fungeert voor de MIM 2016-portal.

2.  Klik op Alle programma's.

3.  Klik in de lijst Alle programma's op Systeembeheer.

4.  Klik onder Systeembeheer op SharePoint Central Administration.

5.  Klik op Bewerkingen op de pagina Centraal beheer.

6.  Op de pagina Bewerkingen onder Globale configuratie klikt u op Timeropdrachtdefinities.

7.  Klik op Vernieuwen van SharePoint Services Search op de pagina Timeropdrachtdefinities.

8.  Klik op Uitschakelen op de pagina Timeropdracht bewerken.

## <a name="mim-2016-initial-data-load"></a>MIM 2016: initiële gegevens laden

In deze sectie vindt u een reeks stappen voor het verhogen van de prestaties bij het initieel laden van gegevens van een extern systeem naar FIM 2010. Een aantal hiervan zijn tijdelijke stappen tijdens de initiële populatie van het systeem en deze moeten na de voltooiing opnieuw worden ingesteld. Dit is een eenmalige bewerking en geen continue synchronisatie.

>[!NOTE]
Zie [How do I Synchronize Users from Active Directory to FIM (Hoe kan ik gebruikers synchroniseren vanuit Active Directory naar FIM)](http://go.microsoft.com/fwlink/?LinkID=188277) in de FIM-documentatie voor meer informatie over het synchroniseren van gebruikers tussen FIM 2010 en Active Directory Domain Services (AD DS).

>[!IMPORTANT]
Zorg ervoor dat u de aanbevolen procedures hebt toegepast die worden beschreven in de sectie SQL Setup van deze handleiding.                                                                                                                                                      |

### <a name="step-1-configure-the-sql-server-for-initial-data-load"></a>Stap 1: De SQL-server configureren voor het initieel laden van gegevens
Wanneer u van plan bent in eerste instantie grote hoeveelheden gegevens te laden, kunt u de tijd die nodig is voor het vullen van de database verkorten door tijdelijk de zoekopdracht in volledige tekst uit te schakelen en weer in te schakelen nadat het exporteren van de MIM 2016 Management Agent (FIM MA) is voltooid.

De zoekopdracht in volledige tekst tijdelijk uitschakelen:

1.  Start SQL Server Management Studio.

2.  Selecteer Nieuwe query.

3.  Voer de volgende SQL-instructies uit:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING =
MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

Het is belangrijk dat u bekend bent met de schijfvereisten voor het herstelmodel van uw SQL server. Afhankelijk van uw back-upschema, kunt u overwegen om met behulp van de modus voor eenvoudig herstel tijdens het initieel laden van het systeem het gebruik van schijfruimte te beperken. U moet echter wel begrijpen welke gevolgen dit kan hebben op het gebied van gegevensverlies.
Wanneer u de volledig-herstelmodus gebruikt, moet u schijfgebruik beheren via back-ups. Dit houdt onder meer in dat u regelmatig back-ups moet maken van het transactielogboek om intensief gebruik van de schijfruimte te voorkomen.

>[!IMPORTANT]
Het niet implementeren van deze procedures kan leiden tot een intensief gebruik van de schijfruimte, wat kan resulteren in onvoldoende schijfruimte. U vindt aanvullende informatie over dit onderwerp in [Recovery Model Overview (Overzicht van herstelmodellen)](http://go.microsoft.com/fwlink/?LinkID=185370). [De FIM Backup and Restore Guide (Handleiding voor back-up en herstel](http://go.microsoft.com/fwlink/?LinkID=165864) bevat aanvullende informatie.

### <a name="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process"></a>Stap 2: De minimaal benodigde MIM-configuratie toepassen tijdens het laadproces

Tijdens het initiële laadproces moet u alleen de minimaal benodigde configuratie toepassen voor uw FIM-configuratie voor uw beheerbeleidsregels (MPR’s) en setdefinities. Nadat het laden van gegevens is voltooid, maakt u de extra sets die vereist zijn voor uw implementatie. Gebruik de instelling Uitvoeren bij beleidsupdates op de actiewerkstromen om dat beleid met terugwerkende kracht op de geladen gegevens toe te passen.

### <a name="step-3-configure-and-populate-the-fim-service-with-external-identity-data"></a>Stap 3: De FIM-service configureren en vullen met externe identiteitsgegevens


Voor deze stap volgt u de procedures beschreven in de Engelstalige handleiding How Do I Synchronize Users from Active Directory

Domain Services to FIM (Hoe kan ik gebruikers synchroniseren uit Active Directory Domain Services naar FIM) om uw systeem te configureren voor en synchroniseren met gebruikers uit Active Directory. Als u groepsgegevens wilt synchroniseren vindt u de procedures hiervoor in de Engelstalige handleiding How Do I Synchronize Groups from Active Directory Domain Services to FIM (Hoe kan ik groepen synchroniseren uit Active Directory Domain Services met FIM).

#### <a name="synchronization-and-export-sequences"></a>Synchronisatie- en exportprocedures

Voor optimale prestaties voert u een exportprocedure uit na een synchronisatieprocedure die resulteert in een groot aantal wachtende exportbewerkingen in een connectorgebied.

Voer dan een import uit op de beheeragent die is gekoppeld aan het betreffende connectorgebied. Wanneer u bijvoorbeeld bij het initieel laden van gegevens synchronisatieprofielen moet uitvoeren op verschillende beheeragenten, moet u een export uitvoeren gevolgd door een delta-import na het uitvoeren van elke afzonderlijke synchronisatie.

Voor elke bronbeheeragent die deel uitmaakt van de initialisatiecyclus, voert u de volgende stappen uit:

1.  Volledige import op een bronbeheeragent.

2.  Volledige synchronisatie op de bronbeheeragent.

3.  Export op alle betrokken doelbeheeragents met gefaseerde exportbewerkingen.

4.  Delta-import op alle betrokken doelbeheeragents met gefaseerde exportbewerkingen.

### <a name="step-4-apply-your-full-mim-configuration"></a>Stap 4: De volledige MIM-configuratie toepassen


Als het initieel laden van gegevens is uitgevoerd, past u de volledige MIM-configuratie toe voor uw implementatie.

Afhankelijk van uw scenario's, kan dit het maken van aanvullende sets, beheerbeleidsregels en werkstromen omvatten. Voor elk beleid dat u met terugwerkende wilt toepassen op alle bestaande objecten in het systeem, gebruikt u de instelling Uitvoeren bij beleidsupdates op actiewerkstromen om die beleidsregels met terugwerkende kracht op de geladen gegevens toe te passen.

### <a name="step-5-reconfigure-sql-to-previous-settings"></a>Stap 5: SQL weer terugzetten op vorige instellingen


Vergeet niet de SQL-instellingen te wijzigen in de normale instellingen. Dit omvat:

-   De zoekopdracht in volledige tekst inschakelen

-   Uw back-upbeleid bijwerken in overeenstemming met het beleid van uw organisatie

Als het initieel laden van gegevens is voltooid, schakelt u zoekopdracht in volledige tekst weer in. Voer de volgende SQL-instructies uit om zoekopdracht in volledige tekst weer in te schakelen:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

Als u naar de eenvoudige herstelmodus moet overschakelen, configureert u uw back-upschema in overeenstemming met het back-upbeleid van uw organisatie. Aanvullende details van FIM-back-upschema's zijn beschikbaar in de [FIM Backup and Restore Guide (Handleiding voor back-up en herstel)](http://go.microsoft.com/fwlink/?LinkID=165864).

## <a name="configuration-migration"></a>Configuratie migreren


### <a name="avoid-changing-display-names"></a>Het wijzigen van weergavenamen vermijden

Voor veel objecttypen, zoals beheerbeleidsregels (MPR’s), gebruikt het script syncproduction.ps1 de weergavenaam als het enige ankerkenmerk tussen de twee systemen. Als gevolg hiervan resulteert een wijziging in een bestaande MPR-weergavenaam in het verwijderen van de bestaande MPR, gevolgd door het maken van een nieuwe MPR. Dit doet zich voor omdat het migratieproces niet kan deelnemen aan beheerbeleidsregels waarvan de samenvoegcriteria zijn gewijzigd. U kunt dit voorkomen door een aangepast kenmerk te verbinden met alle configuratie-objecttypen en dat kenmerk te gebruiken als samenvoegcriterium. Hierdoor kunt u weergavenamen wijzigen zonder het migratieproces te beïnvloeden.

### <a name="avoid-changing-the-content-of-intermediate-files"></a>Het wijzigen van de inhoud van tussenliggende bestanden vermijden

Hoewel de bestandsindeling en API (Application Programming Interface) van de objecten op laag niveau openbaar zijn en bewerkingen worden ondersteund door ontwikkelaars, wordt niet aangeraden de inhoud van de tussenliggende indelingen tijdens de migratie te wijzigen. Het kan echter mogelijk wel nodig zijn om hele ImportObjects te verwijderen uit changes.xml of om zoek- en vervangbewerkingen uit te voeren op pilot.xml om versienummers of DNS-testgegevens (Domain Name System) te vervangen door DNS-productiegegevens.

### <a name="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions"></a>Zorgen dat het versienummer in pilot.xml correct is bij het migreren van andere versies

Hoewel migraties tussen versienummers niet worden aanbevolen of ondersteund, kunt u dit vaak doen door in pilot.xml het testversienummer te vervangen door het productieversienummer. Met name voor de objecten WorkflowDefinition en

ActivityInformationConfiguration moet het versienummer nauwkeurig verwijzen naar werkstroomactiviteiten in de productieomgeving. Wanneer het versienummer niet wordt vervangen, identificeert de cmdlet Compare-FIMConfig verschillen tussen de XOML-kenmerken (Extensible Object Markup Language) op WorkflowDefinitions en migreert het versienummer van de test. Bij een onjuist versienummer is het mogelijk dat de productie-FIM-service geen werkstroomactiviteiten kan starten.

### <a name="avoid-cyclic-references"></a>Cyclische verwijzingen voorkomen

Over het algemeen worden cyclische verwijzingen niet aangeraden in een MIM-configuratie.
Er kunnen echter cycli optreden wanneer set A naar set B verwijst en set B ook naar set A verwijst. U kunt dergelijke cyclische verwijzingen voorkomen door de definitie van set A en set B zo aan te passen dat beide niet naar elkaar verwijzen. Start dan het migratieproces opnieuw. Als er wel sprake is van cyclische verwijzingen en de cmdlet Compare-FIMConfig hierdoor resulteert in een fout, moet de cyclus handmatig worden doorbroken. Omdat de cmdlet Compare-FIMConfig een lijst met wijzigingen uitvoert in volgorde van prioriteit, mogen er geen cycli bestaan tussen de verwijzingen van configuratieobjecten.

## <a name="security"></a>Beveiliging

### <a name="mim-ma-account"></a>MIM MA-account

Het MIM MA-account wordt niet beschouwd als een serviceaccount en moet een gewoon gebruikersaccount zijn. Met de accounts moet lokaal kunnen worden aangemeld om te zorgen dat de FIM-synchronisatieservice deze kan imiteren.

Het MIM MA-account inschakelen voor lokale aanmelding

1.  Klik achtereenvolgens op Start, Systeembeheer en Lokaal beveiligingsbeleid.

2.  Open het knooppunt Lokaal beleid en klik op Toewijzing van gebruikersrecht.

3.  Controleer of het FIM MA-account expliciet is opgegeven in het beleid Lokaal aanmelden toestaan of voeg het toe aan een van de groepen die al toegang hebben.

### <a name="fim-synchronization-service-and-fim-services-accounts"></a>Accounts voor FIM-synchronisatieservice en FIM-services

Wanneer u de servers waarop de onderdelen voor de MIM-server worden uitgevoerd veilig wilt configureren, moeten de serviceaccounts worden beperkt. Met de bovenstaande procedure voor het inschakelen van het MIM MA-account stelt u de volgende beperkingen in voor de accounts van de FIM-synchronisatieservice en FIM-service:

-   Aanmelden als batchtaak weigeren

-   Lokaal aanmelden weigeren

-   Toegang tot deze computer vanaf het netwerk weigeren

De serviceaccounts mogen geen lid zijn van de lokale groep beheerders.

Het serviceaccount van de FIM-synchronisatieservice mag geen lid zijn van de beveiligingsgroepen die worden gebruikt voor het beheren van toegang tot de FIM-synchronisatieservice (groepen die beginnen met FIMSync, bijvoorbeeld FIMSyncAdmins, enzovoort).

>[!IMPORTANT]
 Als u de opties selecteert voor het gebruik van hetzelfde account voor beide serviceaccounts en u de FIM-service en de FIM-synchronisatieservice scheidt, kunt u niet Toegang tot deze computer vanaf het netwerk weigeren instellen op de server voor de MMS-synchronisatieservice. Als de toegang wordt geweigerd, kan de FIM-service geen verbinding maken met de met de FIM-synchronisatieservice om de configuratie te wijzigen en wachtwoorden te beheren.

### <a name="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile"></a>Voor wachtwoordherstel op computers bedoeld als een kiosk, moet de lokale beveiliging zijn ingesteld voor het wissen van het wisselbestand voor virtueel geheugen

Bij het implementeren van FIM-wachtwoordherstel op een werkstation bedoeld als een kiosk is het raadzaam om Afsluiten: wisselbestand voor virtueel geheugen wissen in te schakelen om ervoor te zorgen dat gevoelige informatie in het procesgeheugen niet beschikbaar is voor onbevoegde gebruikers.

### <a name="implementing-ssl-for-the-fim-portal"></a>SSL implementeren voor de FIM-portal

Het is zeer raadzaam SSL (Secure Sockets Layer) te gebruiken op de FIM-portalserver om het verkeer tussen de clients en de server te beveiligen.

SSL implementeren:

1.  Open IIS-beheer op de MIM-portalserver.

2.  Klik op de naam van de lokale computer.

3.  Klik op Servercertificaten.

4.  Klik op Certificaataanvraag maken.

5.  Voer in het tekstvak Algemene naam de naam in van de server.

6.  Klik op Volgende en nogmaals op Volgende.

7.  Sla het bestand op een willekeurige locatie op. Voor de volgende stappen moet u toegang hebben tot deze locatie.

8.  In Windows Internet Explorer® bladert u naar https://servernaam/certsrv. Vervang servernaam door de naam van de server die certificaten uitgeeft.

9.  Klik op Request a new Certificate.

10. Klik op Submit an Advanced Request.

11. Klik op Submit a Certificate Request by using a base-64-encoded.

12. Plak de inhoud van het bestand dat u in de vorige stap hebt opgeslagen.

13. Selecteer Webserver in Certificaatsjabloon.

14. Klik op Verzenden.

15. Sla het certificaat op het bureaublad op.

16. Klik op Complete Certification Request in IIS-beheer.

17. Verwijs IIS-beheer naar het certificaat dat u zojuist hebt opgeslagen op het bureaublad.

18. Typ de naam van de server bij Beschrijvende naam.

19. Klik op Sites en selecteer vervolgens de SharePoint – 80.

20. Klik op Bindingen en vervolgens op Toevoegen.

21. Selecteer https.

22. Selecteer voor certificaat de optie die dezelfde naam heeft als de server heeft (dit is het certificaat dat u zojuist hebt geïmporteerd).

23. Klik op OK.

24. Verwijder de HTTP-binding.

25. Klik op de SSL-instellingen en selecteer vervolgens SSL vereist.

26. Sla de instellingen op.

27. Klik achtereenvolgens op Start, Systeembeheer en SharePoint 3.0 Central Administration.

28. Klik op Bewerkingen en vervolgens op Alternatieve toewijzingen voor toegang.

29. Klik op http://servernaam.

30. Wijzig http://servernaam in https://servernaam en klik op OK.

31. Klik op Start, klik op Uitvoeren, typ iisreset en klik vervolgens op OK.

## <a name="performance"></a>Prestatie

Voor het configureren van optimale prestaties:

-   Pas de aanbevolen procedures voor SQL-instellingen toe zoals beschreven in de sectie SQL-instellingen in dit document.

-   Schakel het indexeren van SharePoint op de site FIM 2010 R2-portal uit. Zie voor meer informatie de sectie Indexering van SharePoint uitschakelen in dit document.

## <a name="feature-specific-best-practices--i-want-to-remove-this-and-collapse-this-section-and-just-have-the-specific-features-at-header-2-level-versus-3"></a>Aanbevolen procedures voor specifieke functies


### <a name="request-management"></a>Aanvraagbeheer

MIM 2016 schoont standaard om de 30 dagen verlopen systeemobjecten op, waaronder voltooide aanvragen met bijbehorende goedkeuringen, reacties op goedkeuringen en werkstroomexemplaren. Als uw organisatie een langere aanvraaggeschiedenis nodig heeft, moet u aanvragen exporteren uit MIM en deze opslaan in een ondersteunende database om ze langer dan 30 dagen te bewaren. U kunt de verwijderingsperiode van 30 dagen voor aanvragen configureren, maar het verlengen van deze periode kan de prestaties negatief beïnvloeden als gevolg van de extra objecten in het systeem.

### <a name="management-policy-rules"></a>Beheerbeleidsregels

#### <a name="use-the-appropriate-mpr-type"></a>Het juiste type beheerbeleidsregel gebruiken

MIM biedt twee typen beheerbeleidsregels: aanvraag en setovergang:

-   Beheerbeleidsregel van het type aanvraag (RMPR)

 - Hiermee definieert u het toegangsbeheerbeleid (verificatie, autorisatie en actie) voor Create-, Read-, Update- of Delete-bewerkingen (CRUD) op resources.
 - Dit wordt toegepast wanneer een CRUD-bewerking is gegeven voor een doelresource in FIM.
   - Het bereik van de bewerking wordt bepaald door de overeenkomende criteria gedefinieerd in de regel, dat wil zeggen de CRUD-aanvragen waarop de regel van toepassing is.


-   Beheerbeleidsregel van het type setovergang (TMPR)
 - Wordt gebruikt om beleidsregels te definiëren, ongeacht hoe het object de huidige status is ingegaan die wordt voorgesteld door de setovergang. Gebruik TMPR om het rechtenbeleid te modelleren.
 - Wordt toegepast wanneer een resource een bijbehorende set binnengaat of verlaat.
 - Afgestemd op de leden van de set.

>[OPMERKING] Zie voor meer informatie [Bedrijfsbeleidsregels ontwerpen](http://go.microsoft.com/fwlink/?LinkID=183691).

#### <a name="only-enable-mprs-as-necessary"></a>Beheerbeleidsregels alleen naar wens inschakelen

Gebruik het principe van minimale bevoegdheden bij het toepassen van uw configuratie. Beheerbeleidsregels bepalen het toegangsbeleid voor uw FIM-implementatie. Schakel alleen de functies in die worden gebruikt door de meeste gebruikers. Niet alle gebruikers gebruiken bijvoorbeeld FIM voor groepsbeheer. Beheerbeleidsregels voor groepsbeheer moet dus worden uitgeschakeld. FIM wordt standaard geleverd met de meeste niet-beheerdersmachtigingen uitgeschakeld.

#### <a name="duplicate-built-in-mprs-instead-of-directly-modifying"></a>Ingebouwde beheerbeleidsregels dupliceren in plaats van rechtstreeks wijzigen

Wanneer u de ingebouwde beheerbeleidsregels wilt wijzigen, moet u een nieuwe beheerbeleidsregel maken met de vereiste configuratie en de ingebouwde beheerbeleidsregel uitschakelen. Dit zorgt ervoor dat toekomstige wijzigingen in de ingebouwde beheerbeleidsregels die zijn geïntroduceerd door het upgradeproces, geen negatieve invloed op uw systeemconfiguratie hebben.

#### <a name="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs"></a>Machtigingen voor eindgebruikers moeten lijsten met expliciete kenmerken omvatten die zijn afgestemd op de zakelijke behoeften van gebruikers

Met behulp van lijsten van expliciete kenmerken kan worden voorkomen dat machtigingen onbedoeld worden toegekend aan niet-gemachtigde gebruikers wanneer kenmerken worden toegevoegd aan objecten.
Beheerders moeten expliciet toegang verlenen tot de nieuwe kenmerken in plaats van de toegang verwijderen.

De toegang tot gegevens moet worden afgestemd op de zakelijke behoeften van de gebruikers. Groepsleden mogen bijvoorbeeld geen toegang hebben tot het filterkenmerk van de groep waarvan ze deel uitmaken. Het filter kan onbedoeld gegevens van de organisatie laten zien waartoe de gebruiker normaal gesproken geen toegang zou hebben.

#### <a name="mprs-should-reflect-effective-permissions-in-the-system"></a>Beheerbeleidsregels moeten de effectieve machtigingen in het systeem weerspiegelen

Vermijd het verlenen van machtigingen aan kenmerken die de gebruiker nooit kan gebruiken. Verleen bijvoorbeeld geen machtiging voor het aanpassen van kernresourcekenmerken, zoals objectType. Ondanks de beheerbeleidsregel wordt elke poging om het type van een resource aan te passen nadat deze is gemaakt, geweigerd door het systeem.

#### <a name="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs"></a>Leesmachtigingen moeten losstaan van wijzig- en maakmachtigingen bij het gebruik van expliciete kenmerken in beheerbeleidsregels

Wanneer kenmerken in beheerbeleidsregels expliciet worden vermeld, zijn de kenmerken die vereist zijn voor maken en wijzigen meestal anders dan de machtigingen die beschikbaar zijn voor het lezen. Lezen kan bijvoorbeeld worden verleend boven systeemkenmerken, zoals Creator of objectId, terwijl maken of wijzigen niet kunnen worden opgegeven voor systeemkenmerken.

#### <a name="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules"></a>Maakmachtigingen moeten losstaan van wijzigmachtigingen bij het gebruik van expliciete kenmerken in regels

De maakbewerking vereist dat de gebruiker het objectType selecteert als onderdeel van de bewerking. Dit is een kernsysteemkenmerk dat niet kan worden gewijzigd nadat een maakbewerking is uitgevoerd.

#### <a name="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements"></a>Gebruik één beheerbeleidsregel van het type aanvraag voor alle kenmerken met dezelfde toegangsvereisten

Kenmerken met dezelfde toegangsvereisten die zeer waarschijnlijk niet zullen veranderen, kunt u combineren in één beheerbeleidsregel van het type aanvraag. Dit is efficiënter.

#### <a name="avoid-giving-unrestricted-access-even-to-selected-principal-groups"></a>Vermijd het verlenen van onbeperkte toegang tot geselecteerde principal-groepen

In FIM worden machtigingen gedefinieerd als een positieve verklaring. Omdat FIM weigeringsmachtigingen niet ondersteunt, bemoeilijkt het verlenen van onbeperkte toegang tot een resource het opnemen van uitsluitingen in de machtigingen. De aanbevolen procedure is om alleen de benodigde machtigingen te verlenen.

>[!NOTE]
Hieronder volgt de sectie over rechten. I am wondering how to merge them to avoid creating 5 level headers
#### <a name="use-tmprs-to-define-custom-entitlements"></a>Beheerbeleidsregels van het type setovergang gebruiken voor het definiëren van aangepaste rechten

Gebruik beheerbeleidsregels van het type setovergang (TMPR's) in plaats van RMPR's om aangepaste rechten te definiëren.
TMPR's bieden een model op basis van status voor het toewijzen of verwijderen van rechten op basis van het lidmaatschap in de gedefinieerde overgangssets, of rollen, en de bijbehorende werkstroomactiviteiten. TMPR's moeten altijd worden gedefinieerd in paren, één voor resources die geleidelijk toetreden en één voor resources die geleidelijk verdwijnen. Daarnaast moet elke overgang-MPR afzonderlijke werkstromen bevatten voor de inrichting en het opheffen van de inrichting van activiteiten.

>[!NOTE]
Elke werkstroom voor het opheffen van inrichtingen moet ervoor zorgen dat het kenmerk Uitvoeren bij beleidsupdates is ingesteld op Waar.

#### <a name="enable-the-set-transition-in-mpr-last"></a>De setovergang In MPR last inschakelen

Bij het maken van een TMPR-paar schakelt u In MPR last in. Deze volgorde zorgt ervoor dat er geen resources met het recht achterblijven als dit wordt toegevoegd aan en verwijderd uit de set terwijl In MPR is ingeschakeld maar voordat Out MPR wordt ingeschakeld.

#### <a name="workflows-in-tmpr-should-check-target-resource-state-first"></a>Werkstromen in TMPR moeten eerst de status van de doelresource controleren

Bij het inrichten van werkstromen moet eerst worden bepaald of de doelresource al is ingericht in overeenstemming met het recht. Als dat zo is, zou er niets moeten gebeuren.

Werkstromen voor het opheffen van inrichtingen moeten eerst bepalen of de doelresource is ingericht. Als dat zo is, zou de inrichting van de doelresource moeten worden opgeheven.
Als dit niet het geval is, gebeurt er niets.

#### <a name="select-run-on-policy-update-for-tmprs"></a>Uitvoeren bij beleidsupdates voor TMPR's selecteren

Dit zorgt ervoor dat het juiste inrichtingsgedrag wordt toegepast wanneer beleidsupdates worden geïmplementeerd en gebruik wordt gemaakt van de vlag Uitvoeren van beleidsupdates op actiewerkstromen die zijn gekoppeld aan de TMPR's. Dit zorgt ervoor dat wijzigingen in de beleidsdefinities de actiewerkstromen toepassen op nieuwe leden van de overgangsset.

#### <a name="avoid-associating-the-same-entitlement-with-two-different-transition-sets"></a>Voorkom dat hetzelfde recht wordt gekoppeld aan twee verschillende overgangssets

Het koppelen van hetzelfde recht aan twee verschillende overgangssets kan leiden tot het onnodig intrekken en opnieuw verlenen van rechten als de resource van de ene set naar de andere set wordt verplaatst. Het is raadzaam ervoor te zorgen dat één set alle resources bevat die het bijbehorende recht nodig hebben. Hierdoor bent u verzekerd van een een-op-een-relatie tussen de overgangsset en het recht dat de werkstroom toekent.

#### <a name="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system"></a>De juiste volgorde van bewerkingen gebruiken bij het verwijderen van rechten in het systeem

De volgorde van de stappen die worden uitgevoerd bij het verwijderen van rechten uit het systeem, kan resulteren in twee verschillende operationele resultaten. Zorg ervoor dat u begrijpt welke volgorde nodig is voor het gewenste effect.

Een recht verwijderen uit het systeem (en het intrekken van het recht voor alle leden die dit momenteel hebben):

1.  Schakel de beheerbeleidsregel T-In uit. Hiermee voorkomt u nieuwe toekenningen.

2.  Verwijder het filter T-Set of pas het aan zodat de set leeg is. Dit zorgt ervoor dat alle bestaande leden geleidelijk verdwijnen en past het overgangsbeleid toe, inclusief de werkstroom voor het ongedaan maken van de inrichting gekoppeld aan het recht.

3.  Schakel de beheerbeleidsregel T-Out uit.

Wanneer u een recht wilt verwijderen maar de huidige leden met rust wilt laten (u wilt bijvoorbeeld stoppen met het gebruik van FIM voor het beheren van rechten):

1.  Schakel de beheerbeleidsregel T-In uit. Hiermee voorkomt u nieuwe toekenningen.

2.  Schakel de beheerbeleidsregel T-Out uit.

3.  Verwijder het filter T-Set of pas het aan zodat de set leeg is. Aangezien de set niet meer is gekoppeld aan een TMPR, worden geen werkstromen toegepast voor het ongedaan maken van de inrichting.

### <a name="sets"></a>Sets

Wanneer u de aanbevolen procedures voor sets toepast, moet u rekening houden met de gevolgen van de optimaliseringen voor de uitvoerbaarheid en het gemak van toekomstig beheer.
Goede tests met de verwachte productieschaal moeten worden uitgevoerd om de juiste balans te bepalen tussen prestaties en beheerbaarheid voordat u deze aanbevelingen toepast.

>[!NOTE]
Alle volgende richtlijnen gelden voor dynamische sets en dynamische groepen.


#### <a name="minimize-the-use-of-dynamic-nesting"></a>Het gebruik van dynamisch nesten minimaliseren

Dit heeft betrekking op het filter van een set die verwijst naar het kenmerk ComputedMember van een andere set. Een veelvoorkomende reden voor het nesten van sets is om te voorkomen dat een voorwaarde voor lidmaatschap in veel gegevenssets wordt gedupliceerd. Deze methode kan leiden tot betere mogelijkheden voor het beheren van de sets, maar heeft negatieve gevolgen voor de prestaties. U kunt de prestaties optimaliseren door de lidmaatschapsvoorwaarden van een geneste set te dupliceren in plaats van de set zelf te nesten.

U kunt gevallen tegenkomen waarin u het nesten van sets niet kunt vermijden omdat moet worden voldaan aan een functionele vereiste. Dit zijn de voornaamste situaties waarin u sets moet nesten. Voor het definiëren van de set met alle groepen zonder fulltime-werknemereigenaars moet het nesten van sets bijvoorbeeld als volgt worden gebruikt: `/Group[not(Owner =
/Set[ObjectID = ‘X’]/ComputedMember]`, waarbij 'X' de ObjectID is van de set met alle fulltime-werknemers.

#### <a name="minimize-the-use-of-negative-conditions"></a>Het gebruik van negatieve situaties minimaliseren

Negatieve voorwaarden zijn de lidmaatschapsvoorwaarden waarbij gebruik wordt gemaakt van de volgende operators of functies: `!=`, `not()`, `\<` , `\<=`. Voor het optimaliseren van de prestaties geeft u waar mogelijk de voorwaarde aan die u met meerdere positieve voorwaarden wilt in plaats van als een negatieve voorwaarde.

#### <a name="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes"></a>Het gebruik van lidmaatschapsvoorwaarden op basis van verwijzingskenmerken met meerdere waarden minimaliseren

Het gebruik van voorwaarden op basis van verwijzingskenmerken met meerdere waarden moet worden geminimaliseerd omdat een groot aantal van deze sets invloed kan hebben op de snelheid van de bewerkingen op het kenmerk dat in de lidmaatschapsvoorwaarde wordt gebruikt.

### <a name="password-reset"></a>Wachtwoordherstel

#### <a name="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile"></a>Op computers bedoeld als kiosk die worden gebruikt voor wachtwoordherstel, moet de lokale beveiliging zijn ingesteld op het wissen van het wisselbestand voor virtueel geheugen

Bij het implementeren van FIM 2010-wachtwoordherstel op een werkstation bedoeld als een kiosk is het raadzaam om Afsluiten: wisselbestand voor virtueel geheugen wissen in te schakelen om ervoor te zorgen dat gevoelige informatie in het procesgeheugen niet beschikbaar is voor onbevoegde gebruikers.

#### <a name="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to"></a>Gebruikers moeten zich altijd registreren voor wachtwoordherstel op een computer waarop ze zijn aangemeld

Wanneer een gebruiker zich probeert te registreren voor wachtwoordherstel via een webportal, initieert FIM 2010 altijd registratie namens de aangemelde gebruiker, ongeacht wie is aangemeld bij de website. Gebruikers moeten zich altijd registreren voor wachtwoordherstel op een computer waarop ze zijn aangemeld.

#### <a name="do-not-set-the-avoidpdconwan-registry-key-to-true"></a>De registersleutel AvoidPdcOnWan niet instellen op Waar

Wanneer u MIM 2016-wachtwoordherstel gebruikt, moet u de registersleutel AvoidPdcOnWan niet instellen op Waar.

Als deze registersleutel is ingesteld op Waar, gaat de gebruiker zeer waarschijnlijk door de wachtwoordcontrole, wordt zijn of haar wachtwoord opnieuw ingesteld op de primaire domeincontroller (PDC) en wordt vervolgens geprobeerd aan te melden. Vanwege deze registersleutel voert de lokale domeincontroller geen secundaire validatie uit met de PDC en wordt daarom de aanmeldingsaanvraag geweigerd. Als gebruikers voldoende keren zijn geweigerd, worden ze buitengesloten van het domein en moeten ze contact opnemen met ondersteuning.

#### <a name="do-not-turn-on-logging-of-clear-text-passwords"></a>De logboekregistratie van wachtwoorden met leesbare tekst niet inschakelen

Het is mogelijk leesbare wachtwoorden in het logboek te registreren wanneer diagnostische tracering van het serviceniveau in Windows is ingeschakeld

Communication Foundation (WCF). Deze optie is niet standaard ingeschakeld en het wordt afgeraden om de optie in te schakelen in een productieomgeving. Deze wachtwoorden zijn zichtbaar als leesbare tekstelementen in een versleuteld SOAP-bericht (Simple Object Access Protocol) wanneer gebruikers zich registreren voor wachtwoordherstel. Zie [Configuring Message Logging (Logboekregistratie van berichten configureren)](http://go.microsoft.com/fwlink/?LinkID=168572) voor meer informatie.

#### <a name="do-not-map-an-authorization-workflow-to-the-password-reset-process"></a>Een autorisatiewerkstroom niet toewijzen aan het proces voor wachtwoordherstel

Koppel een autorisatiewerkstroom niet aan een bewerking voor wachtwoordherstel.
Wachtwoordherstel vereist een synchrone reactie en autorisatiewerkstromen die activiteiten bevatten, zoals de goedkeuringsactiviteit, zijn asynchroon.

#### <a name="do-not-map-multiple-action-activities-to-password-reset"></a>Meerdere actieactiviteiten niet toewijzen aan wachtwoordherstel

Koppel een werkstroom met meer dan één actieactiviteit niet aan een bewerking voor wachtwoordherstel. Een voorbeeld hiervan is het koppelen van een tweede activiteit voor AD DS-wachtwoordherstel aan een beheerbeleidsregel voor wachtwoordherstel. Een dergelijk scenario wordt niet ondersteund.

#### <a name="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow"></a>Opnieuw registreren vereisen bij het toevoegen of verwijderen van activiteiten of het wijzigen van de volgorde van activiteiten in een bestaande werkstroom

Wanneer u autorisatieactiviteiten toevoegt of verwijdert of de volgorde hiervan wijzigt in een bestaande werkstroom, moet u altijd de optie selecteren voor het vereisen van hernieuwde registratie. Gebruikers die proberen te verifiëren voor wachtwoordherstel nadat een activiteit is toegevoegd aan of verwijderd uit een werkstroom, maar voordat ze opnieuw zijn geregistreerd, kunnen te maken krijgen met ongewenste effecten.

### <a name="portal-configuration-and-resource-control-display-configuration"></a>Portalconfiguratie en weergaveconfiguratie voor resourcebeheer

#### <a name="consider-adding-a-privacy-disclaimer-to-the-user-profile-page"></a>Het toevoegen van een privacyvrijwaring aan de gebruikersprofielpagina overwegen

In MIM kunnen standaard sommige gebruikersprofielgegevens worden weergegeven aan andere gebruikers. Als voorziening voor gebruikers zouden beheerders moeten overwegen om aan de pagina Gebruikersprofiel aangepaste tekst toe te voegen die aansluit bij het beleid van hun bedrijf. Zie [Inleiding tot het configureren en aanpassen van de FIM-portal](http://go.microsoft.com/fwlink/?LinkID=165848) voor meer informatie over het toevoegen van aangepaste tekst aan een MIM-portalpagina.

### <a name="schema"></a>Schema

#### <a name="do-not-delete-person-or-group-resource-types"></a>Geen resources van het type Persoon of Groep verwijderen

Hoewel de resourcetypen Persoon en Groep niet zijn gemarkeerd als kernresourcetypen, mogen de resources zelf of de kenmerken die eraan zijn toegewezen, niet worden verwijderd. De gebruikersinterface (UI) op de MIM-portal vereist dat de resourcetypen Persoon en Groep en hun kenmerken aanwezig zijn.

#### <a name="do-not-modify-the-core-attributes"></a>Wijzig geen kernkenmerken

Er zijn 13 kernkenmerken aan alle resourcetypen toegewezen. U mag de relatie van deze kenmerken met een resourcetype op geen enkele manier wijzigen. De 13 kernkenmerken zijn:

-   CreatedTime

-   Creator

-   DeletedTime

-   Beschrijving

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   Landinstellingen

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

Schema-resource die afhankelijkheid is van controlevereisten niet verwijderen

Verwijder uw schema-resources niet zolang u nog steeds controlevereisten voor deze resources hebt.

#### <a name="making-regular-expressions-case-insensitive"></a>Reguliere expressies hoofdlettergevoelig maken

In FIM kan het handig zijn om sommige reguliere expressies hoofdlettergevoelig te maken. U kunt het onderscheid tussen hoofd- en kleine letters binnen een groep negeren met behulp van ?!:. Gebruik bijvoorbeeld voor Employee Type

`\^(?!:contractor\|full time employee)%.`

#### <a name="calculation-of-the-member-attribute"></a>het kenmerk Calculation of the member.

Het kenmerk Member dat is blootgesteld aan de synchronisatie-engine wordt daadwerkelijk toegewezen aan ComputedMembers. Het is een combinatie van leden op basis van criteria en handmatig geselecteerde leden. Zelfs als u alle drie de kenmerken (Filter, ExplicitMembers en ComputedMembers) toevoegt, vindt de dynamische berekening van het lidkenmerk alleen plaats voor de resourcetypen Group en Set.

#### <a name="leading-and-trailing-spaces-in-strings-are-ignored"></a>Voorloopspaties en volgspaties in tekenreeksen worden genegeerd

U kunt in FIM tekenreeksen met voorloop- en volgspaties invoeren, maar deze worden door het FIM-systeem genegeerd. Als u een tekenreeks invoert met een voorloop- en volgspatie, worden die spaties door de synchronisatie-engine en webservices genegeerd.

#### <a name="empty-strings-do-not-equal-null"></a>Lege tekenreeksen zijn niet gelijk aan null

Lege tekenreeksen zijn niet gelijk aan null in deze versie van FIM. Een lege tekenreeks wordt beschouwd als een geldige waarde. Niet aanwezig wordt beschouwd als een null-waarde.

### <a name="workflow-and-request-processing"></a>Verwerking van werkstromen en aanvragen

#### <a name="do-not-delete-default-workflows-that-are-shipped-with-mim-2016"></a>Standaardwerkstromen die worden geleverd met MIM 2016 niet verwijderen

De volgende werkstromen worden geleverd met FIM 2010 en mogen niet worden verwijderd:

-   Werkstroom verloop

-   Filtervalidatiewerkstroom voor beheerders

-   Filtervalidatiewerkstroom voor niet-beheerders

-   Werkstroom voor groepsverloop

-   Werkstroom voor groepsvalidatie

-   Werkstroom voor goedkeuring van eigenaar

-   Werkstroom voor actie wachtwoord herstellen

-   Werkstroom voor autorisatie wachtwoord herstellen

-   Werkstroom voor validatie van aanvrager met autorisatie van eigenaar

-   Werkstroom voor validatie van aanvrager zonder autorisatie van eigenaar

-   Systeemwerkstroom vereist voor registratie

#### <a name="do-not-run-two-or-more-approvalactivities-in-parallel"></a>Twee of meer ApprovalActivities niet parallel uitvoeren

Voer twee of meer ApprovalActivities niet parallel uit. Dit kan ertoe leiden dat de aanvraag in de autorisatiefase vast komt te zitten. Voor meerdere goedkeuringen neemt u een lijst op met goedkeurders in het proces of voert u de twee activiteiten achtereenvolgens uit.

#### <a name="authorization-activities-should-not-modify-mim-resources-data"></a>Bij autorisatieactiviteiten mogen geen MIM-resourcegegevens worden gewijzigd

Voorkom het gebruik van activiteiten waarbij MIM-resources, zoals de activiteit van functie-evaluator, worden aangepast als onderdeel van de werkstromen in de autorisatiewerkstromen. Aangezien de aanvraag niet is doorgevoerd op het autorisatiepunt van de verwerking, kunnen alle wijzigingen aan de identiteitsgegevens worden toegepast ondanks de mogelijkheid dat de aanvraag wordt verworpen.

### <a name="understanding-fim-service-partitions"></a>Meer informatie over FIM-servicepartities

Het doel van FIM is het verwerken van aanvragen die kunnen worden gestart door verschillende FIM-clients, zoals de FIM-synchronisatieservice en de selfserviceonderdelen, volgens uw geconfigureerde bedrijfsbeleid. Standaard behoort elk FIM service-exemplaar toe aan een logische groep die bestaat uit een of meer FIM service-instanties, die ook wel een FIM-servicepartitie wordt genoemd. Als u slechts één FIM service-exemplaar hebt geïmplementeerd voor het verwerken van alle aanvragen, is het mogelijk dat u te maken krijgt met latentie bij de verwerking. Bij bepaalde bewerkingen kunnen zelfs de standaard time-outwaarden worden overschreden die geschikt zijn voor selfservicebewerkingen. U kunt dit probleem oplossen met behulp van FIM-servicepartities. Zie Meer informatie over FIM-servicepartities.
