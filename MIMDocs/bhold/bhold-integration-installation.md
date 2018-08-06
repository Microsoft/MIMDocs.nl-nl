---
title: Installatie van BHOLD FIM/MIM-integratie | Microsoft Docs
description: BHOLD-integratiemodule selfservice rollenbeheer toevoegen aan de MIM- en FIM
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 08a0aaa60891727482e80c8998cc075eacf042cf
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290166"
---
# <a name="bhold-fimmim-integration-installation"></a>Installatie van BHOLD FIM/MIM-integratie

De BHOLD FIM-integratiemodule voegt selfservice rollenbeheer aan Microsoft Identity Manager, waardoor het mogelijk voor gebruikers om aan te vragen van aanvullende functies en het afdwingen van die deze rollen kunnen ondernemen. De BHOLD FIM-integratiemodule breidt de FIM-Portal zodat gemakkelijk voor het beheren van gebruikersrollen als onderdeel van de algehele FIM-beheer. Dit onderwerp wordt beschreven hoe u uw netwerkinfrastructuur zodat u kunt installeren en gebruiken van de integratiemodule BHOLD FIM moet configureren. Ook wordt beschreven hoe de integratiemodule van BHOLD FIM en de configuratie die is vereist na de installatie van de integratiemodule BHOLD FIM wilt installeren.

## <a name="bhold-fim-integration-installation-requirements"></a>Installatievereisten BHOLD FIM-integratie

De BHOLD FIM-integratiemodule breidt de FIM-Portal en FIM-Service zodat gebruikers voor het beheren van de bijbehorende rollen binnen de FIM-Portal. Daarom is het essentieel dat de Core BHOLD-module en de benodigde FIM-onderdelen worden geïnstalleerd en geconfigureerd voordat de integratie van BHOLD FIM-module installeren.
Hier volgen de softwareonderdelen die aanwezig zijn op de computer zijn moeten voordat u de BHOLD FIM-integratiemodule kunt installeren:

- Microsoft Identity Manager 2016-Portal en -Service
- Microsoft Silverlight 3 of hoger
- Internet informatieservices en ASP.NET
- Hulpprogramma's voor Microsoft Silverlight

