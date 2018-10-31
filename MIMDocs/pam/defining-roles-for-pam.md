---
title: Bevoorrechte rollen definiëren voor PAM | Microsoft Docs
description: Bepalen welke bevoorrechte rollen moeten worden beheerd en het beheerbeleid voor elke rol definiëren.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 38a9fc174c037e5d7c3ea17b46dcf9f6ea924822
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50380014"
---
# <a name="define-roles-for-privileged-access-management"></a>Rollen voor Privileged Access Management definiëren

Met Privileged Access Management kunt u gebruikers toewijzen aan bevoorrechte rollen die ze naar behoefte kunnen activeren voor Just-In-Time-toegang. Deze rollen worden handmatig gedefinieerd en ingesteld n de bastionomgeving. In dit artikel wordt uitgelegd hoe u kunt beslissen welke rollen u wilt beheren via PAM en hoe u deze met de juiste machtigingen en beperkingen kunt definiëren.

Een eenvoudige benadering voor het definiëren van rollen voor Privileged Access Management is door alle informatie in een werkblad te verzamelen. Vermeld de rollen in de rollen en gebruik de kolommen om beheervereisten en -machtigingen te identificeren.

De beheervereisten variëren, afhankelijk van de bestaande identiteit en toegangsbeleid of nalevingsvereisten. De parameters om te identificeren voor elke rol zijn onder andere:

- De eigenaar van de rol.
- De mogelijke gebruikers die in deze rol kunnen worden
- De verificatie, goedkeuring of melding besturingselementen die gekoppeld aan het gebruik van de rol worden moeten.

De rolmachtigingen zijn afhankelijk van de toepassingen die worden beheerd. In dit artikel wordt Active Directory gebruikt als een voorbeeldtoepassing, waarbij de machtigingen in twee categorieën worden ingedeeld:

- De machtigingen die nodig zijn voor het beheren van de Active Directory-service zelf (bijvoorbeeld de replicatietopologie configureren)

- De machtigingen die nodig zijn voor het beheren van de gegevens die in Active Directory zijn opgeslagen (bijvoorbeeld gebruikers en groepen maken)

## <a name="identify-roles"></a>Rollen identificeren

Begin met het vaststellen van de rollen die u wilt beheren met PAM. Op het werkblad is elke mogelijke rol in een eigen rij geplaatst.

Bekijk elke toepassing die mogelijk wordt gebruikt voor beheer om de juiste rollen te vinden:

- De toepassing in [laag 0, tier 1 of tier 2](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)?
- Wat zijn de machtigingen die van invloed zijn op de vertrouwelijkheid, integriteit of beschikbaarheid van de toepassing?
- De toepassing beschikt over afhankelijkheden van andere onderdelen van het systeem? Bijvoorbeeld, heeft deze afhankelijkheden van databases, netwerken, infrastructuur voor de beveiliging, virtualisatie of die als host fungeert platform?

Bepaal hoe u deze app-overwegingen wilt groeperen. U kunt het beste rollen instellen met duidelijke grenzen en alleen voldoende machtigingen verlenen om algemene beheertaken in de app te kunnen voltooien.

U kunt het beste rollen ontwerpen voor het toewijzen van de minste machtigingen. Dit kan zijn gebaseerd op de huidige (of geplande) organisatieverantwoordelijkheden voor gebruikers en is inclusief de bevoegdheid die is vereist voor de verantwoordelijkheden van de gebruiker. Het kan ook de bevoegdheden omvatten waardoor bewerkingen worden vereenvoudigd zonder risico’s te veroorzaken.

Andere overwegingen bij het instellen van het bereik van de machtigingen die moeten worden toegevoegd aan een rol zijn:

- Hoeveel mensen werken in een bepaalde rol? Als er niet ten minste twee personen zijn, is de rol mogelijk te nauwkeurig gedefinieerd en is deze niet handig of hebt u de verantwoordelijkheden van een bepaalde persoon gedefinieerd.

- Hoeveel rollen kan een persoon uitvoeren? Kunnen gebruikers de juiste rol voor hun taak selecteren?

