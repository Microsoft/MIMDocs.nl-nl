---
title: PAM-aanvraag maken | Microsoft Docs
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
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de28af5c49eb98c5a1ccfbd8ed9353cf202e9e66
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="create-pam-request"></a>PAM-aanvraag maken
Door een bevoegd account gebruikt voor het uitbreiden naar een PAM-rol.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `http://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
POST     |/API/pamresources/pamrequests

###<a name="query-parameters"></a>Query-Parameters
Parameter | Beschrijving
--------|-------------
Reden | Optioneel. De gebruiker opgegeven reden voor de uitbreiding van bevoegdheden-aanvraag.
RoleId| Vereist. De unieke id (GUID) van de PAM-rol uitbreiden naar.
RequestedTTL| Vereist. De aangevraagde verlooptijd in seconden.
RequestedTime | Optoinal. De tijd om bevoegdheden te verhogen.  
v | Optioneel. De API-versie. Als er niet is opgenomen, wordt de huidige (meest recent) versie van de API worden gebruikt. Zie voor meer informatie [versiebeheer in de Details van de PAM REST API-Service](privileged-access-management-rest-api-service-details.md#versioning)

**Opmerking**: kunt u de *rechtvaardiging*, *RoleId*, *RequestedTTL*, en *RequestedTime* parameters als eigenschappen in de aanvraag, in plaats van als queryparameters. De *v* parameteer kan alleen worden opgegeven als een queryparameter.

###<a name="request-headers"></a>Aanvraagheaders
Raadpleeg voor algemene aanvraagheaders [HTTP-aanvraag en Reactieheaders](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST API-servicedetails*.
###<a name="request-body"></a>Aanvraagtekst
Optioneel. Zoals eerder vermeld, de *rechtvaardiging*, *RoleId*, *RequestedTTL*, en *RequestedTime* parameters kunnen worden opgegeven als de queryreeks van de eigenschappen van een aanvraagtekst in plaats van ze in de URL opgeven.

##<a name="response"></a>Antwoord
###<a name="response-codes"></a>Reactiecodes
Code  |Beschrijving  
---------|---------
200 | OK
401 | Niet geautoriseerd
403 | Is niet toegestaan
408 | Time-out aanvraag   
500 | Interne serverfout
503 | Service is niet beschikbaar

###<a name="response-headers"></a>Antwoordheaders
Zie voor veelgebruikte antwoordheaders [HTTP-aanvraag en Reactieheaders](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST API-servicedetails*.
###<a name="response-body"></a>Antwoordtekst
Een geslaagde reactie bevat een PAM-aanvraag-object met de volgende eigenschappen.

Eigenschap | Beschrijving
--------|-------------
Aanvraag-id | De unieke id (GUID) voor de PAM-aanvraag.
Creatorld | De unieke id (GUID) in de MIM-service voor het account dat de aanvraag gemaakt.
Reden | De reden voor uitbreiding van bevoegdheden.
CreationTime | De aanmaaktijd van de aanvraag.
CreationMethod | De methode die wordt gebruikt voor het maken van de aanvraag.
ExpirationTime | De verlooptijd van de aanvraag.
RoleID| De unieke id (GUID) van de PAM-rol.
RequestedTTL | De aangevraagde verlopen time-out in seconden.
RequestedTime | De aangevraagde tijd voor uitbreiding van bevoegdheden.
RequestStatus | De status van de aanvraag. Mogelijke waarden zijn: 'Verwerken', 'Active', 'Gesloten', 'Gesloten', 'Verlopen', 'PendingApproval', 'PendingMFA' en 'Geweigerd'.

##<a name="example"></a>Voorbeeld

###<a name="request-1"></a>Aanvraag 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###<a name="response-1"></a>Antwoord 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###<a name="request-2"></a>Aanvraag 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###<a name="response-2"></a>Antwoord 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
