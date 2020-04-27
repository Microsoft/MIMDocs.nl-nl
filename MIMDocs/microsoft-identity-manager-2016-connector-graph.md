---
title: De Microsoft Identity Manager-connector voor Microsoft Graph | Microsoft Docs
author: fimguy
description: Met Microsoft Identity Manager connector voor Microsoft Graph kunt u beheer van de externe gebruiker AD-account levenscyclus. In dit scenario heeft een organisatie uitgenodigd voor gasten in hun Azure AD-adres lijst en hen hen toegang te geven tot on-premises Windows-geïntegreerde verificatie of op Kerberos gebaseerde toepassingen
keywords: ''
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 462b649ca02519e5af5c3b1243506a74efa7052a
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044256"
---
# <a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Microsoft Identity Manager connector voor Microsoft Graph


## <a name="summary"></a>Samenvatting 


De [Microsoft Identity Manager connector voor Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) biedt extra integratie scenario's voor Azure AD Premium klanten.  Het Opper vlak van de MIM-Sync-mailverse extra objecten die zijn verkregen van de [Microsoft Graph-API](https://developer.microsoft.com/en-us/graph/) v1 en Beta.

## <a name="scenarios-covered"></a>Scenario's die worden behandeld


### <a name="b2b-account-lifecycle-management"></a>Beheer van levens duur B2B-account


Het eerste scenario voor de Microsoft Identity Manager connector voor Microsoft Graph is een connector waarmee u het beheer van de levens duur van AD DS-account voor externe gebruikers kunt automatiseren. In dit scenario wordt de werk nemers met behulp AD DS van Azure AD Connect door een organisatie gesynchroniseerd met Azure AD. ook zijn gasten uitgenodigd voor hun Azure AD-adres lijst. Als u een gast uitnodigt in een extern gebruikers object in de Azure AD-adres lijst van die organisatie, die zich niet in de AD DS van die organisatie bevindt. Vervolgens wil de organisatie deze gasten toegang geven tot on-premises geïntegreerde Windows-verificatie of op Kerberos gebaseerde toepassingen via de [Azure AD-toepassings proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) of andere gateway mechanismen. De Azure AD-toepassings proxy vereist dat elke gebruiker hun eigen AD DS account heeft, voor identificatie-en delegering.  

Als u wilt weten hoe u de MIM-synchronisatie kunt configureren om automatisch AD DS accounts voor gasten te maken en te onderhouden, gaat u na het lezen van de instructies in dit artikel verder met het lezen van [Azure AD Business-to-Business (B2B)-samen werking met MIM 2016 SP1 met Azure-toepassing proxy](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  In dit artikel ziet u de synchronisatie regels die nodig zijn voor de connector.

### <a name="other-identity-management-scenarios"></a>Andere scenario's voor identiteits beheer


De connector kan worden gebruikt voor andere specifieke scenario's voor identiteits beheer met betrekking tot het maken, lezen, bijwerken en verwijderen van gebruikers-, groeps-en contact objecten in azure AD, buiten gebruikers-en groeps synchronisatie met Azure AD. Houd rekening met het volgende wanneer u mogelijke scenario's evalueert: deze connector kan niet worden gebruikt in een scenario, wat zou leiden tot een overlap ping van een gegevens stroom, een werkelijke of mogelijke synchronisatie conflict met een Azure AD Connect-implementatie.  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) is de aanbevolen aanpak voor het integreren van on-premises directory's met Azure AD, door gebruikers en groepen van on-premises directory's te synchroniseren met Azure AD.  Azure AD Connect heeft veel meer synchronisatie functies en maakt scenario's mogelijk, zoals het terugschrijven van wacht woorden en apparaten, die niet mogelijk zijn voor objecten die zijn gemaakt door MIM. Als de gegevens in AD DS worden gebracht, moet u er bijvoorbeeld voor zorgen dat deze worden uitgesloten van Azure AD Connect een poging om deze objecten te koppelen aan de Azure AD-adres lijst.  Deze connector kan ook worden gebruikt om wijzigingen aan te brengen in azure AD-objecten, die zijn gemaakt door Azure AD Connect.



