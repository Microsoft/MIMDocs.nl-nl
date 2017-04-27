---
title: SharePoint configureren voor Microsoft Identity Manager 2016 | Microsoft Docs
description: Installeer en configureer SharePoint Foundation zodat deze de MIM-portalpagina kan hosten.
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 2af432036033f8914d00228cd3d2d1af84f13054
ms.lasthandoff: 01/24/2017


---

# <a name="set-up-an-identity-management-server-sharepoint"></a>Een server voor identiteitsbeheer instellen: SharePoint

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van de domeincontroller: **mimservername**
> - Domeinnaam: **contoso**
> - Wachtwoord - **Pass@word1**


## <a name="install-sharepoint-foundation-2013-with-sp1"></a>Installeer **SharePoint Foundation 2013 met SP1**

> [!NOTE]
> Voor het installatieprogramma is een internetverbinding vereist zodat de vereiste onderdelen hiervoor kunnen worden gedownload. Als de computer zich op een virtueel netwerk bevindt dat geen verbinding met internet heeft, voegt u een extra netwerkinterface toe aan de computer zodat deze verbinding met internet kan maken. Dit kan worden uitgeschakeld nadat de installatie is voltooid.

Volg onderstaande stappen voor het installeren van SharePoint Foundation 2013 SP1. Wanneer u de installatie hebt voltooid, wordt de server opnieuw gestart.

1.  Start **PowerShell** als een domeinbeheerder.

    -   Ga naar de map waar SharePoint is uitgepakt.

    -   Typ de volgende opdracht:

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Nadat de vereiste onderdelen voor **SharePoint** zijn geïnstalleerd, installeert u **SharePoint Foundation 2013 met SP1** door de volgende opdracht te typen:

    ```
    .\setup.exe
    ```

3.  Selecteer het type Volledige server.

4.  Wanneer de installatie is voltooid, voert u de wizard uit.

## <a name="run-the-wizard-to-configure-sharepoint"></a>De wizard voor het configureren van SharePoint uitvoeren

Voer de stappen uit die in de **wizard Configuratie van SharePoint-producten** worden vermeld, zodat SharePoint zo wordt geconfigureerd dat het met MIM kan samenwerken.

1. Stel de gegevens op het tabblad **Verbinding met een serverfarm maken** in om een nieuwe serverfarm te maken.

2. Geef deze server op als de databaseserver voor de configuratiedatabase en *Contoso\SharePoint* als het databasetoegangsaccount dat door SharePoint moet worden gebruikt.

3. Maak een wachtwoord voor de wachtwoordzin van de farmbeveiliging.

4. Wanneer configuratietaak 10 (van de 10 taken) door de configuratiewizard is voltooid, klikt u op Voltooien en wordt er een webbrowser geopend.

5. Verifieer uzelf in de pop-up van Internet Explorer als *Contoso\Administrator* (of het overeenkomstige domeinbeheerdersaccount) om verder te gaan.

6. Start de wizard (in de web-app) om de SharePoint-farm te configureren.

7. Selecteer de optie om het bestaande beheerde account (*Contoso\SharePoint*) te gebruiken en klik op **Volgende**.

8. Klik in het venster **Een siteverzameling maken** op **Overslaan**.  Klik vervolgens op **Voltooien**.

## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>SharePoint voorbereiden om de MIM-portal te hosten

> [!NOTE]
> In eerste instantie wordt SSL niet geconfigureerd. Configureer SSL of een vergelijkbaar protocol voordat u toegang tot deze portal inschakelt.

1. Start de **SharePoint 2013-beheershell** en voer het volgende PowerShell-script uit om een **SharePoint Foundation 2013-webtoepassing** te maken.

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
    ```

    > [!NOTE]
    > Er wordt een waarschuwing weergegeven waarin wordt vermeld dat de klassieke Windows-verificatiemethode wordt gebruikt en het kan enkele minuten duren voordat de laatste opdracht een waarde retourneert. Wanneer het script is voltooid, wordt in de uitvoer de URL van de nieuwe portal vermeld. Houd het venster **SharePoint 2013-beheershell** geopend zodat u deze later kunt raadplegen.

2. Start de SharePoint 2013-beheershell en voer het volgende PowerShell-script uit om een **SharePoint Foundation 2013-siteverzameling** te maken.

  ```
  $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
  $w = Get-SPWebApplication http://corpidm.contoso.local:82
  New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
  -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
  $s = SpSite($w.Url)
  $s.AllowSelfServiceUpgrade = $false
  $s.CompatibilityLevel
  ```

  > [!NOTE]
  > Controleer of het resultaat van het *compatibiliteitsniveau* de variabele 14 is. Als het resultaat 15 is, is de siteverzameling niet gemaakt voor de 2010-ervaringsversie. Verwijder de siteverzameling en maak deze opnieuw.

3. Schakel **Weergavestatus aan serverzijde voor SharePoint** en de SharePoint-taak Statusanalysetaak (elk uur, Microsoft SharePoint Foundation-timer, alle servers) uit door de volgende PowerShell-opdrachten in de **SharePoint 2013-beheershell** uit te voeren:

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. Open een nieuw browsertabblad op de server voor identiteitsbeheer, navigeer naar http://localhost:82/ en meld u aan als *contoso\Administrator*.  Er wordt een lege SharePoint-site met de naam *MIM-portal* weergegeven.

    ![Afbeelding van de MIM-portal op http://localhost:82/](media/MIM-DeploySP1.png)

5. Kopieer de URL en ga vervolgens in Internet Explorer naar **Internetopties**, open het tabblad **Beveiliging**, selecteer **Lokaal intranet** en klik op **Sites**.

    ![Afbeelding voor Internetopties](media/MIM-DeploySP2.png)

6. Klik in het venster **Lokaal intranet** op **Geavanceerd** en plak de gekopieerde URL in het tekstvak **Deze website aan de zone toevoegen**. Klik op **Toevoegen** en sluit vervolgens de vensters.

7. Open het programma **Systeembeheer**, ga naar **Services** en vervolgens naar de SharePoint-beheerservice en start deze op als deze nog niet wordt uitgevoerd.

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

