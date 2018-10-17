---
title: De Microsoft Identity Manager-connector voor Microsoft Graph configureren voor B2B | Microsoft Docs
author: billmath
description: Microsoft Graph-connector is een externe gebruiker levenscyclusbeheer voor AD-account. In dit scenario, een organisatie is uitgenodigd voor gasten hun Azure AD-directory en wil deze gasten-toegang geven tot on-premises Windows-Integrated verificatie- of Kerberos-toepassingen
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 139c58510117ad422529a4ff0facd23040023713
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358768"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Azure AD-business-to-business (B2B) samenwerking met Microsoft Identity Manager(MIM) 2016 SP1 met Azure Application Proxy
============================================================================================================================

Het eerste scenario is de externe gebruiker levenscyclusbeheer voor AD-account.   In dit scenario, een organisatie is uitgenodigd voor gasten hun Azure AD-directory en wil deze gasten toegang geven tot on-premises Windows-Integrated verificatie- of Kerberos-toepassingen, via de [Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) of andere mechanismen gateway. De Azure AD-toepassingsproxy moet elke gebruiker hebben hun eigen AD DS-account voor identificatie- en overdracht.

## <a name="scenario-specific-guidance"></a>Scenario-specifieke richtlijnen

Enkele veronderstellingen in de configuratie van B2B met MIM en AD-toepassingsproxy:

-   U hebt al geïmplementeerd met een on-premises AD, en Microsoft Identity Manager is geïnstalleerd en eenvoudige configuratie van MIM-Service, MIM-Portal, Active Directory-beheeragent (MA AD) en de FIM-beheeragent (FIM MA).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   U hebt al de instructies in het artikel gevolgd voor het downloaden en installeren de [Graph connector](microsoft-identity-manager-2016-connector-graph.md).

-   Hebt u Azure AD Connect geconfigureerd voor het synchroniseren van gebruikers en groepen naar Azure AD.

