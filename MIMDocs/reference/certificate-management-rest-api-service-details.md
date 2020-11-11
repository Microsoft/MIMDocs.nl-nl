---
title: Details van de REST API-service voor certificaat beheer | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a2dd84e121217772a8831653b2e4790436c32ec
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492443"
---
# <a name="certificate-management-rest-api-service-details"></a>Details van de REST API-service voor certificaat beheer
In de volgende secties worden de details van het Microsoft Identity Manager (MIM) Certificate Management-REST API (CM) beschreven.

## <a name="architecture"></a>Architectuur 
MIM CM REST API-aanroepen worden verwerkt door-controllers. In de volgende tabel ziet u de volledige lijst met controllers en voor beelden van de context waarin ze kunnen worden gebruikt.


|                  Regelaar                   |                                                                                                                                                           Voorbeeld route                                                                                                                                                           |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           CertificateDataController           |                                                                                                                                         /api/v1.0/requests/{requestid}/certificatedata/                                                                                                                                          |
| CertificateRequestGenerationOptionsController |                                                                                                                                                  /api/v1.0/requests/{requestid}                                                                                                                                                  |
|            CertificatesController             |                                                                                                                /api/v1.0/smartcards/{smartcardid}/certificates <br/> /api/v1.0/profiles/{profileid}/certificates                                                                                                                 |
|             OperationsController              |                                                                                                                  /api/v1.0/smartcards/{smartcardid}/operations <br/> /api/v1.0/profiles/{profileid}/operations                                                                                                                   |
|              PoliciesController               |                                                                                                                                   /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                                                   |
|              ProfilesController               |                                                                                                                                                     /api/v1.0/profiles/{id}                                                                                                                                                      |
|          ProfileTemplatesController           |                                                                                               /api/v1.0/profiletemplates/{id} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                |
|              RequestsController               |                                                                                                                                         /api/v1.0/requests/{id} <br/> /api/v1.0/requests                                                                                                                                         |
|             SmartcardsController              | /api/v1.0/requests/{requestid}/smartcards/{id}/diversifiedkey <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/serverproposedpin <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/authenticationresponse <br/> /api/v1.0/requests/{requestid}/smartcards/{id} <br/> /api/v1.0/smartcards/{id} <br/> /api/v1.0/smartcards |
|       SmartcardsConfigurationController       |                                                                                                                             /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards                                                                                                                              |

## <a name="http-request-and-response-headers"></a>HTTP-aanvraag-en reactie headers
HTTP-aanvragen die worden verzonden naar de API moeten de volgende headers bevatten (deze lijst is niet limitatief):

Header | Beschrijving
-------|------------
Autorisatie | Vereist. De inhoud is afhankelijk van de verificatie methode. De methode kan worden geconfigureerd en kan worden gebaseerd op WIA (Windows Integrated Authentication) of Active Directory Federation Services (ADFS).
Content-Type | Vereist als de aanvraag een hoofd tekst heeft. Moet zijn `application/json` .
Content-Length | Vereist als de aanvraag een hoofd tekst heeft. 
Cookies | De sessie cookie. Is mogelijk vereist, afhankelijk van de verificatie methode.


HTTP-antwoorden bevatten de volgende headers (deze lijst is niet limitatief):

Header | Beschrijving
-------|------------
Content-Type | De API retourneert altijd `application/json` .
Content-Length | De lengte van de aanvraag tekst, indien aanwezig, in bytes.


## <a name="api-versioning"></a>API-versie beheer 
De huidige versie van de CM-REST API is 1,0. De versie wordt in het segment opgegeven, direct na het `/api` segment in de URI: `/api/v1.0` . Het versie nummer verandert wanneer er belang rijke wijzigingen zijn aangebracht in de API-interface.


## <a name="enable-the-api"></a>De API inschakelen 
De `<ClmConfiguration>` sectie van het Web.config bestand is uitgebreid met een nieuwe sleutel:

```
<add key="Clm.WebApi.Enabled" value="false" />
```

Deze sleutel bepaalt of de CM-REST API worden blootgesteld aan clients. Als de sleutel is ingesteld op ' false ', wordt de toewijzing van de route voor de API niet uitgevoerd bij het opstarten van de toepassing. Volgende aanvragen voor API-eind punten retour neren een HTTP 404-fout code. De sleutel wordt standaard ingesteld op uitgeschakeld.

>[!NOTE]
>Nadat u deze waarde hebt gewijzigd in ' True ', moet u **IISReset** uitvoeren op de server.

## <a name="enable-tracing-and-logging"></a>Tracering en logboek registratie inschakelen 
Het MIM CM REST API verzendt tracerings gegevens voor elke HTTP-aanvraag die ernaar wordt verzonden. U kunt het uitbreidings niveau instellen van de tracerings gegevens die worden gegenereerd door de volgende configuratie waarde in te stellen:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```

## <a name="error-handling-and-troubleshooting"></a>Fout afhandeling en probleem oplossing 
Wanneer uitzonde ringen optreden tijdens het verwerken van een aanvraag, retourneert de MIM CM REST API een HTTP-status code naar de webclient. Voor veelvoorkomende fouten retourneert de API een juiste HTTP-status code en een fout code. 

Niet-verwerkte uitzonde ringen worden geconverteerd naar een `HttpResponseException` met HTTP-status Code 500 (interne fout) en worden getraceerd in het gebeurtenis logboek en het MIM cm tracerings bestand. Elke onverwerkte uitzonde ring wordt naar het gebeurtenis logboek geschreven met een bijbehorende correlatie-ID. De correlatie-ID wordt ook verzonden naar de consument van de API in het fout bericht. Om de fout op te lossen, kan een beheerder in het gebeurtenis logboek zoeken naar de bijbehorende correlatie-ID en fout Details.

>[!NOTE]
>Stack traceringen die overeenkomen met fouten die worden gegenereerd als gevolg van het gebruik van de MIM CM REST API, worden niet teruggestuurd naar de client. De traceringen worden niet teruggestuurd naar de client als gevolg van beveiligings problemen.