## <a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Voorbereiden op het gebruik van de connector voor Microsoft Graph

### <a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>De connector machtigen om objecten op te halen of te beheren in uw Azure AD-Directory


1.  De connector vereist dat een web app/API-toepassing wordt gemaakt in azure AD, zodat deze kan worden geautoriseerd met de juiste machtigingen voor het uitvoeren van Azure AD-objecten via Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Afbeelding 1. Nieuwe toepassing registreren

2.  Open in de Azure Portal de gemaakte toepassing en sla de toepassings-ID op als een client-ID die u later op de pagina connectiviteit van de MA kunt gebruiken:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Afbeelding 2. Toepassings-id

3.  Genereer een nieuw client geheim door alle instellingen\> -sleutels te openen. Stel een bepaalde sleutel beschrijving in en selecteer duur vereist. Sla de wijzigingen op. Een geheime waarde is niet beschikbaar na het verlaten van de pagina.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Afbeelding 3. Nieuw client geheim

4.  Voeg ' Microsoft Graph API ' toe aan de toepassing door de vereiste machtigingen te openen.

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Afbeelding 4. Nieuwe API toevoegen

De volgende machtiging moet worden toegevoegd aan de toepassing zodat deze de Microsoft Graph-API kan gebruiken, afhankelijk van het scenario:

| Bewerking met object | Machtiging vereist                                                                  | Machtigings type |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Groep importeren          | `Group.Read.All` of `Group.ReadWrite.All`                                                | Toepassing     |
| Gebruiker importeren           | `User.Read.All`, `User.ReadWrite.All` `Directory.Read.All` of`Directory.ReadWrite.All` | Toepassing     |

Meer informatie over de vereiste machtigingen vindt u [hier](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. Ken de vereiste machtigingen toe aan de toepassing.


## <a name="installing-the-connector"></a>De connector installeren


6.  Voordat u de connector installeert, moet u controleren of u over het volgende beschikt op de synchronisatie server: 

 - Microsoft .NET 4.5.2-Framework of hoger
 - Microsoft Identity Manager 2016 SP1 en moet hotfix 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) of hoger worden gebruikt.

