---
title: Werken met hybride rapportage in Azure met behulp van MIM 2016 | Microsoft Docs
description: Informatie over het combineren van on-premises en cloudgegevens in hybride rapporten in Azure en het beheren en weergeven van deze rapporten.
keywords: 
author: fimguy
ms.author: billmath
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: df842309034ad68151dd8cc4151507e7ece6626d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# Werken met hybride rapportage van Identity Manager - Openbare preview-versie (vernieuwen)
<a id="working-with-identity-manager-hybrid-reporting---public-preview-refresh" class="xliff"></a>

## Beschikbare hybride rapporten
<a id="available-hybrid-reports" class="xliff"></a>
De eerste drie MIM-rapporten (Microsoft Identity Manager) die beschikbaar zijn in Azure AD, zijn **Activiteit voor wachtwoord opnieuw instellen**, **Registratie voor wachtwoord opnieuw instellen** en **Activiteit voor selfservicegroepen**.

-   In Activiteit voor wachtwoord opnieuw instellen wordt elke instantie weergegeven waarbij een gebruiker de bewerking voor het opnieuw instellen van het wachtwoord heeft uitgevoerd met behulp van de SSPR en worden de poorten of **methoden** vermeld die worden gebruikt voor verificatie.

-   In Registratie voor wachtwoord opnieuw instellen wordt elke instantie weergegeven waarbij een gebruiker zich registreert voor de SSPR en de **methoden** die worden gebruikt om te verifiëren, bijvoorbeeld een mobieletelefoonnummer of vragen en antwoorden.
    Houd er rekening mee dat voor Registratie van wachtwoord opnieuw instellen geen onderscheid wordt gemaakt tussen de SMS-gate en de MFA-gate; beide worden beschouwd als **Mobiele telefoon**.

