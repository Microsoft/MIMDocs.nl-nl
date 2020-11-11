---
title: Details van PAM-REST API-service | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00a2f4d9c44747d50139655d368e42b11fbd388c
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492426"
---
# <a name="pam-rest-api-service-details"></a>Details van PAM-REST API-service
In de volgende secties worden de details van de Microsoft Identity Manager (MIM) Privileged Access Management (PAM) REST API beschreven.

<h2 id="http-request-and-response-headers">HTTP-aanvraag headers</h2>

HTTP-aanvragen die worden verzonden naar de API moeten de volgende headers bevatten (deze lijst is niet limitatief):

Header | Beschrijving
-------|------------
Autorisatie | Vereist. De inhoud is afhankelijk van de verificatie methode, die kan worden geconfigureerd en kan worden gebaseerd op WIA (Windows Integrated Authentication) of ADFS.
Content-Type | Vereist als de aanvraag een hoofd tekst heeft. Moet worden ingesteld op `application/json`.
Content-Length | Vereist als de aanvraag een hoofd tekst heeft. 
Cookies | De sessie cookie. Is mogelijk vereist, afhankelijk van de verificatie methode.

## <a name="http-response-headers"></a>HTTP-antwoord headers

HTTP-antwoorden moeten de volgende headers bevatten (deze lijst is niet limitatief):

Header | Beschrijving
-------|------------
Content-Type | De API retourneert altijd `application/json` .
Content-Length | De lengte van de aanvraag tekst, indien aanwezig, in bytes.

## <a name="versioning"></a>Versiebeheer 
De huidige versie van de API is 1. De API-versie kan worden opgegeven via een query parameter in de aanvraag-URL, zoals in het volgende voor beeld: `http://localhost:8086/api/pamresources/pamrequests?v=1` als de versie niet is opgegeven in de aanvraag, wordt de aanvraag uitgevoerd op basis van de meest recente versie van de API. 

## <a name="security"></a>Beveiliging 
Voor toegang tot de API is geïntegreerde Windows-verificatie (IWA) vereist. Dit moet hand matig worden geconfigureerd in IIS vóór de installatie van Microsoft Identity Manager (MIM).

HTTPS (TLS) wordt ondersteund, maar moet hand matig worden geconfigureerd in IIS. Zie voor meer informatie: **implementatie van Secure Sockets Layer (SSL) voor de FIM-Portal** in [stap 9: Voer de taken van FIM 2010 R2 na de installatie](https://technet.microsoft.com/library/hh322875.aspx) uit in de test lab-hand leiding voor het installeren van FIM 2010 R2. 

U kunt een nieuw SSL-server certificaat genereren door de volgende opdracht uit te voeren in de Visual Studio-opdracht prompt:

```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
Met de opdracht wordt een zelfondertekend certificaat gemaakt dat kan worden gebruikt voor het testen van een webtoepassing die gebruikmaakt van SSL op een webserver waarop de URL is `test.cwap.com` . Met de OID die is gedefinieerd met de `-eku` optie wordt het certificaat aangeduid als een SSL-server certificaat. Het certificaat wordt opgeslagen in **mijn** Store en is beschikbaar op machine niveau. U kunt het certificaat exporteren vanuit de module Certificaten in mmc.exe.

## <a name="cross-domain-access-cors"></a>Toegang tussen domeinen (CORS) 
CORS wordt ondersteund, maar moet hand matig worden geconfigureerd in IIS. Voeg de volgende elementen toe aan het geïmplementeerde API-web.config bestand om de API te configureren voor het toestaan van cross-domein aanroepen: 

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

## <a name="error-handling"></a>Foutafhandeling 
De API retourneert HTTP-fout berichten om fout voorwaarden aan te geven. Fouten zijn compatibel met OData. De volgende tabel bevat de fout codes die kunnen worden geretourneerd naar een client:

HTTP-statuscode | Beschrijving
-----------------|------------
401 | Niet geautoriseerd 
403 | Verboden 
408 | Time-out van aanvraag   
500 | Interne server fout 
503 | Service niet beschikbaar 

## <a name="filtering"></a>Filteren 
PAM-REST API aanvragen kunnen filters bevatten om de eigenschappen op te geven die in het antwoord moeten worden opgenomen. De filter syntaxis is gebaseerd op OData-expressies.

Filters kunnen een van de eigenschappen van PAM-aanvragen opgeven, PAM-rollen. of PAM-aanvragen in behandeling. Bijvoorbeeld: *ExpirationTime* , *DisplayName* of een andere geldige eigenschap van een PAM-aanvraag, Pam-rol of aanvraag in behandeling.

De API ondersteunt de volgende Opera tors in filter expressies: *en* , *EQUAL* , *NotEqual* , *GreaterThan* , *LessThan* , *GreaterThenOrEqueal* en *LessThanOrEqual*. 

De volgende voorbeeld aanvragen bevatten filters:

- Deze aanvraag retourneert alle PAM-aanvragen tussen specifieke datums: `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime'2015-01-09T08:26:49.721Z' and ExpirationTime lt datetime'2015-02-10T08:26:49.722Z' `
 
- Deze aanvraag retourneert de pam-rol met de weergave naam ' SQL File Access ': `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq 'SQL File Access' `
