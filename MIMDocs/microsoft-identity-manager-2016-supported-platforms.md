---
title: Ondersteunde softwareplatformen | Microsoft Docs
description: De producten en versies zoeken die compatibel met elk van de MIM 2016-onderdelen zijn
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/19/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: 04e96f3c8eb71041dccb173f2a1dc7dbba46ef01
ms.sourcegitcommit: cd503e8e9933d39d6fbf894c7d27bf9566301ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2020
ms.locfileid: "88168334"
---
# <a name="supported-platforms-for-mim-2016"></a>Ondersteunde platformen voor MIM 2016

In deze tabel worden de ondersteunde platformen en versies voor elk onderdeel van Microsoft Identity Manager 2016 beschreven. De versies die zijn gemarkeerd met een * worden alleen ondersteund in MIM 2016 Service Pack 1. De versies die zijn gemarkeerd met * * worden alleen ondersteund in MIM 2016 Service Pack 2 of een latere hotfix. De versies die zijn gemarkeerd met ' NR ', worden niet aanbevolen, worden wel ondersteund, maar worden niet aanbevolen als een nieuwe implementatie van dat platform voor MIM wordt gestart.


| **MIM-onderdeel** | **Platform** | **Versie** |
|-------------------|--------------|--------------|
| **MIM-synchronisatie** | Windows Server | Windows Server 2008 R2 SP1 (NR.)<br/>Windows Server 2012 (NR.)<br/>Windows Server 2012 R2<br/>Windows Server 2016 *<br/>Windows Server 2019 * * |
| | Active Directory functionaliteits niveau voor gebruikers inrichten, PCNS en GAL-synchronisatie | Windows 2000 (NR.)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | MIM-synchronisatiedatabase | SQL Server 2008 R2 SP3 (NR.)<br/>SQL Server 2012 SP4 (NR.)<br/>SQL Server 2014 SP3 (NR.) <br/>SQL Server 2016 SP2 * <br/>SQL Server 2017 * * <br/> SQL Server 2019 * * |
| | Active Directory voor het inrichten van gebruikers, PCNS en GAL-synchronisatie (optioneel)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Exchange voor postvakinrichting en GAL Sync (optioneel)|Exchange Server 2010 SP3 (NR.)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016 *<br/>Exchange Server 2019 * * |
| | Ontwikkelomgeving (optioneel) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | Aanvullend verbonden systeem (optioneel) | Active Directory Domain Services<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2008 of hoger<br/>SharePoint Server 2013<br/> SharePoint Server 2016 *<br/> Share Point server 2019 * * <br/> Andere producten van derden |
| **MIM-service en -portal** | Windows Server | Windows Server 2008 R2 SP1 (NR.)<br/>Windows Server 2012 (NR.)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| |PAM-scenario: Windows Server | Windows Server 2012 R2 (NR.) <br/> Windows Server 2016 * <br/> Windows Server 2019 * *|
| |PAM-scenario: Active Directory voor Bastion Environment PAM-forest | Windows Server 2012 R2 (NR.) <br/> Windows Server 2016 * <br/> Windows Server 2019 * * |
| |PAM-scenario: Active Directory voor de bestaande (CORP) forests van het PAM-scenario | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * <br/> Windows Server 2019 * * |
| | MIM-servicedatabases | SQL Server 2008 R2 SP3 (NR.)<br/>SQL Server 2012 SP4 (NR.)<br/>SQL Server 2014 SP3 (NR.) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 * * <br/> SQL Server 2019 * * |
| | SharePoint | Share point Foundation 2010 (NR.)<br/>Share point Foundation 2013 SP1 (NR.) <br/> SharePoint 2016 *<br/> Share point 2019 * * |
| | Mailserver voor goedkeuring van de MIM-service en e-mailberichten voor groepsbeheer (optioneel) | Exchange Server 2010 SP3 (NR.)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 *<br/> Exchange Server 2019 * * <br/> Exchange Online * (alleen melding voor build [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490)) |
| | Browser | Alle belang rijke ondersteunde browsers * (beperkt mobiele apparaten)|
| **MIM-servicerapportages** | Windows Server |  Windows Server 2008 R2 SP1 (NR.)<br/>Windows Server 2012 (NR.) <br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Datawarehouse | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager * (met 4.4.1459)<br/> System Center 2019 Service Manager * * |
| **MIM-portals voor wachtwoord opnieuw instellen en wachtwoordregistratie** | Windows Server | Windows Server 2008 R2 SP1 (NR.)<br/>Windows Server 2012 (NR.)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Webbrowser | Alle hoofd browsers die worden ondersteund |
| **MIM-invoegtoepassingen en -extensies** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook-integratie (optioneel) | Outlook 2010 (in Windows, met uitzonde ring van klik-en-klaar)<br/>Outlook 2013 (in Windows, met uitzonde ring van klik-en-klaar) <br/> Outlook 2016 (in Windows 10, behalve klik-en-klaar) *<br/>Office 365 Outlook (in Windows 10, inclusief klik-en-klaar) * * |
| | PAM PowerShell-aanvrager-cmdlets (optioneel) | Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (server- en CA-integratie) | Windows-server | Windows Server 2008 R2 SP1 (NR.)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Certificeringsinstantie | Windows Server 2008 R2 SP1 (NR.)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | MIM CM-database | SQL Server 2008 R2 SP3 (NR.)<br/>SQL Server 2012 SP4 (NR.)<br/>SQL Server 2014 SP3 (NR.) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 * * |
| **MIM Certificate Management** (programma) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (bulk client) | Windows | Windows 7 |
| **MIM-certificaat beheer** (op de client gebaseerde Smart Card) | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> Windows 10 |
| **MIM BHOLD-suite** | Windows Server | Windows Server 2008 R2 SP1 (NR.)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | BHOLD-database | SQL Server 2008 R2 SP3 (NR.)<br/>SQL Server 2012 SP4  <br/> SQL Server 2014 SP3 * <br/> SQL Server 2016 SP2 * |
| | E-mailserver (optioneel) | Exchange Server 2010 SP3 (NR.)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Webbrowser | Browsers die door Internet Explorer worden ondersteund met Silverlight |
