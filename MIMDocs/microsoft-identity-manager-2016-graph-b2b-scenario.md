---
title: De Microsoft Identity Manager connector configureren voor Microsoft Graph voor B2B | Microsoft Docs
author: billmath
description: Microsoft Graph-connector is het beheer van de levens cyclus van externe gebruikers in een AD-account. In dit scenario heeft een organisatie uitgenodigd voor gasten in hun Azure AD-adres lijst en hen hen toegang te geven tot on-premises Windows-geïntegreerde verificatie of op Kerberos gebaseerde toepassingen
keywords: ''
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 0d5f970168934f3fcc4c721aad0a439e2babcfe7
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79381502"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Azure AD Business-to-Business (B2B)-samen werking met Microsoft Identity Manager (MIM) 2016 SP1 met Azure-toepassing proxy
============================================================================================================================

Het eerste scenario is het beheer van de levens cyclus van externe gebruikers in een AD-account.   In dit scenario heeft een organisatie uitgenodigd voor gasten in hun Azure AD-Directory en wil deze gasten toegang krijgen tot on-premises Windows-geïntegreerde verificatie of op Kerberos gebaseerde toepassingen via de [Azure AD-toepassings proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) of andere gateway mechanismen. De Azure AD-toepassings proxy vereist dat elke gebruiker hun eigen AD DS account heeft, voor identificatie-en delegering.

## <a name="scenario-specific-guidance"></a>Scenario-specifieke richt lijnen

Enkele hypo Thesen die zijn gemaakt in de configuratie van B2B met MIM en Azure AD-toepassingsproxy:

-   U hebt al een on-premises AD geïmplementeerd en Microsoft Identity Manager is geïnstalleerd en de basis configuratie van de MIM-service, de MIM-Portal, Active Directory beheer agent (AD MA) en FIM-beheer agent (FIM MA).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   U hebt de instructies in het artikel al gevolgd voor het downloaden en installeren van de [Graph-connector](microsoft-identity-manager-2016-connector-graph.md).

-   U hebt Azure AD Connect geconfigureerd voor het synchroniseren van gebruikers en groepen met Azure AD.

-   U hebt al toepassings proxy connectors en connector groepen ingesteld, als dat niet zo is, kunt u [hier](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) naar installeren en configureren

-   U hebt al een of meer toepassingen gepubliceerd, die afhankelijk zijn van geïntegreerde Windows-verificatie of individuele AD-accounts via Azure AD-app proxy

-   U hebt uitgenodigd om een of meer gasten uit te nodigen, wat heeft geresulteerd in een of meer gebruikers die zijn gemaakt in azure AD<https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Voorbeeld scenario voor het implementeren van de B2B-end-to-end

Deze hand leiding is gebaseerd op het volgende scenario:

Contoso farmaceutische werken met Trey Research Inc. als onderdeel van hun R&D-afdeling. Trey Research-mede werkers moeten toegang hebben tot de rapportage toepassing voor onderzoek van Contoso farmaceutische bedrijven.

-   Contoso farmaceutische bedrijven bevinden zich in hun eigen Tenant, om een aangepast domein te configureren.

-   Iemand heeft een externe gebruiker uitgenodigd voor de contoso farmaceutische Tenant.
    Deze gebruiker heeft de uitnodiging geaccepteerd en heeft toegang tot gedeelde resources.

-   Contoso Farmaceutica heeft een toepassing gepubliceerd via een app-proxy. In dit scenario is de voorbeeld toepassing de MIM-Portal. Zo zou een gast gebruiker kunnen deel nemen aan MIM-processen, bijvoorbeeld in Help Desk-scenario's of om toegang aan te vragen voor groepen in MIM.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>AD en Azure AD Connect configureren om gebruikers uit te sluiten die zijn toegevoegd vanuit Azure AD

Azure AD Connect gaat er standaard van uit dat niet-beheerders gebruikers in Active Directory moeten worden gesynchroniseerd met Azure AD.  Als Azure AD Connect een bestaande gebruiker in azure AD vindt die overeenkomt met de gebruiker uit on-premises AD, komt Azure AD Connect overeen met de twee accounts en wordt ervan uitgegaan dat dit een eerdere synchronisatie van de gebruiker is, en de on-premises AD-gezaghebbend te maken.  Dit standaard gedrag is echter niet geschikt voor de B2B-stroom, waarbij het gebruikers account afkomstig is uit Azure AD. 

