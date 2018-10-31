---
title: Overzicht van de PAM-omgeving | Microsoft Docs
description: Zoeken naar het vereiste aantal en de configuratie van virtuele machines die in Privileged Access Management kunnen worden geïmplementeerd
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6f4b6e224b6b50bf2190688a994f35159d273713
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379495"
---
# <a name="environment-overview"></a>Overzicht van de omgeving

Privileged Access Management (PAM) werkt met virtuele machines (VM's) met aparte stations die via een gedeeld netwerk met elkaar zijn verbonden. Deze virtuele machines kunnen worden gehost met Windows 8.1, Windows Server 2012 R2 of andere besturingssystemen.

![PAM-servers: relaties en ondersteunde platformen - diagram](media/pam-test-lab-architecture.png)

U moet minimaal drie virtuele machines.  Als u nog een AD-domein beheren met PAM hebt, moet u een extra virtuele machine om te fungeren als een CORP-domeincontroller.  Als u de PRIV-software voor hoge beschikbaarheid configureren wilt, moet u twee extra virtuele machines.

De stations waar de VM-schijf-installatiekopieën worden opgeslagen moeten ten minste 120 GB aan vrije schijfruimte.  Als u wilt implementeren voor maximale beschikbaarheid, moet u ervoor zorgen dat het schijfsubsysteem voldoet aan de vereisten voor gedeelde opslag via SQL.  De gedeelde opslag kan bestaan uit Windows Server Failover Clustering-clusterschijven, schijven op een Storage Area Network (SAN) of bestandsshares op een SMB-server.

> [!IMPORTANT]
> Opslag moet worden toegewezen aan de bastionomgeving. Opslag delen met andere werkbelastingen buiten de bastionomgeving wordt niet aanbevolen omdat dit de integriteit van de bastionomgeving in gevaar kan brengen.

## <a name="next-steps"></a>Volgende stappen

- [Privileged Access Management voor Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) volgt een overzicht van PAM en hoe het werkt.
- [Inzicht in de PAM-onderdelen](principles-of-operation.md) volgt een overzicht van de verschillende onderdelen van PAM.
