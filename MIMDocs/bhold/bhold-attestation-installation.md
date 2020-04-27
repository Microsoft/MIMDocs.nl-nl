---
title: Installatie van BHOLD-Attestation | Microsoft Docs
description: Met de BHOLD Attestation-module kunt u revisoren aanwijzen en beoordelingen uitvoeren
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3d7e136ee86272429ff16a49b3c1319a4543648f
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042318"
---
# <a name="bhold-attestation-installation"></a>Installatie van BHOLD-attestation

Met de Attestation-module voor BHOLD kunt u revisoren aanwijzen en terugkerende beoordelingen uitvoeren van de relaties tussen gebruikers en machtigingen en accounts voor per toepassing.

## <a name="bhold-attestation-installation-requirements"></a>Vereisten voor installatie van BHOLD-Attestation

Voordat u de BHOLD-Attestation-module installeert, moet u de BHOLD-kern module installeren op de server waarop u van plan bent de BHOLD Attestation-module te installeren. Zie [BHOLD Core-installatie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)voor meer informatie over het installeren van de BHOLD core-module. Omdat de contact personen van de BHOLD-Attestation-module e-mail berichten naar gebruikers verzenden, moet uw omgeving een e-mail server voor Simple Mail Transfer Protocol (SMTP) hebben, zoals micro soft Exchange Server.

> [!IMPORTANT]
> Als u zowel BHOLD Reporting als BHOLD Attestation installeert, moet u BHOLD-rapportage installeren voordat u BHOLD-Attestation installeert.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met de installatie van de Attestation-module BHOLD, moet u voor bereid zijn om de informatie op te geven die de wizard BHOLD Attestation Setup nodig heeft om de installatie te volt ooien. Het volgende werk blad helpt u bij het vastleggen van die informatie, zodat u deze kunt opgeven wanneer dat nodig is.

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beveiligings provider op domein/computer gebruiken** | Hiermee geeft u op dat Active Directory Domain Services beveiliging de toegang tot BHOLD core beheert.                                                                                                                | Schakel het selectievakje in. **Belang rijk:** Als dit selectie vakje niet is ingeschakeld, mislukt de installatie.                                                                                                                                                                                                                   |
| **Domain**                                  | Hiermee geeft u het domein op dat het service account bevat dat u hebt gemaakt bij het installeren van de BHOLD-kern. Zie [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)(Engelstalig) voor meer informatie. | De domein naam wordt automatisch geleverd door de wizard. Wijzig de naam alleen als deze onjuist is. **Belang rijk:** Geef de domein naam op met behulp van de NetBIOS (korte) naam, niet de Fully Qualified Domain Name (FQDN). Als de FQDN van het domein bijvoorbeeld fabrikam.com is, geeft u de domein naam op als FABRIKAM. |
| **Gebruiker**                                    | Hiermee geeft u de aanmeldings naam van het gebruikers account van de BHOLD-basis service.                                                                                                                                                          | Schrijf hier de naam van het gebruikers account:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                                | Hiermee geeft u het wacht woord van het Service gebruikers account.                                                                                                                                                                       | Schrijf hier het wacht woord: **belang rijk:** zorg ervoor dat u dit wacht woord op een verborgen, veilige locatie blijft.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Installatie van BHOLD-Attestation

Als u de BHOLD Attestation-module wilt installeren, meldt u zich aan als lid van de groep domein Administrators, downloadt u het volgende bestand en voert u het uit als beheerder op de server waarop u de BHOLD Attestation-module wilt installeren:

- Release. msi van BholdAttestation\_<em>\<-versie\></em>

Vervang * \<versie\> * door het versie nummer van de BHOLD Attestation-release die u installeert.

Als u het programma bestand als beheerder wilt uitvoeren, klikt u met de rechter muisknop op het bestand en klikt u vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- [Installatie handleiding voor BHOLD](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
