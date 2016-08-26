---
title: Een domein instellen | Microsoft Identity Manager
description: Maak een Active Directory-domeincontroller voordat u MIM 2016 installeert
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: 9c029a9edee015f8843f6001d23c72e3ef98b5af


---

# Stel een domein in

>[!div class="step-by-step"]  
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)

Microsoft Identity Manager (MIM) werkt samen met uw Active Directory-domein (AD). U moet AD al hebben geïnstalleerd en ervoor zorgen dat uw omgeving een domeincontroller bevat voor een domein dat u kunt beheren.

In dit artikel wordt uitgelegd welke stappen u moet doorlopen om uw domein voor te bereiden op een samenwerking met MIM.

## Maak gebruikersaccounts en groepen

Alle onderdelen van uw MIM-implementatie hebben een eigen identiteit in het domein nodig. Dit geldt onder andere voor MIM-onderdelen als Service en Sync, evenals SharePoint en SQL.

> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van de domeincontroller: **mimservername**
> - Domeinnaam: **contoso**
> - Wachtwoord: **Pass@word1**

1. Meld u als domeinbeheerder (*bijvoorbeeld Contoso\Administrator*) aan bij de domeincontroller.

2. Maak de volgende gebruikersaccounts voor MIM-services. Start PowerShell en typ het volgende PowerShell-script voor het bijwerken van het domein.

    ```
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMMA –name MIMMA
    Set-ADAccountPassword –identity MIMMA –NewPassword $sp
    Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSync –name MIMSync
    Set-ADAccountPassword –identity MIMSync –NewPassword $sp
    Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMService –name MIMService
    Set-ADAccountPassword –identity MIMService –NewPassword $sp
    Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSSPR –name MIMSSPR
    Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
    Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SharePoint –name SharePoint
    Set-ADAccountPassword –identity SharePoint –NewPassword $sp
    Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SqlServer –name SqlServer
    Set-ADAccountPassword –identity SqlServer –NewPassword $sp
    Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
    Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
    Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
    ```

2.  Maak beveiligingsgroepen op alle groepen.

    ```
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global      –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global       –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global         –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global      –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global          –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    ```

3.  Voeg SPN's toe om Kerberos-verificatie voor serviceaccounts mogelijk te maken

    ```
    setspn -S http/mimservername.contoso.local Contoso\SharePoint
    setspn -S http/mimservername Contoso\SharePoint
    setspn -S MIMService/mimservername.contoso.local Contoso\MIMService
    setspn -S MIMSync/mimservername.contoso.local Contoso\MIMSync
    ```

>[!div class="step-by-step"]  
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)



<!--HONumber=Jul16_HO3-->


