---
title: PAM-rollen ophalen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f802dc22019f1ad97fc5cce8c9ed2e000349dc58
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758944"
---
# <a name="get-pam-roles"></a>PAM-rollen ophalen
Wordt gebruikt door een bevoegd account voor het weer geven van de PAM-rollen waarvoor het account een kandidaat is.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/api/pamresources/pamroles

### <a name="query-parameters"></a>Queryparameters

Parameter | Beschrijving
----------|--------------
$filter | Optioneel. Geef een van de eigenschappen van de PAM-rol in een filter expressie op om een gefilterde lijst met antwoorden te retour neren. Zie [filteren in PAM rest API service Details](privileged-access-management-rest-api-service-details.md#filtering)voor meer informatie over ondersteunde Opera tors.
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
Een geslaagde reactie bevat een verzameling van een of meer PAM-rollen, die elk de volgende eigenschappen hebben:

Eigenschap | Beschrijving
--------|-------------
RoleID | De unieke id (GUID) van de PAM-rol.
DisplayName | De weergave naam van de PAM-rol in de MIM-service.
Beschrijving | De beschrijving van de PAM-rol in de MIM-service.
TTL | De toegangs rechten van de rol hebben de time-out voor de maximale verloop tijd in seconden.
AvailableFrom | De vroegste tijd van de dag waarop een aanvraag wordt geactiveerd.
AvailableTo | Het laatste tijdstip waarop een aanvraag wordt geactiveerd.
MFAEnabled | Een Booleaanse waarde die aangeeft of activerings aanvragen voor deze rol een MFA-Challenge vereisen.
ApprovalEnabled | Een Booleaanse waarde die aangeeft of activerings aanvragen voor deze rol goed keuring vereisen door een eigenaar van een rol.
AvailabilityWindowEnabled | Een Booleaanse waarde die aangeeft of de rol kan worden geactiveerd tijdens een opgegeven tijds interval.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het ophalen van de PAM-rollen.

### <a name="example-request"></a>Voor beeld: aanvraag

```
GET /api/pamresources/pamroles HTTP/1.1
```

### <a name="example-response"></a>Voor beeld: antwoord

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
