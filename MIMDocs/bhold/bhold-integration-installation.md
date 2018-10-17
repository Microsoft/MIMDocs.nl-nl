---
title: Installatie van BHOLD FIM/MIM-integratie | Microsoft Docs
description: BHOLD-integratiemodule selfservicerol management toevoegen aan MIM en FIM
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 317c9ae4c940a509b6ac328cd5bb7cd7baa4dde9
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358802"
---
# <a name="bhold-fimmim-integration-installation"></a>Installatie van BHOLD FIM/MIM-integratie

De integratiemodule van BHOLD FIM toegevoegd selfservice rolbeheer op prijzen voor Microsoft Identity Manager, waardoor het mogelijk voor gebruikers om aan te vragen van aanvullende functies en afdwingen van die kunnen worden uitgevoerd op deze rollen. De integratie van BHOLD FIM-module een uitbreiding voor de FIM-Portal te maken voor het beheren van gebruikersrollen als onderdeel van de algehele FIM-beheer. In dit onderwerp wordt beschreven hoe u de infrastructuur van uw netwerk om te installeren en gebruiken van de integratiemodule van BHOLD FIM moet configureren. Ook wordt beschreven hoe u de integratiemodule van BHOLD FIM en configuratie die is vereist na de installatie van de integratie van BHOLD FIM-module wilt installeren.

## <a name="bhold-fim-integration-installation-requirements"></a>Vereisten voor installatie van BHOLD FIM-integratie

De integratie van BHOLD FIM-module een uitbreiding voor de FIM-Portal en FIM-Service zodat gebruikers voor het beheren van hun rollen in de FIM-Portal. Daarom is het essentieel dat de BHOLD-Core-module en de benodigde FIM-onderdelen worden geïnstalleerd en geconfigureerd voordat de integratie van BHOLD FIM-module installeren.
Hier volgen de softwareonderdelen die aanwezig zijn op de computer zijn moeten voordat u kunt de integratie van BHOLD FIM-module installeren:

- Microsoft Identity Manager 2016-Portal en Service
- Microsoft Silverlight 3 of hoger
- Internet informatieservices en ASP.NET
- Hulpprogramma's voor Microsoft Silverlight

