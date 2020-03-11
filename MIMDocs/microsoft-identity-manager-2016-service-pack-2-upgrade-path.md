---
title: Voer een upgrade uit van FIM 2010 R2 en MIM 2016 SP2 naar Microsoft Identity Manager 2016 Service Pack 2 | Microsoft Docs
description: Meer informatie over het upgraden van uw FIM 2010 R2-of MIM 2016 SP2-onderdelen en het installeren van de onderdelen die nieuw zijn in MIM 2016.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 09/16/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: bfe604795d44cdea61fe0d10bc2943f9b8433784
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044141"
---
# <a name="mim-2016-sp2-upgrade--from-forefront-identity--or-microsoft-identity-manager"></a>MIM 2016 SP2 upgrade van Forefront Identity of Microsoft Identity Manager

Organisaties kunnen upgraden naar Microsoft Identity Manager 2016 SP2 van eerdere versies van Microsoft Identity Manager of Forefront Identity Manager.  In elke sectie van dit artikel wordt een ondersteund upgradepad beschreven.

Er zijn verschillende upgrade opties beschikbaar. Als u MIM 2016 al uitvoert en het onderliggende platform (Windows Server, SQL, share point, SCSM DW) niet hoeft te vernieuwen of als u de MIM-Services niet gebruikt met beheerde service accounts van een groep, en u geen MIM-taal pakketten maakt, dan is de eenvoudigste optie in-place upgrade/hotfix (. msp)-installatie. Anders wordt de volledige installatie aanbevolen.

