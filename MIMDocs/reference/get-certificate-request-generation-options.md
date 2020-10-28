---
title: Opties voor het genereren van certificaat aanvragen ophalen | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d8baab788c07dfb8c009857b45e38eb662f98199
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758740"
---
# <a name="get-certificate-request-generation-options"></a>Opties voor het genereren van certificaat aanvragen ophalen
Hiermee worden para meters opgehaald voor het genereren van certificaat aanvragen aan de client zijde.

>[!NOTE]
>De Url's in dit artikel zijn relatief ten opzichte van de hostnaam die wordt gekozen tijdens de implementatie van de API, zoals `https://api.contoso.com` .

## <a name="request"></a>Aanvraag

| Methode |                                       Aanvraag-URL                                        |
|--------|------------------------------------------------------------------------------------------|
|  GET   | /CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions |

### <a name="url-parameters"></a>URL-parameters

Parameter | Beschrijving
--------|--------------
requestid| Vereist. De GUID-id van de MIM CM aanvraag waarvoor de para meters voor het genereren van certificaat aanvragen moeten worden opgehaald.

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
204 | Geen inhoud
403 | Verboden
500 | Interne fout

### <a name="response-headers"></a>Antwoordheaders
Zie [HTTP-aanvraag-en reactie headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm rest API service Details* voor algemene aanvraag headers.

### <a name="response-body"></a>Hoofdtekst van de reactie
Als de bewerking is voltooid, wordt een lijst met CertificateRequestGenerationOptions-objecten geretourneerd. Elk CertificateRequestGenerationOptions-object komt overeen met één certificaat aanvraag die de client moet genereren. Elk object heeft de volgende eigenschappen:

Eigenschap| Beschrijving
--------|-----------
Exporteerbaar | Een waarde die aangeeft of de persoonlijke sleutel die voor de aanvraag is gemaakt, kan worden geëxporteerd.
FriendlyName | De weergave naam van het Inge schreven certificaat.
HashAlgorithmName | Het hash-algoritme dat wordt gebruikt bij het maken van de hand tekening van de certificaat aanvraag.
Superalgoritmenaam | Het algoritme van de open bare sleutel.
KeyProtectionLevel | Het niveau van sterke sleutel beveiliging.
KeySize | De grootte, in bits, van de persoonlijke sleutel die moet worden gegenereerd.
KeyStorageProviderNames | Een lijst met acceptabele sleutelarchiefprovider (Ksp's) die kan worden gebruikt om de persoonlijke sleutel te genereren. Wanneer de eerste SLEUTELARCHIEFPROVIDER niet kan worden gebruikt om de certificaat aanvraag te genereren, kan een van de opgegeven Ksp's worden gebruikt totdat er één slaagt.
Gebruik | De bewerking die kan worden uitgevoerd door de persoonlijke sleutel die voor deze certificaat aanvraag is gemaakt. De standaard waarde is ondertekenen.
Onderwerp | De naam van het onderwerp.

>[!NOTE]
>Meer informatie over deze eigenschappen vindt u in de beschrijving van de [klasse Windows. Security. Cryptography. certificates. CertificateRequestProperties](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) . Houd er wel bij dat er geen een-op-een-correspondentie is tussen deze klasse en CertificateRequestGenerationOptions-objecten.
>

## <a name="example"></a>Voorbeeld
In deze sectie vindt u een voor beeld van het ophalen van de opties voor het genereren van een certificaat aanvraag.

### <a name="example-request"></a>Voor beeld: aanvraag

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1
```

### <a name="example-response"></a>Voor beeld: antwoord

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
