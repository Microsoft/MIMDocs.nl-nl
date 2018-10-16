---
title: Installatie van BHOLD-analyse | Microsoft Docs
description: BHOLD-analyse-module biedt toegang tot gegevens op basis van een regel testen
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 5a31ab2ba6f8ff0a8388d4976e4c44adaf044d87
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332269"
---
# <a name="bhold-analytics-installation"></a>Installatie van BHOLD-analyse

De BHOLD-analyse-module biedt toegang tot de gegevens om ervoor te zorgen dat uw organisatie toegang tot de gegevens effectief beheren en aan de toegangsvereisten voor interne en externe voldoet op basis van regels testen. De geautomatiseerde impactanalyse gegenereerd door de module BHOLD-analyse geeft een overzicht waarin het aantal gebruikers dat wordt beïnvloed door het afdwingen van een voorgestelde regel, zowel die zou voldoen aan de regel en degenen die de regel schendt. De BHOLD-analyse-module biedt ook een gedetailleerd overzicht van de gebruikers die u wilt voldoen aan de regel en degenen die de regel schendt.

## <a name="bhold-analytics-installation-requirements"></a>Vereisten voor installatie van BHOLD-analyse

Voordat u de BHOLD-analyse-module installeert, moet u de BHOLD-Core-module installeren op de server waarop u van plan bent om de BHOLD-analyse-module te installeren. Zie voor meer informatie over het installeren van de module BHOLD Core [basisinstallatie van BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met de BHOLD-analyse-module installeren, moet u worden voorbereid voor de informatie die de wizard Setup van BHOLD-analyse is vereist om de installatie te voltooien. Het werkblad voor het volgende kunt u gegevens vastleggen, zodat u klaar om aan te geven wanneer dat nodig is.

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Security Provider worden gebruikt op de computer aan het domein /** | Als er hebt geselecteerd, geeft de Active Directory Domain Services-beveiliging wordt toegangsbeheer voor BHOLD-Core.                                                                                                                | Schakel het selectievakje in. **Belangrijk:** mislukt de installatie als dit selectievakje niet is geselecteerd.                                                                                                                                                                                                                   |
| **Domein**                                  | Hiermee geeft u het domein met het serviceaccount dat u hebt gemaakt bij de installatie van BHOLD-Core. Zie voor meer informatie, [basisinstallatie van BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Naam van het domein wordt automatisch opgegeven door de wizard. De naam alleen wijzigen als dit onjuist is. **Belangrijk:** de domeinnaam opgeven met behulp van de naam van de NetBIOS-(kort), niet de volledig gekwalificeerde domeinnaam (FQDN). Bijvoorbeeld, als de FQDN-naam van het domein fabrikam.com, de domeinnaam opgeven als FABRIKAM. |
| **Gebruiker**                                    | Hiermee geeft u de naam van het gebruikersaccount van BHOLD-Core-service.                                                                                                                                                          | Schrijf hier de accountnaam van de gebruiker:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                                | Hiermee geeft u het wachtwoord van het serviceaccount van de gebruiker.                                                                                                                                                                       | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Installatie van BHOLD-analyse

Meld u aan als een lid van de groep Domeinadministrators, downloadt u het volgende bestand voor het installeren van de module BHOLD-analyse, en als administrator uitvoeren op de server die u van plan bent de BHOLD-analyse-module installeren op:

- BholdAnalytics<em>\<versie\></em>\_Release.msi

Vervang *\<versie\>* met het versienummer van de release van BHOLD-analyse die u installeert.

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

# <a name="next-steps"></a>Volgende stappen

- [Basisinstallatie van BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [Handleiding voor BHOLD-installatie](bhold-installation-guide.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)