-   Hebt u Azure AD Connect geconfigureerd voor het synchroniseren van Office-groepen voor het beheren van de toepassing [terug naar on-premises AD DS](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   U al Application Proxy connectors en connectorgroepen hebt ingesteld, indien niet vindt u [hier](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) installeren en configureren

-   U hebt een of meer toepassingen die afhankelijk van geïntegreerde Windows-verificatie of afzonderlijke AD-accounts via toepassingsproxy van Azure AD zijn al gepubliceerd

-   U hebt uitgenodigd of u een of meer gasten, die hebben geleid tot een of meer gebruikers die worden gemaakt in Azure AD uitnodigen <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Voorbeeldscenario voor B2B-End-to-End implementatie

Deze handleiding is gebaseerd op het volgende scenario:

Contoso Pharmaceuticals werkt met Trey Research Inc. als onderdeel van hun R & D-afdeling. Trey Research werknemers nodig voor toegang tot de toepassing die worden geleverd door Contoso Pharmaceuticals rapportage onderzoeken.

-   Contoso Pharmaceuticals zijn in hun eigen tenant, om een aangepast domein hebt geconfigureerd.

-   Iemand heeft een externe gebruiker voor de tenant van Contoso Pharmaceuticals uitgenodigd.
    Deze gebruiker de uitnodiging heeft geaccepteerd en toegang hebben tot resources die worden gedeeld.

-   Een toepassing via App Proxy, Contoso Pharmaceuticals heeft gepubliceerd. In dit scenario wordt de voorbeeldtoepassing met de MIM-Portal. Hierdoor zou een gastgebruiker om deel te nemen in MIM processen, bijvoorbeeld in de help helpdesk-scenario's of voor het aanvragen van toegang tot groepen in MIM.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Configureren van AD en Azure AD Connect wilt uitsluiten van gebruikers die zijn toegevoegd vanuit Azure AD

Standaard wordt Azure AD Connect wordt ervan uitgegaan dat gebruikers van niet-beheerders in Active Directory moeten worden gesynchroniseerd met Azure AD.  Als Azure AD Connect vindt een bestaande gebruiker in Azure AD die overeenkomt met de gebruiker van on-premises AD, Azure AD Connect wordt overeenkomen met de twee accounts en wordt ervan uitgegaan dat dit een oudere synchronisatie van de gebruiker is en controleer de on-premises AD gezaghebbende.  Dit standaardgedrag is echter niet geschikt is voor de stroom B2B waar het gebruikersaccount dat afkomstig is in Azure AD. 

Daarom moeten de gebruikers in AD DS door MIM gebracht van Azure AD worden opgeslagen op een manier die Azure AD niet getracht wordt te synchroniseren van deze gebruikers terug naar Azure AD.
Een manier om dit te doen is een nieuwe organisatie-eenheid maken in AD DS en Azure AD Connect als u wilt uitsluiten van die organisatie-eenheid configureren.  

Meer informatie vindt u [Azure AD Connect-synchronisatie: filtering configureren](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>De Azure AD-toepassing maken 


Opmerking: Voordat u in MIM Sync de beheeragent voor de graph-connector, zorg ervoor dat u hebt gecontroleerd dat de handleiding voor het implementeren van de [Graph Connector](microsoft-identity-manager-2016-connector-graph.md), en een toepassing gemaakt met een client-ID en geheim.
Zorg ervoor dat de toepassing is geautoriseerd voor ten minste één van deze machtigingen: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` of `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>De nieuwe-beheeragent maken


Selecteer in de UI Synchronization Service Manager **Connectors** en **maken**.
Selecteer **Graph (Microsoft)** en wijs hieraan een beschrijvende naam.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Connectiviteit

Op de pagina verbinding, moet u de Graph API-versie. Productie gereed PAI is **V 1.0**, niet-productie **Bèta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Globale Parameters

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Hiërarchie inrichten configureren

Deze pagina wordt gebruikt om het onderdeel van de DN-naam, bijvoorbeeld organisatie-eenheid toewijzen aan het objecttype dat moet worden ingericht, bijvoorbeeld organizationalUnit. Dit is niet nodig voor dit scenario, dus laat dit als de standaardinstelling en klik op volgende.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Partities en hiërarchieën configureren

Selecteer op de pagina partities en hiërarchieën, alle naamruimten met objecten die u van plan bent om te importeren en exporteren.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Selecteer het objecttype

Selecteer de objecttypen die u van plan bent om te importeren op de pagina van het type object. U moet ten minste selecteren 'User'.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Kenmerken selecteren

Selecteer op het scherm kenmerken selecteren kenmerken van Azure AD die nodig zijn voor het beheren van B2B-gebruikers in AD. Het kenmerk 'ID' is vereist.  De kenmerken `userPrincipalName` en `userType` verderop in deze configuratie wordt gebruikt.  Overige kenmerken zijn optioneel, met inbegrip van

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Ankers configureren

Op het scherm anker configureren is het kenmerk van bronanker configureren een vereiste stap. Standaard, gebruikt u de id-kenmerk voor de Gebruikerstoewijzing van de.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Connectorfilter configureren

Op de pagina van de Connectorfilter configureren kunt MIM u filteren op basis van kenmerkfilter objecten. In dit scenario voor B2B, het doel is alleen in te zetten voor gebruikers met de waarde van de `userType` kenmerk die gelijk is aan `Guest`, en niet voor gebruikers met het userType die gelijk is aan `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Lid worden en Projectieregels configureren

Deze handleiding wordt ervan uitgegaan dat u maakt een synchronisatieregel voor.  Als regels voor lid worden en projectie configureren door de synchronisatieregel worden verwerkt, is het niet nodig hebt om een lid worden en projectie van de connector zelf te identificeren. Laat de standaardinstelling en klik op ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Kenmerkstroom configureren

Deze handleiding wordt ervan uitgegaan dat u maakt een synchronisatieregel voor.  Projectie is niet nodig voor het definiëren van de kenmerkstroom in MIM-synchronisatie, zoals deze wordt verwerkt door de synchronisatieregel die later wordt gemaakt. Laat de standaardinstelling en klik op ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Inrichting configureren

De instelling van inrichting configureren kunt u voor het configureren van MIM sync als u wilt verwijderen van het object als het metaverse-object is verwijderd. In dit scenario maken we ze disconnectors als het doel is om in Azure AD laten staan. In dit scenario, zijn we iets niet exporteren naar Azure AD en de connector is geconfigureerd voor het importeren van alleen.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Extensies configureren

Extensies configureren op het gebied deze agent is een optie, maar niet vereist als u een regel voor synchronisatie. Als we besloten om een geavanceerde regel eerder in de kenmerkstroom gebruiken, zou dan er een optie voor het definiëren van de extensie regels.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Het metaverse-schema uitbreiden


Voordat u de synchronisatie-regel maakt, moet een kenmerk met de naam gekoppeld aan het object persoon met de ontwerpfunctie voor MV userPrincipalName maken.

Selecteer in de synchronisatie-client, Metaverse-ontwerper

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Selecteer het objecttype van de persoon

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Klik daarna op kenmerk toevoegen onder Acties

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Voltooi de volgende details

De naam van kenmerk: **userPrincipalName**

Kenmerktype: **tekenreeks (indexeerbare)**

Geïndexeerde = **True**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Het maken van synchronisatieregels voor MIM-Service

we gaan de toewijzing van B2B-Gast-account en de kenmerkstroom in de onderstaande stappen. Bepaalde veronderstellingen hier: dat u al de Active Directory MA geconfigureerd hebt en de FIM MA geconfigureerd voor het onderbrengen van gebruikers tot de MIM-Service en -Portal.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

De volgende stappen wordt het toevoegen van minimale configuratie voor de FIM MA en de AD MA vereisen.

Meer informatie vindt u hier voor de configuratie van <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> -hoe kan ik gebruikers inrichten voor AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regel voor synchronisatie: Gastgebruiker in MV aan synchronisatie Service Metaverse uit Azure Active Directory importeren<br>

Navigeer naar de MIM-Portal, selecteert u synchronisatieregels en klikt u op Nieuw.  Maak een regel voor inkomende synchronisatie voor de B2B-stroom via de graph-connector.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

Zorg dat u selecteert u 'Bron maken in FIM' bij de stap van de criteria relatie.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Configureer de volgende regels van de inkomende kenmerkstroom-stroom.  Zorg ervoor dat voor het vullen van de `accountName`, `userPrincipalName` en `uid` kenmerken zoals ze verderop in dit scenario worden gebruikt:

| **Alleen initiële stroom** | **Gebruiken als Bestaanstest** | **Stroom (bron waarde ⇒ FIM-kenmerk)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName](javascript:void(0);)                        |
|                       |                           | [Links (id 20) ⇒accountName](javascript:void(0);)                        |
|                       |                           | [id⇒uid](javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType](javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [surname⇒sn](javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
|                       |                           | [id⇒cn](javascript:void(0);)                                          |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone](javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Regel voor synchronisatie: Gast-gebruikersaccount in Active Directory maken 

Deze synchronisatieregel wordt gemaakt van de gebruiker in Active Directory.  Zorg ervoor dat de stroom voor `dn` moet de gebruiker plaatsen in de organisatie-eenheid die is uitgesloten van Azure AD Connect.  De stroom voor ook bijwerken `unicodePwd` om te voldoen aan het wachtwoordbeleid van uw AD - de gebruiker niet moet het wachtwoord kennen.  Noteer de waarde van `262656` voor `userAccountControl` codeert de vlaggen `SMARTCARD_REQUIRED` en `NORMAL_ACCOUNT`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Stroomregels:

| **Alleen initiële stroom** | **Gebruiken als Bestaanstest** | **Stroom (FIM voor waarde ⇒ Bestemmingskenmerk)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN ="+ uid +", OE = B2BGuest, DC = contoso, DC = com" ⇒dn] (javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Optionele Synchronisatieregel: Importeren B2B-Gast gebruiker objecten SID om toe te staan voor aanmelden bij MIM 

Deze synchronisatieregel voor binnenkomende gegevens voorziet in beveiligings-id-kenmerk van de gebruiker uit Active Directory weer in MIM, zodat de gebruiker toegang heeft tot de MIM-Portal.  De MIM-Portal vereist dat de gebruiker de kenmerken `samAccountName`, `domain` en `objectSid` ingevuld in de MIM-Service-database.

Configureren van het externe systeem van de bron als het `ADMA`, als de `objectSid` kenmerk wordt automatisch door AD MIM wanneer de gebruiker maakt worden ingesteld.
 
Houd er rekening mee dat als u gebruikers moet worden gemaakt in de MIM-Service configureren, zorg ervoor dat ze niet binnen het bereik van alle sets die bestemd zijn voor werknemer SSPR beheerbeleidsregels.  Mogelijk moet u uw definities om te voorkomen dat gebruikers die zijn gemaakt door de B2B-stroom wijzigen. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Alleen initiële stroom** | **Gebruiken als Bestaanstest** | **Stroom (bron waarde ⇒ FIM-kenmerk)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ['CONTOSO' ⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>De synchronisatieregels die zijn uitgevoerd

Vervolgens wordt de gebruiker uitnodigen en voer vervolgens de management agent synchronisatieregels in de volgende volgorde:

-   Volledige Import en synchronisatie op de `MIMMA` beheeragent.  Dit zorgt ervoor dat MIM Sync is de meest recente synchronisatieregels die zijn geconfigureerd.

-   Volledige Import en synchronisatie op de `ADMA` beheeragent.  Dit zorgt ervoor dat de MIM- en Active Directory consistent zijn.  Op dit moment, wordt er nog niet worden geëxporteerd in behandeling voor gasten.

-   Volledige Import en synchronisatie op de beheeragent van de B2B-grafiek.  Hiermee wordt in de gastgebruikers in de metaverse.  Op dit moment worden een of meer accounts exportbewerking `ADMA`.  Als er geen uitvoer in behandeling zijn, controleert u of gastgebruikers zijn geïmporteerd in het connectorgebied en dat de regels zijn geconfigureerd voor deze AD-accounts worden vermeld.

-   Exporteren, Delta-Import en synchronisatie op de `ADMA` beheeragent.  Als de uitvoer is mislukt, Controleer de regelconfiguratie van de en bepalen of er ontbrekende schemavereisten zijn. 

-   Exporteren, Delta-Import en synchronisatie op de `MIMMA` beheeragent.  Wanneer dit is voltooid, moet er niet meer worden geëxporteerd in behandeling.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Optioneel: Toepassingsproxy voor B2B-gasten wordt aangemeld bij de MIM-Portal

Nu dat we hebben de synchronisatieregels die zijn gemaakt in MIM. In de configuratie van de App-Proxy, definieert het cloud-principal gebruiken om toe te staan voor KCD op app-proxy.
Ook, naast de gebruiker handmatig toegevoegd aan de gebruikers en groepen beheren. De opties niet weer te geven van de gebruiker tot het maken is opgetreden in MIM om toe te voegen de Gast aan een office-groep eenmaal ingericht iets meer configuratie niet in dit document behandeld nodig is.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Zodra alle geconfigureerde, B2B-gebruikersaanmelding hebt en wordt de toepassing.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Volgende stappen
----------

[Gebruikers inrichten voor AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Functiereferentie voor FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Het bieden van veilige externe toegang tot on-premises toepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Microsoft Identity Manager-connector voor Microsoft Graph downloaden](http://go.microsoft.com/fwlink/?LinkId=717495)
