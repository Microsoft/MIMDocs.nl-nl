---
title: De Windows-toepassing MIM Certificate Manager implementeren | Microsoft Docs
description: Lees hoe u de Certificate Manager-app kunt implementeren zodat de gebruikers hun eigen toegangsrechten kunnen beheren.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/16/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2adf1152aaf874d0ff0d93079fb4bfbfcf731b60
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044290"
---
# <a name="mim-certificate-manager-windows-store-application-deployment"></a>Implementatie van de Windows Store-app voor MIM Certificate Manager

Nadat u MIM 2016 en Certificate Manager hebt geïnstalleerd en uitgevoerd, kunt u de Windows Store-toepassing voor MIM Certificate Manager implementeren. Met de Windows Store-toepassing kunnen uw gebruikers hun fysieke Smart Cards, virtuele Smart Cards en software certificaten beheren. U kunt via de volgende stappen de MIM CM-app implementeren:

1. Maak een certificaatsjabloon.

2. Maak een profielsjabloon.

3. Bereid de app voor.

4. Implementeer de app via SCCM of Intune.

## <a name="create-a-certificate-template"></a>Een certificaatsjabloon maken

U maakt een certificaatsjabloon voor de CM-app op dezelfde manier als u normaal gesproken doet, alleen moet u ervoor zorgen dat de certificaatsjabloon versie 3 of hoger heeft.

1. Meld u aan bij de server waarop AD CS (de certificaat server) wordt uitgevoerd.

2. Open MMC.

3. Klik op **bestand &gt; module toevoegen/verwijderen**.

4. Klik in de lijst met beschikbare modules op **Certificaatsjablonen** en vervolgens op **Toevoegen**.

5. In MMC wordt nu **Certificaatsjablonen** weergegeven onder **Consolebasis**. Dubbelklik erop om alle beschikbare certificaatsjablonen weer te geven.

6. Klik met de rechtermuisknop op de sjabloon **Smartcardaanmelding** en klik op **Sjabloon dupliceren**.

7. Selecteer op het tabblad Compatibiliteit onder certificerings instantie de optie Windows Server 2008. Selecteer onder ontvanger van certificaat de optie Windows 8.1/Windows Server 2012 R2. De versie sjabloon versie wordt ingesteld als de eerste keer dat u de certificaat sjabloon maakt en opslaat. Als u de certificaat sjabloon niet op deze manier hebt gemaakt, is het niet mogelijk om deze naar de juiste versie te wijzigen.

   > [!NOTE]
   >  Deze stap is cruciaal omdat het ervoor zorgt dat u beschikt over een certificaat sjabloon van versie 3 (of hoger). Alleen sjablonen van versie 3 werken met de app certificaat beheer.

8. Typ op het tabblad **Algemeen** in het veld **Weergavenaam** de naam die moet worden weergeven in de gebruikersinterface van de app, zoals **Aanmelding met virtuele smartcard**.

9. Stel op het tabblad **Afhandeling van aanvragen** het **Doel** in op **Handtekening en versleuteling** en selecteer onder **Doe het volgende...** de optie **De gebruiker om invoer vragen tijdens het inschrijven**.

10. Selecteer op het tabblad **Cryptografie** onder **Providercategorie** de optie **Sleutelopslagprovider en aanvragen kunnen gebruikmaken van elke beschikbare provider op de computer van de certificaathouder**.

    > [!NOTE]
    > Sleutelopslagprovider wordt alleen als een optie weergegeven als de sjabloon versie 3 heeft. Als deze niet wordt weergegeven, hebt u de certificaatsjabloon waarschijnlijk niet correct en dus niet met de juiste versie gemaakt. Begin opnieuw bij stap 5 die hierboven wordt vermeld.

