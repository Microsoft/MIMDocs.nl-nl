---
title: Aanvraag maken | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 51016951f9dec22e2a40b44a2b76a840192b518f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758833"
---
# <a name="create-request"></a>Aanvraag maken
Maak een aanvraag voor certificaat beheer (CM) voor Microsoft Identity Manager (MIM).

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

### <a name="url-parameters"></a>URL-parameters
Geen.

### <a name="request-headers"></a>Aanvraagheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene aanvraag headers.

### <a name="request-body"></a>Aanvraagbody
De aanvraag tekst bevat de volgende eigenschappen:

Eigenschap | Beschrijving
---------|-----------
profiletemplateuuid | Vereist. De GUID van de profiel sjabloon waarvoor de gebruiker de aanvraag maakt.
datacollection | Vereist. Een verzameling naam/waarde-paren die de gegevens vertegenwoordigt die door de inschrijving moeten worden verschaft. De verzameling benodigde gegevens die moet worden gegeven, kan worden opgehaald uit het werk stroom beleid van de profiel sjabloon. U kunt een lege verzameling opgeven.
stemming | Optioneel. De GUID van de doel gebruiker voor wie de aanvraag moet worden gemaakt. Als u niets opgeeft, wordt het doel standaard ingesteld op de huidige gebruiker.
type | Vereist. Het type aanvraag dat wordt gemaakt. Beschik bare aanvraag typen zijn ' Inschrijven ', ' duplicaat ', ' OfflineUnblock ', ' OnlineUpdate ', ' vernieuwen ', ' herstellen ', ' RecoverOnBehalf ', ' opnieuw invoeren ', ' buiten gebruik stellen ', ' intrekken ', ' TemporaryCards ' en ' blok kering '.<br/><br/>**Opmerking** : niet alle aanvraag typen worden op alle Profiel sjablonen ondersteund. U kunt bijvoorbeeld de deblokkerings bewerking niet opgeven voor een software profiel sjabloon.
comment | Vereist. Eventuele opmerkingen die door de gebruiker kunnen worden ingevoerd. Het werk stroom beleid definieert of de opmerkings eigenschap nodig is. Er kan een lege teken reeks worden opgegeven.
priority | Optioneel. De prioriteit van de aanvraag. Als dat niet is opgegeven, wordt de standaard aanvraag prioriteit gebruikt, zoals wordt bepaald door de profiel sjabloon instellingen.


## <a name="response"></a>Antwoord
In deze sectie wordt het antwoord beschreven.

### <a name="response-codes"></a>Antwoord codes

Code  |Beschrijving  
---------|---------
201 | Gemaakt
403 | Verboden
500 | Interne fout

### <a name="response-headers"></a>Antwoordheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene antwoord headers.

### <a name="response-body"></a>Hoofdtekst van de reactie
Als de bewerking is voltooid, wordt de URI voor de zojuist gemaakte aanvraag geretourneerd.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het maken van aanvragen voor inschrijven en blok kering opheffen.

### <a name="example-request-1"></a>Voor beeld: aanvraag 1

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```

### <a name="example-response-1"></a>Voor beeld: antwoord 1

```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```

### <a name="example-request-2"></a>Voor beeld: aanvraag 2

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```

### <a name="example-response-2"></a>Voor beeld: respons 2

```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

### <a name="example-request-3"></a>Voor beeld: aanvraag 3

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```

## <a name="see-also"></a>Zie ook

- [Microsoft.Clm.Provision.RequestOperations.Inimethode tiateEnroll](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Inimethode tiateOfflineUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Inimethode tiateRecover](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Inimethode tiateRetire](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Inimethode tiateUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
