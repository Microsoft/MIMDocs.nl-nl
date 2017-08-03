---
title: Steekproef inschrijving overzicht | Microsoft Docs
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
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 574a4d6fc2d5d4a51483a371cf96c2d48c582a8b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="sample-enrollment-walkthrough"></a>Voorbeeld inschrijving scenario
Dit onderwerp bevat instructies voor het uitvoeren van een Virtual Smart card selfservice-inschrijving. Het bevat een automatisch goedgekeurd-aanvraag met een PINCODE voor de gebruiker worden ingesteld.
1.  De client verifieert de gebruiker en vervolgens vraagt een lijst met profielsjablonen die de geverifieerde gebruiker kan inschrijven voor:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    <br/>
2.  De client wordt de resulterende lijst voor de gebruiker. De gebruiker selecteert een Profielsjabloon vSC (virtuele smartcard) met de naam 'Virtuele Smartcard VPN' en UUID *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*.

3.  De client haalt het inschrijvingsbeleid van de geselecteerde Profielsjabloon met behulp van de UUID geretourneerd in de vorige stap:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```
 <br/>   
4.  De client analyseert het veld 'DataCollection' in het resulterende beleid weten dat een enkele gegevensverzameling item het recht 'Aanvragen reden' wordt weergegeven. De client ook aangegeven dat de vlag 'collectComments' is ingesteld op *false*, zodat deze niet wordt gevraagd de gebruiker gevraagd een.

5.  Nadat de gebruiker heeft de reden voor het vereisen van de certificaten worden ingevoerd, maakt de client een aanvraag:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.  De server wordt gemaakt van de aanvraag is en retourneert de URI van de aanvraag naar de client: /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5.

7.  De client haalt het request-object door het aanroepen van de geretourneerde URI:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/>
8.  De client controleert of de eigenschap 'state' in de aanvraag is ingesteld op 'goedgekeurde'. Uitvoeren van de aanvraag kan beginnen.

9.  De client controleert het verzoek om te zien of er een smartcard die al zijn gekoppeld aan de aanvraag door een analyse van de inhoud van de parameter 'newsmartcarduuid'.

10. Omdat die alleen een lege GUID bevat, moet de client gebruik een bestaande kaart niet al in gebruik door MIM CM, of een in het geval van de profielsjabloon worden geconfigureerd voor virtuele smartcards maken.

11. Sinds de laatste is opgegeven op de client via de eerste query voor enrollable profielsjablonen (stap 1), moet de client nu een apparaat virtuele smartcard maken.

12. De client haalt het smartcard-beleid van de profielsjabloon (met de UUID van de sjabloon die is geselecteerd in stap 3):

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```
13. Beleid voor de smartcard wordt de standaard beheersleutel voor de kaart in de eigenschap 'DefaultAdminKeyHex' bevatten. Bij het maken van de smartcard moet de client de initiële Beheersleutel van de smartcard ingesteld voor deze sleutel.  

14. Bij het maken van het apparaat via een smartcard, moet de client deze toewijzen aan de aanvraag:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```
<br/>
15. De server reageert op de client met een URI naar de zojuist gemaakte smartcard-object: api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644.

16. De client haalt het smartcard-object:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17. Door het controleren van de waarde van de markering 'diversifyadminkey' in het smartcard-beleid dat is verkregen in stap 12 kent de client heeft de beheersleutel spreiden zodat.

18. De client haalt de voorgestelde beheersleutel:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```
<br/>
19. De client moet worden geverifieerd met de kaart als beheerder om het instellen van de beheersleutel. U doet dit door de client een verificatievraag-aanvragen van de smartcard en wordt verzonden naar de server:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    De server verzendt antwoord op de vraag in de hoofdtekst van de HTTP-antwoord. Ga naar de hexadecimale autorisatietekenreeks, converteren naar base64 en geven deze als een parameter in de URL. De URL wordt een andere-antwoord dat moet worden geconverteerd naar hexadecimale notatie geretourneerd.
<br/>
20. De server verzendt een antwoord op de vraag in de hoofdtekst van de HTTP-antwoord en de client gebruikt de beheersleutel spreiden zodat.

21. De client merkt dat de gebruiker de gewenste pincode opgeven moet door te inspecteren van het veld 'UserPinOption' in het beleid voor de smartcard.

22. Nadat de gebruiker heeft de gewenste pincode ingevoerd in een dialoogvenster, de client een vraag en antwoord-verificatie uitvoert, zoals wordt beschreven in stap 19, met als enige verschil is dat de vlag gediversifieerde moet nu worden ingesteld op 'true':

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```
<br/>
23. De client gebruikt het antwoord ontvangen van de server de gewenste gebruikerspincode instellen.

24. De client is nu gereed voor het genereren van certificaataanvragen. Deze query's de profiel-certificaat generatie Sjabloonparameters om te bepalen hoe de sleutels/aanvragen moeten worden gegenereerd.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25. De server reageert met een enkel CertificateRequestGenerationOptions JSON-geserialiseerd object met gedetailleerde informatie over het exporteren van de sleutel, certificaat beschrijvende naam, hash-algoritme, sleutelalgoritme, sleutelgrootte, enzovoort. Deze parameters van de client wordt gebruikt voor het genereren van een certificaataanvraag.

26. De aanvraag wordt één certificaat is verzonden naar de server. De client kan een PFX-wachtwoord op dat moet worden gebruikt voor het ontsleutelen van een pfx-BLOB's in het geval waarin de certificaatsjabloon geeft archiveren van de certificaten op de CA ook opgeven dat wil zeggen CA genereert het sleutelpaar en verzendt het naar de client. De client kunt ook een aantal opmerkingen toevoegen.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27. Na enkele seconden reageert de server met een enkele JSON-geserialiseerd Microsoft.Clm.Shared.Certificate-object. Door het controleren van de vlag 'isPkcs7' leert de client deze reactie is niet een PFX-blob. De client haalt de blob met base64 decoderen van de tekenreeks 'pkcs7' en installeert deze.

28. De client markeert de aanvraag als voltooid.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
