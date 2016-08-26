---
title: Meldingen voor wachtwoordwijzigingen | Microsoft Identity Manager
description: Lees welke stappen u moet uitvoeren voor het installeren en configureren van de MIM-meldingsservice voor wachtwoordwijzigingen op uw domeincontroller.
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: e25f4b3a60f2c432cd33c8f84c750110cbe605ee


---

# De MIM-meldingsservice voor wachtwoordwijzigingen op een domeincontroller implementeren

## De meldingsservice voor wachtwoordwijzigingen installeren
De meldingsservice voor wachtwoordwijzigingen (Password Change Notification Service of PCNS) is een service die u installeert op de domeincontrollers waarmee synchronisatie van wachtwoorden door MIM met andere systemen, zoals een adreslijstserver van een andere leverancier, mogelijk wordt gemaakt. Installeer voor wachtwoordsynchronisatie de PCNS op elke domeincontrollerserver.

1.  Meld u aan als domeinadministrator op een server met Windows Server met de rol van Active Directory Domain Services.

2.  Kopieer de installatiemap Meldingsservice voor wachtwoordwijzigingen naar de computer.

3.  Zoek het bestand *Password Change Notification Service.msi*, klik hierop met de rechtermuisknop en maak een snelkoppeling.

4.  Ga naar het snelkoppelingsbestand, klik hierop met de rechtermuisknop en open de **Eigenschappen**.

5.  Voeg in het doelveld de preamble *msiexec.exe /i* toe vóór het pad naar het .msi-bestand en het achtervoegsel *SCHEMAONLY = TRUE* na het MSI-pad. Als de installatiemap bijvoorbeeld *C:\PCNS* is, ziet de uit te voeren opdracht er als volgt uit: (alles op één regel).

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  Sla de wijzigingen op in het snelkoppelingsbestand.

7.  Voer het snelkoppelingsbestand uit om de installatie van PCNS te starten in de modus voor schema-uitbreiding. Als het volgende scherm wordt weergegeven, klikt u op **Volgende**.

8.  Er wordt een melding weergegeven dat Setup het Active Directory-schema bijwerkt voor de meldingsservice voor wachtwoordwijzigingen. Klik op **OK** om door te gaan met de schema-update.

9. Wanneer het schema-uitbreidingsproces is voltooid en het volgende scherm wordt weergegeven, klikt u op **Voltooien**.

10. Voer het bestand *Password Change Notification Service.msi* bestand opnieuw uit, maar nu rechtstreeks. (Er is geen tekenreeks voor uitvoeren nodig.)  Als het volgende scherm wordt weergegeven, klikt u op **Volgende**.

11. Accepteer de gebruiksrechtovereenkomst en klik op **Volgende**.

12. Klik om de installatie te starten.

13. Als het scherm wordt weergegeven waarin wordt aangegeven dat de installatie is voltooid, klikt u op **Voltooien**.

14. Start de computer opnieuw op om de wijzigingen in de configuratie van de MIM-meldingsservice voor wachtwoordwijzigingen van kracht te laten worden. U kunt dit doen in het pop-upvenster dat wordt weergegeven op **Ja** te klikken, maar u kunt ook later opnieuw opstarten.

## De meldingsservice voor wachtwoordwijzigingen configureren
Als u als domeinadministrator opnieuw verbinding hebt gemaakt met de DC-server, gaat u naar *C:\Program Files\Microsoft Password Change Notification.* Voer *pcnscfg.exe* uit.



<!--HONumber=Jul16_HO3-->


