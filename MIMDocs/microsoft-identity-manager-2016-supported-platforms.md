---
title: Ondersteunde softwareplatformen | Microsoft Docs
description: De producten en versies zoeken die compatibel met elk van de MIM 2016-onderdelen zijn
keywords: ''
author: fimguy
ms.author: davidste
manager: davidste
ms.date: 04/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: 1054d611ae0b230005a0f79be69f5c6c2bba7af2
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479295"
---
# <a name="supported-platforms-for-mim-2016"></a>Ondersteunde platformen voor MIM 2016

In deze tabel worden de ondersteunde platformen en versies voor elk onderdeel van Microsoft Identity Manager 2016 beschreven. De versies die zijn gemarkeerd met een * worden alleen ondersteund in MIM 2016-servicepack 1 met de meest recente hotfix.


| **MIM-onderdeel** | **Platform** | **Versie** |
|-------------------|--------------|--------------|
| **MIM-synchronisatie** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | Active Directory-domeinfunctieniveau voor gebruikers inrichten, PCNS en GAL Sync | Windows 2000 <br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | MIM-synchronisatiedatabase | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Active Directory voor gebruikers inrichten, PCNS en GAL Sync (optioneel)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange voor postvakinrichting en GAL Sync (optioneel)|Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | Ontwikkelomgeving (optioneel) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | Aanvullend verbonden systeem (optioneel) | Active Directory Domain Services<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2008 of hoger<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Andere producten van derden |
| **MIM-service en -portal** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |PAM-Scenario: WindowsServer | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |PAM-Scenario: Active Directory voor PAM-forest in bastionomgeving | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |PAM-Scenario: Active Directory voor PAM scenario bestaande (CORP) forests | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | MIM-servicedatabases | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | Mailserver voor goedkeuring van de MIM-service en e-mailberichten voor groepsbeheer (optioneel) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (alleen melding voordat build [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490) |
| | Browser | Alle primaire ondersteunde browsers * (mobiele apparaten beperkt)|
| **MIM-servicerapportages** | Windows Server |  Windows Server 2008 R2 SP1<br/>Windows Server 2012 <br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Datawarehouse | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager * (met 4.4.1459)<br/> [Compatibiliteit van de versie van SQL Server voor System Center 2016](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016)
 |
| **MIM-portals voor wachtwoord opnieuw instellen en wachtwoordregistratie** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Webbrowser | Alle primaire ondersteunde browsers |
| **MIM-invoegtoepassingen en -extensies** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook-integratie (optioneel) | Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (op Windows 10) * |
| | PAM PowerShell-aanvrager-cmdlets (optioneel) | Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (server- en CA-integratie) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Certificeringsinstantie | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM CM-database | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **MIM Certificate Management** (programma) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (bulksgewijze Client) | Windows | Windows 7 |
| **MIM Certificate Management** (Client-ActiveX gebaseerd smartcard) | Windows | Windows 7 </br> Windows 8 </br> Windows 8.1 </br> Windows 10 |
| **MIM BHOLD-suite** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | BHOLD-database | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | E-mailserver (optioneel) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Webbrowser | Internet Explorer ondersteunde browsers met Silverlight |
