---
title: De Microsoft Identity Manager-service en -portal installeren | Microsoft Docs
description: Lees welke stappen u moet uitvoeren voor het configureren en installeren van de MIM-service en -portal voor Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldiwn
ms.date: 10/12/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 204aa33cb21ed3998d9085fc56f0c7bea7afec58
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/27/2018
---
# <a name="install-mim-2016-mim-service-and-portal"></a>MIM 2016 installeren: de MIM-service en -portal

>[!div class="step-by-step"]
[« MIM-synchronisatieservice](install-mim-sync.md)
[Databases synchroniseren »](install-mim-sync-ad-service.md)

> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van de domeincontroller: **mimservername**
> - Domeinnaam: **contoso**
> - Wachtwoord - **Pass@word1**
> - Serviceaccountnaam: **MIMService**

Als u in de laatste stap geen MIM-installatiepakket hebt ingesteld, gaat u terug en installeert u de Microsoft Identity Manager 2016-onderdelen voordat u verdergaat.


## <a name="configure-mim-service-and-portal-for-installation"></a>De MIM-service en -portal configureren voor de installatie

1. Voer het **installatieprogramma voor de MIM-service en -portal** uit in de uitgepakte submap **Service en portal**.

2. Klik in het welkomstscherm op **Volgende**.

3. Lees de gebruiksrechtovereenkomst en klik op **Volgende** als u akkoord gaat met de licentievoorwaarden.

4. Klik in het scherm **Programma voor kwaliteitsverbetering voor MIM** op **Volgende**.

5. Wanneer u voor deze implementatie onderdeelfuncties selecteert, moet u de functies voor de MIM-service (behalve voor MIM-rapportage) en MIM-portal opnemen. U kunt ook het MIM-portal voor het opnieuw instellen van het wachtwoord en de MIM-meldingsservice voor wachtwoordwijzigingen selecteren.

6. Kies op de pagina **De MIM-databaseverbinding configureren** de optie **Een nieuwe database maken**.

    ![Afbeelding voor het configureren van de MIM-databaseverbinding](media/MIM-Install10.png)

7. Op de **mailserververbinding configureren**, voer de naam van uw Exchange-server als **e-mailserver** of kunt u O365 postvak. Als u geen mailserver hebt geconfigureerd, gebruikt u **localhost** als de naam van de mailserver en schakelt u de twee bovenste selectievakjes uit. Klik op **Volgende**.

    ![Afbeelding van Configureren van de e-mailserververbinding](media/MIM-Install11.png)

8. Geef op dat u een nieuw zelfondertekend certificaat wilt genereren of selecteer het betreffende certificaat.

9. Geef de serviceaccountnaam op die moet worden gebruikt, bijvoorbeeld *MIMService*, en het serviceaccountwachtwoord, bijvoorbeeld *Pass@word1*. Geef daarnaast uw serviceaccountdomein, bijvoorbeeld *contoso*, en het e-mailaccount voor de service, bijvoorbeeld *contoso*, op.

    ![Afbeelding van Het MIM-serviceaccount configureren](media/MIM-Install12.png)

10. Er kan een waarschuwing worden weergegeven dat het serviceaccount niet is beveiligd in de huidige configuratie.

11. Accepteer de standaardwaarden voor de locatie van de synchronisatieserver en geef het account van de MIM-beheeragent als *contoso\MIMMA*.

    ![Afbeelding voor het configureren van de MIM-service en -portal](media/MIM-Install13.png)

12. Geef *CORPIDM* (de naam van deze computer) op als het serveradres van de MIM-service voor de MIM-portal.

13. Geef *http://mim.contoso.com* URL als de SharePoint-siteverzameling.

14. Geef *http://passwordregistration.contoso.com* als de URL voor Wachtwoordregistratie poort 80, het beste later bijwerken met SSL-certificaat op 443.

15. Geef *http://passwordreset.contoso.com* als de URL voor wachtwoord opnieuw instellen poort 80, het beste later bijwerken met SSL-certificaat op 443.

16. Schakel het selectievakje in om de poorten 5725 en 5726 in de firewall te openen en schakel het selectievakje in om alle geverifieerde gebruikers toegang te verlenen tot de MIM-portal.

## <a name="configure-mim-password-registration-portal"></a>De MIM-portal voor wachtwoordregistratie configureren

1.  Stel de serviceaccountnaam voor SSPR-registratie in op *contoso\MIMSSPR* en het bijbehorende wachtwoord op *Pass@word1*.

2.  Geef *passwordregistration.contoso.com* als de hostnaam voor de MIM-Wachtwoordregistratie en stel de poort op **80**. Schakel de optie **Poort in de firewall openen** in.

    ![Afbeelding voor het invoeren van de configuratiegegevens die worden gebruikt door IIS](media/MIM-Install14.png)

3.  Er wordt een waarschuwing weergegeven: lees deze en klik op **Volgende**.

4. Geef in het volgende scherm in de configuratie van MIM-Portal voor Wachtwoordregistratie, *mim.contoso.com* als het serveradres van de MIM-Service voor de Portal voor Wachtwoordregistratie.

## <a name="configure-mim-password-reset-portal"></a>Het MIM-portal voor het opnieuw instellen van het wachtwoord configureren

1.  Stel de serviceaccountnaam voor SSPR-registratie voor *Contoso\MIMSSPR* en het bijbehorende wachtwoord op *Pass@word1*.

2.  Geef *passwordreset.contoso.com* als de hostnaam voor MIM wachtwoord opnieuw instellen-Portal, en stel de poort op **80**. Schakel de optie **Poort in de firewall openen** in.

    ![Afbeelding voor het invoeren van de configuratiegegevens die worden gebruikt door IIS](media/MIM-Install15.png)

3.  Er wordt een waarschuwing weergegeven: lees deze en klik op **Volgende**.

4. Geef in het volgende scherm in de configuratie van MIM-Portal voor Wachtwoordregistratie, *mim.contoso.com* als het serveradres van de MIM-Service voor de Portal voor wachtwoord opnieuw instellen.

## <a name="install-mim-service-and-portal"></a>De MIM-service en -portal installeren

Wanneer alle definities voorafgaand aan de installatie gereed zijn, klikt u op **Installeren** om met de installatie van de geselecteerde onderdelen voor de **service en portal** te beginnen.

Nadat de installatie is voltooid, controleert u of de MIM-portal actief is.

1. Start Internet Explorer en maak verbinding met de MIM-Portal op *http://mim.contoso.com/identitymanagement*. Er kan bij het eerste bezoek aan deze pagina een korte vertraging optreden.

    - Indien nodig geverifieerd als *contoso\miminstall* naar Internet Explorer.

2. Ga in Internet Explorer naar **Internetopties**, open het tabblad **Beveiliging** en voeg de site toe aan de zone **Lokaal intranet** als de site hier nog niet wordt weergegeven.  Sluit het dialoogvenster **Internetopties**.

3. Gebruikers in staat stellen om hun eigen vermelding in MIM te bekijken.

    1.  Ga in Internet Explorer naar de **MIM-portal** en klik op **Beheerbeleidsregels**.

    2.  Zoek naar de beheerbeleidsregel **Gebruikersbeheer: gebruikers kunnen hun eigen kenmerken lezen**.

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

>[!div class="step-by-step"]  
[« MIM-synchronisatieservice](install-mim-sync.md)
[Databases synchroniseren »](install-mim-sync-ad-service.md)
