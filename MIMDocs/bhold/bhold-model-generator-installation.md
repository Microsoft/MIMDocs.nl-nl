---
title: BHOLD model generator installatie | Microsoft Docs
description: BHOLD-model kunt u gegevens van de structuur uit diverse bronnen
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 96363fb3b0067ff5c8f8c2f32e9a855464038653
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-model-generator-installation"></a>BHOLD Model Generator-installatie

Met behulp van de module Generator van BHOLD-Model, kunt u gegevens van de gezaghebbende bronnen met gebruikers- en organisatiegegevens samen met toegangsbeheerlijsten (ACL's) in een model die kan worden gebruikt bij het beheren van BHOLD structureren.

## <a name="bhold-model-generator-installation-requirements"></a>Installatievereisten Generator van BHOLD-Model 

Voordat u de module BHOLD Model Generator installeert, moet u het volgende:

1. BHOLD-Core-module op de server waarop u van plan bent de Generator van BHOLD-Model-module te installeren. Zie voor meer informatie over het installeren van de module BHOLD Core [BHOLD Core-installatie](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

2. De Microsoft OLE DB-Provider voor Microsoft Jet moet worden geïnstalleerd. Zie voor meer informatie [in dit artikel](http://support.microsoft.com/kb/271908).

>[!WARNING]
Installeer geen Generator van BHOLD-Model in het productienetwerk. BHOLD Model Generator is bedoeld om het offline in een testomgeving worden gebruikt voor het maken van een genormaliseerde model voor de functie die u in het model van uw enterprise-functie importeren kunt. BHOLD Model Generator uitgevoerd in uw productienetwerk kan leiden tot verlies van het model van uw bestaande functie.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u de module BHOLD Model Generator installeert, moet u de gegevens die de installatiewizard van BHOLD Model Generator vereist om de installatie te voltooien worden voorbereid. Het werkblad voor het volgende kunt u gegevens vastleggen, zodat u gereed om te leveren wanneer deze nodig is. U moet ook om ervoor te zorgen

Microsoft Access Database Engine 2010 Redistributable

 

*Van \<*  <http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.Microsoft.com/en-us/download/Confirmation.aspx?id=13255>

**Accountinstellingen**

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beveiligingsprovider worden gebruikt op de computer van het domein /** | Wanneer u selecteert, geeft u aan dat Active Directory Domain Services-beveiliging de toegang tot BHOLD Core beheert.                                                                                                                | Schakel het selectievakje in. **Belangrijk:** mislukt de installatie als dit selectievakje niet is ingeschakeld.                                                                                                                                                                                                                   |
| **Domein**                                  | Hiermee geeft u het domein met het serviceaccount dat u hebt gemaakt bij het installeren van BHOLD-Core. Zie voor meer informatie [BHOLD Core-installatie](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | Naam van het domein wordt automatisch opgegeven door de wizard. Wijzig de naam alleen als dit onjuist is. **Belangrijk:** de domeinnaam opgeven met de naam van de NetBIOS-(korte), niet de volledig gekwalificeerde domeinnaam (FQDN). Bijvoorbeeld, als de FQDN-naam van het domein fabrikam.com is, geef de domeinnaam als FABRIKAM. |
| **Gebruiker**                                    | Hiermee geeft u de aanmeldingsnaam van de gebruikersaccount van de Core BHOLD-service.                                                                                                                                                          | Schrijf hier de accountnaam van de gebruiker:                                                                                                                                                                                                                                                                                    |
| **Wachtwoord**                                | Hiermee geeft u het wachtwoord van het serviceaccount van de gebruiker.                                                                                                                                                                       | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                  |

**Back-database-instellingen**

| Item                                        | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                                  | Waarde                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Geïntegreerde beveiliging gebruiken**                 | Hiermee geeft u op dat de Windows-verificatie wordt gebruikt voor toegang tot de database.                                                                                                                                                                                                                                                                                                                                                        | Schakel het selectievakje in als Windows-verificatie wordt gebruikt voor verbinding met de SQL-Server. Schakel het selectievakje in als SQL Server-verificatie wordt gebruikt. De database moet zijn gemaakt voordat uitgevoerd BHOLD Core Setup als SQL Server-verificatie wordt gebruikt. **Opmerking:** als Windows-verificatie wordt gebruikt, u moet zijn aangemeld met een account met de serverrol sysadmin op de databaseserver. **Belangrijk:** Gebruik SQL Server-verificatie alleen in een testomgeving. Microsoft raadt het gebruik van Windows-verificatie in productie-implementaties. |
| **Databasegebruiker** en **databasewachtwoord** | Hiermee geeft u de gebruikersnaam en wachtwoord van een gebruiker met de serverrol sysadmin op de databaseserver. Deze waarden worden geleverd, alleen wanneer SQL Server-verificatie wordt gebruikt.                                                                                                                                                                                                                                                  | Schrijf hier de gebruikersnaam van de SQL Server: SQL Server-gebruikerswachtwoord hier schrijven: </br></br> **Belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Databaseserver** en **databasenaam**   | Hiermee geeft u de NetBIOS-naam van de databaseserver en de naam van de back-updatabase die BHOLD Model Generator Setup wordt gemaakt. Als u niet het standaardexemplaar van de database-server gebruikt, geeft u de database-server-exemplaar in het formulier  *\<server\>*\\*\<exemplaar\>* .  Microsoft raadt aan dat u de back-updatabase met de naam van de Core BHOLD-database gevolgd door de naam \_back-up, bijvoorbeeld B1_BACKUP. | De server (of server en -exemplaar) naam hier schrijven: </br> Schrijf hier de naam van de database:

## <a name="bhold-model-generator-setup"></a>BHOLD Model Generator setup

Meld u aan als lid van de groep Domeinadministrators voor het installeren van de module BHOLD Model Generator, downloadt u het volgende bestand en als administrator uitvoeren op de server die u wilt de Core BHOLD-module installeren op:

- BholdModelGenerator  *\<versie\>*\_Release.msi

Vervang  *\<versie\>*  met het versienummer van de Generator van BHOLD-Model-versie die u installeert.

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- Voor informatie over het maken van invoerbestanden [technische documentatie voor Microsoft BHOLD-Suite](https://technet.microsoft.com/en-us/library/jj134935(v=ws.10).aspx)
- [BHOLD-installatiehandleiding](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)