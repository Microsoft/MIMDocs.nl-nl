---
title: Wat is hybride rapportage? | Microsoft Docs
description: In de rapporten voor hybride controle-activiteiten in Azure Active Directory kunt u controlegebeurtenissen in de cloud of on-premises bekijken.
keywords: 
author: fimguy
ms.author: fimguy
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 678626e7c32659570de88d8178c16821cceaf7ee
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory---public-previewrefresh"></a>Controlerapporten voor beheer van hybride identiteiten in Azure Active Directory - Openbare preview-versie (vernieuwen)
Met rapporten voor controle-activiteiten in Azure Active Directory (AD) kunt u één rapport weergeven waarin u identiteitsbeheeractiviteiten kunt bewaken die on-premises of in de cloud plaatsvinden. Met deze functie kunt u al uw identiteits- en toegangsgegevens op één locatie beheren en zodoende tijd besparen en de totale kosten reduceren.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Wat is hybride rapportage van Azure Active Directory?
Hybride controlerapportage helpt IT-professionals met het oplossen van algemene rapportage-vraagstukken op het gebied van identiteitsbeheer.

1. **Activiteiten in identiteitsbeheer verzamelen over verschillende systemen.** In hybride rapporten worden activiteit op het gebied van identiteitsbeheer uit Azure AD en Identity Manager weergegeven.

2. **Rapportgegevens exporteren en aangepaste rapporten maken.** U kunt de gegevens niet alleen weergeven in Azure Portal, maar ook gegevens exporteren om zelf aangepaste weergaven te maken.

3. **Infrastructuurkosten voor rapportagesystemen verminderen.** Dankzij hybride rapportage in de cloud hebt u niet langer een datawarehouse-infrastructuur voor on-premises rapportage nodig.

## <a name="how-does-it-work"></a>Hoe werkt dit?

Als u de on-premises gegevens wilt verzamelen, moet u eerst een rapportageagent installeren op uw Identity Manager 2016-server. De rapportageagent kan [hier](https://www.microsoft.com/en-us/download/details.aspx?id=55112) worden gedownload van de downloadpagina van Microsoft.

Het proces voor hybride rapportage verloopt als volgt:
1. Nadat de rapportageagent is geïnstalleerd, worden de activiteitsgegevens van Identity Manager verzonden naar het Windows-gebeurtenislogboek.
2. De gebeurtenissen voor verschillen in het Windows-gebeurtenislogboek worden om de tien minuten of bij het opnieuw starten van een service verwerkt door de rapportageagent en geüpload naar Azure Portal.
3. De gegevens worden binnen een uur nadat deze zijn ontvangen, verwerkt in Azure Portal
4. De activiteitsgegevens worden gedurende één maand opgeslagen in Azure.
5. De rapportagegegevens voor controle worden opgehaald in Azure Portal en worden als controle weergeven op de blade voor controlerapportage van Azure.

## <a name="see-also"></a>Zie tevens
- Meer informatie over [werken met hybride rapportage van Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- Meer informatie over [Rapporten voor controle-activiteiten in de Azure Active Directory-portal](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)