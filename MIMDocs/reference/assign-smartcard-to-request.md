---
title: Een Smart Card toewijzen aan een aanvraag | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a8acf3defc2087fc6ea08bf8063579058465fcc8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758752"
---
# <a name="assign-a-smart-card-to-a-request"></a>Een Smart Card toewijzen aan een aanvraag
Verbindt de opgegeven Smart Card met de opgegeven aanvraag. Nadat een Smart Card is gebonden, kan de aanvraag alleen worden uitgevoerd met de opgegeven kaart.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

### <a name="url-parameters"></a>URL-parameters
Geen.

### <a name="request-headers"></a>Aanvraagheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene aanvraag headers.

### <a name="request-body"></a>Aanvraagbody
De aanvraag tekst bevat de volgende eigenschappen:

Eigenschap | Beschrijving
---------|-----------
requestid | De ID van de aanvraag om verbinding te maken met de Smart Card.
cardid | De cardid van de Smart Card.
ATR | De ATR-teken reeks (answer-to-reset) voor Smart Card.


## <a name="response"></a>Antwoord
In deze sectie wordt het antwoord beschreven.

### <a name="response-codes"></a>Antwoord codes

Code  |Beschrijving  
---------|---------
201 | Gemaakt
204 | Geen inhoud
403 | Verboden
500 | Interne fout

### <a name="response-headers"></a>Antwoordheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene antwoord headers.

### <a name="response-body"></a>Hoofdtekst van de reactie
Als de bewerking is voltooid, wordt een URI geretourneerd naar het zojuist gemaakte Smart Card-object.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het binden van een Smart Card.

### <a name="example-request"></a>Voor beeld: aanvraag

```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}
```

### <a name="example-response"></a>Voor beeld: antwoord

```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```

## <a name="see-also"></a>Zie ook

- [Micro soft. clm. Provision. RequestOperations. CreateSmartcard methode (teken reeks, teken reeks, aanvraag)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
