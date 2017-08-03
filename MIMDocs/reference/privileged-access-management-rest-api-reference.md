---
title: Privileged Access Management REST API-verwijzing | Microsoft Docs
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 02de09a7de843cb42080daa4ce43a5a0c74f2c18
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="privileged-access-management-rest-api-reference"></a>REST API-verwijzing voor Privileged Access Management
Microsoft Identity Manager (MIM) 2016 voegt een nieuw scenario Privileged Access Management (PAM) genoemd. PAM kan een organisatie hebt meer controle over de toegangsrechten van hoge bevoegdheden gebruikersaccounts, zoals systeem of de service-beheerders tot gevoelige resources. PAM bepaalt toegang met hoge bevoegdheid dankzij de toegangsrechten beperkte periode, alleen in de tijd (Just in time), wanneer de toegangsrechten nodig zijn.

Een gebruiker MIM-Service kunt vragen voor privileged toegangsrechten (uitbreiding) op twee manieren:

- Met behulp van de PAM REST API
- Met de cmdlet PAM PowerShell New-PAMRequest

De onderwerpen in deze handleiding worden beschreven de PAM REST API. Voor meer informatie over het gebruik van de PowerShell-cmdlet Zie de Test Lab Guide: demonstratie Privileged Access Management met Microsoft Identity Manager, die beschikbaar is op de connect-site.

##<a name="pam-rest-api-resources-and-operations"></a>PAM REST API bronnen en bewerkingen
De PAM REST API is van invloed op de volgende bronnen
- **PAM-Role**: een PAM-rol een verzameling gebruikers koppelt aan een verzameling van toegangsrechten. De toegangsrechten zijn gedefinieerd op basis van beveiligingsgroepen.  Elke PAM-rol heeft een lijst van gebruikersaccounts, aangeroepen kandidaten die onder de uitbreiden naar de PAM-rol. U kunt de volgende bewerkingen uit op de PAM-rollen uitvoeren:

    - [PAM-rollen ophalen](privileged-access-management-get-roles.md)

- **PAM Request**: een gebruiker die wil uitbreiden naar de toegangsrechten van een PAM-rol heeft tot een PAM-aanvraag indient en de goedkeuring voor de aanvraag om te kunnen uitbreiden. De PAM-Request-object houdt de levenscyclus van deze aanvraag in de MIM-Service. U kunt de volgende bewerkingen uit op de PAM-aanvragen uitvoeren:

    - [PAM-aanvraag maken](privileged-access-management-create-request.md)
    - [PAM-aanvragen ophalen](privileged-access-management-get-requests.md)
    - [PAM-aanvraag sluiten](privileged-access-management-close-request.md)

- **In afwachting van PAM Request**: gebruikt voor het goedkeuren of afwijzen PAM-aanvragen die zijn ingediend door gebruikers. U kunt de volgende bewerkingen uitvoeren op PAM-aanvragen in behandeling:

    - [PAM-aanvragen in behandeling ophalen](privileged-access-management-get-pending-requests.md)
    - [Een PAM-aanvraag in behandeling goedkeuren of weigeren](privileged-access-management-approve-reject-pending-request.md)

- **PAM Session**: wanneer u de PAM REST API, de client (bijvoorbeeld een webbrowser) heeft een sessie met de PAM REST API-eindpunt. In deze sessie is de client is geverifieerd met de REST-API-eindpunt. U kunt de volgende bewerkingen uit op de PAM-sessies uitvoeren:

     - [Gegevens van PAM-sessie ophalen](privileged-access-management-get-session-info.md)

Zie voor meer informatie over de service, [Details van de PAM REST API-Service](privileged-access-management-rest-api-service-details.md)

##<a name="pam-sample-portal-on-github"></a>PAM-Voorbeeldportal op GitHub
Er is een manier om informatie over het gebruik van de PAM REST API met behulp van de PAM-voorbeeldportal een voorbeeld van een webtoepassing die gebruikmaakt van de API. U vindt de code voor de portal voor PAM Sample in de [PAM voorbeeld opslagplaats op GitHub](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). U kunt informatie over het implementeren van de voorbeeldportal in de PAM Test Lab Guide.
