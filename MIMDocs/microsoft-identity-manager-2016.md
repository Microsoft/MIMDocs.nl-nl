---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: MIM omvat de mogelijkheden voor toegangsbeheer van FIM 2010 en zorgt ervoor dat u gebruikers, referenties, beleidsregels en toegang in uw organisatie kunt beheren.
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b3cdc1a71b6e9eb14a132429ea66bb4ab33fe3c4
ms.sourcegitcommit: 0a78e39976cd03225a8e24a508e9ee23585e67cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2017
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016
Microsoft Identity Manager (MIM) 2016 bouwt voort op de mogelijkheden voor identiteits- en toegangsbeheer van [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Net als bij de voorganger kunt u met MIM de gebruikers, de referenties, het beleid en de toegang in uw organisatie beheren.  Daarnaast voegt MIM 2016 hieraan een hybride-ervaring, beheermogelijkheden voor bevoegde toegang en ondersteuning voor nieuwe platformen toe.

Deze versie van Microsoft Identity Manager beschikt over nieuwe functies zoals Privileged Identity Manager en biedt ondersteuning in Certificate Management voor REST-API-toegang. Certificate Management is nu toegevoegde ondersteuning voor topologieën met meerdere forests, een Windows-app voor virtuele smartcard en de levensduur van certificaten beheren, gebeurtenissen en mogelijkheden voor probleemoplossing bijgewerkte. Selfservicescenario's omvatten nu een functie voor het ontgrendelen van accounts en een poort voor meervoudige verificatie (Azure MFA) voor het opnieuw instellen van het wachtwoord.

## <a name="hybrid-experience"></a>Hybride-ervaring
Microsoft Identity Manager 2016 werkt samen met Azure AD zodat u de volledige omgeving kunt beheren. Met de hybride rapportage in Azure AD worden de cloud- en on-premises gegevens op één plek weergegeven. De portal voor het opnieuw instellen van het wachtwoord van Selfservice ondersteunt ook de meervoudige verificatie van Azure (MFA of multi-factor authentication).

## <a name="privileged-identity-management"></a>Beschermde identiteitsbeheer
Met Privileged Identity Management regelt en beheert u de beheerderstoegang door middel van tijdelijke, taakgebaseerde toegang tot gevoelige resources. Dit betekent dat u aan gebruikers alleen de machtiging kunt toekennen die strikt noodzakelijk is. Hierdoor wordt de kans verkleind dat bij een cyberaanval de volledige beheerderstoegang wordt overgenomen. Daarnaast extraheert en isoleert u met Privileged Identity Management beheerdersaccounts uit bestaande Active Directory-forests.

MIM ondersteunt een on-premises Privileged Identity Management-oplossing voor het beheer van Active Directory. Aan de slag gaan met [Privileged Access Management gebruiken](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Verwante onderwerpen

- Microsoft Identity Manager is nog nauw verwant aan diens voorganger Forefront Identity Manager. Zie [FIM 2010 R2 Documentation Roadmap](https://technet.microsoft.com/library/jj133885.aspx) (Engelstalig) als u FIM nog gebruikt of aanvullende documentatie wilt raadplegen.
- [Aandachtspunten voor topologie voor een MIM-implementatie](topology-considerations.md)
- [Handleiding voor capaciteitsplanning](capacity-planning-guide.md)