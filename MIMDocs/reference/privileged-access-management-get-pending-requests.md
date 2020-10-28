---
title: PAM-aanvragen in behandeling ophalen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0ec4388ce5d5f748d9f779819d0a2d0bdec06791
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758884"
---
# <a name="get-pending-pam-requests"></a>PAM-aanvragen in behandeling ophalen
Wordt gebruikt door een privileged account om een lijst te retour neren met openstaande aanvragen waarvoor goed keuring is vereist.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

### <a name="query-parameters"></a>Queryparameters

Parameter | Beschrijving
----------|--------------
$filter | Optioneel. Geef een van de eigenschappen van de PAM-aanvraag in behandeling op in een filter expressie om een gefilterde lijst met antwoorden te retour neren. Zie [filteren in PAM rest API service Details](privileged-access-management-rest-api-service-details.md#filtering)voor meer informatie over ondersteunde Opera tors.
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
Een geslaagd antwoord bevat een lijst met goedkeurings objecten voor PAM-aanvragen met de volgende eigenschappen:

Eigenschap | Beschrijving
---------|-------------
RoleName | De weergave naam van de rol waarvoor goed keuring is vereist.
Requestor | De gebruikers naam van de aanvrager die moet worden goedgekeurd.
Reden | De reden van de gebruiker.
RequestedTTL | De aangevraagde verloop tijd in seconden.
RequestedTime | De aangevraagde tijd voor uitbrei ding.
CreationTime | De aanmaak tijd van de aanvraag.
FIMRequestID | Bevat één element, ' waarde ', met de unieke id (GUID) van de PAM-aanvraag.
RequestorID | Bevat één element, ' waarde ', met de unieke id (GUID) voor het Active Directory-account waarmee de PAM-aanvraag is gemaakt.
ApprovalObjectID | Bevat één element, ' waarde ', met de unieke id (GUID) voor het goedkeurings object.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het ophalen van PAM-aanvragen in behandeling.

### <a name="example-request"></a>Voor beeld: aanvraag

```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```

### <a name="example-response"></a>Voor beeld: antwoord

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
