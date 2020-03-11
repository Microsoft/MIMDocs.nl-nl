---
title: Installatie van BHOLD-rapportage | Microsoft Docs
description: Met BHOLD Reporting module kunt u rapporten genereren over rollen en autorisatie beleid
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 7bac40105aa53add914599c59e812587b6be9585
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042030"
---
# <a name="bhold-reporting-installation"></a>Installatie van BHOLD-rapporten

De BHOLD-rapportage module biedt u de mogelijkheid om rapporten te genereren over rollen en andere autorisatie beleidsregels in BHOLD. Deze rapporten zijn vaak nuttig voor het controleren of voor het demonstreren van de naleving van wettelijke vereisten. Met deze module kunt u ook de autorisatie in uw organisatie beheren door gebruikers de informatie te geven die ze nodig hebben voor het analyseren van het lidmaatschap van hun rollen. De rapporten kunnen een beperkt aantal weer gaven hebben die ervoor zorgen dat de gebruikers die rapporten maken alleen de informatie weer geven die ze mogen zien.

## <a name="bhold-reporting-installation-requirements"></a>Installatie vereisten voor BHOLD-rapportage

Voordat u de BHOLD-rapportage module installeert, moet u de BHOLD-kern module installeren op de server waarop u van plan bent de BHOLD-rapportage module te installeren. Zie [BHOLD Core-installatie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)voor meer informatie over het installeren van de BHOLD core-module.

> [!IMPORTANT]
> Als u zowel BHOLD Reporting als BHOLD Attestation installeert, moet u BHOLD-rapportage installeren voordat u BHOLD-Attestation installeert.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met het installeren van de BHOLD-rapportage module, moet u voor bereid zijn om de informatie op te geven die door de wizard BHOLD Reporting Setup is vereist om de installatie te volt ooien. Het volgende werk blad helpt u bij het vastleggen van die informatie, zodat u deze kunt opgeven wanneer dat nodig is.

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beveiligings provider op domein/computer gebruiken** | Hiermee geeft u op dat Active Directory Domain Services beveiliging de toegang tot BHOLD core beheert.                                                                                                                | Schakel het selectie vakje in. </br>**Belang rijk:** Als dit selectie vakje niet is ingeschakeld, mislukt de installatie.                                                                                                                                                                                                                   |
| **Domein**                                  | Hiermee geeft u het domein op dat het service account bevat dat u hebt gemaakt bij het installeren van de BHOLD-kern. Zie [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)(Engelstalig) voor meer informatie. | De domein naam wordt automatisch geleverd door de wizard. Wijzig de naam alleen als deze onjuist is. **Belang rijk:** Geef de domein naam op met behulp van de NetBIOS (korte) naam, niet de Fully Qualified Domain Name (FQDN). Als de FQDN van het domein bijvoorbeeld fabrikam.com is, geeft u de domein naam op als FABRIKAM. |
| **Gebruiker**                                    | Hiermee geeft u de aanmeldings naam van het gebruikers account van de BHOLD-basis service.                                                                                                                                                          | Schrijf hier de naam van het gebruikers account:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                                | Hiermee geeft u het wacht woord van het Service gebruikers account.                                                                                                                                                                       | Schrijf hier het wacht woord: </br>**Belang rijk:** Zorg ervoor dat u dit wacht woord op een verborgen, veilige locatie blijft.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Installatie van BHOLD-rapporten

Als u de BHOLD-rapportage module wilt installeren, meldt u zich aan als lid van de groep domein Administrators, downloadt u het volgende bestand en voert u het uit als beheerder op de server waarop u de BHOLD-rapportage module wilt installeren:

- BholdReporting<em>\<versie\></em>\_release. msi

Vervang *\<versie\>* door het versie nummer van de BHOLD-rapportage versie die u installeert.

Als u het programma bestand als beheerder wilt uitvoeren, klikt u met de rechter muisknop op het bestand en klikt u vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- [Installatie handleiding voor BHOLD](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
