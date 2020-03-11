---
title: Ondersteunde connectors | Microsoft Docs
description: Gebruik connectors voor het beheren van de gegevens overdracht tussen MIM en de verbonden gegevens bronnen.
keywords: ''
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
author: EugeneSergeev
manager: daveba
editor: ''
reviewer: markwahl-msft
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/23/2019
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f6e43abea8b58ccff7fa376b266a91cb138f5aa9
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044379"
---
# <a name="connect-to-your-directories"></a>Verbinding maken met uw adreslijsten

Connectors koppelen specifieke verbonden gegevens bronnen aan Microsoft Identity Manager SP1 (MIM). Met een connector worden gegevens van een verbonden gegevensbron naar MIM verplaatst. Wanneer gegevens in MIM worden gewijzigd, kunnen met de connector de gegevens ook worden geëxporteerd naar de gekoppelde gegevensbron om deze te synchroniseren met MIM. Over het algemeen is er ten minste één connector beschikbaar voor elke gekoppelde adreslijst.

In Forefront Identity Manager heetten connectors beheeragents. Deze term wordt nog steeds in sommige artikelen of delen van het product gebruikt, maar beide termen verwijzen naar hetzelfde concept.

Dit artikel heeft betrekking op de connectors die zijn opgenomen & worden ondersteund in MIM, maar de connector voor de uitbreid bare connectiviteit 2,0 maakt het mogelijk om verbinding te maken met nog meer gegevens bronnen. Sommige partners hebben hun eigen connectors op deze manier gemaakt. Een volledige lijst is beschikbaar in de wiki [FIM 2010: Management Agents from Partners](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (FIM 2010: beheeragents van partners).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Ondersteunde connectors in MIM 2016 SP1

| Naam | Ondersteunde versies van de verbonden gegevens bron & technische koppelingen |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory in Windows Server 2012-2019 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Active Directory Global Address List (GAL) | Active Directory globale adres lijst (GAL): Exchange 2013-2019 |
| Extensible Connectivity 2.0 | Een willekeurige gegevensbron op basis van oproep of op basis van bestanden |
| FIM-service | MIM-service. Houd er rekening mee dat de MIM-synchronisatie service en de MIM-service dezelfde versie moeten zijn. |
| IBM DB2 Universal Database | IBM DB2-versie 9,5 of 9,7; IBM DB2 OLEDB v 9.5 FP5 of v 9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory versie 8.7.3, 8.8.5 en 8.8.6 |
| Oracle Database | Oracle Database 10g of 11g; 64-bit client |
| Microsoft SQL Server | SQL Server 2012-2017 |
| Oracle (eerder Sun en Netscape) Directory Servers | Sun Directory Server 6.x, 7.x en Oracle 11 |
| [Windows Power shell-connector](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 of hoger |
| [Microsoft Azure Active Directory-connector](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory (niet aanbevolen voor nieuwe implementaties) |
| [Algemene LDAP-connector](https://msdn.microsoft.com/library/dn510997.aspx) | [LDAP v3-server (RFC 4510-compatibel)](reference/microsoft-identity-manager-2016-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| [Algemene SQL-connector](reference/microsoft-identity-manager-2016-connector-genericsql.md) | [De connector wordt ondersteund met alle 64-bits ODBC-stuur Programma's](reference/microsoft-identity-manager-2016-connector-genericsql.md#overview-of-the-generic-sql-connector) |
| [Connector voor Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes Release v 8.5. x, v 9.0. x |
| [UPA share Point services-connector](https://msdn.microsoft.com/library/dn511003.aspx) | Share Point server 2013-2019 met Service toepassing voor gebruikers profielen (UPA) |
| [Connector voor Web Services](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5,0 of 6,0; Oracle People Soft 9,1; Oracle eBusiness 12,1 en andere SOAP-en REST-Api's](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Kenmerk-waardepaar tekst bestand](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Kenmerk-waardepaar tekstbestanden |
| [Tekst bestand met scheidings tekens](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Tekstbestanden met scheidingstekens |
| [DSML (Directory Services Mark-up Language)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Tekst bestand met vaste breedte](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Tekstbestanden met vast breedte |
| [LDAP Data Interchange Format (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange Format (LDIF) |
| [Microsoft Graph-connector](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Verwante onderwerpen

[Beheeragents in FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
