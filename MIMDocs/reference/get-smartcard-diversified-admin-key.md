---
title: Ophalen van de Smartcard gediversifieerde beheersleutel | Microsoft Docs
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
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 57a8481b2a4f976717b07061e96ccb041164a5a6
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-diversified-admin-key"></a>Smartcard gediversifieerde Admin sleutel ophalen
Hiermee haalt u de gediversifieerde administratorsleutel voor de opgegeven smartcard.

**Opmerking**: de beheersleutel hoeft alleen te worden gediversifieerd als het beleid van de sjabloon profiel geeft aan dat moet worden.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/API/V1.0/Requests/{reqid}/smartcards/{scid}/diversifiedkey

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
Indien geslaagd kunt retourneert een byte BLOB die de gediversifieerde beheersleutel vertegenwoordigt.

##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```
###<a name="response"></a>Antwoord
```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       
##<a name="see-also"></a>Zie ook

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard-methode (String, String, aanvraag)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
