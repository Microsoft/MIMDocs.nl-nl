---
title: Smartcard-beleid | Microsoft Docs
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1164795e81ff0ef2f93086144b721e760f8dd028
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-policy"></a>Smartcard-beleid niet ophalen

Hiermee haalt u het beleid van de sjabloon profiel voor de opgegeven workflow. Deze gegevens worden gebruikt tijdens het maken van de aanvraag. De werkstroom-beleid bepaalt welke gegevens door de client nodig is om te kunnen maken van een aanvraag. Deze gegevens kan bevatten: verschillende gegevensverzamelingsitems, aanvraag-opmerkingen en één keer wachtwoordbeleid.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/ CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type}

###<a name="url-parameters"></a>URL-Parameters
Parameter| Beschrijving
--------|-------------
ID| Vereist. De GUID die overeenkomt met de profielsjabloon die het beleid moet worden opgehaald uit.
Type| Vereist. Het type van het beleid wordt aangevraagd. Mogelijke waarden zijn: *inschrijven*, *dubbele*, *OfflineUnblock*, *OnlineUpdate*, *vernieuwen*, *herstellen*, *RecoverOnBehalf*, *reactiveren*, *buiten gebruik stellen*, *intrekken*, *TemporaryEnroll*, *blokkering*.

###<a name="request-headers"></a>Aanvraagheaders
Raadpleeg voor algemene aanvraagheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="request-body"></a>Aanvraagtekst
geen

##<a name="response"></a>Antwoord
###<a name="response-codes"></a>Reactiecodes
Code  |Beschrijving  
---------|---------
200     | OK
403 | Is niet toegestaan
204 | Er is geen inhoud
500 | Interne fout

###<a name="response-headers"></a>Antwoordheaders
Zie voor veelgebruikte antwoordheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="response-body"></a>Antwoordtekst
Indien geslaagd kunt resulteert in een beleidsobject op basis van een [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) object. Ten minste de beleidsobject bevat de eigenschappen in de volgende tabel, maar mogelijk aanvullende eigenschappen, afhankelijk van het beleid dat is aangevraagd. Bijvoorbeeld, een aanvraag voor een beleid voor inschrijven retourneert een [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy) object. Zie voor meer informatie de documentatie voor de beleidsobject dat is gekoppeld met de parameter {type} in de aanvraag. De documentatie voor de verschillende soorten beleidsobjecten kan worden gevonden in de [Microsoft.Clm.Shared.ProfileTemplates Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates) documentatie.

Eigenschap | Beschrijving
---------|------------
ApprovalsNeeded | Het aantal goedkeuringen die vereist voor FIM CM-aanvragen voor het beleid zijn.
AuthorizedApprover | De security descriptor voor gebruikers die zijn gemachtigd voor het goedkeuren van FIM CM-aanvragen voor het beleid.
AuthorizedEnrollmentAgent | De security descriptor voor gebruikers die als inschrijvingsagenten voor het beleid fungeren kan.
AuthorizedInitiator | De security descriptor voor gebruikers die FIM CM-aanvragen voor het beleid kunnen initiëren.
CollectComments | Een Boolean die aangeeft of de verzameling Opmerking voor FIM CM-aanvragen voor het beleid is ingeschakeld.
CollectRequestPriority | Een Boolean die aangeeft of de verzameling van de prioriteit aanvragen voor FIM CM-aanvragen voor het beleid is ingeschakeld.
DefaultRequestPriority | De standaardprioriteit voor FIM CM-aanvragen voor het beleid.
Documenten | De beleidsdocumenten die zijn geconfigureerd voor het beleid.
Ingeschakeld | Een Boolean die aangeeft of het beleid is ingeschakeld.
EnrollAgentRequired | Een Boolean die aangeeft of inschrijvingsagenten vereist voor FIM CM-aanvragen voor het beleid zijn.
OneTimePasswordPolicy | Hiermee haalt u hoe eenmalige wachtwoorden voor FIM CM-aanvragen voor het beleid worden gedistribueerd.
Persoonlijke instellingen | De smartcard personalisatie opties voor het beleid.
PolicyDataCollection | De gegevens verzamelingsitems die gekoppeld aan het beleid zijn.
SelfServiceEnabled | Een Boolean die aangeeft of inleiding selfservice van FIM CM-aanvragen voor het beleid is ingeschakeld.

##<a name="example"></a>Voorbeeld

###<a name="request-1"></a>Aanvraag 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###<a name="response-1"></a>Antwoord 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###<a name="request-2"></a>Aanvraag 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###<a name="response-2"></a>Antwoord 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##<a name="see-also"></a>Zie ook

- [Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy klasse](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Microsoft.Clm.Shared.ProfileTemplates Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
