---
title: Stap 3 SQL configureren
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
ms.openlocfilehash: 375a34e5255c90559fc0ffb3a80fc7c92ebd27a2


---
# <a name="step-3-configuring-sql"></a>Stap 3 SQL configureren

>[!div class="step-by-step"]
[« Stap 2](sp1-step2-configuring-corp-domain.md)
[Stap 4 »](sp1-step4-configuring-sharepoint.md)

Voordat u verdergaat met de stappen hieronder, zorgt u ervoor dat u SQL Server 2012 SP1 of hoger of SQL server 2014 gebruikt. Voor gekoppelde domeinservers meldt u zich aan met de MIMAdmin-account of meldt u zich aan als een lokale administrator
1. Voer PowerShell uit als Administrator
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selecteer menuoptie 3 (SQL Server Setup)

  Als de server nog niet gekoppeld is aan het PRIV-domein, wordt u gevraagd om uw referenties op te geven en verbinding te maken met de server op het domein.
  Nadat u dit hebt gedaan, wordt de computer opnieuw opgestart. Nadat de computer opnieuw is opgestart, meldt u zich aan bij de server met het MIMAdmin-account.

1. Voer PowerShell uit als Administrator
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selecteer menuoptie 3 (SQL Server Setup)

Wanneer u erom wordt gevraagd, voert u het wachtwoord voor de MIMAdmin-serviceaccount in en laat u de installatie doorgaan. Zodra dit is voltooid, gaat u door met stap 4.

>[!div class="step-by-step"]
[« Stap 2](sp1-step2-configuring-corp-domain.md)
[Stap 4 »](sp1-step4-configuring-sharepoint.md)



<!--HONumber=Nov16_HO2-->