- Zijn de gebruikers en de manier waarop ze omgaan met de toepassingen geschikt voor Privileged Access Management?

- Is het mogelijk om beheer en controle van elkaar te scheiden, zodat een gebruiker met een beheerrol de controlerecords van zijn acties niet kan wissen?

## <a name="establish-role-governance-requirements"></a>Stel de vereisten voor rolbeheer vast

Begin met het invullen van het werkblad wanneer u mogelijke rollen identificeert. Kolommen maken voor de vereisten die relevant zijn voor uw organisatie. Een aantal vereisten die u moet overwegen:

- Wie is de roleigenaar die verantwoordelijk is voor de verdere definitie van de rol, machtigingen kiest en de beheerinstellingen voor de rol onderhoudt?

- Wie zijn de rolhouders (gebruikers) die de rolverantwoordelijkheden of -taken uitvoeren?

- Welke toegangsmethode (besproken in de volgende sectie) zou geschikt zijn voor de rolhouders?

- Is handmatige goedkeuring door een roleigenaar vereist wanneer een gebruiker de rol activeert?

- Is een melding vereist wanneer een gebruiker de rol activeert?

- Moet voor het gebruik van deze rol een waarschuwing of melding in een SIEM-systeem worden gegenereerd om deze gegevens bij te houden?

- Is het nodig om de activering van de rol door gebruikers te beperken zodat ze alleen kunnen aanmelden bij computers waar toegang is vereist om de rolverantwoordelijkheden uit te voeren en waar er voldoende verificatie is van de host zodat hiermee de machtigingen/referenties kunnen worden beveiligd tegen misbruik?

- Is het vereist om een toegewezen beheerwerkstation toe te wijzen aan de rolhouders?

- Welke toepassingsmachtigingen (zie de lijst met voorbeelden voor AD hieronder) zijn gekoppeld aan deze rol?

## <a name="select-an-access-method"></a>Een toegangsmethode selecteren

Mogelijk zijn er meerdere rollen in een privileged access management-systeem met dezelfde machtigingen die aan hen zijn toegewezen. Dit kan gebeuren als verschillende community's van gebruikers verschillende vereisten voor toegangsbeheer hebben. Een organisatie kan bijvoorbeeld andere beleidsregels toepassen voor fulltime werknemers dan voor externe IT-medewerkers van een andere organisatie.

In sommige gevallen kan een gebruiker permanent worden toegewezen aan een rol. In dat geval hoeft ze niet aan te vragen of een roltoewijzing activeren. Voorbeelden van scenario’s voor permanente toewijzing:

- Een beheerd serviceaccount in een bestaand forest

- Een gebruikersaccount in het bestaande forest, met een referentie beheerd buiten PAM. Dit wordt mogelijk een "verbreken om"-account. De noodaccount kan moet een rol, zoals "domein / DC-onderhoud" problemen zoals vertrouwen en DC health om problemen te verhelpen. Als een noodaccount zou er de rol permanent toegewezen met een fysiek beveiligd wachtwoord)

- Een gebruikersaccount in het beheerforest dat wordt geverifieerd met een wachtwoord. Dit kan zijn, een gebruiker die permanent 24 x 7 met beheerdersrechten machtigingen hebben en zich aanmeldt vanaf een apparaat waarop sterke verificatie kan niet worden ondersteund.

- Een gebruikersaccount in het beheerforest met een smartcard of virtuele smartcard (bijvoorbeeld, een account met een offlinesmartcard, die nodig is voor zeldzame onderhoudstaken)

Voor organisaties die zich zorgen maken over de mogelijkheid van diefstal of misbruik van referenties bevat de handleiding [Azure MFA gebruiken voor activering](use-azure-mfa-for-activation.md) instructies voor het configureren van MIM om een extra buiten-bandcontrole te vereisen op het moment dat de rol wordt geactiveerd.

## <a name="delegate-active-directory-permissions"></a>Machtigingen voor Active Directory delegeren