11. Voeg op het tabblad **Beveiliging** de beveiligingsgroep toe waarvoor u toegang voor **Inschrijven** wilt verlenen. Als u bijvoorbeeld aan alle gebruikers toegang wilt verlenen, selecteert u de groep **Geverifieerde gebruikers** en selecteert u vervolgens **Inschrijvingsmachtigingen** voor deze gebruikers.

12. Klik op **OK** om de wijzigingen te voltooien en de nieuwe sjabloon te maken. De nieuwe sjabloon zou nu in de lijst met certificaatsjablonen moeten worden weergegeven.

13. Selecteer **Bestand** en klik op **Module toevoegen/verwijderen** om de module Certificeringsinstantie toe te voegen aan de MMC-console. Wanneer u wordt gevraagd welke computer u wilt beheren, selecteert u **Lokale computer**.

14. Vouw in het linkerdeelvenster van MMC **Certificeringsinstantie (lokaal)** uit en vouw vervolgens uw certificeringsinstantie in de lijst met certificeringsinstanties uit.

15. Klik met de rechtermuisknop op **Certificaatsjablonen**, klik op **Nieuw &gt; Certificaatsjabloon** die moet worden uitgegeven.

16. Selecteer in de lijst de nieuwe sjabloon die u hebt gemaakt en klik op **OK**.

## <a name="create-a-profile-template"></a>Een profielsjabloon maken

Wanneer u een profielsjabloon maakt, moet u deze instellen op Virtuele smartcard maken/verwijderen en op Gegevensverzameling verwijderen. De CM-app kan geen verzamelde gegevens verwerken, dus is het belangrijk dat u deze optie als volgt uitschakelt.

1.  Meld u bij de CM-portal aan als gebruiker met beheerdersbevoegdheden.

2.  Ga naar beheer &gt; Profiel sjablonen beheren. Zorg ervoor dat het selectie vakje naast MIM CM voor **beeld van een smartcard logboek op profiel sjabloon** is ingeschakeld en klik vervolgens op een geselecteerde profiel sjabloon kopiëren.

3.  Typ de naam van de profielsjabloon en klik op **OK**.

4.  Klik in het volgende scherm op **Nieuwe certificaatsjabloon toevoegen** en zorg ervoor dat het selectievakje naast de naam van de certificeringsinstantie is ingeschakeld.

5.  Schakel het selectievakje naast de naam van de profielsjabloon **Aanmelding** in en klik op **Toevoegen**.

6.  Verwijder de SmartCardLogon-sjabloon door het selectievakje ernaast in te schakelen, op **Geselecteerde certificaatsjablonen verwijderen** en vervolgens op **OK** te klikken.

7.  Schuif helemaal naar beneden en klik op **Instellingen wijzigen**.

8.  Schakel de selectievakjes naast **Virtuele smartcard maken/verwijderen** en **Beheersleutel diversifiëren** in.

9. Selecteer onder **Beleid voor gebruikerspincode** de optie **Door de gebruiker opgegeven**.

10. Klik in het linkerdeelvenster op **Beleid vernieuwen &gt; Algemene instellingen wijzigen**. Selecteer **Kaart opnieuw gebruiken bij het vernieuwen** en klik op **OK**.

11. U moet gegevens verzamelings items voor elk beleid uitschakelen door op het beleid in het linkerdeel venster te klikken. Vervolgens schakelt u het selectie vakje naast **voorbeeld gegevens item** in en klikt u op **gegevens verzamelings items verwijderen** en vervolgens op **OK**.

## <a name="prepare-the-cm-app-for-deployment"></a>De CM-app voor de implementatie voorbereiden