-   In Activiteit voor selfservicegroepen wordt elke poging weergegeven die door iemand wordt uitgevoerd om zichzelf aan een groep toe te voegen, uit een groep te verwijderen of een groep te maken.

    ![Afbeelding hybride rapportage van Azure - Activiteit wachtwoord opnieuw instellen](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> Momenteel worden de gegevens maximaal één maand weergegeven in de rapporten.</br>
> De vorige hybride-agent moet worden verwijderd</br>
> Als u hybride rapporten wilt verwijderen, verwijdert u de agent MIMreportingAgent.msi.

## Vereisten
<a id="prerequisites" class="xliff"></a>

1.  Installeer Microsoft Identity Manager 2016 RTM /of SP1 MIM-service.

2.  Zorg ervoor dat uw adreslijst een Azure AD Premium-tenant met een gelicentieerde beheerder bevat.

3.  Zorg ervoor dat u beschikt over een uitgaande internetverbinding vanaf de Microsoft Identity Manager-server naar Azure.

## Vereisten
<a id="requirements" class="xliff"></a>
De volgende tabel bevat een lijst met alle vereisten voor hybride rapportage Microsoft Identity Manager.

| Vereiste | Beschrijving |
| --- | --- |
| Azure AD Premium | Hybride rapportage is een Azure AD Premium-functie waarvoor Azure AD Premium is vereist. </br></br>Zie [Aan de slag met Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-get-started-premium) voor meer informatie </br>Zie [Een proefversie starten](https://azure.microsoft.com/trial/get-started-active-directory/) voor een gratis proefversie van 30 dagen. |
| U moet een globale beheerder van Azure AD zijn om te beginnen |Alleen globale beheerders kunnen de agents installeren en configureren om aan de slag te gaan, toegang te krijgen tot de portal en bewerkingen in Azure uit te voeren. </br></br>**Belangrijk:** Het account dat wordt gebruikt voor installatie van de agents, moet een werk- of schoolaccount zijn. Dit mag geen Microsoft-account zijn. Zie [Sign up for Azure as an organization](https://docs.microsoft.com/en-us/azure/active-directory/sign-up-organization) (Aanmelden voor Azure als bedrijf) voor meer informatie |
| De agent voor hybride rapportage van Microsoft Identity Manager is geïnstalleerd op elke doelserver van de MIM-service | Voor hybride rapportage is vereist dat de agents worden geïnstalleerd en geconfigureerd op de doelservers voor de ontvangst van gegevens en voor het verstrekken van mogelijkheden voor controle en analyse </br>|
| Uitgaande verbinding met de service-eindpunten van Azure | Tijdens de installatie en runtime moet de agent verbinding hebben met service-eindpunten van Azure. Als uitgaande connectiviteit is geblokkeerd via firewalls, moet u ervoor zorgen dat de volgende eindpunten worden toegevoegd aan de lijst: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net - poort: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Uitgaande verbindingen op basis van IP-adressen | Zie [IP-bereiken van Azure](https://www.microsoft.com/en-us/download/details.aspx?id=41653) voor filters op basis van IP-adressen op firewalls.|
| SSL-inspectie voor uitgaand verkeer is gefilterd of uitgeschakeld | De registratie- of uploadbewerkingen in de agent mislukken mogelijk als er sprake is van SSL-inspectie of beëindiging van uitgaand verkeer in de netwerklaag. |
| Firewallpoorten op de server waarop de agent wordt uitgevoerd. |Voor de agent is vereist dat de volgende firewallpoorten zijn geopend zodat de agent kan communiceren met de service-eindpunten van Azure.</br></br><li>TCP-poort 443</li><li>TCP-poort 5671</li> |
| De volgende websites moeten zijn toegestaan als verbeterde beveiliging van Internet Explorer is ingeschakeld |Als verbeterde beveiliging van Internet Explorer is ingeschakeld, moeten de volgende websites worden toegestaan op de server waarop u de agent wilt installeren.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>De federatieve server voor uw organisatie die wordt vertrouwd door Azure Active Directory. Bijvoorbeeld: https://sts.contoso.com</li> |
</BR>

## Installeer de Microsoft Identity Manager-rapportageagent in Azure AD
<a id="install-microsoft-identity-manager-reporting-agent-in-azure-ad" class="xliff"></a>
Nadat de rapportageagent is geïnstalleerd worden de gegevens van de Microsoft Identity Manager-activiteit uit MIM geëxporteerd naar het Windows-gebeurtenislogboek. De MIM-reportageagent verwerkt de gebeurtenissen en uploadt deze naar Azure. In Azure worden de gebeurtenissen geparseerd, ontsleuteld en gefilterd voor de vereiste rapporten.

1.  Installeer Microsoft Identity Manager 2016.

2.  Download de Microsoft Identity Manager-rapportageagents:

    1.  Meld u aan bij de Azure AD-beheerportal en klik op het Active Directory-pictogram.

    2.  Dubbelklik op de map waarvoor u een globale beheerder bent en u een Azure AD Premium-abonnement hebt.

    3.  Klik op **configuratie** en download de rapportageagent.

3.  Installeer de Microsoft Identity Manager-rapportageagent:

    1.  Download [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) naar de server van de Microsoft Identity Manager-service.
    2.  Voer `MIMHReportingAgentSetup.exe` uit 
    3.  Voer het installatieprogramma van de agent uit.

    4.  Zorg ervoor dat de MIM-reportageagentservice wordt uitgevoerd

    5.  Start de MIM-service opnieuw op.

4.  Controleer of Microsoft Identity Manager-rapportage in Azure AD werkt.

    U kunt rapportgegevens maken met behulp van de Microsoft Identity Manager-selfservice portal voor wachtwoord opnieuw instellen om het wachtwoord van een gebruiker opnieuw in te stellen. Zorg ervoor dat het opnieuw instellen van het wachtwoord is voltooid en controleer vervolgens of de gegevens worden weergegeven in de Azure AD-beheerportal.

## Hybride rapporten weergeven in Azure Portal
<a id="view-hybrid-reports-in-the-azure-portal" class="xliff"></a>

1.  Meld u met uw globale beheerdersaccount voor de tenant aan bij [Azure Portal](https://portal.azure.com/).

2.  Klik op het pictogram **Azure Active Directory**.

3.  Selecteer de tenantmap in de lijst met beschikbare mappen voor uw abonnement.

4.  Klik op **Auditlogboeken**.

5.  Zorg ervoor dat u **MIM-service** selecteert in het vervolgkeuzemenu Categorie.

> [!WARNING]
> Het kan enige tijd duren voordat controlegegevens van Microsoft Identity Manager worden weergegeven in Azure Portal.

## Stoppen met het maken van hybride rapporten
<a id="stop-creating-hybrid-reports" class="xliff"></a>
Als u wilt stoppen met het uploaden van controlegegevens van Microsoft Identity Manager naar Azure Active Directory, verwijdert u de agent voor hybride rapportage. Gebruik het Windows-hulpprogramma **Programma's installeren of verwijderen** om hybride rapportage van Microsoft Identity Manager te verwijderen.

## Windows-gebeurtenissen die worden gebruikt voor hybride rapportage
<a id="windows-events-used-for-hybrid-reporting" class="xliff"></a>
Gebeurtenissen die worden gegenereerd door Microsoft Identity Manager, worden geregistreerd in het Windows-gebeurtenislogboek en zijn zichtbaar in Logboeken bij Logboeken voor toepassingen en services-&gt; **Logboek Identity Manager-aanvragen**. Elke MIM-aanvraag wordt geëxporteerd als een gebeurtenis in het Windows-gebeurtenislogboek in JSON-structuur. Dit kan worden geëxporteerd naar uw SIEM.

|Gebeurtenistype|ID|Details van gebeurtenis|
|--------------|------|-----------------|
|Informatie|4121|MIM-gebeurtenisgegevens met alle aanvraaggegevens.|
|Informatie|4137|Extensie van MIM-gebeurtenis 4121, indien er te veel gegevens zijn voor één gebeurtenis. De koptekst in deze gebeurtenis heeft de volgende notatie: `"Request: <GUID> , message <xxx> out of <xxx>`|
