---
title: Ophalen van Profielsjablonen | Microsoft Docs
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
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8736b328926445f6434bd037a1ecf2e5de9ee466
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-templates"></a>Profielsjablonen ophalen
Hiermee wordt een lijst met profielsjablonen die de opgegeven gebruiker kan inschrijven voor opgehaald. Deze methode retourneert een beperkte weergave van de profielsjabloon. De sjabloon profielgegevens geretourneerd moet voldoende zijn voor het inschakelen van de aanvragende gebruiker om te bepalen welke Profielsjabloon, indien aanwezig, dat ze nodig hebben om in te schrijven voor. Als er geen werkstroom en de machtiging worden opgegeven, worden alle profielsjablonen zichtbaar is voor de gebruiker wordt geretourneerd.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/ CertificateManagement/api/v1.0/profiletemplates? \[doelgebruiker\] 

###<a name="url-parameters"></a>URL-Parameters
Parameter| Beschrijving
--------|-------------
doelgebruiker| Optioneel. Hiermee geeft u de doelgebruiker om terug te keren profielsjablonen voor. Als u niet opgeeft, wordt de identiteit van de huidige gebruiker gebruikt. <br/>**Opmerking**: alleen de huidige gebruiker wordt momenteel ondersteund.

###<a name="request-headers"></a>Aanvraagheaders
Zie het onderwerp van de service voor algemene aanvraagheaders
###<a name="request-body"></a>Aanvraagtekst
geen

##<a name="response"></a>Antwoord
###<a name="response-codes"></a>Reactiecodes
Code  |Beschrijving  
---------|---------
200     | OK
204 | Er is geen inhoud
500 | Interne fout

###<a name="response-headers"></a>Antwoordheaders
Zie het onderwerp van de service voor algemene antwoordheaders.
###<a name="response-body"></a>Antwoordtekst
Retourneert een lijst met objecten met de volgende eigenschappen ProfileTemplateLimitedView geslaagd kunt.

Eigenschap| Type| Beschrijving
--------|-----|--------
Naam| string| De weergavenaam van de profielsjabloon.
Beschrijving| string| De beschrijving voor de profielsjabloon.
UUID| GUID| De id voor de profielsjabloon.
IsSmartcardProfileTemplate| BOOL| Hiermee wordt aangegeven of de sjabloon een Profielsjabloon van de smartcard.
IsVirtualSmartcardProfileTemplate| BOOL| Geeft aan of de profielsjabloon een virtuele smartcard vereist.

##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###<a name="response"></a>Antwoord
```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]

```       
