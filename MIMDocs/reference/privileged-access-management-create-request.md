---
title: PAM-aanvraag maken | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d58155232b3f99f32df9cb6b4c2e344d0cbad278
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758881"
---
# <a name="create-pam-request"></a>PAM-aanvraag maken
Wordt gebruikt door een privileged account om te verhogen naar een PAM-rol.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
POST     |/api/pamresources/pamrequests

### <a name="query-parameters"></a>Queryparameters

Parameter | Beschrijving
--------|-------------
Reden | Optioneel. De door de gebruiker opgegeven reden voor de aanvraag van de verhoging.
RoleId | Vereist. De unieke id (GUID) van de PAM-rol die moet worden uitgebreid naar.
RequestedTTL | Vereist. De aangevraagde verloop tijd, in seconden.
RequestedTime | Optioneel. De tijd om bevoegdheden uit te breiden.  
v | Optioneel. De API-versie. Als dat niet het geval is, wordt de huidige versie van de API (meest recent vrijgegeven) gebruikt. Zie [versie beheer in PAM rest API service Details](privileged-access-management-rest-api-service-details.md#versioning)voor meer informatie.

>[!NOTE]
>U kunt de para meters **rechtvaardig** , **RoleId** , **RequestedTTL** en **RequestedTime** opgeven als eigenschappen in de hoofd tekst van de aanvraag, in plaats van als query parameters. De para meter **v** kan alleen worden opgegeven als een query parameter.

### <a name="request-headers"></a>Aanvraagheaders
Zie [HTTP-aanvraag-en reactie headers](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *pam-rest API service Details* voor algemene aanvraag headers.

### <a name="request-body"></a>Aanvraagbody
Optioneel. De para meters voor de **reden** , **RoleId** , **RequestedTTL** en **RequestedTime** kunnen worden opgegeven als eigenschappen van een aanvraag tekst in plaats van deze op te geven in de URL-query teken reeks.

## <a name="response"></a>Antwoord
In deze sectie wordt het antwoord beschreven.

### <a name="response-codes"></a>Antwoord codes

Code  |Beschrijving  
---------|---------
200 | OK
401 | Niet geautoriseerd
403 | Verboden
408 | Time-out van aanvraag   
500 | Interne server fout
503 | Service niet beschikbaar

### <a name="response-headers"></a>Antwoordheaders
Zie [HTTP-aanvraag-en reactie headers](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *pam-rest API service Details* voor algemene aanvraag headers.

### <a name="response-body"></a>Hoofdtekst van de reactie
Een geslaagde reactie bevat een PAM-aanvraag object met de volgende eigenschappen:

Eigenschap | Beschrijving
--------|-------------
RequestID | De unieke id (GUID) voor de PAM-aanvraag.
CreatorID | De unieke id (GUID) in de MIM-service voor het account dat de aanvraag heeft gemaakt.
Reden | De reden voor uitbrei ding.
CreationTime | De aanmaak tijd van de aanvraag.
CreationMethod | De methode die wordt gebruikt om de aanvraag te maken.
ExpirationTime | De verval tijd van de aanvraag.
RoleID| De unieke id (GUID) van de PAM-rol.
RequestedTTL | De aangevraagde time-out voor de verloop tijd in seconden.
RequestedTime | De aangevraagde tijd voor uitbrei ding.
RequestStatus | De status van de aanvraag. De mogelijke waarden zijn ' verwerken ', ' actief ', ' gesloten ', ' sluiten ', ' verlopen ', ' PendingApproval ', ' PendingMFA ' en ' rejected '.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u voor beelden van het maken van een PAM-aanvraag.

### <a name="example-request-1"></a>Voor beeld: aanvraag 1

```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```

### <a name="example-response-1"></a>Voor beeld: antwoord 1

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

### <a name="example-request-2"></a>Voor beeld: aanvraag 2

```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```

### <a name="example-response-2"></a>Voor beeld: respons 2

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