> [!IMPORTANT]
> Controleer de sectie bijgewerkte [Software vereisten](prepare-server-ws2016.md#software-prerequisites) voordat u MIM 2016 SP2 installeert

## <a name="upgrade-from-fim-2010-r2-sp1-or-later-fim-builds"></a>Upgrade van FIM 2010 R2 SP1 of hoger FIM-builds

> [!NOTE]
> De mini maal ondersteunde versie van Forefront Identity Manager die rechtstreeks kan worden bijgewerkt naar MIM 2016 SP2 is FIM 2010 R2 SP1 (build 4.1.3419.0). Directe upgrade naar MIM 2016 SP2 van eerdere versies van FIM wordt niet ondersteund. Als u gebruikmaakt van FIM-builds die ouder zijn dan 4.1.3419.0, moet u een upgrade uitvoeren naar FIM 2010 R2 SP1 voordat u een upgrade uitvoert naar MIM 2016 SP2.

1. **Optie 1: volledige installatie met behulp van bestaande data bases**
    1. Maak een back-up van uw FIMSynchronizationService-en FIMService-data bases.
    1. Exporteer alle RCDC-objecten en RCDC-bron teken reeksen van FIM-service waarnaar u wijzigingen hebt aangebracht.
    1. Exporteer de versleutelings sleutels van de synchronisatie service.
    1. Met backup miisserver. exe. config, de map Extensions van de synchronisatie server, het micro soft. ResourceManagement. service. exe. config als MIM-installatie programma worden mogelijk aangepaste wijzigingen in de configuratie bestanden overschreven.
    1. Verwijder **alle** FIM-onderdelen, inclusief taal pakketten (om eerst te verwijderen).
    1. *Optioneel:* Verplaats FIM-data bases naar een andere SQL-Server. Het is raadzaam om SQL-aliassen te maken op MIM-servers en deze aliassen te gebruiken in plaats van echte SQL Server-naam om de Db's migratie van MIM in de toekomst te vereenvoudigen.
    1. MIM 2016 SP2 installeren vanaf de ISO-installatie media op dezelfde of op een andere server na de [MIM-implementatie handleiding](microsoft-identity-manager-deploy.md), ervoor kiezen om bestaande data bases te gebruiken wanneer u daarom wordt gevraagd, geeft u eerder geëxporteerde synchronisatie service versleutelings sleutels.
    1. Controleer de bestanden miisserver. exe. config en Microsoft. ResourceManagement. service. exe. config voor ontbrekende .net-omleidingen of aangepaste secties die u hebt toegevoegd.
    1. Installeer, indien nodig, de taal pakketten voor MIM 2016 SP2.
    1. Indien nodig aanpassingen aan RCDC-objecten en RCDC-resource teken reeksen en-lokalisatie opnieuw verzenden.
    1. Voer een upgrade uit voor FIM-invoeg toepassingen en wacht woorden voor het opnieuw instellen van de MIM-service Server als de naam van de MIM-service Server is gewijzigd.
    
## <a name="upgrade-from-previous-mim-2016-builds"></a>Upgrade van eerdere MIM 2016-builds
1. Maak een back-up van uw FIMSynchronizationService-en FIMService-data bases.
1. Exporteer alle aangepaste FIM-service-lokalisaties die zijn gemaakt met RCDC-objecten en RCDC-bron teken reeksen.
1. Exporteer de versleutelings sleutels van de synchronisatie service.
1. Met backup miisserver. exe. config, de map Extensions van de synchronisatie server, het micro soft. ResourceManagement. service. exe. config als MIM-installatie programma worden mogelijk aangepaste wijzigingen in de configuratie bestanden overschreven.
1. Verwijder de MIM-taal pakketten als u deze gebruikt.
1. De MIM-services stoppen.
1. **Optie 1: in-place upgrade-hotfix-installatie**
    1. De [Update](https://www.microsoft.com/download/details.aspx?id=100412) van de MIM 2016 SP2-synchronisatie service Toep assen
    1. De [hotfix](https://www.microsoft.com/download/details.aspx?id=100412) voor de MIM 2016 SP2-Service Toep assen
    1. Controleer de bestanden miisserver. exe. config en Microsoft. ResourceManagement. service. exe. config voor ontbrekende .net-omleidingen of aangepaste secties die moeten worden toegevoegd.
    1. Installeer, indien nodig, de taal pakketten voor MIM 2016 SP2.
    1. Indien nodig aanpassingen aan RCDC-objecten en RCDC-resource teken reeksen en-lokalisatie opnieuw verzenden.
    1. Voer een upgrade uit van MIM 2016-invoeg toepassingen en-clients met een wacht woord opnieuw instellen.
1. **Optie 2: volledige installatie met behulp van bestaande data bases**
    1. Verwijder **alle** MIM-onderdelen.
    1. *Optioneel:* Verplaats FIM-data bases naar een andere SQL-Server. Het is raadzaam om SQL-aliassen te maken op MIM-servers en deze aliassen te gebruiken in plaats van echte SQL Server-naam om de Db's migratie van MIM in de toekomst te vereenvoudigen.
    1. MIM 2016 SP2 installeren vanaf de ISO-installatie media op dezelfde of op een andere server na de [MIM-implementatie handleiding](microsoft-identity-manager-deploy.md), ervoor kiezen om bestaande data bases te gebruiken wanneer u daarom wordt gevraagd, geeft u eerder geëxporteerde synchronisatie service versleutelings sleutels.
    1. Controleer de bestanden miisserver. exe. config en Microsoft. ResourceManagement. service. exe. config voor ontbrekende .net-omleidingen of aangepaste secties die moeten worden toegevoegd.
    1. Installeer, indien nodig, de taal pakketten voor MIM 2016 SP2.
    1. Indien nodig aanpassingen aan RCDC-objecten en RCDC-resource teken reeksen en-lokalisatie opnieuw verzenden.
    1. Voer een upgrade uit van MIM 2016-invoeg toepassingen en-clients voor wacht woord opnieuw instellen en geef nieuwe naam van de MIM-service Server op als de naam van de MIM-service

> [!NOTE]
> Taal pakketten worden bijgewerkt nadat MIM 2016 SP2 wordt gedistribueerd als hotfixes (MSP-bestanden), waardoor het niet nodig is om taal pakketten te verwijderen en opnieuw te installeren.

Meer gedetailleerde informatie over de back-upprocedureen voor upgrades en data bases vindt u in het artikel [upgrade naar fim 2010 R2](https://docs.microsoft.com/previous-versions/mim/jj134291%28v%3dws.10%29) , dat van toepassing is op alle FIM-of MIM-upgrade processen.
