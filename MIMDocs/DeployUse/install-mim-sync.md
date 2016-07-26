---
title: MIM-synchronisatieservice installeren | Microsoft Identity Manager
description: Ga aan de slag met de MIM 2016-onderdelen en installeer en configureer de synchronisatieservice.
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: 2dfc89d32ef3b615201f1eb57ed3b8f5daed513e


---

# MIM 2016: MIM-synchronisatieservice installeren

>[!div class="step-by-step"]
[« Exchange Server](prepare-server-exchange.md)
[MIM-service en -portal »](install-mim-service-portal.md)

> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van de domeincontroller: **mimservername**
> - Domeinnaam: **contoso**
> - Wachtwoord: **Pass@word1**

Stel eerst het installatiepakket in voordat u de Microsoft Identity Manager 2016-onderdelen installeert.

1. Meld u als *contoso\Administrator* aan bij de server die u voor het identiteitsbeheer gebruikt.

2. Pak het MIM-installatiepakket uit of plaats de dvd met de MIM-installatiekopie.

## De MIM 2016-synchronisatieservice installeren

1. Navigeer in de uitgepakte MIM-installatiemap naar de map **Synchronisatieservice**.

2. Voer het **installatieprogramma voor de MIM-synchronisatieservice** uit. Volg de richtlijnen van het installatieprogramma en voltooi de installatie.

3. Klik in het welkomstscherm op **Volgende**.

    ![Afbeelding voor het welkomstscherm van de wizard voor het MIM-installatieprogramma](media/MIM-Install1.png)

4. Lees de licentievoorwaarden en klik op **Volgende** om deze te accepteren.

5. Klik in het scherm **Aangepaste installatie** op **Volgende**.

    ![Afbeelding van Aangepaste installatie](media/MIM-Install2.png)

6.  Selecteer het volgende in het configuratiescherm voor de synchronisatiedatabase:

    1.  SQL Server bevindt zich op: **Deze computer**.

    2.  Het SQL Server-exemplaar is: **Het standaardexemplaar**.

    ![Afbeelding voor de databaseverbinding](media/MIM-Install3.png)

7.  Configureer het synchronisatieserviceaccount volgens het account dat u eerder hebt gemaakt:

    1.  Serviceaccount: *MIMSync*

    2.  Wachtwoord: *Pass@word1*

    3.  Serviceaccountdomein of naam van de lokale computer: *contoso*

    ![Afbeelding voor het serviceaccount](media/MIM-Install4.png)

8.  Geef de betreffende beveiligingsgroepen op voor het MIM Sync-installatieprogramma:

    1. Beheerder = *contoso\MIMSyncAdmins*

    2. Operator = *contoso\MIMSyncOperators*

    3. Verbinding = *contoso\MIMSyncJoiners*

    4. Browsen met connectoren = *contoso\MIMSyncBrowse*

    5. WMI-wachtwoordbeheer = *contoso\MIMSyncPasswordReset*

    ![Afbeelding voor de beveiligingsgroepen](media/MIM-Install5.png)

9. Schakel in het scherm voor de beveiligingsinstellingen de optie **Firewallregels voor binnenkomende RPC-communicatie inschakelen** in en klik op **Volgende**.

10. Klik op **Installeren** om de installatie van MIM Sync te starten.

    1. Er kan een waarschuwing voor het MIM Sync-serviceaccount worden weergegeven. Klik op **OK**.

    2. MIM Sync wordt geïnstalleerd.

    3. Er wordt een bericht weergegeven over het maken van een back-up voor de versleutelingssleutel: klik op **OK**, selecteer vervolgens een map waarin de back-up voor de versleutelingssleutel moet worden opgeslagen.

        ![Afbeelding voor het bericht over de back-up voor de versleutelingssleutel voor MIM Sync](media/MIM-Install7.png)

    4. Wanneer de installatie is voltooid met het installatieprogramma, klikt u op **Voltooien**.

    5. U moet zich afmelden en weer aanmelden om de wijzigingen voor een groepslidmaatschap door te voeren. Klik op **Ja** om u af te melden.

>[!div class="step-by-step"]  
[« Exchange Server](prepare-server-exchange.md)
[MIM-service en -portal »](install-mim-service-portal.md)



<!--HONumber=Jul16_HO3-->


