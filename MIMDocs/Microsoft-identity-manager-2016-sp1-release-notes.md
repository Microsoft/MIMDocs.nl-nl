---
title: Microsoft Identity Manager 2016 Service Pack 1 | Microsoft Docs
description: Lees hoe MIM 2016 bijdraagt aan een veiliger, gebruiksvriendelijker identiteitsbeheer in de cloud en on-premises.
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 69d44af5eaef3665f3a55ea91f48d3658cd5e65c
ms.contentlocale: nl-nl
ms.lasthandoff: 07/10/2017


---
# Nieuw in Microsoft Identity Manager 2016 Service Pack 1
<a id="whats-new-for-microsoft-identity-manager-2016-service-pack-1" class="xliff"></a> #

Als onderdeel van de standaardreleasecyclus voor de service en het bijwerken van Microsoft Identity Manager hebben we [Microsoft Identity Manager (MIM) 2016 servicepack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212) uitgebracht. Dit document geeft een overzicht van de updates, verbeteringen, functies en wijzigingen die zijn opgenomen in deze release.

Als u tijdens een productie-implementatie van MIM SP1 problemen ondervindt, neemt u contact op met de klantenondersteuning van Microsoft.

We stellen uw feedback op prijs. Feedback of opmerkingen voor het productteam kunt u per e-mail verzenden naar [mim2016@microsoft.com.](mailto:mim2016@microsoft.com)



## Updates in dit servicepack
<a id="updates-in-this-service-pack" class="xliff"></a> #

### MIM
<a id="mim" class="xliff"></a>

- **Compatibiliteit van MIM-portal met meerdere browsers voor selfservice voor eindgebruikers:** met dit servicepack introduceren we ondersteuning voor de meest belangrijke browsers. Gebruikers hebben nu toegang tot en kunnen communiceren met de MIM-portal voor selfservice groeps- en profielbeheer vanuit Microsoft Edge, Chrome en Safari.

- **MIM-service-ondersteuning voor Exchange Online:** de MIM-service biedt ondersteuning voor het versturen en ontvangen van e-mailberichten voor goedkeuringen en meldingen. Vóór SP1 MIM werd alleen Exchange Server of SMTP ondersteund. De MIM-service kan met Service Pack 1 aanvragen en e-mailmeldingen verzenden en ontvangen met een Office365 Exchange online-account.

- **Validatie van bestandsindeling van afbeeldingen:** MIM kan de bestandsindeling van afbeeldingen controleren wanneer deze naar de portal worden geüpload.

### Privileged Access Management (PAM) gebruiken
<a id="privileged-access-managementpam" class="xliff"></a>

- **PAM PRIV (bastionhost) forestondersteuning voor het Windows Server 2016-functionaliteitsniveau:** de MIM PAM-service kan worden geconfigureerd in een omgeving met domeincontrollers die worden uitgevoerd op het functionele Active Directory Domain Services-forestniveau van Windows Server 2016. Als dit is geconfigureerd, wordt voor een Kerberos-ticket van een gebruiker een tijdslimiet ingesteld voor de resterende tijd van de activatie van de rol.

    >[!Note]
    Als u het forest-functionaliteitsniveau van Windows Server 2012 R2 in uw CORP-domein wilt behouden, is het raadzaam om [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) en [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) op de CORP-domeincontroller te installeren.

- **Bevoegde uitbreiding van accounts in groepen, exclusief voor het PRIV-forest (bastion):** administrators kunnen de MIM-service van groepen en gebruikers exclusief informeren in het PRIV-forest. Hierdoor kunt u deze groepen en gebruikers opnemen in PAM-rollen.  Vervolgens kunnen de gebruikers worden geactiveerd voor een rol en toegewezen lidmaatschap aan groepen in het PRIV-forest.

- **PAM-implementatiescripts:** met PAM-implementatiescripts kunnen administrators de installatie van de PAM-omgeving stroomlijnen.

- **PAM-cmdlets voor de configuratie van authenticatiebeleidssilo’s:** Service Pack 1 introduceert nieuwe cmdlets om de beveiliging van uw bastionforest op te schroeven. Deze cmdlets maken automatisch een authenticatiebeleidssilo die gebonden is aan een authenticatiebeleidssjabloon.

    >[!Note]
    Deze cmdlets worden automatisch uitgevoerd als onderdeel van de implementatiescripts.


## Platformondersteuning
<a id="platform-support" class="xliff"></a>
Informatie over bijgewerkte platformondersteuning vindt u in het document met de naam [Ondersteunde platformen voor MIM 2016](microsoft-identity-manager-2016-supported-platforms.md).  Nieuwe ondersteunde platformen in dit servicepack, waaronder SQL Server 2016 en SharePoint 2016

## In deze release zijn problemen met de algemene beschikbaarheid van MIM 2016 opgelost
<a id="issues-fixed-in-this-release-from-mim-2016-general-availability" class="xliff"></a>

### PAM
<a id="pam" class="xliff"></a>
- Nieuwe PAMGroup heeft geen MIM-objecten voor lokale domeingroepen gemaakt in het PRIV-forest
- Nieuwe PAMDomainConfiguration mislukt met een netdom-foutbericht
- PAM-controleservice heeft waarschuwingen voor groepen in het PRIV-forest vastgelegd

## Bijwerken naar Service Pack 1
<a id="how-to-upgrade-to-service-pack-1" class="xliff"></a>

Klanten die een upgrade naar Microsoft Identity Manager 2016 Service Pack 1 willen uitvoeren, moeten de onderstaande instructies volgen voor alle services die van toepassing zijn op hun implementatie.

>[!Note]
>Klanten met Forefront Identity Manager 2010 R2 SP1 of eerder moeten eerst hun omgeving bijwerken naar Microsoft Identity Manager 2016, uitgebracht in augustus 2015, en vervolgens de onderstaande stappen volgen.

Voordat u begint

U moet de MIM-synchronisatie-engine upgraden voordat u de MIM-service en -portal bijwerkt.
U moet een back-up maken van de MIMService- en MIM-Sync-databases.

  1. Het Microsoft Identity Manager-onderdeel dat u wilt bijwerken, verwijderen
  2. Nadat de verwijdering is voltooid, opent u de welkomstpagina FIMSplash.htm in uw installatiemedia
  3. Het MIM-onderdeel selecteren dat u wilt bijwerken
  4. Ga door met de installatie door de prompts te volgen
    * Installatie van de MIM-service en -portal: als u Exchange Online als het e-mail-account selecteert, voert u het e-mailadres en de referenties van het Exchange Online-account in het volgende scherm in.

