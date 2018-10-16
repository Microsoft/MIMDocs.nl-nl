---
title: Een domein instellen voor Microsoft Identity Manager 2016 | Microsoft Docs
description: Maak een Active Directory-domeincontroller voordat u MIM 2016 installeert
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/26/2017
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c4fd7d2e8cc6e5efc092d92bc4a97a9516fe7d4c
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333238"
---
# <a name="set-up-a-domain"></a>Stel een domein in

> [!div class="step-by-step"]
> [WindowsServer 2016»](prepare-server-ws2016.md)

Microsoft Identity Manager (MIM) werkt samen met uw Active Directory-domein (AD). U moet AD al hebben geïnstalleerd en ervoor zorgen dat uw omgeving een domeincontroller bevat voor een domein dat u kunt beheren.

In dit artikel wordt uitgelegd welke stappen u moet doorlopen om uw domein voor te bereiden op een samenwerking met MIM.

## <a name="create-user-accounts-and-groups"></a>Maak gebruikersaccounts en groepen

Alle onderdelen van uw MIM-implementatie hebben een eigen identiteit in het domein nodig. Dit geldt onder andere voor MIM-onderdelen als Service en Sync, evenals SharePoint en SQL.

> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van de domeincontroller - **corpdc**
> - Domeinnaam: **contoso**
> - Naam van de MIM-Service-Server - **corpservice**
> - Naam van de MIM-synchronisatieserver - **corpsync**
> - Naam van SQL Server - **corpsql**
> - Wachtwoord - <strong>Pass@word1</strong>

1. Meld u als domeinbeheerder (*bijvoorbeeld Contoso\Administrator*) aan bij de domeincontroller.

2. Maak de volgende gebruikersaccounts voor MIM-services. Start PowerShell en typ het volgende PowerShell-script voor het bijwerken van het domein.

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMINSTALL –name MIMMA
    Set-ADAccountPassword –identity MIMINSTALL –NewPassword $sp
    Set-ADUser –identity MIMINSTALL –Enabled 1 –PasswordNeverExpires 1
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
    New-ADUser –SamAccountName MIMpool –name BackupAdmin
    Set-ADAccountPassword –identity MIMPool –NewPassword $sp
    Set-ADUser –identity MIMPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Maak beveiligingsgroepen op alle groepen.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMInstall
    ```

4.  Voeg SPN's toe om Kerberos-verificatie voor serviceaccounts mogelijk te maken

    ```CMD
    setspn -S http/mim.contoso.com Contoso\mimpool
    setspn -S http/mim Contoso\mimpool
    setspn -S http/passwordreset.contoso.com Contoso\mimsspr
    setspn -S http/passwordregistration.contoso.com Contoso\mimsspr
    setspn -S FIMService/mim.contoso.com Contoso\MIMService
    setspn -S FIMService/corpservice.contoso.com Contoso\MIMService
    ```
5.  Tijdens de installatie moeten we de volgende "A" DNS-records voor een juiste naamomzetting toevoegen

- Mim.contoso.com punt corpservice fysieke IP-adres
- PasswordReset.contoso.com punt corpservice fysieke IP-adres
- passwordregistration.contoso.com punt corpservice fysieke IP-adres

> [!div class="step-by-step"]
> [WindowsServer 2016»](prepare-server-ws2016.md)
