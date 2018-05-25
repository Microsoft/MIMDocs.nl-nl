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
ms.openlocfilehash: ac11a4dfb23944d50dbbcf0b0d70c915f186c159
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/25/2018
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>Azure AD-business-to-business (B2B) samenwerking met Microsoft Identity Manager(MIM) 2016 SP1 met Azure-toepassingsproxy (openbare Preview)
============================================================================================================================

Het eerste scenario in preview voor is externe gebruiker levenscyclusbeheer van AD-account.   In dit scenario kan een organisatie gasten in hun Azure AD-directory heeft uitgenodigd en wenst te geven die gasten toegang krijgen tot lokale Windows-verificatie of toepassingen op basis van Kerberos via de [Azure AD-toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)proxy of andere wijze gateway. De Azure AD-toepassingsproxy is vereist voor elke gebruiker hebben hun eigen AD DS-account voor identificatie en overdracht

## <a name="scenario-specific-supported-guidance"></a>Scenario-specifieke ondersteund richtlijnen

In dit scenario wordt een organisatie gasten in hun Azure AD-directory heeft uitgenodigd en wil deze gasten toegang geven tot lokale Windows. Geïntegreerde verificatie of toepassingen op basis van Kerberos via de [Azure AD-toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) proxy of andere wijze gateway. De Azure AD-toepassingsproxy is vereist voor elke gebruiker hebben hun eigen AD DS-account voor identificatie en overdracht

Enkele veronderstellingen in de configuratie van B2B met MIM en Azure-toepassingsproxy

-   U al hebt geïnstalleerd het [grafiek Management Agent](microsoft-identity-manager-2016-connector-graph.md).

