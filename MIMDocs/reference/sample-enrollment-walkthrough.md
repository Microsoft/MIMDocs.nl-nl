---
title: Voor beeld van een inschrijvings procedure | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5a560e68ea820211caa1dd0a40a0143f44d5f7fb
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758686"
---
# <a name="sample-enrollment-walkthrough"></a>Voor beeld van inschrijvings overzicht
In dit artikel worden de stappen beschreven die nodig zijn voor het uitvoeren van een selfservice registratie voor een virtuele Smart Card. Er wordt een automatisch goedgekeurde aanvraag met een pincode van een gebruiker weer gegeven.

1. De client verifieert de gebruiker en vraagt vervolgens een lijst met profiel sjablonen op die de geverifieerde gebruiker kan inschrijven voor:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    
2. De client geeft de resulterende lijst voor de gebruiker weer. De gebruiker selecteert een vSC-profiel sjabloon (virtuele Smart Card) met de naam ' virtuele-smartcard VPN ' en UUID `97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1` .

3. De client haalt het inschrijvings beleid van de geselecteerde profiel sjabloon op door gebruik te maken van de UUID die in de vorige stap is geretourneerd:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```

4. De client analyseert het veld **DataCollection** in het geretourneerde beleid, waarbij wordt aangegeven dat één gegevensverzamelings item met de titel ' reden van aanvraag ' wordt weer gegeven. De client geeft ook aan dat de vlag **collectComments** is ingesteld op False, zodat de gebruiker niet wordt gevraagd om deze in te voeren.

5. Nadat de gebruiker de reden voor het vereisen van de certificaten heeft opgegeven, maakt de client een aanvraag:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```

6. De server heeft de aanvraag gemaakt en retourneert de URI van de aanvraag naar de client:  `/CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5` .

7. De client haalt het aanvraag object op door de geretourneerde URI aan te roepen:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```

8. De client controleert of de **status** eigenschap in de aanvraag is ingesteld op goedgekeurd. Het uitvoeren van de aanvraag kan beginnen.

9. De client onderzoekt de aanvraag om te zien of er al een Smart Card is gekoppeld aan de aanvraag door de inhoud van de para meter **newsmartcarduuid** te analyseren.

10. Omdat het alleen een lege GUID bevat, moet de client een bestaande kaart gebruiken die niet al in gebruik is door MIM CM, of er een maken als de profiel sjabloon wordt geconfigureerd voor virtuele Smart Cards.

11. Omdat deze laatste is aangegeven met de client via de eerste query voor inschrijvings profielen (stap 1), moet de client nu een virtueel smartcard apparaat maken.

12. De client haalt het smartcard beleid op uit de profiel sjabloon. Het gebruikt de UUID van de sjabloon die is geselecteerd in stap 3:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```

13. Het smartcard beleid bevat de standaard beheerders sleutel voor de kaart in de eigenschap **DefaultAdminKeyHex** . Bij het maken van de Smart Card moet de client de eerste beheerders sleutel van de Smart Card instellen op deze sleutel.  
14. Wanneer u het apparaat voor de Smart Card maakt, moet de client deze toewijzen aan de aanvraag:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```

15. De server reageert op de client met een URI naar het zojuist gemaakte Smart Card-object: `api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644` .

16. De client haalt het smartcard object op:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```

17. Door de waarde van de vlag **diversifyadminkey** te controleren in het smartcard beleid dat is verkregen in stap 12, weet de client dat deze de beheerder sleutel moet spreiden.

18. De client haalt de voorgestelde beheerders sleutel op:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```

19. De client moet bij de kaart worden geverifieerd als beheerder om de beheerders sleutel in te stellen. Hiertoe vraagt de client een verificatie vraag van de Smart Card aan en verzendt deze naar de server:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    De server verzendt het antwoord op de vraag naar de hoofd tekst van het HTTP-antwoord. Zoek de hexadecimale vraag teken reeks, converteer deze naar base64 en geef deze door als een para meter in de URL. De URL retourneert een andere reactie, die opnieuw moet worden geconverteerd naar een hexadecimale indeling.

20. De server verzendt het antwoord op de vraag naar de hoofd tekst van het HTTP-antwoord en de client gebruikt deze om de beheerders sleutel te spreiden.

21. De client merkt dat de gebruiker de gewenste pincode moet opgeven door het veld **UserPinOption** in het smartcard beleid te controleren.

22. Nadat de gebruiker de gewenste pincode in een dialoog venster heeft ingevoerd, voert de client een Challenge-Response-verificatie uit, zoals beschreven in stap 19, met het enige verschil dat de vlag gediversifieerd nu moet worden ingesteld op True:

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```

23. De client gebruikt het antwoord dat is ontvangen van de server om de gewenste gebruikers pincode in te stellen.

24. De client is nu klaar om certificaat aanvragen te genereren. Hiermee worden de para meters voor het genereren van certificaat sjablonen opgevraagd om te bepalen hoe de sleutels/aanvragen moeten worden gegenereerd.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```

25. De server reageert met één JSON-geserialiseerd **CertificateRequestGenerationOptions** -object. Het object bevat para meters voor het exporteren van de sleutel, beschrijvende naam van het certificaat, hash-algoritme, sleutel algoritme, sleutel grootte, enzovoort. De client gebruikt deze para meters voor het genereren van een certificaat aanvraag.

26. De aanvraag voor één certificaat wordt verzonden naar de server. De client kan ook een PFX-wacht woord opgeven dat moet worden gebruikt voor het ontsleutelen van PFX-blobs wanneer de certificaat sjabloon het archiveren van de certificaten op de CA opgeeft, dat wil zeggen dat de CA het sleutel paar genereert en vervolgens verzendt naar de client. De client kan er ook voor kiezen om een aantal opmerkingen toe te voegen.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```

27. Na een paar seconden reageert de server met één JSON-geserialiseerd **micro soft. clm. Shared. Certificate** -object. Door de **isPkcs7** -vlag te controleren, weet de client dat dit antwoord geen pfx-blob is. De client extraheert de BLOB door base64 de pkcs7-teken reeks te decoderen en installeert.

28. De client markeert de aanvraag als voltooid.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
