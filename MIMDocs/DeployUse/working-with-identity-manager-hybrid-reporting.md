---
title: Hybride rapporten in Azure | Microsoft Identity Manager
description: Informatie over het combineren van on-premises en cloudgegevens in hybride rapporten in Azure en het beheren en weergeven van deze rapporten.
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: 0a104a5f79dd48cb2dfc3d739e3ce8dcbd236c0f


---

# Werken met hybride rapportage van Identity Manager

## Beschikbare hybride rapporten
De eerste drie MIM-rapporten (Microsoft Identity Manager) die beschikbaar zijn in Azure AD, zijn **Activiteit voor wachtwoord opnieuw instellen**, **Registratie voor wachtwoord opnieuw instellen** en **Activiteit voor selfservicegroepen**.

-   In Activiteit voor wachtwoord opnieuw instellen wordt elke instantie weergegeven waarbij een gebruiker de bewerking voor het opnieuw instellen van het wachtwoord heeft uitgevoerd met behulp van de SSPR en worden de poorten of **methoden** vermeld die worden gebruikt voor verificatie.

    ![Afbeelding hybride rapportage van Azure - Activiteit wachtwoord opnieuw instellen](media/MIM-Hybrid-passwordreset.jpg)

-   In Registratie voor wachtwoord opnieuw instellen wordt elke instantie weergegeven waarbij een gebruiker zich registreert voor de SSPR en de **methoden** die worden gebruikt om te verifiëren, bijvoorbeeld een mobieletelefoonnummer of vragen en antwoorden.
    Houd er rekening mee dat voor Registratie van wachtwoord opnieuw instellen geen onderscheid wordt gemaakt tussen de SMS-gate en de MFA-gate; beide worden beschouwd als **Mobiele telefoon**.

-   In Activiteit voor selfservicegroepen wordt elke poging weergegeven die door iemand wordt uitgevoerd om zichzelf aan een groep toe te voegen, uit een groep te verwijderen of een groep te maken.

> [!NOTE]
> Momenteel worden de gegevens maximaal één maand weergegeven in de rapporten.
>
> Als u hybride rapporten wilt verwijderen, verwijdert u de agent MIMreportingAgent.msi.

## Vereisten

1.  Installeer Microsoft Identity Manager 2016, inclusief de MIM-service.

2.  Zorg ervoor dat uw adreslijst een Azure AD Premium-tenant met een gelicentieerde beheerder bevat.

3.  Zorg ervoor dat u beschikt over een uitgaande internetverbinding vanaf de Microsoft Identity Manager-server naar Azure.

## Microsoft Identity Manager-rapportage in Azure AD installeren
Nadat de rapportageagent is geïnstalleerd worden de gegevens van de Microsoft Identity Manager-activiteit uit MIM geëxporteerd naar het Windows-gebeurtenislogboek. De MIM-reportageagent verwerkt de gebeurtenissen en uploadt deze naar Azure. In Azure worden de gebeurtenissen geparseerd, ontsleuteld en gefilterd voor de vereiste rapporten.

1.  Installeer Microsoft Identity Manager 2016.

2.  Download de Microsoft Identity Manager-rapportageagents:

    1.  Meld u aan bij de Azure AD-beheerportal en klik op het Active Directory-pictogram.

    2.  Dubbelklik op de map waarvoor u een globale beheerder bent en u een Azure AD Premium-abonnement hebt.

    3.  Klik op **configuratie** en download de rapportageagent.

3.  Installeer de Microsoft Identity Manager-rapportageagent:

    1.  Maak een map op de computer.

    2.  Pak de bestanden `MIMHybridReportingAgent.msi` en `tenant.cert` uit naar de map.

    3.  Voer het installatieprogramma van de agent uit.

    4.  Zorg ervoor dat de MIM-reportageagentservice wordt uitgevoerd

    5.  Start de MIM-service opnieuw op.

4.  Controleer of Microsoft Identity Manager-rapportage in Azure AD werkt.

    U kunt rapportgegevens maken met behulp van de Microsoft Identity Manager-selfservice portal voor wachtwoord opnieuw instellen om het wachtwoord van een gebruiker opnieuw in te stellen. Zorg ervoor dat het opnieuw instellen van het wachtwoord is voltooid en controleer vervolgens of de gegevens worden weergegeven in de Azure AD-beheerportal.

## Hybride-rapporten weergeven in de klassieke Azure Portal

1.  Meld u met uw globale beheerdersaccount voor de tenant aan bij de [klassieke Azure Portal](https://manage.windowsazure.com/).

2.  Klik op het pictogram **Active Directory**.

3.  Selecteer de tenantmap in de lijst met beschikbare mappen voor uw abonnement.

4.  Klik op **Rapporten** en vervolgens op **Activiteit voor wachtwoordherstel**.

5.  Zorg ervoor dat u **Identity Manager** selecteert in de bronvervolgkeuzelijst.

> [!WARNING]
> Het kan even duren voordat Microsoft Identity Manager-gegevens worden weergeven in Azure AD.

## Stoppen met het maken van hybride rapporten
Als u wilt stoppen met het uploaden van rapportagegegevens van Microsoft Identity Manager naar Azure Active Directory, verwijdert u de agent voor hybride rapportage. Gebruik het Windows-hulpprogramma **Programma's installeren of verwijderen** om hybride rapportage van Microsoft Identity Manager te verwijderen.

## Windows-gebeurtenissen die worden gebruikt voor hybride rapportage
Gebeurtenissen die worden gegenereerd door Microsoft Identity Manager, worden geregistreerd in het Windows-gebeurtenislogboek en zijn zichtbaar in Logboeken bij Logboeken voor toepassingen en services-&gt; **Logboek Identity Manager-aanvragen**. Elke MIM-aanvraag wordt geëxporteerd als een gebeurtenis in het Windows-gebeurtenislogboek in JSON-structuur. Dit kan worden geëxporteerd naar uw SIEM.

|Gebeurtenistype|ID|Details van gebeurtenis|
|--------------|------|-----------------|
|Informatie|4121|MIM-gebeurtenisgegevens met alle aanvraaggegevens.|
|Informatie|4137|Extensie van MIM-gebeurtenis 4121, indien er te veel gegevens zijn voor één gebeurtenis. De koptekst in deze gebeurtenis heeft de volgende notatie: `"Request: <GUID> , message <xxx> out of <xxx>`|



<!--HONumber=Jul16_HO3-->


