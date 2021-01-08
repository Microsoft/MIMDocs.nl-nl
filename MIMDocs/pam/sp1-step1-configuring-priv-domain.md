---
title: Stap 1 het PRIV-domein configureren
description: Het PRIV-domein voorbereiden met bestaande of nieuwe identiteiten die worden beheerd door Microsoft Identity Manager met behulp van scripts
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: a4d7c8ed4f5d7a9d6b4653caf03de69f1d18e509
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010622"
---
# <a name="step-1-configuring-the-priv-domain"></a>Stap 1 het PRIV-domein configureren

> [!div class="step-by-step"]
> [Stap 2»](sp1-step2-configuring-corp-domain.md)

1. Meld u aan bij de PRIVDC als beheerder
   * Als deze PAM-implementatie een PRIV-Only omgeving is, meldt u zich aan bij de CORPDC
2. Voer PowerShell uit als Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecteer menuoptie 1 (PRIV Forest Configuration)


De vereiste service accounts voor het beheer van SQL, share point en MIM worden automatisch gemaakt als ze nog niet aanwezig zijn in het domein. U wordt gevraagd de wachtwoorden voor het maken van deze service-accounts tijdens het uitvoeren van het script in te voeren.

Als het PRIV-domein Windows Server 2016 is, waarbij het functionaliteits niveau is ingesteld op Windows Server 2016, wordt u door het script gevraagd om de optionele Active Directory ' Privileged Access Management functie ' in te scha kelen ' die is vereist voor PAM. Bevestig de optie Ja om door te gaan. Voor functionele niveaus onder Windows Server 2016 kunt u de waarschuwing negeren dat extra configuratie niet wordt uitgevoerd. U moet de configuratie van de PAMDeployment.ps1-en PAM-forest opnieuw uitvoeren zodra de beheerder het functionaliteits niveau heeft verhoogd naar Windows Server 2016.

>[!NOTE]
>De volgende stappen zijn niet vereist voor PRIVOnly-configuraties

7. Kopieer de SIDs.txt die wordt gegenereerd in $env: SYSTEMDRIVE\PAM naar de vergelijkbare map op de CORPDC.
   * U hebt deze lijst met Sid's op de CORPDC nodig om machtigingen in een volgende stap in te stellen voor PRIV-gebruikers om de eigenschappen van CORP-gebruikers te kunnen lezen.
8. Nadat het script is voltooid, wordt u gevraagd om de computer opnieuw op te starten om de wijzigingen van kracht te laten worden.

> [!div class="step-by-step"]
> [Stap 2»](sp1-step2-configuring-corp-domain.md)
