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
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518537"
---
# <a name="environment-overview"></a>Overzicht van de omgeving

Privileged Access Management (PAM) werkt met virtuele machines (VM's) met aparte stations die via een gedeeld netwerk met elkaar zijn verbonden. Deze virtuele machines kunnen worden gehost met Windows 8.1, Windows Server 2012 R2 of andere besturingssystemen.

![PAM-servers: relaties en ondersteunde platformen - diagram](media/pam-test-lab-architecture.png)

U hebt mini maal drie virtuele machines nodig.  Als u nog geen AD-domein voor PAM wilt beheren, hebt u een extra virtuele machine nodig om te fungeren als een CORP-domein controller.  Als u de PRIV-software wilt configureren voor maximale Beschik baarheid, hebt u twee extra virtuele machines nodig.

De stations waarop de VM-schijf installatie kopieën worden opgeslagen, moeten ten minste 120 GB vrije schijf ruimte hebben.  Als u wilt implementeren voor maximale beschikbaarheid, moet u ervoor zorgen dat het schijfsubsysteem voldoet aan de vereisten voor gedeelde opslag via SQL.  De gedeelde opslag kan bestaan uit Windows Server Failover Clustering-clusterschijven, schijven op een Storage Area Network (SAN) of bestandsshares op een SMB-server.

> [!IMPORTANT]
> Opslag moet worden toegewezen aan de Bastion-omgeving. Het delen van opslag met andere workloads buiten de Bastion-omgeving wordt niet aanbevolen, omdat het de integriteit van de Bastion-omgeving zou kunnen afleiden.

## <a name="next-steps"></a>Volgende stappen

- [Privileged Access Management voor Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) is een overzicht van Pam en hoe het werkt.
- Meer [informatie over de onderdelen van Pam](principles-of-operation.md) is een overzicht van de verschillende onderdelen van pam.
