---
title: SharePoint configureren voor Microsoft Identity Manager 2016 | Microsoft Docs
description: Installeer en configureer SharePoint Foundation zodat deze de MIM-portalpagina kan hosten.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 62ef8796717dbcaea18d21bc3d28248efdeef92e
ms.sourcegitcommit: 323c2748dcc6b6991b1421dd8e3721588247bc17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73568111"
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Een server voor identiteitsbeheer instellen: SharePoint

> [!div class="step-by-step"]
> [«SQL Server](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
> 

> [!NOTE]
> De installatie procedure van share Point server 2019 verschilt niet van de installatie procedure van share Point server 2016 **, met uitzonde ring** van een extra stap die moet worden uitgevoerd voor het deblokkeren van de bestanden die worden gebruikt door de MIM-Portal

> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van domein controller- **corpdc**
> - Domeinnaam: **contoso**
> - Naam van de MIM-service Server- **corpservice**
> - Naam MIM-synchronisatie server- **corpsync**
> - SQL Server naam- **corpsql**
> - Wachtwoord - <strong>Pass@word1</strong>


## <a name="install-sharepoint-2016"></a>**Share point 2016** installeren

> [!NOTE]
> Voor het installatieprogramma is een internetverbinding vereist zodat de vereiste onderdelen hiervoor kunnen worden gedownload. Als de computer zich op een virtueel netwerk bevindt dat geen verbinding met internet heeft, voegt u een extra netwerkinterface toe aan de computer zodat deze verbinding met internet kan maken. Dit kan worden uitgeschakeld nadat de installatie is voltooid.

Volg deze stappen om share point 2016 te installeren. Wanneer u de installatie hebt voltooid, wordt de server opnieuw gestart.

1.  Start **Power shell** als een domein account met een lokale beheerder op de **corpservice** en **sysadmin** op SQL database server gaan we **contoso\miminstall**gebruiken.

    -   Ga naar de map waar SharePoint is uitgepakt.

    -   Typ de volgende opdracht:
    ```
    .\prerequisiteinstaller.exe
    ```

2.  Nadat de vereiste onderdelen voor **share point** zijn geïnstalleerd, installeert u **share point 2016** door de volgende opdracht te typen:

    ```
    .\setup.exe
    ```

3.  Selecteer het type Volledige server.

4.  Wanneer de installatie is voltooid, voert u de wizard uit.

## <a name="run-the-wizard-to-configure-sharepoint"></a>De wizard voor het configureren van SharePoint uitvoeren

Voer de stappen uit die in de **wizard Configuratie van SharePoint-producten** worden vermeld, zodat SharePoint zo wordt geconfigureerd dat het met MIM kan samenwerken.

1. Stel de gegevens op het tabblad **Verbinding met een serverfarm maken** in om een nieuwe serverfarm te maken.

2. Geef deze server op als de database server als **corpsql** voor de configuratie database en *Contoso\SharePoint* als het database toegangs account dat door share point moet worden gebruikt.
3. Maak een wachtwoord voor de wachtwoordzin van de farmbeveiliging.

4. In de configuratie wizard raden we u aan om [MinRole](/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server) type **front-end** te selecteren

5. Wanneer configuratie taak 10 van 10 door de configuratie wizard is voltooid, klikt u op volt ooien en wordt er een webbrowser geopend.

6. Als u wordt gevraagd om de pop-up van Internet Explorer, verifieer als *Contoso\miminstall* (of het equivalente beheerders account) om door te gaan.

7. Klik in de wizard Web (in de web-app) op **Annuleren/overs Laan**.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>SharePoint voorbereiden om de MIM-portal te hosten

> [!NOTE]
> In eerste instantie wordt SSL niet geconfigureerd. Configureer SSL of een vergelijkbaar protocol voordat u toegang tot deze portal inschakelt.

1. Start **share point 2016-beheer shell** en voer het volgende Power shell-script uit om een **share point 2016-webtoepassing**te maken.

    ```PowerShell
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Er wordt een waarschuwing weergegeven waarin wordt vermeld dat de klassieke Windows-verificatiemethode wordt gebruikt en het kan enkele minuten duren voordat de laatste opdracht een waarde retourneert. Wanneer het script is voltooid, wordt in de uitvoer de URL van de nieuwe portal vermeld. Houd het **share point 2016 Management Shell-** venster geopend om later naar referentie te verwijzen.

2. Start share point 2016-beheer shell en voer het volgende Power shell-script uit om een **share point-site verzameling** te maken die aan die webtoepassing is gekoppeld.
    ```PowerShell
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
    ```
    > [!NOTE]
    > Controleer of het resultaat van de variabele *compatibiliteits niveau* ' 15 ' is. Als het resultaat geen ' 15 ' is, is de site verzameling niet de juiste ervarings versie gemaakt. Verwijder de site verzameling en maak deze opnieuw.

    > [!IMPORTANT]
    > Share Point server 2019 maakt gebruik van een andere eigenschap voor een webtoepassing om een lijst met geblokkeerde bestands extensies te gebruiken. Daarom kunt u de blok kering opheffen. ASHX-bestanden die worden gebruikt door de MIM-Portal drie extra opdrachten moeten hand matig worden uitgevoerd vanuit de share point-beheer shell.
    <br/>
    **Voer de volgende drie opdrachten alleen uit voor share point 2019:**

    ```PowerShell
    $w.BlockedASPNetExtensions.Remove("ashx")
    $w.Update()
    $w.BlockedASPNetExtensions
    ```
   > [!NOTE]
   > Controleer of de *BlockedASPNetExtensions* -lijst geen ashx-extensie bevat, anders kunnen niet meerdere MIM-Portal pagina's correct worden weer gegeven.


3. Schakel **weergave status van share Point server** en de share point-taak Health Analysis (elk uur, micro soft share point Foundation-timer, alle servers) uit door de volgende Power shell-opdrachten uit te voeren in de **share point 2016-beheer shell**:

   ```PowerShell
   $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   ```

4. Open op uw server voor identiteits beheer een nieuw tabblad in de webbrowser, navigeer naar http://mim.contoso.com/ en meld u aan als *contoso\miminstall*.  Er wordt een lege SharePoint-site met de naam *MIM-portal* weergegeven.

    ![MIM-Portal op http://mim.contoso.com/ -installatie kopie](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. Kopieer de URL en ga vervolgens in Internet Explorer naar **Internetopties**, open het tabblad **Beveiliging**, selecteer **Lokaal intranet** en klik op **Sites**.

    ![Afbeelding voor Internetopties](media/MIM-DeploySP2.png)

6. Klik in het venster **Lokaal intranet** op **Geavanceerd** en plak de gekopieerde URL in het tekstvak **Deze website aan de zone toevoegen**. Klik op **Toevoegen** en sluit vervolgens de vensters.

7. Open het programma **Systeembeheer**, ga naar **Services** en vervolgens naar de SharePoint-beheerservice en start deze op als deze nog niet wordt uitgevoerd.

> [!div class="step-by-step"]  
> [«SQL Server](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
