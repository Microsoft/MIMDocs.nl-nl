---
title: Aanvraag maken | Microsoft Docs
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
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d5af8767583cb6999de399de58b321147846291a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="create-request"></a>Aanvraag maken
Hiermee maakt u een MIM CM-aanvraag.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
POST     |/CertificateManagement/API/V1.0/Requests

###<a name="url-parameters"></a>URL-Parameters
geen

###<a name="request-headers"></a>Aanvraagheaders
Raadpleeg voor algemene aanvraagheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="request-body"></a>Aanvraagtekst
De aanvraagtekst bevat de volgende eigenschappen.

Eigenschap | Beschrijving
---------|-----------
profiletemplateuuid | Vereist. De GUID van de profielsjabloon die de gebruiker de aanvraag voor maakt.
datacollection | Vereist. Een verzameling naam / waarde-paren die de gegevens die worden opgegeven door de persoon die zich inschrijft vertegenwoordigt. De verzameling van gegevens die nodig zijn die moet worden geleverd, kan worden opgehaald uit de profielsjabloon werkstroom beleid. Een lege verzameling kan worden opgegeven.
doel | Optioneel. De GUID van de doelgebruiker die de aanvraag moet worden gemaakt voor. Als niet wordt opgegeven, wordt dit standaard naar de huidige gebruiker.
Type | Vereist. Het type van de aanvraag die wordt gemaakt. Beschikbare aanvraagtypen zijn: 'Inschrijven', 'Dubbele', 'OfflineUnblock', 'OnlineUpdate', 'Vernieuwen', 'Herstellen', 'RecoverOnBehalf', 'Herstellen', 'Buiten gebruik stellen', 'Intrekken', 'TemporaryCards' en 'Deblokkeren'.<br/>**Opmerking**: niet alle soorten aanvraag worden ondersteund op alle profielsjablonen. U kunt een bewerking van de blokkering voor een Profielsjabloon voor software bijvoorbeeld kan niet opgeven.
Opmerking | Vereist. Eventuele opmerkingen die kunnen worden ingevoerd door de gebruiker. Het beleid van de werkstroom wordt gedefinieerd of dit nodig is. Een lege tekenreeks kan worden opgegeven.
Prioriteit | Optioneel. De prioriteit van de aanvraag. Als niet wordt opgegeven, wordt de aanvraag standaardprioriteit, zoals wordt bepaald door de instellingen van de profielsjabloon, gebruikt.


##<a name="response"></a>Antwoord
###<a name="response-codes"></a>Reactiecodes
Code  |Beschrijving  
---------|---------
201     | Gemaakt
403 | Is niet toegestaan
500 | Interne fout

###<a name="response-headers"></a>Antwoordheaders
Zie voor veelgebruikte antwoordheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="response-body"></a>Antwoordtekst
Retourneert de URI voor de zojuist gemaakte aanvraag geslaagd kunt.
##<a name="example"></a>Voorbeeld

###<a name="request-1"></a>Aanvraag 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###<a name="response-1"></a>Antwoord 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###<a name="request-2"></a>Aanvraag 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###<a name="response-2"></a>Antwoord 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###<a name="request-3"></a>Aanvraag 3
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
##<a name="see-also"></a>Zie ook

- [Microsoft.Clm.Provision.RequestOperations.InitiateEnroll methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRecover methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRetire methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateUnblock methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