Bovendien de modules BHOLD-Core en Access Management-Connector al moeten worden geïmplementeerd op een server in de omgeving en FIM moet worden geconfigureerd met een of meer BHOLD-beheeragents. Zie voor meer informatie over het installeren en configureren van de module BHOLD Core [basisinstallatie van BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Zie voor meer informatie over het installeren en gebruik van de module Access Management-Connector [Access Management-Connector installeren](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) en [Test Lab Guide: BHOLD-Access Management-Connector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

> [!IMPORTANT]
> De naam van de FIM-servicedatabase moet FIMService zijn. Installatie van BHOLD FIM Integration mislukt als FIM niet is geïnstalleerd met de standaardnaam FIM service-database.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met de integratie van BHOLD FIM-module installeren, moet u een BHOLD-map maken in de hoofdmap van de C:-schijf (C:\BHOLD).

U moet bovendien worden voorbereid voor de informatie die de installatiewizard van BHOLD FIM-integratie is vereist om de installatie te voltooien. Het werkblad voor het volgende kunt u gegevens vastleggen, zodat u klaar om aan te geven wanneer dat nodig is.

### <a name="bholdfim-account-settings"></a>BHOLDFim Accountinstellingen

| **Item**                            | **Beschrijving**                                                                                                                                                                                                               | **Waarde**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Security Provider worden gebruikt op domein** | Als er hebt geselecteerd, geeft de Active Directory Domain Services-beveiliging wordt toegangsbeheer voor BHOLD-Core.                                                                                                                    | Schakel het selectievakje in. **Belangrijk:** mislukt de installatie als dit selectievakje niet is geselecteerd.                                                                                                                                                                                                                   |
| **Domein**                          | Hiermee geeft u het domein waarin de **-serviceaccount** die u hebt gemaakt bij de installatie van BHOLD-Core. Zie voor meer informatie, [basisinstallatie van BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Naam van het domein wordt automatisch opgegeven door de wizard. De naam alleen wijzigen als dit onjuist is. **Belangrijk:** de domeinnaam opgeven met behulp van de naam van de NetBIOS-(kort), niet de volledig gekwalificeerde domeinnaam (FQDN). Bijvoorbeeld, als de FQDN-naam van het domein fabrikam.com, de domeinnaam opgeven als FABRIKAM. |
| **Gebruikersnaam**                        | Hiermee geeft u de naam van het gebruikersaccount van BHOLD-Core-service.                                                                                                                                                              | Schrijf hier de accountnaam van de gebruiker:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                        | Hiermee geeft u het wachtwoord van het serviceaccount van de gebruiker.                                                                                                                                                                           | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>FIM-Service-instellingen

| **Item**            | **Beschrijving**                                                                                                                                                                                                                               | **Waarde**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Gebruiker**            | Hiermee geeft u de naam van een account met administrator-bevoegdheden voor FIM. Microsoft raadt u aan het account die is gekoppeld aan de hoofdgebruiker in BHOLD-Core (standaard het account dat wordt gebruikt voor de installatie van BHOLD-Core) niet te gebruiken. | Schrijf hier de accountnaam van de gebruiker:                                                                   |
| **Wachtwoord**        | Hiermee geeft u het wachtwoord van de FIM-beheerdersaccount.                                                                                                                                                                                 | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie. |
| **FIM-Database**    | Hiermee geeft u de naam van de FIM-Service-database.                                                                                                                                                                                               | FIMService                                                                                          |
| **IP/poort van website** | Hiermee geeft u de naam of IP-adres van de FIM-Portal-server en de poort van website.                                                                                                                                                               | Schrijf de servernaam of het adres en de poort hier:                                                     |

### <a name="bhold-core-connection"></a>BHOLD-Core-verbinding

| **Item**               | **Beschrijving**                                                                                                                                                                                                                                                                                                                                                                               | **Waarde**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domein**             | Hiermee geeft u de naam van het domein van het account dat is opgegeven **gebruiker**hieronder. Geef het domein in (korte) NetBIOS-indeling.                                                                                                                                                                                                                                                                   | Domeinnaam van het gebruikersaccount hier schrijven?                                                            |
| **Gebruiker**               | Hiermee geeft u de naam van het account van **een BHOLD-gebruiker die een supervisor** van alle gebruikers en rollen en bevoegd is om te koppelen en ontkoppelen van gebruikersrollen. Microsoft raadt u aan het account die is gekoppeld aan de hoofdgebruiker in BHOLD-Core (standaard het account dat wordt gebruikt voor de installatie van BHOLD-Core) niet te gebruiken. Dit account mag hetzelfde account dat u kunt verbinding maken met FIM | Schrijf hier de accountnaam van de gebruiker:                                                                   |
| **Wachtwoord**           | Hiermee geeft u het wachtwoord van het gebruikersaccount dat is opgegeven in **gebruiker**.                                                                                                                                                                                                                                                                                                                             | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie. |
| **IP/Machine adres** | Hiermee geeft u het IP-adres van de server Core van BHOLD-website. Gebruik niet de naam van de server.                                                                                                                                                                                                                                                                                                        | Noteer het IP-adres:                                                                          |
| **Poortnummer**        | Hiermee geeft u het poortnummer van de website van BHOLD-Core.                                                                                                                                                                                                                                                                                                                                          | Schrijf hier het poortnummer van de:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Installatie van BHOLD FIM Integration

Meld u aan als een lid van de groep Domeinadministrators voor het installeren van de integratiemodule van BHOLD FIM, downloadt u het volgende bestand en als administrator uitvoeren op de server die u van plan bent de integratie van BHOLD FIM-module installeren op:

- BholdFIMIntegration<em>\<versie\></em>\_Release.msi

Vervang *\<versie\>* met het versienummer van de integratie van BHOLD FIM-versie die u installeert.

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

![MSI-bestand uitvoeren](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Taken na de installatie

Na de installatie van BHOLD FIM-integratie, moet u Microsoft SharePoint zodat de machtigingen voor site-eigenaren van BHOLD-service-account configureren. Als de FIM-Portal is geconfigureerd voor het gebruik van Secure Sockets Layer (SSL)-beveiliging, moet u ook bestanden die verwijzingen naar de adressen van BHOLD-pagina's toegevoegd aan de FIM-Portal bevatten wijzigen.

### <a name="configuring-sharepoint"></a>SharePoint configureren

Integratie van BHOLD FIM vereist werkt alleen goed als de BHOLD-serviceaccount moet lid zijn van het site-machtigingen in Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Verlenen van machtigingen aan het BHOLD-serviceaccount lid zijn van site

1.  Meld u op de server met BHOLD FIM-integratie met administrator-bevoegdheden.

2.  Klik op **Start**, en klik vervolgens op **Internet Verkenner**.

3.  Typ in de adresbalk <https://localhost> als SharePoint is geconfigureerd voor gebruik van SSL-beveiliging, anders typt <http://localhost>.

4.  Aan de linkerkant van de **teamsite** pagina, klikt u op **personen en groepen**.

5.  Onder **groepen** klikt u op **Site teamleden**, en in de werkbalk van het middelste deelvenster, klikt u op **nieuw**, en klik vervolgens op **gebruikers toevoegen**.

6.  Op de **gebruikers toevoegen: teamsite** pagina **gebruikers/groepen**, typt u BHOLDApplicationGroup en klik vervolgens op de knop Namen controleren onder de **gebruikers/groepen** vak. Naam van de groep moet worden omgezet naar de domeinnaam bevatten.

7.  Klik op **rechtstreeks gebruikersmachtigingen geven**, selecteer **volledig beheer – heeft volledig beheer**, en klik vervolgens op **OK**.

8.  Controleer of BHOLDApplicationGroup wordt weergegeven **machtigingen: teamsite**, en sluit vervolgens Internet Explorer.

![MSI-bestand uitvoeren](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>BHOLD ter ondersteuning van SSL configureren

Als de FIM-Portal is geconfigureerd voor het gebruik van SSL-beveiliging, moet u de bestanden op de FIM-server wijzigen zodat koppelingen naar BHOLD-pagina wordt geopend. De bestanden bevinden zich in de volgende map: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

De volgende tabel bevat de bestanden en de oorspronkelijke en gewijzigde versies van de tekenreeksen die moeten worden bewerkt.

| **Bestand**                  | **Oorspronkelijke tekenreeks**                                                                                                                   | **Gewijzigde tekenreeks**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.HTML | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.HTML       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.HTML |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.HTML |
| Selfservice.aspx          | RoleExchangePoint=http://\<*FIM_Server*\>: \<*FIM_Port*\>/BHOLD RoleExchangePoint/BHOLDRoleExchangePoint.svc,TransportMode=Transport | RoleExchangePoint=https://\<*FIM_Server_FQDN*\>: \<*FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Waar:

-   *\<BHOLD_Server\>* geeft de naam van de server BHOLD zoals gevonden in de oorspronkelijke versie van het bestand

-   *\<MIM_Server\>* geeft de naam van de FIM-server zoals gevonden in de oorspronkelijke versie van het bestand

-   *\<BHOLD_Server_FQDN\>* Hiermee geeft u de volledig gekwalificeerde domeinnaam (FQDN) van de BHOLD-server

-   *\<MIM_Port\>* geeft u het poortnummer van de FIM-server zoals gevonden in de oorspronkelijke versie van het bestand

-   *\<MIM_Server_FQDN\>* geeft de FQDN van de FIM-server

-   *\<MIM_SSL_Port\>* geeft een andere poort voor gebruik met SSL op de FIM-server

### <a name="enable-approval-workflows-in-bhold-core"></a>Goedkeuringswerkstromen in BHOLD Core inschakelen

Als FIM en BHOLD voor self-service zijn geïntegreerd, worden de werkstromen voor goedkeuringen uitgevoerd in de FIM-Service. Dit is vergelijkbaar met het model van de werkstroom voor aanvragen die afkomstig uit de FIM-Portal zijn, zoals wanneer een gebruiker een aanvraag voor deelname aan een distributielijst indient. Er zijn belangrijke verschillen tussen BHOLD rol werkstromen en andere werkstromen die in de FIM-Service, maar worden gehost. In het geval van BHOLD afkomstig werkstroomparameters die opgeeft welke gebruikers een aanvraag om de rol moeten goedkeuren is van BHOLD, in plaats van in de definitie van de werkstroom in de FIM-Service-database worden opgeslagen. Deze parameters zijn die de FIM-Service van BHOLD wanneer de eerste aanvraag wordt gedaan en een actiewerkstroom de resultaten terug naar BHOLD-Core.

BHOLD selecteert een goedkeurder een aanvraag selfservice op drie manieren:

-   **Lijnmanager als fiatteur: selectie op basis van rollen voor een organisatie-eenheid (OrgUnit)** als een rol heeft een kenmerk met de naam roletype die is ingesteld op goedkeurder of roltrap, en die rol is gekoppeld aan een of meer gebruikers in de context van een OrgUnit, aanvragen van gebruikers binnen die OrgUnit moeten worden goedgekeurd door een van de gebruikers die is gekoppeld aan de rol die de goedkeurder of roltrap roletype.

-   **Lijnmanager als fiatteur: selectie op basis van een kenmerk voor een OrgUnit** elke OrgUnit kan hebben een of meer kenmerken die de aliassen van gebruikers die kunnen roltoewijzingen voor andere gebruikers in de OrgUnit goedkeuren opgeven. Deze kenmerken zijn benoemde approver1, approver2, enzovoort. Wanneer een gebruiker in de OrgUnit vraagt om een roltoewijzing, stuurt de aanvraag (door middel van FIM) door BHOLD aan de gebruikers die zijn opgegeven door de kenmerken van de fiatteur OrgUnit. Als een OrgUnit beschikt niet over een van deze set kenmerken, controleert BHOLD bovenliggende OrgUnits tot de hoofdmap OrgUnit.

-   **Rolbeheer als fiatteur: selectie op basis van een kenmerk voor een rol** een rol kan een of meer kenmerken hebben (ook met de naam approver1, enzovoort) die de aliassen van gebruikers die de toewijzing van de rol kunnen goedkeuren opgeeft. Wanneer een gebruiker vraagt om een rol heeft deze goedkeurder kenmerken instellen die worden toegewezen, stuurt BHOLD de aanvraag door naar de gebruikers die zijn opgegeven door de kenmerken.

Als een goedkeurder een aanvraag om de rol selfservice niet is opgegeven door een van deze methoden, standaard wijst BHOLD automatisch de rol zonder goedkeuring. Om deze reden moet onmiddellijk na de installatie van BHOLD FIM-integratie, u configureren de hoofdmap OrgUnit met de alias van de fiatteur, zoals het root-account. Hiermee wordt voorkomen dat een gebruiker per ongeluk wordt verleend een rol voordat een uitgebreidere goedkeuring-beleid kan worden geïmplementeerd.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Een goedkeurder voor de basis-OrgUnit configureren

1.  Meld u aan de BHOLD-Core-server aan als beheerder.

2.  Klik op **Start**, en klik vervolgens op **Internet Explorer**.

3.  Typ in de adresbalk van Internet Explorer <http://localhost:5151/bhold/core>, en druk vervolgens op ENTER.

4.  Op de belangrijkste BHOLD startpagina, onder **kenmerk def**, klikt u op **kenmerken van het type**.

5.  Op de **kenmerktype** pagina, klikt u op **toevoegen**.

6.  Op de **pagina van type kenmerk toevoegen**in **identiteit**, approver1, typ in het **gegevenstype** lijst, klikt u op **alfanumeriek**, in **Maximumlengte**, typt u 255, in **lijst met waarden**, klikt u op **Nee**in **Engels**, typt u 1 van de fiatteur, klikt u op **OK**, en klik vervolgens op **gedaan**.

7.  In het linkerdeelvenster onder **kenmerk def** klikt u op **kenmerk type sets**.

8.  Op de **kenmerk type sets** pagina, klikt u op **toevoegen**.

9.  Op de **kenmerk type set toevoegen** pagina **beschrijving**, typt u OrgUnit kenmerken en klik vervolgens op **OK**.

10. Op de **OrgUnit kenmerken** pagina uit, vouw **kenmerken van het type**, en klik vervolgens op **wijzigen**.

11. In de **kenmerktype** lijst, klikt u op **approver1**, klikt u op **toevoegen**, en klik vervolgens op **gedaan**.

12. Klik in het linkerdeelvenster op **objecttypen**.

13. Op de **objecttypen** pagina, klikt u op **OrgUnit**.

14. Op de **Object van type/OrgUnit** pagina uit, vouw **kenmerk type sets**, en klik vervolgens op **wijzigen**.

15. Op de **koppelen kenmerk type set/OrgUnit** pagina **volgorde**, 10, typt u in de **type kenmerkset** lijst, klikt u op **OrgUnit kenmerken**, Klik op **toevoegen**, en klik vervolgens op **gedaan**.

16. In het linkerdeelvenster onder **Model**, klikt u op **organisatie-eenheden**.

17. Op de **organisatie-eenheden** pagina, klikt u op **hoofdmap**.

18. Op de **organisatie-eenheid/root** pagina, klikt u op **wijzigen**.

19. Op de **wijzigen van de organisatie-eenheid kenmerken/root** pagina **goedkeurder**, typt u de naam van de domein- en gebruikersnaam van de gebruiker die roltoewijzing aanvragen, in de indeling goedkeuren zal  *\<domein\>*\\*\<gebruiker\>*, waarbij *\<domein\>* is de (Kort) NetBIOS-domeinnaam en *\<gebruiker\>* aanmeldingsnaam van de gebruiker is.
20. Klik op **OK**.

> [!IMPORTANT]
> De naam van de domein- en gebruikersnaam moet overeenkomen met de standaardalias van een gebruiker in de kern van BHOLD-database.

Als alternatief voor het opgeven van een goedkeurder voor organisatie-eenheden, kunt u een goedkeurder voor voorgestelde functies in de kern van BHOLD-database. Om dit te doen, maken het kenmerk approver1, een kenmerk worden vervangen toevoegen die zijn gekoppeld aan het objecttype van de rol en wijzig vervolgens elke voorgestelde rol om op te geven van de fiatteur.

Voor betere beveiliging van de werkstroom, naast de fiatteurs, moet u aanvullende goedkeuring modi en gebruikers opgeven door te maken en invullen van de volgende kenmerken voor OrgUnits en rollen:

- roltrap<em>\<n\></em>

- de eigenaar van<em>\<n\></em>

- securityOfficer<em>\<n\></em>

- melding<em>\<n\></em>

waar *\<n\>* geeft aan dat een optioneel numeriek achtervoegsel voor meerdere kenmerken van hetzelfde type.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Goedkeuringswerkstromen die zijn geconfigureerd in de FIM-Service controleren

Installatie van BHOLD FIM-integratie maakt sets, werkstroomdefinities en beheerbeleidsregels (MPR's) naar de FIM-Service. Als u uw FIM-implementatie om de sets administrators of de sets van gebruikers die kunnen aanvragen te wijzigen is aangepast, moet u ervoor zorgen dat de beheerbeleidsregels verwijzen naar de juiste gebruiker ingesteld.

> [!NOTE]
> Voordat gebruikers van de FIM-Portal de selfservice-functies van BHOLD gebruiken kunnen, moeten de gebruikersaccounts worden gesynchroniseerd met de BHOLD-database van de FIM-synchronisatieservice. In het bijzonder, moet er een gebruikersrecord in de kern van BHOLD-database en in de FIM-Service-database voor elke gebruiker die u kunt een self-servicegebruikers indienen of is opgegeven als een goedkeurder of roltrap voor self-service aanvragen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het installeren van FIM-Portal en andere functies FIM [Planning en architectuur](https://technet.microsoft.com/library/ee808044.aspx) in de technische bibliotheek van Microsoft Forefront.
- [Handleiding voor BHOLD-installatie](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
