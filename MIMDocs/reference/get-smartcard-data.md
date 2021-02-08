---
title: Smart Card-profielen ophalen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 605dfe1359b5706b27682a086f039f53a5ddaa05
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835685"
---
# <a name="get-smart-card-profiles"></a>Smart Card-profielen ophalen
Hiermee haalt u een lijst met smartcard profielen voor een gebruiker op. De lijst bevat de mogelijke bewerkingen die kunnen worden uitgevoerd door de huidige gebruiker. Een aanvraag kan vervolgens worden gestart voor elk van de opgegeven bewerkingen.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards <br/> /CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


### <a name="url-parameters"></a>URL-parameters

Eigenschap| Beschrijving
---------|--------
smartcarduuid | Optioneel. De UUID van de Smart Card zoals aangegeven door Microsoft Identity Manager (MIM) Certificate Management (CM). De waarde komt overeen met het veld ' uuid ' in het object [micro soft. clm. Shared. Smart Cards. Smart Card](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

### <a name="query-parameters"></a>Queryparameters

Eigenschap| Beschrijving
---------|--------
cardid | Optioneel. De UUID van de Smart Card zoals aangegeven door MIM CM. De waarde komt overeen met het veld ' uuid ' in het object [micro soft. clm. Shared. Smart Cards. Smart Card](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

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
Als dit is gelukt, wordt een JSON-Serialized [micro soft. clm. Shared. Smart Cards. Smart Card](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) -object met de volgende eigenschappen geretourneerd:

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
In deze sectie vindt u een voor beeld van het ophalen van smartcard profielen voor een gebruiker.

### <a name="example-request-1"></a>Voor beeld: aanvraag 1

```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1
```

### <a name="example-response-1"></a>Voor beeld: antwoord 1

```
HTTP/1.1 200 OK

{
    "Uuid":"438d1b30-f3b4-4bed-85fa-285e08605ba7",
    "Status":3,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{d1ef6869-5517-42a0-8f05-16ca1a0b834d}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       

### <a name="example-request-2"></a>Voor beeld: aanvraag 2

```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```

### <a name="example-response-2"></a>Voor beeld: respons 2

```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":2,
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
