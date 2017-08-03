---
title: Ophalen van profielgegevens | Microsoft Docs
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
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 068497509b07b879fcadde6b60a9530d3d8db093
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-data"></a>Profielgegevens ophalen
Een lijst met de softwarematige certificaatprofielen van een gebruiker met een lijst van mogelijke bewerkingen die kunnen worden uitgevoerd door de huidige gebruiker ophalen. Een aanvraag kan vervolgens worden gestart voor een van de opgegeven bewerkingen.

**Opmerking**: de server wordt de PINCODE alleen ingesteld als het beleid van de sjabloon profiel geeft aan dat moet worden gedaan. Anders moet de gebruiker deze opgeven.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/API/V1.0/Profiles<br/>/ CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/API/V1.0/Requests/{RequestId}/Profiles

###<a name="url-parameters"></a>URL-Parameters
Parameter | Beschrijving
---------|------------
ID | De id (GUID) van het profiel te retourneren.
aanvraag-id | De id van de aanvraag voor het retourneren van de profielen voor.

###<a name="query-parameters"></a>Query-Parameters
Parameter | Beschrijving
---------|------------
status | Optioneel. Geeft de status van de profielen voor het ophalen van gegevens voor. Mogelijke statussen zijn: 'Active', 'Goedgekeurd', 'Geannuleerd', 'Voltooid', 'Geweigerd', 'Uitvoeren', 'Mislukt', 'None' en 'In behandeling'. <br/>Als geen status wordt opgegeven, worden alle profielen, ongeacht de status wordt geretourneerd.
###<a name="request-headers"></a>Aanvraagheaders
Raadpleeg voor algemene aanvraagheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="request-body"></a>Aanvraagtekst
geen

##<a name="response"></a>Antwoord
###<a name="response-codes"></a>Reactiecodes
Code  |Beschrijving  
---------|---------
200 | OK
204 | Er is geen inhoud
403 | Is niet toegestaan
500 | Interne fout

###<a name="response-headers"></a>Antwoordheaders
Zie voor veelgebruikte antwoordheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="response-body"></a>Antwoordtekst
Indien geslaagd kunt retourneert een lijst met JSON-geserialiseerd [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) objecten met de volgende eigenschappen:

Eigenschap | Beschrijving
---------|------------
AssignedUserUuid | De id van de gebruiker aan wie het profiel is toegewezen.
Opmerking | De reactie die worden beschreven van het profiel.
Vlaggen | De vlaggen die beschrijven van het profiel.
ParentProfileUuid | De id van het oude profiel dat het profiel is vervangen.
PrimaryProfileUuid | De id van het primaire profiel.
ProfileOperations | De lijst met mogelijke bewerkingen die kunnen worden uitgevoerd door de huidige gebruiker op het profiel.
ProfileTemplateUuid | De id van de profielsjabloon die bevat de beleidsregels en instellingen die bepalen van het profiel.
ProfileTemplateVersion | De versie van de profielsjabloon op het moment dat het profiel is gemaakt.
Status | De status van het profiel.
UUID | Id van het profiel.


##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###<a name="response"></a>Antwoord
```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```       
##<a name="see-also"></a>Zie ook

- [Microsoft.Clm.Shared.Profiles.Profile klasse](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
