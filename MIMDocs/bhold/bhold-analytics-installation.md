---
title: Installatie van BHOLD Analytics | Microsoft Docs
description: De BHOLD Analytics-module biedt op regels gebaseerde tests van gegevens toegang
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 7d26abb58fa976d40638b9512d5684d86f483378
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79041927"
---
# <a name="bhold-analytics-installation"></a>Installatie van BHOLD Analytics

De module BHOLD Analytics biedt op regels gebaseerde tests van gegevens toegang om ervoor te zorgen dat uw organisatie de toegang tot de gegevens effectief kan beheren en voldoet aan de interne en externe toegangs vereisten. De automatische impact analyse die wordt gegenereerd door de module BHOLD Analytics geeft u een overzicht van het aantal gebruikers dat wordt beïnvloed door het afdwingen van een voorgestelde regel, beide die zouden kunnen voldoen aan de regel en degenen die de regel zouden schenden. De BHOLD Analytics-module kan ook een gedetailleerde lijst bevatten met de gebruikers die voldoen aan de regel en degenen die de regel zouden schenden.

## <a name="bhold-analytics-installation-requirements"></a>Installatie vereisten voor BHOLD Analytics

Voordat u de BHOLD Analytics-module installeert, moet u de BHOLD-kern module installeren op de server waarop u van plan bent de BHOLD Analytics-module te installeren. Zie [BHOLD Core-installatie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)voor meer informatie over het installeren van de BHOLD core-module.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met het installeren van de BHOLD Analytics-module, moet u voor bereid zijn om de informatie op te geven die de wizard BHOLD Analytics Setup nodig heeft om de installatie te volt ooien. Het volgende werk blad helpt u bij het vastleggen van die informatie, zodat u deze kunt opgeven wanneer dat nodig is.

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beveiligings provider op domein/computer gebruiken** | Hiermee geeft u op dat Active Directory Domain Services beveiliging de toegang tot BHOLD core beheert.                                                                                                                | Schakel het selectievakje in. **Belang rijk:** Als dit selectie vakje niet is ingeschakeld, mislukt de installatie.                                                                                                                                                                                                                   |
| **Domain**                                  | Hiermee geeft u het domein op dat het service account bevat dat u hebt gemaakt bij het installeren van de BHOLD-kern. Zie [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)(Engelstalig) voor meer informatie. | De domein naam wordt automatisch geleverd door de wizard. Wijzig de naam alleen als deze onjuist is. **Belang rijk:** Geef de domein naam op met behulp van de NetBIOS (korte) naam, niet de Fully Qualified Domain Name (FQDN). Als de FQDN van het domein bijvoorbeeld fabrikam.com is, geeft u de domein naam op als FABRIKAM. |
| **Gebruiker**                                    | Hiermee geeft u de aanmeldings naam van het gebruikers account van de BHOLD-basis service.                                                                                                                                                          | Schrijf hier de naam van het gebruikers account:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                                | Hiermee geeft u het wacht woord van het Service gebruikers account.                                                                                                                                                                       | Schrijf hier het wacht woord: **belang rijk:** zorg ervoor dat u dit wacht woord op een verborgen, veilige locatie blijft.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Installatie van BHOLD Analytics

Als u de BHOLD Analytics-module wilt installeren, meldt u zich aan als lid van de groep domein Administrators, downloadt u het volgende bestand en voert u het uit als beheerder op de server waarop u de BHOLD Analytics-module wilt installeren:

- Release. msi van BholdAnalytics\_<em>\<-versie\></em>

Vervang * \<versie\> * door het versie nummer van de BHOLD Analytics-release die u installeert.

Als u het programma bestand als beheerder wilt uitvoeren, klikt u met de rechter muisknop op het bestand en klikt u vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- [BHOLD Core-installatie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [Installatie handleiding voor BHOLD](bhold-installation-guide.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
