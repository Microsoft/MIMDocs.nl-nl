---
title: Installatie van BHOLD model generator | Microsoft Docs
description: Met BHOLD-model kunt u gegevens uit verschillende bronnen structureren
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3d2510d6ea604dd88e56436812ed8bc975bc5c2b
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516735"
---
# <a name="bhold-model-generator-installation"></a>Installatie van BHOLD-model generator

Met behulp van de module BHOLD model generator kunt u gegevens van gezaghebbende bronnen die gebruikers-en organisatie gegevens bevatten samen met toegangs beheer lijsten (Acl's), structureren in een model dat kan worden gebruikt bij het beheren van BHOLD.

## <a name="bhold-model-generator-installation-requirements"></a>Installatie vereisten voor BHOLD model generator 

Voordat u de BHOLD-model Generator module installeert, moet u het volgende installeren:

1. BHOLD core-module op de server waarop u de BHOLD model Generator-module wilt installeren. Zie [BHOLD Core-installatie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)voor meer informatie over het installeren van de BHOLD core-module.

2. De micro soft OLE DB-provider voor micro soft Jet moet zijn geïnstalleerd. Raadpleeg [dit artikel](http://support.microsoft.com/kb/271908)voor meer informatie.

> [!WARNING]
> Installeer BHOLD model generator niet in uw productie netwerk. BHOLD model Generator is bedoeld om offline te worden gebruikt in een faserings omgeving om een genormaliseerd rolcentrum te maken dat u in uw bedrijfsfunctie model kunt importeren. Het uitvoeren van BHOLD-model generator in uw productie netwerk kan leiden tot verlies van uw bestaande rollen model.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u de BHOLD-model Generator module installeert, moet u voor bereid zijn om de informatie op te geven die door de installatie wizard van de BHOLD-model generator moet worden uitgevoerd om de installatie te volt ooien. Het volgende werk blad helpt u bij het vastleggen van die informatie, zodat u deze kunt opgeven wanneer dat nodig is. U moet er ook voor zorgen dat

Micro soft Access-data base-engine 2010 Redistributable

 

*Van \<* <http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems> *\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Account instellingen**

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beveiligings provider op domein/computer gebruiken** | Hiermee geeft u op dat Active Directory Domain Services beveiliging de toegang tot BHOLD core beheert.                                                                                                                | Schakel het selectievakje in. **Belang rijk:** Als dit selectie vakje niet is ingeschakeld, mislukt de installatie.                                                                                                                                                                                                                   |
| **Domein**                                  | Hiermee geeft u het domein op dat het service account bevat dat u hebt gemaakt bij het installeren van de BHOLD-kern. Zie [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)(Engelstalig) voor meer informatie. | De domein naam wordt automatisch geleverd door de wizard. Wijzig de naam alleen als deze onjuist is. **Belang rijk:** Geef de domein naam op met behulp van de NetBIOS (korte) naam, niet de Fully Qualified Domain Name (FQDN). Als de FQDN van het domein bijvoorbeeld fabrikam.com is, geeft u de domein naam op als FABRIKAM. |
| **Gebruiker**                                    | Hiermee geeft u de aanmeldings naam van het gebruikers account van de BHOLD-basis service.                                                                                                                                                          | Schrijf hier de naam van het gebruikers account:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                                | Hiermee geeft u het wacht woord van het Service gebruikers account.                                                                                                                                                                       | Schrijf hier het wacht woord: **belang rijk:** zorg ervoor dat u dit wacht woord op een verborgen, veilige locatie blijft.                                                                                                                                                                                                                  |

**Instellingen voor back-updatabase**

| Item                                        | Description                                                                                                                                                                                                                                                                                                                                                                                                                  | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Geïntegreerde beveiliging gebruiken**                 | Hiermee geeft u op dat Windows-verificatie wordt gebruikt voor toegang tot de data base.                                                                                                                                                                                                                                                                                                                                                        | Schakel het selectie vakje in als Windows-verificatie wordt gebruikt om verbinding te maken met de SQL Server. Schakel het selectie vakje uit als SQL Server verificatie wordt gebruikt. De data base moet zijn gemaakt voordat u de BHOLD-installatie uitvoert als SQL Server verificatie wordt gebruikt. **Opmerking:** Als Windows-verificatie wordt gebruikt, moet u zijn aangemeld met een account dat beschikt over de serverrol sysadmin op de database server. **Belang rijk:** Gebruik SQL Server verificatie alleen in test omgevingen. Micro soft raadt u ten zeerste aan om Windows-verificatie te gebruiken in productie-implementaties. |
| **Database gebruiker** en **database wachtwoord** | Hiermee geeft u de gebruikers naam en het wacht woord van een gebruiker met de serverrol sysadmin op de database server. Deze waarden worden alleen opgegeven als SQL Server verificatie wordt gebruikt.                                                                                                                                                                                                                                                  | Schrijf hier de SQL Server gebruikers naam: Schrijf hier het SQL Server gebruikers wachtwoord: </br></br> **Belang rijk:** Zorg ervoor dat u dit wacht woord op een verborgen, veilige locatie blijft.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Database server** -en **database naam**   | Hiermee geeft u de NetBIOS-naam van de database server en de naam van de back-updatabase op die door de installatie van de BHOLD-model generator wordt gemaakt. Als u geen gebruik maakt van het standaard exemplaar van de database server, geeft u het Database Server exemplaar op in het formulier *\<server\>* \\ *\<-exemplaar\>* .  Micro soft raadt u aan de back-updatabase te benoemen met de naam van de BHOLD Core-Data Base, gevolgd door \_back-up, bijvoorbeeld B1_BACKUP. | Schrijf hier de naam van de server (of de server en het exemplaar): </br> Schrijf de naam van de data base hier:

## <a name="bhold-model-generator-setup"></a>Setup van BHOLD-model generator

Als u de module BHOLD-model generator wilt installeren, meldt u zich aan als lid van de groep domein Administrators, downloadt u het volgende bestand en voert u het uit als beheerder op de server waarop u de BHOLD-kern module wilt installeren:

- BholdModelGenerator *\<versie\>* \_release. msi

Vervang *\<versie\>* door het versie nummer van de release van de BHOLD-model generator die u installeert.

Als u het programma bestand als beheerder wilt uitvoeren, klikt u met de rechter muisknop op het bestand en klikt u vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- Voor informatie over het maken van invoer bestanden met [technische documentatie over micro soft BHOLD Suite](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [Installatie handleiding voor BHOLD](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
