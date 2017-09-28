---
title: Ondersteunde connectors | Microsoft Docs
description: Met connectors kunt gegevensoverdracht tussen MIM en uw verbonden gegevensbronnen beheren.
keywords: 
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 09/26/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 99e98f3f9cb5e68fde0e3018856bf613c082325d
ms.sourcegitcommit: ba4cd133f7b49752c5470c9fc46e7e302cc99b49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2017
---
# <a name="connect-to-your-directories"></a>Verbinding maken met uw adreslijsten

Connectors koppelen specifieke verbonden gegevensbronnen aan Microsoft Identity Manager SP1 (MIM). Met een connector worden gegevens van een verbonden gegevensbron naar MIM verplaatst. Wanneer gegevens in MIM worden gewijzigd, kunnen met de connector de gegevens ook worden geëxporteerd naar de gekoppelde gegevensbron om deze te synchroniseren met MIM. Over het algemeen is er ten minste één connector beschikbaar voor elke gekoppelde adreslijst.

In Forefront Identity Manager heetten connectors beheeragents. Deze term wordt nog steeds in sommige artikelen of delen van het product gebruikt, maar beide termen verwijzen naar hetzelfde concept.

In dit artikel gaat over de connectors die zijn opgenomen & in MIM worden ondersteund, maar de connector voor Extensible Connectivity 2.0 maakt het mogelijk om te verbinden met nog meer gegevensbronnen. Sommige partners hebben hun eigen connectors op deze manier gemaakt. Een volledige lijst is beschikbaar in de wiki [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (FIM 2010: beheeragents van partners).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Ondersteunde connectors in MIM 2016 SP1

| Naam | Ondersteunde versies van de gekoppelde gegevensbron |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory-2012 2016 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Active Directory Global Address List (GAL) | Active Directory globale adreslijst (GAL) – Exchange 2013 2016 |
| Extensible Connectivity 2.0 | Een willekeurige gegevensbron op basis van oproep of op basis van bestanden |
| FIM-service | FIM-Service Management Agent (Sycronization Service) moet op dezelfde versie van de 'Forefront Identity Manager Service' geïnstalleerd |
| IBM DB2 Universal Database | IBM DB2 versie 9.5 of 9.7; IBM DB2 OLEDB v9.5 FP5 of v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory versie 8.7.3, 8.8.5 en 8.8.6 |
| Oracle Database | Oracle Database 10g of 11g; 64-bit client |
| Microsoft SQL Server | SQL Server 2012, 2014 2016 |
| Oracle (eerder Sun en Netscape) Directory Servers | Sun Directory Server 6.x, 7.x en Oracle 11 |
| [Windows PowerShell Connector voor FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 of hoger |
| [Microsoft Azure Active Directory Connector voor FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Algemene LDAP-Connector voor FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | LDAP v3-server (compatibel met RFC 4510) |
| [Connector voor Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes Release v8.5.x |
| [SharePoint Services-Connector UPA](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | SharePoint server 2013 of 2016 met servicetoepassing voor gebruikersprofielen (UPA) |
| [Connector voor Web Services](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 of 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 |
| [Kenmerk-waardepaar tekstbestand](https://technet.microsoft.com/en-us/library/cc708644(v=ws.10).aspx) | Kenmerk-waardepaar tekstbestanden |
| [Tekstbestand met scheidingstekens](https://technet.microsoft.com/en-us/library/cc720612(v=ws.10).aspx) | Tekstbestanden met scheidingstekens |
| [Adreslijstservices winstmarge taal (DSML)](https://technet.microsoft.com/en-us/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Tekstbestand met vast breedte](https://technet.microsoft.com/en-us/library/cc720633(v=ws.10).aspx) | Tekstbestanden met vast breedte |
| [LDAP Data Interchange Format (LDIF)](https://technet.microsoft.com/en-us/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange Format (LDIF) |

## <a name="related-topics"></a>Verwante onderwerpen

[Beheeragents in FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
