---
title: Een upgrade uitvoeren voor FIM 2010 R2 | Microsoft Identity Manager
description: Informatie over het uitvoeren van een upgrade van uw FIM 2010 R2-onderdelen en het installeren van onderdelen die nieuw zijn in MIM 2016.
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: c77a41b47baa81f003e52f79d338a7810fa817b5


---

# Upgrade uitvoeren in Forefront Identity Manager 2010 R2

Als u een omgeving met Forefront Identity Manager (FIM) 2010 R2 hebt en u Microsoft Identity Manager (MIM) 2016 wilt uitproberen, kunt u deze handleiding gebruiken. Deze upgrade bestaat uit drie fasen:

1.  Installeer de MIM-synchronisatieservice (Sync) op een server die deel uitmaakt van het AD-domein (Active Directory). Hiermee vervangt u het FIM 2010 R2-exemplaar van de synchronisatieservice.

2.  MIM-service en -portal installeren Op dit moment kunt u er ook voor kiezen om de registratie- en serviceportal voor de selfservice voor wachtwoordherstel (SSPR) te installeren en de functieset voor Privileged Access Management uit te sluiten.

3.  Implementeer de MIM-invoegtoepassingen en -extensies op een afzonderlijke clientcomputer. Dit is inclusief de geïntegreerde SSRP-client voor de Windows-aanmelding.


In deze gids wordt verondersteld dat u het volgende al hebt ingesteld.
- FIM 2010 R2 is geïmplementeerd in een testomgeving
- Servers met Windows Server 2012, Windows Server 2012 R2 of Windows Server 2008 R2
- Lokale en milieuvereisten (SQL Server, Exchange Server, SharePoint Services enzovoort) die zijn geconfigureerd voor FIM 2010 R2.


## Voorbereiding

1.  Maak een back-up van uw FIM-servicedatabase, FIM-synchronisatiedatabase, FIM-synchronisatie en -serviceconfiguratie en -software.

2.  Meld u op elke server waarop FIM 2010 R2-onderdelen zijn geïnstalleerd, bijvoorbeeld *CORPIDM*, aan als Contoso\Administrator. In dit implementatievoorbeeld moet u over beheerdersrechten beschikken om de upgrade van FIM 2010 R2 naar **MIM** uit te voeren.

3.  Download de MIM-software of pak deze uit.

## De upgrade van de synchronisatieservice uitvoeren

1.  Meld u aan als beheerder op een server waarop FIM 2010 R2-synchronisatieservice (synchronisatie) is geïmplementeerd.

2.  Zorg ervoor dat u een back-up maakt van uw database voordat u deze procedure start.

3.  Open de console **Services**, zoek de **Forefront Identity Manager-synchronisatieservice ** en stop deze.

    ![Afbeelding van de console Services](media/MIM-UpgFIM1.PNG)

4.  Voer het **installatieprogramma voor de MIM-synchronisatieservice** uit. Het installatieprogramma detecteert de bestaande synchronisatieversie en stelt voor een upgrade uit te voeren. Klik op de knop **Bijwerken** om door te gaan.

5.  Klik op **Volgende** om door te gaan als u de licentievoorwaarden accepteert.

6.  Voer het wachtwoord in voor het serviceaccount dat door de synchronisatie wordt gebruikt en klik op **Volgende**.

    ![Afbeelding van MIM-synchronisatieserviceaccount configureren](media/MIM-UpgFIM3.png)

7.  Controleer of de namen van de beveiligingsgroepen juist zijn en klik op **Volgende**.

    ![Afbeelding van MIM-synchronisatiebeveiligingsgroepen configureren](media/MIM-UpgFIM4.png)

8.  Laat het selectievakje voor firewallregels voor inkomende RPC-communicaties ongewijzigd.

9. Het installatieprogramma is gereed voor het uitvoeren van de upgrade van synchronisatie van FIM 2010 R2 naar MIM. Klik op **Upgrade uitvoeren** om het upgradeproces te starten.

10. De upgrade wordt uitgevoerd. Sluit het installatieprogramma niet af en start de computer niet opnieuw op terwijl de upgrade wordt uitgevoerd.

    ![Afbeelding van MIM-synchronisatie installeren](media/MIM-UpgFIM7.png)

