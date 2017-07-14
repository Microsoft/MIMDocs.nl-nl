---
title: Privileged Access Management voor Active Directory Domain Services | Microsoft Docs
description: Meer informatie over Privileged Access Management en hoe u hiermee uw Active Directory-omgeving kunt beheren en beveiligen.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 1034db2b33ffd680e673f975af17145e9cf4c312
ms.contentlocale: nl-nl
ms.lasthandoff: 07/10/2017

---

# Privileged Access Management voor Active Directory Domain Services
<a id="privileged-access-management-for-active-directory-domain-services" class="xliff"></a>
Privileged Access Management (PAM) is een oplossing waarmee organisaties bevoorrechte toegang in een bestaande Active Directory-omgeving kunnen beperken.

Met Privileged Access Management kunnen twee doelen worden behaald:

- De controle terugkrijgen over een aangetaste Active Directory-omgeving door een afzonderlijke bastionomgeving te behouden waarvan bekend is dat deze niet is beïnvloed door aanvallen.  
- Het gebruik van bevoorrechte accounts isoleren om het risico te verminderen dat deze referenties worden gestolen.

> [!NOTE]
> PAM is een instantie van [Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM) die is geïmplementeerd met Microsoft Identity Manager (MIM).

## Welke problemen kunnen worden opgelost met PAM?
<a id="what-problems-does-pam-help-solve" class="xliff"></a>
Een echt aandachtspunt voor moderne ondernemingen is toegang tot resources in een Active Directory-omgeving. Nieuws over beveiligingsproblemen, escalaties door niet-geautoriseerde verhoging van bevoegdheden andere typen onbevoegde toegang, waaronder pass-the-hash, pass-the-ticket, spear phishing en Kerberos-problemen zijn vooral zorgwekkend.

Het is tegenwoordig te gemakkelijk voor aanvallers om de accountreferenties van Domeinbeheerders te achterhalen en het is te moeilijk om deze aanvallen achteraf te detecteren. Het doel van PAM is om de mogelijkheden voor kwaadwillende gebruikers om toegang te krijgen te verminderen terwijl het beheer en het bewustzijn van de omgeving voor u worden vergroot.

Dankzij PAM is het moeilijker voor kwaadwillende personen om door te dringen tot een netwerk en bevoorrechte accounttoegang te verkrijgen. Er wordt met PAM beveiliging toegevoegd aan bevoorrechte groepen waarmee de toegang wordt bepaald op verschillende computers die lid zijn van een domein en de toepassingen op deze computers. Ook worden meer controle, meer zichtbaarheid en specifieke besturingselementen toegevoegd zodat organisaties zien kunnen wie de bevoorrechte beheerders zijn en wat ze doen. Dankzij PAM hebben organisaties meer inzicht in hoe beheerdersaccounts worden gebruikt in de omgeving.

## Hoe wordt PAM ingesteld?
<a id="how-is-pam-set-up" class="xliff"></a>
PAM borduurt voort op het principe van Just-In-Time-beheer; [Just Enough Administration (JEA)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362). JEA is een Windows PowerShell-toolkit waarmee een reeks opdrachten wordt gedefinieerd voor het uitvoeren van bevoorrechte activiteiten en een eindpunt waar beheerders autorisatie kunnen verkrijgen voor het uitvoeren van deze opdrachten. In JEA besluit een beheerder dat gebruikers met een bepaalde bevoegdheid een bepaalde taak kunnen uitvoeren. Elke keer dat een in aanmerking komende gebruiker deze taak moet uitvoeren, wordt deze machtiging ingeschakeld. De machtigingen verlopen na een opgegeven periode, zodat een kwaadwillende gebruiker de toegang niet kan stelen.

De installatie en het gebruik van PAM bestaat uit vier stappen.

![PAM-stappen: voorbereiden, beveiligen, werken, bewaken - diagram](media/MIM_PIM_SetupProcess.png)

1.  **Voorbereiden**: identificeer welke groepen in uw bestaande forest aanzienlijke bevoegdheden hebben. Maak deze groepen opnieuw zonder leden in het bastionforest.

