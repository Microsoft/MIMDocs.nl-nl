---
title: PAM implementeren - Stap 3 - PAM-server | Microsoft Docs
description: Een PAM-server voorbereiden die fungeert als host voor SQL en SharePoint van uw Privileged Access Management-implementatie.
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 618b834452aa07a9f31582994fe32129a49f4249


---

# <a name="step-3-prepare-a-pam-server"></a>Stap 3 – Een PAM-server voorbereiden

>[!div class="step-by-step"]
[« Stap 2](step-2-prepare-priv-domain-controller.md)
[Stap 4 »](step-4-install-mim-components-on-pam-server.md)

## <a name="install-windows-server-2012-r2"></a>Windows Server 2012 R2 installeren
Installeer op een derde virtuele machine Windows Server 2012 R2, meer specifiek Windows Server 2012 R2 Standard (server met een GUI) x64, om *PAMSRV* te maken. Omdat SQL Server en SharePoint 2013 worden geïnstalleerd op deze computer, is er ten minst 8 GB aan RAM-geheugen vereist.

1. Selecteer **Windows Server 2012 R2 Standard (server met een GUI) x64**.

    ![Windows Server Standard met GUI kiezen - schermafbeelding](media/PAM_GS_Select_WS2012.png)

2. Lees en accepteer de licentievoorwaarden.

3.  Omdat de schijf leeg is, selecteert u **Aangepast: alleen Windows installeren** en gebruikt u de **niet-geïnitialiseerde schijfruimte**.

4.  Meld u als beheerder aan bij deze nieuwe computer.  Gebruik Configuratiescherm om een statisch IP-adres op het virtuele netwerk toe te wijzen aan de computer, configureer deze netwerkinterface voor het verzenden van DNS-query's naar het IP-adres van PRIVDC en stel de computernaam in op *PAMSRV*.  Hiervoor moet de server opnieuw worden opgestart.

5.  Als het virtuele netwerk geen verbinding met internet heeft, voegt u een extra netwerkinterface toe aan de computer zodat deze verbinding met internet kan maken.  Dit nodig is voor de installatie van SharePoint en de interface kan worden uitgeschakeld nadat deze stap is voltooid.

6.  Nadat de server opnieuw is opgestart, moet u zich aanmelden als beheerder. Via Configuratiescherm configureert u de computer om te controleren op updates en installeert u alle vereiste updates.  Hiervoor moet de server mogelijk opnieuw worden opgestart.

7.  Nadat de server opnieuw is opgestart, meldt u zich aan als beheerder, opent u Configuratiescherm en voegt u PAMSRV toe aan het PRIV-domein (priv.contoso.local).  U moet hiervoor de gebruikersnaam en referenties opgeven van een PRIV-domeinbeheerder (PRIV\Administrator). Nadat het welkomstbericht wordt weergegeven, sluit u het dialoogvenster en start u deze server opnieuw op.


### <a name="add-the-web-server-iis-and-application-server-roles"></a>De functies van de webserver (IIS) en de toepassingsserver toevoegen
Voeg de functies van de webserver (IIS) en de toepassingsserver, de onderdelen van .NET Framework 3.5, de Active Directory-module voor Windows PowerShell en andere functies toe die zijn vereist voor SharePoint.

1.  Meld u aan als een PRIV-domeinbeheerder (PRIV\Administrator) en start PowerShell.

2.  Typ de volgende opdrachten: U moet mogelijk een andere locatie opgeven voor de bronbestanden voor de .NET Framework 3.5-onderdelen. Deze onderdelen zijn meestal niet aanwezig wanneer Windows Server wordt geïnstalleerd, maar zijn beschikbaar in de map SxS (side-by-side), die deel uitmaakt van de map Bronnen op de installatieschijf voor het besturingssysteem, bijvoorbeeld: *d:\Sources\SxS.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### <a name="configure-the-server-security-policy"></a>Het beveiligingsbeleid van de server configureren
Configureer het beveiligingsbeleid van de server zodanig dat de zojuist gemaakte accounts als services kunnen worden uitgevoerd.

1.  Start het programma **Lokaal beveiligingsbeleid**.   
2.  Navigeer naar **Lokaal beleid**  > **Toewijzing van gebruikersrechten**.  
3.  Klik in het detailvenster met de rechtermuisknop op **Aanmelden als service** en selecteer **Eigenschappen**.  
4.  Klik op **Gebruiker of groep toevoegen** en voer bij Gebruiker en groepsnamen *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer* in. Klik op **Namen controleren** en vervolgens op **OK**.  

