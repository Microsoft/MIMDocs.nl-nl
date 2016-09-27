---
title: Wat is hybride rapportage? | Microsoft Identity Manager
description: Met hybride rapportage van Azure Active Directory maakt u aangepaste rapporten waarin zowel cloud- als on-premises gebeurtenissen worden opgenomen.
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: a074f3361e6d5be497b1a3c25d56aaa7008b128e


---

# Rapporten voor hybride-identiteitsbeheer in Azure
Met Azure Active Directory (AD) kunt u één rapport maken waarin u identiteitsbeheeractiviteiten kunt bewaken die on-premises of in de cloud plaatsvinden. Met deze functie kunt u al uw identiteits- en toegangsgegevens op één locatie beheren en zodoende tijd besparen en de totale kosten reduceren.

## Wat is hybride rapportage van Azure AD?
Hybride rapportage helpt IT-professionals met het oplossen van algemene rapportage-vraagstukken op het gebied van identiteitsbeheer.

1. **Activiteiten in identiteitsbeheer verzamelen over verschillende systemen.** In hybride rapporten worden activiteit op het gebied van identiteitsbeheer uit Azure AD en Identity Manager weergegeven.

2. **Rapportgegevens exporteren en aangepaste rapporten maken.** U kunt de gegevens niet alleen weergeven in Azure Portal, maar ook gegevens exporteren om zelf aangepaste weergaven te maken.

3. **Infrastructuurkosten voor rapportagesystemen verminderen.** Met hybride rapportage in de cloud kunt u on-premises datawarehouse-infrastructuur voor rapportage elimineren.

## Hoe werkt dit?

Als u de on-premises gegevens wilt verzamelen, moet u eerst een rapportageagent installeren op uw Identity Manager-server. De rapportageagent moet worden gedownload vanaf de configuratiepagina van de map in de [klassieke Azure-portal](https://manage.windowsazure.com/).

Het proces voor hybride rapportage verloopt als volgt:
1. Nadat de rapportageagent is geïnstalleerd, worden de activiteitsgegevens van Identity Manager verzonden naar het Windows-gebeurtenislogboek.
2. De gebeurtenissen in het Windows-gebeurtenislogboek worden verwerkt door de rapportageagent en geüpload naar Azure.
3. De activiteitsgegevens worden gedurende één maand opgeslagen in Azure.
4. Wanneer u een rapport aanvraagt, worden de activiteitsgebeurtenissen geparseerd en vervolgens gefilterd voor de vereiste rapporten.
5. De klassieke Azure Portal haalt de rapportagegegevens op en geeft deze weer als activiteitenrapport.

## Zie ook
- Meer informatie over [werken met hybride rapportage van Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)



<!--HONumber=Jul16_HO3-->