2.  **Beveiligen**: Stel levensduur- en verificatiebeveiliging, zoals Multi-Factor Authentication (MFA), in voor wanneer gebruikers Just-In-Time-beheer aanvragen. MFA voorkomt programmatische aanvallen van schadelijke software of na de diefstal van referenties.

3.  **Werken**: Nadat is voldaan aan de verificatievereisten en een aanvraag is goedgekeurd, wordt een gebruikersaccount tijdelijk toegevoegd aan een bevoorrechte groep in het bastionforest. De beheerder beschikt gedurende een vooraf ingestelde periode over alle bevoegdheden en toegangsmachtigingen die zijn toegewezen aan die groep. Na deze periode wordt het account verwijderd uit de groep.

4.  **Controleren**: Er wordt met PAM functionaliteit toegevoegd voor controle, waarschuwingen en rapporten van aanvragen voor bevoorrechte toegang. U kunt de geschiedenis van bevoorrechte toegang controleren en zien wie een activiteit heeft uitgevoerd. U kunt beslissen of de activiteit geldig is en eenvoudig onbevoegde activiteiten identificeren, zoals een poging tot het rechtstreeks toevoegen van een gebruiker aan een bevoorrechte groep in het oorspronkelijke forest. Deze stap is niet alleen belangrijk voor het identificeren van schadelijke software, maar ook voor bijhouden van aanvallers van binnenuit.

## Hoe werkt PAM?
<a id="how-does-pam-work" class="xliff"></a>
PAM is gebaseerd op de nieuwe mogelijkheden in AD DS, met name voor domeinaccountverificatie en -autorisatie en de nieuwe functies in Microsoft Identity Manager. Met PAM worden bevoorrechte accounts gescheiden van een bestaande Active Directory-omgeving. Wanneer een bevoorrecht account moet worden gebruikt, moet dit eerst worden aangevraagd en vervolgens goedgekeurd. Na de goedkeuring, wordt aan het bevoorrechte account een machtiging verleend via een Foreign Principal Group in een nieuw bastionforest in plaats van in het huidige forest van de gebruiker of toepassing. De organisatie heeft meer controle dankzij het gebruik van een bastionforest, zoals wanneer een gebruiker een lid kan zijn van een bevoorrechte groep en hoe de gebruiker moet worden geverifieerd.

Active Directory, de MIM-service en andere delen van deze oplossing kunnen ook worden geïmplementeerd in een maximaal beschikbare configuratie.

In het volgende voorbeeld wordt in detail uitgelegd hoe PIM werkt.

![PIM-proces en -deelnemers - diagram](media/MIM_PIM_howitworks.png)

Er worden met het bastionforest tijdelijke groepslidmaatschappen verleend, waarmee vervolgens tijdelijke tickets worden verleend (TGT’s). Kerberos-toepassingen of -services kunnen deze TGT's handhaven en afdwingen als de apps en services voorkomen in forests die het bastionforest vertrouwen.

Dagelijkse gebruikersaccounts hoeven niet te worden verplaatst naar een nieuw forest. Hetzelfde geldt voor de computers, toepassingen en de groepen. Ze blijven waar ze zijn in een bestaand forest. Bekijk het voorbeeld van een organisatie die zich bezighoudt met deze cyberbeveiligingsproblemen, maar geen directe plannen heeft om de serverinfrastructuur te upgraden naar de volgende versie van Windows Server. Deze organisatie kan nog steeds profiteren van deze gecombineerde oplossing met behulp van MIM en een nieuw bastionforest en de toegang tot bestaande resources beter beheren.

PAM biedt de volgende voordelen:

-   **Isolatie/bereik van bevoegdheden**: Gebruikers hebben geen bevoegdheden voor accounts die ook voor onbevoegde taken worden gebruikt, zoals e-mail controleren of internet gebruiken. Gebruikers moeten bevoegdheden aanvragen. Aanvragen worden goedgekeurd of geweigerd op basis van MIM-beleidsregels die zijn gedefinieerd door een PAM-beheerder. Tot een aanvraag is goedgekeurd, is bevoorrechte toegang niet beschikbaar.

