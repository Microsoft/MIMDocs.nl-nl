---
title: Overzicht van de PAM-omgeving | Microsoft Docs
description: Zoeken naar het vereiste aantal en de configuratie van virtuele machines die in Privileged Access Management kunnen worden geïmplementeerd
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: fb38ee26d829acd5c54bf690f7a80f0659baaa85
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104068"
---
# <a name="mim-pam-test-lab-environment-overview"></a>Overzicht van de test omgeving voor MIM PAM

> [!NOTE]
> De PAM-benadering die door MIM wordt verstrekt, is bedoeld om te worden gebruikt in een aangepaste architectuur voor geïsoleerde omgevingen waar Internet toegang niet beschikbaar is, waarbij deze configuratie vereist is voor de regelgeving, of in zeer belang rijke, geïsoleerde omgevingen zoals offline onderzoek laboratoria en niet-verbonden operationele technologie of omgevingen voor gegevens verzameling. Als uw Active Directory deel uitmaakt van een omgeving met Internet verbinding, raadpleegt u in plaats daarvan [privileged Access beveiligen](/security/compass/overview) voor meer informatie over waar u moet beginnen.

Als u een testlab van MIM PAM wilt instellen, kunt u de software installeren op virtuele machines.
Privileged Access Management (PAM) werkt met virtuele machines (VM's) met aparte stations die via een gedeeld netwerk met elkaar zijn verbonden. Deze virtuele machines kunnen worden gehost met Windows 8.1, Windows Server 2012 R2 of andere besturingssystemen.

![PAM-servers: relaties en ondersteunde platformen - diagram](media/pam-test-lab-architecture.png)

U hebt mini maal drie virtuele machines nodig.  Als u nog geen AD-domein voor PAM wilt beheren, hebt u een extra virtuele machine nodig om te fungeren als een CORP-domein controller.  Als u de PRIV-software wilt configureren voor maximale Beschik baarheid, hebt u twee extra virtuele machines nodig.

De stations waarop de VM-schijf installatie kopieën worden opgeslagen, moeten ten minste 120 GB vrije schijf ruimte hebben.  Als u wilt implementeren voor maximale beschikbaarheid, moet u ervoor zorgen dat het schijfsubsysteem voldoet aan de vereisten voor gedeelde opslag via SQL.  De gedeelde opslag kan bestaan uit Windows Server Failover Clustering-clusterschijven, schijven op een Storage Area Network (SAN) of bestandsshares op een SMB-server.

> [!IMPORTANT]
> Opslag moet worden toegewezen aan de Bastion-omgeving. Het delen van opslag met andere workloads buiten de Bastion-omgeving wordt niet aanbevolen, omdat het de integriteit van de Bastion-omgeving zou kunnen afleiden.

## <a name="next-steps"></a>Volgende stappen

- [Privileged Access Management voor Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) is een overzicht van Pam en hoe het werkt.
- Meer [informatie over de onderdelen van Pam](principles-of-operation.md) is een overzicht van de verschillende onderdelen van pam.
