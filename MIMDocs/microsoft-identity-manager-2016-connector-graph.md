---
title: De Microsoft Identity Manager-management-agent voor Microsoft Graph | Microsoft Docs
author: fimguy
description: Microsoft Graph (preview) is een externe gebruiker levenscyclusbeheer van AD-account. In dit scenario wordt een organisatie gasten in hun Azure AD-directory heeft uitgenodigd en wil deze gasten-toegang geven tot het lokale Windows-verificatie of Kerberos-toepassingen
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: a66d424e8388005855ac8e64623f5a00f89682e9
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/25/2018
---
<a name="the-microsoft-identity-manager-management-agent-for-microsoft-graph-public-preview"></a>De Microsoft Identity Manager-management-agent voor Microsoft Graph (openbare Preview)
=======================================================================================

<a name="summary"></a>Samenvatting 
=======

De [Microsoft Identity Manager-beheeragent voor Microsoft Graph (preview)](http://go.microsoft.com/fwlink/?LinkId=717495) kunt u extra integratiescenario's voor Azure AD Premium-klanten.

[Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) on-premises directory's integreert met Azure AD en zorgt ervoor dat gebruikers hebben een gemeenschappelijke identiteit en consistente verificatie via AD DS, Office 365, Azure en SaaS toepassingen die zijn geïntegreerd met Azure AD door het synchroniseren van gebruikers en groepen van on-premises adreslijsten naar Azure AD.   Deze agent management kan worden geïmplementeerd voor gespecialiseerde identiteits- en beheerbewerkingen toegang buiten de gebruiker en groepssynchronisatie met Azure AD.  Deze agent management geeft weer in de MIM-synchronisatie metaverse aanvullende objecten verkregen van de [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 en beta. 

<a name="scenarios-covered"></a>Scenario's worden behandeld
=================

<a name="b2b-account-lifecycle-management"></a>Levenscyclusbeheer B2B-account
--------------------------------

Het eerste scenario in preview voor de Microsoft Identity Manager-management-agent voor Microsoft Graph (preview) is de levenscyclus van beheer van externe gebruikers AD-account. In dit scenario kan een organisatie gasten in hun Azure AD-directory heeft uitgenodigd en wenst te geven die gasten toegang krijgen tot lokale Windows-verificatie of toepassingen op basis van Kerberos via de [Azure AD-toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)proxy of andere wijze gateway. De Azure AD-toepassingsproxy is vereist voor elke gebruiker hebben hun eigen AD DS-account voor identificatie en overdracht

Aanvullende scenario's in de toekomst kunnen worden toegevoegd en [ze hier zijn beschreven](./microsoft-identity-manager-2016-graph-b2b-scenario.md)

<a name="determining-your-deployment-topology"></a>Uw implementatietopologie bepalen
====================================


<a name="preparing-to-use-the-management-agentma-for-microsoft-graph"></a>Voorbereidingen voor het gebruik van de Management-Agent(MA) voor Microsoft Graph
=============================================================

<a name="authorizing-the-ma-to-manage-your-azure-ad-directory"></a>De Beheeragent voor het beheren van uw Azure AD-directory autoriseren
----------------------------------------------------

1.  Grafiek-beheeragent vereist Web-app / API-toepassing in AzureAD worden gemaakt.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Afbeelding 1. Nieuwe toepassing registreren

2.  De gemaakte toepassing openen en gebruiken van toepassings-ID zoals Client-Id op van de MA connectiviteit pagina:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Afbeelding 2. Toepassings-ID

2.  Nieuw Clientgeheim genereren met het openen van alle instellingen-\> sleutels. Sommige beschrijving sleutel instellen en selecteer needful duur. Sla de wijzigingen. Een geheime waarde wordt na het verlaten van de pagina niet meer beschikbaar.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Afbeelding 3. Nieuw Clientgeheim

3.  'Microsoft Graph API' toevoegen aan de toepassing door het openen van "Vereiste machtigingen."

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Afbeelding 4. Nieuwe API toevoegen

De volgende machtigingen moet worden toegevoegd aan de 'Microsoft Graph API':

| Met een object | Vereiste machtiging                                                                  | Machtigingstype |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Groep importeren          | Group.Read.All of Group.ReadWrite.All                                                | Toepassing     |
| Gebruikers importeren           | User.Read.All of User.ReadWrite.All of Directory.Read.All of Directory.ReadWrite.All | Toepassing     |

Meer informatie over de vereiste machtigingen gevonden [hier](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)

1.  Maken van connector met de toepassings-ID en gegenereerde Client Secret.Each beheeragent moet een eigen toepassing in AzureAD om te voorkomen dat importeren parallel voor dezelfde toepassing hebben. Grafiek-connector ondersteunt de volgende lijst met objecttypen:

-   Gebruiker

    -   Volledige/Delta-Import

    -   Exporteren (toevoegen, bijwerken en verwijderen)

-   Groep

    -   Volledige/Delta-Import

    -   Exporteren (toevoegen, bijwerken en verwijderen)


De lijst met kenmerktypen die worden ondersteund:

-   Edm.Boolean

-   Edm.String

-   Edm.DateTimeOffset (tekenreeks in de connectorruimte)

-   microsoft.graph.directoryObject (verwijzing in de connectorruimte met een van de ondersteunde objecten)

-   Microsoft.Graph.contact

Kenmerken met meerdere waarden (verzameling) worden ook ondersteund voor een van de vorm van een type eerder in de lijst.

Grafiek-connector gebruikt id-kenmerk voor het anker en DN-naam voor alle objecten.

Wijzig de naam wordt niet ondersteund op dit moment GraphAPI zijn niet toegestaan voor object wijziging in de id-kenmerk voor een bestaand object.

<a name="access-token-lifetime"></a>Levensduur van token toegang
=====================

Een grafiek toepassing vereist een toegangstoken voor toegang tot de GraphAPI. Een connector wordt u gevraagd om een nieuw toegangstoken voor elke herhaling importeren (import herhaling is afhankelijk van de paginagrootte). Bijvoorbeeld:

-   AzureAD bevat 10000 objecten

-   Paginagrootte geconfigureerd in de connector is 5000

In dit geval zullen er twee iteraties tijdens het importeren, elk van deze 5 000 objecten wordt geretourneerd om te synchroniseren. Aanvraag wordt dus tweemaal door een nieuw toegangstoken worden.

Houd er rekening mee dat tijdens het exporteren een nieuw toegangstoken gevraagd voor elk object dat toegevoegd/bijgewerkt/verwijderd worden moet.

<a name="installing-the-connector"></a>De connector installeren
========================

Voordat u de Connector gebruikt, controleert u of hebt u de volgende op de server voor de synchronisatie: Microsoft.NET 4.5.2 of hoger Microsoft Identity Manager 2016 SP1 moet gebruiken hotfix 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) of hoger.

