---
title: Vereiste stappen voor implementatie van Microsoft Identity Manager 2016 | Microsoft Docs
description: Lees de beschrijving van alle stappen voor het implementeren van Microsoft Identity Manager 2016, van het voorbereiden van de omgeving tot het configureren van de portals.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/17/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: 8a0af59191d19f91f3358b5dc997938f0fe29959
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010537"
---
# <a name="deploy-microsoft-identity-manager-2016-sp2"></a>Microsoft Identity Manager 2016 SP2 implementeren
In de artikelen in deze sectie vindt u stapsgewijze instructies voor het implementeren van Microsoft Identity Manager (MIM) 2016 voor selfservicescenario's voor eindgebruikers op een nieuwe server waarop niet eerder FIM of MIM is geïmplementeerd.

> [!NOTE]
> De implementatietopologie die in deze sectie wordt beschreven, is alleen bedoeld voor de voorbereidende fase waarin u kennis kunt opdoen over MIM.  De [handleiding voor capaciteitsplanning](capacity-planning-guide.md) bevat meer informatie over topologieën voor productie-implementaties.  Het wordt aanbevolen om die documentatie door te nemen voordat u MIM implementeert en in gebruik neemt.

Het scenario voor Privileged Access Management wordt anders geïmplementeerd dan andere MIM-scenario's, aangezien hiervoor een speciale bastionomgeving met forest is vereist.  Zie [Configure the MIM Environment for privileged Access Management](./pam/configuring-mim-environment-for-pam.md)(Engelstalig) voor meer informatie over het implementeren van mim voor privileged Access Management.

Het proces voor het implementeren van MIM is vergelijkbaar met het proces voor de voorafgaande taak, FIM 2010 R2. Zie [Forefront Identity Manager 2010 R2 Deployment Guide](https://technet.microsoft.com/library/jj134310) (Engelstalig) als u de FIM-documentatie wilt raadplegen.

## <a name="first-prepare-a-domain"></a>Eerste stap: een domein voorbereiden
MIM werkt met Active Directory (AD). Volg daarom onderstaande stappen om de AD-domeincontroller te configureren.
- [Domein installatie](preparing-domain.md) of
- [Domein instelling voor door groep beheerde service accounts](preparing-domain-gmsa.md)


## <a name="next-prepare-identity-management-servers"></a>Volgende: identiteits beheerser vers voorbereiden
Als uw domein eenmaal is geïmplementeerd en geconfigureerd, bereidt u de server voor identiteitsbeheer van uw bedrijf voor.

Zie [ondersteunde platforms voor MIM 2016 of hoger](microsoft-identity-manager-2016-supported-platforms.md) voor meer informatie over ondersteunde platforms

 Het volgende moet hiervoor worden ingesteld:
- [Windows Server](prepare-server-ws2016.md)
- [SQL Server](prepare-server-sql2016.md)
- [SharePoint Server](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (optioneel)

## <a name="finally-install-microsoft-identity-manager-2016-sp2-components"></a>Ten slotte: Installeer Microsoft Identity Manager 2016 SP2-onderdelen
Nadat u het domein en de server hebt ingesteld, kunt u nu de MIM-onderdelen installeren en configureren zodat deze kunnen worden gesynchroniseerd met AD.
- [MIM-synchronisatieservice](install-mim-sync.md)
- [MIM-service en -portal](install-mim-service-portal.md)
- [Active Directory en de databases voor de MIM-service synchroniseren](install-mim-sync-ad-service.md)
