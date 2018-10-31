---
title: Stap 8 Verificatie van PAM-implementatie
description: De procedure voor implementatie van PAM met behulp van scripts omvat verificatiescripts waarmee een PAM-scenario kan worden uitgevoerd om te controleren of de PAM-implementatie naar behoren werkt.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: d6b0327b39a76799b2943565dd0c3e00f55f745f
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379953"
---
# <a name="step-8-pam-deployment-verification"></a>Stap 8 Verificatie van PAM-implementatie

> [!div class="step-by-step"]
> [« Stap 7](sp1-step7-setup-sidhistory-sidfiltering.md)
> [Bijlage »](sp1-pam-deployment-addendum.md)

Het implementatiepakket wordt geleverd met verificatiescripts die een PAM-scenario kunnen uitvoeren om te controleren of de PAM-implementatie naar behoren werkt.
Om deze controlestap te kunnen gebruiken, wijzigt u in PAMDeploymentConfig.xml de sectie <PamValidation/>.

>[!NOTE]
>Voor de validatie is een clientcomputer vereist die lid is van het CORP-domein en waarop de PAM-onderdelen aan de clientzijde zijn geïnstalleerd. Zie de bijlage voor scripts voor het installeren van een client.

De naam van de clientcomputer moet worden bijgewerkt in de <PAMValidationClient/>-code van PAMDeploymentConfig.xml De rest van de gegevens in het <PAMValidation/>-knooppunt moet alleen worden bewerkt als deze strijdig is met bestaande gebruikers/groepen, omdat deze tijdens de validatie worden gemaakt.
Voer een validatie uit aan de hand van de volgende stappen:

Stap 1:

1. Meld u aan bij de CORPDC als een CORP-domeinadministrator
2. Voer PowerShell uit als Administrator
3. cd $env: SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

Hiermee maakt u de vereiste groepen en gebruikers voor de validatie

Stap 2:

1. Meld u aan bij de PAM-Server als MIMAdmin
2. Voer PowerShell uit als Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

Met deze stap worden de gebruikers en groepen gemigreerd naar de PAM-omgeving

Stap 3:

1. Meld u aan bij de client CORP als lokale administrator
2. Voer PowerShell uit als Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


In deze stap moet u uw CORPAdmin-referenties opgeven. Zodra u deze hebt opgegeven, worden de juiste gebruikers toegevoegd aan de groepen Extern bureaubladgebruikers en Extern beheer-gebruikers.
Gebruik op de CORP-client de volgende opdracht om PowerShell als de PRIV-gebruiker te openen die u valideert. </br></br>
**Uitvoeren als u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
Typ in het PowerShell-venster het volgende:

1. cd $env: SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  Hier ziet de status van de aanvraag.
  In eerste instantie heeft de gebruiker geen toegang tot de resource. Nadat de gebruiker Just-In-Time is toegevoegd aan de rol, heeft de gebruiker wel toegang. Zodra de duur van de aanvraag is verlopen, heeft de gebruiker opnieuw geen toegang.
  Voor het script geldt de standaardduur (11 minuten) voordat de aanvraag verloopt.

> [!div class="step-by-step"]
> [« Stap 7](sp1-step7-setup-sidhistory-sidfiltering.md)
> [Bijlage »](sp1-pam-deployment-addendum.md)
