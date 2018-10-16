---
title: De meldingsservice voor wachtwoordwijzigingen implementeren | Microsoft Docs
description: Lees welke stappen u moet uitvoeren voor het installeren en configureren van de MIM-meldingsservice voor wachtwoordwijzigingen op uw domeincontroller.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5f9380b3bd9d77cc9e46b0af9b63141c940cc808
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334207"
---
# <a name="deploy-the-mim-password-change-notification-service-on-a-domain-controller"></a>De MIM-meldingsservice voor wachtwoordwijzigingen op een domeincontroller implementeren

## <a name="install-the-password-change-notification-service"></a>De meldingsservice voor wachtwoordwijzigingen installeren
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

## <a name="configuring-the-password-change-notification-service"></a>De meldingsservice voor wachtwoordwijzigingen configureren
Als u als domeinadministrator opnieuw verbinding hebt gemaakt met de DC-server, gaat u naar *C:\Program Files\Microsoft Password Change Notification.* Voer *pcnscfg.exe* uit.
