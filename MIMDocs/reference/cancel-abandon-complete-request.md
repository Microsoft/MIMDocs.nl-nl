---
title: Een aanvraag annuleren, afbreken of volt ooien | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2016
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e971309963968ddd52ef44644fb9319738df6242
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758743"
---
# <a name="cancel-abandon-or-complete-a-request"></a>Een aanvraag annuleren, afbreken of volt ooien
Een Microsoft Identity Manager (MIM)-aanvraag voor certificaat beheer (CN) markeren als voltooid, geannuleerd of afgebroken.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
PUT     |/CertificateManagement/api/v1.0/requests/{id}

### <a name="url-parameters"></a>URL-parameters

Eigenschap| Beschrijving
---------|--------
id| Vereist. De GUID van de aanvraag die moet worden voltooid.


### <a name="request-headers"></a>Aanvraagheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene aanvraag headers.

### <a name="request-body"></a>Aanvraagbody
De aanvraag tekst bevat de volgende eigenschappen:

Eigenschap | Beschrijving
---------|-----------
status | De status om de aanvraag in te stellen op. De mogelijke waarden zijn ' voltooid ', ' geannuleerd ' of ' verlaten '.


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
Als de bewerking is geslaagd, wordt het object [micro soft. clm. Shared. aanvragen. Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) geretourneerd. Het object beschrijft de MIM CM aanvraag die is gemarkeerd als voltooid. Het object bevat de volgende eigenschappen:

Naam | Beschrijving
-----|------------
Opmerking | De opmerking die aan de MIM CM aanvraag is gekoppeld.
Voltooid | Het tijdstip waarop de MIM CM aanvraag is voltooid.
DataCollection | De gegevensverzamelings items die zijn gekoppeld aan de MIM CM aanvraag.
DataCollectionFlags | De opties voor het verzamelen van gegevens voor de MIM CM aanvraag.
Markering | De opties die zijn gekoppeld aan de MIM CM aanvraag.
IsDataCollectionComplete | Een Booleaanse waarde die aangeeft of de gegevens verzameling is voltooid voor de MIM CM aanvraag.
IsEnrollmentAgent | Een Booleaanse waarde die aangeeft of een inschrijvings agent vereist is om de MIM CM aanvraag uit te voeren.
IsSmartcard | Een Booleaanse waarde die aangeeft of de MIM CM aanvraag een aanvraag voor een Smart Card of een software profiel is.
NewProfileUuid | De identiteit van het nieuwe software profiel dat door de MIM CM aanvraag wordt geproduceerd.
NewSmartcardUuid | De identiteit van de nieuwe Smart Card die is toegewezen door de MIM CM aanvraag.
OldProfileUuid | De identiteit van het software profiel waarvoor de MIM CM aanvraag is gemaakt.
OldSmartcardUuid | De identiteit van de Smart Card waarvoor de MIM CM aanvraag is gemaakt.
OriginatorUserUuid | De identiteit van de gebruiker die de MIM CM aanvraag heeft gestart.
Prioriteit | De prioriteit van de MIM CM aanvraag.
ProfileTemplateUuid | De identiteit van de profiel sjabloon waarvoor de MIM CM aanvraag is gemaakt.
RequestType | Het type van de MIM CM aanvraag.
Security descriptor | De security descriptor voor de MIM CM aanvraag.
Status | De status van de MIM CM aanvraag.
Ingediend | Het tijdstip waarop de MIM CM aanvraag is ingediend.
TargetUserUuid | De identiteit van de doel gebruiker voor de MIM CM aanvraag.
UUID | De id voor de MIM CM aanvraag.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het markeren van een aanvraag als voltooid.

### <a name="example-request"></a>Voor beeld: aanvraag

```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    "status":"Completed"
}
```

### <a name="example-response"></a>Voor beeld: antwoord

```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```

## <a name="see-also"></a>Zie ook

- [Microsoft.Clm.Provision.Exemethode cuteOperations. complete](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [De klasse micro soft. clm. Shared. aanvragen. Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
