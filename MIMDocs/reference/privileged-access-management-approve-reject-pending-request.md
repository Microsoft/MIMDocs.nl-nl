---
title: Goedkeuren of afwijzen een PAM-aanvraag in behandeling | Microsoft Docs
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
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3e5506f96a22d1918cff7d0c9b822babb0f6cfa9
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="approve-or-reject-a-pending-pam-request"></a>Goedkeuren of afwijzen een PAM-aanvraag in behandeling
Gebruikt door een bevoegd account om goed te keuren, sluiten of negeren van een aanvraag voor het uitbreiden naar een PAM-rol.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `http://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
POST     |/API/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/API/pamresources/pamrequeststoapprove({approvalId)/Reject

###<a name="url-parameters"></a>URL-Parameters
Parameter | Beschrijving
----------|-----------
approvalId | De id (GUID) van het goedkeuringsobject in PAM als volgt worden opgegeven`guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###<a name="query-parameters"></a>Query-Parameters
Parameter | Beschrijving
----------|--------------
v | Optioneel. De API-versie. Als er niet is opgenomen, wordt de huidige (meest recent) versie van de API worden gebruikt. Zie voor meer informatie [versiebeheer in de Details van de PAM REST API-Service](privileged-access-management-rest-api-service-details.md#versioning)


###<a name="request-headers"></a>Aanvraagheaders
Raadpleeg voor algemene aanvraagheaders [HTTP-aanvraag en Reactieheaders](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST API-servicedetails*.
###<a name="request-body"></a>Aanvraagtekst
Geen.

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
geen
##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1


```
###<a name="response"></a>Antwoord
```
HTTP/1.1 200 OK

```       
