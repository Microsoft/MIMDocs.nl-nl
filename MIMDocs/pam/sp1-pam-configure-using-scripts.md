---
title: PAM configureren met behulp van scripts
description: Dit artikel is onderdeel van de serie artikelen over het configureren van PAM met behulp van scripts. In het artikel wordt de aanpassing behandeld van het XML-bestand dat wordt gebruikt voor de PAM-implementatiescripts.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 07/20/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 102754fc88af32cb9abed40716ba9168a041d58e
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043882"
---
# <a name="configure-pam-using-scripts"></a>PAM configureren met behulp van scripts

Als u ervoor kiest om SQL en SharePoint op afzonderlijke servers te installeren, moet u de servers configureren met de onderstaande instructies. Als SQL, SharePoint en de PAM-onderdelen zijn geïnstalleerd op dezelfde computer, moeten de onderstaande stappen worden uitgevoerd vanaf deze computer.

In de volgende stappen wordt ervan uitgegaan dat er al een PRIV-domein is ingesteld voor de instructies om een PRIV-domein te configureren en de bijlage aan het einde van het document te bekijken.

stappen:

1. Downloaden [PAM-implementatiescripts](https://www.microsoft.com/download/details.aspx?id=53941)
2. Pak het gecomprimeerde bestand PAMDeploymentScripts.zip uit naar de map %SYSTEMDRIVE%\PAM op alle computers.
3. Open op een van de machines het bestand **PAMDeploymentConfig.xml** en werk de details bij met behulp van de onderstaande grafiek of aan de hand van de richtlijnen in het XML-bestand. Als de CORP- en PRIV-forests al zijn ingesteld, hoeft u alleen de **DNS-naam** en de **NetBIOS-naam** bij te werken.
4. In de sectie Rollen werkt u het **serviceaccount**, **de details van de computer** en de **locatie van de binaire installatiebestanden** voor SQL-, SharePoint- en MIM-rollen bij.
    1. De locatie van het binaire bestand voor MIM moet verwijzen naar de map met de map Service and Portal. De locatie van het binaire bestand voor de client moet verwijzen naar de map met Add-ins and Extensions.msi.

5. Als dit een PRIVOnly-omgeving is, moet de tag PRIVOnly zijn ingesteld op Waar.
    1. Voor PRIVOnly-omgevingen werkt u de **DNS-naam** en **NetBIOS-naam** van het PRIV-domein bij, zodat deze overeenkomen met het CORP-domein. Zorg dat de computerachtervoegsels juist zijn voor de computers waarop SQL, SharePoint en MIM worden geïnstalleerd, omdat met het standaardsjabloonbestand wordt uitgegaan van een CORP- en PRIV-configuratie.
    2. Klik hier voor meer informatie over PRIVOnly-omgevingen.

6. Kopieer dezelfde PAMDeploymentConfig.xml naar de map %SYSTEMDRIVE%\PAM op alle computers, CORPDC-, PRIVDC-, PAM-Server-, SQL Server-, en SharePoint-servers.


## <a name="deployment-worksheet"></a>Werkblad voor implementatie

Voordat u doorgaat, gaat u verder met het bijwerken van PAMDeploymentConfig. XML en plaatst u de bijgewerkte kopie op alle machines.

### <a name="setup"></a>Instellen

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


> [!div class="step-by-step"]
> [Start »](sp1-step1-configuring-priv-domain.md)
