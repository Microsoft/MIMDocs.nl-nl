---
title: SharePoint configureren voor Microsoft Identity Manager 2016 | Microsoft Docs
description: Installeer en configureer SharePoint Foundation zodat deze de MIM-portalpagina kan hosten.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 466f5eb7d4aee27336948e15f96087d6ba898170
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358632"
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Een server voor identiteitsbeheer instellen: SharePoint

> [!div class="step-by-step"]
> [«SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
> 
> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van de domeincontroller - **corpdc**
> - Domeinnaam: **contoso**
> - Naam van de MIM-Service-Server - **corpservice**
> - Naam van de MIM-synchronisatieserver - **corpsync**
> - Naam van SQL Server - **corpsql**
> - Wachtwoord - <strong>Pass@word1</strong>


## <a name="install-sharepoint-2016"></a>Installeer **SharePoint 2016**

> [!NOTE]
> Voor het installatieprogramma is een internetverbinding vereist zodat de vereiste onderdelen hiervoor kunnen worden gedownload. Als de computer zich op een virtueel netwerk bevindt dat geen verbinding met internet heeft, voegt u een extra netwerkinterface toe aan de computer zodat deze verbinding met internet kan maken. Dit kan worden uitgeschakeld nadat de installatie is voltooid.

Volg deze stappen voor het installeren van SharePoint 2016. Wanneer u de installatie hebt voltooid, wordt de server opnieuw gestart.

1.  Start **PowerShell** als een domeinaccount met lokale beheerder op de **corpservice** en **sysadmin** op SQL database-server zullen worden gebruikt voor out **contoso\ miminstall**.

    -   Ga naar de map waar SharePoint is uitgepakt.

    -   Typ de volgende opdracht:

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Na **SharePoint** vereiste onderdelen zijn geïnstalleerd, installeert u **SharePoint 2016** door de volgende opdracht te typen:

    ```
    .\setup.exe
    ```

3.  Selecteer het type Volledige server.

4.  Wanneer de installatie is voltooid, voert u de wizard uit.

## <a name="run-the-wizard-to-configure-sharepoint"></a>De wizard voor het configureren van SharePoint uitvoeren

Voer de stappen uit die in de **wizard Configuratie van SharePoint-producten** worden vermeld, zodat SharePoint zo wordt geconfigureerd dat het met MIM kan samenwerken.

1. Stel de gegevens op het tabblad **Verbinding met een serverfarm maken** in om een nieuwe serverfarm te maken.

2. Geef op deze server als de database-server, zoals **corpsql** voor de configuratiedatabase en *Contoso\SharePoint* als het account van de database-toegang voor SharePoint moet worden gebruikt.
3. Maak een wachtwoord voor de wachtwoordzin van de farmbeveiliging.

4. In de configuratie van de Wizard wordt u geadviseerd [MinRole](https://docs.microsoft.com/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server-2016) type **front-end**

5. Wanneer configuratietaak 10 van 10 in de configuratiewizard is voltooid, klikt u op Voltooien en wordt er een web browser wordt geopend...

6. Als u hierom wordt gevraagd de pop-up van Internet Explorer, verifiëren als *Contoso\miminstall* (of het equivalent administrator-account) om door te gaan.

7. Klik in de webwizard (binnen de web-app) op **annuleren/overslaan**.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>SharePoint voorbereiden om de MIM-portal te hosten

> [!NOTE]
> In eerste instantie wordt SSL niet geconfigureerd. Configureer SSL of een vergelijkbaar protocol voordat u toegang tot deze portal inschakelt.

1. Start **SharePoint 2016-beheershell** en voer de volgende PowerShell-script voor het maken van een **webtoepassing voor SharePoint 2016**.

    ```
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Er wordt een waarschuwing weergegeven waarin wordt vermeld dat de klassieke Windows-verificatiemethode wordt gebruikt en het kan enkele minuten duren voordat de laatste opdracht een waarde retourneert. Wanneer het script is voltooid, wordt in de uitvoer de URL van de nieuwe portal vermeld. Houd de **SharePoint 2016-beheershell** venster openen om te verwijzen naar later opnieuw.

2. Open SharePoint 2016 Management Shell en voer de volgende PowerShell-script voor het maken een **SharePoint-siteverzameling** die zijn gekoppeld aan deze webtoepassing.

   ```
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
   ```

   > [!NOTE]
   > Controleer het resultaat van de *compatibiliteitsniveau* variabele is '15'. Als het resultaat is dan '15', klikt u vervolgens de siteverzameling niet gemaakt de versie van de juiste ervaring; Verwijder de siteverzameling en maak deze opnieuw.

3. Uitschakelen **SharePoint Server-Side weergavestatus** en de SharePoint-taak 'Statusanalysetaak (elk uur, Microsoft SharePoint Foundation-Timer, alle Servers)' door het uitvoeren van de volgende PowerShell-opdrachten de  **SharePoint 2016-beheershell**:

   ```
   $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   ```

4. Open op uw server voor identiteitsbeheer, een nieuw tabblad in de webbrowser, gaat u naar http://mim.contoso.com/ en meld u aan als *contoso\miminstall*.  Er wordt een lege SharePoint-site met de naam *MIM-portal* weergegeven.

    ![MIM-Portal op http://mim.contoso.com/ installatiekopie](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. Kopieer de URL en ga vervolgens in Internet Explorer naar **Internetopties**, open het tabblad **Beveiliging**, selecteer **Lokaal intranet** en klik op **Sites**.

    ![Afbeelding voor Internetopties](media/MIM-DeploySP2.png)

6. Klik in het venster **Lokaal intranet** op **Geavanceerd** en plak de gekopieerde URL in het tekstvak **Deze website aan de zone toevoegen**. Klik op **Toevoegen** en sluit vervolgens de vensters.

7. Open het programma **Systeembeheer**, ga naar **Services** en vervolgens naar de SharePoint-beheerservice en start deze op als deze nog niet wordt uitgevoerd.

> [!div class="step-by-step"]  
> [«SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
