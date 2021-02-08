---
title: Get-aanvraag | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1bc42eb0fb1e54a3425586350ae5ad20495534c5
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835702"
---
# <a name="get-request"></a>Aanvraag ophalen
Hiermee haalt u een of meer opgegeven Microsoft Identity Manager (MIM) Certificate Management (CM)-aanvragen.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{id}

### <a name="url-parameters"></a>URL-parameters

Eigenschap| Beschrijving
---------|--------
id| Optioneel. De GUID van een aanvraag die moet worden opgehaald.

### <a name="query-parameters"></a>Queryparameters

Eigenschap| Beschrijving
---------|--------
target User| Optioneel. De doel gebruiker van de aanvraag. Als er geen doel is opgegeven, worden alle aanvragen, ongeacht de doel gebruiker, weer gegeven. <br/><br/>**Opmerking**: alleen de waarde huidig wordt momenteel ondersteund.
status| Optioneel. Hiermee wordt de status van de aanvraag aangegeven die moet worden opgehaald. De mogelijke status typen zijn ' goedgekeurd ', geannuleerd, ' voltooid ', ' geweigerd ', ' wordt uitgevoerd ', ' mislukt ', ' geen ' en ' in behandeling '. <br/>Als er geen status is opgegeven, worden alle aanvragen, ongeacht de status, worden geretourneerd.

### <a name="request-headers"></a>Aanvraagheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene aanvraag headers.

### <a name="request-body"></a>Aanvraagbody
Geen.

## <a name="response"></a>Antwoord
In deze sectie wordt het antwoord beschreven.

### <a name="response-codes"></a>Antwoord codes

Code  |Description  
---------|---------
200 | OK
204 | Geen inhoud
403 | Verboden
500 | Interne fout

### <a name="response-headers"></a>Antwoordheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene antwoord headers.

### <a name="response-body"></a>Hoofdtekst van de reactie
Als de bewerking is voltooid, worden een of meer [aanvraag](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx) objecten met de volgende eigenschappen geretourneerd:

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
In deze sectie vindt u een voor beeld van het ophalen van de opgegeven MIM CM-aanvragen.

### <a name="example-request-1"></a>Voor beeld: aanvraag 1

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1
```

### <a name="example-response-1"></a>Voor beeld: antwoord 1

```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":3,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"00000000-0000-0000-0000-000000000000",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"00000000-0000-0000-0000-000000000000",
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

### <a name="example-request-2"></a>Voor beeld: aanvraag 2

```
GET /certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1
```

### <a name="example-response-2"></a>Voor beeld: respons 2

```
HTTP/1.1 200 OK

{
    "Uuid":"0c96d73f-967b-420e-854a-43ad2a1504bc",
    "RequestType":5,
    "Status":3,
    "Flags":2,
    "DataCollection":[
        {
            "Name":"Sample Data Item",
            "Value":null
        }
    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:40:33.133Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```     

## <a name="see-also"></a>Zie ook

- [Methode micro soft. clm. Provision. FindOperations. FindRequest](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests.aspx)
- [Inventarisatie van micro soft. clm. Shared. RequestPermission](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requestpermission.aspx)
- [De klasse micro soft. clm. Shared. aanvragen. Request](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)
