---
title: De Microsoft Identity Manager-service en -portal installeren | Microsoft Docs
description: Lees welke stappen u moet uitvoeren voor het configureren en installeren van de MIM-service en -portal voor Microsoft Identity Manager 2016
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: e381bb418ce8215dafc369bf33782483a6e4de3e
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042437"
---
# <a name="install-mim-2016-mim-service-and-portal"></a>MIM 2016 installeren: de MIM-service en -portal

> [!div class="step-by-step"]
> [«MIM-synchronisatie service](install-mim-sync.md)
> [data bases synchroniseren»](install-mim-sync-ad-service.md)
 
> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van de domeincontroller: **mimservername**
> - Domeinnaam: **contoso**
> - Wacht woord<strong>Pass@word1</strong>
> - Serviceaccountnaam: **MIMService**

Als u in de laatste stap geen MIM-installatiepakket hebt ingesteld, gaat u terug en installeert u de Microsoft Identity Manager 2016-onderdelen voordat u verdergaat.


## <a name="configure-mim-service-and-portal-for-installation"></a>De MIM-service en -portal configureren voor de installatie

1. Voer het **installatieprogramma voor de MIM-service en -portal** uit in de uitgepakte submap **Service en portal**.

2. Klik in het welkomstscherm op **Volgende**.

3. Lees de gebruiksrechtovereenkomst en klik op **Volgende** als u akkoord gaat met de licentievoorwaarden.

4. Klik in het scherm **Programma voor kwaliteitsverbetering voor MIM** op **Volgende**.

5. Wanneer u voor deze implementatie onderdeelfuncties selecteert, moet u de functies voor de MIM-service (behalve voor MIM-rapportage) en MIM-portal opnemen. U kunt ook het MIM-portal voor het opnieuw instellen van het wachtwoord en de MIM-meldingsservice voor wachtwoordwijzigingen selecteren.

6. Kies op de pagina **De MIM-databaseverbinding configureren** de optie **Een nieuwe database maken**.

    ![Afbeelding voor het configureren van de MIM-databaseverbinding](media/install-mim-service-portal/MIM_Install10.png)

7. Voer op de **e-mail server verbinding configureren**de naam van uw Exchange-server in als **e-mail server** of u kunt het **O365-postvak**gebruiken. Als u geen mailserver hebt geconfigureerd, gebruikt u **localhost** als de naam van de mailserver en schakelt u de twee bovenste selectievakjes uit. Klik op **Volgende**.
    >[!NOTE]
    >MIM 2016 SP2 en hoger: als u gebruikmaakt van door groepen beheerde service accounts, moet u het selectie vakje **andere gebruiker voor Exchange gebruiken** inschakelen, zelfs als u niet van plan bent om Exchange te gebruiken.
    
    >[!NOTE]
    >Wanneer u de optie **Exchange Online gebruiken** hebt geselecteerd, moet u de register sleutel HKLM\SYSTEM\CurrentControlSet\Services\FIMService waarde PollExchangeEnabled instellen op 1 na de installatie om de MIM-service in te scha kelen voor het verwerken van goedkeurings reacties van de MIM Outlook-invoeg toepassing.

    ![Afbeelding van Configureren van de e-mailserververbinding](media/install-mim-service-portal/MIM_Install11.png)

8. Geef op dat u een nieuw zelfondertekend certificaat wilt genereren of selecteer het betreffende certificaat.

9. Geef de serviceaccountnaam op die moet worden gebruikt, bijvoorbeeld *MIMService*, en het serviceaccountwachtwoord, bijvoorbeeld <em>Pass@word1</em>. Geef daarnaast uw serviceaccountdomein, bijvoorbeeld *contoso*, en het e-mailaccount voor de service, bijvoorbeeld *contoso*, op.
    >[!NOTE]
    >MIM 2016 SP2 en hoger: als u beheerde service accounts voor groepen gebruikt, moet u ervoor zorgen dat het **$** teken aan het einde van de naam van het service account wordt weer geven, bijvoorbeeld MIMService $, en laat het veld wacht woord van het service account leeg.

    ![Afbeelding van Het MIM-serviceaccount configureren](media/install-mim-service-portal/MIM_Install12.png)

10. Er kan een waarschuwing worden weergegeven dat het serviceaccount niet is beveiligd in de huidige configuratie.

11. Accepteer de standaard waarden voor de locatie van de synchronisatie server en geef het account van de MIM-beheer agent op als *contoso\MIMMA*.
    >[!NOTE]
    >MIM 2016 SP2 en hoger: als u van plan bent om het beheerde service account van de MIM-synchronisatie service te gebruiken in MIM Sync, en de functie MIM-synchronisatie account gebruiken in te scha kelen, voert u de naam van de MIM-synchronisatie service gMSA in als het MIM MA-account, bijvoorbeeld *beheeragentaccount $*.

    ![Afbeelding voor het configureren van de MIM-service en -portal](media/install-mim-service-portal/MIM_Install13.png)

12. Geef *CORPIDM* (de naam van deze computer) op als het serveradres van de MIM-service voor de MIM-portal.

