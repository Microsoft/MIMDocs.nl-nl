---
title: Installatie van BHOLD FIM/MIM-integratie | Microsoft Docs
description: BHOLD Integration-module Voeg Self-Service Role Management toe aan MIM en FIM
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3005e06606ec4b3b6854003213c712770376b35d
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042199"
---
# <a name="bhold-fimmim-integration-installation"></a>Installatie van BHOLD FIM/MIM-integratie

De BHOLD FIM-integratie module voegt Self-Service Role Management toe aan Microsof Identity Manager, zodat gebruikers extra rollen kunnen aanvragen en afdwingen wie deze rollen kan uitvoeren. De FIM-integratie module van BHOLD breidt de FIM-Portal uit om het eenvoudig te maken om de rollen van gebruikers te beheren als onderdeel van het algemene FIM-beheer. In dit onderwerp wordt beschreven hoe u uw netwerk infrastructuur moet configureren om de BHOLD FIM-integratie module te kunnen installeren en gebruiken. Ook wordt uitgelegd hoe u de BHOLD FIM-integratie module en-configuratie installeert die vereist zijn nadat u de BHOLD FIM-integratie module hebt geïnstalleerd.

## <a name="bhold-fim-integration-installation-requirements"></a>Installatie vereisten voor BHOLD FIM-integratie

De FIM-integratie module BHOLD breidt de FIM-Portal en FIM-service uit om gebruikers in staat te stellen hun rollen in de FIM-Portal te beheren. Daarom is het van essentieel belang dat de BHOLD-kern module en de benodigde FIM-functies worden geïnstalleerd en geconfigureerd voordat u de BHOLD FIM-integratie module installeert.
De volgende software onderdelen moeten aanwezig zijn op de computer voordat u de BHOLD FIM-integratie module kunt installeren:

- Microsoft Identity Manager 2016-Portal en-service
- Micro soft Silverlight 3 of hoger
- Internet Information Services en ASP.NET
- Micro soft Silverlight-Hulpprogram Ma's

Daarnaast moeten de BHOLD core-en Access Management-connector modules al zijn geïmplementeerd op een server in de omgeving, en FIM moet worden geconfigureerd met een of meer BHOLD-beheer agenten. Zie [BHOLD Core-installatie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)voor meer informatie over het installeren en configureren van de BHOLD core-module. Voor informatie over het installeren en gebruiken van de module toegangs beheer connector, Zie [installatie van toegangs beheer connector](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) en [test lab Guide: BHOLD Access Management connector (Engelstalig](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx)).

> [!IMPORTANT]
> De naam van de FIM-service database moet FIMService zijn. Setup van BHOLD FIM-integratie mislukt als FIM niet is geïnstalleerd met de standaard naam van de FIM-service database.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met de installatie van de BHOLD FIM-integratie module, moet u een BHOLD-map maken in de hoofdmap van station C: schijf (C:\BHOLD).

Bovendien moet u voor bereid zijn om de informatie op te geven die door de installatie wizard van de BHOLD-FIM-integratie moet worden uitgevoerd om de installatie te volt ooien. Het volgende werk blad helpt u bij het vastleggen van die informatie, zodat u deze kunt opgeven wanneer dat nodig is.

### <a name="bholdfim-account-settings"></a>BHOLDFim-account instellingen

| **Item**                            | **Beschrijving**                                                                                                                                                                                                               | **Waarde**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beveiligings provider op domein gebruiken** | Hiermee geeft u op dat Active Directory Domain Services beveiliging de toegang tot BHOLD core beheert.                                                                                                                    | Schakel het selectievakje in. **Belang rijk:** Als dit selectie vakje niet is ingeschakeld, mislukt de installatie.                                                                                                                                                                                                                   |
| **Domain**                          | Hiermee geeft u het domein op dat het **Service account** bevat dat u hebt gemaakt bij het installeren van de BHOLD-kern. Zie [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)(Engelstalig) voor meer informatie. | De domein naam wordt automatisch geleverd door de wizard. Wijzig de naam alleen als deze onjuist is. **Belang rijk:** Geef de domein naam op met behulp van de NetBIOS (korte) naam, niet de Fully Qualified Domain Name (FQDN). Als de FQDN van het domein bijvoorbeeld fabrikam.com is, geeft u de domein naam op als FABRIKAM. |
| **Gebruikers**                        | Hiermee geeft u de aanmeldings naam van het gebruikers account van de BHOLD-basis service.                                                                                                                                                              | Schrijf hier de naam van het gebruikers account:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                        | Hiermee geeft u het wacht woord van het Service gebruikers account.                                                                                                                                                                           | Schrijf hier het wacht woord: **belang rijk:** zorg ervoor dat u dit wacht woord op een verborgen, veilige locatie blijft.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>FIM-service-instellingen