Bovendien moeten de modules BHOLD-Core en Access Management-Connector al geïmplementeerd op een server in de omgeving en FIM moet worden geconfigureerd met een of meer BHOLD-beheeragents. Zie voor meer informatie over het installeren en configureren van de module BHOLD Core [BHOLD Core-installatie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Zie voor meer informatie over het installeren en met behulp van de module Access Management-Connector [Access Management-Connectorinstallatie](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) en [Test Lab Guide: BHOLD Access Management-Connector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

> [!IMPORTANT]
> De naam van de FIM-servicedatabase moet FIMService zijn. BHOLD FIM-integratie-instellingen mislukt als FIM niet is geïnstalleerd met de standaardnaam FIM service-database.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met het installeren van de integratiemodule BHOLD FIM, moet u een BHOLD-map maken in de hoofdmap van het schijfstation C: (C:\BHOLD).

Bovendien moet u de gegevens die de installatiewizard van BHOLD FIM-integratie is vereist om de installatie te voltooien worden voorbereid. Het werkblad voor het volgende kunt u gegevens vastleggen, zodat u gereed om te leveren wanneer deze nodig is.

### <a name="bholdfim-account-settings"></a>BHOLDFim Accountinstellingen

| **Item**                            | **Beschrijving**                                                                                                                                                                                                               | **Waarde**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beveiligingsprovider in domein gebruiken** | Wanneer u selecteert, geeft u aan dat Active Directory Domain Services-beveiliging de toegang tot BHOLD Core beheert.                                                                                                                    | Schakel het selectievakje in. **Belangrijk:** mislukt de installatie als dit selectievakje niet is ingeschakeld.                                                                                                                                                                                                                   |
| **Domein**                          | Hiermee geeft u het domein waarin de **-serviceaccount** dat u hebt gemaakt bij het installeren van BHOLD-Core. Zie voor meer informatie [BHOLD Core-installatie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Naam van het domein wordt automatisch opgegeven door de wizard. Wijzig de naam alleen als dit onjuist is. **Belangrijk:** de domeinnaam opgeven met de naam van de NetBIOS-(korte), niet de volledig gekwalificeerde domeinnaam (FQDN). Bijvoorbeeld, als de FQDN-naam van het domein fabrikam.com is, geef de domeinnaam als FABRIKAM. |
| **Gebruikersnaam**                        | Hiermee geeft u de aanmeldingsnaam van de gebruikersaccount van de Core BHOLD-service.                                                                                                                                                              | Schrijf hier de accountnaam van de gebruiker:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                        | Hiermee geeft u het wachtwoord van het serviceaccount van de gebruiker.                                                                                                                                                                           | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>FIM-Service-instellingen

| **Item**            | **Beschrijving**                                                                                                                                                                                                                               | **Waarde**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Gebruiker**            | Hiermee geeft u de naam van een account met administrator-bevoegdheden voor FIM. Microsoft raadt u aan het account dat is gekoppeld aan de hoofdgebruiker in BHOLD Core (standaard is dit het account dat wordt gebruikt voor het installeren van BHOLD-Core) niet te gebruiken. | Schrijf hier de accountnaam van de gebruiker:                                                                   |
| **Wachtwoord**        | Hiermee geeft u het wachtwoord van de FIM-beheerdersaccount.                                                                                                                                                                                 | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie. |
| **FIM Database**    | Hiermee geeft u de naam van de FIM-servicedatabase.                                                                                                                                                                                               | FIMService                                                                                          |
| **IP/poort van website** | Hiermee geeft u de naam of IP-adres van de FIM-Portal-server en de poort van website.                                                                                                                                                               | De servernaam of het adres en de poort hier schrijven:                                                     |

### <a name="bhold-core-connection"></a>BHOLD-Core verbinding

| **Item**               | **Beschrijving**                                                                                                                                                                                                                                                                                                                                                                               | **Waarde**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domein**             | Hiermee geeft u de naam van het domein van het account opgegeven in **gebruiker**onderstaande. Geef het domein (korte) NetBIOS-indeling.                                                                                                                                                                                                                                                                   | De domein gebruikersaccountnaam hier schrijven?                                                            |
| **Gebruiker**               | Hiermee geeft u de aanmeldingsnaam van de account van **BHOLD-gebruiker die een supervisor** van alle gebruikers en -rollen en gemachtigd is om te koppelen en ontkoppelen van gebruikersrollen. Microsoft raadt u aan het account dat is gekoppeld aan de hoofdgebruiker in BHOLD Core (standaard is dit het account dat wordt gebruikt voor het installeren van BHOLD-Core) niet te gebruiken. Dit account kan worden hetzelfde account waarmee u verbinding maken met FIM | Schrijf hier de accountnaam van de gebruiker:                                                                   |
| **Wachtwoord**           | Hiermee geeft u het wachtwoord van het gebruikersaccount dat is opgegeven in **gebruiker**.                                                                                                                                                                                                                                                                                                                             | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie. |
| **Adres van de IP/Machine** | Hiermee geeft u het IP-adres van de server Core van BHOLD-website. Gebruik niet de naam van de server.                                                                                                                                                                                                                                                                                                        | Noteer het IP-adres:                                                                          |
| **Poortnummer**        | Hiermee geeft u het poortnummer van de Core BHOLD-website.                                                                                                                                                                                                                                                                                                                                          | Schrijf het poortnummer hier:                                                                         |

## <a name="bhold-fim-integration-setup"></a>BHOLD FIM-integratie-instellingen

Voor het installeren van de integratiemodule BHOLD FIM aanmelden als lid van de groep Domeinadministrators, moet het volgende bestand downloaden en uitvoeren als administrator op de server die u wilt de integratie van BHOLD FIM-module installeren op:

- BholdFIMIntegration<em>\<versie\></em>\_Release.msi

Vervang *\<versie\>* met het versienummer van de integratie van BHOLD FIM-versie die u installeert.

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

![msi wordt uitgevoerd](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Taken na de installatie

U moet Microsoft SharePoint zodat de machtigingen voor site-eigenaren van BHOLD service-account configureren na de installatie van BHOLD FIM-integratie. Als de FIM-Portal voor het gebruik van Secure Sockets Layer (SSL)-beveiliging is geconfigureerd, moet u ook bestanden met verwijzingen naar de adressen van BHOLD-pagina's toegevoegd aan de FIM-Portal wijzigen.

### <a name="configuring-sharepoint"></a>SharePoint configureren

BHOLD FIM-integratie vereist werkt alleen goed als de BHOLD-serviceaccount lid zijn van het site-machtigingen hebben in Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Lid zijn van het site-machtigen om de BHOLD-serviceaccount

1.  Meld u bij de server met BHOLD FIM-integratie met administrator-bevoegdheden.

2.  Klik op **Start**, en klik vervolgens op **Internet Verkenner**.

3.  Typ in de adresbalk <https://localhost> als SharePoint is geconfigureerd voor gebruik van SSL-beveiliging, anders typt <http://localhost>.

4.  Aan de linkerkant van de **teamsite** pagina, klikt u op **personen en groepen**.

5.  Onder **groepen** klikt u op **Site teamleden**, en klik op de werkbalk middelste deelvenster op **nieuw**, en klik vervolgens op **gebruikers toevoegen**.

6.  Op de **gebruikers toevoegen: teamsite** pagina **gebruikers/groepen**, typt u BHOLDApplicationGroup en klik op de knop Namen controleren onder de **gebruikers/groepen** vak. Naam van de groep moet worden omgezet naar de domeinnaam omvatten.

7.  Klik op **machtigen gebruikers rechtstreeks**, selecteer **volledig beheer – heeft volledig beheer**, en klik vervolgens op **OK**.

8.  Controleer of BHOLDApplicationGroup wordt vermeld **machtigingen: teamsite**, en sluit Internet Explorer.

![msi wordt uitgevoerd](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>BHOLD ter ondersteuning van SSL configureren

Als de FIM-Portal voor het gebruik van SSL-beveiliging is geconfigureerd, moet u de bestanden op de FIM-server wijzigen zodat u koppelingen naar BHOLD-pagina's wordt geopend. De bestanden bevinden zich in de volgende map: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

De volgende tabel bevat de bestanden en de oorspronkelijke en gewijzigde versies van de tekenreeksen die moeten worden bewerkt.

| **File**                  | **Oorspronkelijke tekenreeks**                                                                                                                   | **Gewijzigde tekenreeks**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Selfservice.aspx          | RoleExchangePoint=http://\<*FIM_Server*\>: \<*FIM_Port*\>/BHOLD RoleExchangePoint/BHOLDRoleExchangePoint.svc,TransportMode=Transport | RoleExchangePoint=https://\<*FIM_Server_FQDN*\>: \<*FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Waarbij:

-   *\<BHOLD_Server\>* geeft de naam van de server BHOLD zoals gevonden in de oorspronkelijke versie van het bestand

-   *\<MIM_Server\>* geeft de naam van de FIM-server zoals gevonden in de oorspronkelijke versie van het bestand

-   *\<BHOLD_Server_FQDN\>* Hiermee geeft u de volledig gekwalificeerde domeinnaam (FQDN) van de BHOLD-server

-   *\<MIM_Port\>* geeft u het poortnummer van de FIM-server zoals gevonden in de oorspronkelijke versie van het bestand

-   *\<MIM_Server_FQDN\>* geeft de FQDN van de FIM-server

-   *\<MIM_SSL_Port\>* geeft een andere poort voor gebruik met SSL op de FIM-server

### <a name="enable-approval-workflows-in-bhold-core"></a>Van goedkeuringswerkstromen in BHOLD Core inschakelen

Als FIM en BHOLD voor self-service zijn geïntegreerd, worden de werkstromen voor goedkeuringen uitgevoerd in de FIM-Service. Dit is vergelijkbaar met het model van de werkstroom voor aanvragen die afkomstig uit de FIM-Portal zijn, zoals wanneer een gebruiker een aanvraag voor het koppelen van een distributielijst verzendt. Er zijn belangrijke verschillen tussen BHOLD rol werkstromen en andere werkstromen echter gehost in de FIM-Service. In het geval van BHOLD afkomstig Workflowparameters die opgeeft welke gebruikers een rol-aanvraag moeten goedkeuren in BHOLD in plaats van wordt opgeslagen in de definitie van de werkstroom in de FIM-servicedatabase. Deze parameters zijn bedoeld om de FIM-Service door BHOLD wanneer de eerste aanvraag wordt gedaan en een actiewerkstroom de resultaten teruggestuurd naar BHOLD Core.

BHOLD selecteert u een goedkeurder voor een aanvraag selfservice op drie manieren:

-   **Lijnmanager als goedkeurder: selectie op basis van rollen voor een organisatie-eenheid (OrgUnit)** als een rol heeft een kenmerk genaamd roletype die is ingesteld op goedkeurder of roltrap, en als die rol is gekoppeld aan een of meer gebruikers in de context van een OrgUnit, aanvragen van gebruikers binnen die OrgUnit moeten worden goedgekeurd door een van de gebruikers die is gekoppeld aan de rol met de goedkeurder of roltrap roletype.

-   **Lijnmanager als goedkeurder: selectie op basis van kenmerken voor een OrgUnit** elke OrgUnit kan een of meer kenmerken opgeven van de gebruikers die roltoewijzingen voor andere gebruikers in de OrgUnit goedkeuren kunnen-aliassen hebben. Deze kenmerken zijn benoemde approver1, approver2, enzovoort. Wanneer een gebruiker in de OrgUnit een roltoewijzing aanvraagt, routeert BHOLD de aanvraag (via FIM) aan de gebruikers die is opgegeven door de kenmerken van de goedkeurder OrgUnit. Als een OrgUnit waarop geen van deze kenmerken is ingesteld, controleert BHOLD bovenliggende OrgUnits tot de hoofdmap OrgUnit.

-   **Rolmanager als goedkeurder: selectie op basis van kenmerken voor een functie** een rol kan een of meer kenmerken hebben (ook benoemde approver1, enzovoort) die de aliassen van gebruikers die de toewijzing van de rol kunnen goedkeuren. Wanneer een gebruiker wil een rol heeft die over de kenmerken van deze goedkeurder ingesteld worden toegewezen, stuurt BHOLD de aanvraag door naar de gebruikers die zijn opgegeven door de kenmerken.

Als een goedkeurder voor een aanvraag voor een selfservice-rol niet is opgegeven door een van deze methoden, standaard wijst BHOLD automatisch de rol zonder goedkeuring. Om deze reden moet onmiddellijk na de installatie van BHOLD FIM-integratie, u de hoofdmap OrgUnit met configureren de alias van een goedkeurder, zoals het root-account. Hierdoor kan een gebruiker niet per ongeluk wordt verleend een rol voordat een uitgebreidere goedkeuring beleid kan worden geïmplementeerd.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Een goedkeurder voor de basis-OrgUnit configureren

1.  Meld u aan de BHOLD-Core-server aan als beheerder.

2.  Klik op **Start**, en klik vervolgens op **Internet Explorer**.

3.  Typ in de adresbalk van Internet Explorer <http://localhost:5151/bhold/core>, en druk vervolgens op ENTER.

4.  Voor de belangrijkste BHOLD startpagina, onder **kenmerk def**, klikt u op **kenmerken van het type**.

5.  Op de **kenmerktype** pagina, klikt u op **toevoegen**.

6.  Op de **toevoegen kenmerk type pagina**in **identiteit**, approver1, typ in het **gegevenstype** lijst, klikt u op **alfanumeriek**, in **Maximumlengte**, 255, typt u in **lijst met waarden**, klikt u op **Nee**in **Engels**, typt u goedkeurder 1, klik op **OK**, en klik vervolgens op **gedaan**.

7.  In het linkerdeelvenster onder **kenmerk def** klikt u op **kenmerk type sets**.

8.  Op de **kenmerk type sets** pagina, klikt u op **toevoegen**.

9.  Op de **kenmerk type set toevoegen** pagina **beschrijving**, typt u OrgUnit kenmerken en klik op **OK**.

10. Op de **OrgUnit kenmerken** pagina uit, vouw **kenmerken van het type**, en klik vervolgens op **wijzigen**.

11. In de **kenmerktype** lijst, klikt u op **approver1**, klikt u op **toevoegen**, en klik vervolgens op **gedaan**.

12. Klik in het linkerdeelvenster op **objecttypen**.

13. Op de **objecttypen** pagina, klikt u op **OrgUnit**.

14. Op de **Object van type/OrgUnit** pagina uit, vouw **kenmerk type sets**, en klik vervolgens op **wijzigen**.

15. Op de **koppelen kenmerk type set/OrgUnit** pagina **volgorde**, 10, typ in de **type kenmerkenset** lijst, klikt u op **OrgUnit kenmerken**, Klik op **toevoegen**, en klik vervolgens op **gedaan**.

16. In het linkerdeelvenster onder **Model**, klikt u op **organisatie-eenheden**.

17. Op de **organisatie-eenheden** pagina, klikt u op **hoofdmap**.

18. Op de **organisatie-eenheid/root** pagina, klikt u op **wijzigen**.

19. Op de **wijzigen van de organisatie-eenheid kenmerken/root** pagina **goedkeurder**, typ de naam van het domein en gebruikersnaam van de gebruiker die roltoewijzing aanvragen, in de indeling goedkeurt  *\<domein\>*\\*\<gebruiker\>*, waarbij *\<domein\>* is de (Korte) NetBIOS-domeinnaam en *\<gebruiker\>* aanmeldingsnaam van de gebruiker.
20. Klik op **OK**.

> [!IMPORTANT]
> De naam van de domein- en gebruikersnaam moet overeenkomen met de standaardalias van een gebruiker in de kern van BHOLD-database.

Als alternatief voor het opgeven van een goedkeurder voor organisatie-eenheden, kunt u een goedkeurder voor voorgestelde functies in de kern van BHOLD-database. Het kenmerk approver1 maken om dit te doen, toevoegen om een kenmerk te vervangen die zijn gekoppeld aan de rol van het type en wijzigt u elke voorgestelde rol om op te geven van de goedkeurder.

Betere beveiliging van de werkstroom, naast goedkeurders, moet u aanvullende goedkeuring modi en gebruikers aanwijzen door het maken en vullen met de volgende kenmerken voor OrgUnits en rollen:

- roltrap<em>\<n\></em>

- eigenaar<em>\<n\></em>

- securityOfficer<em>\<n\></em>

- melding<em>\<n\></em>

waar *\<n\>* geeft een optionele numeriek achtervoegsel zodat meerdere kenmerken van hetzelfde type.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Werkstromen voor goedkeuring geconfigureerd in de FIM-Service controleren

Installatie van BHOLD FIM-integratie maakt sets, werkstroomdefinities en beheerbeleidsregels (MPR's) naar de FIM-Service. Als u uw FIM-implementatie om de sets van administrators of de sets van gebruikers die kunnen aanvragen te wijzigen is aangepast, moet u ervoor zorgen dat de MPR's verwijzen naar de juiste gebruiker sets.

> [!NOTE]
> Voordat gebruikers van de FIM-Portal de selfservicefuncties geleverd door BHOLD gebruiken kunnen, moeten de gebruikersaccounts worden gesynchroniseerd in de BHOLD-database van de FIM-synchronisatieservice. In het bijzonder, moet er een gebruikersrecord in de kern van BHOLD-database en in de FIM-servicedatabase voor elke gebruiker die u kunt een selfservice indienen of is opgegeven als een goedkeurder of roltrap voor self-service aanvragen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het installeren van FIM-Portal en andere functies FIM [Planning en architectuur](https://technet.microsoft.com/library/ee808044.aspx) in de technische bibliotheek van Microsoft Forefront.
- [BHOLD-installatiehandleiding](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
