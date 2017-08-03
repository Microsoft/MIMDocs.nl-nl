---
title: PAM-sessie Info ophalen | Microsoft Docs
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
ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 150bc7e863fc679e785526935155611fb75cd69b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-session-info"></a>PAM-sessie Info ophalen
Door een bevoegd account gebruikt voor het ophalen van de gebruikersnaam van het account dat aan de sessie is aangemeld.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `http://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/API/Session/sessioninfo

###<a name="query-parameters"></a>Query-Parameters
Parameter | Beschrijving
----------|--------------
v | Optioneel. De API-versie. Als er niet is opgenomen, wordt de huidige (meest recent) versie van de API worden gebruikt. Zie voor meer informatie [versiebeheer in de Details van de PAM REST API-Service](privileged-access-management-rest-api-service-details.md#versioning)

###<a name="request-headers"></a>Aanvraagheaders
Raadpleeg voor algemene aanvraagheaders [HTTP-aanvraag en Reactieheaders](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST API-servicedetails*.
###<a name="request-body"></a>Aanvraagtekst
geen

##<a name="response"></a>Antwoord
###<a name="response-codes"></a>Reactiecodes
Code  |Beschrijving  
---------|---------
200 | OK
401 | Niet geautoriseerd
403 | Is niet toegestaan
408 | Time-out aanvraag   
500 | Interne serverfout
503 | Service is niet beschikbaar

###<a name="response-headers"></a>Antwoordheaders
Zie voor veelgebruikte antwoordheaders [HTTP-aanvraag en Reactieheaders](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST API-servicedetails*.
###<a name="response-body"></a>Antwoordtekst
Een geslaagde reactie retourneert een object van de PAM-sessie met de volgende eigenschappen.

Eigenschap| Beschrijving
--------|-------------
Gebruikersnaam| De gebruikersnaam van het account dat is aangemeld bij deze sessie.

##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
GET /api/session/sessioninfo/ HTTP/1.1
```
###<a name="response"></a>Antwoord
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       
