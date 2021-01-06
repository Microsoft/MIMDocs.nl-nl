---
title: Versie geschiedenis van Identity Manager-BHOLD | Microsoft Docs
description: In dit artikel worden de verschillende wijzigingen gedocumenteerd die zijn aangebracht als onderdeel van updates voor BHOLD binnen MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/1/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 4e3c247370c6c78e7814d894e9f0a6166d2e397a
ms.sourcegitcommit: 8f81767ec92e1b80658aaebb9463aa4d62396d43
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2021
ms.locfileid: "97927605"
---
# <a name="bhold-modules-version-release-history"></a>Release geschiedenis van versie van BHOLD-modules

Het Microsoft Identity Manager team brengt regel matig updates uit die worden vermeld in de [versie](version-history.md) geschiedenis. Dit artikel helpt u bij het bijhouden van de versies die zijn vrijgegeven voor BHOLD, een subonderdeel van de Microsoft Identity Manager. U kunt vervolgens beslissen of u wilt bijwerken naar de nieuwste versie of niet.

## <a name="version-60620"></a>Versie 6.0.62.0

- Status: oktober 2018
- [Downloaden](https://www.microsoft.com/download/details.aspx?id=55950)

## <a name="version-60360"></a>Versie 6.0.36.0

- Status: 7 september 2017

### <a name="enhancements"></a>Verbeteringen  
BHOLD-versie 6.0.36.0 bevat de volgende verbeteringen:

- Platform update x86 naar x64.

### <a name="fixed-issues"></a>Opgeloste problemen
De volgende problemen zijn opgelost in BHOLD versie 6.0.36.0.

#### <a name="bhold-core"></a>BHOLD-kern

- Het installatie programma is bijgewerkt voor x86 naar x64.
- Met de webinterface worden niet alle machtigingen weer gegeven.
- Oplossingen voor de B1Common-bibliotheek.
- Gebruiker kan rol niet toewijzen aan OrgUnit wanneer de rol weinig machtigingen heeft met kardinaliteit.
- De kardinaliteit van de machtiging werkt niet voor max. aantallen gebruikers.

#### <a name="access-management-connector"></a>Toegangs beheer connector

- n.v.t.

#### <a name="bhold-integration-module"></a>Integratie module BHOLD

- Het installatie programma is bijgewerkt voor x86 naar x64.
- INDELING map onjuiste locatie.

#### <a name="bhold-model-generator"></a>BHOLD-model generator

- Model rollen van bovenliggende afdelingen worden niet overgenomen.

#### <a name="bhold-analytics"></a>BHOLD-analyse

- n.v.t.

#### <a name="bhold-attestation"></a>BHOLD-Attestation

- n.v.t.

#### <a name="bhold-reporting"></a>BHOLD rapportage

- Het aangepaste kenmerk ' employeeID ' is niet beschikbaar.
- Grote gegevens kunnen niet goed worden geladen met 3-bestanden sets.

## <a name="next-steps"></a>Volgende stappen

- [Handleiding voor BHOLD-concepten](../bhold/bhold-concepts-guide.md)
- [Installatie handleiding voor BHOLD](../bhold/bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van MIM](version-history.md)

