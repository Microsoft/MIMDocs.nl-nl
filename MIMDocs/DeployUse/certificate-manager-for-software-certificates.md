---
title: Softwarecertificaten maken | Microsoft Identity Manager
description: Informatie over het gebruik van de certificaatbeheerder voor het maken en vernieuwen van softwarecertificaten met profielsjablonen.
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fed5ada9-d80f-4825-aad7-4172ac5d71d3
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: d5c8fc4f9a3eaab95441f7a915f7e02d55042ae9


---

# Softwarecertificaten maken met de certificaatbeheerder
U hoeft geen beheerder te zijn of over een virtuele smartcard te beschikken om softwarecertificaten te registreren en verlengen. Op een bepaald moment wordt u wel gevraagd toestemming te geven voor het bewerken van het certificaat. Dit is normaal.

## Een profielsjabloon van een softwarecertificaat maken in MIM 2016 Certificate Manager

1.  Maak een sjabloon voor het certificaat dat u wilt aanvragen voor de virtuele smartcard. Open MMC.

2.  Klik op **Bestand** en klik vervolgens op **Module toevoegen/verwijderen**.

3.  Klik in de lijst met beschikbare modules op **Certificaatsjablonen** en vervolgens op **Toevoegen**.

4.  In MMC wordt **Certificaatsjablonen** nu weergegeven onder Consolebasis. Dubbelklik erop om alle beschikbare certificaatsjablonen weer te geven.

5.  Klik met de rechtermuisknop op **Gebruikerssjabloon** en klik op **Sjabloon dupliceren**.

6.  Selecteer op het tabblad **Compatibiliteit** onder Certificeringsinstantie de optie Windows Server 2008 en onder Ontvanger van certificaat de optie Windows 8.1/Windows Server 2012 R2.

    1.  Typ op het tabblad **Algemeen** in het veld met de weergavenaam **Gearchiveerde certificaatsjabloon**.

    2.  b.  Op het tabblad **Afhandeling van aanvragen**

        1.  Stel bij **Doel** de waarde in op Handtekening en versleuteling.

        2.  Schakel **Symmetrische algoritmen opnemen die door het object worden toegestaan** in.

        3.  Als u de sleutel wilt archiveren, schakelt u **Persoonlijke versleutelingssleutel van de houder archiveren**.

        4.  Selecteer onder Ga als volgt te werk... de optie **De gebruiker om invoer vragen tijdens het inschrijven**.

    3.  Op het tabblad **Cryptografie**

        1.  Selecteer onder Providercategorie **Sleutelarchiefprovider**

        2.  Selecteer **Voor aanvragen kan elke provider worden gebruikt die beschikbaar is op de computer**.

    4.  Voeg op het tabblad **Beveiliging** de beveiligingsgroep toe waarvoor u toegang voor **Inschrijven** wilt verlenen. Als u bijvoorbeeld aan alle gebruikers toegang wilt verlenen, selecteert u de groep **Geverifieerde gebruikers** en selecteert u vervolgens **Inschrijvingsmachtigingen** voor deze gebruikers.

    5.  Op het tabblad **Onderwerpnaam**

        1.  Schakel het selectievakje **E-mailnaam in onderwerpnaam opnemen** uit.

        2.  Schakel onder **De volgende informatie in de alternatieve objectnaam opnemen** het selectievakje **E-mailnaam** uit.

    6.  Klik op **OK** om de wijzigingen te voltooien en de nieuwe sjabloon te maken. De nieuwe sjabloon wordt nu in de lijst met certificaatsjablonen weergegeven.

    7.  Selecteer **Bestand**, klik vervolgens op **Module toevoegen/verwijderen** om de module Certificeringsinstantie toe te voegen aan de MMC-console. Wanneer u wordt gevraagd welke computer u wilt beheren, selecteert u **Lokale computer**.

    8.  Vouw in het linkerdeelvenster van MMC **Certificeringsinstantie (lokaal)** uit en vouw vervolgens uw certificeringsinstantie in de lijst met certificeringsinstanties uit.

    9. Klik met de rechtermuisknop op **Certificaatsjablonen**, klik op **Nieuw** en klik vervolgens op **Te verlenen certificaatsjablonen**.

    10. Selecteer in de lijst de nieuwe sjabloon die u zojuist hebt gemaakt (**Gearchiveerde certificaatsjabloon**) en klik vervolgens op **OK**.

## Het profielsjabloon maken

1.  Meld u bij de CM-portal aan als gebruiker met beheerdersbevoegdheden.

2.  Ga naar **Beheer&gt; Profielsjablonen beheren** en zorg ervoor dat het selectievakje naast **Voorbeeldprofielsjabloon voor MIM CM voor het aanmelden via een smartcard** is ingeschakeld en klik vervolgens op **Een geselecteerde profielsjabloon kopiëren**.

3.  Typ de naam van de profielsjabloon en klik op **OK**.

4.  Klik in het volgende scherm op **Nieuwe certificaatsjabloon toevoegen** en zorg ervoor dat het selectievakje naast de naam van de certificeringsinstantie is ingeschakeld.

5.  Schakel het selectievakje in naast de naam van de gearchiveerde certificaatsjabloon en klik op **Toevoegen**.

6.  Verwijder de gebruikerssjabloon door het selectievakje ernaast in te schakelen, op **Geselecteerde certificaatsjablonen verwijderen** en vervolgens op **OK** te klikken.

7.  Klik op **Algemene instellingen wijzigen**.

8.  Schakel de selectievakjes links naast **Coderingssleutels op de server genereren** in en klik op **OK**. Klik in het linkerdeelvenster op **Herstelbeleid**.

9. Klik op **Algemene instellingen wijzigen**.

10. Als u gearchiveerde certificaten opnieuw uitgegeven wilt, schakelt u de selectievakjes links naast **Gearchiveerde certificaten opnieuw verlenen** in en klikt u op **OK**.

11. Als u de CM virtuele smartcard gebruikt, moet u gegevensverzamelingsitems uitschakelen omdat de smartcard niet werkt met wanneer gegevensverzameling is ingeschakeld. Schakel gegevensverzameling voor elk beleid uit door op het beleid in het linkerdeelvenster te klikken, het selectievakje naast **Voorbeeldgegevensitem** uit te schakelen en vervolgens op **Gegevensverzamelingsitems verwijderen** te klikken. Klik vervolgens op **OK**.



<!--HONumber=Jul16_HO3-->


