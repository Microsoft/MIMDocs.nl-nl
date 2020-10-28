---
title: Naslag informatie over Privileged Access Management REST API | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8a318ae5f12fd49e2ee949d81aefd86a7221e120
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758860"
---
# <a name="privileged-access-management-rest-api-reference"></a>Naslag informatie over Privileged Access Management REST API
Microsoft Identity Manager (MIM) 2016 voegt een nieuw scenario met de naam Privileged Access Management (PAM) toe. Met PAM kan een organisatie meer controle krijgen over de toegangs rechten van gebruikers accounts met hoge bevoegdheden, zoals systeem-of service beheerders, tot gevoelige bronnen. PAM beheert toegang tot account met hoge bevoegdheden door beperkte tijd toegangs rechten te bieden, just-in-time (JIT), wanneer de toegangs rechten nodig zijn.

Een gebruiker kan een van de volgende twee manieren om de MIM-service vragen om bevoegdheden voor bevoegde toegang:

- Door gebruik te maken van de PAM-REST API.
- Met behulp van de PAM Power shell New-PAMRequest-cmdlet.

De onderwerpen in deze hand leiding beschrijven de PAM-REST API. Voor meer informatie over het gebruik van de Power shell-cmdlet raadpleegt u _de test lab Guide: demonstring privileged Access Management using Microsoft Identity Manager_ , dat beschikbaar is op de Connect-site.

## <a name="pam-rest-api-resources-and-operations"></a>PAM-REST API resources en-bewerkingen
De PAM-REST API werkt op de volgende bronnen:
- **Pam-rol** : een pam-rol koppelt een verzameling gebruikers aan een verzameling toegangs rechten. De toegangs rechten worden gedefinieerd door verwijzing naar beveiligings groepen.  Elke PAM-rol heeft een lijst met gebruikers accounts met de naam kandidaten die recht hebben op verhoging naar de PAM-rol. U kunt de volgende bewerkingen uitvoeren op PAM-rollen:

    - [PAM-rollen ophalen](privileged-access-management-get-roles.md)

- **Pam-aanvraag** : een gebruiker die moet worden uitgebreid naar de pam-rol toegangs rechten, moet een pam-aanvraag indienen en goed keuring krijgen voor de aanvraag om deze uit te breiden. Het object PAM-aanvraag houdt de levens cyclus van deze aanvraag in de MIM-service bij. U kunt de volgende bewerkingen uitvoeren op PAM-aanvragen:

    - [PAM-aanvraag maken](privileged-access-management-create-request.md)
    - [PAM-aanvragen ophalen](privileged-access-management-get-requests.md)
    - [PAM-aanvraag sluiten](privileged-access-management-close-request.md)

- **Pam-aanvraag in behandeling** : wordt gebruikt voor het goed keuren of afwijzen van Pam-aanvragen die door gebruikers zijn ingediend. U kunt de volgende bewerkingen uitvoeren op PAM-aanvragen in behandeling:

    - [PAM-aanvragen in behandeling ophalen](privileged-access-management-get-pending-requests.md)
    - [Een PAM-aanvraag in behandeling goedkeuren of weigeren](privileged-access-management-approve-reject-pending-request.md)

- **Pam-sessie** : wanneer u de pam-rest API gebruikt, heeft de client (bijvoorbeeld een webbrowser) een sessie met het PAM-rest API eind punt. In deze sessie wordt de client geverifieerd voor het REST API-eind punt. U kunt de volgende bewerkingen uitvoeren op PAM-sessies:

     - [Gegevens van PAM-sessie ophalen](privileged-access-management-get-session-info.md)

Zie [PAM rest API service-Details](privileged-access-management-rest-api-service-details.md)voor meer informatie over de service.

## <a name="pam-sample-portal-on-github"></a>PAM-voorbeeld Portal op GitHub
Een manier om te leren hoe u de PAM-REST API gebruikt, is door gebruik te maken van de PAM-voorbeeld Portal, een webtoepassing die gebruikmaakt van de API. U kunt de code voor de PAM-voorbeeld Portal vinden in de [pam-voorbeeld opslagplaats op github](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). Meer informatie over het implementeren van de voorbeeld Portal vindt u in de PAM-test lab Guide.
