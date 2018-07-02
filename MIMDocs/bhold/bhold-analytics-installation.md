---
title: Installatie van BHOLD Analytics | Microsoft Docs
description: BHOLD-analyse-module biedt toegang tot gegevens op basis van een regel testen
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 24e43d8ad886fcd7bfa7a9e694754b956f1659cd
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290268"
---
# <a name="bhold-analytics-installation"></a>Installatie van BHOLD-analyse

De module BHOLD-analyse biedt toegang tot gegevens om ervoor te zorgen dat uw organisatie toegang tot de gegevens effectief beheren en aan de vereisten voor interne en externe toegang voldoet op basis van een regel testen. De geautomatiseerde impactanalyse gegenereerd door de module BHOLD-analyse biedt u een overzicht waarin het aantal gebruikers dat wordt beïnvloed door het afdwingen van een voorgestelde regel zowel degenen die zou voldoen aan de regel en degenen die de regel zou schenden. De module BHOLD-analyse biedt ook een gedetailleerde lijst van de gebruikers die zou voldoen aan de regel en degenen die de regel zou schenden.

## <a name="bhold-analytics-installation-requirements"></a>Installatievereisten BHOLD-analyse

Voordat u de module BHOLD-analyse installeert, moet u de belangrijkste BHOLD-module installeren op de server waarop u wilt installeren van de module BHOLD-analyse. Zie voor meer informatie over het installeren van de module BHOLD Core [BHOLD Core-installatie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met het installeren van de module BHOLD-analyse, moet u worden voorbereid voor de informatie die de wizard Setup van BHOLD-analyse is vereist om de installatie te voltooien. Het werkblad voor het volgende kunt u gegevens vastleggen, zodat u gereed om te leveren wanneer deze nodig is.

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beveiligingsprovider worden gebruikt op de computer van het domein /** | Wanneer u selecteert, geeft u aan dat Active Directory Domain Services-beveiliging de toegang tot BHOLD Core beheert.                                                                                                                | Schakel het selectievakje in. **Belangrijk:** mislukt de installatie als dit selectievakje niet is ingeschakeld.                                                                                                                                                                                                                   |
| **Domein**                                  | Hiermee geeft u het domein met het serviceaccount dat u hebt gemaakt bij het installeren van BHOLD-Core. Zie voor meer informatie [BHOLD Core-installatie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Naam van het domein wordt automatisch opgegeven door de wizard. Wijzig de naam alleen als dit onjuist is. **Belangrijk:** de domeinnaam opgeven met de naam van de NetBIOS-(korte), niet de volledig gekwalificeerde domeinnaam (FQDN). Bijvoorbeeld, als de FQDN-naam van het domein fabrikam.com is, geef de domeinnaam als FABRIKAM. |
| **Gebruiker**                                    | Hiermee geeft u de aanmeldingsnaam van de gebruikersaccount van de Core BHOLD-service.                                                                                                                                                          | Schrijf hier de accountnaam van de gebruiker:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                                | Hiermee geeft u het wachtwoord van het serviceaccount van de gebruiker.                                                                                                                                                                       | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Installatie van BHOLD-analyse

Voor het installeren van de module BHOLD-analyse, meld u aan als lid van de groep Domeinadministrators, moet het volgende bestand downloaden en uitvoeren als beheerder op de server die u wilt de BHOLD-analyse-module installeren op:

- BholdAnalytics<em>\<versie\></em>\_Release.msi

Vervang *\<versie\>* met het versienummer van de release van BHOLD-analyse die u installeert.

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

# <a name="next-steps"></a>Volgende stappen

- [BHOLD-Core-installatie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [BHOLD-installatiehandleiding](bhold-installation-guide.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)