5.  Klik op **OK** om het venster Eigenschappen te sluiten.  
6.  Klik in het detailvenster met de rechtermuisknop op **Toegang tot deze computer vanaf het netwerk weigeren** en selecteer **Eigenschappen**.  
7.  Klik op **Gebruiker of groep toevoegen**, voer bij Gebruiker en groepsnamen *priv\mimmonitor; priv\MIMService; priv\mimcomponent* in en klik op **OK**.  
8.  Klik op **OK** om het venster Eigenschappen te sluiten.  

9. Klik in het detailvenster met de rechtermuisknop op **Lokaal aanmelden weigeren** en selecteer **Eigenschappen**.  
10. Klik op **Gebruiker of groep toevoegen**, voer bij Gebruiker en groepsnamen *priv\mimmonitor; priv\MIMService; priv\mimcomponent* in en klik op **OK**.  
11. Klik op **OK** om het venster Eigenschappen te sluiten.  
12. Sluit het venster Lokaal beveiligingsbeleid.  

13. Open Configuratiescherm en schakel over naar **Gebruikersaccounts**.  
14. Klik op **Andere gebruikers toegang tot deze computer geven **.  
15. Klik op **Toevoegen**, voer de gebruiker *MIMADMIN* in het domein *PRIV* in en klik in het volgende scherm van de wizard op **Deze gebruiker toevoegen als beheerder**.  
16. Klik op **Toevoegen**, voer de gebruiker *SharePoint* in het domein *PRIV* in en klik in het volgende scherm van de wizard op **Deze gebruiker toevoegen als beheerder**.  
17. Sluit Configuratiescherm.  

### <a name="change-the-iis-configuration"></a>IIS-configuratie wijzigen
Er zijn twee manieren waarop u de IIS-configuratie kunt wijzigen zodat toepassingen kunnen gebruikmaken van de Windows-verificatiemodus. Zorg dat u bent aangemeld als MIMAdmin en gebruik vervolgens een van deze opties.

Als u PowerShell wilt gebruiken:
1.  Klik met de rechtermuisknop op PowerShell en selecteer **Als administrator uitvoeren**.  
2.  Stop IIS en ontgrendel de instellingen voor de toepassingshost met de volgende opdrachten.  
    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

Als u een teksteditor wilt gebruiken zoals Kladblok:   
1. Open het bestand **C:\Windows\System32\inetsrv\config\applicationHost.config**   
2. Schuif naar regel 82 van het bestand. De labelwaarde van **overrideModeDefault** moet **<section name="windowsAuthentication" overrideModeDefault="Deny" />** zijn.  
3. Wijzig de waarde van **overrideModeDefault** in *Allow*.  
4. Sla het bestand op en start IIS opnieuw met de PowerShell-opdracht `iisreset /START`.

## <a name="install-sql-server"></a>SQL Server installeren
Als SQL Server nog niet aanwezig is in de bastionomgeving, installeert u SQL Server 2012 (Service Pack 1 of hoger) of SQL Server 2014. In de volgende stappen wordt ervan uitgegaan dat u SQL Server 2014 gebruikt.

1. Zorg dat u bent aangemeld als MIMAdmin.
2. Klik met de rechtermuisknop op PowerShell en selecteer **Als administrator uitvoeren**.   
3. Ga naar de map met het installatieprogramma van SQL Server.  
4. Typ de volgende opdracht:  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1" /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## <a name="install-sharepoint-foundation-2013"></a>SharePoint Foundation 2013 installeren

Gebruik het installatieprogramma van SharePoint Foundation 2013 met Service Pack 1 om de vereiste software van SharePoint te installeren op PAMSRV.

> [!NOTE]
> Voor het installatieprogramma is een internetverbinding vereist zodat de vereiste onderdelen kunnen worden gedownload. Nadat de vereiste onderdelen zijn geïnstalleerd, wordt de server opnieuw opgestart.

1. Klik met de rechtermuisknop op PowerShell en selecteer **Als administrator uitvoeren**.  
2. Ga naar de map waar SharePoint is uitgepakt.  
3. Typ de opdracht `.\prerequisiteinstaller.exe`.

Nadat de vereiste onderdelen voor SharePoint zijn geïnstalleerd, installeert u SharePoint Foundation 2013 met SP1.

1.  Klik met de rechtermuisknop op PowerShell en selecteer **Als administrator uitvoeren**.  
2.  Ga naar de map waar SharePoint is uitgepakt.  
3.  Typ de opdracht `.\setup.exe`.  
4.  Selecteer het type voor de **volledige server**.  
5.  Voer de wizard uit nadat de installatie is voltooid.  

### <a name="configure-sharepoint"></a>SharePoint configureren
Voer de wizard Configuratie van SharePoint-producten uit.

