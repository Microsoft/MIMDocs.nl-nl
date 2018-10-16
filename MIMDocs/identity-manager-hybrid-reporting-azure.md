---
title: Wat is hybride rapportage in Azure AD? | Microsoft Docs
description: Hybride controleactiviteitenrapporten in Azure Active Directory kunt u de gecontroleerde gebeurtenissen weergeven in de cloud en de on-premises.
keywords: ''
author: davidste
ms.author: davidste
manager: bhu
ms.date: 02/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: 459e1c0c04f28b1ecc1c74f5f672c6318684cc90
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332830"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Beheer van hybride identiteit controleren rapportage in Azure Active Directory
Met Azure Active Directory (Azure AD) activiteit controlerapportage, u kunt identity management-activiteit controleren on-premises of in de cloud. Door het beheer van al uw identiteits- en gegevens in een rapport, kunt u tijd besparen en de totale kosten te verlagen.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Wat is hybride rapportage van Azure Active Directory?
Hybride controle en rapportage helpt IT-professionals uitdagingen algemene identiteitsbeheer rapportage, zoals:

* **Identiteitsbeheer verzamelen over verschillende systemen**. In hybride rapporten worden activiteit op het gebied van identiteitsbeheer uit Azure AD en Identity Manager weergegeven.

* **Rapportage van gegevens en het maken van aangepaste rapporten exporteert**. Naast uw rapporten weergeven in de Azure-portal, kunt u de gegevens voor het genereren van uw eigen aangepaste weergaven exporteren.

* **Infrastructuur voor rapportagesystemen verminderen**. Hybride rapportage in de cloud betekent dat u de kosten die gekoppeld aan uw on-premises, datawarehouse-infrastructuur zijn kunt voorkomen.

## <a name="how-does-it-work"></a>Hoe werkt dit?

Als u de on-premises gegevens wilt verzamelen, moet u eerst een rapportageagent installeren op uw Identity Manager 2016-server. [Downloaden van de hybride van Microsoft Identity Manager-Rapportageagent](https://www.microsoft.com/download/details.aspx?id=55112).

Hybride rapportage ondergaat het volgende proces:
1. Nadat u de agent voor rapportage installeert, wordt de activiteitsgegevens van Identity Manager verzonden naar de Windows-gebeurtenislogboek.
2. De rapportageagent verwerkt de delta-gebeurtenissen om de 10 minuten of wanneer de Windows Event Log-service opnieuw wordt opgestart. De agent uploadt vervolgens de gebeurtenissen naar de Azure-portal.
3. De ontvangen gegevens verwerkt in de Azure portal binnen een uur na ontvangst van het.
4. De activiteitsgegevens worden gedurende één maand opgeslagen in Azure.
5. De Azure portal haalt de rapportagegegevens voor controle en geeft deze weer in het venster Azure controleren Reporting.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:
- [Werken met hybride rapportage van Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- [Controleactiviteitenrapporten in de Azure Active Directory-portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Bewaarbeleid voor rapportage](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure-Logboekintegratie (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
