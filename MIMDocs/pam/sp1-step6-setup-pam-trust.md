---
title: Stap 6 De PAM-vertrouwensrelatie installeren
description: Het CORP-domein voorbereiden met bestaande of nieuwe identiteiten die worden beheerd door Privileged Identity Manager via scripts
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 10/04/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bc56b57a06592527bab13aad879ca13466e968b3
ms.openlocfilehash: 52226bfa5742e39d834f80dac69317e10b6259c7


---

# Stap 6 De PAM-vertrouwensrelatie installeren

**Dit is niet vereist voor alleen een PRIV-omgeving** Meld u aan bij de PAM-server met het MIMAdmin-account.

1. Meld u aan bij de PAM-server met het MIMAdmin-account
2. Voer PowerShell uit als Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecteer menuoptie 6 (PAM trust setup)

  Als u erom wordt gevraagd, geeft u de referenties voor het CORP-beheeraccount in. Na het opgeven van referenties wordt de vertrouwensrelatie bevestigd en is de configuratie voltooid.

>[!div class="step-by-step"]
[« Stap 5](sp1-step5-configuring-pam.md)
[Stap 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)



<!--HONumber=Oct16_HO1-->


