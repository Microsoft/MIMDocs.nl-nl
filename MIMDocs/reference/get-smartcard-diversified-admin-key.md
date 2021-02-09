---
title: Smart Card gediversifieerde beheerders sleutel ophalen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5e21cd92496fd31ec12044f3b69599bf96bef62a
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835606"
---
# <a name="get-smart-card-diversified-admin-key"></a>Smart Card gediversifieerde beheerders sleutel ophalen
Hiermee wordt de gediversifieerde beheerder sleutel voor de opgegeven Smart Card opgehaald.

>[!IMPORTANT]
>De beheerders sleutel moet alleen worden gespreid wanneer het profiel sjabloon beleid aangeeft dat het moet zijn.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/diversifiedkey

### <a name="url-parameters"></a>URL-parameters

Parameter | Beschrijving
---------|------------
reqid | Vereist. De aanvraag-id die specifiek is voor Microsoft Identity Manager (MIM) Certificate Management (CM).
scid | Vereist. De Smart Card-ID die specifiek is voor MIM CM. De scid wordt opgehaald uit het object [micro soft. clm. Shared. Smart Cards. Smart Card](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

### <a name="query-parameters"></a>Queryparameters

Parameter | Beschrijving
---------|------------
ATR | Optioneel. De ATR-teken reeks (answer-to-reset) voor Smart Card.
cardid | Vereist. De kaart-ID.

### <a name="request-headers"></a>Aanvraagheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene aanvraag headers.

### <a name="request-body"></a>Aanvraagbody
Geen.

## <a name="response"></a>Antwoord
In deze sectie wordt het antwoord beschreven.

### <a name="response-codes"></a>Antwoord codes

Code  |Description  
---------|---------
200 | OK
204 | Geen inhoud
403 | Verboden
500 | Interne fout


### <a name="response-headers"></a>Antwoordheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene antwoord headers.

### <a name="response-body"></a>Hoofdtekst van de reactie
Als de bewerking is voltooid, wordt een byte-BLOB geretourneerd die de gediversifieerde beheerders sleutel vertegenwoordigt.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het ophalen van de gediversifieerde beheerders sleutel voor een Smart Card.

### <a name="example-request"></a>Voor beeld: aanvraag

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```

### <a name="example-response"></a>Voor beeld: antwoord

```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       

## <a name="see-also"></a>Zie ook

- [Micro soft. clm. Provision. RequestOperations. CreateSmartcard methode (teken reeks, teken reeks, aanvraag)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
