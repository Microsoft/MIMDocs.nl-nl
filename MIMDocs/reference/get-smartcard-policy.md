---
title: Smartcard beleid ophalen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e8e8b2ac7dcf8f72d4989d458272d78b8367d923
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758806"
---
# <a name="get-smart-card-policy"></a>Smartcard beleid ophalen
Hiermee wordt het profiel sjabloon beleid voor de opgegeven werk stroom opgehaald. Deze gegevens worden gebruikt tijdens het maken van de aanvraag. Het werk stroom beleid geeft aan welke gegevens nodig zijn door de client om een aanvraag te maken. De gegevens kunnen diverse items voor gegevens verzameling, aanvraag opmerkingen en een eenmalig wachtwoord beleid bevatten.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type}

### <a name="url-parameters"></a>URL-parameters

Parameter| Beschrijving
--------|-------------
id| Vereist. De GUID die overeenkomt met de profiel sjabloon waaruit het beleid moet worden opgehaald.
type| Vereist. Het type beleid dat wordt aangevraagd. De mogelijke waarden zijn ' Inschrijven ', ' dupliceren ', ' OfflineUnblock ', ' OnlineUpdate ', ' vernieuwen ', ' herstellen ', ' RecoverOnBehalf ', ' opnieuw invoeren ', ' buiten gebruik stellen ', ' intrekken ', ' TemporaryEnroll ' en ' blok kering '.

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
403 | Verboden
204 | Geen inhoud
500 | Interne fout

### <a name="response-headers"></a>Antwoordheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene antwoord headers.

### <a name="response-body"></a>Hoofdtekst van de reactie
Wanneer dit is voltooid, wordt een beleids object geretourneerd op basis van een [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) -object. Het object Policy bevat mini maal de eigenschappen in de volgende tabel, maar kan extra eigenschappen bevatten, afhankelijk van het aangevraagde beleid. Bijvoorbeeld een aanvraag voor een inschrijvings beleid retourneert een [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy) -object. Zie de documentatie voor het beleids object dat is gekoppeld aan de para meter {type} in de aanvraag voor meer informatie. De documentatie voor de verschillende soorten beleids objecten vindt u in de documentatie van de [naam ruimte micro soft. clm. Shared. ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates) .

Eigenschap | Beschrijving
---------|------------
ApprovalsNeeded | Het aantal goed keuringen dat is vereist voor Forefront Identity Manager (FIM)-aanvragen voor het beleid.
AuthorizedApprover | De security descriptor voor gebruikers die zijn geautoriseerd om FIM CM-aanvragen voor het beleid goed te keuren.
AuthorizedEnrollmentAgent | De security descriptor voor gebruikers die als inschrijvings agenten voor het beleid kunnen fungeren.
AuthorizedInitiator | De security descriptor voor gebruikers die FIM CM-aanvragen voor het beleid kunnen initiëren.
CollectComments | Een Booleaanse waarde die aangeeft of het verzamelen van opmerkingen is ingeschakeld voor FIM CM-aanvragen voor het beleid.
CollectRequestPriority | Een Booleaanse waarde die aangeeft of het verzamelen van aanvraag prioriteiten is ingeschakeld voor FIM CM-aanvragen voor het beleid.
DefaultRequestPriority | De standaard prioriteit voor FIM CM-aanvragen voor het beleid.
Documenten | De beleids documenten die voor het beleid zijn geconfigureerd.
Ingeschakeld | Een Booleaanse waarde die aangeeft of het beleid is ingeschakeld.
EnrollAgentRequired | Een Booleaanse waarde die aangeeft of inschrijvings agenten vereist zijn voor FIM CM-aanvragen voor het beleid.
OneTimePasswordPolicy | De distributie methode voor eenmalige wacht woorden voor FIM CM-aanvragen voor het beleid.
Personalisatie | De opties voor personalisatie van Smart Cards voor het beleid.
PolicyDataCollection | De gegevensverzamelings items die aan het beleid zijn gekoppeld.
SelfServiceEnabled | Een Booleaanse waarde die aangeeft of de self-service initiatie van FIM CM-aanvragen is ingeschakeld voor het beleid.

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het profiel sjabloon beleid voor een werk stroom. 

### <a name="example-request-1"></a>Voor beeld: aanvraag 1

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```

### <a name="example-response-1"></a>Voor beeld: antwoord 1

```
HTTP/1.1 200 OK

... body coming soon
```       

### <a name="example-request-2"></a>Voor beeld: aanvraag 2

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```

### <a name="example-response-2"></a>Voor beeld: respons 2

```
HTTP/1.1 200 OK

... body coming soon
```       

## <a name="see-also"></a>Zie ook

- [De klasse micro soft. clm. Shared. ProfileTemplates. ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Naam ruimte micro soft. clm. Shared. ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