1. Voer in de opdracht prompt de volgende opdracht uit om de app uit te pakken. Met de opdracht wordt de inhoud in een nieuwe submap met de naam appx geëxtraheerd en wordt een kopie gemaakt zodat u het oorspronkelijke bestand niet wijzigt.

    ```cmd
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2. Wijzig in de map appx de naam van het bestand CustomDataExample.xml in Custom.data

3. Open het bestand Custom.data en wijzig zo nodig de parameters.


   |                     |                                                                                                                                                                                                          |
   |---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      URL van MIMCM      |                                              De FQDN van de portal die u hebt gebruikt voor het configureren van CM. bijvoorbeeld https://mimcmServerAddress/certificatemanagement                                              |
   |      URL van ADFS       | Als u AD FS gebruikt, voert u de URL van AD FS in. bijvoorbeeld <https://adfsServerSame/adfs> </br> Als ADFS niet wordt gebruikt, configureert u deze instelling met een lege teken reeks.  Bijvoorbeeld ```<ADFS URL=""/>``` |
   |     PrivacyUrl      |                                         U kunt een URL naar een webpagina opnemen waarop wordt uitgelegd wat u doet met de gebruikersgegevens die worden verzameld voor de certificaatinschrijving.                                          |
   |     SupportMail     |                                                                           U kunt een e-mailadres opnemen voor ondersteuningsproblemen.                                                                           |
   | LobComplianceEnable |                                                                     U kunt dit instellen op Waar of Onwaar. Deze waarde is standaard ingesteld op Waar.                                                                      |
   |  MinimumPinLength   |                                                                                        Deze waarde is standaard ingesteld op 6.                                                                                         |
   |      NonAdmin       |           U kunt dit instellen op Waar of Onwaar. Deze waarde is standaard ingesteld op Onwaar. Wijzig deze instelling alleen als u gebruikers die geen beheerder zijn op hun computer in staat wilt stellen om certificaten in te schrijven en te vernieuwen.            |

   > [!IMPORTANT]
   > Er moet een waarde worden opgegeven voor de ADFS-URL. Als er geen waarde is opgegeven, zal de moderne app tijdens het eerste gebruik uitvallen.
4. Sla het bestand op en sluit de editor af.

5. Met de ondertekening van het pakket wordt een handtekeningbestand gemaakt. U moet daarom het oorspronkelijke handtekeningbestand met de naam AppxSignature.p7x verwijderen.

6. In het bestand AppxManifest.xml wordt de onderwerpnaam van het handtekeningcertificaat vermeld. Open dit bestand om het te bewerken.

7. U moet over een handtekeningcertificaat beschikken voordat u aan deze sectie begint. Zie Vernieuwen van smartcard inschakelen voor niet-beheerders in MIM 2016 Certificate Manager, stap 1 hieronder.

8. Wijzig in het element &lt;Identiteit&gt; de waarde van het kenmerk Uitgever zodat deze hetzelfde is als het onderwerp dat wordt vermeld in het handtekeningcertificaat, bijvoorbeeld CN=SUBJECT.

9. Sla het bestand op en sluit de editor af.

10. Voer in het opdrachtpromptvenster de volgende opdracht uit om het appx-bestand opnieuw in te pakken en te ondertekenen.

    ```cmd
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    waarbij app package name dezelfde naam is die u tijdens het maken van de kopie hebt gebruikt.

    ```cmd
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    Hiermee wordt het nieuwe appx-bestand opgegeven. Het pfx-bestand bevat een locatie voor het handtekeningcertificaat en een wachtwoord voor het pfx-bestand.

11. U kunt als volgt met AD FS-verificatie werken:

    - Open de toepassing voor virtuele smartcards. Zo kunt u de waarden die nodig zijn voor de volgende stap gemakkelijker vinden.

    - Als u de toepassing als een client aan de AD FS-server wilt toevoegen en CM op de server wilt configureren, opent u Windows PowerShell op de AD FS-server en voert u de opdracht `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>` uit.

       Hieronder ziet u het script ConfigureMimCMClientAndRelyingParty.ps1:

      ```PowerShell
       # HELP

       <#
       .SYNOPSIS
                       Configure ADFS for CM client app and server.
       .DESCRIPTION
          What the Script does:
                                       a. Registers the MIM CM client app on the ADFS server.
                                       b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                       For parameter information, see 'get-help -detailed'
       .PARAMETER redirectUri
                       The redirectUri for CM client app. Will be added to ADFS server.
                       It can be found as follows:
                       1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                       2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
       .PARAMETER serverFqdn
                       Your deployed MIM CM server’s FQDN.
       .EXAMPLE
          .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
       #>

       # Parameter declaration
       [CmdletBinding()]
       Param(
         [Parameter(Mandatory=$True)]
          [string]$redirectUri,

          [Parameter(Mandatory=$True)]
          [string]$serverFqdn
       )

       Write-Host "Configuring ADFS Objects for OAuth.."

       #Configure SSO to get persistent sign on cookie
       Set-ADFSProperties -SsoLifetime 2880

       #Configure Authentication Policy
       #Intranet to use Kerberos
       #Extranet to use U/P

       #Create Client Objects

       Write-Host "Creating Client Objects..."

       $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

       if ($existingClient -ne $null)
       {
           Write-Host "Found existing instance of the MIM CM Modern App, removing"
           Remove-ADFSClient -TargetName "MIM CM Modern App"
           Write-Host "Client object removed"
       }

       Write-Host "Adding Client Object for MIM CM Modern App client"
       Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
       Write-Host "Client Object for MIM CM Modern App client Created"

       #Create Relying Parties
       Write-Host "Creating Relying Party Objects"

       $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
       if ($existingRp -ne $null)
       {
           Write-Host "Found existing instance of the MIM CM Service RP, removing"
           Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
           Write-Host "RP object Removed"
       }

       $authorizationRules =
       "@RuleTemplate = `"AllowAllAuthzRule`"
       => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

       $issuanceRules =
       "@RuleTemplate = `"LdapClaims`"
       @RuleName = `"Emit UPN and common name`"
       c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
       => issue(store = `"Active Directory`", types =
       (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
       `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
       `";userPrincipalName,cn;{0}`", param = c.Value);

       @RuleTemplate = `"PassThroughClaims`"
       @RuleName = `"Pass through Windows Account Name`"
       c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

       Write-Host "Creating RP Trust for MIM CM Service"
       Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
       Write-Host "RP Trust for MIM CM Service has been created"
       ```

    - Werk de waarden van redirectUri en serverFQDN bij.

    - Als u naar de redirectUri wilt gaan, opent u in de toepassing voor virtuele smartcards het paneel met toepassingsinstellingen, klikt u op **Instellingen**. De omleidings-URI zou in de adresbalk van de AD FS-server moeten worden vermeld. De URI wordt alleen weergegeven als het adres van een AD FS-server is geconfigureerd.

    - De serverFQDN is alleen de volledige computernaam van de MIMCM-server.

    - Voor hulp bij het script **ConfigureMIimCMClientAndRelyingParty. ps1** voert u de volgende handelingen uit: </br> 
      ```Powershell
      get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1
      ```

## <a name="deploy-the-app"></a>De app implementeren

Wanneer u de CM-app instelt, moet u in het Downloadcentrum het bestand MIMDMModernApp_&lt;versie&gt;_AnyCPU_Test.zip downloaden en alle inhoud daarvan uitpakken. Het appx-bestand is het installatieprogramma. U kunt de app implementeren zoals u normaal gesproken met [System Center Configuration Manager](https://technet.microsoft.com/library/dn613840.aspx) of [Intune](https://technet.microsoft.com/library/dn613839.aspx) Windows Store-apps implementeert. Hierbij wordt de app gesideload zodat gebruikers de bedrijfsportal moeten gebruiken om toegang te krijgen tot de app of de app wordt rechtstreeks naar de computers van de gebruikers gepusht.

## <a name="next-steps"></a>Volgende stappen

- [Profiel sjablonen configureren](https://technet.microsoft.com/library/cc708656)
- [Smartcardtoepassingen beheren](https://technet.microsoft.com/library/cc708681)
