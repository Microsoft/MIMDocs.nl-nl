---
title: SQL Server configureren voor Microsoft Identity Manager 2016 SP2 | Microsoft Docs
description: Installeer SQL Server 2016 of 2017 ter voor bereiding op de installatie van MIM 2016.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2ca9131a6c4f38ed559618d662848b74e1bffe66
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043491"
---
# <a name="set-up-an-identity-management-server-sql-server-2016-or-2017"></a>Een server voor identiteits beheer instellen: SQL Server 2016 of 2017

> [!div class="step-by-step"]
> [«Windows Server](prepare-server-ws2016.md)
> [share point»](prepare-server-sharepoint.md)
 
> [!NOTE] 
> De installatie procedure van SQL Server 2017 verschilt niet van de installatie procedure van SQL Server 2016.

> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van domein controller- **corpdc**
> - Domeinnaam: **contoso**
> - Naam van de MIM-service Server- **corpservice**
> - Naam MIM-synchronisatie server- **corpsync**
> - SQL Server naam- **corpsql**
> - Wacht woord<strong>Pass@word1</strong>

> [!IMPORTANT]
> MIM 2016 SP2 ondersteunt de SQL AlwaysOn-beschikbaarheids groep (AoAG) listeners waarvoor de *RegisterAllProvidersIP* -optie is ingesteld op 0, wat betekent dat SQL Server failover voor meerdere subnetten op dit moment niet wordt ondersteund.

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Installeer **SQL Server 2016 Standard/Enter prise Edition**

1. Start **PowerShell** als een domeinbeheerder.

2. Ga naar de map waar zich het installatieprogramma van SQL Server bevindt.

3. Typ de volgende opdrachten:

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```
    
Meer informatie over SQL-implementatie accounts en-services vindt u [hier](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)

> [!NOTE]
> SSMS is niet meer opgenomen in SQL 2016. Download gegevens vindt u [hier](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)

> [!div class="step-by-step"]  
> [«Windows Server](prepare-server-ws2016.md)
> [share point»](prepare-server-sharepoint.md)
