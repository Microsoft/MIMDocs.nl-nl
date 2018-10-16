---
title: De Microsoft Identity Manager-connector voor Microsoft Graph | Microsoft Docs
author: fimguy
description: Microsoft Identity Manager-connector voor Microsoft Graph kunnen externe gebruiker levenscyclusbeheer voor AD-account. In dit scenario, een organisatie is uitgenodigd voor gasten hun Azure AD-directory en wil deze gasten-toegang geven tot on-premises Windows-Integrated verificatie- of Kerberos-toepassingen
keywords: ''
ms.author: billmath
manager: bhu
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 121b4f3759ef57a9b80d0c1f68c605542e72ae24
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333595"
---
<a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Microsoft Identity Manager-connector voor Microsoft Graph
=======================================================================================

<a name="summary"></a>Samenvatting 
=======

De [Microsoft Identity Manager-connector voor Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) extra integratiescenario's de mogelijkheid voor klanten van Azure AD Premium.  Deze oppervlakken in de MIM sync metaverse extra objecten verkregen uit de [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1- en Bètaservices.

<a name="scenarios-covered"></a>Behandelde scenario 's
=================

<a name="b2b-account-lifecycle-management"></a>Levenscyclusbeheer voor B2B-account
--------------------------------

Het eerste scenario voor de Microsoft Identity Manager-connector voor Microsoft Graph is als een connector voor het automatiseren van AD DS-account levenscyclusbeheer voor externe gebruikers. In dit scenario wordt een organisatie werknemers wordt gesynchroniseerd met Azure AD uit AD DS met behulp van Azure AD Connect en gasten heeft ook worden uitgenodigd in hun Azure AD-directory. Uitnodigen een gast resulteert in een externe gebruiker-object wordt in die organisatie Azure AD-directory, die zich niet in die organisatie AD DS. En vervolgens de organisatie wil deze gasten toegang geven tot on-premises Windows geïntegreerde verificatie- of Kerberos-toepassingen, via de [Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) of andere mechanismen gateway. De Azure AD-toepassingsproxy moet elke gebruiker hebben hun eigen AD DS-account voor identificatie- en overdracht.  

Lees voor meer informatie over het configureren van MIM sync voor het automatisch maken en onderhouden van AD DS-accounts voor gasten, na het lezen van de instructies in dit artikel verder in het artikel [samenwerking van Azure AD-business-to-business (B2B) met MIM 2016 SP1 met Azure Application Proxy](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  Dit artikel ziet u de synchronisatieregels die nodig zijn voor de connector.

<a name="other-identity-management-scenarios"></a>Scenario's voor het beheer van andere identiteit
---------------

De connector kan worden gebruikt voor andere specifieke identiteitsbeheer scenario's met maken, lezen, bijwerken en verwijderen van gebruikers, groepen en neem contact op met objecten in Azure AD, naast de gebruikers en groepen synchroniseren naar Azure AD. Bij het evalueren van mogelijke scenario's, houd er rekening mee: deze connector kan niet worden uitgevoerd in een scenario dat zou leiden tot een gegevensstroom elkaar overlappen, werkelijke of potentiële synchronisatie in strijd zijn met een Azure AD Connect-implementatie.  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) is de aanbevolen aanpak voor het on-premises directory's integreren met Azure AD, door het synchroniseren van gebruikers en groepen uit on-premises directory's met Azure AD.  Azure AD Connect beschikt over veel meer synchronisatiefuncties en scenario's mogelijk zoals wachtwoord- en apparaat terugschrijven van wachtwoorden, die niet mogelijk voor objecten die zijn gemaakt door MIM zijn. Als gegevens worden opgehaald in AD DS, bijvoorbeeld, zorgt u ervoor dat deze wordt uitgesloten van Azure AD Connect probeert om deze objecten terug naar de Azure AD-directory te koppelen.  En kan deze connector worden gebruikt om wijzigingen aanbrengen in Azure AD-objecten die zijn gemaakt door Azure AD Connect.



<a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Voorbereidingen voor het gebruik van de Connector voor Microsoft Graph
=============================================================

<a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>De connector ophalen of beheren van objecten in uw Azure AD-directory autoriseren
----------------------------------------------------

