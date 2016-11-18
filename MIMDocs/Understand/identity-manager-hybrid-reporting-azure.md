---
title: Wat is hybride rapportage? | Microsoft Docs
description: Met hybride rapportage van Azure Active Directory maakt u aangepaste rapporten waarin zowel cloud- als on-premises gebeurtenissen worden opgenomen.
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: f21c15fdaa5fba9176cfc60a3c49017fa97fa935


---

# <a name="hybrid-identity-management-reports-in-azure"></a>Rapporten voor hybride-identiteitsbeheer in Azure
Met Azure Active Directory (AD) kunt u één rapport maken waarin u identiteitsbeheeractiviteiten kunt bewaken die on-premises of in de cloud plaatsvinden. Met deze functie kunt u al uw identiteits- en toegangsgegevens op één locatie beheren en zodoende tijd besparen en de totale kosten reduceren.

## <a name="what-is-azure-ad-hybrid-reporting"></a>Wat is hybride rapportage van Azure AD?
Hybride rapportage helpt IT-professionals met het oplossen van algemene rapportage-vraagstukken op het gebied van identiteitsbeheer.

1. **Activiteiten in identiteitsbeheer verzamelen over verschillende systemen.** In hybride rapporten worden activiteit op het gebied van identiteitsbeheer uit Azure AD en Identity Manager weergegeven.

2. **Rapportgegevens exporteren en aangepaste rapporten maken.** U kunt de gegevens niet alleen weergeven in Azure Portal, maar ook gegevens exporteren om zelf aangepaste weergaven te maken.

3. **Infrastructuurkosten voor rapportagesystemen verminderen.** Met hybride rapportage in de cloud kunt u on-premises datawarehouse-infrastructuur voor rapportage elimineren.

## <a name="how-does-it-work"></a>Hoe werkt dit?

Als u de on-premises gegevens wilt verzamelen, moet u eerst een rapportageagent installeren op uw Identity Manager-server. De rapportageagent moet worden gedownload vanaf de configuratiepagina van de map in de [klassieke Azure-portal](https://manage.windowsazure.com/).

Het proces voor hybride rapportage verloopt als volgt:
1. Nadat de rapportageagent is geïnstalleerd, worden de activiteitsgegevens van Identity Manager verzonden naar het Windows-gebeurtenislogboek.
2. De gebeurtenissen in het Windows-gebeurtenislogboek worden verwerkt door de rapportageagent en geüpload naar Azure.
3. De activiteitsgegevens worden gedurende één maand opgeslagen in Azure.
4. Wanneer u een rapport aanvraagt, worden de activiteitsgebeurtenissen geparseerd en vervolgens gefilterd voor de vereiste rapporten.
5. De klassieke Azure Portal haalt de rapportagegegevens op en geeft deze weer als activiteitenrapport.

## <a name="see-also"></a>Zie tevens
- Meer informatie over [werken met hybride rapportage van Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)



<!--HONumber=Nov16_HO2-->


