---
title: Get-verzoek | Microsoft Docs
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
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 56bb25f7282879943dc624f3c24b836c7ae4a58c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-request"></a>Get-verzoek
Hiermee haalt u een of meer opgegeven MIM CM-aanvragen.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/ CertificateManagement/api/v1.0/requests/{id}

###<a name="url-parameters"></a>URL-Parameters
Eigenschap| Beschrijving
---------|--------
ID| Optioneel. De GUID van een aanvraag om op te halen.
###<a name="query-parameters"></a>Query-Parameters
Eigenschap| Beschrijving
---------|--------
doelgebruiker| Optioneel. De doelgebruiker van de aanvraag. Als geen doel is opgegeven, worden alle aanvragen, ongeacht de doelgebruiker wordt geretourneerd. <br/> **Opmerking**: alleen 'huidige' wordt momenteel ondersteund.
status| Optioneel. Geeft de status van de aanvraag om op te halen. Mogelijke statussen zijn: *goedgekeurd*, *geannuleerd*, *voltooid*, *geweigerd*, *Executing*, *mislukt*, *geen*, en *in behandeling*. <br/>Als geen status wordt opgegeven, worden alle aanvragen, ongeacht de status wordt geretourneerd.

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
Indien geslaagd kunt retourneert een of meer [aanvragen](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx) objecten met de volgende eigenschappen:

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

###<a name="request-1"></a>Aanvraag 1
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1

```
###<a name="response-1"></a>Antwoord 1
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
}```       
###Request 2
```
/Certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1 ophalen
```
###Response 2
```
HTTP/1.1 200 OK

{'Uuid': '0c96d73f-967b-420e-854a-43ad2a1504bc', 'RequestType': 5, 'Status': 3, 'vlaggen': 2, 'DataCollection': [{'Name': 'Voorbeeldgegevensitem', 'Waarde': null}], 'DataCollectionFlags': 32, 'OriginatorUserUuid': '8f1590dc-d932-4b66-8e68-2e91c5880780', 'TargetUserUuid': '8f1590dc-d932-4b66-8e68-2e91c5880780', 'verzonden': ' 2015-07-07T23:40:33.133Z ', 'Voltooid': ' 0001-01-01T00:00:00 ', 'NewProfileUuid': 'b99ff38c-6653-471f-ae3c-325bb351a6bc', 'OldProfileUuid': 'b99ff38c-6653-471f-ae3c-325bb351a6bc', 'NewSmartcardUuid': '17cf063d-e337-4aa9-a822-346554ddd3c9', 'OldSmartcardUuid': '17cf063d-e337-4aa9-a822-346554ddd3c9', 'Prioriteit': 0, "Comment": "', 'ProfileTemplateUuid': 'a039b4d0-5ce8-4eff-8651-181c6576fda3', 'SecurityDescriptor': ' O:S-1-5-21-2127521184-1604012920-1887927527-14134865 D:(D;; KREDIETBRIEF;; S-1-5-21-2127521184-1604012920-1887927527-14995557) (A; SWRC;; S-1-5-21-2127521184-1604012920-1887927527-5094533) (A; RC;; WORD) (A; CCDCLCSWSDRC;; S-1-5-21-2127521184-1604012920-1887927527-14134865) (A; D CSW;; S-1-5-21-2127521184-1604012920-1887927527-14995557) (A; CCDCSW;; S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000 ', 'IsSmartcard': true, 'IsEnrollmentAgent': false, 'IsDataCollectionComplete': false}
```       

##See Also

- [Microsoft.Clm.Provision.FindOperations.FindRequest Method](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests.aspx)
- [Microsoft.Clm.Shared.RequestPermission Enumeration](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requestpermission.aspx)
- [Microsoft.Clm.Shared.Requests.Request Class](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)
