---
title: Bewerkingen voor profiel status ophalen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 35246570b52285f916b74785c17ce55f2967fb7d
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758824"
---
# <a name="get-profile-state-operations"></a>Bewerkingen voor profiel status ophalen
Hiermee haalt u een lijst met mogelijke bewerkingen op die kunnen worden uitgevoerd door de huidige gebruiker op het opgegeven profiel. Een aanvraag kan vervolgens worden gestart voor elk van de opgegeven bewerkingen.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/operations <br/>/CertificateManagement/api/v1.0/smartcards/{id}/operations

### <a name="url-parameters"></a>URL-parameters

Parameter | Beschrijving
---------|------------
id | De id (GUID) van het profiel of de Smart Card.

### <a name="request-headers"></a>Aanvraagheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene aanvraag headers.

### <a name="request-body"></a>Aanvraagbody
Geen.

## <a name="response"></a>Antwoord
In deze sectie wordt het antwoord beschreven.

### <a name="response-codes"></a>Antwoord codes

Code  |Beschrijving  
---------|---------
200 | OK
204 | Geen inhoud
403 | Verboden
500 | Interne fout

### <a name="response-headers"></a>Antwoordheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene antwoord headers.

### <a name="response-body"></a>Hoofdtekst van de reactie
Als de bewerking is voltooid, wordt een lijst met mogelijke bewerkingen geretourneerd die kunnen worden uitgevoerd door de gebruiker op de Smart Card. De lijst kan een wille keurig aantal van de volgende bewerkingen bevatten: **OnlineUpdate** , **vernieuwen** , **herstellen** , **RecoverOnBehalf** , **buiten gebruik stellen** , **intrekken** en **blok kering opheffen** .

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het ophalen van profiel status bewerkingen voor de huidige gebruiker.

### <a name="example-request"></a>Voor beeld: aanvraag

```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```

### <a name="example-response"></a>Voor beeld: antwoord

```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
