---
title: Ondersteuning voor het bijwerken van Azure AD Premium-klanten met behulp van Microsoft Identity Manager | Microsoft Docs
description: In dit artikel wordt beschreven hoe Azure AD Premium klanten ondersteuning kunnen krijgen na 21 januari 2021.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 6/9/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
reviewer: markwahl-msft
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 26dcaf121c4fd980d296ffee893af3ca66249c6c
ms.sourcegitcommit: ea16fea5d69aff58b862468d4bebfb05100d037a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/12/2020
ms.locfileid: "84749243"
---
# <a name="support-update-for-azure-ad-premium-customers-using-microsoft-identity-manager"></a>Ondersteuning voor het bijwerken van Azure AD Premium-klanten met Microsoft Identity Manager

Van toepassing op: Azure AD Premium, Microsoft Identity Manager (MIM)

Voor Azure AD Premium klanten is de Standard-ondersteuning vanaf juni 2020 beschikbaar, na januari 2021 voor specifieke onderdelen van [Microsoft Identity Manager 2016 Service Pack 2](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016)of latere service packs die Azure AD-integratie mogelijk maken. Dit is een aanvulling op de bestaande ondersteuning voor Microsoft Identity Manager die al worden geboden door het [beleid voor vaste levens cyclus](https://docs.microsoft.com//lifecycle/policies/fixed) en abonnementen voor de [ondersteuning van bedrijven](https://support.microsoft.com/help/4341255).

De MIM-onderdelen waarvoor Standard-ondersteuning beschikbaar is, zijn onder andere:
- MIM-synchronisatie service en meldings service voor wachtwoord wijzigingen (PCNS)
- MIM-service en-Portal, invoeg toepassingen en uitbrei dingen, Data Warehouse-ondersteunings scripts en taal pakketten
- MIM-connectors

Deze MIM-onderdelen vullen Active Directory en op uitbrei ding Azure AD via Azure AD Connect met de gebruikers en groepen die zijn ingericht op basis van een on-premises HR-systeem of een ander systeem van record bronnen. Dit zorgt ervoor dat klanten die Azure AD Premium met on-premises systemen gebruiken, nog steeds kunnen worden ondersteund tijdens de migratie van hun identiteits beheer scenario's van on-premises systemen naar Azure AD. 

## <a name="opening-a-support-request-in-the-azure-portal"></a>Een ondersteunings aanvraag openen in de Azure Portal

Als een extra ondersteunings optie voor Microsoft Identity Manager, Azure AD Premium klanten ondersteuning aanvragen voor de hierboven genoemde onderdelen van Microsoft Identity Manager 2016 Service Pack 2 of een latere hotfix of update, via de Azure Portal.

Een klant kan een ondersteunings aanvraag voor Azure maken met behulp van de instructies [voor het maken van een Azure-ondersteunings aanvraag](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request):
1. *type probleem selecteren: technisch*
1. scha kelen om *alle services* weer te geven
1. Klik in de lijst met Services onder Azure Active Directory *Gebruikers inrichten en synchronisatie* selecteren
1. Selecteer *probleem type: Microsoft Identity Manager (MIM)*
1. subtype van *probleem*selecteren: *connectors*, *service en Portal* of *synchronisatie-engine*

![MIM-Ondersteuningsaanvraag maken](media/azure-active-directory-new-support-request.png)

De MIM-onderdelen worden weer gegeven als probleem typen in *Azure Active Directory gebruikers inrichten en synchronisatie* in de Azure Portal.

Voor aanvragen die via de Azure Portal worden geopend, is standaard ondersteuning beschikbaar voor Azure AD Premium klanten voor de volgende onderdelen van Microsoft Identity Manager 2016 Service Pack 2 of een [latere hotfix of update](reference/version-history.md): synchronisatie service, PCNS (Password changes service), connectors, service en Portal, invoeg toepassingen en extensies, Data Warehouse-ondersteunings scripts en taal pakketten.

## <a name="other-support-options"></a>Andere ondersteuningsopties

MIM 2016 SP2, build 4.6.34.0, is uitgebracht in oktober 2019. Klanten worden nadrukkelijk geadviseerd om een volledig ondersteund Service Pack te blijven om ervoor te zorgen dat ze de nieuwste en veiligste versie van hun product hebben. Zie het beleid voor de [levens cyclus van service packs](https://support.microsoft.com/help/17138)voor meer informatie.

Voor klanten die nog steeds gebruikmaken van een oudere versie van MIM, of klanten die geen Azure-ondersteuning of-abonnement hebben op een suite met Azure Active Directory Premium, of voor problemen met andere onderdelen van MIM die niet hierboven worden vermeld, blijft ondersteuning beschikbaar. Het ondersteunings beleid wordt beschreven in het [beleid voor vaste levens cyclus](https://docs.microsoft.com/lifecycle/policies/fixed) met de specifieke datums bij de [ondersteunings levenscyclus voor Microsoft Identity Manager 2016](https://support.microsoft.com/lifecycle/search?alpha=microsoft%20identity%20manager%202016).

Naast ondersteuning voor Azure kunnen er meerdere andere ondersteunings opties worden gebruikt voor het verkrijgen van ondersteuning. Als u bijvoorbeeld micro soft Professional Support hebt, kunt u [een nieuwe ondersteunings aanvraag maken](https://support.microsoft.com/supportforbusiness/productselection). Het relevante MIM-onderdeel selecteren:
1. *beveiliging* van product familie selecteren
1. Selecteer product *Identity Manager 2016*
