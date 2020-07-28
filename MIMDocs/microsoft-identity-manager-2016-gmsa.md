---
title: Microsoft Identity Manager-specifieke services converteren naar gMSA | Microsoft Docs
description: Dit artikel bevat de vereisten en basis stappen voor het configureren van een beheerd service account voor een groep (gMSA).
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 03/10/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 5985ded45a53a804728572404fb0db43e988ac1d
ms.sourcegitcommit: f87be3d09cee6a8880b3a6babf32e0d064fde36b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2020
ms.locfileid: "87176758"
---
# <a name="convert-microsoft-identity-manager-specific-services-to-use-group-managed-service-accounts"></a>Microsoft Identity Manager-specifieke services converteren voor het gebruik van door groepen beheerde service accounts

Dit artikel is een hand leiding voor het configureren van ondersteunde Microsoft Identity Manager services voor het gebruik van door groepen beheerde service accounts (gMSA). Nadat u uw omgeving vooraf hebt geconfigureerd, is het proces van het converteren naar gMSA eenvoudig.

## <a name="prerequisites"></a>Vereisten

- Down load en installeer de volgende vereiste hotfix: [Microsoft Identity Manager 4.5.26.0 of hoger](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history).

    Ondersteunde services:

    -   Microsoft Identity Manager-synchronisatie service (FIMSynchronizationService)
    -   Microsoft Identity Manager-service (FIMService)
    -   Microsoft Identity Manager wachtwoord registratie
    -   Microsoft Identity Manager wacht woord opnieuw instellen
    -   Privileged Access Management (PAM)-bewakings service (PamMonitoringService)
    -   PAM component-service (PrivilegeManagementComponentService)

    Niet-ondersteunde services:

    -   De Microsoft Identity Manager-portal wordt niet ondersteund. Het maakt deel uit van de share point-omgeving en moet worden geïmplementeerd in de farm modus en het [automatisch wijzigen van het wacht woord in share Point server configureren](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
    -   Alle beheer agenten, met uitzonde ring van de Microsoft Identity Manager-Service beheer agent
    -   Micro soft certificaat beheer
    -   BHOLD

- Zie voor achtergrond informatie en algemene referentie gegevens over het instellen van uw omgeving: 

    -   [Overzicht van door groepen beheerde service accounts](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)  
    -   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)  

