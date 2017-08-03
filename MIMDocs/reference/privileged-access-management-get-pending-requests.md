---
title: Aanvragen in behandeling PAM | Microsoft Docs
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
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aa1e8cd48b1bcca6e856bd553b6b92a062a08ff4
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-pending-pam-requests"></a>Ophalen van in behandeling PAM-aanvragen
Door een bevoegd account gebruikt voor het retourneren van een lijst met openstaande aanvragen die moeten worden goedgekeurd.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `http://api.contoso.com`.
##<a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/API/pamresources/pamrequeststoapprove

###<a name="query-parameters"></a>Query-Parameters
Parameter | Beschrijving
----------|--------------
$filter | Optioneel. Geef een van de eigenschappen Perding PAM Request in een filterexpressie een gefilterde lijst van antwoorden retourneren. Zie voor meer informatie over ondersteunde operatoren [filteren in de Details van de PAM REST API-Service](privileged-access-management-rest-api-service-details.md#filtering)
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
Een geslaagde reactie bevat een lijst met PAM-aanvraag goedkeuring objecten met de volgende eigenschappen.

Eigenschap | Beschrijving
---------|-------------
RoleName | De weergavenaam van de rol waarvoor goedkeuring is vereist.
Aanvrager | De gebruikersnaam van de aanvrager worden goedgekeurd.
Reden | De reden dat is opgegeven door de gebruiker.
RequestedTTL | De aangevraagde verlooptijd in seconden.
RequestedTime | De aangevraagde tijd voor uitbreiding van bevoegdheden.
CreationTime | De aanmaaktijd van de aanvraag.
FIMRequestID | Een enkel element '' waarde '' met de unieke id (GUID) van de PAM-aanvraag bevat.
RequestorID | Bevat een enkel element '' waarde '' met de unieke id (GUID) voor het Active Directory-account dat de PAM-aanvraag gemaakt.
ApprovalObjectID | Bevat een enkel element '' waarde '' met de unieke id (GUID) voor het Object goedkeuring.

##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###<a name="response"></a>Antwoord
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
