---
title: Een PAM-aanvraag in behandeling goed keuren of afwijzen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 743c28b343c374d3406600751b379e8a4695c2d0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758887"
---
# <a name="approve-or-reject-a-pending-pam-request"></a>Een PAM-aanvraag in behandeling goed keuren of afwijzen
Wordt gebruikt door een bevoegd account voor het goed keuren, sluiten of afwijzen van een aanvraag voor een verhoging van een PAM-rol.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
POST     |/api/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/api/pamresources/pamrequeststoapprove({approvalId)/Reject

### <a name="url-parameters"></a>URL-parameters

Parameter | Beschrijving
----------|-----------
approvalId | De id (GUID) van het goedkeurings object in PAM, opgegeven als `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'` .

### <a name="query-parameters"></a>Queryparameters

Parameter | Beschrijving
----------|--------------
v | Optioneel. De API-versie. Als dat niet het geval is, wordt de huidige versie van de API (meest recent vrijgegeven) gebruikt. Zie [versie beheer in PAM rest API service Details](privileged-access-management-rest-api-service-details.md#versioning)voor meer informatie.


### <a name="request-headers"></a>Aanvraag headers
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
Geen.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het goed keuren van een aanvraag om te verhogen naar een PAM-rol.

### <a name="example-request"></a>Voor beeld: aanvraag

```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1
```

### <a name="example-response"></a>Voor beeld: antwoord

```
HTTP/1.1 200 OK
```       
