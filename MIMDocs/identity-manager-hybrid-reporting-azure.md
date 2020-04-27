---
title: Wat is hybride rapportage in azure AD? | Microsoft Docs
description: Met de hybride controle activiteiten rapporten in Azure Active Directory kunt u gecontroleerde gebeurtenissen in zowel de Cloud als on-premises bekijken.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: 6f4f2aea998fc5682d1fb21d77e4d4f1c582d770
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042148"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Controle rapportage voor Hybrid Identity Management in Azure Active Directory
Met Azure Active Directory (Azure AD) rapportage van controle activiteiten kunt u de activiteiten voor identiteits beheer on-premises of in de Cloud bewaken. Door al uw identiteiten en toegang tot gegevens in één rapport te beheren, kunt u tijd besparen en de totale kosten verlagen.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Wat is hybride rapportage van Azure Active Directory?
Hybrid audit Reporting helpt IT-professionals bij het oplossen van veelvoorkomende problemen met identiteits beheer, zoals:

* **Activiteiten voor identiteits beheer op verschillende systemen verzamelen**. In hybride rapporten worden activiteit op het gebied van identiteitsbeheer uit Azure AD en Identity Manager weergegeven.

* **Rapport gegevens exporteren en aangepaste rapporten maken**. Naast het weer geven van uw rapporten in de Azure Portal, kunt u de gegevens exporteren om uw eigen aangepaste weer gaven te genereren.

* De **rapportage systeem infrastructuur kosten te verlagen**. Met hybride rapportage in de cloud kunt u de kosten verwijderen die zijn gekoppeld aan uw on-premises, Data Warehouse-infra structuur.

## <a name="how-does-it-work"></a>Hoe werkt het?

Als u de on-premises gegevens wilt verzamelen, moet u eerst een rapportageagent installeren op uw Identity Manager 2016-server. [Down load de Microsoft Identity Manager Hybrid Reporting agent](https://www.microsoft.com/download/details.aspx?id=55112).

Hybride rapportage ondergaat het volgende proces:
1. Nadat u de Reporting agent hebt geïnstalleerd, worden de activiteit gegevens van Identity Manager verzonden naar het Windows-gebeurtenis logboek.
2. De rapportage agent verwerkt de Delta gebeurtenissen elke 10 minuten of wanneer de Windows Event Log-service opnieuw wordt gestart. De agent uploadt de gebeurtenissen vervolgens naar de Azure Portal.
3. Het Azure Portal verwerkt de ontvangen gegevens binnen één uur nadat deze zijn ontvangen.
4. De activiteitsgegevens worden gedurende één maand opgeslagen in Azure.
5. De Azure Portal haalt de controle rapport gegevens op en geeft deze weer in het Azure audit Reporting-venster.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:
- [Werken met hybride rapportage van Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- [Controleactiviteitenrapporten in Azure Active Directory Portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Bewaar beleid voor rapporten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure-logboek integratie (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Rapportage-API Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
