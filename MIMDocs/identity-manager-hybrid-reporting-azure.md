---
title: Wat is hybride rapportage in Azure AD | Microsoft Docs
description: Hybride audit activiteitsrapporten in Azure Active Directory kunt u de gecontroleerde gebeurtenissen bekijken in de cloud en de on-premises.
keywords: ''
author: davidste
ms.author: davidste
manager: bhu
ms.date: 02/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: eb9725df484fb5ac2ee44bd9a0423bdb4fbe7e86
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2018
ms.locfileid: "29370375"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Beheer van hybride identiteit controleren rapportage in Azure Active Directory
Met Azure Active Directory (Azure AD) audit activiteitenrapporten, u kunt identity management-activiteit controleren on-premises of in de cloud. Door het beheer van al uw identiteits- en toegangsbeheer gegevens in één rapport, kunt u tijd besparen en de algehele kosten te verlagen.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Wat is hybride rapportage van Azure Active Directory?
Hybride audit rapportage helpt IT-professionals uitdagingen algemene identiteitsbeheer rapportage, zoals:

* **Identiteitsbeheer verzamelen over verschillende systemen**. In hybride rapporten worden activiteit op het gebied van identiteitsbeheer uit Azure AD en Identity Manager weergegeven.

* **Exporteren van rapportgegevens en het maken van aangepaste rapporten**. U kunt niet alleen uw rapporten weergeven in de Azure portal, de gegevens voor het genereren van uw eigen aangepaste weergaven exporteren.

* **De reporting system infrastructuurkosten te verlagen**. Hybride rapportage in de cloud betekent dat u de kosten die gekoppeld aan uw on-premises, datawarehouse-infrastructuur zijn kan oplossen.

## <a name="how-does-it-work"></a>Hoe werkt dit?

Als u de on-premises gegevens wilt verzamelen, moet u eerst een rapportageagent installeren op uw Identity Manager 2016-server. [Download de Agent voor het melden van Microsoft Identity Manager hybride](https://www.microsoft.com/download/details.aspx?id=55112).

Hybride rapportage ondergaat het volgende proces:
1. Nadat u de agent voor rapportage installeert, is de activiteitsgegevens van Identity Manager verzonden naar de Windows-gebeurtenislogboek.
2. De reportageagent verwerkt de gebeurtenissen delta elke 10 minuten of wanneer de Windows Event Log-service opnieuw wordt opgestart. De agent geüpload en de gebeurtenissen naar de Azure portal.
3. De Azure-portal verwerkt de ontvangen gegevens binnen een uur na ontvangst van het.
4. De activiteitsgegevens worden gedurende één maand opgeslagen in Azure.
5. De Azure portal haalt de rapportagegegevens audit en weergegeven in het venster Azure Audit-rapportage.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:
- [Werken met hybride rapportage van Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- [Controlerapporten van activiteit in de Azure Active Directory-portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Bewaarbeleid voor rapportage](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Integratie met Microsoft Azure-logboek (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Azure Active Directory-rapportage-API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
