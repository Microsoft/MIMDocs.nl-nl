---
title: Installatie van BHOLD-attestation | Microsoft Docs
description: BHOLD-attestation-module kunt u revisoren aanwijzen en uitvoeren van beoordelingen
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e4c3a6248585d55fddbbca3153f33734d7c5c429
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358105"
---
# <a name="bhold-attestation-installation"></a>Installatie van BHOLD-attestation

De BHOLD-Attestation-module kunt u revisoren aanwijzen en uitvoeren van terugkerende beoordelingen van de relaties tussen gebruikers en machtigingen per toepassing en -accounts.

## <a name="bhold-attestation-installation-requirements"></a>Vereisten voor installatie van BHOLD-Attestation

Voordat u de BHOLD-Attestation-module installeert, moet u de BHOLD-Core-module installeren op de server waarop u van plan bent om de BHOLD-Attestation-module te installeren. Zie voor meer informatie over het installeren van de module BHOLD Core [basisinstallatie van BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Omdat het BHOLD-Attestation module contactpersonen verzonden e-mailbericht naar gebruikers berichten, moet uw omgeving een e-mailserver van Simple Mail Transfer Protocol (SMTP), zoals Microsoft Exchange Server hebben.

> [!IMPORTANT]
> Als u zowel BHOLD-rapportage en BHOLD-Attestation installeert, moet u BHOLD-rapportage installeren voor de installatie van BHOLD-Attestation.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met het installeren van de module BHOLD-Attestation, moet u worden voorbereid voor de informatie die de wizard Setup van BHOLD-Attestation is vereist om de installatie te voltooien. Het werkblad voor het volgende kunt u gegevens vastleggen, zodat u klaar om aan te geven wanneer dat nodig is.

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Security Provider worden gebruikt op de computer aan het domein /** | Als er hebt geselecteerd, geeft de Active Directory Domain Services-beveiliging wordt toegangsbeheer voor BHOLD-Core.                                                                                                                | Schakel het selectievakje in. **Belangrijk:** mislukt de installatie als dit selectievakje niet is geselecteerd.                                                                                                                                                                                                                   |
| **Domein**                                  | Hiermee geeft u het domein met het serviceaccount dat u hebt gemaakt bij de installatie van BHOLD-Core. Zie voor meer informatie, [basisinstallatie van BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Naam van het domein wordt automatisch opgegeven door de wizard. De naam alleen wijzigen als dit onjuist is. **Belangrijk:** de domeinnaam opgeven met behulp van de naam van de NetBIOS-(kort), niet de volledig gekwalificeerde domeinnaam (FQDN). Bijvoorbeeld, als de FQDN-naam van het domein fabrikam.com, de domeinnaam opgeven als FABRIKAM. |
| **Gebruiker**                                    | Hiermee geeft u de naam van het gebruikersaccount van BHOLD-Core-service.                                                                                                                                                          | Schrijf hier de accountnaam van de gebruiker:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                                | Hiermee geeft u het wachtwoord van het serviceaccount van de gebruiker.                                                                                                                                                                       | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Installatie van BHOLD-Attestation

Meld u aan als een lid van de groep Domeinadministrators voor het installeren van de module BHOLD-Attestation, het volgende bestand downloaden en als administrator uitvoeren op de server die u van plan bent de BHOLD-Attestation-module installeren op:

- BholdAttestation<em>\<versie\></em>\_Release.msi

Vervang *\<versie\>* met het versienummer van de release van BHOLD-Attestation die u installeert.

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- [Handleiding voor BHOLD-installatie](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
