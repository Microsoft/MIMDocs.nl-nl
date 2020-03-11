---
title: Windows Server 2016 of 2019 configureren voor MIM 2016 SP2 | Microsoft Docs
description: De stappen en minimale vereisten ophalen voor het voorbereiden van Windows Server 2016 of 2019 voor gebruik met MIM 2016 SP2.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cf8261c4e6f6529fd82760206b62b689a75d0acb
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044142"
---
# <a name="set-up-an-identity-management-server-windows-server-2016-or-2019"></a>Een server voor identiteits beheer instellen: Windows Server 2016 of 2019

> [!div class="step-by-step"]
> [«Voorbereiden van een domein](preparing-domain.md)
> [SQL Server»](prepare-server-sql2016.md)
> 

> [!NOTE]
> De installatie procedure van Windows Server 2019 verschilt niet van de installatie procedure van Windows Server 2016.


> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van domein controller- **corpdc**
> - Domeinnaam: **contoso**
> - Naam van de MIM-service Server- **corpservice**
> - Naam MIM-synchronisatie server- **corpsync**
> - SQL Server naam- **corpsql**
> - Wachtwoord - <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>Windows Server 2016 toevoegen aan uw domein

Begin met een Windows Server 2016-computer, met mini maal 8 GB RAM-geheugen. Geef bij de installatie de editie Windows Server 2016 Standard/Data Center (server met een GUI) x64 op.

1. Meld u als beheerder aan bij de nieuwe computer.

2. Geef in Configuratiescherm de computer een statisch IP-adres op het netwerk. Configureer die netwerk interface voor het verzenden van DNS-query's naar het IP-adres van de domein controller in de vorige stap en stel de computer naam in op **CORPSERVICE**.  Voor deze bewerking moet de server opnieuw worden opgestart.

3. Open het configuratie scherm en voeg de computer toe aan het domein dat u in de laatste stap hebt geconfigureerd, *contoso.com*.  Met deze bewerking wordt de gebruikers naam en referenties van een domein beheerder, zoals *Contoso\Administrator*, verstrekt.  Nadat het welkomstbericht wordt weergegeven, sluit u het dialoogvenster en start u deze server opnieuw op.

4. Meld u aan bij de computer *CORPSERVICE* als een domein account met een lokale computer beheerder, zoals *Contoso\MIMINSTALL*.


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
> Afhankelijk van uw configuratie, één server (alles-in-één) of gedistribueerde servers die u alleen moet toevoegen, op basis van de rol van de leden computer, zoals synchronisatie server. 

1. Start het programma Lokaal beveiligingsbeleid

2. Navigeer naar **Lokaal beleid > Toewijzing van gebruikersrechten**.

3. Klik in het detail venster met de rechter muisknop op **Aanmelden als service**en selecteer **Eigenschappen**.

    ![Afbeelding voor Lokaal beveiligingsbeleid](media/MIM-DeployWS3.png)

4. Klik op **gebruiker of groep toevoegen**, en typ in het tekstvak volgende op basis van rol `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, klik op **Namen controleren**en klik vervolgens op **OK**.

5. Klik op **OK** om het venster **Aanmelden als service > Eigenschappen** te sluiten.

6.  Klik in het detail venster met de rechter muisknop op **toegang tot deze computer vanaf het netwerk weigeren**en selecteer **Eigenschappen**. >

7. Klik op **Gebruiker of groep toevoegen**, typ `contoso\MIMSync; contoso\MIMService` in het tekstvak en klik op **OK**.

8. Klik op **OK** om het venster **Toegang tot deze computer vanaf het netwerk weigeren > Eigenschappen** te sluiten.

9. Klik in het detail venster met de rechter muisknop op **lokaal aanmelden weigeren**en selecteer **Eigenschappen**.

10. Klik op **Gebruiker of groep toevoegen**, typ `contoso\MIMSync; contoso\MIMService` in het tekstvak en klik op **OK**.

11. Klik op **OK** om het venster **Lokaal aanmelden weigeren > Eigenschappen** te sluiten.

12. Sluit het venster Lokaal beveiligingsbeleid.

## <a name="software-prerequisites"></a>Software vereisten

Voordat u MIM 2016 SP2-onderdelen installeert, moet u alle software vereisten installeren:

13. Installeer [Visual C++ 2013 Redistributable packages](https://www.microsoft.com/download/details.aspx?id=40784).

14. Installeer .NET Framework 4,6.

15. Op de server die als host dient voor de MIM-synchronisatie service, is voor de MIM-synchronisatie service [SQL Server Native Client](https://www.microsoft.com/download/details.aspx?id=50402)vereist.

16. Op de server waarop de MIM-service wordt gehost, is voor de MIM-service .NET Framework 3,5 vereist.

17. Als u TLS 1,2 of FIPS-modus gebruikt, raadpleegt u [MIM 2016 SP2 in ' tls 1,2 only ' of FIPS-modus omgevingen](preparing-tls.md).

## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Wijzig indien nodig de Windows-verificatie modus voor IIS

1.  Open een PowerShell-venster.

2.  Stop IIS met de opdracht *iisreset /STOP*

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [«Voorbereiden van een domein](preparing-domain.md)
> [SQL Server»](prepare-server-sql2016.md)