11. Tijdens het uitvoeren van de upgrade wordt een waarschuwing met betrekking tot de upgrade van de synchronisatiedatabase weergegeven. Aangeraden wordt een back-up te maken van de database voordat u de upgrade uitvoert.

12. Zodra de upgrade is uitgevoerd, klikt u op **Voltooien**

    ![Afbeelding van voltooide installatie van MIM-synchronisatie](media/MIM-UpgSP1.png)

13. Controleer of de **Synchronisatieservice** opnieuw is opgestart.

## Een upgrade uitvoeren voor de Service en de Portal

1.  Meld u aan als beheerder op een server waarop de FIM 2010 R2-service en -portal zijn geïmplementeerd.

2.  Open de console **Services**, zoek de **Forefront Identity Manager-service ** en stop deze service.

    ![Afbeelding van de console Services](media/MIM-UpgFIM9.PNG)

3.  Voer het installatieprogramma voor de MIM-service en portal uit. Klik op de knop **Installeren** om door te gaan.

    ![Afbeelding van MIM-service en -portal installeren](media/MIM-UpgSP2.png)

4.  Klik op **Volgende** om door te gaan als u de licentievoorwaarden accepteert.

5.  Klik in het MIM-scherm Programma voor verbetering van de gebruikerservaring op **Volgende** om door te gaan.

6.  Selecteer de MIM-functies en onderdelen die u wilt installeren en klik als u klaar bent op **Volgende**.

    ![Afbeelding van Aangepaste installatie](media/MIM-UpgSP4.png)

    1.  **MIM-service:** deze functie is verplicht op ten minste één server. Hiervoor is een SQL Server-databaseserver vereist die in hetzelfde volume of op een andere server is geplaatst.

    2.  **MIM-portal:** deze functie is verplicht op ten minste één server. Hiervoor is SharePoint 2013 Foundation vereist.

    3.  **MIM-portal voor wachtwoordregistratie:** dit onderdeel is nodig voor self-service voor wachtwoordherstel.

    4.  **MIM-portal voor wachtwoordherstel:** dit onderdeel is nodig voor wachtwoordherstel.

7.  Geef details op van de SQL Server die wordt gebruikt voor de FIM-servicedatabase. Selecteer de optie voor het opnieuw gebruiken van de bestaande database en het behouden van de gegevens. Klik op **Volgende** om door te gaan.

8. Als de optie voor het opnieuw gebruiken van de bestaande database is geselecteerd, wordt een herinnering weergegeven voor het maken van een back-up van de database.

9. Voer de details van de e-mailserver in. Als de e-mailserver zich op de huidige server bevindt, voert u localhost in als locatie van de mail-server. Klik op **Volgende** om door te gaan.

    ![Afbeelding van Configureren van de e-mailserververbinding](media/MIM-UpgSP6.png)

10. Selecteer een certificaat dat de service moet gebruiken om clients te valideren. U moet het bestaande certificaat uit het lokale certificaatarchief gebruiken dat eerder werd gebruikt door de FIM-service.

    ![Afbeelding van Servicecertificaat configureren](media/MIM-UpgSP7.png)

    1.  Als de optie voor het lokale certificaatarchief is geselecteerd, klikt u op de knop **Certificaat selecteren** en selecteert u een certificaat in de lijst in het pop-upvenster. Klik achtereenvolgens op **OK** en **Volgende**.

        ![Afbeelding pop-upvenster Certificaat selecteren](media/MIM-UpgSP8.PNG)

11. Referenties voor Service-Account configureren voor MIM-service. Houd er rekening mee dat het serviceaccount niet hetzelfde serviceaccount mag zijn als het account dat wordt gebruikt door de synchronisatieservice. Dit moet hetzelfde account zijn dat werd gebruikt door de FIM-service.

    ![Afbeelding van Het MIM-serviceaccount configureren](media/MIM-UpgSP9.png)

12. Configureer de details van de MIM-synchronisatieserver op basis van de implementatie van de MIM-service die u in de vorige stap hebt geconfigureerd.

    ![Afbeelding van De MIM-service -en portalsynchronisatie configureren](media/MIM-UpgSP10.png)

13. Als u de MIM-portal installeert, moet u het adres van de server van de MIM-service opgeven. Klik op **Volgende**.

