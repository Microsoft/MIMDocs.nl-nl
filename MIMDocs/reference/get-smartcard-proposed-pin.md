---
title: Voorgestelde pincode voor Smart Card ophalen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9d4f1444efa490f980e29440342844ac246d8a20
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758800"
---
# <a name="get-smart-card-proposed-pin"></a>Voorgestelde pincode voor Smart Card ophalen
Hiermee wordt de door de server gegenereerde gebruikers pincode opgehaald.

>[!IMPORTANT]
>De server stelt alleen de pincode in als het profiel sjabloon beleid aangeeft dat het moet worden uitgevoerd. Anders moet de gebruiker de pincode opgeven.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpin

### <a name="url-parameters"></a>URL-parameters

Parameter | Beschrijving
---------|------------
id | De Smart Card-ID die specifiek is voor Microsoft Identity Manager (MIM) Certificate Management (CM). De ID wordt opgehaald uit het object micro soft. clm. Shared. Smart Card.

### <a name="query-parameters"></a>Queryparameters

Parameter | Beschrijving
---------|------------
ATR | De ATR-teken reeks (answer-to-reset) voor Smart Card.
cardid | De ID van de Smart Card.
uitdaging | Een met base 64 gecodeerde teken reeks die de uitdaging vertegenwoordigt die is uitgegeven door de Smart Card.

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
Als de bewerking is voltooid, wordt een teken reeks geretourneerd die de pincode vertegenwoordigt die door de server wordt voorgesteld.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het ophalen van de door de server gegenereerde gebruikers pincode.

### <a name="example-request"></a>Voor beeld: aanvraag

```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```

### <a name="example-response"></a>Voor beeld: antwoord

```
HTTP/1.1 200 OK

... body coming soon
```       
