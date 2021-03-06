---
title: Planning van Microsoft Identity Manager 2016 in TLS 1,2-omgeving | Microsoft Docs
description: Planning van Microsoft Identity Manager 2016 in TLS 1,2-omgeving
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/26/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4e208e2f42c206ae50febefb6a8206dc3823e084
ms.sourcegitcommit: c9f5f960fd39745bf5b57161a2fd0238c88d035a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2020
ms.locfileid: "85133529"
---
# <a name="planning-mim-2016-sp2-in-tls-12-or-fips-mode-environments"></a>MIM 2016 SP2 plannen in TLS 1,2-en FIPS-modus omgevingen


> [!IMPORTANT]
> Dit artikel is alleen van toepassing op MIM 2016 SP2

Wanneer u MIM 2016 SP2 installeert in de vergrendelde omgeving met alle versleutelings protocollen, maar TLS 1,2 is uitgeschakeld, gelden de volgende vereisten:
- Als u de MIM-onderdelen beveiligde TLS 1,2-verbinding wilt maken, moeten de meest recente updates voor Windows Server en .NET Framework waarmee TLS 1,2-ondersteuning in .NET 3,5 Framework wordt geïnstalleerd. Afhankelijk van de configuratie van uw server moet u *mogelijk* [SystemDefaultTlsVersions inschakelen voor .NET Framework](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the-net-framework) en/of [alle SCHANNEL-protocollen behalve TLS 1,2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) in het REGI ster in de *client* -en *Server* subsleutels uitschakelen.

## <a name="mim-synchronization-service-sql-ma"></a>MIM-synchronisatie service, SQL MA

- Voor het tot stand brengen van een beveiligde TLS 1,2-verbinding met SQL Server, is voor de MIM-synchronisatie service en de ingebouwde SQL-beheer agent [SQL Native Client 11.0.7001.0](https://www.microsoft.com/download/details.aspx?id=50402) of hoger vereist.

## <a name="mim-service"></a>MIM-service
   >[!NOTE]
   >MIM 2016 SP2 installatie zonder toezicht mislukt in omgeving met alleen TLS 1,2. Installeer de MIM-service in de interactieve modus of als u de installatie zonder toezicht wilt uitvoeren, Controleer of TLS 1,1 is ingeschakeld. Nadat de installatie zonder toezicht is voltooid, dwingt u TLS 1,2 af, indien nodig.

- Zelfondertekende certificaten kunnen niet worden gebruikt door de MIM-service in een TLS 1,2-omgeving. Kies een krachtig, compatibel certificaat dat is uitgegeven door een vertrouwde certificerings instantie bij de installatie van de MIM-service.
- Voor het installatie programma van de MIM-service is [voor SQL Server versie 18,2](https://www.microsoft.com/download/details.aspx?id=56730) of hoger extra vereist OLE DB Stuur Programma's.

## <a name="fips-mode-considerations"></a>Overwegingen voor FIPS-modus

Als u de MIM-service installeert op een server waarvoor FIPS-modus is ingeschakeld, moet u de FIPS-beleids validatie uitschakelen zodat de MIM-service werk stromen kunnen worden uitgevoerd. Als u dit wilt doen, voegt u *enforceFIPSPolicy Enabled = False* element toe aan de sectie *runtime* van *Microsoft.ResourceManagement.Service.exe.config* -bestand tussen *runtime* -en *assemblyBinding* -secties, zoals hieronder wordt weer gegeven:

```XML
<runtime>
<enforceFIPSPolicy enabled="false"/>
<assemblyBinding ...>
```    
