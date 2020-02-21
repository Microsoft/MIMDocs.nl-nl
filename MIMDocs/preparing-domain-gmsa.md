---
title: Stel een Gmsa's in voor Microsoft Identity Manager 2016 | Microsoft Docs
description: Door groepen beheerde service accounts instellen in een domein voor Microsoft Identity Manager 2016
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 1/7/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: be5dc1e8615f56d3157a78891e80897e446eafab
ms.sourcegitcommit: 32c7a46b2f8ed3f2f9ebc6f79a4ecb0019fe62e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2020
ms.locfileid: "77527919"
---
# <a name="configure-a-domain-for-group-managed-service-accounts-gmsa-scenario"></a>Een domein configureren voor een gMSA-scenario (Group managed service accounts)

> [!div class="step-by-step"]
> [Windows Server»](prepare-server-ws2016.md)

> [!IMPORTANT]
> Dit artikel is alleen van toepassing op MIM 2016 SP2.

Microsoft Identity Manager (MIM) werkt samen met uw Active Directory-domein (AD). U moet AD al hebben geïnstalleerd en ervoor zorgen dat uw omgeving een domeincontroller bevat voor een domein dat u kunt beheren.  In dit artikel wordt beschreven hoe u door de groep beheerde service accounts in dat domein instelt voor gebruik door MIM.

## <a name="overview"></a>Overzicht

Groep beheerde service accounts elimineren de nood zaak om wacht woorden van service accounts niet regel matig te wijzigen. Met de release van MIM 2016 SP2 kunnen voor de volgende MIM-onderdelen gMSA-accounts worden geconfigureerd die moeten worden gebruikt tijdens het installatie proces:

-   MIM-synchronisatie service (FIMSynchronizationService)
-   MIM-service (FIMService)
-   MIM-wachtwoord registratie voor de toepassings groep van de website
-   Toepassings groep voor MIM-wacht woord opnieuw instellen
-   PAM REST API-toepassings groep voor website
-   PAM-bewakings service (PamMonitoringService)
-   PAM component-service (PrivilegeManagementComponentService)

De volgende MIM-onderdelen ondersteunen geen uitvoeren als gMSA-accounts:

