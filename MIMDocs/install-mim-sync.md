---
title: De synchronisatieservice van MIM installeren | Microsoft Docs
description: Ga aan de slag met de MIM 2016-onderdelen en installeer en configureer de synchronisatieservice.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: b9bc580710a13b3a0868b0580d55469a753e7f33
ms.sourcegitcommit: 323c2748dcc6b6991b1421dd8e3721588247bc17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73568082"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>MIM 2016: MIM-synchronisatieservice installeren

> [!div class="step-by-step"]
> [« Exchange Server](prepare-server-exchange.md)
> [MIM-service en -portal »](install-mim-service-portal.md)
 
> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van domein controller- **corpdc**
> - Domeinnaam: **contoso**
> - Naam van de MIM-service Server- **corpservice**
> - Naam MIM-synchronisatie server- **corpsync**
> - SQL Server naam- **corpsql**
> - Wachtwoord - <strong>Pass@word1</strong>

Stel eerst het installatiepakket in voordat u de Microsoft Identity Manager 2016-onderdelen installeert.

1. Meld u aan als *contoso\miminstall* voor de server die u gebruikt voor Identity Management synchronisatie server **corpsync**.

2. Pak het MIM-installatiepakket uit of plaats de dvd met de MIM-installatiekopie.  Als u deze DVD niet hebt, raadpleegt u [Microsoft Identity Manager licenties en down loads](microsoft-identity-manager-licensing.md).

## <a name="install-mim-2016-sp1-synchronization-service"></a>MIM 2016 SP1-synchronisatie service installeren

1. Navigeer in de uitgepakte MIM-installatiemap naar de map **Synchronisatieservice**.

2. Voer het **installatieprogramma voor de MIM-synchronisatieservice** uit. Volg de richtlijnen van het installatieprogramma en voltooi de installatie.

3. Klik in het welkomstscherm op **Volgende**.

    ![Afbeelding voor het welkomstscherm van de wizard voor het MIM-installatieprogramma](media/install-mim-sync/MIM_Install1.png)

4. Lees de licentievoorwaarden en klik op **Volgende** om deze te accepteren.

5. Klik in het scherm **Aangepaste installatie** op **Volgende**.

    ![Afbeelding van Aangepaste installatie](media/install-mim-sync/MIM_Install2.png)

6. Selecteer het volgende in het configuratiescherm voor de Sync-servicedatabase:

   1.  De SQL Server bevindt zich op: **een externe computer met de** naam **corpsql.contoso.com**.

   2.  Het SQL Server-exemplaar is: **het standaardexemplaar**

   ![Afbeelding voor de databaseverbinding](media/install-mim-sync/MIM_Install3.png)

    3. *Mim 2016 SP2 en hoger*: de naam van de MIM-synchronisatie service-data base configureren

7. Configureer het synchronisatieserviceaccount volgens het account dat u eerder hebt gemaakt:

   1. Serviceaccount: *MIMSync*

   2. Wachtwoord: <em>Pass@word1</em>

   3. Serviceaccountdomein of naam van de lokale computer: *contoso*

    >[!NOTE]
    >MIM 2016 SP2 en hoger: voor beheerde service accounts voor groepen, controleert u of het **$** -teken aan het einde van de naam van het service account is, bijvoorbeeld MIMSync $, en laat het veld wacht woord leeg.

    ![Afbeelding voor het serviceaccount](media/install-mim-sync/MIM_Install4.png)

8. Geef de betreffende beveiligingsgroepen op voor het installatieprogramma van de MIM Sync-service:

   1. Beheerder = *contoso\MIMSyncAdmins*

   2. Operator = *contoso\MIMSyncOperators*

   3. Verbinding = *contoso\MIMSyncJoiners*

   4. Browsen met connectoren = *contoso\MIMSyncBrowse*

   5. WMI-wachtwoordbeheer = *contoso\MIMSyncPasswordReset*

   ![Afbeelding voor de beveiligingsgroepen](media/install-mim-sync/MIM_Install5.png)

9. Schakel in het scherm voor de beveiligingsinstellingen de optie **Firewallregels voor binnenkomende RPC-communicatie inschakelen** in en klik op **Volgende**.

10. Klik op **Installeren** om de installatie van de MIM Sync-service te starten.

    1. Er kan een waarschuwing voor het MIM Sync-serviceaccount worden weergegeven. Klik op **OK**.

    2. MIM Sync-service wordt geïnstalleerd.

    3. Er wordt een bericht weergegeven over het maken van een back-up voor de versleutelingssleutel: klik op **OK**, selecteer vervolgens een map waarin de back-up voor de versleutelingssleutel moet worden opgeslagen.

    ![Afbeelding voor het bericht over de back-up voor de versleutelingssleutel voor MIM Sync](media/MIM-Install7.png)

    4. Wanneer de installatie is voltooid met het installatieprogramma, klikt u op **Voltooien**.

    5. U moet zich afmelden en weer aanmelden om de wijzigingen voor een groepslidmaatschap door te voeren. Klik op **Ja** om u af te melden.

> [!div class="step-by-step"]  
> [« Exchange Server](prepare-server-exchange.md)
> [MIM-service en -portal »](install-mim-service-portal.md)