| **Item**            | **Beschrijving**                                                                                                                                                                                                                               | **Waarde**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Gebruiker**            | Hiermee geeft u de aanmeldings naam op van een account met beheerders bevoegdheden voor FIM. Micro soft raadt u ten zeerste aan het account dat is gekoppeld aan de hoofd gebruiker in BHOLD core (standaard het account dat wordt gebruikt voor het installeren van BHOLD core) niet te gebruiken. | Schrijf hier de naam van het gebruikers account:                                                                   |
| **Wachtwoord**        | Hiermee geeft u het wacht woord van het gebruikers account van de FIM-beheerder.                                                                                                                                                                                 | Schrijf hier het wacht woord: **belang rijk:** zorg ervoor dat u dit wacht woord op een verborgen, veilige locatie blijft. |
| **FIM-data base**    | Hiermee geeft u de naam op van de FIM-service database.                                                                                                                                                                                               | FIMService                                                                                          |
| **IP/poort van website** | Hiermee geeft u de naam of het IP-adres van de FIM-Portal Server en de website poort op.                                                                                                                                                               | Schrijf hier de server naam of het adres en de poort:                                                     |

### <a name="bhold-core-connection"></a>BHOLD core-verbinding

| **Item**               | **Beschrijving**                                                                                                                                                                                                                                                                                                                                                                               | **Waarde**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domain**             | Hiermee geeft u de naam op van het domein van het account dat is opgegeven in **gebruiker**, hieronder. Geef het domein op in de indeling NetBIOS (korte).                                                                                                                                                                                                                                                                   | De domein naam van het gebruikers account hier schrijven?                                                            |
| **Gebruiker**               | Hiermee geeft u de aanmeldings naam op van het account van **een BHOLD-gebruiker die een super Visor is** van alle gebruikers en rollen en toestemming heeft om gebruikers rollen te koppelen en ontkoppelen. Micro soft raadt u ten zeerste aan het account dat is gekoppeld aan de hoofd gebruiker in BHOLD core (standaard het account dat wordt gebruikt voor het installeren van BHOLD core) niet te gebruiken. Dit account kan hetzelfde account zijn dat u gebruikt om verbinding te maken met FIM | Schrijf hier de naam van het gebruikers account:                                                                   |
| **Wachtwoord**           | Hiermee geeft u het wacht woord van het gebruikers account dat is opgegeven in de **gebruiker**.                                                                                                                                                                                                                                                                                                                             | Schrijf hier het wacht woord: **belang rijk:** zorg ervoor dat u dit wacht woord op een verborgen, veilige locatie blijft. |
| **IP/machine adres** | Hiermee geeft u het IP-adres op van de BHOLD core-website server. Gebruik niet de server naam.                                                                                                                                                                                                                                                                                                        | Schrijf hier het IP-adres:                                                                          |
| **Poortnummer**        | Hiermee geeft u het poort nummer van de BHOLD-basis website op.                                                                                                                                                                                                                                                                                                                                          | Schrijf hier het poort nummer:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Setup van BHOLD FIM-integratie

Als u de BHOLD FIM-integratie module wilt installeren, meldt u zich aan als lid van de groep domein Administrators, downloadt u het volgende bestand en voert u het uit als beheerder op de server waarop u de BHOLD FIM-integratie module wilt installeren:

- Release. msi van BholdFIMIntegration\_<em>\<-versie\></em>

Vervang * \<versie\> * door het versie nummer van de BHOLD FIM-integratie versie die u installeert.

Als u het programma bestand als beheerder wilt uitvoeren, klikt u met de rechter muisknop op het bestand en klikt u vervolgens op **als administrator uitvoeren**.