7. De connector voor Microsoft Graph, naast andere connectors voor Microsoft Identity Manager 2016 SP1, is beschikbaar als down load vanuit het [micro soft Download centrum](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Start de MIM-synchronisatie service opnieuw.
 
## <a name="connector-configuration"></a>Connector configuratie



9.  Selecteer in de Synchronization Service Manager-gebruikers interface de optie **connectors** en **maken**.
Selecteer **Graph (micro soft)** , maak een connector en geef deze een beschrijvende naam.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. Geef de toepassings-ID en het gegenereerde client geheim op in de gebruikers interface van de MIM-synchronisatie service. Elke beheer agent die in MIM-synchronisatie is geconfigureerd, moet een eigen toepassing hebben in azure AD om te voor komen dat import parallel wordt uitgevoerd voor dezelfde toepassing.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Afbeelding 5. Pagina connectiviteit

De pagina connectiviteit (afbeelding 5) bevat de Graph API versie die wordt gebruikt en de naam van de Tenant. De client-ID en het client geheim vertegenwoordigen de toepassings-ID en de sleutel waarde van de WebAPI-toepassing die moeten worden gemaakt in azure AD.

11. Breng de benodigde wijzigingen aan op de pagina globale para meters:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Afbeelding 6. Pagina algemene para meters

De pagina globale para meters bevat de volgende instellingen:

- Datum notatie: notatie die wordt gebruikt voor elk kenmerk met het type EDM. date time offset. Alle datums worden geconverteerd naar een teken reeks door deze indeling te gebruiken tijdens het importeren. De ingestelde indeling wordt toegepast voor elk kenmerk dat de datum opslaat.

 - HTTP-time-out (seconden): time-out in seconden die wordt gebruikt tijdens elke HTTP-aanroep naar WebAPI-toepassing.

 - Wacht woord voor het wijzigen van de gebruiker bij de volgende aanmelding afdwingen: deze optie wordt gebruikt voor een nieuwe gebruiker die tijdens het exporteren wordt gemaakt. Als de optie is ingeschakeld, wordt de eigenschap [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) ingesteld op True, anders is deze false.

## <a name="configuring-the-connector-schema-and-operations"></a>Het schema en de bewerkingen van de connector configureren


12.   Configureer het schema.  De connector ondersteunt de volgende lijst met object typen:

-   Gebruiker

    -   Volledige/Delta-import

    -   Exporteren (toevoegen, bijwerken, verwijderen)

-   Groep

    -   Volledige/Delta-import

    -   Exporteren (toevoegen, bijwerken, verwijderen)


De lijst met kenmerk typen die worden ondersteund:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset`(teken reeks in connector ruimte)

-   `microsoft.graph.directoryObject`(verwijzing in een connector ruimte naar een van de ondersteunde objecten)

-   `microsoft.graph.contact`

Kenmerken met meerdere waarden (verzameling) worden ook ondersteund voor een van de typen in de bovenstaande lijst.

De connector gebruikt het kenmerk`id`' ' voor het anker en de DN voor alle objecten.  De naam kan daarom niet worden gewijzigd, omdat in Graph API niet is toegestaan dat een object het id-kenmerk wijzigt.


## <a name="access-token-lifetime"></a>Levens duur van toegangs token


Een grafiek toepassing vereist een toegangs token voor toegang tot de Graph API. Een connector vraagt een nieuw toegangs token voor elke import herhaling (import herhaling is afhankelijk van de pagina grootte). Bijvoorbeeld:

-   Azure AD bevat 10000 objecten

-   De pagina grootte die is geconfigureerd in de connector is 5000

In dit geval zijn er tijdens het importeren twee herhalingen, die allemaal 5000 objecten retour neren om te synchroniseren. Een nieuw toegangs token wordt dus twee keer aangevraagd.

Tijdens het exporteren wordt een nieuw toegangs token aangevraagd voor elk object dat moet worden toegevoegd/bijgewerkt/verwijderd.

## <a name="troubleshooting"></a>Problemen oplossen


**Logboeken inschakelen**

Als er problemen zijn in Graph, kunnen de logboeken worden gebruikt om het probleem te lokaliseren. Traceringen kunnen dus op [dezelfde manier worden ingeschakeld als voor algemene connectors](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Of alleen door het volgende toe te `miiserver.exe.config` voegen `system.diagnostics/sources` aan (in de sectie):

```
\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

\<add initializeData="ConnectorsLog"
type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,
Culture=neutral, PublicKeyToken=b77a5c561934e089"
name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,
DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>
```
>[!NOTE]
>Als ' deze beheer agent in een afzonderlijk proces uitvoeren ' is ingeschakeld, moet `dllhost.exe.config` u in plaats van `miiserver.exe.config`gebruiken.

**Fout met het toegangs token is verlopen**

Connector kan HTTP-fout 401 niet-geautoriseerd retour neren, bericht toegangs token is verlopen. ":

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Afbeelding 7. Het toegangs token is verlopen. Fout

De oorzaak van dit probleem kan de configuratie van de toegangs token levensduur van de Azure-zijde zijn. Het toegangs token verloopt standaard na 1 uur. Raadpleeg [dit artikel](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes)als u de verloop tijd wilt verhogen.

Voor beeld van het gebruik van de [open bare preview-versie van Azure AD Power shell-module](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy-definition \@({"TokenLifetimePolicy": {"versie": 1, **"AccessTokenLifetime": "5:00:00"**}} ")-DisplayName" OrganizationDefaultPolicyScenario "-IsOrganizationDefault \$True-Type" TokenLifetimePolicy "

## <a name="next-steps"></a>Volgende stappen

- [Graph Explorer, geweldig voor het oplossen van problemen met HTTP-aanroepen]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versie beheer, ondersteuning en het verbreken van beleid voor het wijzigen van Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Microsoft Identity Manager connector voor Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)
[MIM B2B-end-to]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md) -end-implementatie downloaden
