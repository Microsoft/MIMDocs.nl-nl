---
title: MIM2016 implementeren | Microsoft Docs
description: Lees de beschrijving van alle stappen voor het implementeren van Microsoft Identity Manager 2016, van het voorbereiden van de omgeving tot het configureren van de portals.
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 09/07/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 7fdad09650af245157264778d2732ac4327f25b5


---

# <a name="deploy-mim-2016"></a>MIM 2016 implementeren
In de artikelen in deze sectie vindt u stapsgewijze instructies voor het implementeren van Microsoft Identity Manager (MIM) 2016 voor selfservicescenario's voor eindgebruikers op een nieuwe server waarop niet eerder FIM of MIM is geïmplementeerd.

> [!NOTE]
> De implementatietopologie die in deze sectie wordt beschreven, is alleen bedoeld voor de voorbereidende fase waarin u kennis kunt opdoen over MIM.  De [handleiding voor capaciteitsplanning](/microsoft-identity-manager/plan-design/capacity-planning-guide) bevat meer informatie over topologieën voor productie-implementaties.  Het wordt aanbevolen om die documentatie door te nemen voordat u MIM implementeert en in gebruik neemt.

Het scenario voor Privileged Access Management wordt anders geïmplementeerd dan andere MIM-scenario's, aangezien hiervoor een speciale bastionomgeving met forest is vereist.  Zie [De MIM-omgeving voor Privileged Access Management configureren](/microsoft-identity-manager/pam/configuring-mim-environment-for-pam) voor meer informatie over het implementeren van MIM voor Privileged Identity Management.

De procedure voor het implementeren van MIM 2016 lijkt sterk op die van diens voorganger FIM 2010 R2. Zie [Forefront Identity Manager 2010 R2 Deployment Guide](https://technet.microsoft.com/library/jj134310) (Engelstalig) als u de FIM-documentatie wilt raadplegen.

## <a name="first-prepare-a-domain"></a>Eerste stap: een domein voorbereiden
MIM werkt met Active Directory (AD). Volg daarom onderstaande stappen om de AD-domeincontroller te configureren.
- [Domeininstelling](preparing-domain.md)

## <a name="next-prepare-an-identity-management-server"></a>Volgende stap: een server voor identiteitsbeheer voorbereiden
Als uw domein eenmaal is geïmplementeerd en geconfigureerd, bereidt u de server voor identiteitsbeheer van uw bedrijf voor. Het volgende moet hiervoor worden ingesteld:
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (optioneel)

## <a name="finally-install-microsoft-identity-manager-2016-components"></a>Ten slotte: de Microsoft Identity Manager 2016-onderdelen installeren
Nadat u het domein en de server hebt ingesteld, kunt u nu de MIM-onderdelen installeren en configureren zodat deze kunnen worden gesynchroniseerd met AD.
- [MIM-synchronisatieservice](install-mim-sync.md)
- [MIM-service en -portal](install-mim-service-portal.md)
- [Active Directory en de databases voor de MIM-service synchroniseren](install-mim-sync-ad-service.md)



<!--HONumber=Nov16_HO2-->


