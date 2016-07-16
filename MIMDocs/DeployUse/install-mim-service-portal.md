---
title: MIM 2016&#58; MIM-service en -portal installeren | Microsoft Identity Manager
description: Lees welke stappen u moet uitvoeren voor het configureren en installeren van de MIM-service en -portal voor Microsoft Identity Manager 2016
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c023d147d0fcc1525fefbe866c952e217f7bee6b
ms.openlocfilehash: eb2af412d9638035de591197fa191e990ade0ca1


---

# MIM 2016 installeren: de MIM-service en -portal

>[!div class="step-by-step"]
[« MIM-synchronisatieservice](install-mim-sync.md)
[Databases synchroniseren »](install-mim-sync-ad-service.md)

> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van de domeincontroller: **mimservername**
> - Domeinnaam: **contoso**
> - Wachtwoord: **Pass@word1**
> - Serviceaccountnaam: **MIMService**

Als u in de laatste stap geen MIM-installatiepakket hebt ingesteld, gaat u terug en installeert u de Microsoft Identity Manager 2016-onderdelen voordat u verdergaat.


## De MIM-service en -portal configureren voor de installatie

1. Voer het **installatieprogramma voor de MIM-service en -portal** uit in de uitgepakte submap **Service en portal**.

2. Klik in het welkomstscherm op **Volgende**.

3. Lees de gebruiksrechtovereenkomst en klik op **Volgende** als u akkoord gaat met de licentievoorwaarden.

4. Klik in het scherm **Programma voor kwaliteitsverbetering voor MIM** op **Volgende**.

5. Wanneer u voor deze implementatie onderdeelfuncties selecteert, moet u de functies voor de MIM-service (behalve voor MIM-rapportage) en MIM-portal opnemen. U kunt ook het MIM-portal voor het opnieuw instellen van het wachtwoord en de MIM-meldingsservice voor wachtwoordwijzigingen selecteren.

6. Kies op de pagina **De MIM-databaseverbinding configureren** de optie **Een nieuwe database maken**.

    ![Afbeelding voor het configureren van de MIM-databaseverbinding](media/MIM-Install10.png)

7. Voer op de pagina **De mailserververbinding configureren** de naam van uw Exchange-server in als **Mailserver**. Als u geen mailserver hebt geconfigureerd, gebruikt u **localhost** als de naam van de mailserver en schakelt u de twee bovenste selectievakjes uit. Klik op **Volgende**.

    ![Afbeelding van Configureren van de e-mailserververbinding](media/MIM-Install11.png)

8. Geef op dat u een nieuw zelfondertekend certificaat wilt genereren of selecteer het betreffende certificaat.

9. Geef de serviceaccountnaam op die moet worden gebruikt, bijvoorbeeld *MIMService*, en het serviceaccountwachtwoord, bijvoorbeeld *Pass@word1*. Geef daarnaast uw serviceaccountdomein, bijvoorbeeld *contoso*, en het e-mailaccount voor de service, bijvoorbeeld *contoso*, op.

    ![Afbeelding van Het MIM-serviceaccount configureren](media/MIM-Install12.png)

10. Er kan een waarschuwing worden weergegeven dat het serviceaccount niet is beveiligd in de huidige configuratie.

11. Accepteer de standaardinstellingen voor de locatie van de synchronisatieserver en geef *contoso\MIMsync* op als het MIM-beheeragentaccount.

    ![Afbeelding voor het configureren van de MIM-service en -portal](media/MIM-Install13.png)

12. Geef *CORPIDM* (de naam van deze computer) op als het serveradres van de MIM-service voor de MIM-portal.

13. Geef *http://CorpIDM.contoso.local:82* op als de URL van de SharePoint-siteverzameling.

14. Geef *http://CorpIDM.contoso.local:8080* op als de URL voor de wachtwoordregistratie.

15. Geef *http://CorpIDM.contoso.local:8088* op als de URL voor het opnieuw instellen van het wachtwoord.

16. Schakel het selectievakje in om de poorten 5725 en 5726 in de firewall te openen en schakel het selectievakje in om alle geverifieerde gebruikers toegang te verlenen tot de MIM-portal.

## De MIM-portal voor wachtwoordregistratie configureren

1.  Stel de serviceaccountnaam voor SSPR-registratie in op *contoso\MIMSSPR* en het bijbehorende wachtwoord op *Pass@word1*.

2.  Geef *CORPIDM* op als de hostnaam voor de MIM-wachtwoordregistratie en stel de poort in op **8080**. Schakel de optie **Poort in de firewall openen** in.

    ![Afbeelding voor het invoeren van de configuratiegegevens die worden gebruikt door IIS](media/MIM-Install14.png)

3.  Er wordt een waarschuwing weergegeven: lees deze en klik op **Volgende**.

4. Geef in het volgende configuratiescherm voor de MIM-portal voor wachtwoordregistratie *http://CorpIDM.contoso.local* op als serveradres van de MIM-service voor de MIM-portal voor wachtwoordregistratie.

## Het MIM-portal voor het opnieuw instellen van het wachtwoord configureren

1.  Stel de serviceaccountnaam voor SSPR-registratie in op *Contoso\MIMSSPRService* en het bijbehorende wachtwoord op *Pass@word1*.

2.  Geef *CORPIDM* op als de hostnaam voor de MIM-wachtwoordregistratie en stel de poort in op **8080**. Schakel de optie **Poort in de firewall openen** in.

    ![Afbeelding voor het invoeren van de configuratiegegevens die worden gebruikt door IIS](media/MIM-Install15.png)

3.  Er wordt een waarschuwing weergegeven: lees deze en klik op **Volgende**.

4. Geef in het volgende configuratiescherm voor de MIM-portal voor wachtwoordregistratie *CorpIDname  http://CorpIDname.domain.local* op als het serveradres van de MIM-service voor de portal voor het opnieuw instellen van het wachtwoord.

## De MIM-service en -portal installeren

Wanneer alle definities voorafgaand aan de installatie gereed zijn, klikt u op **Installeren** om met de installatie van de geselecteerde onderdelen voor de **service en portal** te beginnen.

Nadat de installatie is voltooid, controleert u of de MIM-portal actief is.

1. Start Internet Explorer en maak verbinding met de MIM-portal op *http://corpidm.contoso.local:82/identitymanagement*. Er kan bij het eerste bezoek aan deze pagina een korte vertraging optreden.

    - Verifieer u zelf zo nodig als *contoso\Administrator* bij Internet Explorer.

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



<!--HONumber=Jun16_HO4-->


