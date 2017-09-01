---
title: Overzicht van de PAM-omgeving | Microsoft Docs
description: "Zoeken naar het vereiste aantal en de configuratie van virtuele machines die in Privileged Access Management kunnen worden geïmplementeerd"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3be2e19673a863098739e830d9c83ce264abf412
ms.sourcegitcommit: 210195369d2ecd610569d57d0f519d683ea6a13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/01/2017
---
# <a name="environment-overview"></a>Overzicht van de omgeving

Privileged Access Management (PAM) werkt met virtuele machines (VM's) met aparte stations die via een gedeeld netwerk met elkaar zijn verbonden. Deze virtuele machines kunnen worden gehost met Windows 8.1, Windows Server 2012 R2 of andere besturingssystemen.

![PAM-servers: relaties en ondersteunde platformen - diagram](media/pam-test-lab-architecture.png)

U moet minimaal drie virtuele machines.  Als u nog geen AD-domein beheren met PAM hebt, moet u een extra virtuele machine om te fungeren als een CORP-domeincontroller.  Als u de PRIV-software voor hoge beschikbaarheid configureren wilt, moet u twee extra virtuele machines.

De stations waar de installatiekopieën van het VM-schijf worden opgeslagen moeten ten minste 120 GB aan vrije schijfruimte.  Als u wilt implementeren voor maximale beschikbaarheid, moet u ervoor zorgen dat het schijfsubsysteem voldoet aan de vereisten voor gedeelde opslag via SQL.  De gedeelde opslag kan bestaan uit Windows Server Failover Clustering-clusterschijven, schijven op een Storage Area Network (SAN) of bestandsshares op een SMB-server.

>[!IMPORTANT]
Opslag moet worden toegewezen aan de bastionomgeving. Opslag delen met andere werkbelastingen buiten de bastionomgeving wordt niet aanbevolen omdat dit de integriteit van de bastionomgeving in gevaar brengen.

## <a name="next-steps"></a>Volgende stappen

- [Privileged Access Management voor Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) is een overzicht van PAM en hoe het werkt.
- [Inzicht in de PAM-onderdelen](principles-of-operation.md) is een overzicht van de verschillende onderdelen van PAM.