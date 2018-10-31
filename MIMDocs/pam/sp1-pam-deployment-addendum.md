---
title: Bijlage
description: Dit is het addendum voor de documenten die betrekking hebben op de implementatie van PAM met behulp van scripts. In het artikel wordt uitgelegd hoe u het PRIV- en CORP-domein kunt configureren en een client kunt instellen voor het uitvoeren van validatie. Tevens bevat het artikel informatie over hoe u hulp kunt aanvragen.
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
ms.openlocfilehash: 814386272bba0c3fa2cb73bc93dc842ac46e86fb
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379461"
---
# <a name="pam-deployment-scripts-addendum"></a>Bijlage PAM-implementatiescripts:

## <a name="addendum-1-setting-up-the-priv-domain"></a>Bijlage 1 instellen van het PRIV-domein

Nadat het gecomprimeerde bestand in de map $env:SYSTEMDRIVE\PAM is uitgepakt, bewerkt u het bestand MDeploymentConfig.xm, zodat details over het PRIV-forest worden weergegeven. Werk de DNS-naam, de NetBIOS-naam, de DC-naam en het Database/logboekpad & Sysvol-pad bij. Ook het domein & ForestMode bijwerken. Als u Windows Server Technical Preview 5 test, stel de DomainMode & ForestMode in op WinThreshold.

1. Meld u aan bij het PRIV-domein als administrator
2. Voer PowerShell uit als Administrator
3. cd $env: SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Selecteer menuoptie 9 (Priv Forest setup)


De domeincontroller wordt automatisch opnieuw opgestart na de voltooiing. Het administratorwachtwoord van Directory Services Restore Mode (DSRM) moet voldoen aan de volgende criteria:

  * Lengte van het wachtwoord is een minimum van 15 tekens
  * Het wachtwoord bevat ten minste één kleine letter
  * Het wachtwoord bevat ten minste één HOOFDLETTER
  * Het wachtwoord bevat ten minste één cijfer of speciale letter

## <a name="addendum-2-setting-up-the-corp-domain"></a>Bijlage 2 Het CORP-domein instellen

Als u net aan de slag gaat met PAM en een testomgeving wilt opstarten, kunt u met dit script ook de configuratie van een CORP-domein uitvoeren. Nadat het gecomprimeerde bestand in de map $env:SYSTEMDRIVE\PAM is uitgepakt, bewerkt u het bestand AMDeploymentConfig.xm, zodat details over het PRIV-forest worden weergegeven. Werk de DNS-naam, NetBIOS-naam, DC-naam/pad naar logboek en pad voor Sysvol bij. Het functionele niveau moet minstens Windows Server 2012 R2 zijn.

1. Meld u aan bij het CORP-domein als administrator
2. Voer PowerShell uit als Administrator
3. cd $env: SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Selecteer menuoptie 10 (CORP forest setup)

De domeincontroller wordt automatisch opnieuw opgestart nadat deze is voltooid

## <a name="addendum-3-setting-up-a-corp-client-to-do-the-validation"></a>Bijlage 3 Een CORP-client instellen voor de validatie

De ClientBinaryLocation in het configuratiebestand moet verwijzen naar de locatie waarin setup.exe zich bevindt.
Meld u aan bij de client als een lokale administrator en voer de volgende opdrachten uit in PowerShell-venster met verhoogde bevoegdheid:

1. cd $env: SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Selecteer menuoptie 7 (MIM PAM Client Setup)


Als de machine niet verbonden met het verbonden domein, worden de referentie van het CORP-domein opgevraagd om de verbinding met het domein uit te voeren. De computer moet opnieuw worden opgestart na het verbinden met het domein. Meld u aan bij de client als lokale administrator en de volgende opdrachten uit in een PowerShell-venster met verhoogde bevoegdheid:

1. cd $env: SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Selecteer menuoptie 7 (MIM PAM Client Setup)

Ga verder met stap 8 hierboven.

## <a name="addendum-4-if-something-goes-wrong"></a>Bijlage 4 als er iets verkeerd gaat

Alle scriptlogboeken worden opgeslagen in %AppData%\MIMPAMInstall. Comprimeer de map in een zipbestand en verstuur het via e-mail naar [mim2016@microsoft.com](mailto:mim2016@microsoft.com), samen met details van de uitvoering en de fout.
