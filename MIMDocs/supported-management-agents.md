---
title: Ondersteunde connectors | Microsoft Docs
description: Met connectors voor het beheren van gegevensoverdracht tussen MIM en uw verbonden gegevensbronnen.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 1/24/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 023232b9ddb3cb0a299cbc14ab4c311b8c63fc47
ms.sourcegitcommit: fa30a8eb9c3a7f1ed6f8ce0f67362ca32751e00d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56667196"
---
# <a name="connect-to-your-directories"></a>Verbinding maken met uw adreslijsten

Connectors koppelen specifieke verbonden gegevensbronnen aan Microsoft Identity Manager SP1 (MIM). Met een connector worden gegevens van een verbonden gegevensbron naar MIM verplaatst. Wanneer gegevens in MIM worden gewijzigd, kunnen met de connector de gegevens ook worden geëxporteerd naar de gekoppelde gegevensbron om deze te synchroniseren met MIM. Over het algemeen is er ten minste één connector beschikbaar voor elke gekoppelde adreslijst.

In Forefront Identity Manager heetten connectors beheeragents. Deze term wordt nog steeds in sommige artikelen of delen van het product gebruikt, maar beide termen verwijzen naar hetzelfde concept.

In dit artikel gaat over de connectors die zijn opgenomen en in MIM worden ondersteund, maar de connector voor Extensible Connectivity 2.0 maakt het mogelijk om te verbinden met nog meer gegevensbronnen. Sommige partners hebben hun eigen connectors op deze manier gemaakt en een volledige lijst is beschikbaar in de wiki [FIM 2010: Beheeragents van Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Ondersteunde connectors in MIM 2016 SP1

| Name | Ondersteunde versies van de verbonden gegevensbron & technische koppelingen |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory in WindowsServer 2012 en WindowsServer 2016 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Active Directory Global Address List (GAL) | Active Directory globale adreslijst (GAL) – Exchange 2013, Exchange 2016 |
| Extensible Connectivity 2.0 | Een willekeurige gegevensbron op basis van oproep of op basis van bestanden |
| FIM-service | MIM-Service. Houd er rekening mee dat de MIM-synchronisatieservice en de MIM-Service dezelfde versie moet. |
| IBM DB2 Universal Database | IBM DB2 versie 9.5 of 9.7; IBM DB2 OLEDB v9.5 FP5 of v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory versie 8.7.3, 8.8.5 en 8.8.6 |
| Oracle Database | Oracle Database 10g of 11g; 64-bit client |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Oracle (eerder Sun en Netscape) Directory Servers | Sun Directory Server 6.x, 7.x en Oracle 11 |
| [Windows PowerShell Connector](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 of hoger |
| [Microsoft Azure Active Directory Connector](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory (niet aanbevolen voor nieuwe implementaties) |
| [Algemene LDAP-Connector](https://msdn.microsoft.com/library/dn510997.aspx) | [LDAP v3-server (met RFC 4510)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericldap) |
| [Algemene SQL-connector](./reference/microsoft-identity-manager-2016-connector-genericsql.md) | [De Connector wordt ondersteund met alle 64-bits ODBC-stuurprogramma 's](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericsql.md) |
| [Connector voor Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes Release v8.5.x |
| [SharePoint Services Connector UPA](https://msdn.microsoft.com/library/dn511003.aspx) | SharePoint server 2013 of 2016 met servicetoepassing voor gebruikersprofielen (UPA) |
| [Connector voor Web Services](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 of 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 en andere SOAP en REST-API 's](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Kenmerk-waardepaar tekstbestand](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Kenmerk-waardepaar tekstbestanden |
| [Tekstbestand met scheidingstekens](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Tekstbestanden met scheidingstekens |
| [Directory Services een taal (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Tekstbestand met vast breedte](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Tekstbestanden met vast breedte |
| [LDAP Data Interchange Format (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange Format (LDIF) |
| [Microsoft Graph Connector](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Verwante onderwerpen

[Beheeragents in FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