Connectors voor Microsoft Identity Manager 2016 SP1, de grafiek-Connector is gedownload van de [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

<a name="connector-configuration"></a>Connector-configuratie
=======================

Connectiviteit pagina:

![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Afbeelding 5. Pagina Verbindingsmogelijkheden

De pagina voor verbindingsmogelijkheden (afbeelding 1) bevat de Graph API-versie die wordt gebruikt en de naam van de tenant. De Client-Id en Clientgeheim vertegenwoordigen de toepassings-ID en de waarde van de sleutel van de WebAPI-toepassing die moet worden gemaakt in AzureAD.

Pagina Algemene Parameters:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Afbeelding 6. Pagina met globale Parameters

Globale parameters pagina bevat de volgende instellingen:

Datum-/ tijdindeling-indeling die wordt gebruikt voor kenmerken met Edm.DateTimeOffset type. Alle datums worden geconverteerd naar een tekenreeks met behulp van deze indeling tijdens het importeren. Set-indeling is toegepast voor elk kenmerk waarin datum wordt opgeslagen.

HTTP-time-out (seconden): time-out in seconden die wordt gebruikt tijdens elke aanroep van HTTP-WebAPI-toepassing.

Force wachtwoord voor gebruiker bij volgende aanmelding wijzigen: deze optie wordt gebruikt voor de nieuwe gebruiker die wordt gemaakt tijdens het exporteren. Als de optie is ingeschakeld, klikt u vervolgens [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) eigenschap wordt ingesteld op true, anders moeten false.

<a name="troubleshooting"></a>Probleemoplossing
===============

**Logboeken inschakelen**

Als er problemen in grafiek zijn, kunnen logboeken worden gebruikt voor het probleem te lokaliseren. De grafiek-connector maakt gebruik van dezelfde bron als alle generieke connectors in. Dus traceringen kunnen worden ingeschakeld [dezelfde manier zoals voor algemene connectors Zie het wiki](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Of gewoon door het volgende toe te voegen aan miiserver.exe.config (in de sectie system.diagnostics/sources):

\<de bronnaam = 'ConnectorsLog' switchValue = 'Uitgebreid'\>

\<listeners\>

>   \<initializeData toevoegen 'ConnectorsLog' type="System.Diagnostics.EventLogTraceListener, systeem, versie = 4.0.0.0-prestatiemeters, Culture = neutral, PublicKeyToken = b77a5c561934e089 = ' name = 'ConnectorsLogListener' traceOutputOptions ="LogicalOperationStack,   Aanroepstack DateTime, Timestamp,' /\>

\<Verwijder naam = 'Default' /\>

\</listeners\>

\</ Source\>

Opmerking: als 'Deze agent management uitvoeren in een afzonderlijk proces' ingeschakeld moet dllhost.exe.config moet worden gebruikt in plaats van miiserver.exe.config.

**Toegangsfout van het token verlopen**

Connector mogelijk HTTP-fout 401 onbevoegde, bericht "toegangstoken is verlopen." retourneren:

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Afbeelding 7. "Toegangstoken is verlopen." Fout

De oorzaak van dit probleem mogelijk de configuratie van de levensduur van token toegang vanaf de Azure. Standaard wordt het toegangstoken verloopt na 1 uur. Raadpleeg voor een verhoging van de verlooptijd van [in dit artikel](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Voorbeeld van deze [Azure AD PowerShell-Module Public Preview-versie](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

Nieuwe AzureADPolicy-definitie \@('{"TokenLifetimePolicy": {"-versie: 1, **'AccessTokenLifetime': ' 5: 00:00 '**}}') - weergavenaam 'OrganizationDefaultPolicyScenario' - IsOrganizationDefault \$true - Type 'TokenLifetimePolicy'

<a name="next-steps"></a>Volgende stappen
----------
- [Grafiek Explorer (handig voor het oplossen van problemen voor HTTP-aanroep)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versiebeheer, ondersteuning en recente wijzigen beleidsregels voor Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Microsoft Identity Manager-management-agent voor Microsoft Graph (preview) downloaden](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-supported-guides"></a>Scenario-specifieke ondersteund handleidingen
----------------------------------
[MIM B2B-End-to-End-implementatie]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
