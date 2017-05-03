---
title: Wat is hybride rapportage? | Microsoft Docs
description: In de rapporten voor hybride controle-activiteiten in Azure Active Directory kunt u controlegebeurtenissen in de cloud of on-premises bekijken.
keywords: 
author: kgremban
ms.author: fimguy
manager: femila
ms.date: 04/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3144ee195675df5dc120896cc801a7124ee12214
ms.openlocfilehash: 8ca0af93f2d72ccf2e314b20d13323b631eb02bc
ms.lasthandoff: 04/27/2017


---

# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory"></a>Controlerapporten voor hybride-identiteitsbeheer in Azure Active Directory
Met rapporten voor controle-activiteiten in Azure Active Directory (AD) kunt u één rapport weergeven waarin u identiteitsbeheeractiviteiten kunt bewaken die on-premises of in de cloud plaatsvinden. Met deze functie kunt u al uw identiteits- en toegangsgegevens op één locatie beheren en zodoende tijd besparen en de totale kosten reduceren.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Wat is hybride rapportage van Azure Active Directory?
Hybride controlerapportage helpt IT-professionals met het oplossen van algemene rapportage-vraagstukken op het gebied van identiteitsbeheer.

1. **Activiteiten in identiteitsbeheer verzamelen over verschillende systemen.** In hybride rapporten worden activiteit op het gebied van identiteitsbeheer uit Azure AD en Identity Manager weergegeven.

2. **Rapportgegevens exporteren en aangepaste rapporten maken.** U kunt de gegevens niet alleen weergeven in Azure Portal, maar ook gegevens exporteren om zelf aangepaste weergaven te maken.

3. **Infrastructuurkosten voor rapportagesystemen verminderen.** Met hybride rapportage in de cloud kunt u on-premises datawarehouse-infrastructuur voor rapportage elimineren.

## <a name="how-does-it-work"></a>Hoe werkt dit?

Als u de on-premises gegevens wilt verzamelen, moet u eerst een rapportageagent installeren op uw Identity Manager 2016-server. De rapportageagent kan [hier](https://www.microsoft.com/en-us/download/details.aspx?id=####/) worden gedownload van de downloadpagina van Microsoft.

Het proces voor hybride rapportage verloopt als volgt:
1. Nadat de rapportageagent is geïnstalleerd, worden de activiteitsgegevens van Identity Manager verzonden naar het Windows-gebeurtenislogboek.
2. De gebeurtenissen in het Windows-gebeurtenislogboek worden verwerkt door de rapportageagent en geüpload naar Azure Portal.
3. De activiteitsgegevens worden gedurende één maand opgeslagen in Azure.
4. Wanneer u een rapport aanvraagt, worden de activiteitsgebeurtenissen geparseerd en vervolgens gefilterd voor de vereiste rapporten.
5. Azure Portal haalt de controlerapportagegegevens op en geeft deze weer als controle in het activiteitenrapport.

## <a name="see-also"></a>Zie tevens
- Meer informatie over [werken met hybride rapportage van Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)
- Meer informatie over [Rapporten voor controle-activiteiten in de Azure Active Directory-portal](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)