-   **Meer bevoegdheden en verificatie**: Dit zijn nieuwe verificatie- en autorisatie-uitdagingen voor het beheer van de levenscyclus van afzonderlijke beheerdersaccounts. De gebruiker kan de uitbreiding van bevoegdheden voor een beheerdersaccount aanvragen en die aanvraag wordt verwerkt via de MIM-werkstroom.

-   **Aanvullende logboekregistratie**: samen met de geïntegreerde MIM-werkstromen, is er extra logboekregistratie voor PAM waarmee de aanvraag wordt geïdentificeerd en alle gebeurtenissen die plaatsvinden na de goedkeuring.

-   **Aanpasbare werkstroom**: de MIM-werkstromen kunnen worden geconfigureerd voor verschillende scenario's en meerdere werkstromen kunnen worden gebruikt, op basis van de parameters van de aanvragende gebruiker of aangevraagde rollen.

## Hoe kunnen gebruikers bevoorrechte toegang aanvragen?
<a id="how-do-users-request-privileged-access" class="xliff"></a>
Er zijn verschillende manieren waarop een gebruiker een aanvraag kan indienen, waaronder:  
- De webservices-API voor de MIM-services  
- Een REST-eindpunt  
- Windows PowerShell (`New-PAMRequest`)

Meer informatie over de [Privileged Access Management-cmdlets](https://technet.microsoft.com/library/mt604080.aspx).

## Welke werkstromen en controle-opties zijn beschikbaar?
<a id="what-workflows-and-monitoring-options-are-available" class="xliff"></a>
Stel dat een gebruiker lid was van een beheergroep voordat PIM werd ingesteld. Als onderdeel van de PIM-installatie wordt de gebruiker verwijderd uit de beheerdersgroep en wordt er een beleid gemaakt in MIM. Het beleid bepaalt dat als die gebruiker beheerdersbevoegdheden aanvraagt en wordt geverifieerd met MFA, de aanvraag wordt goedgekeurd en een afzonderlijk account voor de gebruiker wordt toegevoegd aan de bevoorrechte groep in het bastionforest.

Ervan uitgaande dat de aanvraag wordt goedgekeurd, communiceert de actiewerkstroom rechtstreeks met het bastionforest Active Directory om een gebruiker aan een groep toe te voegen. Wanneer Jen bijvoorbeeld een aanvraag indient voor het beheer van de HR-database, wordt het beheerdersaccount van Jen binnen enkele seconden toegevoegd aan de bevoorrechte groep in het bastionforest. Haar lidmaatschap van het beheerdersaccount in die groep verloopt na een bepaalde tijd. Met Windows Server Technical Preview wordt dat lidmaatschap gekoppeld in Active Directory met een tijdslimiet; met Windows Server 2012 R2 in het bastionforest wordt die termijn afgedwongen door MIM.

> [!NOTE]
> Wanneer u een nieuw lid toevoegt aan een groep, moet de wijziging worden gerepliceerd naar andere domeincontrollers (DC's) in het bastionforest. Replicatievertraging van invloed kan hebben op de mogelijkheid voor gebruikers om toegang krijgen tot resources. Zie [Hoe werkt Active Directory-replicatietopologie](https://technet.microsoft.com/library/cc755994.aspx) voor meer informatie over replicatielatentie.
>
> Daarentegen wordt een verlopen koppeling in real-time geëvalueerd door de Security Accounts Manager (SAM). Hoewel de toevoeging van een groepslid moet worden gerepliceerd door de domeincontroller die de toegangsaanvraag ontvangt, wordt het verwijderen van een groepslid direct geëvalueerd op een domeincontroller.

Deze werkstroom is specifiek bedoeld voor deze beheerdersaccounts. Beheerders (of zelfs scripts) die alleen incidenteel toegang nodig hebben voor bevoorrechte groepen, kunnen die toegang nauwkeurig aanvragen. De aanvraag en wijzigingen in Active Directory worden in het logboek vastgelegd met MIM. U kunt ze weergeven in Logboeken of de gegevens verzenden naar de bewakingsoplossingen van de onderneming zoals System Center 2012 - Operations Manager Audit Collection Services (ACS) of andere hulpprogramma's van derden.

