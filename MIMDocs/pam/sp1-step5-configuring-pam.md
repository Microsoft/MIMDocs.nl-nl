---
title: Stap 5 PAM installeren/configureren
description: Het CORP-domein voorbereiden met bestaande of nieuwe identiteiten die worden beheerd door Privileged Identity Manager via scripts
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: c641865548f753a609ccee8dbf12c329bb6a1c9f


---
# <a name="step-5-installingconfiguring-pam"></a>Stap 5 PAM installeren/configureren

>[!div class="step-by-step"]
[« Stap 4](sp1-step4-configuring-sharepoint.md)
[Stap 6 »](sp1-step6-setup-pam-trust.md)

Voor een PAM-server die lid is van een domein meldt u zich aan als MIMAdmin. Anders meldt u zich aan als een lokale administrator.
1. Voer PowerShell uit als Administrator
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. Selecteer menuoptie 5 (MIM PAM Setup)

>[!NOTE]
>Als de computer nog geen lid van een PRIV-domein is, wordt u gevraagd referenties in te voeren. Nadat u dit hebt gedaan, wordt de computer opnieuw opgestart.

Nadat de PAMServer opnieuw is opgestart, meldt u zich weer aan bij de computer met het MIMAdmin-account.

1. Voer PowerShell uit als Administrator
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selecteer menuoptie 5 (MIM PAM Setup)

  Als u erom wordt gevraagd, voert u het wachtwoord van het MIM-controleaccount, MIM-onderdeelaccount, MIM MA-account, MIM-serviceaccount, MIM-beheeraccount en het SharePoint-account in.
  Nadat de installatie is voltooid, wordt de machine wordt opnieuw opgestart.

>[!div class="step-by-step"]
[« Stap 4](sp1-step4-configuring-sharepoint.md)
[Stap 6 »](sp1-step6-setup-pam-trust.md)



<!--HONumber=Nov16_HO2-->