-   MIM-Portal. Dit komt omdat de MIM-Portal deel uitmaakt van de share point-omgeving. In plaats daarvan kunt u share point implementeren in de farm modus en [Automatische wachtwoord wijziging configureren in share Point server](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
-   Alle beheer agenten
-   Micro soft certificaat beheer
-   BHOLD


Meer informatie over gMSA vindt u in de volgende artikelen:
-   [Overzicht van door groepen beheerde service accounts](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)

-   [De KDS-hoofd sleutel voor Key Distribution Services maken](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx)

## <a name="create-user-accounts-and-groups"></a>Maak gebruikersaccounts en groepen

Alle onderdelen van uw MIM-implementatie hebben een eigen identiteit in het domein nodig. Dit geldt onder andere voor MIM-onderdelen als Service en Sync, evenals SharePoint en SQL.


> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van domein controller- **DC**
> - Domeinnaam: **contoso**
> - Naam van de MIM-service Server- **mimservice**
> - Naam MIM-synchronisatie server- **mimsync**
> - SQL Server naam- **SQL**
> - Wachtwoord - <strong>Pass@word1</strong>

1. Meld u als domeinbeheerder (*bijvoorbeeld Contoso\Administrator*) aan bij de domeincontroller.

2. Maak de volgende gebruikersaccounts voor MIM-services. Start Power shell en typ het volgende Power shell-script om nieuwe AD-domein gebruikers te maken (niet alle accounts zijn verplicht, hoewel het script alleen ter informatie wordt verstrekt, is het een best practice om een speciaal *MIMAdmin* -account te gebruiken voor het MIM-en share point-installatie proces).

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

    New-ADUser –SamAccountName MIMAdmin –name MIMAdmin
    Set-ADAccountPassword –identity MIMAdmin –NewPassword $sp
    Set-ADUser –identity MIMAdmin –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcSharePoint –name svcSharePoint
    Set-ADAccountPassword –identity svcSharePoint –NewPassword $sp
    Set-ADUser –identity svcSharePoint –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMSql –name svcMIMSql
    Set-ADAccountPassword –identity svcMIMSql –NewPassword $sp
    Set-ADUser –identity svcMIMSql –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMAppPool –name svcMIMAppPool
    Set-ADAccountPassword –identity svcMIMAppPool –NewPassword $sp
    Set-ADUser –identity svcMIMAppPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Maak beveiligingsgroepen op alle groepen.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordSet –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordSet
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupMember -identity MIMSyncAdmins -Members MIMAdmin
    ```

4.  Voeg SPN's toe om Kerberos-verificatie voor serviceaccounts mogelijk te maken

    ```PowerShell
    setspn -S http/mim.contoso.com contoso\svcMIMAppPool
    ```

5.  Zorg ervoor dat u de volgende DNS-records voor de juiste naam omzetting registreert (ervan uitgaande dat de MIM-service, de MIM-Portal, de wachtwoord herstel-en wachtwoord registratie websites op dezelfde computer worden gehost)

    - mim.contoso.com: het fysieke IP-adres van de MIM-service en Portal Server
    - passwordreset.contoso.com: het fysieke IP-adres van de MIM-service en Portal Server
    - passwordregistration.contoso.com: het fysieke IP-adres van de MIM-service en Portal Server

## <a name="create-key-distribution-service-root-key"></a>Basis sleutel van de Key Distribution-service maken

Zorg ervoor dat u bent aangemeld bij uw domein controller als beheerder om de groep Key Distribution-Service voor te bereiden.

Als er al een hoofd sleutel voor het domein is (gebruik **Get-KdsRootKey** om te controleren), gaat u verder met de volgende sectie.

6.  Maak, indien nodig, de hoofd sleutel distributie Services (KDS) (slechts eenmaal per domein). De basis sleutel wordt door de KDS-service op domein controllers (samen met andere informatie) gebruikt voor het genereren van wacht woorden. Als domein beheerder typt u de volgende Power shell-opdracht:

    ```PowerShell
    Add-KDSRootKey –EffectiveImmediately
    ```
    *-EffectiveImmediately* kan een vertraging van maxi maal \~tien uur vereisen, omdat deze moet worden gerepliceerd naar alle domein controllers. Deze vertraging was ongeveer 1 uur voor twee domein controllers.

    ![](media/7fbdf01a847ea0e330feeaf062e30668.png)

    >[!NOTE]
    >In de test omgeving kunt u tien uur replicatie vertraging voor komen door de volgende opdracht uit te voeren:
    ><br/>
    >Add-KDSRootKey-EffectiveTime ((Get-date). AddHours (-10)

## <a name="create-mim-synchronization-service-account-group-and-service-principal"></a>MIM-synchronisatie service account, groeps-en service-principal maken
-----------------------

Zorg ervoor dat alle computer accounts voor computers waarop MIM-software wordt geïnstalleerd, al lid zijn van het domein.  Voer vervolgens deze stappen uit in Power shell als een domein beheerder.

7.  Maak een groep *MIMSync_Servers* en voeg alle MIM-synchronisatie servers toe aan deze groep.
    Typ het volgende om een nieuwe AD-groep voor MIM-synchronisatie servers te maken. Vervolgens voegt u de MIM-synchronisatie Server Active Directory computer accounts, bijvoorbeeld *beheeragentaccount $* , toe aan deze groep.

    ```PowerShell
    New-ADGroup –name MIMSync_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMSync_Servers
    Add-ADGroupmember -identity MIMSync_Servers -Members MIMSync$
    ```

8.  De MIM-synchronisatie service gMSA maken. Typ de volgende Power shell.

    ```PowerShell
    New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"
    ```

    Controleer de details van de GSMA die zijn gemaakt door de Power shell *-opdracht Get-ADServiceAccount* uit te voeren:

    ![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

9.  Als u de meldings service voor wachtwoord wijzigingen wilt uitvoeren, moet u de naam van de Service-Principal registreren door de volgende Power shell-opdracht uit te voeren:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames @{Add="PCNSCLNT/mimsync.contoso.com"}
    ```

10. Start de MIM-synchronisatie server opnieuw op om een Kerberos-token te vernieuwen dat is gekoppeld aan de server, omdat het groepslid maatschap MIMSync_Server is gewijzigd.

## <a name="create-mim-service-management-agent-service-account"></a>Het service account van de MIM-Service beheer agent maken

11. Normaal gesp roken maakt u bij de installatie van de MIM-service een nieuw account voor de MIM-Service beheer agent (MIM MA-account).  Bij gMSA zijn er twee opties beschikbaar:

- Het beheerde service account van de MIM-synchronisatie service-groep gebruiken en geen afzonderlijk account maken

    U kunt het maken van het service account van de MIM-Service beheer agent overs Laan. Gebruik in dit geval de MIM-synchronisatie service gMSA naam, bijvoorbeeld *contoso\MIMSyncGMSAsvc $* , in plaats van het MIM ma-account bij de installatie van de MIM-service. Verderop in de configuratie van de MIM-Service beheer agent schakelt u de optie *MIMSync account gebruiken* in.

    Schakel aanmelden op het netwerk weigeren voor de MIM-synchronisatie service gMSA niet in als MIM MA-account vereist toestemming voor netwerk aanmelding toestaan.

- Een normaal service account gebruiken voor het service account van de MIM-Service beheer agent

    Start Power shell als domein beheerder en typ het volgende om een nieuwe AD-domein gebruiker te maken:

    ```PowerShell
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    
    New-ADUser –SamAccountName svcMIMMA –name svcMIMMA
    Set-ADAccountPassword –identity svcMIMMA –NewPassword $sp
    Set-ADUser –identity svcMIMMA –Enabled 1 –PasswordNeverExpires 1
    ```

    Schakel aanmelden via netwerk weigeren niet in voor het MIM MA-account omdat de machtiging ' netwerk aanmelding toestaan ' is vereist.

## <a name="create-mim-service-accounts-groups-and-service-principal"></a>MIM-service accounts,-groepen en-service-principal maken

Ga door met het gebruik van Power shell als een domein beheerder.
   
12. Maak een groep *MIMService_Servers* en voeg alle MIM-Service servers toe aan deze groep.  Typ de volgende Power shell om een nieuwe AD-groep voor de MIM-Service servers te maken en de MIM-service Server toe te voegen Active Directory computer account, bijvoorbeeld *contoso\MIMPortal $* , in deze groep.

    ```PowerShell
    New-ADGroup –name MIMService_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMService_Servers
    Add-ADGroupMember -identity MIMService_Servers -Members MIMPortal$
    ```

1.  De gMSA van de MIM-service maken.

    ```PowerShell
    New-ADServiceAccount -Name MIMSrvGMSAsvc -DNSHostName MIMSrvGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMService_Servers" -OtherAttributes @{'msDS-AllowedToDelegateTo'='FIMService/mimportal.contoso.com'} 
    ```

1.  Registreer de service principal name en Schakel Kerberos delegering in door de volgende Power shell-opdracht uit te voeren:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSrvGMSAsvc -TrustedForDelegation $true -ServicePrincipalNames @{Add="FIMService/mimportal.contoso.com"}
    ```

1.  Voor SSPR-scenario's moet u het MIM-service account kunnen communiceren met de MIM-synchronisatie service, dus het MIM-service account dient lid te zijn van MIMSyncAdministrators of het wacht woord voor het opnieuw instellen van het gebruikers wachtwoord en door groepen te zoeken:

    ```PowerShell
    Add-ADGroupmember -identity MIMSyncPasswordSet -Members MIMSrvGMSAsvc$ 
    Add-ADGroupmember -identity MIMSyncBrowse -Members MIMSrvGMSAsvc$ 
    ```

16. Start de MIM-service Server opnieuw op om een Kerberos-token te vernieuwen dat is gekoppeld aan de server, omdat het groepslid maatschap MIMService_Servers is gewijzigd.

## <a name="create-other-mim-accounts-and-groups-if-needed"></a>Andere MIM-accounts en-groepen maken, indien nodig

Als u MIM SSPR configureert, volgt u dezelfde richt lijnen als hierboven beschreven voor de MIM-synchronisatie service en de MIM-service, kunt u andere gMSA maken voor:

- Toepassings groep voor MIM-wacht woord opnieuw instellen
- MIM-wachtwoord registratie voor de toepassings groep van de website

Als u de MIM PAM configureert, volgt u dezelfde richt lijnen als hierboven beschreven voor de MIM-synchronisatie service en de MIM-service, kunt u andere gMSA maken voor:

- MIM PAM REST API-toepassings groep voor website
- MIM PAM component-service
- MIM PAM-bewakings service

## <a name="specifying-a-gmsa-when-installing-mim"></a>Een gMSA opgeven tijdens de installatie van MIM

Als algemene regel moet u in de meeste gevallen, wanneer u een MIM-installatie programma gebruikt, opgeven dat u een gMSA wilt gebruiken in plaats van een gewoon account, een dollar teken toevoegen aan de gMSA-naam, bijvoorbeeld **contoso\MIMSyncGMSAsvc $** , en het veld wacht woord leeg laten. Een uitzonde ring is het hulp programma *miisactivate. exe* dat gMSA naam accepteert zonder het dollar teken.
<br/>

> [!div class="step-by-step"]
> [Windows Server»](prepare-server-ws2016.md)