Er worden met Windows Server automatisch standaardgroepen gemaakt zoals Domeinbeheerders wanneer nieuwe domeinen worden gemaakt. Dankzij deze groepen kunt u snel aan de slag en deze zijn mogelijk geschikt voor kleinere organisaties. Grotere organisaties of organisaties waarvoor meer isolatie van beheerdersbevoegdheden, moeten deze groepen leeg en deze vervangen door groepen met afzonderlijke machtigingen.

Een beperking van de groep Domeinbeheerders is dat deze geen leden van een extern domein kan bevatten. Een andere beperking is dat hiermee machtigingen voor drie afzonderlijke functies worden verleend:

- De Active Directory-service zelf beheren
- De gegevens in Active Directory beheren
- Externe aanmelding bij computers die lid zijn van het domein inschakelen.

In plaats van standaardgroepen zoals domeinbeheerders, nieuwe beveiligingsgroepen die alleen de benodigde machtigingen te maken. Vervolgens moet u MIM om dynamisch beheerdersaccounts met die groepslidmaatschappen gebruiken.

### <a name="service-management-permissions"></a>Machtigingen voor servicebeheer

In de volgende tabel worden voorbeelden gegeven van machtigingen die geschikt zijn voor opname in rollen voor het beheer van AD.

| Rol | Beschrijving |
| ---- | ---- |
| Domein/DC-onderhoud | Lidmaatschap van de groep domein\beheerders kunt voor het oplossen van problemen en het wijzigen van het besturingssysteem van de domain controller. Bewerkingen zoals het promoveren van een nieuwe domeincontroller naar een bestaand domein in het forest en de delegatie van de AD-rol.
|Virtuele DC's beheren | Domeincontrollers en virtuele machines beheren met de software voor virtualisatiebeheer. Deze bevoegdheid kan worden verleend via volledig beheer van alle virtuele machines in het beheerhulpprogramma of de functie voor op rollen gebaseerde toegang. |
| Schema uitbreiden | Het schema beheren, inclusief nieuwe objectdefinities toevoegen, machtigingen voor schemaobjecten wijzigen en standaardmachtigingen voor objecttypen van het schema wijzigen |
| Een back-up maken van een Active Directory-database | Een back-up maken van de volledige Active Directory-database, inclusief alle geheimen toevertrouwd aan de domeincontroller en het domein. |
| Vertrouwensrelaties en functionele niveaus beheren | Vertrouwensrelaties met externe domeinen en forests maken en verwijderen. |
| Sites, subnetten en replicatie beheren | De Active Directory-replicatietopologieobjecten beheren, inclusief sites, subnetten en sitekoppelingsobjecten wijzigen en replicatiebewerkingen initialiseren |
| GPO's beheren | Groepsbeleidsobjecten in het hele domein maken, verwijderen en aanpassen |
| Zones beheren | DNS-zones en -objecten in Active Directory maken, verwijderen en wijzigen |
| Organisatie-eenheden op het niveau tier 0 wijzigen | Organisatie-eenheden op het niveau tier 0 en opgenomen objecten in Active Directory wijzigen |

### <a name="data-management-permissions"></a>Machtigingen voor gegevensbeheer

De volgende tabel bevat voorbeelden van machtigingen die zijn geschikt voor opname in rollen voor het beheer of met behulp van de gegevens die zijn opgeslagen in AD.

| Rol | Beschrijving |
| ---- | ---- |
| Organisatie-eenheid op het niveau tier 1 voor beheer wijzigen                 | Organisatie-eenheden met beheerobjecten op het niveau tier 1 in Active Directory wijzigen |
| Organisatie-eenheid op het niveau tier 2 voor beheer wijzigen                 | Organisatie-eenheden met beheerobjecten op het niveau tier 2 in Active Directory wijzigen |
| Accountbeheer: maken/verwijderen/verplaatsen | Standaardgebruikersaccounts wijzigen                                      |
| Accountbeheer: opnieuw instellen/ontgrendelen       | Wachtwoorden opnieuw instellen en accounts ontgrendelen                                  |
| Beveiligingsgroep: maken/wijzigen          | Beveiligingsgroepen maken en wijzigen in Active Directory              |
| Beveiligingsgroep: verwijderen                 | Beveiligingsgroepen verwijderen uit Active Directory                         |
| GPO-beheer                         | Alle groepsbeleidsobjecten in een domein/forest beheren die niet van invloed zijn op servers op het niveau tier 0 beheren             |
| Pc/lokale beheerder toevoegen                    | Lokale beheerdersrechten op alle werkstations                               |
| Server/lokale beheerder toevoegen                   | Lokale beheerdersrechten op alle servers                                    |

