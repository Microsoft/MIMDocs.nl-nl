---
title: Smartcard-gegevens ophalen | Microsoft Docs
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
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a84982971d169b5c9e7bd758cc74d795416975b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-data"></a>Smartcard-gegevens ophalen
Een lijst van een gebruikersprofielen smartcard met een lijst van mogelijke bewerkingen die kunnen worden uitgevoerd door de huidige gebruiker ophalen. Een aanvraag kan vervolgens worden gestart voor een van de opgegeven bewerkingen.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/API/V1.0/smartcards <br/> / CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


###<a name="url-parameters"></a>URL-Parameters
Eigenschap| Beschrijving
---------|--------
smartcarduuid | Optioneel. De smartcard-UUID zoals aangegeven door MIM CM. Dit is het veld 'uuid' in de [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) object.

###<a name="query-parameters"></a>Query-Parameters
Eigenschap| Beschrijving
---------|--------
kaart-id | Optioneel. De smartcard-UUID zoals aangegeven door MIM CM. Dit is het veld 'uuid' in de [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) object.


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
Indien geslaagd kunt retourneert een JSON-geserialiseerd [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) object met de volgende eigenschappen:

Naam | Beschrijving
-----|-----------
AssignedUserUuid | De id van de gebruiker aan wie de smartcard is toegewezen.
ATR | De smartcard antwoord opnieuw instellen (ATR)-tekenreeks voor de kaart die momenteel wordt geïnitialiseerd.
Opmerking | De reactie die worden beschreven van de smartcard.
Vlaggen | De vlaggen die de smartcard beschrijven.
Middleware | De middleware voor de smartcard.
ParentSmartcardUuid | De id van de oude smartcard die is vervangen door de smartcard.
PermanentSmartcardUuid | De id van de permanente smartcard die is gekoppeld aan de smartcard.
PrimarySmartcardUuid | De id van de primaire smartcard.
ProfileTemplateUuid | De id van de profielsjabloon die bevat de beleidsregels en instellingen die bepalen van de smartcard.
ProfileTemplateVersion | De versie van de profielsjabloon op het moment dat het smartcard-profiel is gemaakt.
SerialNumber | Serienummer van de smartcard.
Status | De status van de smartcard.
UUID | Id van het profiel van de smartcard.

##<a name="example"></a>Voorbeeld

###<a name="request-1"></a>Aanvraag 1
```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1

```
###<a name="response-1"></a>Antwoord 1
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
###<a name="request-2"></a>Aanvraag 2
```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```
###<a name="response-2"></a>Antwoord 2
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
##<a name="see-also"></a>Zie ook

-[Microsoft.Clm.Shared.Smartcards.Smartcard klasse](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
