---
title: Ophalen van PAM-rollen | Microsoft Docs
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
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4cb4bbc6c7696f354e5759a677ece88a4e606544
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-roles"></a>Ophalen van PAM-rollen
Door een bevoegd account gebruikt voor het weergeven van de PAM-rollen waarvan het account geschikt is is.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `http://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/API/pamresources/pamroles

###<a name="query-parameters"></a>Query-Parameters
Parameter | Beschrijving
----------|--------------
$filter | Optioneel. Geef een van de PAM-Role-eigenschappen in een filterexpressie een gefilterde lijst van antwoorden retourneren. Zie voor meer informatie over ondersteunde operatoren [filteren in de Details van de PAM REST API-Service](privileged-access-management-rest-api-service-details.md#filtering)
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
Een geslaagde reactie bevat een verzameling van een of meer PAM-rollen, die elk de volgende eigenschappen heeft.

Eigenschap | Beschrijving
--------|-------------
RoleID | De unieke id (GUID) van de PAM-rol.
DisplayName | Weergavenaam van de PAM-rol in de MIM-service.
Beschrijving | Beschrijving van de PAM-rol in de MIM-service.
TTL | De rol toegangsrechten maximale verlopen time-out in seconden.
AvailableFrom | De vroegste tijd van de dag dat een verzoek wordt geactiveerd.
AvailableTo | De laatste tijd van de dag dat een aanvraag wordt geactiveerd.
MFAEnabled | Een Boolean die aangeeft of activeringsaanvragen voor deze rol een challenge MFA nodig.
ApprovalEnabled | Een Boolean die aangeeft of activeringsaanvragen voor deze rol moeten worden goedgekeurd door de roleigenaar van een.
AvailibitlyWindowEnabled | Een Boolean die aangeeft of de rol kan alleen worden geactiveerd tijdens een opgegeven tijdsinterval.

##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
GET /api/pamresources/pamroles HTTP/1.1
```
###<a name="response"></a>Antwoord
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
