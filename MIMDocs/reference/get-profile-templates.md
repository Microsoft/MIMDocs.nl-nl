---
title: Profiel sjablonen ophalen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9f14853b3895dece1098270d35439d6c4999be2b
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758710"
---
# <a name="get-profile-templates"></a>Profiel sjablonen ophalen
Hiermee wordt een lijst met profiel sjablonen opgehaald waarvoor de opgegeven gebruiker zich kan inschrijven. Deze methode retourneert een beperkte weer gave van de profiel sjabloon. De geretourneerde profiel sjabloon gegevens moeten voldoende zijn om ervoor te zorgen dat de gebruiker die vraagt om te bepalen welke profiel sjabloon eventueel moet worden inge schreven voor. Als er geen werk stroom en machtiging zijn opgegeven, worden alle Profiel sjablonen die zichtbaar zijn voor de gebruiker geretourneerd.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates? \[ target User\] 

### <a name="url-parameters"></a>URL-parameters

Parameter| Beschrijving
--------|-------------
target User| Optioneel. Hiermee geeft u de doel gebruiker voor het retour neren van profiel sjablonen voor. Als u niets opgeeft, wordt de identiteit van de huidige gebruiker gebruikt. <br/><br/>**Opmerking** : op dit moment wordt alleen de huidige gebruiker ondersteund.

### <a name="request-headers"></a>Aanvraagheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene aanvraag headers.

### <a name="request-body"></a>Aanvraagbody
Geen.

## <a name="response"></a>Antwoord
In deze sectie wordt het antwoord beschreven.

### <a name="response-codes"></a>Antwoord codes

Code  |Beschrijving  
---------|---------
200 | OK
204 | Geen inhoud
500 | Interne fout

### <a name="response-headers"></a>Antwoordheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene antwoord headers.

### <a name="response-body"></a>Hoofdtekst van de reactie
Als de bewerking is voltooid, wordt een lijst met ProfileTemplateLimitedView-objecten met de volgende eigenschappen geretourneerd:

Eigenschap| Type| Beschrijving
--------|-----|--------
Naam| tekenreeks| De weergave naam van de profiel sjabloon.
Description| tekenreeks| De beschrijving voor de profiel sjabloon.
UUID| Guid| De id voor de profiel sjabloon.
IsSmartcardProfileTemplate| booleaans| Hiermee wordt aangegeven of de sjabloon een profiel sjabloon voor een Smart Card is.
IsVirtualSmartcardProfileTemplate| booleaans| Hiermee geeft u op of de profiel sjabloon een virtuele Smart Card vereist.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het ophalen van de lijst met profiel sjablonen voor de opgegeven gebruiker.

### <a name="example-request"></a>Voor beeld: aanvraag

```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```

### <a name="example-response"></a>Voor beeld: antwoord

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