1.  De connector vereist is voor een Web-app / API-toepassing moet worden gemaakt in Azure AD, zodat deze gemachtigd kunnen zijn met de juiste machtigingen om te worden uitgevoerd op Azure AD via Microsoft Graph-objecten.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Afbeelding 1. Nieuwe toepassing registreren

2.  In de Azure-portal, opent u de gemaakte toepassing en de toepassings-ID opslaan als een Client-ID te gebruiken later op de pagina-connectiviteit van de MA:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Afbeelding 2. Toepassings-id

3.  Nieuwe Clientgeheim genereren door het openen van alle instellingen-\> sleutels. De beschrijving van sommige sleutel instellen en selecteer needful duur. Sla de wijzigingen. Een geheime waarde wordt na het verlaten van de pagina niet meer beschikbaar.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Afbeelding 3. Nieuwe Clientgeheim

4.  'Microsoft Graph-API' toevoegen aan de toepassing door te openen 'Vereist machtigingen'.

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Afbeelding 4. Nieuwe API toevoegen

De volgende machtiging moet worden toegevoegd aan de toepassing toe te staan dat de "Microsoft Graph API gebruikt", afhankelijk van het scenario:

| Bewerking met object | Machtiging is vereist                                                                  | Machtigingstype |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Groep importeren          | `Group.Read.All` of `Group.ReadWrite.All`                                                | Toepassing     |
| Gebruikers importeren           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` of `Directory.ReadWrite.All` | Toepassing     |

Meer informatie over vereiste machtigingen kunnen worden gevonden [hier](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. De vereiste machtigingen verlenen aan de toepassing.


<a name="installing-the-connector"></a>De connector installeren
========================

6.  Voordat u de Connector installeert, zorg er dan voor dat u hebt het volgende op de server voor de synchronisatie: 

 - Microsoft.NET 4.5.2 of hoger
 - Microsoft Identity Manager 2016 SP1 en moet gebruiken hotfix 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) of hoger.

7. De connector voor Microsoft Graph, naast andere connectors voor Microsoft Identity Manager 2016 SP1 is beschikbaar als een download van de [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  MIM-synchronisatieservice opnieuw.
 
<a name="connector-configuration"></a>Connector-configuratie
=======================


9.  Selecteer in de UI Synchronization Service Manager **Connectors** en **maken**.
Selecteer **Graph (Microsoft)** , een connector maken en wijs hieraan een beschrijvende naam.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. In de MIM-synchronisatieservice UI, geef de toepassings-ID en Clientgeheim gegenereerd. Elke beheeragent die is geconfigureerd in de MIM-synchronisatie moet een eigen toepassing in Azure AD om te voorkomen dat het uitvoeren van import parallel voor dezelfde toepassing hebben.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Afbeelding 5. Pagina-connectiviteit

De pagina-connectiviteit (afbeelding 5) bevat de Graph API-versie die wordt gebruikt en de naam van tenant. De Client-ID en Clientgeheim vertegenwoordigen de toepassings-ID en sleutel-waarde van de WebAPI-toepassing die moet worden gemaakt in Azure AD.

11. Breng eventueel benodigde wijzigingen op de pagina Algemene Parameters:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Afbeelding 6. Pagina met algemene Parameters

Globale parameters pagina bevat de volgende instellingen:

- Datum-/ tijdindeling-indeling die wordt gebruikt voor een kenmerk met Edm.DateTimeOffset type. Alle datums worden geconverteerd naar een tekenreeks met behulp van deze indeling tijdens het importeren. Indeling van de set wordt toegepast voor elk kenmerk, die datum wordt opgeslagen.

 - HTTP-time-out (seconden): time-out in seconden dat wordt gebruikt tijdens elke HTTP-aanroep van WebAPI-toepassing.

 - Force wachtwoord voor de gemaakte gebruiker bij volgende aanmelding wijzigen: deze optie wordt gebruikt voor de nieuwe gebruiker die wordt gemaakt tijdens het exporteren. Als de optie is ingeschakeld, klikt u vervolgens [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) eigenschap wordt ingesteld op true, anders worden de waarde false.

<a name="configuring-the-connector-schema-and-operations"></a>Configureren van de connectorschema en bewerkingen
=========================

12.   Configureer het schema.  De connector ondersteunt de volgende objecttypen:

-   Gebruiker

    -   Volledige/Delta-Import

    -   Exporteren (toevoegen, bijwerken en verwijderen)

-   Groep

    -   Volledige/Delta-Import

    -   Exporteren (toevoegen, bijwerken en verwijderen)


De lijst met kenmerktypen die worden ondersteund:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (string in het connectorgebied)

-   `microsoft.graph.directoryObject` (verwijzen naar in connectorgebied met een van de ondersteunde objecten)

-   `microsoft.graph.contact`

Kenmerken met meerdere waarden (verzameling) worden ook ondersteund voor het gebruik van een type in de bovenstaande lijst.

De connector gebruikt de '`id`' kenmerk voor het anker en DN-naam voor alle objecten.  Naam wijzigen is daarom niet nodig, omdat een object te wijzigen van de id-kenmerk niet is toegestaan met Graph API.


<a name="access-token-lifetime"></a>Toegang tot de levensduur van token
=====================

Een Graph-toepassing vereist een toegangstoken voor toegang tot de Graph API. Een connector wordt gevraagd een nieuw toegangstoken voor elke herhaling importeren (import iteratie is afhankelijk van het formaat van pagina). Bijvoorbeeld:

-   Azure AD bevat 10000-objecten

-   Het formaat van pagina geconfigureerd in de connector is 5000

In dit geval zullen er twee iteraties tijdens het importeren, wordt elk van deze 5000 objecten retourneren om te synchroniseren. Aanvraag wordt dus twee keer door een nieuw toegangstoken worden.

Tijdens het exporteren wordt een nieuw toegangstoken worden aangevraagd voor elk object dat toegevoegd/bijgewerkt/verwijderd worden moet.

<a name="troubleshooting"></a>Probleemoplossing
===============

**Logboeken inschakelen**

Als er problemen in de grafiek zijn, kunnen logboeken worden gebruikt om het probleem te lokaliseren. Dus, traceringen kunnen worden ingeschakeld [dezelfde manier zoals voor algemene connectors](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Of door alleen toe te voegen van de volgende `miiserver.exe.config` (binnen `system.diagnostics/sources` sectie):


\<bronnaam = "ConnectorsLog" switchValue = "Uitgebreide"\>

\<listeners\>

>   \<initializeData toevoegen "ConnectorsLog" type="System.Diagnostics.EventLogTraceListener, systeem, versie = 4.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 =" name = "ConnectorsLogListener" traceOutputOptions = "LogicalOperationStack,   Datum/tijd, Timestamp, aanroepstack"/\>

\<Verwijder naam = 'Standaard' /\>

\</listeners\>

\</ Source\>

>[!NOTE]
>Als 'Deze beheeragent uitvoeren in een afzonderlijk proces' is ingeschakeld, klikt u vervolgens `dllhost.exe.config` moet worden gebruikt in plaats van `miiserver.exe.config`.

**Toegangsfout token verlopen**

Connector mogelijk HTTP-fout 401 niet-geautoriseerde, bericht 'het toegangstoken is verlopen.' retourneren:

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Afbeelding 7. 'Het toegangstoken is verlopen'. Fout

De oorzaak van dit probleem mogelijk de configuratie van de levensduur van vernieuwingstoken toegang in Azure. Standaard is het toegangstoken is verlopen na 1 uur. Als u wilt de verlooptijd van verhogen, Zie [in dit artikel](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Voorbeeld van dit met [Azure AD PowerShell-Module openbare Preview-versie](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

Nieuwe AzureADPolicy-definitie \@('{"TokenLifetimePolicy": {'Versie': 1, **"AccessTokenLifetime": "5: 00:00"**}}') - DisplayName "OrganizationDefaultPolicyScenario" - IsOrganizationDefault \$true - Type "TokenLifetimePolicy"

<a name="next-steps"></a>Volgende stappen
----------
- [Graph Explorer, ideaal voor het oplossen van HTTP-aanroepen problemen]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versiebeheer, ondersteuning en een belangrijke wijzigingsbeleid voor Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Microsoft Identity Manager-connector voor Microsoft Graph downloaden](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-guides"></a>Scenario-specifieke handleidingen
----------------------------------
[MIM B2B End-to-implementatie]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
