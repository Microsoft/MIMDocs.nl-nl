---
title: Annuleren, afbreken of op een aanvraag voltooien | Microsoft Docs
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
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4f81c9d86006c477993b2ac8cc2e845de2c1d2f3
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="cancel-abandon-or-complete-a-request"></a>Annuleren, afbreken of op een aanvraag voltooien
Markeer een MIM CM-aanvraag voltooid, geannuleerd of afgebroken.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
PLAATSEN     |/ CertificateManagement/api/v1.0/requests/{id}

###<a name="url-parameters"></a>URL-Parameters
Eigenschap| Beschrijving
---------|--------
ID| Vereist. De GUID van de aanvraag te voltooien.


###<a name="request-headers"></a>Aanvraagheaders
Raadpleeg voor algemene aanvraagheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="request-body"></a>Aanvraagtekst
De aanvraagtekst bevat de volgende eigenschappen.

Eigenschap | Beschrijving
---------|-----------
status | De status in te stellen van de aanvraag op. Mogelijke waarden zijn: 'Voltooid', 'Geannuleerd' of 'Abandoned'.


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
Indien geslaagd kunt retourneert een [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) object met de volgende eigenschappen die worden beschreven van de MIM CM-aanvraag die is gemarkeerd als voltooid:

Naam | Beschrijving
-----|------------
Opmerking | De reactie die is gekoppeld aan de MIM CM-aanvraag.
Voltooid | De tijd waarop de MIM CM-aanvraag is voltooid.
DataCollection | De gegevens verzamelingsitems die gekoppeld aan de MIM CM-aanvraag zijn.
DataCollectionFlags | De opties voor het verzamelen van gegevens voor de MIM CM-aanvraag.
Vlaggen | De opties die gekoppeld aan de MIM CM-aanvraag zijn.
IsDataCollectionComplete | Een Boolean die aangeeft of het verzamelen van gegevens voor de MIM CM-aanvraag is voltooid.
IsEnrollmentAgent | Een Booleaanse waarde waarmee wordt aangegeven als een Inschrijvingsagent is vereist voor het uitvoeren van de MIM CM-aanvraag.
IsSmartcard | Een Boolean die aangeeft of de MIM CM-aanvraag de aanvraag voor een software-profiel of een smartcard-aanvraag is.
NewProfileUuid | De identiteit van het nieuwe profiel voor software die wordt geproduceerd door de MIM CM-aanvraag.
NewSmartcardUuid | De identiteit van de nieuwe smartcard dat door de MIM CM-aanvraag is toegewezen.
OldProfileUuid | De identiteit van het software-profiel waarvoor de MIM CM-aanvraag is gemaakt.
OldSmartcardUuid | De identiteit van de smartcard waarvoor de MIM CM-aanvraag is gemaakt.
OriginatorUserUuid | De identiteit van de gebruiker die de MIM CM-aanvraag afkomstig is
Priority | Prioriteit van de MIM CM-aanvraag.
ProfileTemplateUuid | De identiteit van de profielsjabloon waarvoor de MIM CM-aanvraag is gemaakt.
RequestType | Het type van de MIM CM-aanvraag.
SecurityDescriptor | De security descriptor voor de MIM CM-aanvraag.
Status | De status van de MIM CM-aanvraag.
Verzonden | De tijd waarop de MIM CM-aanvraag is ingediend.
TargetUserUuid | De identiteit van de doelgebruiker voor de MIM CM-aanvraag.
UUID | De id voor de MIM CM-aanvraag.

##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    “status”:”Completed”
}
```
###<a name="response"></a>Antwoord
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
##<a name="see-also"></a>Zie ook

- [Microsoft.Clm.Provision.ExecuteOperations.Complete methode](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
