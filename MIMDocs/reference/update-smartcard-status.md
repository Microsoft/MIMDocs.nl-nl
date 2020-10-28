---
title: Status van Smart Card bijwerken | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: fe6d59377ef3218fde0df99365506ef9ec143a6f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758983"
---
# <a name="update-smart-card-status"></a>Smart Card-status bijwerken
Hiermee wordt de status van een Smart Card bijgewerkt.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### <a name="url-parameters"></a>URL-parameters

Parameter | Beschrijving
---------|------------
reqid | Vereist. De aanvraag-id die specifiek is voor Microsoft Identity Manager (MIM) Certificate Management (CM).
scid | Vereist. De Smart Card-ID die specifiek is voor MIM CM. De waarde komt overeen met het veld ' uuid ' in het object [micro soft. clm. Shared. Smart Cards. Smart Card](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

### <a name="request-headers"></a>Aanvraagheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene aanvraag headers.

### <a name="request-body"></a>Aanvraagbody
De aanvraag tekst bevat de volgende eigenschappen:

Eigenschap | Beschrijving
---------|-----------
status | De status voor het instellen van de aanvraag in, bijvoorbeeld ' buiten gebruik '.

## <a name="response"></a>Antwoord
In deze sectie wordt het antwoord beschreven.

### <a name="response-codes"></a>Antwoord codes

Code  |Beschrijving  
---------|---------
200     | OK
204 | Geen inhoud
403 | Verboden
500 | Interne fout

### <a name="response-headers"></a>Antwoordheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene antwoord headers.

### <a name="response-body"></a>Hoofdtekst van de reactie
Als dit is gelukt, wordt een JSON-Serialized [micro soft. clm. Shared. Smart Cards. Smart Card](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) -object met de volgende eigenschappen geretourneerd:

Naam | Beschrijving
-----|-----------
AssignedUserUuid | De id van de gebruiker aan wie de Smart Card is toegewezen.
ATR | De ATR-teken reeks (answer-to-reset) voor de Smart Card voor de kaart die momenteel wordt geïnitialiseerd.
Opmerking | De opmerking met een beschrijving van de Smart Card.
Markering | De vlaggen waarmee de Smart Card wordt beschreven.
Middleware | De middleware voor de Smart Card.
ParentSmartcardUuid | De id van de oude Smart Card die is vervangen door de Smart Card.
PermanentSmartcardUuid | De id van de permanente Smart Card die is gekoppeld aan de Smart Card.
PrimarySmartcardUuid | De id van de primaire Smart Card.
ProfileTemplateUuid | De id van de profiel sjabloon die het beleid en de instellingen bevat die van toepassing zijn op de Smart Card.
ProfileTemplateVersion | De versie van de profiel sjabloon op het moment dat het Smart Card-profiel is gemaakt.
SerialNumber | Het serie nummer van de Smart Card.
Status | De status van de Smart Card.
UUID | De id van het Smart Card-profiel.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het bijwerken van de status van een Smart Card.

### <a name="example-request"></a>Voor beeld: aanvraag

```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```

### <a name="example-response"></a>Voor beeld: antwoord

```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":6,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       

## <a name="see-also"></a>Zie ook

- [De klasse micro soft. clm. Shared. Smart Cards. Smart Card](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
