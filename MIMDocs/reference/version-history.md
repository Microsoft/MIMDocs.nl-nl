---
title: Versie geschiedenis van Identity Manager | Microsoft Docs
description: In dit artikel worden de verschillende wijzigingen gedocumenteerd die zijn aangebracht als onderdeel van updates voor MIM 2016
keywords: ''
author: EugeneSergeev
manager: aashiman
editor: ''
reviewer: markwahl-msft
ms.devlang: na
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/11/2020
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.topic: reference
ms.openlocfilehash: 424381cca56223e80403bad9f858c33362bb819d
ms.sourcegitcommit: dae61d97c9db5402d35e2757a1ce844d16236032
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2020
ms.locfileid: "94532134"
---
# <a name="identity-manager-version-release-history"></a>Release geschiedenis van de versie van Identity Manager

Het Microsoft Identity Manager team brengt regel matig updates uit. Dit artikel is bedoeld om u te helpen bij het bijhouden van de versies die zijn vrijgegeven. U kunt vervolgens bepalen of u al in het meest recente Service Pack en de update versie van de hotfix bent of als u een upgrade wilt uitvoeren. 

>[!NOTE]
>In dit artikel worden alleen releases van de MIM-synchronisatie, service en Portal, client, CM en PAM-onderdelen beschreven.  Weet u niet zeker welke onderdelen u nodig hebt?  Zie [Microsoft Identity Manager licenties en down loads](../microsoft-identity-manager-licensing.md).
>
>De versie geschiedenis voor onderdelen van micro soft BHOLD Suite vindt u in de [versie geschiedenis](version-bhold-history.md)van de BHOLD-modules.
>
>De versie geschiedenis voor de algemene LDAP-, algemene SQL-, Web Services-, Power shell-, Graph-en Lotus Domino-connectors vindt u in de [release geschiedenis van de connector versie](microsoft-identity-manager-2016-connector-version-history.md).  

