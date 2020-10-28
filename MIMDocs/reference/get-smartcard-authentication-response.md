---
title: Smart card-verificatie reactie ophalen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ab320457c5d676cc381306e83f685fe288dc7ef9
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758701"
---
# <a name="get-smart-card-authentication-response"></a>Smart card-verificatie reactie ophalen
Hiermee wordt de reactie van een basis verificatie vraag naar een CSP (Cryptographic Service Provider) opgehaald.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse

### <a name="url-parameters"></a>URL-parameters

Parameter | Beschrijving
---------|------------
reqid | Vereist. De aanvraag-id die specifiek is voor Microsoft Identity Manager (MIM) Certificate Management (CM).
scid | Vereist. De Smart Card-ID die specifiek is voor MIM CM. De scid wordt opgehaald uit het object [micro soft. clm. Shared. Smart Cards. Smart Card](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

### <a name="query-parameters"></a>Queryparameters

Parameter | Beschrijving
---------|------------
ATR | Optioneel. De ATR-teken reeks (answer-to-reset) voor Smart Card.
cardid | Vereist. De ID van de Smart Card.
uitdaging | Vereist. Een met base 64 gecodeerde teken reeks die de uitdaging vertegenwoordigt die is uitgegeven door de Smart Card.
voldoende | Vereist. Een Booleaanse vlag die aangeeft of de beheerder sleutel voor de Smart Card is gespreid.

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
Als de bewerking is voltooid, wordt een byte-BLOB geretourneerd die het antwoord op de vraag vertegenwoordigt.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het verkrijgen van de reactie op een basis verificatie controle van de CSP.

### <a name="example-request"></a>Voor beeld: aanvraag

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1
```

### <a name="example-response"></a>Voor beeld: antwoord

```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       

## <a name="see-also"></a>Zie ook

- [Microsoft.Clm.Provision.Exemethode cuteOperations. GetBaseCspResponse](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
