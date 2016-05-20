---
# required metadata

title: Een server voor identiteitsbeheer instellen: Windows Server 2012 R2 | Microsoft Identity Manager
description: Lees welke stappen moeten worden uitgevoerd en wat de minimale vereisten zijn voor het voorbereiden van Windows Server 2012 RS zodat dit kan samenwerken met MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Een server voor identiteitsbeheer instellen: Windows Server 2012 R2

>[!div class=stapsgewijs]
[« Een domein voorbereiden](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van de domeincontroller: **mimservername**
> - Domeinnaam: **contoso**
> - Wachtwoord: **Pass@word1**

## Windows Server 2012 R2 aan uw domein koppelen

Begin met een Windows Server 2012 R2-computer, met minimaal 8 GB aan RAM-geheugen. Geef bij de installatie de editie Windows Server 2012 R2 Standard x64 (server met een GUI) op.

1. Meld u als beheerder aan bij de nieuwe computer.

2. Geef in Configuratiescherm de computer een statisch IP-adres op het netwerk. Configureer de netwerkinterface zo dat deze DNS-query's naar het IP-adres van de domeincontroller uit de vorige stap verzendt en stel de computer in op **CORPIDM**.  Hiervoor moet de server opnieuw worden opgestart.

3. Open Configuratiescherm en voeg de computer toe aan het domein, *contoso.local*, dat u in de laatste stap hebt geconfigureerd.  U moet hiervoor de gebruikersnaam en referenties van een domeinbeheerder, zoals *Contoso\Administrator*, opgeven.  Nadat het welkomstbericht wordt weergegeven, sluit u het dialoogvenster en start u deze server opnieuw op.

4. Meld u aan bij de computer *CorpIDM* als een domeinbeheerder, zoals *Contoso\Administrator*.

5. Start als beheerder een PowerShell-venster en typ de volgende opdracht om de computer bij te werken met de groepsbeleidsinstellingen.

    ```
    gpupdate /force /target:computer
    ```

    Na maximaal een minuut wordt de taak voltooid met het bericht Het bijwerken van het computerbeleid is voltooid.

6. Voeg de functies van de **webserver (IIS)** en de **toepassingsserver**, de onderdelen **.NET Framework** 3.5, 4.0 en 4.5, en de **Active Directory-module voor Windows PowerShell** toe.

    ![Afbeelding voor de PowerShell-functies](media/MIM-DeployWS2.png)

7. Typ in PowerShell de volgende opdrachten: U moet mogelijk een andere locatie opgeven voor de bronbestanden voor de **.NET Framework** 3.5-onderdelen. Deze onderdelen zijn meestal niet aanwezig wanneer Windows Server wordt geïnstalleerd, maar zijn beschikbaar in de map SxS (side-by-side), die onderdeel uitmaakt van de map Bronnen op de installatieschijf voor het besturingssysteem, bijvoorbeeld: *d:\Bronnen\SxS\*.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## Het beveiligingsbeleid van de server configureren

Stel het beveiligingsbeleid van de server zo in dat de zojuist gemaakte accounts als services kunnen worden uitgevoerd.

1. Start het programma Lokaal beveiligingsbeleid

2. Navigeer naar **Lokaal beleid > Toewijzing van gebruikersrechten**.

3. Klik in het detailvenster met de rechtermuisknop op **Aanmelden als service** en selecteer **Eigenschappen**.

    ![Afbeelding voor Lokaal beveiligingsbeleid](media/MIM-DeployWS3.png)

4. Klik op **Gebruiker of groep toevoegen** en typ `contoso\mimsync; contoso\mimma; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\mimsspr` in het tekstvak, klik op **Namen controleren** en klik op **OK**.

5. Klik op **OK** om het venster **Aanmelden als service > Eigenschappen** te sluiten.

6.  Klik in het detailvenster met de rechtermuisknop op **Toegang tot deze computer vanaf het netwerk weigeren** en selecteer **Eigenschappen**.

7. Klik op **Gebruiker of groep toevoegen**, typ `contoso\MIMSync; contoso\MIMService` in het tekstvak en klik op **OK**.

8. Klik op **OK** om het venster **Toegang tot deze computer vanaf het netwerk weigeren > Eigenschappen** te sluiten.

9. Klik in het detailvenster met de rechtermuisknop op **Lokaal aanmelden weigeren** en selecteer **Eigenschappen**.

10. Klik op **Gebruiker of groep toevoegen**, typ `contoso\MIMSync; contoso\MIMService` in het tekstvak en klik op **OK**.

11. Klik op **OK** om het venster **Lokaal aanmelden weigeren > Eigenschappen** te sluiten.

12. Sluit het venster Lokaal beveiligingsbeleid.


## De Windows-verificatiemodus voor IIS wijzigen

1.  Open een PowerShell-venster.

2.  Stop IIS met de opdracht *iisreset /STOP*

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>[!div class=stapsgewijs]  
[« Een domein voorbereiden](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)


<!--HONumber=Apr16_HO4-->