## <a name="example-role-definitions"></a>Voorbeeldroldefinities

De keuze van roldefinities is afhankelijk van de laag van servers die worden beheerd. Dit is ook afhankelijk van de keuze van de toepassingen die worden beheerd. Toepassingen, zoals Exchange of van derden enterprise-producten, zoals SAP, vaak zijn voorzien hun eigen aanvullende roldefinities voor gedelegeerd beheer.

De volgende secties bevatten voorbeelden voor typisch zakelijke scenario's.

### <a name="tier-0---administrative-forest"></a>Tier 0 - beheerderforest

Rollen die geschikt zijn voor accounts in de bastionomgeving zijn onder andere:

- Toegang tot het beheerforest in noodgevallen
- Beheerders met een rode kaart: gebruikers die beheerders zijn van het beheerforest
- Gebruikers die beheerders zijn van het productieforest
- Gebruikers aan wie beperkte beheerdersrechten voor toepassingen in het productieforest zijn verleend

### <a name="tier-0---enterprise-production-forest"></a>Tier 0 - zakelijk productieforest

Rollen die geschikt zijn voor het beheren van productieforestaccounts en resources op het niveau tier 0 zijn onder andere:

- Toegang tot het productieforest in noodgevallen
- Beheerders van het groepsbeleid
- DNS-beheerders
- PKI-beheerders
- Beheerders van AD-topologie en replicatie
- Virtualisatiebeheerders voor servers op het niveau van tier 0
- Opslagbeheerders
- Antimalwarebeheerder voor servers op het niveau van tier 0
- SCCM-beheerders voor SCCM op het niveau van tier 0
- SCOM-beheerders voor SCOM op het niveau van tier 0
- Back-upbeheerders voor servers op het niveau van tier 0
- Gebruikers van buiten-band- en baseboardbeheercontrollers (voor KVM of LOM) gekoppeld aan hosts op het niveau van tier 0

### <a name="tier-1"></a>Tier 1

Rollen voor het beheer en de back-up van servers op het niveau van tier 1 zijn onder andere:

- Serveronderhoud
- Virtualisatiebeheerders voor servers op het niveau van tier 1
- Beveiligingsscanneraccount
- Antimalwarebeheerder voor servers op het niveau van tier 1
- SCCM-beheerders voor SCCM op het niveau van tier 1
- SCOM-beheerders voor SCOM op het niveau van tier 1
- Back-upbeheerders voor servers op het niveau van tier 1
- Gebruikers van buiten-band- en baseboardbeheercontrollers (voor KVM of LOM) gekoppeld aan hosts op het niveau van tier 1

Rollen voor het beheer van bedrijfstoepassingen op tier 1 zijn mogelijk ook:

- DHCP-beheerders
- Exchange-beheerders
- Skype voor Bedrijven-beheerders
- SharePoint-farmbeheerders
- Cloudservicebeheerders, bijvoorbeeld een bedrijfswebsite of de openbare DNS
- Beheerders van HCM, financiële en juridische systemen

### <a name="tier-2"></a>Tier 2

Rollen voor het beheer van niet-administratieve gebruikers en computerbeheer zijn onder andere:

- Accountbeheerders
- Helpdesk
- Beveiligingsgroepbeheerders
- Ondersteuning voor werkstations op locatie

## <a name="next-steps"></a>Volgende stappen

- [Referentiemateriaal voor bevoegde toegang beveiligen](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)
- [Azure MFA gebruiken voor activering](use-azure-mfa-for-activation.md)