14. Als u de MIM-portal installeert, moet u de URL opgeven van de SharePoint-siteverzameling waarin de FIM-portal momenteel wordt gehost. Klik op **Volgende**.

## De MIM-portal voor wachtwoordregistratie installeren

1. Als u de MIM-portal voor wachtwoordregistratie installeert, moet u de aangevraagde URL voor de portal voor wachtwoordregistratie opgeven. Klik op **Volgende**.

2. Configureer de mogelijkheid voor clients en eindgebruikers om de service en de portal te gebruiken.

    1.  Controleer of u **poort 5725 en 5726 in de firewall wilt openen**.

    2.  Controleer of u **Geverifieerde gebruikers toegang wilt verlenen tot de MIM-portalsite**.

    3.  Klik op **Volgende**.

3. Geef de toegangsgegevens en -referenties op voor MIM-wachtwoordregistratie.

    ![Afbeelding van De MIM-portal voor wachtwoordregistratie configureren](media/MIM-UpgSP15.png)

    1.  Geef de naam van het serviceaccount (inclusief domein) en het wachtwoord voor MIM-wachtwoordregistratie op.

    2.  Geef de details op van de host, naam en poort (bijvoorbeeld 8080), van de portal voor wachtwoordregistratie.

    3.  Schakel de optie **Poort in de firewall openen** in.

    4.  Klik op **Volgende**.

4. In het volgende scherm in de configuratie van MIM-wachtwoordregistratie:

    1.  Geef aan de MIM-wachtwoordregistratie op waar de MIM-service is geïnstalleerd. Dit is doorgaans hetzelfde systeem.

    2.  Bepaal of deze portal toegankelijk is voor extranet- en intranetgebruikers, of alleen voor intranetgebruikers, zoals eerder was geconfigureerd voor wachtwoord opnieuw instellen voor FIM.

## De MIM-portal voor wachtwoord opnieuw instellen installeren

1. Als u de MIM-portal voor wachtwoord opnieuw instellen installeert, moet u de toegangsdetails en -referenties opgeven voor MIM-wachtwoord opnieuw instellen.

    ![Afbeelding van de MIM-portal voor wachtwoord opnieuw instellen configureren](media/MIM-UpgSP17.png)

    1.  Geef de naam van het serviceaccount (inclusief domein) en het wachtwoord voor MIM-wachtwoord opnieuw instellen op.

    2.  Geef de details op van de host, naam en poort (bijvoorbeeld 8088), van de portal voor wachtwoord opnieuw instellen.

    3.  Schakel de optie **Poort in de firewall openen** in.

    4.  Klik op **Volgende**.

2. In het volgende scherm in de configuratie van MIM-wachtwoord opnieuw instellen:

    1.  Geef aan MIM-wachtwoord opnieuw instellen op waar de MIM-service is geïnstalleerd.

    2.  Geef op of deze portal toegankelijk is voor extranet- en intranetgebruikers, of alleen intranetgebruikers.

## Installatie en upgrade voltooien

1. Als alle configuratiedefinities zijn voltooid, wordt een installatiepagina weergegeven. Klik op **Installeren** om de installatie en upgrade van de MIM-service en -portal te starten.

2. De installatie en upgrade van de MIM-service en de portal wordt uitgevoerd. Annuleer het installatieprogramma niet en start de computer niet opnieuw op tijdens de installatie.

3. Zodra de installatie (upgrade) van de MIM-service en portal correct is voltooid, wordt een bevestigingsvenster weergegeven. Klik op **Voltooien** om de installatie te voltooien en het installatieprogramma af te sluiten.

4. Controleer of de **Forefront Identity Manager-service** opnieuw is opgestart.

Opmerking: Als de FIM-invoegtoepassingen en -extensies momenteel zijn geïmplementeerd op de computers van gebruikers voor SSPR, configureert u de nieuwe MFA-telefoonpoorten voor wachtwoordherstel pas nadat de upgrade voor alle FIM-invoegtoepassingen en -uitbreidingen voor MIM 2016 is uitgevoerd.  Omdat de FIM 2010 en FIM 2010 R2-invoegtoepassingen en -extensies de nieuwe poorten niet herkennen, wordt hierdoor een fout veroorzaakt en kan een gebruiker wachtwoordherstel niet voltooien.



<!--HONumber=Jul16_HO3-->


