---
title: Smartcards voor niet-beheerders registreren | Microsoft Identity Manager
description: Informatie over het registreren van smartcards voor gebruikers die geen beheerdersrechten hebben voor hun computers, zodat ze de certificaatbeheerder kunnen gebruiken.
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 875ed6b96929822ac166a4a262cc8547a4ea3b2a
ms.openlocfilehash: 50c0edd60abfa6550cb5a873d32a842e8c099811


---

# Smartcards voor niet-beheerders registreren
Gebruikers die geen lokale beheerder zijn van hun computer, kunnen standaard geen smartcard registreren op hun eigen computer. Dit probleem kunt u met de volgende procedure omzeilen.

## Vernieuwen van smartcard inschakelen voor niet-beheerders in MIM 2016 Certificate Manager

1.  **Pak het APPX-bestand uit**

    Haal een handtekeningcertificaat op. Volg de stappen voor het [aanmelden bij Windows 8-programma’s met behulp van een interne PKI](http://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx). Stop als u 'Het programma ondertekenen' bereikt. Geef het geëxporteerde PFX-bestand een naam. Exporteer dit tevens naar een CER-bestand en importeer dit in de client, waarbij u het CER-bestand van het nieuwe handtekeningcertificaat gebruikt.

    Voer de volgende opdracht uit om het APPX-bestand uit te pakken:

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **Wijzig het configuratiebestand**

    Wijzig de naam van het bestand met de naam `CustomDataExample.xml custom.data`. De CM-app zoekt naar deze bestandsnaam.

    Bewerk het bestand custom.data en wijzig het volgende:

    1.  Wijzig in het element &lt;NonAdmin&gt; de waarde van het kenmerk Value in True

    2.  Sla het bestand op en sluit de editor af

    3.  Verwijder het bestand met de naam AppxSignature.p7x

    4.  Bewerk het bestand met de naam AppxManifest.xml

    5.  Wijzig in het element &lt;Identiteit&gt; de waarde van het kenmerk Uitgever zodat deze hetzelfde is als het onderwerp van het handtekeningcertificaat, bijvoorbeeld CN = ABCD

        Het onderwerp moet gelijk zijn aan het onderwerp in het handtekeningcertificaat dat u gebruikt om de app te ondertekenen.

    6.  Sla het bestand op en sluit de editor af.

3.  **Pak het bestand opnieuw in en onderteken het app-pakket (APPX-bestand)**

    Voer de volgende opdracht uit om het APPX-bestand in te pakken en te ondertekenen:

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    s`igntool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  Dupliceer de profielsjabloon en voeg de initiële beheersleutel toe om de MIM-server te configureren:

    1.  Meld u bij de CM-portal aan als gebruiker met beheerdersbevoegdheden.

    2.  Ga naar **Beheer** &gt; **Profielsjablonen beheren**. Controleer of het selectievakje is ingeschakeld naast de profielsjabloon die u zojuist hebt gemaakt en klik op Een geselecteerde profielsjabloon kopiëren.

    3.  Typ de naam van de profielsjabloon, voeg nonAdmin toe en klik op **OK**.

    4.  Wanneer de algemene instellingen van de profielsjabloon worden weergegeven, schuift u helemaal naar beneden en klikt u onder **Smartcards configureren** op **Instellingen wijzigen**.

    5.  Voer onder **Beginwaarde van de beheersleutel (hex)** de standaard beheersleutel in: 010203040506070801020304050607080102030405060708

    6.  Schuif naar beneden en klik op **OK**.

5.  **Maak een niet-beheerdersaccount op de clientcomputer**

    Gebruikers zonder beheerdersrechten kunnen op de TPM geen virtuele smartcard maken. U moet deze dus voor hen maken.

6.  **Maak een virtuele smartcard met TpmVscMgr**

    Voer de volgende stappen uit (nog steeds als de beheerder) om een lege virtuele smartcard te maken op een computer. U kunt dit doen via Intune, SCCM of groepsbeleid.

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **Installeer de CM-app in het niet-beheerdersaccount**

8.  **Start de CM-app en registratie voor een virtuele smartcard**



<!--HONumber=Jun16_HO4-->


