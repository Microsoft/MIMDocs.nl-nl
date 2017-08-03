---
title: Get-profiel staat Operations | Microsoft Docs
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
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8e8428984404b2aebda8f53e4f7841b699a5d23a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-state-operations"></a>Bewerkingen van de status van Get-profiel
Een lijst van mogelijke bewerkingen die kunnen worden uitgevoerd door de huidige gebruiker op het opgegeven profiel ophalen. Een aanvraag kan vervolgens worden gestart voor een van de opgegeven bewerkingen.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/API/V1.0/Profiles/{id}/operations <br/>/CertificateManagement/API/V1.0/smartcards/{id}/operations

###<a name="url-parameters"></a>URL-Parameters
Parameter | Beschrijving
---------|------------
ID | De id (GUID) van het profiel of smartcard.

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
Indien geslaagd kunt retourneert een lijst van mogelijke bewerkingen die kunnen worden uitgevoerd door de gebruiker op de smartcard. Deze lijst kan een onbeperkt aantal het volgende bevatten: *OnlineUpdate*, *vernieuwen*, *herstellen*, *RecoverOnBehalf*, *buiten gebruik stellen*, *intrekken*, en *blokkering*.

##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```
###<a name="response"></a>Antwoord
```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
