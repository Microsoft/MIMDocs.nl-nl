---
title: Opties voor het genereren van aanvraag certificaat ophalen | Microsoft Docs
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5822da1b9cf8558becccff815fd0208923fcaaa0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="get-certificate-request-generation-options"></a>Opties voor het genereren van aanvraag certificaat ophalen

Parameters voor het genereren van de aanvraag aan de clientzijde certificaat opgehaald.

**Opmerking**: URL's weergegeven in dit onderwerp zijn ten opzichte van de hostnaam van de gekozen tijdens de implementatie van de API; bijvoorbeeld: `https://api.contoso.com`.
##<a name="request"></a>Aanvraag


Methode  |Aanvraag-URL  
---------|---------
GET     |/CertificateManagement/API/V1.0/Requests/{RequestId}/certificaterequestgenerationoptions

###<a name="url-parameters"></a>URL-Parameters
Parameter | Beschrijving
--------|--------------
aanvraag-id| Vereist. De GUID-id van de MIM CM-aanvraag waarvoor de generatie van certificaat aanvraagparameters worden worden opgehaald.

###<a name="request-headers"></a>Aanvraagheaders
Raadpleeg voor algemene aanvraagheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="request-body"></a>Aanvraagtekst
Geen.


##<a name="response"></a>Antwoord
###<a name="response-codes"></a>Reactiecodes
Code  |Beschrijving  
---------|---------
200 | OK
204 | Er is geen inhoud
403 | Is niet toegestaan
500 | Interne fout

###<a name="response-headers"></a>Antwoordheaders
Zie voor veelgebruikte antwoordheaders [HTTP-aanvraag en Reactieheaders](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API-servicedetails*.
###<a name="response-body"></a>Antwoordtekst
Retourneert de lijst met objecten CertificateRequestGenerationOptions geslaagd kunt. Elk object CertificateRequestGenerationOptions overeenkomt met een aanvraag wordt één certificaat dat de client moet genereren en de volgende eigenschappen heeft:

Eigenschap| Beschrijving
--------|-----------
Exporteerbaar | Een waarde die aangeeft of de persoonlijke sleutel gemaakt voor de aanvraag kan worden geëxporteerd.
FriendlyName | De weergavenaam van het ingeschreven certificaat.
HashAlgorithmName | De hash-algoritme wordt gebruikt bij het maken van de handtekening van de aanvraag certificaat.
KeyAlgorithmName | De algoritme van openbare sleutel.
KeyProtectionLevel | Het niveau van de sleutelbeveiliging.
KeySize | De grootte, in bits, van de persoonlijke sleutel moet worden gegenereerd.
KeyStorageProviderNames | Een lijst met toegestane sleutelarchiefproviders (ksp's) die kunnen worden gebruikt om de persoonlijke sleutel te genereren. In het geval kan dat de eerste KSP kan niet worden gebruikt voor het genereren van de aanvraag certificaat kan een van de opgegeven ksp's worden gebruikt tot er één lukt.
KeyUsages | De bewerking die kan worden uitgevoerd door de persoonlijke sleutel gemaakt voor de aanvraag van dit certificaat. De standaardwaarde is ondertekening.
Onderwerp | De naam van de certificaathouder.

**Opmerking**: meer informatie over deze eigenschappen is beschikbaar in de [Windows.Security.Cryptography.Certificates.CertificateRequestProperties klasse](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) beschrijving, maar is niet een-op-een overeenkomst is tussen deze klasse en CertificateRequestGenerationOptions-objecten.

##<a name="example"></a>Voorbeeld

###<a name="request"></a>Aanvraag
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###<a name="response"></a>Antwoord
```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```       
