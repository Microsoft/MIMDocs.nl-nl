---
title: Details van CM REST API-Service | Microsoft Docs
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3d724dea8b1536f0155f9fc287d0cd74f5f16b3c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="cm-rest-api-service-details"></a>Details van CM REST-API-Service
De volgende secties worden de details van de REST-API van Microsoft Identity Manager (MIM) Certificate Management (CM).

## <a name="architecture"></a>Architectuur 
MIM CM REST API-aanroepen worden afgehandeld door domeincontrollers. De volgende tabel bevat de volledige lijst met domeincontrollers en voorbeelden van de context waarin ze kunnen worden gebruikt.

Domeincontroller| Voorbeeld route
----------|-------------
CertificateDataController| /API/V1.0/Requests/{RequestId}/certificatedata /
CertificateRequestGenerationOptionsController| /API/V1.0/Requests/{RequestId}/certificaterequestgenerationoptions
CertificatesController| /API/V1.0/smartcards/{smartcardid}/Certificates <br/> /API/V1.0/Profiles/{profileid}/Certificates
OperationsController| /API/V1.0/smartcards/{smartcardid}/operations <br/> /API/V1.0/Profiles/{profileid}/operations
PoliciesController| / api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
ProfilesController| / api/v1.0/profiles/{id} <br/> /API/V1.0/Profiles <br/> / api/v1.0/requests/{requestid}/profiles/{id}
ProfileTemplatesController| / api/v1.0/profiletemplates/{id} <br/> /API/V1.0/profiletemplates <br/> / api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
RequestsController| / api/v1.0/requests/{id} <br/> /API/V1.0/Requests
SmartcardsController| /API/V1.0/Requests/{RequestId}/smartcards/{id}/diversifiedkey <br/> /API/V1.0/Requests/{RequestId}/smartcards/{id}/serverproposedpin <br/> /API/V1.0/Requests/{RequestId}/smartcards/{id}/authenticationresponse <br/> / api/v1.0/requests/{requestid}/smartcards/{id} <br/> / api/v1.0/smartcards/{id} <br/> /API/V1.0/smartcards
SmartcardsConfigurationController| /API/V1.0/profiletemplates/{profiletemplateid}/Configuration/smartcards
<br/>

## <a name="http-request-and-response-headers"></a>HTTP-aanvraag en Reactieheaders

HTTP-aanvragen verzonden naar de API moeten de volgende headers (deze lijst is niet volledig) zijn:

Koptekst | Beschrijving
-------|------------
Autorisatie | Vereist. De inhoud hangen af van de verificatiemethode is configureerbaar en kunnen worden gebaseerd op WIA (Windows geïntegreerde verificatie) of AD FS.
Content-Type | Vereist als de aanvraag een instantie heeft. Moet ' application/json'.
Content-Length | Vereist als de aanvraag een instantie heeft. 
Cookie | De sessiecookie. Mogelijk zijn vereist, afhankelijk van de verificatiemethode.
<br/>
HTTP-antwoorden bevat de volgende headers (deze lijst is niet volledig):

Koptekst | Beschrijving
-------|------------
Content-Type | De API retourneert altijd ' application/json'.
Content-Length | De lengte van de aanvraagtekst, indien aanwezig, in bytes.


## <a name="api-versioning"></a>API-versies 
De huidige versie van de REST-API van CM is 1.0. De versie is opgegeven in het segment dat direct na de `/api` segment in de URI: `/api/v1.0`. In de toekomst, het versienummer verandert moet er belangrijke wijzigingen in de API-interface.


## <a name="enabling-the-api"></a>Inschakelen van de API 
De `<ClmConfiguration>` gedeelte van het bestand Web.config is uitgebreid met een nieuwe sleutel:

```
<add key="Clm.WebApi.Enabled" value="false" />
```
<br/>

Deze sleutel bepaalt of de REST-API van CM wordt blootgesteld aan clients. Als de sleutel is ingesteld op 'false', wordt de toewijzing van de route voor de API niet uitgevoerd op opstarten van de toepassing. Dit betekent dat een HTTP 404-foutcode wordt geretourneerd door de volgende aanvragen die naar de API-eindpunten. De sleutel is standaard ingesteld op "uitgeschakeld".
Vergeet niet om uit te voeren nadat u deze waarde wijzigt op 'true', **iisreset** op de server.

## <a name="enabling-tracing-and-logging"></a>Tracering en logboekregistratie inschakelen 
De REST-API van MIM CM zendt traceringsgegevens voor elke HTTP-aanvraag verzonden naar het netwerkvirtualisatiebeleid. U kunt het uitbreidingsniveau van de traceringsinformatie verzonden door de volgende configuratiewaarde instellen:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```
<br/>

## <a name="error-handling-and-troubleshooting"></a>Foutafhandeling en probleemoplossing 
Wanneer uitzonderingen optreden tijdens het verwerken van een aanvraag, retourneert de MIM CM REST API een HTTP-statuscode aan de webclient. De API voor veelvoorkomende fouten als resultaat geeft een juiste HTTP-statuscode en een foutcode. 

Niet-verwerkte uitzonderingen worden geconverteerd naar een `HttpResponseException` Code met HTTP-Status 500 ('Interne fout') en in het gebeurtenislogboek en de MIM CM-traceringsbestand getraceerd. Elke niet-verwerkte uitzondering wordt geschreven naar het gebeurtenislogboek met een bijbehorende correlatie-ID. De correlatie-ID wordt ook verzonden naar de verbruiker van de API in het foutbericht. Voor het oplossen van de fout kan een beheerder het gebeurtenislogboek voor de corresponderende correlatie-ID en de fout gegevens zoeken.

Stack-traces overeenkomt met fouten als gevolg van de MIM CM REST-API gebruiken die zijn gegenereerd worden om veiligheidsredenen niet terug naar de client verzonden.