-   U hebt een on-premises AD en Azure AD Connect set up voor het synchroniseren van gebruikers en groepen naar Azure AD.

    -   Office-groepen beheren van de toepassing openen met behulp [Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   U al toepassingsproxy connectors en groepen van de connector hebt ingesteld, als dat niet het vindt u [hier](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) installeren en configureren

-   Een of meer toepassingen die afhankelijk van geïntegreerde Windows-verificatie of afzonderlijke AD-accounts via toepassingsproxy van Azure AD zijn gepubliceerd

-   U hebt uitgenodigd of u een of meer gasten die zijn gemaakt in Azure AD uitnodigen <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>

-   Microsoft Identity Manager is geïnstalleerd en eenvoudige configuratie van de Service en -Portal en Active Directory-beheeragent.
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>B2B-End-to-End-implementatie

Scenario

Trey Research Inc. werkt Contoso Pharmaceuticals als onderdeel van hun R & D-afdeling. Trey Research werknemers nodig toegang tot de toepassing geleverd door Contoso Pharmaceuticals rapportage onderzoeken.

-   Contoso Pharmaceuticals zijn in een onafhankelijke tenant, om een aangepast domein hebt geconfigureerd.

-   Een externe gebruiker uitgenodigd: aan de Contoso Pharmaceuticals tenant.
    Deze gebruiker de uitnodiging heeft geaccepteerd en toegang tot bronnen die worden gedeeld.

-   Gepubliceerd van een toepassing via toepassingsproxy van en in dit scenario als voorbeeld gebruiken MIM-Service en Portal voor gastgebruiker deel te nemen in MIM proces voorbeeld helpdeskscenario's.

## <a name="create-the-graph-management-agent"></a>De grafiek-beheeragent maken

Opmerking: Controleer voordat u het maken van connector, lees de [grafiek Management Agent](microsoft-identity-manager-2016-connector-graph.md).

Selecteer in de UI Synchronization Service Manager **Connectors** en **maken**.
Selecteer **grafiek (Microsoft)** en wijs hieraan een beschrijvende naam

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Connectiviteit

Op de pagina Verbindingsmogelijkheden moet u de Graph API-versie productie-klaar is **V 1.0**, niet-productieve is **Beta**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>Mogelijkheden

Op de pagina Algemene Parameters, moet u de DN-naam naar de delta-wijzigingenlogboek en extra functies voor LDAP configureren. De pagina is al ingevuld met de informatie die wordt geleverd door de LDAP-server.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>Globale Parameters

Op de pagina Algemene Parameters, moet u de DN-naam naar de delta-wijzigingenlogboek en extra functies voor LDAP configureren. De pagina is al ingevuld met de informatie die wordt geleverd door de LDAP-server.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Hiërarchie inrichten configureren

Deze pagina wordt gebruikt voor het onderdeel DN-naam, bijvoorbeeld OU toewijzen aan het objecttype dat moet worden ingericht, bijvoorbeeld organizationalUnit. Laat deze standaard klikt u op volgende

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Partities en hiërarchieën configureren

Selecteer op de pagina partities en hiërarchieën alle naamruimten met objecten die u wilt importeren en exporteren.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Selecteer het objecttype

Selecteer op de pagina partities en hiërarchieën alle naamruimten met objecten die u wilt importeren en exporteren.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Kenmerken selecteren

Selecteer op het scherm kenmerken selecteren vereiste kenmerken voor het beheren van B2B-gebruikers. Het kenmerk 'id' is vereist

-   ID

-   displayName

-   E-mail

-   givenName

-   Achternaam

-   userPrincipalName

-   UserType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Ankers configureren

Met het anker configureren is het anker configureren een vereiste stap. standaard gebruik van het kenmerk id voor de toewijzing.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Connectorfilter configureren

Op de configureren kunt Connectorfilter pagina, u filteren op basis van kenmerkfilter objecten. In dit scenario voor B2B, wordt het doel alleen op te zetten in gebruikers met het userType die gelijk is aan de Gast en geen lid is.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Lid worden en Projectieregels configureren

Regels configureren lid worden en projectie worden verwerkt door de synchronisatieregel voor, niet nodig hebt om een lid worden en projectie op de connector zelf te identificeren. Laat de standaardinstelling en klik op ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Kenmerkstroom configureren

Als het lid worden en projectie is niet nodig voor het definiëren van de kenmerkstroom omdat deze ingang door de synchronisatieregel die moet worden later maken. Laat de standaardinstelling en klik op ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configureren van identiteitsgegevens

Configureer deprovision kunt u het object verwijderen als metaverse-object is verwijderd. In deze test maken we ze disconnectors als het doel is te laat ze in Azure ook we niet exporteert naar azure omdat deze alleen importeren.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Extensies configureren

Uitbreidingen configureren op het gebied van deze agent is een optie maar niet vereist als we een synchronisatieregel gebruiken. Als we besloten voor een geavanceerde regel in de kenmerkstroom eerder is, zou dit een optie voor het definiëren van zijn.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>Maken van synchronisatieregels voor MIM-Service

we beginnen de toewijzing van Gast-account voor B2B en de kenmerkstroom in de onderstaande stappen. Bepaalde veronderstellingen hier dat u al het Active Directory Management Agent is geconfigureerd hebt en de FIM MA geconfigureerd om te communiceren met de MIM-Service en de portal.

Voordat u de synchronisatieregel maakt, moet er een kenmerk met de naam gekoppeld aan de persoon-object op met de ontwerpfunctie MV userPrincipalName maken.

Selecteer in de client synchronisatie Metaverse Designer

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Selecteer het objecttype persoon

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Klik vervolgens op kenmerk toevoegen onder Acties

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Voltooi ten slotte de volgende details

Kenmerknaam: **userPrincipalName**

Kenmerktype: **tekenreeks (worden geïndexeerd)**

Geïndexeerde = **True**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

De volgende stappen wordt de minimale configuratie van de configuratie van de FIM-Service Management Agent en de Active Directory Domain Services-beheeragent vereisen.

Meer informatie vindt u hier om de configuratie <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> -hoe kan ik gebruikers inrichten in AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regel voor het synchroniseren: Gastgebruiker in MV voor synchronisatie Service Metaverse van Azure Active Directory importeren<br>

Navigeer naar de MIM-Service en Portal en selecteer Sycronization regels en klik op Nieuw.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png) Selecteer 'Resource maken in FIM' ![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Stroomregels:

| **Alleen de initiële stroom** | **Gebruik als de Test bestaan** | **Stroom (FIM waarde ⇒ bestemming kenmerk)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName] (javascript:void(0);)                        |
|                       |                           | [Links (id 20) ⇒accountName] (javascript:void(0);)                        |
|                       |                           | [id⇒uid] (javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType] (javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName] (javascript:void(0);)                            |
|                       |                           | [surname⇒sn] (javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName] (javascript:void(0);)            |
|                       |                           | [id⇒cn] (javascript:void(0);)                                          |
|                       |                           | [mail⇒mail] (javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone] (javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Regel voor het synchroniseren: Gast-gebruikersaccount in Active Directory maken 

Deze synchronisatieregel maakt de gebruiker in active directory

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Stroomregels:

| **Alleen de initiële stroom** | **Gebruik als de Test bestaan** | **Stroom (FIM waarde ⇒ bestemming kenmerk)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName] (javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName] (javascript:void(0);)                            |
|                       |                           | [mail⇒mail] (javascript:void(0);)                                      |
|                       |                           | [sn⇒sn] (javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName] (javascript:void(0);)            |
| **Y**                 |                           | ["CN = '+ uid +', organisatie-eenheid B2BGuest, DC = = scontoso, DC = com" ⇒dn] (javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd] (javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl] (javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Regel voor het synchroniseren: Import B2B Gast objecten SID gebruiker voor aanmelding bij MIM 

Deze synchronisatieregel maakt de gebruiker in active directory

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

Ten slotte we nodigen van de gebruiker en voer vervolgens het beheer in de volgende volgorde:

-   Volledige Import en synchronisatie op de beheeragent MIMMA

-   Volledige Import en synchronisatie op de beheeragent ADMA_SCONTOSO_B2B

-   Volledige Import en synchronisatie op de beheeragent van de B2B-grafiek

-   Exporteren, Delta-Import en synchronisatie op de beheeragent ADMA_SCONTOSO_B2B

-   Exporteren, Delta-Import en synchronisatie op de beheeragent MIMMA

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>Ten slotte: Toepassingsproxy met B2B Gast en logboekregistratie in MIM

Nu dat we de synchronisatieregels hebt gemaakt in MIM. In de App-proxyconfiguratie definieert het principe van de cloud gebruiken om toe te staan voor KCD op app-proxy.
Ook, naast de gebruiker handmatig toegevoegd aan de gebruikers en groepen beheren. De opties weer te geven van de gebruiker totdat maken is opgetreden in MIM om toe te voegen op de Gast aan een groep eenmaal is ingericht met office vereist iets meer configuratie niet in dit document worden behandeld.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **Alleen de initiële stroom** | **Gebruik als de Test bestaan** | **Stroom (FIM waarde ⇒ bestemming kenmerk)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName] (javascript:void(0);)                     |
|                       |                           | ['CONTOSO' ⇒domain] (javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid] (javascript:void(0);)                                      |

Zodra alle configureren

Ten slotte B2B gebruikersaanmelding en de toepassing bekijken

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Volgende stappen
----------

[Gebruikers inrichten voor AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Functiereferentie voor FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Het verstrekken van veilige externe toegang tot on-premises toepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Microsoft Identity Manager-management-agent voor Microsoft Graph (preview) downloaden](http://go.microsoft.com/fwlink/?LinkId=717495)
