---
title: Get-Smartcard of het profiel certificaten | Microsoft Docs
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
ms.assetid: 206bde70-4af8-46fa-9c0b-f574745b0977
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 786562f1a6ffd0dcaefd7b11af8756b08727ecae
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-or-profile-certificates"></a>Get-Smartcard of het profiel voor certificaten
Hiermee wordt een lijst met certificaten die zijn gekoppeld aan het profiel voor bepaalde smartcard of software opgehaald.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/API/V1.0/Profiles/{id}/Certificates <br/>/CertificateManagement/API/V1.0/smartcards/{id}/Certificates

###<a name="url-parameters"></a>URL-Parameters
Parameter | Beschrijving
---------|------------
ID | De id (GUID) van het profiel of smartcard.

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
Indien geslaagd kunt retourneert een lijst met JSON-geserialiseerd [Microsoft.Clm.Shared.Certificates.X509ClmCertificate](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx) objecten met de volgende eigenschappen:

Naam | Beschrijving
-----|------------
ArchivedOnCa | Een Boolean die aangeeft of het certificaat op de certificeringsinstantie (CA) wordt gearchiveerd.
CertificateType | Het type van het certificaat.
isKeyHistory | Een Boolean die aangeeft of het certificaat een certificaat van de geschiedenis van de sleutel is.
SerialNumber | Serienummer van het certificaat.
TemplateCommonName | Algemene naam van het certificaatsjabloon voor het certificaat.
Vingerafdruk | Vingerafdruk van het certificaat.

##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
GET /certificatemanagement/api/v1.0/smartcards/5badfea3-de31-4837-99f9-8249515a5473/certificates HTTP/1.1
```
###<a name="response"></a>Antwoord
```
HTTP/1.1 200 OK

[
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01052AFA01313FB77AC00010000B010",
        "Thumbprint":"C52B0C5FB8AAD31A5B239FF2712ED14122D67D30"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B011AB48AE7D664ED5D900010000B011",
        "Thumbprint":"E7C4324896271BE869544FF28AA2B1BF3B3BDFCF"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01ABCF2D38A0CCEAD8F00010000B01A",
        "Thumbprint":"D9293B5414C644888444541B64631E90F2612425"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B1F02865CF2C3A9DD96D00010000B1F0",
        "Thumbprint":"6615DDC8603DBA789D724502627682F37D0FC2D0"
    }
]
```       
##<a name="see-also"></a>Zie ook

- [Microsoft.Clm.Provision.FindOperations.FindCertificates methode](https://msdn.microsoft.com/library/microsoft.clm.provision.findoperations.findcertificates.aspx)
- [Microsoft.Clm.Shared.Certificates.X509ClmCertificate klasse](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx)
