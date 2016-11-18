---
title: PAM configureren met behulp van scripts
description: Het CORP-domein voorbereiden met bestaande of nieuwe identiteiten die worden beheerd door Privileged Identity Manager via scripts
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/04/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: 3aca2fb513280f118e760bdbc2ba471151c41b17


---

# <a name="configure-pam-using-scripts"></a>PAM configureren met behulp van scripts

Als u ervoor kiest SQL en SharePoint op afzonderlijke servers te installeren, moeten deze worden geconfigureerd volgens onderstaande instructies. Als SQL, SharePoint en de PAM-onderdelen zijn geïnstalleerd op dezelfde computer, moeten de onderstaande stappen worden uitgevoerd vanaf deze computer.

In de volgende stappen wordt ervan uitgegaan dat er al een PRIV-domein is ingesteld voor de instructies om een PRIV-domein te configureren en de bijlage aan het einde van het document te bekijken.

stappen:

1. Pak het gecomprimeerde bestand PAMDeploymentScripts.zip uit naar de map %SYSTEMDRIVE%\PAM op alle computers.
2. Open op een van de machines het bestand **PAMDeploymentConfig.xml** en werk de details bij met behulp van de onderstaande grafiek of aan de hand van de richtlijnen in het XML-bestand. Als de CORP- en PRIV-forests al zijn ingesteld, hoeft u alleen de **DNS-naam** en de **NetBIOS-naam** bij te werken.
3. In de sectie Rollen werkt u het **serviceaccount**, **de details van de computer** en de **locatie van de binaire installatiebestanden** voor SQL-, SharePoint- en MIM-rollen bij.
    1. De locatie van het binaire bestand voor MIM moet verwijzen naar de map met de map Service and Portal. De locatie van het binaire bestand voor de client moet verwijzen naar de map met Add-ins and Extensions.msi.

4. Als dit een PRIVOnly-omgeving is, moet de tag PRIVOnly zijn ingesteld op Waar.
    1. Voor PRIVOnly-omgevingen werkt u de **DNS-naam** en **NetBIOS-naam** van het PRIV-domein bij, zodat deze overeenkomen met het CORP-domein. Zorg dat de computerachtervoegsels juist zijn voor de computers waarop SQL, SharePoint en MIM worden geïnstalleerd, omdat met het standaardsjabloonbestand wordt uitgegaan van een CORP- en PRIV-configuratie.
    2. Klik hier voor meer informatie over PRIVOnly-omgevingen.

5. Kopieer dezelfde PAMDeploymentConfig.xml naar de map %SYSTEMDRIVE%\PAM op alle computers, CORPDC-, PRIVDC-, PAM-Server-, SQL Server-, en SharePoint-servers.


## <a name="deployment-worksheet"></a>Werkblad voor implementatie

Voordat u verdergaat, moet u de PAMDeploymentConfig.xml bijwerken en het bijgewerkte exemplaar op alle computers plaatsen.

### <a name="setup"></a>Setup

|Machine   | Uitvoeren als   |Opdrachten   |
|---|---|---|
|  PRIVDC |PRIV-domeinadministrator   | .\PAMDeployment.ps1 Selecteer menuoptie 1 (PRIV Forest Configuration)   |
|   |   |  Met de stap hierboven wordt een SIDS.txt gegenereerd. Dit bestand moet naar $envDrive:PAM van de CORPDC worden gekopieerd voordat u de volgende stap uitvoert. |
| CORPDC  |CORP-domeinadministrator   | .\PAMDeployment.ps1 Selecteer menuoptie 2 (CORP Forest Configuration)   |
| PAMServer (van SQL Server)   |CORP-domeinadministrator   |  .\PAMDeployment.ps1 Selecteer menuoptie 2 (CORP Forest Configuration)  |
|  PAMServer |  Lokale administrator (MIM Admin na toevoeging aan het domein) |  .\PAMDeployment.ps1 Selecteer menuoptie 4 (SharePoint Setup)  |
| PAMServer  | Lokale administrator (MIM Admin na toevoeging aan het domein)  | .\PAMDeployment.ps1 Selecteer menuoptie 5 (MIM PAM Setup)   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 Selecteer menuoptie 6 (PAM Trust Setup) .\PAMDeployment.ps1 Selecteer menuoptie 6 (PAM Trust Setup) |

### <a name="validation"></a>Validatie

|  Machine | Uitvoeren als   | Opdrachten   |
|---|---|---|
| CORP-client  | CORP-gebruiker (lokale administrator)  |   .\PAMDeployment.ps1 Selecteer menuoptie 7 (MIM PAM Client Setup)  |
| CORPDC  | CORP-domeinadministrator   | Importmodule.\PAMValidation.psm1; Maken-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Importmodule.\PAMValidation.psm1; Verplaatsen-PAMValidationUsersToPAM  |
| CORP-client  | CORP-gebruiker (lokale administrator)   |   Importmodule.\PAMValidation.psm1; Inschakelen PAMUsersCORPClientRemote |
|  CORP-client | <PRIV>\PRIV.pamRequestor-gebruiker en in het geval van PRIVOnly: <CORP>\pamrequestor   | Importmodule .\PAMValidation.psm1; Testen-PAMValidationScenarioNoApprovalRequest  |


>[!div class="step-by-step"]
[Start »](sp1-step1-configuring-priv-domain.md)



<!--HONumber=Nov16_HO2-->


