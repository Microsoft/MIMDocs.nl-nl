---
title: Ophalen van de Smartcard voorgestelde PINCODE | Microsoft Docs
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
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 08d28819402dd56f996d39aa03b8ac829bef0838
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-proposed-pin"></a>Voorgestelde PINCODE Smartcard ophalen
Hiermee haalt u de server gegenereerde gebruikerspincode.

**Opmerking**: de server wordt de PINCODE alleen ingesteld als het beleid van de sjabloon profiel geeft aan dat moet worden gedaan. Anders moet de gebruiker deze opgeven.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/API/V1.0/smartcards/{id}/serverproposedpin

###<a name="url-parameters"></a>URL-Parameters
Parameter | Beschrijving
---------|------------
ID | De smartcard-id (MIM CM-specifiek). Bij het Microsft.Clm.Shared.Smartcard-object ophalen.
###<a name="query-parameters"></a>Query-Parameters
Parameter | Beschrijving
---------|------------
ATR | De atr kaart.
kaart-id | De kaart-id.
uitdaging | Een base-64 gecodeerde tekenreeks voor de uitdaging uitgegeven door de smartcard.

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
Indien geslaagd kunt retourneert een tekenreeks met de PINCODE voorgesteld door de server.

##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###<a name="response"></a>Antwoord
```
HTTP/1.1 200 OK

... body coming soon
```       
