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
ms.openlocfilehash: a0fa1e837fd73872043748ee73f19a29d1d1412f
ms.sourcegitcommit: 3b514aba69af203f176b40cdb7c2a51c477c944a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51718335"
---
# <a name="set-up-an-identity-management-server-windows-server-2016"></a>Een server voor identiteitsbeheer instellen: Windows Server 2016

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

2. Geef in Configuratiescherm de computer een statisch IP-adres op het netwerk. Configureer de netwerkinterface voor het verzenden van DNS-query's naar het IP-adres van de domeincontroller in de vorige stap en stel de computernaam op **CORPSERVICE**.  Deze bewerking wordt een server opnieuw opstarten vereist.

3. Open het Configuratiescherm en de computer koppelen aan het domein dat u hebt geconfigureerd in de laatste stap *contoso.com*.  Met deze bewerking bevat die de gebruikersnaam en referenties van een domeinbeheerder, zoals *Contoso\Administrator*.  Nadat het welkomstbericht wordt weergegeven, sluit u het dialoogvenster en start u deze server opnieuw op.

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
> Afhankelijk van uw configuratie, één server(all-in-one) of gedistribueerde servers u alleen hoeft toe te voegen, op basis van rol van de lidcomputer, zoals synchronisatieserver. 

1. Start het programma Lokaal beveiligingsbeleid

2. Navigeer naar **Lokaal beleid > Toewijzing van gebruikersrechten**.

3. In het detailvenster met de rechtermuisknop op **aanmelden als een service**, en selecteer **eigenschappen**.

    ![Afbeelding voor Lokaal beveiligingsbeleid](media/MIM-DeployWS3.png)

4. Klik op **gebruiker of groep toevoegen**, en in het tekstvak te volgen op basis van rol `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, klikt u op **namen controleren**, en klikt u op **OK**.

5. Klik op **OK** om het venster **Aanmelden als service > Eigenschappen** te sluiten.

6.  In het detailvenster met de rechtermuisknop op **toegang tot deze computer vanaf het netwerk weigeren**, en selecteer **eigenschappen**. >

[!NOTE] Scheiden van de rol van servers, worden sommige functies, zoals SSPR verbroken.

7. Klik op **Gebruiker of groep toevoegen**, typ `contoso\MIMSync; contoso\MIMService` in het tekstvak en klik op **OK**.

8. Klik op **OK** om het venster **Toegang tot deze computer vanaf het netwerk weigeren > Eigenschappen** te sluiten.

9. In het detailvenster met de rechtermuisknop op **lokaal aanmelden weigeren**, en selecteer **eigenschappen**.

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
