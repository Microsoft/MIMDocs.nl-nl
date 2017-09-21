---
title: Installatie van reporting BHOLD | Microsoft Docs
description: BHOLD-rapportagemodule kunt u rapporten over de functies en autorisatiebeleid genereren
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: aa6a263daadc4abdcad0eaaba554b6bc739fbd5f
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-reporting-installation"></a>BHOLD reporting installatie

De rapportage van BHOLD-module biedt u de mogelijkheid om rapporten te genereren over de functies en andere autorisatiebeleid in BHOLD. Deze rapporten zijn vaak nuttig voor het controleren of voor het demonstreren van naleving van voorschriften. Deze module is bovendien de mogelijkheid voor het beheren van autorisatie binnen uw organisatie doordat gebruikers de gegevens die ze nodig hebben voor het analyseren van het lidmaatschap van hun rollen. De rapporten kunnen beperkte weergaven die ervoor zorgen dat alleen de gegevens worden weergegeven door de gebruikers die rapporten maken ze kunnen bekijken.

## <a name="bhold-reporting-installation-requirements"></a>Installatievereisten BHOLD-rapportage

Voordat u de module BHOLD Reporting installeert, moet u de belangrijkste BHOLD-module installeren op de server waarop u van plan bent om de BHOLD rapportagemodule te installeren. Zie voor meer informatie over het installeren van de module BHOLD Core [BHOLD Core-installatie](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

>[!IMPORTANT]
Als u BHOLD-rapportage en BHOLD Attestation installeert, moet u BHOLD Reporting installeren voordat u BHOLD Attestation installeert.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met de rapportage van BHOLD-module installeren, moet u worden voorbereid voor de informatie die de wizard Setup van BHOLD Reporting vereist om de installatie te voltooien. Het werkblad voor het volgende kunt u gegevens vastleggen, zodat u gereed om te leveren wanneer deze nodig is.

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beveiligingsprovider worden gebruikt op de computer van het domein /** | Wanneer u selecteert, geeft u aan dat Active Directory Domain Services-beveiliging de toegang tot BHOLD Core beheert.                                                                                                                | Schakel het selectievakje in. </br>**Belangrijk:** mislukt de installatie als dit selectievakje niet is ingeschakeld.                                                                                                                                                                                                                   |
| **Domein**                                  | Hiermee geeft u het domein met het serviceaccount dat u hebt gemaakt bij het installeren van BHOLD-Core. Zie voor meer informatie [BHOLD Core-installatie](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | Naam van het domein wordt automatisch opgegeven door de wizard. Wijzig de naam alleen als dit onjuist is. **Belangrijk:** de domeinnaam opgeven met de naam van de NetBIOS-(korte), niet de volledig gekwalificeerde domeinnaam (FQDN). Bijvoorbeeld, als de FQDN-naam van het domein fabrikam.com is, geef de domeinnaam als FABRIKAM. |
| **Gebruiker**                                    | Hiermee geeft u de aanmeldingsnaam van de gebruikersaccount van de Core BHOLD-service.                                                                                                                                                          | Schrijf hier de accountnaam van de gebruiker:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                                | Hiermee geeft u het wachtwoord van het serviceaccount van de gebruiker.                                                                                                                                                                       | Schrijf hier het wachtwoord: </br>**Belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Installatie van BHOLD-rapportage

Meld u aan als lid van de groep Domeinadministrators voor het installeren van de rapportage van BHOLD-module, downloadt u het volgende bestand en als administrator uitvoeren op de server die u van plan bent de BHOLD rapportagemodule installeren op:

- BholdReporting*\<versie\>*\_Release.msi

Vervang * \<versie\> * met het versienummer van de rapportage van BHOLD-versie die u installeert.

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- [BHOLD-installatiehandleiding](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)