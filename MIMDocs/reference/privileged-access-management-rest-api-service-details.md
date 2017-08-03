---
title: Details van de PAM REST-API-Service | Microsoft Docs
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
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e248ba4a65e75b1e914c61de70072c7e094123a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="pam-rest-api-service-details"></a>Details van de PAM REST-API-Service
De volgende secties worden de details van de REST-API van Microsoft Identity Manager (MIM) Privileged Access Management (PAM).

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

## <a name="versioning"></a>Versiebeheer voor onderdelen 
De huidige versie van de API is 1. De API-versie kan worden opgegeven via een queryparameter in de aanvraag-URL: `http://localhost:8086/api/pamresources/pamrequests?v=1` als de versie niet is opgegeven in de aanvraag, de aanvraag is uitgevoerd met de meest recent uitgebrachte versie van de API. 

## <a name="security"></a>Beveiliging 
Toegang tot de API vereist geïntegreerde Windows-verificatie (IWA). Dit moet handmatig worden geconfigureerd in IIS voordat de installatie van Microsoft Identity Manager (MIM).

HTTPS (TLS) wordt ondersteund, maar moet handmatig worden geconfigureerd in IIS. Zie voor informatie: **implementeren SSL Secure Sockets Layer () voor de FIM-Portal** in [stap 9: uitvoeren van FIM 2010 R2 taken na de installatie] (https://technet.microsoft.com/library/hh322875 (v=ws.10%29.aspx) in de installatie van FIM 2010 R2 Test Lab Guide. 

U kunt een nieuw SSL-servercertificaat genereren met de volgende opdracht in de Visual Studio-opdrachtprompt:
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
Deze opdracht maakt u een zelfondertekend certificaat dat kan worden gebruikt voor het testen van een webtoepassing die gebruikmaakt van SSL op een webserver waarvan de URL test.cwap.com is. De OID die zijn gedefinieerd door de optie - eku identificeert het certificaat als een SSL-servercertificaat. Het certificaat wordt opgeslagen in de winkel en is beschikbaar op het niveau van de machine, zodat u kunt het exporteren van de module Certificaten in mmc.exe

## <a name="cross-domain-access-cors"></a>Cross-domeintoegang (CORS) 
CORS wordt ondersteund, maar moet handmatig worden geconfigureerd in IIS. De volgende elementen toevoegen aan het geïmplementeerde API web.config-bestand voor het configureren van de API-aanroepen tussen domeinen toestaan: 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```
<br/>

## <a name="error-handling"></a>Foutafhandeling 
De API retourneert HTTP-foutberichten foutcondities aangeeft. Fouten zijn OData-compatibel. De volgende tabel bevat de foutcodes die kunnen worden geretourneerd met een client.

HTTP-statuscode | Beschrijving
-----------------|------------
401 | Niet geautoriseerd 
403 | Is niet toegestaan 
408 | Time-out aanvraag   
500 | Interne serverfout 
503 | Service is niet beschikbaar 
<br/>

## <a name="filtering"></a>Filteren 
PAM REST API-aanvragen kunnen filters om op te geven van de eigenschappen die moeten worden opgenomen in het antwoord bevatten. Hierbij de filtersyntaxis is gebaseerd op de OData-expressies.

Filters kunnen opgeven dat een van de eigenschappen van PAM-aanvragen, PAM-rollen. of PAM-aanvragen in behandeling. Bijvoorbeeld: *ExpirationTime*, *DisplayName*, of een andere geldige eigenschap van een PAM Request, het PAM-Role of de aanvraag die in behandeling.

De API ondersteunt de volgende operators in filterexpressies: *en*, *gelijk*, *NotEqual*, *groter dan*, *LessThan*, *GreaterThenOrEqueal*, en *LessThanOrEqual*. 

Het volgende voorbeeld aanvragen omvatten filters:

- Deze aanvraag retourneert de PAM-aanvragen in een bepaalde periode:`http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- Deze aanvraag retourneert de Pam-Role met de weergavenaam 'SQL bestandstoegang':`http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `
