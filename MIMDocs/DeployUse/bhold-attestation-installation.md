---
title: BHOLD attestation installatie | Microsoft Docs
description: BHOLD attestation module kunt u revisoren aanwijzen en beoordelingen uitvoeren
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 93d0b9a17d82911b71b1b220465b6d637687444b
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-attestation-installation"></a>BHOLD attestation installatie

De Attestation BHOLD-module kunt u revisoren aanwijzen en terugkerende beoordelingen van de relaties tussen gebruikers en machtigingen per toepassing en de accounts uitvoeren.

## <a name="bhold-attestation-installation-requirements"></a>BHOLD Attestation installatievereisten

Voordat u de module BHOLD Attestation installeert, moet u de belangrijkste BHOLD-module installeren op de server waarop u van plan bent de Attestation BHOLD-module te installeren. Zie voor meer informatie over het installeren van de module BHOLD Core [BHOLD Core-installatie](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). Omdat de BHOLD Attestation module contactpersonen verzendt e-mail naar gebruikers berichten, moet uw omgeving een e-mailserver van Simple Mail Transfer Protocol (SMTP), zoals Microsoft Exchange Server hebben.

>[!IMPORTANT]
Als u BHOLD-rapportage en BHOLD Attestation installeert, moet u BHOLD Reporting installeren voordat u BHOLD Attestation installeert.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met de Attestation BHOLD-module installeren, moet u worden voorbereid voor de informatie die de wizard Setup van BHOLD-verklaring vereist om de installatie te voltooien. Het werkblad voor het volgende kunt u gegevens vastleggen, zodat u gereed om te leveren wanneer deze nodig is.

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beveiligingsprovider worden gebruikt op de computer van het domein /** | Wanneer u selecteert, geeft u aan dat Active Directory Domain Services-beveiliging de toegang tot BHOLD Core beheert.                                                                                                                | Schakel het selectievakje in. **Belangrijk:** mislukt de installatie als dit selectievakje niet is ingeschakeld.                                                                                                                                                                                                                   |
| **Domein**                                  | Hiermee geeft u het domein met het serviceaccount dat u hebt gemaakt bij het installeren van BHOLD-Core. Zie voor meer informatie [BHOLD Core-installatie](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | Naam van het domein wordt automatisch opgegeven door de wizard. Wijzig de naam alleen als dit onjuist is. **Belangrijk:** de domeinnaam opgeven met de naam van de NetBIOS-(korte), niet de volledig gekwalificeerde domeinnaam (FQDN). Bijvoorbeeld, als de FQDN-naam van het domein fabrikam.com is, geef de domeinnaam als FABRIKAM. |
| **Gebruiker**                                    | Hiermee geeft u de aanmeldingsnaam van de gebruikersaccount van de Core BHOLD-service.                                                                                                                                                          | Schrijf hier de accountnaam van de gebruiker:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                                | Hiermee geeft u het wachtwoord van het serviceaccount van de gebruiker.                                                                                                                                                                       | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>BHOLD Attestation-installatie

Voor het installeren van de module BHOLD Attestation aanmelden als lid van de groep Domeinadministrators, moet het volgende bestand downloaden en uitvoeren als beheerder op de server die u wilt de Attestation BHOLD-module installeren op:

- BholdAttestation*\<versie\>*\_Release.msi

Vervang * \<versie\> * met het versienummer van de Attestation BHOLD-versie die u installeert.

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- [BHOLD-installatiehandleiding](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)