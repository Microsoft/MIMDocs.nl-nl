---
title: BHOLD-installatie van reporting | Microsoft Docs
description: BHOLD-rapportagemodule kunt u voor het genereren van rapporten over rollen en -autorisatiebeleid
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 1f69f9f08cba24898509c771e4477b81c5ed272f
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49357999"
---
# <a name="bhold-reporting-installation"></a>Rapportage van de installatie van BHOLD

De rapportage van BHOLD-module biedt u de mogelijkheid om rapporten te genereren over de functies en andere autorisatiebeleid in BHOLD. Deze rapporten zijn vaak nuttig voor controle of voor het demonstreren van naleving van wettelijke vereisten. Deze module is een uitbreiding ook de mogelijkheid om autorisatie te beheren binnen uw organisatie doordat gebruikers de gegevens die ze nodig hebben voor het analyseren van het lidmaatschap van hun rollen. Weergaven die ervoor zorgen dat de gebruikers die rapporten maakt alleen de gegevens worden weergegeven door de rapporten kunnen hebben beperkte ze mogen zien.

## <a name="bhold-reporting-installation-requirements"></a>Vereisten voor installatie van BHOLD-rapportage

Voordat u de module BHOLD-rapportage installeert, moet u de BHOLD-Core-module installeren op de server waarop u van plan bent om de module BHOLD-rapportage te installeren. Zie voor meer informatie over het installeren van de module BHOLD Core [basisinstallatie van BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

> [!IMPORTANT]
> Als u zowel BHOLD-rapportage en BHOLD-Attestation installeert, moet u BHOLD-rapportage installeren voor de installatie van BHOLD-Attestation.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met de rapportage van BHOLD-module installeren, moet u worden voorbereid voor de informatie die de wizard installatie van BHOLD Reporting is vereist om de installatie te voltooien. Het werkblad voor het volgende kunt u gegevens vastleggen, zodat u klaar om aan te geven wanneer dat nodig is.

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Security Provider worden gebruikt op de computer aan het domein /** | Als er hebt geselecteerd, geeft de Active Directory Domain Services-beveiliging wordt toegangsbeheer voor BHOLD-Core.                                                                                                                | Schakel het selectievakje in. </br>**Belangrijk:** mislukt de installatie als dit selectievakje niet is geselecteerd.                                                                                                                                                                                                                   |
| **Domein**                                  | Hiermee geeft u het domein met het serviceaccount dat u hebt gemaakt bij de installatie van BHOLD-Core. Zie voor meer informatie, [basisinstallatie van BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Naam van het domein wordt automatisch opgegeven door de wizard. De naam alleen wijzigen als dit onjuist is. **Belangrijk:** de domeinnaam opgeven met behulp van de naam van de NetBIOS-(kort), niet de volledig gekwalificeerde domeinnaam (FQDN). Bijvoorbeeld, als de FQDN-naam van het domein fabrikam.com, de domeinnaam opgeven als FABRIKAM. |
| **Gebruiker**                                    | Hiermee geeft u de naam van het gebruikersaccount van BHOLD-Core-service.                                                                                                                                                          | Schrijf hier de accountnaam van de gebruiker:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                                | Hiermee geeft u het wachtwoord van het serviceaccount van de gebruiker.                                                                                                                                                                       | Schrijf hier het wachtwoord: </br>**Belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Installatie van BHOLD-rapportage

Meld u aan als een lid van de groep Domeinadministrators voor het installeren van de module BHOLD-rapportage, downloadt u het volgende bestand en als administrator uitvoeren op de server die u van plan bent de rapportage van BHOLD-module installeren op:

- BholdReporting<em>\<versie\></em>\_Release.msi

Vervang *\<versie\>* met het versienummer van de rapportage van BHOLD-versie die u installeert.

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- [Handleiding voor BHOLD-installatie](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
