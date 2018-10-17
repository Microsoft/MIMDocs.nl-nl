---
title: Configureren van WindowsServer 2016 voor MIM 2016 SP1 | Microsoft Docs
description: Lees de stappen en de minimale vereisten voor het voorbereiden van Windows Server 2016 om te werken met MIM 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 49e549913a5fd87528df2205b8d5b0a83f3d2b24
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358241"
---
# <a name="set-up-an-identity-management-servers-windows-server-2016"></a>Een identity management servers instellen: Windows Server 2016

> [!div class="step-by-step"]
> [«Een domein voorbereiden](preparing-domain.md)
> [SQL Server 2016»](prepare-server-sql2016.md)
> 
> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van de domeincontroller - **corpdc**
> - Domeinnaam: **contoso**
> - Naam van de MIM-Service-Server - **corpservice**
> - Naam van de MIM-synchronisatieserver - **corpsync**
> - Naam van SQL Server - **corpsql**
> - Wachtwoord - <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>Windows Server 2016 toevoegen aan uw domein

Beginnen met een computer met Windows Server 2016, met een minimum van 8 tot 12GB aan RAM-geheugen. Geef bij de installatie 'Windows Server 2016 Standard/Datacenter (Server met een GUI) x 64' edition.

1. Meld u als beheerder aan bij de nieuwe computer.

2. Geef in Configuratiescherm de computer een statisch IP-adres op het netwerk. Configureer de netwerkinterface voor het verzenden van DNS-query's naar het IP-adres van de domeincontroller in de vorige stap en stel de computernaam op **CORPSERVICE**.  Hiervoor moet de server opnieuw worden opgestart.

3. Open het Configuratiescherm en de computer koppelen aan het domein dat u hebt geconfigureerd in de laatste stap *contoso.com*.  U moet hiervoor de gebruikersnaam en referenties van een domeinbeheerder, zoals *Contoso\Administrator*, opgeven.  Nadat het welkomstbericht wordt weergegeven, sluit u het dialoogvenster en start u deze server opnieuw op.

4. Meld u aan bij de computer *CORPSERVICE* als een domeinaccount met de beheerder van de lokale computer, zoals *Contoso\MIMINSTALL*.


5. Start als beheerder een PowerShell-venster en typ de volgende opdracht om de computer bij te werken met de groepsbeleidsinstellingen.

    ```
    gpupdate /force /target:computer
    ```

    Na maximaal een minuut wordt de taak voltooid met het bericht Het bijwerken van het computerbeleid is voltooid.

6. Voeg de functies van de **webserver (IIS)** en de **toepassingsserver**, de onderdelen **.NET Framework** 3.5, 4.0 en 4.5, en de **Active Directory-module voor Windows PowerShell** toe.

    ![Afbeelding voor de PowerShell-functies](media/MIM-DeployWS2.png)

7. Typ in PowerShell de volgende opdrachten: U moet mogelijk een andere locatie opgeven voor de bronbestanden voor de **.NET Framework** 3.5-onderdelen. Deze onderdelen zijn meestal niet aanwezig wanneer Windows Server wordt geïnstalleerd, maar zijn beschikbaar in de map SxS (side-by-side), die deel uitmaakt van de map Bronnen op de installatieschijf voor het besturingssysteem, bijvoorbeeld: *d:\Sources\SxS\*.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>Het beveiligingsbeleid van de server configureren

Stel het beveiligingsbeleid van de server zo in dat de zojuist gemaakte accounts als services kunnen worden uitgevoerd.
> [!NOTE] 
> Afhankelijk van de configuratie van één server(all-in-one) of server voor gedistribueerde u alleen hoeft toe te voegen op basis van rol van de lidcomputer zoals synchronisatieserver. 

1. Start het programma Lokaal beveiligingsbeleid

2. Navigeer naar **Lokaal beleid > Toewijzing van gebruikersrechten**.

3. Klik in het detailvenster met de rechtermuisknop op **Aanmelden als service** en selecteer **Eigenschappen**.

    ![Afbeelding voor Lokaal beveiligingsbeleid](media/MIM-DeployWS3.png)

4. Klik op **gebruiker of groep toevoegen**, en in het tekstvak te volgen op basis van rol `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, klikt u op **namen controleren**, en klikt u op **OK**.

5. Klik op **OK** om het venster **Aanmelden als service > Eigenschappen** te sluiten.

6.  In het detailvenster, klik met de rechtermuisknop op **toegang tot deze computer vanaf het netwerk weigeren**, en selecteer **eigenschappen**. >

[!NOTE] Als de rol van de afzonderlijke servers in deze stap wordt verbroken bepaalde functionaliteit, zoals de SSPR-functie.

7. Klik op **Gebruiker of groep toevoegen**, typ `contoso\MIMSync; contoso\MIMService` in het tekstvak en klik op **OK**.

8. Klik op **OK** om het venster **Toegang tot deze computer vanaf het netwerk weigeren > Eigenschappen** te sluiten.

9. Klik in het detailvenster met de rechtermuisknop op **Lokaal aanmelden weigeren** en selecteer **Eigenschappen**.

10. Klik op **Gebruiker of groep toevoegen**, typ `contoso\MIMSync; contoso\MIMService` in het tekstvak en klik op **OK**.

11. Klik op **OK** om het venster **Lokaal aanmelden weigeren > Eigenschappen** te sluiten.

12. Sluit het venster Lokaal beveiligingsbeleid.


## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Indien nodig, de Windows-verificatie voor IIS-modus wijzigen

1.  Open een PowerShell-venster.

2.  Stop IIS met de opdracht *iisreset /STOP*

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [«Een domein voorbereiden](preparing-domain.md)
> [SQL Server 2016»](prepare-server-sql2016.md)