1.  Schakel op het tabblad Verbinding met een serverfarm over naar **Een nieuwe serverfarm maken**.  
2.  Geef **PAMSRV** op als de databaseserver voor de configuratiedatabase en **PRIV\SharePoint** als het account voor databasetoegang dat voor SharePoint moet worden gebruikt.  
3.  Geef een wachtwoord op als wachtwoordzin voor beveiliging van de farm (wordt verder niet gebruikt in dit overzicht).  
4.  Accepteer voorlopig de overige standaardinstellingen van de configuratiewizard van SharePoint om een farm met één server te maken.    
5.  Nadat configuratietaak 10 van 10 is voltooid met de wizard, klikt u op **Voltooien** waarna een webbrowser wordt geopend.  
6.  Voer in de pop-up van Internet Explorer de verificatie als domeinbeheerder (PRIV\MIMAdmin) uit om door te gaan.  
7.  Start de wizard in de web-app om de SharePoint-farm te configureren.  
8.  Geef aan dat u het bestaande beheerde account (PRIV\SharePoint) wilt gebruiken, schakel de optionele services uit en klik op **Volgende**.  
9. Klik in het venster Een siteverzameling maken op **Overslaan** en **Voltooien**.  

## <a name="create-a-sharepoint-foundation-2013-web-application"></a>Een SharePoint Foundation 2013-webtoepassing maken
Nadat de wizard is voltooid, gebruikt u PowerShell om een SharePoint Foundation 2013-webtoepassing te maken waarmee de MIM-portal wordt gehost. Omdat dit overzicht alleen is bedoeld voor demonstratiedoeleinden, wordt SSL niet ingeschakeld.

1.  Klik met de rechtermuisknop op de SharePoint 2013-beheershell, selecteer **Als administrator uitvoeren** en voer het volgende PowerShell-script uit:

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"            -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. Er wordt een waarschuwing weergegeven waarin wordt aangegeven dat de klassieke Windows-verificatiemethode wordt gebruikt en dat het enkele minuten kan duren voordat de laatste opdracht een waarde retourneert.  Nadat het script is voltooid, wordt in de uitvoer de URL van de nieuwe portal aangegeven.

> [!NOTE]
> Houd het venster SharePoint 2013-beheershell geopend zodat u dit in de volgende stap kunt gebruiken.

## <a name="create-a-sharepoint-site-collection"></a>Een SharePoint-siteverzameling maken
Vervolgens moet u een SharePoint-siteverzameling maken die is gekoppeld aan deze webtoepassing waarmee de MIM-portal wordt gehost.

1.  Start **SharePoint 2013-beheershell**als het venster nog niet is geopend en voer het volgende PowerShell-script uit.

    ```
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    Controleer of de variabele **CompatibilityLevel** is ingesteld op *14*. Als de waarde *15* wordt geretourneerd, is de siteverzameling niet gemaakt voor de 2010-ervaringsversie en moet u de siteverzameling verwijderen en opnieuw maken.

2.  Voer de volgende PowerShell-opdrachten uit in de **SharePoint 2013-beheershell**. Hiermee worden de weergavestatus op de SharePoint-server en de SharePoint-taak **Statusanalysetaak (Elk uur, Microsoft SharePoint Foundation-timer, Alle servers)** uitgeschakeld.

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

## <a name="change-update-settings"></a>Instellingen voor updates wijzigen

1. Open Configuratiescherm, navigeer naar **Windows Update** en klik op **instellingen wijzigen**.  
2. Wijzig de instellingen om updates te ontvangen van Windows Update en andere Microsoft Update-producten.  
3. Controleren of er nieuwe updates beschikbaar zijn en zorg dat de beschikbare belangrijke updates zijn geïnstalleerd voordat u doorgaat.

## <a name="set-the-website-as-the-local-intranet"></a>De website instellen als het lokale intranet

1. Start Internet Explorer en open een nieuw tabblad in de webbrowser.
2. Navigeer naar http://pamsrv.priv.contoso.local:82/ en meld u aan als PRIV\MIMAdmin.  Er wordt een lege SharePoint-site met de naam MIM-portal weergegeven.  
3. Open in Internet Explorer het gedeelte **Internetopties**, ga naar het tabblad **Beveiliging**, selecteer **Lokaal intranet** en voeg de URL `http://pamsrv.priv.contoso.local:82/` toe.

Als het aanmelden mislukt, moeten de Kerberos-SPN-namen die eerder zijn gemaakt in [stap 2](step-2-prepare-priv-domain-controller.md), mogelijk worden bijgewerkt.

## <a name="start-the-sharepoint-administration-service"></a>De SharePoint-beheerservice starten

Gebruik **Services** (in Systeembeheer) om de **SharePoint-beheerservice** te starten als de service nog niet actief is.

In step 4 begint u met het installeren van de MIM-onderdelen op de PAM-server.

>[!div class="step-by-step"]
[« Stap 2](step-2-prepare-priv-domain-controller.md)
[Stap 4 »](step-4-install-mim-components-on-pam-server.md)



<!--HONumber=Nov16_HO2-->


