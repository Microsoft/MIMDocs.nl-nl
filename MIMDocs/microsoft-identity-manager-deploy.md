---
title: Vereiste stappen voor implementatie van Microsoft Identity Manager 2016 | Microsoft Docs
description: Lees de beschrijving van alle stappen voor het implementeren van Microsoft Identity Manager 2016, van het voorbereiden van de omgeving tot het configureren van de portals.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a1f2a30dd6d8519ec09ea3765e5584123725fe03
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/27/2018
---
# <a name="deploy-microsoft-identity-manager-2016-sp1"></a>Implementatie van Microsoft Identity Manager 2016 SP1
In de artikelen in deze sectie vindt u stapsgewijze instructies voor het implementeren van Microsoft Identity Manager (MIM) 2016 voor selfservicescenario's voor eindgebruikers op een nieuwe server waarop niet eerder FIM of MIM is geïmplementeerd.

> [!NOTE]
> De implementatietopologie die in deze sectie wordt beschreven, is alleen bedoeld voor de voorbereidende fase waarin u kennis kunt opdoen over MIM.  De [handleiding voor capaciteitsplanning](capacity-planning-guide.md) bevat meer informatie over topologieën voor productie-implementaties.  Het wordt aanbevolen om die documentatie door te nemen voordat u MIM implementeert en in gebruik neemt.

Het scenario voor Privileged Access Management wordt anders geïmplementeerd dan andere MIM-scenario's, aangezien hiervoor een speciale bastionomgeving met forest is vereist.  Zie [De MIM-omgeving voor Privileged Access Management configureren](./pam/configuring-mim-environment-for-pam.md) voor meer informatie over het implementeren van MIM voor Privileged Identity Management.

Het proces voor het implementeren van MIM is vergelijkbaar met het proces voor de voorganger FIM 2010 R2. Zie [Forefront Identity Manager 2010 R2 Deployment Guide](https://technet.microsoft.com/library/jj134310) (Engelstalig) als u de FIM-documentatie wilt raadplegen.

## <a name="first-prepare-a-domain"></a>Eerste stap: een domein voorbereiden
MIM werkt met Active Directory (AD). Volg daarom onderstaande stappen om de AD-domeincontroller te configureren.
- [Domeininstelling](preparing-domain.md)

## <a name="next-prepare-an-identity-management-servers"></a>Volgende stap: Een identity management-servers voorbereiden
Als uw domein eenmaal is geïmplementeerd en geconfigureerd, bereidt u de server voor identiteitsbeheer van uw bedrijf voor. Het volgende moet hiervoor worden ingesteld:
- [Windows Server 2012 R2](prepare-server-ws2016.md)
- [SQL Server 2016](prepare-server-sql2016.md)
- [SharePoint 2016](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (optioneel)

## <a name="finally-install-microsoft-identity-manager-2016-sp1-components"></a>Ten slotte: Microsoft Identity Manager 2016 SP1 onderdelen installeren
Nadat u het domein en de server hebt ingesteld, kunt u nu de MIM-onderdelen installeren en configureren zodat deze kunnen worden gesynchroniseerd met AD.
- [MIM-synchronisatieservice](install-mim-sync.md)
- [MIM-service en -portal](install-mim-service-portal.md)
- [Active Directory en de databases voor de MIM-service synchroniseren](install-mim-sync-ad-service.md)
- [Ondersteunde platforms voor MIM 2016 of hoger](microsoft-identity-manager-2016-supported-platforms.md)