![MSI uitvoeren](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Taken na de installatie

Nadat u de BHOLD FIM-integratie hebt geïnstalleerd, moet u micro soft share point configureren om de site-eigenaar machtigingen voor het BHOLD-service account te geven. Als de FIM-Portal is geconfigureerd voor het gebruik van Secure Sockets Layer (SSL)-beveiliging, moet u ook bestanden wijzigen die verwijzingen bevatten naar de adressen van BHOLD-pagina's die zijn toegevoegd aan de FIM-Portal.

### <a name="configuring-sharepoint"></a>Share point configureren

Voor een juiste werking moet de BHOLD-service account voor BHOLD FIM-integratie machtigingen voor de site leden hebben in micro soft share point.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Machtigingen van de site leden verlenen aan het BHOLD-service account

1.  Meld u aan bij de server met BHOLD FIM-integratie met Administrator bevoegdheden.

2.  Klik op **Start**en klik vervolgens op **Internet-Exporer**.

3.  Typ <https://localhost> in de adres balk of share point is geconfigureerd voor het gebruik van SSL-beveiliging <http://localhost>, anders typt u.

4.  Klik aan de linkerkant van de pagina **team site** op **personen en groepen**.

5.  Klik onder **groepen** op **team site leden**en klik in de werk balk van het middelste deel venster op **Nieuw**en klik vervolgens op **gebruikers toevoegen**.

6.  Op de pagina **gebruikers toevoegen: team site** , typt u BHOLDApplicationGroup in **gebruikers/groepen**en klikt u op de knop Namen controleren onder het vak **gebruikers/groepen** . De groeps naam moet worden omgezet om de domein naam op te neemt.

7.  Klik op **gebruikers machtigingen rechtstreeks toekennen**, selecteer **volledig beheer – heeft volledig beheer**en klik vervolgens op **OK**.

8.  Controleer of BHOLDApplicationGroup wordt vermeld in **machtigingen: team site**, en sluit Internet Explorer.

![MSI uitvoeren](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>BHOLD configureren voor ondersteuning van SSL

Als de FIM-Portal is geconfigureerd voor het gebruik van SSL-beveiliging, moet u bestanden op de FIM-server wijzigen zodat koppelingen naar BHOLD-pagina's worden geopend. De bestanden bevinden zich in de volgende map ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```:.

De volgende tabel bevat de bestanden en de oorspronkelijke en gewijzigde versies van de teken reeksen die moeten worden bewerkt.

| **Bestand**                  | **Oorspronkelijke teken reeks**                                                                                                                   | **Gewijzigde teken reeks**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics. aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns. aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx? hideMenu = 1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx? hideMenu = 1 | 
| AttestationMain. aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx? hideMenu = 1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx? hideMenu = 1 |
| Reporting. aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Selfservice. aspx          | RoleExchangePoint = http://\<*FIM_Server*\>: \< *FIM_Port*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint. SVC, TransportMode = Trans Port | RoleExchangePoint = https://\<*FIM_Server_FQDN*\>: \< *FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint. SVC, TransportMode = Trans Port |

Waar:

-   BHOLD_Server geeft de naam aan van de BHOLD-server, zoals deze in de oorspronkelijke versie van het bestand is gevonden * \<\> *

-   MIM_Server geeft de naam van de FIM-server op, zoals gevonden in de oorspronkelijke versie van het bestand * \<\> *

-   BHOLD_Server_FQDN Hiermee wordt de Fully Qualified Domain Name (FQDN) van de BHOLD-server opgegeven * \<\> *

-   MIM_Port geeft het poort nummer van de FIM-server op, zoals gevonden in de oorspronkelijke versie van het bestand * \<\> *

-   MIM_Server_FQDN Hiermee wordt de FQDN-naam van de FIM-server opgegeven * \<\> *

-   MIM_SSL_Port geeft een andere poort op voor gebruik met SSL op de FIM-server * \<\> *

### <a name="enable-approval-workflows-in-bhold-core"></a>Goedkeurings werk stromen inschakelen in BHOLD core

Wanneer FIM en BHOLD zijn geïntegreerd voor Self-service, worden de werk stromen voor goed keuringen uitgevoerd in de FIM-service. Dit is vergelijkbaar met het werk stroom model voor aanvragen die afkomstig zijn van de FIM-Portal, zoals wanneer een gebruiker een aanvraag indient om deel te nemen aan een distributie lijst. Er zijn belang rijke verschillen tussen werk stromen van BHOLD en andere workflows die worden gehost in de FIM-service. In het geval van BHOLD, werk stroom parameters die opgeven welke gebruikers een roltoewijzing moeten goed keuren, zijn afkomstig uit BHOLD, in plaats van te worden opgeslagen in de werk stroom definities in de FIM-service database. Deze para meters worden door BHOLD aan de FIM-service door gegeven wanneer de eerste aanvraag wordt gedaan, en een actie werk stroom communiceert de resultaten terug naar BHOLD core.

BHOLD selecteert een goed keurder voor een self-service aanvraag op een van de volgende drie manieren:

-   **Lijn beheerder als fiatteur: op rollen gebaseerde selectie voor een organisatie-eenheid (OrgUnit)** Als een rol een kenmerk heeft met de naam roletype dat is ingesteld op goed keurder of roltrap, en als die rol is gekoppeld aan een of meer gebruikers in de context van een OrgUnit, moeten aanvragen van gebruikers binnen die OrgUnit worden goedgekeurd door een van de gebruikers die aan de rol zijn gekoppeld met de fiatteur of roltrap roletype.

-   **Lijn beheerder als fiatteur: selectie op basis van kenmerken voor een OrgUnit** Elk OrgUnit kan een of meer kenmerken hebben die de aliassen opgeven van gebruikers die roltoewijzingen kunnen goed keuren voor andere gebruikers in de OrgUnit. Deze kenmerken hebben de naam approver1, approver2, enzovoort. Wanneer een gebruiker in de OrgUnit een roltoewijzing aanvraagt, stuurt BHOLD de aanvraag (via FIM) door naar de gebruikers die zijn opgegeven door de kenmerken van de OrgUnit-fiatteur. Als een OrgUnit geen van deze kenmerken sets bevat, controleert BHOLD de bovenliggende OrgUnits tot de root OrgUnit.

-   **Role Manager als fiatteur: selectie op basis van kenmerken voor een rol** Een rol kan een of meer kenmerken hebben (ook wel approver1 genoemd, enzovoort) die de aliassen opgeven van gebruikers die de toewijzing van de rol kunnen goed keuren. Wanneer een gebruiker aan wie een rol is toegewezen waarvoor de kenmerken van deze goed keurder is ingesteld, stuurt BHOLD de aanvraag door naar de gebruikers die zijn opgegeven door de kenmerken.

Als een goed keurder voor een Self-Service Role-aanvraag niet wordt opgegeven door een van deze methoden, wijst BHOLD standaard automatisch de rol toe zonder goed keuring. Daarom moet u onmiddellijk na de installatie van BHOLD FIM-integratie de basis-OrgUnit met de alias van een goed keurder configureren, zoals het hoofd account. Hiermee wordt voor komen dat een gebruiker onbedoeld een rol krijgt voordat een uitgebreid goedkeurings beleid kan worden geïmplementeerd.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Een goed keurder configureren voor de hoofdmap OrgUnit

1.  Meld u aan bij de BHOLD core-server als beheerder.

2.  Klik op **Start**en vervolgens op **Internet Explorer**.

3.  Typ <http://localhost:5151/bhold/core>in de adres balk van Internet Explorer en druk op de Enter-toets.

4.  Klik op de start pagina van de BHOLD-kern onder **kenmerk def**op **kenmerk typen**.

5.  Klik op de pagina **type kenmerk** op **toevoegen**.

6.  Op de **pagina kenmerk type toevoegen**, in **identiteit**, typt u Approver1 in de lijst **gegevens type** , klikt u op **alfanumeriek**in **maximum lengte**, typt u 255 in de **lijst met waarden**en klikt u op **Nee**, in het **Engels**, typt u goed keurder 1, klikt u op **OK**en klikt u vervolgens op **gereed**.

7.  Klik in het linkerdeel venster, onder **kenmerk def** , op **kenmerk type sets**.

8.  Klik op de pagina **kenmerk type sets** op **toevoegen**.

9.  Op de pagina **kenmerk type set toevoegen** typt u OrgUnit Attributes in **Description**en klikt u vervolgens op **OK**.

10. Vouw op de pagina **OrgUnit-kenmerken** de optie **kenmerk typen**uit en klik vervolgens op **wijzigen**.

11. Klik in de lijst **kenmerk type** op **approver1**, klik op **toevoegen**en klik vervolgens op **gereed**.

12. Klik in het linkerdeel venster op **object typen**.

13. Klik op de pagina **object typen** op **OrgUnit**.

14. Vouw op de pagina **object type-OrgUnit** de optie **kenmerk type sets**uit en klik vervolgens op **wijzigen**.

15. Op de pagina **kenmerk type set/OrgUnit van koppeling** typt u **10 in de**lijst **kenmerk type set** , klikt u op **OrgUnit-kenmerken**, klikt u op **toevoegen**en klikt u vervolgens op **gereed**.

16. Klik in het linkerdeel venster onder **model**op **organisatie-eenheden**.

17. Klik op de pagina **organisatie-eenheden** op **basis**.

18. Klik op de pagina **organisatie-eenheid/root** op **wijzigen**.

19. Op de pagina **kenmerken van organisatie-eenheid wijzigen/hoofd niveau** , in **goed keurder**, typt u het domein en de gebruikers naam van de gebruiker die aanvragen voor roltoewijzing goedkeurt, in de indeling * \<\>domein**\<\>*\\gebruiker, waarbij * \<domein\> * de NetBIOS (Short) domein naam is en * \<de gebruiker\> * de aanmeldings naam van de gebruiker is.
20. Klik op **OK**.

> [!IMPORTANT]
> Het domein en de gebruikers naam moeten overeenkomen met de standaard alias van een gebruiker in de BHOLD Core-Data Base.

Als alternatief voor het opgeven van een fiatteur voor organisatie-eenheden kunt u een fiatteur opgeven voor voorgestelde rollen in de BHOLD Core-Data Base. Als u dit wilt doen, maakt u het kenmerk approver1, voegt u het toe aan een kenmerk typeset die is gekoppeld aan het Role-object type en wijzigt u vervolgens elke voorgestelde rol om de goed keurder op te geven.

Ter verbetering van de beveiliging van werk stromen, naast goed keurders, moet u aanvullende goedkeurings modi en gebruikers opgeven door de volgende kenmerken voor OrgUnits en rollen te maken en in te vullen:

- roltrap<em>\<n\></em>

- eigenaar<em>\<n\></em>

- securityOfficer<em>\<n\></em>

- melding<em>\<n\></em>

waarbij * \<n\> * een optioneel numeriek achtervoegsel aangeeft om meerdere kenmerken van hetzelfde type te bieden.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Goedkeurings werk stromen verifiëren die zijn geconfigureerd in de FIM-service

Bij de installatie van BHOLD FIM-integratie worden sets, werk stroom definities en beheer beleidsregels (beheer beleidsregels) voor de FIM-service gemaakt. Als u uw FIM-implementatie hebt aangepast om de groepen beheerders of de sets van gebruikers die aanvragen kunnen indienen, te wijzigen, moet u ervoor zorgen dat de beheer beleidsregels naar de juiste gebruikers sets verwijzen.

> [!NOTE]
> Voordat gebruikers van de FIM-Portal de selfservice functies van BHOLD kunnen gebruiken, moeten de gebruikers accounts worden gesynchroniseerd in de BHOLD-data base van de FIM-synchronisatie service. In het bijzonder moet er een gebruikers record zijn in de BHOLD Core-Data Base en in de FIM-service database voor elke gebruiker die een self-service aanvraag kan doen of is opgegeven als een fiatteur of roltrap voor Self-service aanvragen.

## <a name="next-steps"></a>Volgende stappen

- Zie [planning en architectuur](https://technet.microsoft.com/library/ee808044.aspx) in de technische bibliotheek van micro soft Forefront voor meer informatie over het installeren van de FIM-Portal en andere FIM-functies.
- [Installatie handleiding voor BHOLD](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
