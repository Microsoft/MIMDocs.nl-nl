---
title: Stap 3 SQL configureren
description: Dit artikel is stap 3 van de reeks artikelen die betrekking hebben op het configureren van Microsoft Identity Manager met behulp van scripts en de beschrijving van de SQL Server-configuratie stappen.
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
ms.openlocfilehash: e4317363084da53e5683cd1a9b3d7bd0ce3ebcda
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010520"
---
# <a name="step-3-configuring-sql"></a>Stap 3 SQL configureren

> [!div class="step-by-step"]
> [«Stap 2](sp1-step2-configuring-corp-domain.md) 
>  [Stap 4»](sp1-step4-configuring-sharepoint.md)

Voordat u verdergaat met de stappen hieronder, zorgt u ervoor dat u SQL Server 2012 SP1 of hoger of SQL server 2014 gebruikt. Meld u bij servers die lid zijn van het domein aan met het MIMAdmin-account, anders aanmelden als lokale beheerder.
1. Voer PowerShell uit als Administrator
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selecteer menuoptie 3 (SQL Server Setup)

   Als de server nog niet gekoppeld is aan het PRIV-domein, wordt u gevraagd om uw referenties op te geven en verbinding te maken met de server op het domein.
   Nadat u dit hebt gedaan, wordt de computer opnieuw opgestart. Wanneer het systeem opnieuw is opgestart, meldt u zich aan bij de server met het MIMAdmin-account.

5. Voer PowerShell uit als Administrator
6. cd $env:SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. Selecteer menuoptie 3 (SQL Server Setup)

Wanneer u erom wordt gevraagd, voert u het wachtwoord voor de MIMAdmin-serviceaccount in en laat u de installatie doorgaan. Zodra dit is voltooid, gaat u door met stap 4.

> [!div class="step-by-step"]
> [«Stap 2](sp1-step2-configuring-corp-domain.md) 
>  [Stap 4»](sp1-step4-configuring-sharepoint.md)
