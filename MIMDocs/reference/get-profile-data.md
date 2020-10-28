---
title: Profiel gegevens ophalen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 62b4c336122376cd7da836b9ec6c6e09eac24729
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758722"
---
# <a name="get-profile-data"></a>Profiel gegevens ophalen
Hiermee haalt u een lijst met software certificaat profielen voor een gebruiker op. De lijst bevat de mogelijke bewerkingen die kunnen worden uitgevoerd door de huidige gebruiker. Een aanvraag kan vervolgens worden gestart voor elk van de opgegeven bewerkingen.

>[!IMPORTANT]
>De server stelt alleen de pincode in als het profiel sjabloon beleid aangeeft dat het moet worden uitgevoerd. Anders moet de gebruiker de pincode opgeven.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/api/v1.0/requests/{requestid}/profiles

### <a name="url-parameters"></a>URL-parameters

Parameter | Beschrijving
---------|------------
id | De id (GUID) van het profiel dat moet worden geretourneerd.
requestId | De id van de aanvraag voor het retour neren van de profielen voor.

### <a name="query-parameters"></a>Queryparameters

Parameter | Beschrijving
---------|------------
status | Optioneel. Hiermee wordt de status van de profielen aangegeven waarvoor gegevens moeten worden opgehaald. De mogelijke status typen zijn ' actief ', ' goedgekeurd ', geannuleerd, ' voltooid ', ' geweigerd ', ' wordt uitgevoerd ', ' mislukt ', ' geen ' en ' in behandeling '. <br/>Als er geen status is opgegeven, worden alle profielen, ongeacht de status geretourneerd.

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
403 | Verboden
500 | Interne fout

### <a name="response-headers"></a>Antwoordheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene antwoord headers.

### <a name="response-body"></a>Hoofdtekst van de reactie
Als de bewerking is voltooid, wordt een lijst met JSON-geserialiseerde [micro soft. clm. Shared. Profiles. profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) -objecten met de volgende eigenschappen geretourneerd:

Eigenschap | Beschrijving
---------|------------
AssignedUserUuid | De id van de gebruiker aan wie het profiel is toegewezen.
Opmerking | De opmerking die het profiel beschrijft.
Markering | De vlaggen die het profiel beschrijven.
ParentProfileUuid | De id van het oude profiel dat door het profiel is vervangen.
PrimaryProfileUuid | De id van het primaire profiel.
ProfileOperations | De lijst met mogelijke bewerkingen die kunnen worden uitgevoerd door de huidige gebruiker in het profiel.
ProfileTemplateUuid | De id van de profiel sjabloon die het beleid en de instellingen bevat die van toepassing zijn op het profiel.
ProfileTemplateVersion | De versie van de profiel sjabloon op het moment dat het profiel is gemaakt.
Status | De status van het profiel.
UUID | De id van het profiel.


## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het ophalen van de profiel gegevens voor een gebruiker.

### <a name="example-request"></a>Voor beeld: aanvraag

```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```

### <a name="example-response"></a>Voor beeld: antwoord

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

## <a name="see-also"></a>Zie ook

- [De klasse micro soft. clm. Shared. Profiles. profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
