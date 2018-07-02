---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: MIM omvat de mogelijkheden voor toegangsbeheer van FIM 2010 en zorgt ervoor dat u gebruikers, referenties, beleidsregels en toegang in uw organisatie kunt beheren.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/02/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: bd483ecb0abc3e4bb4444c87715971a3fba9820b
ms.sourcegitcommit: 5405ed10fea6f50b711eca1153446c04d4faff7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36927184"
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

Microsoft Identity Manager (MIM) 2016 bouwt voort op de mogelijkheden voor identiteits- en toegangsbeheer van [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Net als bij de voorganger kunt u met MIM de gebruikers, de referenties, het beleid en de toegang in uw organisatie beheren.  Daarnaast voegt MIM 2016 hieraan een hybride-ervaring, beheermogelijkheden voor bevoegde toegang en ondersteuning voor nieuwe platformen toe.

Naast de bestaande functionaliteit voor identiteitsbeheer opgenomen in [FIM](https://technet.microsoft.com/library/jj133868). MIM 2016 biedt nieuwe functies en verbeteringen, zoals:

- Beschermde identiteitsbeheer
- Nieuwe functionaliteit in Certificaatbeheer
  - [Naslaginformatie voor REST API van Certificate Management](./reference/certificate-management-rest-api-reference.md)
  - Ondersteuning voor topologieën met meerdere forests.
  - [Een Windows-app voor de virtuele smartcard](working-with-mim-certificate-manager.md)
  - Bijgewerkte gebeurtenissen en mogelijkheden voor probleemoplossing. 
- [Selfservicescenario's](working-with-self-service-password-reset.md) omvatten nu ontgrendelen van accounts en Azure MFA (Multi-factor authentication)-gate voor wachtwoord opnieuw instellen.

## <a name="hybrid-experience"></a>Hybride-ervaring

Microsoft Identity Manager 2016 werkt goed samen met [Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) geven u controle over de volledige omgeving. Met de hybride rapportage in Azure AD worden de cloud- en on-premises gegevens op één plek weergegeven. Ook de [wachtwoord opnieuw instellen in selfservice portal](working-with-self-service-password-reset.md) biedt ondersteuning voor Azure multi-factor authentication (MFA).

## <a name="privileged-identity-management"></a>Beschermde identiteitsbeheer

Met Privileged Identity Management regelt en beheert u de beheerderstoegang door middel van tijdelijke, taakgebaseerde toegang tot gevoelige resources. Dit betekent dat u aan gebruikers alleen de machtiging kunt toekennen die strikt noodzakelijk is. Hierdoor wordt de kans verkleind dat bij een cyberaanval de volledige beheerderstoegang wordt overgenomen. Daarnaast extraheert en isoleert u met Privileged Identity Management beheerdersaccounts uit bestaande Active Directory-forests.

MIM ondersteunt een on-premises Privileged Identity Management-oplossing voor het beheer van Active Directory. Aan de slag gaan met [Privileged Access Management gebruiken](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Verwante onderwerpen

- Microsoft Identity Manager is nog nauw verwant aan diens voorganger Forefront Identity Manager. Zie [FIM 2010 R2 Documentation Roadmap](https://technet.microsoft.com/library/jj133885.aspx) (Engelstalig) als u FIM nog gebruikt of aanvullende documentatie wilt raadplegen.
- [Aandachtspunten voor topologie voor het implementeren van MIM](topology-considerations.md) in dit artikel bevat meerdere implementatietopologieën die u implementeren overwegen kunt.
- [Handleiding voor capaciteitsplanning](capacity-planning-guide.md) kunt u deze handleiding en testomgevingen om te begrijpen van het juiste bereik voor uw implementatie.