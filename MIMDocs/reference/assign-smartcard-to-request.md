---
title: Smartcard toewijzen aan een aanvraag | Microsoft Docs
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
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2886186ade7058b034565501e200ab3773b0af31
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="assign-smart-card-to-a-request"></a>Smartcard toewijzen aan een aanvraag
De opgegeven smartcard koppelt aan de opgegeven aanvraag. Eenmaal gebonden, kan de aanvraag alleen worden uitgevoerd met deze kaart.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
POST     |/CertificateManagement/API/V1.0/smartcards

###<a name="url-parameters"></a>URL-Parameters
Geen.
###<a name="request-headers"></a>Aanvraagheaders
Raadpleeg voor algemene aanvraagheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="request-body"></a>Aanvraagtekst
De aanvraagtekst bevat de volgende eigenschappen.

Eigenschap | Beschrijving
---------|-----------
aanvraag-id | De ID van de aanvraag die de smartcard moet worden gebonden aan.
kaart-id | De kaart-id van de smartcard.
ATR | De smartcard tekenreeks (ATR) antwoord opnieuw instellen.


##<a name="response"></a>Antwoord
###<a name="response-codes"></a>Reactiecodes
Code  |Beschrijving  
---------|---------
201     | Gemaakt
204 | Er is geen inhoud
403 | Is niet toegestaan
500 | Interne fout

###<a name="response-headers"></a>Antwoordheaders
Zie voor veelgebruikte antwoordheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="response-body"></a>Antwoordtekst
Indien geslaagd kunt retourneert een URI naar de zojuist gemaakte smartcard-object.
##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###<a name="response"></a>Antwoord
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##<a name="see-also"></a>Zie ook

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard methode (String, String, aanvraag)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