13. Geef `http://mim.contoso.com` de URL van de share point-site verzameling op.

14. Opgeven `http://passwordregistration.contoso.com` als de URL-poort 80 van de wachtwoord registratie, raadt u aan om later te updaten met SSL-certificaten op 443.

15. Opgeven `http://passwordreset.contoso.com` als de URL-poort 80 voor het opnieuw instellen van het wacht woord, raadt u aan later te updaten met SSL-certificaat op 443.

16. Schakel het selectievakje in om de poorten 5725 en 5726 in de firewall te openen en schakel het selectievakje in om alle geverifieerde gebruikers toegang te verlenen tot de MIM-portal.

## <a name="configure-mim-password-registration-portal"></a>De MIM-portal voor wachtwoordregistratie configureren

1. Stel de serviceaccountnaam voor SSPR-registratie in op *contoso\MIMSSPR* en het bijbehorende wachtwoord op <em>Pass@word1</em>.

2. Geef *passwordregistration.contoso.com* op als de HOSTNAAM voor MIM-wachtwoord registratie en stel de poort in op **80**. Schakel de optie **Poort in de firewall openen** in.

   ![Afbeelding voor het invoeren van de configuratiegegevens die worden gebruikt door IIS](media/install-mim-service-portal/MIM_Install14.png)

3. Er wordt een waarschuwing weergegeven: lees deze en klik op **Volgende**.

4. Geef in het volgende MIM-configuratie scherm voor wachtwoord registratie *mim.contoso.com* op als het MIM-service Server adres voor de portal voor wachtwoord registratie.

## <a name="configure-mim-password-reset-portal"></a>Het MIM-portal voor het opnieuw instellen van het wachtwoord configureren

1. Stel de service accountnaam voor SSPR-registratie in op *Contoso\MIMSSPR* en het <em>Pass@word1</em>bijbehorende wacht woord in.

2. Geef *PasswordReset.contoso.com* op als de HOSTNAAM voor MIM-portal voor wachtwoord herstel en stel de poort in op **80**. Schakel de optie **Poort in de firewall openen** in.

   ![Afbeelding voor het invoeren van de configuratiegegevens die worden gebruikt door IIS](media/install-mim-service-portal/MIM_Install15.png)

3. Er wordt een waarschuwing weergegeven: lees deze en klik op **Volgende**.

4. Geef in het volgende MIM-configuratie scherm voor wachtwoord registratie *mim.contoso.com* op als het MIM-service Server adres voor de portal voor het opnieuw instellen van wacht woorden.

## <a name="install-mim-service-and-portal"></a>De MIM-service en -portal installeren

Wanneer alle definities voorafgaand aan de installatie gereed zijn, klikt u op **Installeren** om met de installatie van de geselecteerde onderdelen voor de **service en portal** te beginnen.

Nadat de installatie is voltooid, controleert u of de MIM-portal actief is.

1. Start Internet Explorer en maak verbinding met de MIM- *http://mim.contoso.com/identitymanagement*Portal op. Houd er rekening mee dat er een korte vertraging kan optreden op het eerste bezoek aan deze pagina.
    - Als dat nodig is, kunt u als *contoso\miminstall* verifiëren bij Internet Explorer.

2. Ga in Internet Explorer naar **Internetopties**, open het tabblad **Beveiliging** en voeg de site toe aan de zone **Lokaal intranet** als de site hier nog niet wordt weergegeven.  Sluit het dialoogvenster **Internetopties**.

3. Gebruikers in staat stellen om hun eigen vermelding in MIM te bekijken.

    1.  Ga in Internet Explorer naar de **MIM-portal** en klik op **Beheerbeleidsregels**.

    2.  Zoek naar de beheer beleidsregel, **gebruikers beheer: gebruikers kunnen hun eigen kenmerken lezen**.

    3.  Selecteer deze beheerbeleidsregel en schakel het selectievakje **Beleid is uitgeschakeld** uit.

    4.  Klik op **OK** en vervolgens op **Verzenden**.

4.  Controleer of de firewall binnenkomende verbindingen voor TCP-poort 5725 en 5726 toestaat.

    1.  Start **Systeembeheer » Windows Firewall** met **Geavanceerde beveiliging**.

    2.  Klik op **Regels voor binnenkomende verbindingen**.

    3.  Controleer of de volgende twee regels worden weergegeven:

    -   Forefront Identity Manager Service (STS).
    -   Forefront Identity Manager Service (webservice).

    4.  Voltooi de wizard en sluit de toepassing voor de **Windows-firewall**.

    5.  Start **Configuratiescherm » Netwerk en internet » Netwerkstatus en taken weergeven**.

    6.  Controleer of er een actief netwerk wordt vermeld met contoso.local als domeinnetwerk.

    7.  Sluit **Configuratiescherm**.

> [!NOTE]
> Optioneel: op dit moment kunt u MIM-invoegtoepassingen en -uitbreidingen installeren.
 
> [!div class="step-by-step"]  
> [«MIM-synchronisatie service](install-mim-sync.md)
> [data bases synchroniseren»](install-mim-sync-ad-service.md)
