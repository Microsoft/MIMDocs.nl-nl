---
title: Overzicht van de PAM-omgeving | Microsoft Docs
description: "Zoeken naar het vereiste aantal en de configuratie van virtuele machines die in Privileged Access Management kunnen worden geïmplementeerd"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3e6c5a70c6b9ed140a56135676bbd14a84504317
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="environment-overview"></a>Overzicht van de omgeving

Privileged Access Management (PAM) werkt met virtuele machines (VM's) met aparte stations die via een gedeeld netwerk met elkaar zijn verbonden. Deze virtuele machines kunnen worden gehost met Windows 8.1, Windows Server 2012 R2 of andere besturingssystemen.

![PAM-servers: relaties en ondersteunde platformen - diagram](media/pam-test-lab-architecture.png)

U hebt minimaal drie virtuele machines nodig.  Als u nog geen AD-domein hebt dat wordt beheerd met PAM, hebt u een extra virtuele machine nodig om te fungeren als een CORP-domeincontroller.  Als u de PRIV-software wilt configureren voor maximale beschikbaarheid, hebt u ook twee extra virtuele machines nodig.

De stations waar de schijfinstallatiekopieën van de virtuele machines worden opgeslagen moeten over ten minste 120 GB aan vrijeschijfruimte beschikken om alle virtuele machines te kunnen opslaan.  Als u wilt implementeren voor maximale beschikbaarheid, moet u ervoor zorgen dat het schijfsubsysteem voldoet aan de vereisten voor gedeelde opslag via SQL.  De gedeelde opslag kan bestaan uit Windows Server Failover Clustering-clusterschijven, schijven op een Storage Area Network (SAN) of bestandsshares op een SMB-server. Houd er rekening mee dat deze moeten worden toegewezen aan de bastionomgeving; opslag delen met andere werkbelastingen buiten de bastionomgeving wordt niet aanbevolen omdat dit de integriteit van de bastionomgeving in gevaar kan brengen.

> [!NOTE]
> De huidige MIM Customer Technical Preview (CTP) is niet compatibel met de database of de mapinhoud van de vorige CTP. Als u eerder MIM voor PAM of andere scenario's hebt geëvalueerd, maakt u een back-up en archief van de virtuele machines die zijn gebruikt voor die test en start u de implementatie met de nieuwe installatiekopieën van virtuele machines die nog niet eerder zijn gebruikt voor MIM-scenario's.