- Voordat u begint, moet u [de basis sleutel voor Key Distribution Services maken](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx) op uw Windows-domein controller. Houd de volgende informatie in acht:  

    - Hoofd sleutels worden gebruikt door de KDS-service (Key Distribution Services) voor het genereren van wacht woorden en andere informatie op domein controllers.
    - Maak eenmaal per domein een basis sleutel, indien nodig.  
    - Include `Add-KDSRootKey –EffectiveImmediately` . "– EffectiveImmediately" betekent dat het Maxi maal 10 uur kan duren om de basis sleutel naar alle domein controllers te repliceren. Het kan ongeveer één uur duren voordat er naar twee domein controllers wordt gerepliceerd. 
    ![De teken reeks "– EffectiveImmediately"](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="actions-to-run-on-the-active-directory-domain-controller"></a>Acties die moeten worden uitgevoerd op de Active Directory-domein controller

1.  Maak een groep met de naam *MIMSync_Servers*en voeg hieraan alle synchronisatie servers toe.

    ![Een MIMSync_Servers groep maken](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

1.  Meld u aan bij Windows Power shell als een *domein beheerder* met een account dat al is toegevoegd aan het domein en voer de volgende opdracht uit: 

    `New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"`

    ![De opdracht in Power shell](media/f6deb0664553e01bcc6b746314a11388.png)

    - Details van de gMSA voor synchronisatie weer geven:  
     ![Details van de gMSA voor synchronisatie](media/c80b0a7ed11588b3fb93e6977b384be4.png)

    - Als u de meldings service voor wachtwoord wijzigingen (PCNS) uitvoert, werkt u de overdracht bij door de volgende opdracht uit te voeren:

        `Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames
        @{Add="PCNSCLNT/mimsync.contoso.com"}`

## <a name="actions-to-run-on-the-microsoft-identity-manager-synchronization-server"></a>Acties die moeten worden uitgevoerd op de Microsoft Identity Manager synchronisatie server

1. Maak een back-up van de versleutelings sleutel in Synchronization Service Manager. Er wordt gevraagd om de installatie van de wijzigings modus. Ga als volgt te werk:

    a. Op de server waarop Synchronization Service Manager is geïnstalleerd, zoekt u naar het hulp programma voor sleutel beheer van de synchronisatie service. De **sleutelset voor exporteren**   is standaard al geselecteerd.

    b. Selecteer **Next**. 
    
    c. Voer de gegevens van het Microsoft Identity Manager of het Forefront Identity Manager (FIM)-synchronisatie service account bij de prompt in en controleer deze:

    -   **Account naam**: de naam van het synchronisatie service account dat wordt gebruikt tijdens de eerste installatie.  
    -   **Wacht woord**: het wacht woord van het synchronisatie service account.  
    -   **Domein**: het domein waarvan het synchronisatie service account deel uitmaakt.

    d. Selecteer **Next**.

    Als u de account gegevens hebt ingevoerd, hebt u de mogelijkheid om de bestemming of de locatie van het export bestand van de back-versleutelings sleutel te wijzigen. Standaard is de locatie van het export bestand *C:\Windows\system32\miiskeys-1.bin*.

1. Installeer Microsoft Identity Manager SP1, dat u kunt vinden op het Volume Licensing service center of de MSDN down loads-site. Sla de sleutelset *miiskeys. bin*nadat u de installatie hebt voltooid.

   ![Het venster voortgang van de installatie van de Microsoft Identity Manager-synchronisatie service](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)

1. Installeer [hotfix 4.5.2.6.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) of hoger.

1. Nadat de patch is geïnstalleerd, stopt u de FIM-synchronisatie service door het volgende te doen:

   a. Selecteer in het configuratie scherm de optie **Program ma's en onderdelen**  >  **Microsoft Identity Manager**.  
   b. Selecteer op de pagina **synchronisatie service** de optie **wijzigen**  >  **volgende**.  
   c. Selecteer **configureren**in het venster **onderhouds opties** .

   ![Het venster onderhouds opties](media/dc98c011bec13a33b229a0e792b78404.png)

   d. In het venster **Microsoft Identity Manager synchronisatie service configureren wist u** de standaard waarde in het vak **Service account** en voert u vervolgens **MIMSyncGMSA $**. Zorg ervoor dat u het symbool voor het dollar teken ($) opneemt, zoals wordt weer gegeven in de volgende afbeelding. Laat het vak **wacht woord** leeg.

   ![Het venster Microsoft Identity Manager synchronisatie service configureren](media/38df9369bf13e1c3066a49ed20e09041.png)

   e. Selecteer **volgende**  >  **volgende**  >  **installatie**.  
   f. Herstel de sleutelset uit het *miiskeys. bin* -bestand dat u eerder hebt opgeslagen.

   ![Optie om de sleutelset te herstellen](media/44cd474323584feb6d8b48b80cfceb9b.png)

   ![De lijst met beheer agenten in Synchronization Service Manager](media/03e7762f34750365e963f0b90e43717c.png)

## <a name="microsoft-identity-manager-service"></a>Microsoft Identity Manager-service

>[!IMPORTANT]
>Volg de instructies in deze sectie zorgvuldig wanneer u Microsoft Identity Manager service-gerelateerde accounts omzet in gMSA-accounts.

1. Maak groeps beheerde accounts voor Microsoft Identity Manager service, de PAM rest API, PAM monitoring service, PAM component service, de registratie Portal selfservice voor het opnieuw instellen van wacht woorden (SSPR) en de SSPR reset-Portal.

    -   De gMSA-overdracht en de Service Principal Name (SPN) bijwerken:

        - `Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames
            @{Add="\<SPN\>"}`
    -   Overdracht

        - `Set-ADServiceAccount -Identity \<gmsaaccount\>
                -TrustedForDelegation $true`
    -   Beperkte delegering:
        -   `$delspns = 'http/mim', 'http/mim.contoso.com'`
        -   `New-ADServiceAccount -Name \<gmsaaccount\> -DNSHostName
                \<gmsaaccount\>.contoso.com
                -PrincipalsAllowedToRetrieveManagedPassword \<group\>
                -ServicePrincipalNames $spns -OtherAttributes
                @{'msDS-AllowedToDelegateTo'=$delspns }`

1. Voeg een account toe voor de Microsoft Identity Manager-service in synchronisatie groepen. Deze stap is nodig voor SSPR.

    ![Het venster gebruikers en computers Active Directory](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

    > [!NOTE]  
    > Een bekend probleem in Windows Server 2012 R2 is dat services die gebruikmaken van een beheerd account niet meer reageren nadat de server opnieuw is opgestart, omdat micro soft Key Distribution-service niet is gestart nadat Windows opnieuw is opgestart. De tijdelijke oplossing voor dit probleem is om de volgende opdracht uit te voeren: 
    >
    > `sc triggerinfo kdssvc start/networkon`
    >
    > De opdracht start micro soft Key Distribution service wanneer het netwerk is ingeschakeld (meestal in de opstart cyclus).
    >
    > Zie [AD FS Windows 2012 R2: adfssrv is vastgelopen in de start modus](https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS)voor een discussie over een soortgelijk probleem.

1.  Voer een verhoogde MSI uit van Microsoft Identity Manager service en selecteer **wijzigen**.

1.  Schakel in het venster **e-mail server verbinding configureren** het selectie vakje **andere gebruiker gebruiken voor Exchange (voor beheerde accounts)** in. U krijgt de mogelijkheid om het huidige Exchange-account of het Cloud postvak te gebruiken.

    >[!NOTE]
    >Als u de optie **Exchange Online gebruiken** selecteert, stelt u Microsoft Identity Manager-service in staat om goedkeurings reacties van de Microsoft Identity Manager Outlook-invoeg toepassing te verwerken door de register sleutel **HKLM\SYSTEM\CurrentControlSet\Services\FIMService** waarde *PollExchangeEnabled* in te stellen op **1** na de installatie.
    
    ![Het venster e-mail server verbinding configureren](media/0cd8ce521ed7945c43bef6100f8eb222.png)

1.  Voer in het venster **het MIM-service account configureren** in het vak **Service account naam** de naam in. Zorg ervoor dat u het symbool voor het dollar teken ($) opneemt. Voer een wacht woord in het vak **wacht woord voor service-e-mail account** in. Het vak **wacht woord voor service account** moet niet beschikbaar zijn.

    ![Het venster het MIM-service account configureren](media/db0d543df6e1b0174a47135617c23fcb.png)

    Omdat de functie LogonUser niet werkt voor beheerde accounts, wordt in de volgende pagina de waarschuwing weer gegeven: ' Controleer of het service account veilig is in de huidige configuratie '.

    ![Waarschuwings venster account beveiliging](media/d350bc13751b2d0a884620db072ed019.png)

1.  Voer in het venster **Privileged Access Management rest API configureren** in het vak **account naam van groep van toepassingen** de naam van het account in. Zorg ervoor dat u het symbool voor het dollar teken ($) opneemt. **Wacht woord voor account voor groep van toepassingen** leeg laten.

    ![Het venster Privileged Access Management REST API configureren](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

1.  Voer in het venster **pam-component service configureren** in het vak **Service account name** de account naam in. Zorg ervoor dat u het symbool voor het dollar teken ($) opneemt. Laat het vak **wacht woord voor service account** leeg.

    ![Het venster PAM-component service configureren](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

    ![Het venster account beveiligings waarschuwing](media/9d2b52f6faed10601e7e2166a339fb47.png)

1.  In het venster **privileged Access Management monitoring service configureren** in het vak **Service account name** , typt u de naam van het service account. Zorg ervoor dat u het symbool voor het dollar teken ($) opneemt. Laat het vak **wacht woord voor service account** leeg.

    ![Het venster Privileged Access Management monitoring service configureren](media/d1e824248edf12a77fc9ffb011475164.png)

1.  Voer in het venster **MIM-wachtwoord registratie configureren** in het vak **account naam** de naam van het account in. Zorg ervoor dat u het symbool voor het dollar teken ($) opneemt. Laat het vak **wacht woord** leeg.

    ![Het venster MIM-wachtwoord registratie-Portal configureren](media/601e935cdfda298b61ae753a2a152996.png)

1.  Voer in het venster **MIM-wacht woord opnieuw instellen configureren** in het vak **account naam** de naam van het account in. Zorg ervoor dat u het symbool voor het dollar teken ($) opneemt. Laat het vak **wacht woord** leeg.

    ![Het venster MIM-wacht woord opnieuw instellen configureren](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

1.  Voltooi de installatie.

    > [!NOTE]
    > Tijdens de installatie worden er twee nieuwe sleutels gemaakt in het registerpad **HKEY_LOCAL_MACHINE \Software\microsoft\forefront Identity Manager\2010\Service** voor het opslaan van het versleutelde Exchange-wacht woord. Eén vermelding is voor *ExchangeOnline*en de andere voor *ExchangeOnPremise*. Voor een van de vermeldingen moet de waarde in de kolom **gegevens** leeg zijn.

    > ![De REGI ster-editor](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

[Down load dit Power shell-script](microsoft-identity-manager-2016-gmsascript.md)om het wacht woord bij te werken naar uw opgeslagen accounts zonder dat u de modus wijzigen hoeft uit te voeren.

Het installatie programma maakt een extra service en voert deze uit onder het beheerde account om het Exchange-wacht woord te versleutelen. De volgende berichten worden tijdens de installatie toegevoegd aan het gebeurtenis logboek van de **toepassing** :

![Het Logboeken venster](media/95b315454705cd4d939b55ac5ad910f5.jpg)
