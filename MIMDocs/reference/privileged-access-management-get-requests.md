---
title: PAM-aanvragen ophalen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 620eebd6-e4c3-473b-b824-ee6cfe83e509
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0eeaee3a6355229de94bcc390ad07da3e00b829f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758866"
---
# <a name="get-pam-requests"></a>PAM-aanvragen ophalen
Wordt gebruikt door een privileged account om een geschiedenis te retour neren van eerder geboekte PAM-aanvragen.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/api/pamresources/pamrequests

### <a name="query-parameters"></a>Queryparameters

Parameter | Beschrijving
----------|--------------
$filter | Optioneel. Geef een van de PAM-aanvraag eigenschappen in een filter expressie op om een gefilterde lijst met antwoorden te retour neren. Zie [filteren in PAM rest API service Details](privileged-access-management-rest-api-service-details.md#filtering)voor meer informatie over ondersteunde Opera tors.
v | Optioneel. De API-versie. Als dat niet het geval is, wordt de huidige versie van de API (meest recent vrijgegeven) gebruikt. Zie [versie beheer in PAM rest API service Details](privileged-access-management-rest-api-service-details.md#versioning)voor meer informatie.

### <a name="request-headers"></a>Aanvraagheaders
Zie [HTTP-aanvraag-en reactie headers](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *pam-rest API service Details* voor algemene aanvraag headers.

### <a name="request-body"></a>Aanvraagbody
Geen.

## <a name="response"></a>Antwoord
In deze sectie wordt het antwoord beschreven.

### <a name="response-codes"></a>Antwoord codes

Code  |Beschrijving  
---------|---------
200 | OK
401 | Niet geautoriseerd
403 | Verboden
408 | Time-out van aanvraag   
500 | Interne server fout
503 | Service niet beschikbaar

### <a name="response-headers"></a>Antwoordheaders
Zie [HTTP-aanvraag-en reactie headers](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *pam-rest API service Details* voor algemene aanvraag headers.

### <a name="response-body"></a>Hoofdtekst van de reactie
Een geslaagd antwoord bevat een lijst met PAM-aanvraag objecten met de volgende eigenschappen:

Eigenschap | Beschrijving
--------|-------------
RequestID | De unieke id (GUID) voor de PAM-aanvraag.
CreatorID | Een unieke id (GUID) voor het Active Directory-account waarmee de PAM-aanvraag is gemaakt.
Reden | De reden voor uitbrei ding.
DisplayName | De weergave naam van de PAM-aanvraag in MIM.
CreationTime | De aanmaak tijd van de aanvraag.
CreationMethod | De methode die wordt gebruikt om de aanvraag te maken.
ExpirationTime | De verval tijd van de aanvraag.
RoleID| De unieke id (GUID) van de PAM-rol.
RequestedTTL | De aangevraagde time-out voor de verloop tijd in seconden.
RequestedTime | De aangevraagde tijd voor uitbrei ding.
RequestedStatus | De status van de aanvraag. De mogelijke waarden zijn ' actief ', ' gesloten ', ' sluiten ', ' verlopen ', ' PendingApproval ' en ' geweigerd '.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het ophalen van de geposte PAM-aanvragen.

### <a name="example-request"></a>Voor beeld: aanvraag

```
GET /api/pamresources/pamrequests?$filter=CreationTime%20gt%20datetime'2015-06-12T04:49:32.431Z'%20and%20CreationTime%20lt%20datetime'2015-07-13T04:49:32.432Z' HTTP/1.1
```

### <a name="example-response"></a>Voor beeld: antwoord

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests",
    "value":[
        {
            "RequestId":"b22e1343-9a2b-4e33-a70a-1bb7b2d405b9",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-06-23T11:34:38.58Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-06-23T12:34:38.847Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-23T11:34:36.417Z",
            "RequestStatus":"Expired"
        },
        {
            "RequestId":"3a98d1c7-524d-4b72-9da7-bd9f907eab55",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Reason for Request",
            "CreationTime":"2015-07-12T04:35:14.433Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T04:43:51.95Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:35:00Z",
            "RequestStatus":"Closed"
        },
        {
            "RequestId":"f5e80be1-e9a3-42c4-81f8-4be5a4a429f4",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-07-12T04:48:17.46Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T05:48:17.853Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-12T04:48:14.057Z",
            "RequestStatus":"Active"
        },
        {
            "RequestId":"b0f0ddc0-c809-4770-9d39-0d706f97a2de",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"",
            "CreationTime":"2015-06-30T07:01:13.147Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-30T07:01:13.119Z",
            "RequestStatus":"Rejected"
        },
        {
            "RequestId":"5ec10e61-cdd1-404e-a18e-740467d87dbf",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Example Reason",
            "CreationTime":"2015-07-12T04:49:09.963Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:50:00Z",
            "RequestStatus":"PendingApproval"
        }
    ]
}
```       
