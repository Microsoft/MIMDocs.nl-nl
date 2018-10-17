---
title: Installatie van BHOLD-model generator | Microsoft Docs
description: BHOLD-model kunt u gegevens van de structuur uit verschillende bronnen
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3d2510d6ea604dd88e56436812ed8bc975bc5c2b
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358519"
---
# <a name="bhold-model-generator-installation"></a>Generator voor installatie van BHOLD-Model

Met behulp van de module Generator van BHOLD-Model, kunt u gegevens uit gezaghebbende bronnen met gebruikers- en organisatiegegevens, samen met toegangsbeheerlijsten (ACL's) in een model dat kan worden gebruikt bij het beheren van BHOLD structureren.

## <a name="bhold-model-generator-installation-requirements"></a>Vereisten voor installatie van BHOLD-Model Generator 

Voordat u de Generator voor BHOLD-Model-module installeert, moet u de volgende installeren:

1. BHOLD-Core-module op de server waarop u van plan bent om de Generator voor BHOLD-Model-module te installeren. Zie voor meer informatie over het installeren van de module BHOLD Core [basisinstallatie van BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. De Microsoft OLE DB-Provider voor Microsoft Jet moet worden geïnstalleerd. Zie voor meer informatie [in dit artikel](http://support.microsoft.com/kb/271908).

> [!WARNING]
> Moet de Generator voor BHOLD-Model niet installeren in uw productienetwerk. Generator voor BHOLD-Model is bedoeld om offline in een faseringsomgeving worden gebruikt voor het maken van een genormaliseerde rol-model dat u in het model van uw enterprise-functie importeren kunt. Generator voor BHOLD-Model uitgevoerd in uw productienetwerk kan leiden tot verlies van het model van uw bestaande functie.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u de Generator voor BHOLD-Model-module installeert, moet u worden voorbereid voor de informatie die de installatiewizard van BHOLD-Model Generator is vereist om de installatie te voltooien. Het werkblad voor het volgende kunt u gegevens vastleggen, zodat u klaar om aan te geven wanneer dat nodig is. U moet ook om ervoor te zorgen

Microsoft Access-Database-Engine 2010 Redistributable installeren

 

*Van \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Accountinstellingen**

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Security Provider worden gebruikt op de computer aan het domein /** | Als er hebt geselecteerd, geeft de Active Directory Domain Services-beveiliging wordt toegangsbeheer voor BHOLD-Core.                                                                                                                | Schakel het selectievakje in. **Belangrijk:** mislukt de installatie als dit selectievakje niet is geselecteerd.                                                                                                                                                                                                                   |
| **Domein**                                  | Hiermee geeft u het domein met het serviceaccount dat u hebt gemaakt bij de installatie van BHOLD-Core. Zie voor meer informatie, [basisinstallatie van BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Naam van het domein wordt automatisch opgegeven door de wizard. De naam alleen wijzigen als dit onjuist is. **Belangrijk:** de domeinnaam opgeven met behulp van de naam van de NetBIOS-(kort), niet de volledig gekwalificeerde domeinnaam (FQDN). Bijvoorbeeld, als de FQDN-naam van het domein fabrikam.com, de domeinnaam opgeven als FABRIKAM. |
| **Gebruiker**                                    | Hiermee geeft u de naam van het gebruikersaccount van BHOLD-Core-service.                                                                                                                                                          | Schrijf hier de accountnaam van de gebruiker:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                                | Hiermee geeft u het wachtwoord van het serviceaccount van de gebruiker.                                                                                                                                                                       | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                  |

**Back-database-instellingen**

| Item                                        | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                                  | Waarde                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Geïntegreerde beveiliging gebruiken**                 | Hiermee geeft u op dat Windows-verificatie wordt gebruikt voor toegang tot de database.                                                                                                                                                                                                                                                                                                                                                        | Selecteer het selectievakje in als Windows-verificatie wordt gebruikt om verbinding maken met de SQL-Server. Schakel dit selectievakje uit als SQL Server-verificatie wordt gebruikt. De database moet zijn gemaakt voordat het uitvoeren van BHOLD Core Setup als SQL Server-verificatie wordt gebruikt. **Opmerking:** als Windows-verificatie wordt gebruikt, u moet zijn aangemeld met een account met de serverrol sysadmin op de database-server. **Belangrijk:** Gebruik SQL Server-verificatie alleen in een testomgeving. Microsoft raadt het gebruik van Windows-verificatie in productie-implementaties. |
| **Databasegebruiker** en **databasewachtwoord** | Hiermee geeft u de gebruikersnaam en wachtwoord van een gebruiker met de serverrol sysadmin op de databaseserver. Deze waarden worden geleverd alleen wanneer SQL Server-verificatie wordt gebruikt.                                                                                                                                                                                                                                                  | Schrijf hier de gebruikersnaam van de SQL Server: SQL Server-gebruikerswachtwoord hier schrijven: </br></br> **Belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Databaseserver** en **databasenaam**   | Hiermee geeft u de NetBIOS-naam van de database-server en de naam van de back-up die BHOLD Model Generator door Setup wordt gemaakt. Als u niet het standaardexemplaar van de database-server gebruikt, geeft u de database-server-exemplaar in de notatie  *\<server\>*\\*\<exemplaar\>* .  Microsoft raadt aan dat u de back-database met behulp van de naam van de Core BHOLD-database gevolgd door de naam \_back-up, bijvoorbeeld B1_BACKUP. | De server (of server en exemplaar) naam hier schrijven: </br> Schrijf hier de naam van de database:

## <a name="bhold-model-generator-setup"></a>Installatie van BHOLD-Model Generator

Als u wilt de Generator voor BHOLD-Model-module installeert, meld u aan als een lid van de groep Domeinadministrators, het volgende bestand downloaden en als administrator uitvoeren op de server die u van plan bent de BHOLD-Core-module installeren op:

- BholdModelGenerator  *\<versie\>*\_Release.msi

Vervang *\<versie\>* met het versienummer van de release van BHOLD-Model Generator die u installeert.

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- Voor informatie over het maken van de invoerbestanden [technische naslaginformatie voor Microsoft BHOLD-Suite](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [Handleiding voor BHOLD-installatie](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
