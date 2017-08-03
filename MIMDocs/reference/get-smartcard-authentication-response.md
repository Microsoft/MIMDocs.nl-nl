---
title: Smartcard verificatie respons ophalen | Microsoft Docs
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1c7a6f5e7ebd1e6e4cfc0992607adb91743f343e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-authentication-response"></a>Smartcard verificatie respons ophalen
Het antwoord op een Base CSP-verificatievraag opgehaald.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/API/V1.0/Requests/{reqid}/smartcards/{scid}/authenticationresponse

###<a name="url-parameters"></a>URL-Parameters
Parameter | Beschrijving
---------|------------
reqid | Vereist. De aanvraag-id (MIM CM-specifiek).
scid | Vereist. De smartcard-id (MIM CM-specifiek). Verkregen van de [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) object.
###<a name="query-parameters"></a>Query-Parameters
Parameter | Beschrijving
---------|------------
ATR | Optioneel. De smartcard tekenreeks (ATR) antwoord opnieuw instellen.
kaart-id | Vereist. De kaart-id.
uitdaging | Vereist. Een base-64 gecodeerde tekenreeks voor de uitdaging uitgegeven door de smartcard.
gediversifieerd | Vereist. Er is een Booleaanse vlag die aangeeft weer de smartcard beheersleutel gediversifieerd.


###<a name="request-headers"></a>Aanvraagheaders
Raadpleeg voor algemene aanvraagheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="request-body"></a>Aanvraagtekst
geen

##<a name="response"></a>Antwoord
###<a name="response-codes"></a>Reactiecodes
Code  |Beschrijving  
---------|---------
200     | OK
204 | Er is geen inhoud
403 | Is niet toegestaan
500 | Interne fout

###<a name="response-headers"></a>Antwoordheaders
Zie voor veelgebruikte antwoordheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="response-body"></a>Antwoordtekst
Indien geslaagd kunt retourneert een byte BLOB die het challenge-antwoord vertegenwoordigt.

##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1

```
###<a name="response"></a>Antwoord
```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       
##<a name="see-also"></a>Zie ook

- [Microsoft.Clm.Provision.ExecuteOperations.GetBaseCspResponse methode](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