Daarom moeten de gebruikers die door MIM vanuit Azure AD worden AD DS, worden opgeslagen op een manier waarop Azure AD deze gebruikers niet meer probeert te synchroniseren met Azure AD.
Een manier om dit te doen is het maken van een nieuwe organisatie-eenheid in AD DS en het configureren van Azure AD Connect om die organisatie-eenheid uit te sluiten.  

Meer informatie vindt [u in azure AD Connect Sync: filters configureren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>De Azure AD-toepassing maken 


Opmerking: voordat u een MIM-verbinding maakt, moet u ervoor zorgen dat u de hand leiding voor het implementeren van de [Graph](microsoft-identity-manager-2016-connector-graph.md)-connector hebt gelezen en een toepassing met een client-id en geheim hebt gemaakt.
Zorg ervoor dat de toepassing ten minste een van de volgende machtigingen heeft: `User.Read.All`, `User.ReadWrite.All` `Directory.Read.All` of `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>De nieuwe beheer agent maken


Selecteer in de Synchronization Service Manager-gebruikers interface de optie **connectors** en **maken**.
Selecteer **Graph (micro soft)** en geef deze een beschrijvende naam.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Connectiviteit

Op de pagina connectiviteit moet u de Graph API versie opgeven. Gereed voor productie PAI is **V 1,0**, non-Production is **beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Globale parameters

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Inrichtings hiërarchie configureren

Deze pagina wordt gebruikt om het DN-onderdeel, bijvoorbeeld OE, toe te wijzen aan het object type dat moet worden ingericht, bijvoorbeeld organizationalUnit. Dit is niet nodig voor dit scenario. Als u dit niet wilt, moet u de standaard instelling wijzigen en op Volgende klikken.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Partities en hiërarchieën configureren

Selecteer op de pagina partities en hiërarchieën alle naam ruimten met objecten die u wilt importeren en exporteren.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Objecttypes selecteren

Selecteer op de pagina object typen de object typen die u wilt importeren. U moet ten minste ' gebruiker ' selecteren.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Kenmerken selecteren

Selecteer in het scherm kenmerken selecteren de optie kenmerken van Azure AD die nodig zijn voor het beheren van B2B-gebruikers in AD. Het kenmerk ' ID ' is vereist.  De kenmerken `userPrincipalName` en `userType` worden later in deze configuratie gebruikt.  Andere kenmerken zijn optioneel, waaronder

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Ankers configureren

In het scherm voor het configureren van het anker is het configureren van het anker kenmerk een vereiste stap. Gebruik standaard het kenmerk ID voor gebruikers toewijzing.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Connectorfilter configureren

Op de pagina connector filter configureren kunt u met MIM objecten filteren op basis van kenmerk filter. In dit scenario voor B2B is het doel om alleen gebruikers te meenemen met de waarde van het `userType` kenmerk dat gelijk `Guest`is aan en niet aan gebruikers met de User type die gelijk `member`is aan.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Regels voor lid worden en projectie configureren

In deze hand leiding wordt ervan uitgegaan dat u een synchronisatie regel maakt.  Als het configureren van de samen Voeg-en projectie regels worden verwerkt door de synchronisatie regel, hoeft het niet nodig zijn om een koppeling en projectie te identificeren op de connector zelf. Geef de standaard waarde op en klik op OK.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Kenmerkstroom configureren

In deze hand leiding wordt ervan uitgegaan dat u een synchronisatie regel maakt.  Er is geen projectie vereist voor het definiëren van de kenmerk stroom in de MIM-synchronisatie, omdat deze wordt verwerkt door de synchronisatie regel die later wordt gemaakt. Geef de standaard waarde op en klik op OK.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Ongedaan maken van de inrichting configureren

Met de instelling voor het configureren van de inrichting kunt u de MIM-synchronisatie configureren om het object te verwijderen als het omgekeerde object wordt verwijderd. In dit scenario maken we deze afsluitingen omdat het doel is om ze in azure AD te laten staan. In dit scenario exporteren we niets naar Azure AD en de connector is alleen voor importeren geconfigureerd.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Extensies configureren

Het configureren van extensies op deze beheer agent is een optie, maar is niet vereist omdat we een synchronisatie regel gebruiken. Als we hebben besloten een geavanceerde regel te gebruiken in de kenmerk stroom, is er dan een optie om de uitbrei ding van de regels te definiëren.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Het omgekeerde schema uitbreiden


Voordat u de synchronisatie regel maakt, moet u een kenmerk met de naam userPrincipalName maken dat is gekoppeld aan het persoons object met behulp van de MV Designer.

Selecteer in de synchronisatie-client de optie multiverse Designer

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Selecteer vervolgens het object type persoon

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Klik vervolgens onder acties op kenmerk toevoegen

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Voer vervolgens de volgende gegevens uit

Kenmerk naam: **userPrincipalName**

Kenmerk type: **teken reeks (indexeerbaar)**

Geïndexeerd = **waar**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Synchronisatie regels voor MIM-service maken

in de onderstaande stappen beginnen we met het toewijzen van het B2B-gast account en de kenmerk stroom. Hier vindt u enkele hypo Thesen: u hebt de Active Directory MA geconfigureerd en de FIM-MA geconfigureerd om gebruikers naar de MIM-service en-portal te brengen.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Voor de volgende stappen is het toevoegen van een minimale configuratie aan de FIM-MA en de AD-MA vereist.

Meer informatie vindt u hier voor de configuratie <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> : hoe kan ik gebruikers inrichten voor AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Synchronisatie regel: gast gebruiker importeren naar MV naar de synchronisatie service van Azure Active Directory<br>

Ga naar de MIM-Portal, selecteer synchronisatie regels en klik op nieuw.  Maak een regel voor binnenkomende synchronisatie voor de B2B-stroom via de Graph-connector.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

Zorg ervoor dat u in de stap relatie criteria de optie ' resource maken in FIM ' selecteert.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Configureer de volgende regels voor binnenkomende kenmerk stromen.  Zorg ervoor dat u de `accountName`kenmerken `userPrincipalName` , `uid` en hebt ingevuld, zoals ze later in dit scenario worden gebruikt:

| **Alleen eerste stroom** | **Gebruiken als bestaans test** | **Flow (bron waarde ⇒ FIM-kenmerk)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName ⇒ displayName] (Java script: void (0);)                        |
|                       |                           | [Left (id, 20) ⇒ AccountName] (Java script: void (0);)                        |
|                       |                           | [id ⇒ uid] (Java script: void (0);)                                         |
|                       |                           | [User type ⇒ Employee type] (Java script: void (0);)                          |
|                       |                           | [voor gegeven ⇒ OpgegevenNaam] (Java script: void (0);)                            |
|                       |                           | [Achternaam ⇒ sn] (Java script: void (0);)                                     |
|                       |                           | [userPrincipalName ⇒ userPrincipalName] (Java script: void (0);)            |
|                       |                           | [id ⇒ CN] (Java script: void (0);)                                          |
|                       |                           | [e-mail ⇒ mail] (Java script: void (0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone] (Java script: void (0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Synchronisatie regel: gast gebruikers account maken voor Active Directory 

Met deze synchronisatie regel maakt u de gebruiker in Active Directory.  Zorg ervoor dat de stroom `dn` voor de gebruiker in de organisatie-eenheid die is uitgesloten van Azure AD Connect, moet worden geplaatst.  Werk ook de stroom voor `unicodePwd` om te voldoen aan uw AD-wachtwoord beleid. de gebruiker hoeft het wacht woord niet te weten.  Let op de waarde `262656` voor `userAccountControl` voor het coderen van `SMARTCARD_REQUIRED` de `NORMAL_ACCOUNT`vlaggen en.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Stroom regels:

| **Alleen eerste stroom** | **Gebruiken als bestaans test** | **Flow (FIM-waarde ⇒-doel kenmerk)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [AccountName ⇒ sAMAccountName] (Java script: void (0);)                     |
|                       |                           | [voor gegeven ⇒ OpgegevenNaam] (Java script: void (0);)                            |
|                       |                           | [e-mail ⇒ mail] (Java script: void (0);)                                      |
|                       |                           | [sn ⇒ sn] (Java script: void (0);)                                          |
|                       |                           | [userPrincipalName ⇒ userPrincipalName] (Java script: void (0);)            |
| **Y**                 |                           | ["CN =" + UID + ", OE = B2BGuest, DC = contoso, DC = com" ⇒ DN] (Java script: void (0);) |
| **Y**                 |                           | [RandomNum (0999) + userPrincipalName ⇒ unicodePwd] (Java script: void (0);)  |
| **Y**                 |                           | [262656 ⇒ userAccountControl] (Java script: void (0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Optionele synchronisatie regel: Importeer B2B gast User Objects SID om u aan te melden bij MIM 

Met deze regel voor binnenkomende synchronisatie wordt het SID-kenmerk van de gebruiker weer gegeven van Active Directory terug naar MIM, zodat de gebruiker toegang kan krijgen tot de MIM-Portal.  De MIM- `samAccountName` `domain` Portal vereist dat de gebruiker de kenmerken heeft en `objectSid` ingevuld is in de data base van de MIM-service.

Configureer het externe bron systeem als de `ADMA`, omdat het `objectSid` kenmerk automatisch wordt ingesteld door AD wanneer MIM de gebruiker maakt.
 
Houd er rekening mee dat als u gebruikers configureert om te worden gemaakt in de MIM-service, ervoor moet zorgen dat ze niet binnen het bereik vallen van de sets die zijn bedoeld voor SSPR beheer beleidsregels voor werk nemers.  Mogelijk moet u de ingestelde definities wijzigen om gebruikers uit te sluiten die zijn gemaakt door de B2B-stroom. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Alleen eerste stroom** | **Gebruiken als bestaans test** | **Flow (bron waarde ⇒ FIM-kenmerk)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName ⇒ accountnaam] (Java script: void (0);)                     |
|                       |                           | ["CONTOSO" ⇒-domein] (Java script: void (0);)                            |
|                       |                           | [objectSid ⇒ objectSid] (Java script: void (0);)                                      |


## <a name="run-the-synchronization-rules"></a>De synchronisatie regels uitvoeren

Daarna nodigen we de gebruiker uit en voeren ze de beheer agent synchronisatie regels in de volgende volg orde uit:

-   Volledige import en synchronisatie in de `MIMMA` beheer agent.  Dit zorgt ervoor dat de MIM-synchronisatie de meest recente synchronisatie regels heeft geconfigureerd.

-   Volledige import en synchronisatie in de `ADMA` beheer agent.  Dit zorgt ervoor dat MIM en Active Directory consistent zijn.  Op dit moment is er nog geen export voor gasten in behandeling.

-   Volledige import en synchronisatie in de B2B Graph Management-Agent.  Hiermee worden de gast gebruikers omgezet in de tekst.  Op dit moment worden een of meer accounts geëxporteerd voor `ADMA`.  Als er geen uitvoer in behandeling is, controleert u of gast gebruikers zijn geïmporteerd in de connector ruimte en of de regels zijn geconfigureerd om AD-accounts te krijgen.

-   Exporteren, Delta-import en synchronisatie in de `ADMA` beheer agent.  Als de export is mislukt, controleert u de regel configuratie en bepaalt u of er ontbrekende schema vereisten zijn. 

-   Exporteren, Delta-import en synchronisatie in de `MIMMA` beheer agent.  Wanneer dit is voltooid, moeten er geen export bewerkingen meer in behandeling zijn.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Optioneel: toepassings proxy voor B2B-gasten meldt zich aan bij de MIM-Portal

Nu hebben we de synchronisatie regels in MIM gemaakt. In de configuratie van de app-proxy definieert u de Cloud-Principal gebruiken om KCD op de app-proxy toe te staan.
Daarnaast hebt u de gebruiker vervolgens hand matig toegevoegd aan de gebruikers en groepen beheren. De opties om de gebruiker pas weer te geven nadat deze is gemaakt in MIM om de gast toe te voegen aan een Office-groep nadat het is ingericht, hebt u nog iets meer configuratie nodig die niet in dit document wordt besproken.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Als alle configuraties zijn geconfigureerd, hebben B2B-gebruikers zich aangemeld en zien ze de toepassing.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Volgende stappen
----------

[Hoe kan ik gebruikers inrichten voor AD DS?](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Functiereferentie voor FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[How to provide secure remote access to on-premises applications](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) (Beveiligde externe toegang bieden voor on-premises toepassingen)

[Microsoft Identity Manager connector voor Microsoft Graph downloaden](https://go.microsoft.com/fwlink/?LinkId=717495)
