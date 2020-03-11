---
title: Stap 6 De PAM-vertrouwensrelatie installeren
description: Stap 6 van de procedure voor het configureren van PAM met behulp van scripts. Deze sectie bevat informatie over het instellen van de vereiste vertrouwensrelatie tussen het CORP- en het PRIV-domein
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: baab111f1b8f0f290611b9b63bb6981d8ae9538a
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043763"
---
# <a name="step-6-set-up-the-pam-trust"></a>Stap 6 De PAM-vertrouwensrelatie installeren

> [!div class="step-by-step"]
> [« Stap 5](sp1-step5-configuring-pam.md)
> [Stap 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

**Dit is niet vereist voor alleen een PRIV-omgeving** Meld u aan bij de PAM-server met het MIMAdmin-account.

1. Meld u aan bij de PAM-server met het MIMAdmin-account
2. Voer PowerShell uit als Administrator
3. cd $env: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecteer menuoptie 6 (PAM trust setup)

   Als u erom wordt gevraagd, geeft u de referenties voor het CORP-beheeraccount in. Na het opgeven van referenties wordt de vertrouwensrelatie bevestigd en is de configuratie voltooid.

> [!div class="step-by-step"]
> [« Stap 5](sp1-step5-configuring-pam.md)
> [Stap 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)
