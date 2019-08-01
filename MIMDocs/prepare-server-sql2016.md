---
title: SQL Server configureren voor Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: Installeer SQL Server 2016 in voor bereiding op de installatie van MIM 2016.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d32638bda8fd757233af0c697ea3d1ac9eb47eb9
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701374"
---
# <a name="set-up-an-identity-management-server-sql-server-2016"></a>Een server voor identiteits beheer instellen: SQL Server 2016

> [!div class="step-by-step"]
> [«Windows Server 2016](prepare-server-ws2016.md)
> [share point»](prepare-server-sharepoint.md)
> 
> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van domein controller- **corpdc**
> - Domeinnaam: **contoso**
> - Naam van de MIM-service Server- **corpservice**
> - Naam MIM-synchronisatie server- **corpsync**
> - SQL Server naam- **corpsql**
> - Wachtwoord - <strong>Pass@word1</strong>

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
> 
> [!div class="step-by-step"]  
> [«Windows Server 2016](prepare-server-ws2016.md)
> [share point»](prepare-server-sharepoint.md)
