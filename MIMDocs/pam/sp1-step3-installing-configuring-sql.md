---
title: Stap 3 SQL configureren
description: Dit artikel is stap 3 van de serie artikelen over het configureren van Privileged Identity Manager met behulp van scripts. In het artikel worden de configuratiestappen voor de SQL-server besproken.
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 93ae9f198d73d21ae966fe3c3b22e47435bd5608
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# Stap 3 SQL configureren
<a id="step-3-configuring-sql" class="xliff"></a>

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