## <a name="mim-version-463550"></a>MIM-versie 4.6.355.0
- Status: 6 november 2020
- [Hotfix downloaden](https://www.microsoft.com/download/details.aspx?id=102301)
- [KB-artikel 4585922](https://support.microsoft.com/help/4585922)

Deze hotfix bevat updates voor de MIM-synchronisatie beheerder, de MIM-service en MIM-Portal onderdelen en bevat ook cumulatieve updates voor MIM-onderdelen uit de vorige hotfixes voor MIM 2016 SP2.


## <a name="mim-version-462630"></a>MIM-versie 4.6.263.0
- Status: 7 augustus 2020
- [Hotfix downloaden](https://www.microsoft.com/download/details.aspx?id=101612)
- [KB-artikel 4576473](https://support.microsoft.com/help/4576473)

Deze hotfix bevat updates voor de MIM CM, MIM Synchronization Manager en PAM-onderdelen.


## <a name="mim-version-462580"></a>MIM-versie 4.6.258.0
- Status: 22 mei 2020
- [Hotfix downloaden](https://www.microsoft.com/download/details.aspx?id=101305)
- [KB-artikel 4560335](https://support.microsoft.com/help/4560335)

Deze hotfix bevat updates voor de MIM-service, de MIM-Portal en de PAM-onderdelen.


## <a name="mim-version-46340"></a>MIM-versie 4.6.34.0
* Status: MIM 2016 Service Pack 2 (SP2) van oktober, 2019
* Bijbehorend BHOLD-versie nummer: 6.0.62.0
- [Hotfix downloaden](https://www.microsoft.com/download/details.aspx?id=100412)
- [KB-artikel 4512924](https://support.microsoft.com/help/4512924)
- ISO-bestand kan worden gedownload [van VLSC](https://www.microsoft.com/Licensing/servicecenter/default.aspx) of via  [Visual Studio-down loads](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202)

> [!IMPORTANT]
>- .NET Framework 4,6 is vereist<br>
>- Het [te distribueren pakket Visual C++ 2013 (vcredist_x64.exe of vcredist_x86.exe)](https://www.microsoft.com/download/details.aspx?id=40784) is vereist<br>

>[!NOTE]
> Lees de MIM- [implementatie handleiding](../microsoft-identity-manager-deploy.md) voor een bijgewerkte lijst met vereisten en beperkingen met betrekking tot alleen TLS 1,2-omgevingen, ondersteuning van door groepen beheerde service accounts en het [upgradepad van eerdere versies van MIM en FIM](../microsoft-identity-manager-2016-service-pack-2-upgrade-path.md).

Er is een pakket met Service Pack 2 (SP2) (build 4.6.34.0) beschikbaar voor Microsoft Identity Manager (MIM) 2016. Het is een cumulatieve update waarmee eerder MIM 2016 SP1-updates 4.4.1302.0 via build 4.5.412.0 worden vervangen.
### <a name="updates-in-mim-2016-service-pack-2"></a>Updates in MIM 2016 Service Pack 2

#### <a name="mim-client-addons"></a>Invoeg toepassingen voor MIM-client
- Er is ondersteuning toegevoegd voor de MIM Outlook-invoeg toepassing die moet worden geladen in de Outlook voor Microsoft 365 klik-en-klaar-versie.

#### <a name="service-and-portal"></a>Service en portal
- Er is ondersteuning toegevoegd voor de MIM-service en-portal voor installatie op Windows Server 2019 en het gebruik van SQL Server 2017, Exchange Server 2019, share point 2019, System Center Service Manager Data Warehouse 2019
- De MIM-service en-Portal-installatie zijn ingeschakeld in omgevingen met alleen TLS 1,2.
- Ingeschakelde installatie van de MIM-service, wacht woord opnieuw instellen en websites voor wachtwoord registratie, PAM-bewakings service, PAM component-service voor het gebruik van door groepen beheerde service accounts.
- De installatie parameter keepSQLjobs is toegevoegd.
- De tijdelijke taken van MIM SQL Server Agent worden niet langer gestart op replica's van de secundaire SQL-Always-On-beschikbaarheids groep.
- ' ExplicitMember. add ' en ' ExplicitMember. Remove ' virtuele kenmerken ingeschakeld voor aangepaste object typen op RCDC-formulieren om te werken met wijzigingen in de Delta.

#### <a name="synchronization-service"></a>Synchronisatieservice
- Er is ondersteuning toegevoegd voor de MIM-synchronisatie service die moet worden geïnstalleerd op Windows Server 2019, en gebruikt SQL Server 2017, Exchange Server 2019
- De MIM-synchronisatie service is alleen geïnstalleerd in omgevingen met TLS 1,2.
- De installatie van de MIM-synchronisatie service voor het gebruik van een beheerd service account voor een groep is ingeschakeld.
- De optie MIMSync-account gebruiken is toegevoegd voor de MIM-Service beheer agent.
 
#### <a name="privilege-access-management"></a>Toegangs beheer met bevoegdheden 
- De Power shell-cmdlet Get-PAMRequest retourneert een extra eigenschap ' FIMRequestID '.


## <a name="mim-version-454120"></a>MIM-versie 4.5.412.0
> [!IMPORTANT]
>- Het formulier RCDC voor groeps objecten kan niet worden weer gegeven wanneer de kenmerk waarde ' displayedOwner ' niet is ingevuld. Het is niet mogelijk om groepen te bewerken/weer te geven als er een fout optreedt.<br>

- Beschikbaarheids datum: 10 mei 2019
- [Hotfix downloaden](https://www.microsoft.com/en-us/download/details.aspx?id=58213)
- [KB-artikel 4489646](https://support.microsoft.com/en-us/help/4489646/hotfix-rollup-4-5-412-0-available-for-mim-2016-sp1)

Deze hotfix bevat updates voor de MIM-synchronisatie, de MIM-service, de MIM-Portal, MIM CM en PAM-onderdelen.  Het is een cumulatieve update waarmee eerder MIM 2016 SP1-updates 4.4.1302.0 via build 4.5.286.0 worden vervangen.

> [!IMPORTANT]
>- .NET Framework 4,6 is ook vereist voor het installatie programma <br>
>- De [herdistribueerbare pakketten voor Visual C++ 2013 x64 (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) zijn vereist<br>

## <a name="mim-hybrid-reporting-agent-version-31410"></a>MIM Hybrid Reporting agent versie 3.1.41.0
- Beschikbaarheids datum: 22 april 2019
- [Hybrid Reporting agent downloaden](https://www.microsoft.com/en-us/download/details.aspx?id=55112)

De MIM Hybrid Reporting agent versie 3.1.41.0 bevat updates voor TLS 1,2 en een oplossing voor fouten.  De Hybrid Reporting agent kan worden gebruikt met MIM 2016 SP1-versies 4.4.1302.0 of hoger en een Azure AD Premium-abonnement.


## <a name="mim-version-452860"></a>MIM-versie 4.5.286.0
- Status: 19 november 2018
- [Hotfix downloaden](https://www.microsoft.com/en-us/download/details.aspx?id=57596)
- [KB-artikel 4469694](https://support.microsoft.com/en-us/help/4469694/hotfixrolluppackagebuild452860isavailableformicrosoftidentitymanager20)


Deze hotfix bevat updates voor de MIM-service, de MIM-Portal en de PAM-onderdelen.  .NET Framework 4,6 is vereist voor het installatie programma.


## <a name="version-452020"></a>Versie 4.5.202.0
- Status: 30 augustus 2018
- [KB-release KB](https://support.microsoft.com/en-us/help/4346632)

> [!IMPORTANT]
>- .NET Framework 4,6 is ook vereist voor het installatie programma <br>
>- De [herdistribueerbare pakketten voor Visual C++ 2013 x64 (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) zijn vereist<br>
>- Ondersteunde land instellingen bijgewerkt naar nieuwe ISO-normen ([hier](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-language-support))<br>
>- * Hiermee wordt de nieuwe uitbrei ding aangegeven 

#### <a name="mim-service"></a>MIM-service
- Integratie van Azure MFA-server-alternatieve Multi-Factor Authentication provider

#### <a name="privilege-access-management"></a>Toegangs beheer met bevoegdheden 
- PAM-REST API kan niet worden gestart omdat het bestand of de assembly niet kan worden geladen

#### <a name="microsoft-identity-portal"></a>Micro soft Identity Portal
- De portal wordt weer gegeven met een onjuiste tabel lengte
- Geavanceerd zoek venster van de portal, de schuif balken worden niet correct weer gegeven
- Handtekening verificatie van sterke naam van taal pakket taal pakket is mislukt

#### <a name="certificate-management"></a>Certificaatbeheer
- Instructie voor het omleiden van bindingen voor REST API

## <a name="version-45260"></a>Versie 4.5.26.0
- Status: 30 juni 2018
- [Release-KB4073679 KB](https://support.microsoft.com/en-us/help/4073679)

> [!IMPORTANT]
>- .NET Framework 4,6 is ook vereist voor het installatie programma <br>
>- De [herdistribueerbare pakketten voor Visual C++ 2013 x64 (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) zijn vereist<br>
>- Ondersteunde land instellingen bijgewerkt naar nieuwe ISO-normen ([hier](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-language-support))<br>
>- * Hiermee wordt de nieuwe uitbrei ding aangegeven 

#### <a name="synchronization-service"></a>Synchronisatie service
- * Ondersteuning voor door groep beheerde service accounts
- * Ondersteuning voor Visual Studio (Visual Studio 2013, Visual Studio 2015, Visual Studio 2017)
- Updates voor MIISACTIVATE.EXE, gMSA-ondersteuning toegevoegd 
    - niet-gMSA: Miisactivate.exe c:\configBU\ miiserver_01. bin "contoso\mimSyncService" *
    - gMSA: Miisactivate.exe c:\configBU\ miiserver_01. bin "contoso\mimSyncService"
- Updates voor MIISKMU.exe, gMSA-ondersteuning toegevoegd 
    - non-gMSA:MIISKMU.exe/e c:\configBU\ miiserver_02. bin "/u:" contoso\mimSyncService "
    - gMSA:MIISKMU.exe/e c:\configBU\ miiserver_02. bin "/u:" contoso\mimSyncService "*
- Bijgewerkte partitie gegevens worden op de verwachte manier opgeslagen wanneer de knop Vernieuwen en vervolgens op OK klikt
- Wanneer het indexeren van een Indexeer bare teken reeks kenmerk te lang is, is er een onverwachte fout geretourneerd. er wordt nu een meer beschrijvender fout bericht weer gegeven
- Het maken van een beheer agent voor tekst bestanden wanneer de MIM-synchronisatie service is geïnstalleerd op Windows Server 2016, sommige opties voor tekst codering, inclusief Unicode, zijn niet beschikbaar
- MIM-service: als een export fout bericht een ongeldig teken bevat, veroorzaakt dit een beschadiging in de vermeldingen in de uitvoerings geschiedenis. Deze build is verwijderd uit het fout bericht voordat deze wordt opgeslagen in het connector-ruimte object en de uitvoerings geschiedenis

#### <a name="mim-service"></a>MIM-service
- * Ondersteuning voor door groep beheerde service accounts
- * Verbeterde taal ondersteuning voor nieuwe gedefinieerde standaard
- * FIMAutomation Export-FIMConfig Power shell-cmdlet het argument '-PamConfig ' is beschikbaar om te zorgen dat de PAM-configuratie objecten worden geëxporteerd
- * FIMAutomation Export-FIMConfig Power shell-cmdlet de para meter-request is toegevoegd
- * Booleaanse kenmerken worden altijd ingesteld op NULL bij het maken van de binding, de vorige Booleaanse waarde voordat hotfix wordt niet bijgewerkt
> [!IMPORTANT]
>Dit kan een belang rijke wijziging zijn als u een configuratie migratie voordoet. De configuratie moet worden geëvalueerd en bijgewerkt voor een nieuwe functie, omdat de configuratie migratie wordt beschouwd als een nieuwe 
    - Initialisatie van nieuwe MIM-Booleaanse kenmerken is geïmplementeerd op False bij het maken van een nieuw object
    - De initialisatie van nieuwe MIM-Booleaanse kenmerken is geïmplementeerd op False voor het toevoegen van een nieuwe Boole-kenmerk binding aan de resource
- De instelling van Customer Experience Improvement Program blijft onwaar 
- De installatie van de MIM-service is mislukt omdat de data base-upgrade fout is opgetreden: kan de waarde NULL niet invoegen in de kolom naam als er geen standaard database naam wordt gebruikt
- In hotfixes wordt de Microsoft 365 instelling gewist, het versleutelde wacht woord voor het Exchange Online-postvak van de MIM-service niet gewijzigd
- * Er is geen limiet ingesteld voor het logboek bestand van de MIM-service, de standaard instelling voor logboek registratie is bijgewerkt en de circulaire logboek functie is geïmplementeerd

#### <a name="privileged-access-management"></a>Privileged Access Management (Windows en Microsoft Active Directory beveiligen met beheer van bevoegde toegang) 
- * Ondersteuning voor door groep beheerde service accounts
- * Verbeterde taal ondersteuning voor nieuwe gedefinieerde standaard
- Objecten die niet-beheerde resources gebruiken, worden niet op tijd gewist.  deze objecten worden goed opgeruimd
- * New-PAMRole Power shell-cmdlet de '-disableAutoApproveIfOwner ' zelf goed keuring voor de rol weigeren
- * Get-PamRequest Power shell-cmdlet '-CreatedFrom ' staat de specifieke aanvraag voor filter od PAM toe
- * Toevoegingen aan PAM-module
    - Get-PAMSet
    - Add-PAMSetMember
    - Remove-PAMSetMember
- De waarschuwing (uitzonde ring: System. ObjectDisposedException: kan geen toegang krijgen tot een verwijderd object) wordt niet meer weer gegeven in het gebeurtenis logboek van PAM
- Met Set-PAMUser cmdlet kan de PrivAccountName zonder verwijderen worden gewijzigd
- New-PamRole valideert nu of de datum ' beschikbaar op ' groter is dan de datum ' beschikbaar van '
- De waarden ' beschikbaar van ' en ' beschikbaar tot ' worden geretourneerd door de Get-PAMRole Power shell-cmdlet
- Het filter van de Get-PamRequest-cmdlet is nu correct
- * De cmdlet Set-PamGroup is nu in staat om de Active Directory Shadow Principal Group-object bij te werken
- Remove-PamUser Power shell-cmdlet mislukt met een onduidelijk fout bericht, als de gebruiker is gekoppeld aan een rol als kandidaat. De validatie aan de client zijde is nu toegevoegd aan de cmdlet en de geretourneerde uitzonde ring is verduidelijkt
- Modus PAM-accounts wijzigen is niet beschikbaar voor configuratie
    - PAM rest API-account
    - Service account van PAM-component
    - PAM-bewakings service account

#### <a name="microsoft-identity-portal"></a>Micro soft Identity Portal
- * Ondersteuning voor door groep beheerde service accounts
- * Verbeterde taal ondersteuning voor nieuwe gedefinieerde standaard
- Besturings element identiteits kiezer, het besturings element lijkt dynamisch de breedte te verg Roten in plaats van de tekst te verpakken
- -Portal, pop-updialoogvensters worden niet correct weer gegeven wanneer ze worden weer gegeven in Internet Explorer (IE) 10
- Cyrillische symbolen in de tekst van de titel balk worden correct weer gegeven
- De extra schuif balk wordt niet meer weer gegeven in pop-upvensters in Internet Explorer
- Mislukt ' werk stroom definitie importeren ' genereert een uitzonde ring en herstelt, waardoor een synchronisatie regel activiteit kan worden toegevoegd aan de definitie van de werk stroom
- <httpRuntime enableVersionHeader="false" /> toegevoegd aan de standaard web.config
- Speciale tekens in de DN verhinderen niet langer dat Self-Service wacht woord opnieuw instellen het wacht woord van de gebruiker opnieuw instelt in de Active Directory
- Verbeteringen in de zinnen zijn op de juiste wijze gelokaliseerd in de weer gave
- De MIM-invoeg toepassing voor Outlook bevat een kopie van de ontbrekende binaire bestanden voor Outlook Interop

#### <a name="certificate-management"></a>Certificaatbeheer
- Een virtuele Smart Card vernieuwen via de MIM CM moderne app, de gebruiker ontvangt een uitzonde ring voor geweigerd
- * Verbeterde taal ondersteuning voor nieuwe gedefinieerde standaard
- Het hulp programma PIN ' CLM is een fout opgetreden bij het wijzigen van de pincode van de Smart Card.  Onjuist aantal argumenten of ongeldige eigenschappen toewijzing.
- Het bijwerken van de modules van de MIM-certificerings instantie van 4.4.1302.0 naar een build later dan 4.4.1459, mislukt de installatie
- Moderne app voor Renew-, inschrijvings-en vervang bewerkingen, de aanvraag geschiedenis bevat niet alle items van de aanvraag status, zoals vastgelegd
- Online-update is niet voltooid en de uitzonde ring wordt geretourneerd of is door een andere gebruiker bijgewerkt of verwijderd.  
- De koppeling ' certificaat downloaden ' in het certificaat Beheerportal, het certificaat dat is gedownload (. cer-bestand), is te groot
- MIM Certificate Management bulksgewijze client werkt met zowel TLS 1,1 als TLS 1,2.  

 
## <a name="version-4417490"></a>Versie 4.4.1749.0

- Status: 30 november 2017
- [Downloaden](https://www.microsoft.com/download/details.aspx?id=56271)

### <a name="fixed-issues"></a>Opgeloste problemen
De volgende problemen zijn opgelost in MIM versie 4.4.1749.0.

#### <a name="synchronization-service"></a>Synchronisatie service

- De routine voor het opnieuw instellen van het wacht woord mislukt wanneer het synchronisatie Server domein geen vertrouwens relatie met het doel domein heeft.

#### <a name="mim-service"></a>MIM-service

- Data base-upgrade fout. Er wordt een uitzonde ring voor de beperking van een afwijkings sleutel in het data base-upgrade logboek vastgelegd.
- Wanneer u aanvragen voor het opnieuw instellen van wacht woorden uitvoert, wordt de MIM-service wille keurig gestopt.
- De berekening van de set kan niet overeenkomen met het juiste lidmaatschap. U kunt een binding voor een kenmerk niet verwijderen als ernaar wordt verwezen in een dynamisch set-of groeps filter.
- De MIM-service werkt niet voor het goedkeurings scenario voor aanvragen met Exchange Online waarvoor goed keuringen worden beantwoord via de MIM-invoeg toepassing voor Outlook.
- Het kenmerk msidmPhoneGatePhoneNumber zonder land code gebruikt niet de waarde DefaultCountryCode in MFASettings.xml.
- Stel lidmaatschappen kunnen dynamisch worden bijgewerkt zonder dat ze afhankelijk zijn van de FIM_TemporalEventsJob.
- Synchronisatie regels bieden geen ondersteuning voor het maken van kenmerk stroom regels voor kenmerken waarvan de namen het hekje (#) bevatten.
  
#### <a name="privilege-access-management"></a>Toegangs beheer met bevoegdheden 

- New-PAMDomainConfiguration Power shell-cmdlet een onjuiste waarde instellen voor de configuratie van de domein vertrouwensrelatie als gevolg van een fout (deze onbekende aanvraag parameter kan niet worden verwerkt).

#### <a name="microsoft-identity-portal"></a>Micro soft Identity Portal

- Er wordt een uitzonde ring weer gegeven in het hoofd scherm van de identiteits Beheerportal en er wordt ook een sluit knop weer gegeven.
- Knoppen worden onjuist weer gegeven in het venster item verwijderen.  Dit probleem is opgetreden in Internet Explorer, Firefox en Chrome. 
- De zoek knop overlapt de knop Resource kiezer in een venster goedkeurings activiteit in de werk stroom autorisatie. Dit probleem is opgetreden in Internet Explorer, Firefox en Chrome. 
- In het pop-upvenster met groeps eigenschappen overlapt het knop gebied de besturings elementen van de List View-navigatie in het besturings element leden verwijderen.  Dit probleem is opgetreden in Internet Explorer, Firefox en Chrome.
- Meerdere elementen van de gebruikers interface worden niet correct weer gegeven. De volgende elementen zijn opgelost:

    - Pijl omhoog en omlaag in sommige eigenschappen Vensters.
    - Lege ruimte aan de onderkant van sommige pagina's en dialoog vensters.
    - Ontbrekende popup-overlays.

- Wanneer u de filter functie voor verschillende gebieden van het product (zoals geavanceerde zoek opdrachten) gebruikt, krijgt de filter functie ' vastgelopen ' als de knop OK op het dialoog venster waarde selecteren wordt weer geven zonder dat er een object is geselecteerd in het gedeelte instructie toevoegen.
- De pop-up van het nieuwe kenmerk flow in een dialoog venster voor het bewerken van synchronisatie regels werkt niet zoals verwacht in Chrome.
- In een scherm voor object beheer (zoals distributie groepen), als er meerdere objecten zijn geselecteerd door het selectie vakje in te gebruiken en de objecten lange weergave namen hebben. De grootte van het dialoog venster is nu verticaal, zodat het besturings element niet meer wordt weer gegeven na het einde van het browser scherm.
- In een object beheer-of lijst scherm (zoals distributie groepen) is het mogelijk dat het besturings element voor geselecteerde items direct onder het laatste object wordt weer gegeven in de tabel lijsten.
- De filter functie voor filters (zoals Geavanceerd zoeken) in de Safari-browser is niet functioneel.
- Portal dialoog vensters waarin kenmerk waarden worden weer gegeven, de kortere woorden worden gedistribueerd in de hele cel met veel witruimte in plaats van links uitgelijnd. 
- In sommige browser versies worden de geselecteerde items niet bijgewerkt wanneer de selectie van het item wordt gewijzigd.
- Dialoog tabbladen en kopiëren naar klem bord markeren wanneer u navigeert naar met behulp van de tab-toets.  
- Wanneer u in Internet Explorer 10 een weer gave van een object Raster weergeeft (zoals distributie groepen), wordt het deel venster ' Zoek de distributie groepen die u wilt gebruiken met de bovenstaande zoek opdracht ' het span doek van het lint.  
- Na de installatie van een update voor de MIM-Portal, mislukt de weer gave van de portal in Internet Explorer.
- Wanneer u de geavanceerde zoek opdracht in de Firefox-browser gebruikt, wordt er een fout geretourneerd als u op de Enter-toets drukt op een kenmerk waarde.  

#### <a name="certificate-management"></a>Certificaatbeheer

- Een aanvrager van een aanvraag (certificaat beheerder) kan geen aanvraag afbreken die is gedupliceerd of is verg eten door een gebruiker die over uitvoerings machtigingen beschikt.
- Wanneer u probeert TPM virtuele Smart Card te vernieuwen vanuit de moderne app, wordt een verboden uitzonde ring geretourneerd.
- Tijdens sommige smartcard activiteiten blijven bestaande verbindingen met de CertificateManagement-data base onverwacht open.  
- Als er een installatie van een update van MIM Certificate Management (CM) wordt uitgevoerd voordat de wizard MIM CM configuratie wordt gestart, mislukt de update met een uitzonde ring die niet aan het probleem is gerelateerd.
- De gegevens van de product versie worden onjuist weer gegeven in de wizard MIM CM configuratie en het logo wordt niet correct weer gegeven.  
- De geëxporteerde gegevens voor een MIM Certificate Management-rapport verschillen van de rapport gegevens.  De kolom gegevens komen niet altijd overeen met de kolom koppen.

## <a name="version-4416420"></a>Versie 4.4.1642.0

- Status: 29 augustus 2017
- [Downloaden](https://www.microsoft.com/download/details.aspx?id=55794)

### <a name="fixed-issues"></a>Opgeloste problemen
De volgende problemen zijn opgelost in MIM versie 4.4.1642.0.

#### <a name="synchronization-service"></a>Synchronisatieservice

- De routine voor het opnieuw instellen van het wacht woord mislukt wanneer het synchronisatie Server domein geen vertrouwens relatie met het doel domein heeft.
- Het gedeclareerde import filter (Distinguished Name) werkt niet correct wanneer een object wordt verplaatst van een organisatie-eenheid waar het moet worden gefilterd op een andere locatie, waarbij het niet moet worden gebruikt als gevolg van het gebruik van een oude naam van een phantom-object.
- Beheer agent Designer loopt vast op de pagina ' partities en hiërarchieën configureren '.
- De prioriteit wordt niet overgedragen naar het volgende object wanneer het vorige item met de prioriteit wordt losgekoppeld.
- Sun One-beheer agent zoeken naar onderliggende containers op de LDAP-server met paging veroorzaakt een fout als de server geen paginering ondersteunt.
- Het object type van de tekst is dynamisch gewijzigd, waardoor het vastloopt.

#### <a name="mim-service"></a>MIM-service

- De functie Word retourneert geen lege teken reeks als de teken reeks kleiner is dan het aantal woorden.
- Leden weer geven gebruikers interface toont een onjuist lidmaatschap wanneer de criteria een verwijzing bevatten en als het verwijderen onder een ander criterium gedeelte plaatsvindt. 
- De AuthZ-werk stroom weigert een aanvraag met een fout bericht ' werk stroom is niet gevonden in de opslag voor status persistentie '.
- De werk stroom voert een opsommen van de resource activiteit uit voor het opvragen van MIM en er loopt af en toe een storing.

#### <a name="privilege-access-management"></a>Toegangs beheer met bevoegdheden

- Get-PAMRequestToApprove-commandlet retourneert geen kandidaten als de fiatteur niet in de rol goed keurt.
- Het gerelateerde beheer beleidsregels-en PAM-navigatie balk knooppunt zijn ingeschakeld, zelfs wanneer PAM niet is geïnstalleerd.
- De MIM-service genereert een uitzonde ring en logboeken waarschuwing van de domein configuratie synchronisatie routine voor PAM.

#### <a name="microsoft-identity-portal"></a>Micro soft Identity Portal

- Wanneer u Firefox gebruikt om toegang te krijgen tot de filter functie in de MIM-Portal, kunt u de interne besturings elementen (vervolg keuzelijsten, tekst vakken, enzovoort) niet gebruiken. Ze kunnen pas worden geselecteerd als u eerst met de rechter muisknop hebt geklikt (Klik op om het context menu weer te geven).
- De portal Zoek resultaten worden onjuist op sommige scherm resoluties weer gegeven. 
- Het besturings element kalender in geavanceerde zoek opdracht is afgekapt.
- De gebruikers interface van filter Builder is verbroken: in bewerkings modus (verkeerd geplaatste elementen).
- Het pop-upvenster had een onjuiste grootte en besturings elementen voor bewerking hebben niet de juiste grootte.
- Teken reeks delen voor Noor wegen en andere talen in het hoofd menu.
- De gekopieerde URL van de pop-up werkt niet.

#### <a name="certificate-management"></a>Certificaatbeheer

- MIM self service-wachtwoord portals.

#### <a name="reporting"></a>Rapportage

- Import-FIMReportingSchemaDefinition-cmdlet voor rapportage mislukt met fout.

#### <a name="client-add-in"></a>Client-invoeg toepassing

- De taal van de gebruikers interface controleert niet de gelijkheid van de land code.

### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
De volgende functies en verbeteringen zijn toegevoegd aan MIM versie 4.4.1642.0.

#### <a name="mim-service"></a>MIM-service

- Nieuwe poging toegevoegd in de langste aanvraag verwerkings bewerkingen (validatie fase). Het biedt geen garantie dat de verwerking van aanvragen is voltooid, maar dat aanvragen stabieler worden. De oplossing is standaard uitgeschakeld. Als u de oplossing wilt inschakelen, voegt u alwaysOnRetryRequestProcessingTransaction = ' True ' toe aan de sectie resourceManagementService van het bestand FIMService config

#### <a name="certificate-management"></a>Certificaatbeheer

- Nieuwere versies van CM-server kunnen werken met oudere BulkClient (maar niet ouder dan 4.4. xxxx. 0). 

#### <a name="mim-self-service-password-portals"></a>Portals van MIM self-service-wacht woorden 

- Waarschuwing voor QAGate weer geven als deze functie wordt gebruikt, wordt een antwoord met een double-byte teken
- Configureer bare eigenschap toevoegen om het inschakelen/uitschakelen van het gebruik van IME op SSPR-registratie formulier te toestaan 
- SSPR maskeert de vragen over wachtwoord registratie op de portal-client

## <a name="version-4415490"></a>Versie 4.4.1549.0
 
* Status: vorige update van de hotfix voor MIM 2016 SP1, van 27 maart 2017
* Bijbehorend BHOLD-versie nummer: 6.0.36.0
* Meer informatie vindt u op [https://support.microsoft.com/en-us/help/4012498](https://support.microsoft.com/en-us/help/4012498)


## <a name="version-4413020"></a>Versie 4.4.1302.0

* Status: MIM 2016 Service Pack 1 (SP1) van 9 november 2016
* Bijbehorend BHOLD-versie nummer: 5.0.3355.0

### <a name="updates-in-this-service-pack"></a>Updates in dit servicepack


#### <a name="mim"></a>MIM

- **Compatibiliteit van MIM-portal met meerdere browsers voor selfservice voor eindgebruikers:** met dit servicepack introduceren we ondersteuning voor de meest belangrijke browsers. Gebruikers hebben nu toegang tot en kunnen communiceren met de MIM-portal voor selfservice groeps- en profielbeheer vanuit Microsoft Edge, Chrome en Safari.

- **MIM-service-ondersteuning voor Exchange Online:** de MIM-service biedt ondersteuning voor het versturen en ontvangen van e-mailberichten voor goedkeuringen en meldingen. Vóór SP1 MIM werd alleen Exchange Server of SMTP ondersteund. Met Service Pack 1 kan de MIM-service aanvragen verzenden en ontvangen, evenals e-mail meldingen met een Microsoft 365 Exchange Online-account.

- **Validatie van bestandsindeling van afbeeldingen:** MIM kan de bestandsindeling van afbeeldingen controleren wanneer deze naar de portal worden geüpload.

#### <a name="privileged-access-managementpam"></a>Privileged Access Management (PAM) gebruiken

- **PAM PRIV (bastionhost) forestondersteuning voor het Windows Server 2016-functionaliteitsniveau:** de MIM PAM-service kan worden geconfigureerd in een omgeving met domeincontrollers die worden uitgevoerd op het functionele Active Directory Domain Services-forestniveau van Windows Server 2016. Als dit is geconfigureerd, wordt voor een Kerberos-ticket van een gebruiker een tijdslimiet ingesteld voor de resterende tijd van de activatie van de rol.

    >[!Note]
    Als u het forest-functionaliteitsniveau van Windows Server 2012 R2 in uw CORP-domein wilt behouden, is het raadzaam om [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) en [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) op de CORP-domeincontroller te installeren.

- **Bevoegde uitbreiding van accounts in groepen, exclusief voor het PRIV-forest (bastion):** administrators kunnen de MIM-service van groepen en gebruikers exclusief informeren in het PRIV-forest. Hierdoor kunt u deze groepen en gebruikers opnemen in PAM-rollen.  Vervolgens kunnen de gebruikers worden geactiveerd voor een rol en toegewezen lidmaatschap aan groepen in het PRIV-forest.

- **PAM-implementatiescripts:** met PAM-implementatiescripts kunnen administrators de installatie van de PAM-omgeving stroomlijnen.

- **PAM-cmdlets voor de configuratie van authenticatiebeleidssilo’s:** Service Pack 1 introduceert nieuwe cmdlets om de beveiliging van uw bastionforest op te schroeven. Deze cmdlets maken automatisch een authenticatiebeleidssilo die gebonden is aan een authenticatiebeleidssjabloon.

    >[!Note]
    Deze cmdlets worden automatisch uitgevoerd als onderdeel van de implementatiescripts.



### <a name="platform-support"></a>Platformondersteuning 

Informatie over bijgewerkte platformondersteuning vindt u in het document met de naam [Ondersteunde platformen voor MIM 2016](../microsoft-identity-manager-2016-supported-platforms.md).  Nieuwe platforms die in dit Service Pack worden ondersteund, zijn onder andere SQL Server 2016, share point 2016.


### <a name="issues-fixed-in-this-release"></a>Problemen die in deze release zijn opgelost


#### <a name="pam"></a>PAM
- Nieuwe PAMGroup heeft geen MIM-objecten voor lokale domeingroepen gemaakt in het PRIV-forest
- Nieuwe PAMDomainConfiguration mislukt met een netdom-foutbericht
- PAM-controleservice heeft waarschuwingen voor groepen in het PRIV-forest vastgelegd


### <a name="how-to-upgrade"></a>Een upgrade uitvoeren

Klanten die een upgrade naar Microsoft Identity Manager 2016 Service Pack 1 willen uitvoeren, moeten de onderstaande instructies volgen voor alle services die van toepassing zijn op hun implementatie.

>[!Note]
>Klanten met Forefront Identity Manager 2010 R2 SP1 of eerder moeten eerst hun omgeving bijwerken naar Microsoft Identity Manager 2016, uitgebracht in augustus 2015, en vervolgens de onderstaande stappen volgen.

Voordat u begint

U moet de MIM-synchronisatie-engine upgraden voordat u de MIM-service en -portal bijwerkt.
U moet een back-up maken van de MIMService- en MIM-Sync-databases.

1. Het Microsoft Identity Manager-onderdeel dat u wilt bijwerken, verwijderen
2. Nadat de verwijdering is voltooid, opent u de welkomstpagina FIMSplash.htm in uw installatiemedia
3. Het MIM-onderdeel selecteren dat u wilt bijwerken
4. Ga door met de installatie door de prompts te volgen
   * Installatie van de MIM-service en -portal: als u Exchange Online als het e-mail-account selecteert, voert u het e-mailadres en de referenties van het Exchange Online-account in het volgende scherm in.

## <a name="version-4322660"></a>Versie 4.3.2266.0


* Status: MIM 2016-hotfix van 15 juli 2016; klanten moeten upgraden naar een recentere versie om ondersteund te blijven
* Meer informatie vindt u op [https://support.microsoft.com/en-us/kb/3171342](https://support.microsoft.com/en-us/kb/3171342)

## <a name="version-4321950"></a>Versie 4.3.2195.0

* Status: MIM 2016-hotfix van 20 maart 2016, klanten moeten upgraden naar een recentere versie om de ondersteuning te blijven
* Bijbehorend BHOLD-versie nummer: 5.0.3355.0
* Meer informatie vindt u op [https://support.microsoft.com/kb/3134725](https://support.microsoft.com/kb/3134725)
    
## <a name="version-4320640"></a>Versie 4.3.2064.0

* Status MIM 2016-hotfix van 11 december 2015; klanten moeten upgraden naar een recentere versie om ondersteund te blijven
* Bijbehorend BHOLD-versie nummer: 5.0.3176.0
* Meer informatie vindt u op [https://support.microsoft.com/kb/3092179](https://support.microsoft.com/kb/3092179)

## <a name="version-4319350"></a>Versie 4.3.1935.0

* Status MIM 2016 GA van 6 augustus 2015; klanten moeten upgraden naar een recentere versie om ondersteund te blijven
* Bijbehorend BHOLD-versie nummer: 5.0.3079.0

