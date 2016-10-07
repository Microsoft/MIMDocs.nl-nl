---
title: Stap 3 SQL configureren
description: Het CORP-domein voorbereiden met bestaande of nieuwe identiteiten die worden beheerd door Privileged Identity Manager via scripts
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: a7d456b1c2baf31ef2d7ca801a567cf42eaef52e


---
# Stap 3 SQL configureren

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



<!--HONumber=Sep16_HO4-->


