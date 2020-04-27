---
title: Werken met hybride rapportage in azure met behulp van Identity Manager 2016 | Microsoft Docs
description: Informatie over het combineren van on-premises en cloudgegevens in hybride rapporten in Azure en het beheren en weergeven van deze rapporten.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 2/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: fd0efd3e3d5c42f4b67d0abd42f6dab8254573e5
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044341"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Werken met hybride rapportage in Identity Manager

In dit artikel wordt beschreven hoe u on-premises en Cloud gegevens kunt combi neren in hybride rapporten in Azure en hoe u deze rapporten kunt beheren en weer geven.

## <a name="available-hybrid-reports"></a>Beschikbare hybride rapporten
De eerste drie Microsoft Identity Manager rapporten die beschikbaar zijn in Azure Active Directory (Azure AD) zijn als volgt:

- **Activiteit voor het opnieuw instellen van het wacht woord**: geeft elk exemplaar weer wanneer een gebruiker een wacht woord opnieuw instelt met behulp van selfservice voor wachtwoord herstel (SSPR) en biedt de poorten of methoden die voor verificatie worden gebruikt.

- **Registratie van wacht woord opnieuw instellen**: elke keer dat een gebruiker zich registreert voor SSPR en de methoden die worden gebruikt voor verificatie. Voor beelden van methoden zijn mogelijk een mobiel telefoon nummer of vragen en antwoorden.
   > [!NOTE]
   > Voor rapporten over het *opnieuw instellen van wacht woorden* wordt geen onderscheid gemaakt tussen de SMS-Gate en de MFA-Gate. Beide worden beschouwd als mobiele-telefoon methoden.

- **Activiteit voor selfservice groepen**: hier wordt elke poging van iemand weer gegeven om hem of haar toe te voegen of te verwijderen uit het maken van een groep en groep.

    ![Afbeelding hybride rapportage van Azure - Activiteit wachtwoord opnieuw instellen](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * In de rapporten worden momenteel gegevens weer gegeven voor Maxi maal één maand van de activiteit.
> * De vorige Hybrid Reporting agent moet worden verwijderd.
> * Als u hybride rapporten wilt verwijderen, verwijdert u de agent mimreportingagent. msi-agent.

## <a name="prerequisites"></a>Vereisten

* Identity Manager 2016 SP1 Identity Manager-service, aanbevolen build [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* Een Azure AD Premium Tenant met een gelicentieerde beheerder in uw Directory.

* Uitgaande internet connectiviteit van de Identity Manager-server naar Azure.

## <a name="requirements"></a>Vereisten
De vereisten voor het gebruik van Hybrid Reporting van Identity Manager worden weer gegeven in de volgende tabel:


|                                         Vereiste                                         |                                                                                                                                                                                                                                                                                    Beschrijving                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        Hybride rapportage is een Azure AD Premium functie en vereist Azure AD Premium. </br>Zie [aan de slag met Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium)voor meer informatie. </br>Ontvang een [gratis proef versie van 30 dagen van Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     U moet een globale beheerder van uw Azure AD zijn                     |                                                                   Standaard kunnen alleen globale beheerders de agents installeren en configureren om aan de slag te gaan, de portal openen en bewerkingen uitvoeren binnen Azure. </br>**Belang rijk**: het account dat u gebruikt wanneer u de agents installeert, moet een werk-of school account zijn. Het mag geen Microsoft-account zijn. Zie [registreren voor Azure als organisatie](https://docs.microsoft.com/azure/active-directory/sign-up-organization)voor meer informatie.                                                                   |
| Hybride agent voor Identity Manager is geïnstalleerd op elke doel server van de identiteits Manager-service |                                                                                                                                                                                                       Voor hybride rapportage moeten de agents worden geïnstalleerd en geconfigureerd op de doel servers om de gegevens te kunnen ontvangen en bewakings-en analyse mogelijkheden te bieden.  </br>                                                                                                                                                                                                       |
|                    Uitgaande verbinding met de Azure-service-eindpunten                     | Tijdens de installatie en runtime moet de agent verbinding hebben met service-eindpunten van Azure. Als uitgaande connectiviteit wordt geblokkeerd door firewalls, moet u ervoor zorgen dat de volgende eind punten worden toegevoegd aan de lijst toegestaan:<ul><li>\*.blob.core.windows.net </li><li>\*. servicebus.windows.net-poort: 5671 </li><li>\*. adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Uitgaande verbindingen op basis van IP-adressen                         |                                                                                                                                                                                                                      Voor IP-adressen op basis van filteren op firewalls, raadpleegt u de [Azure IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 SSL-inspectie voor uitgaand verkeer wordt gefilterd of uitgeschakeld                 |                                                                                                                                                                                                               De agent registratie stap of bewerkingen voor het uploaden van gegevens kunnen mislukken als er een SSL-inspectie of beëindiging voor uitgaand verkeer op de netwerklaag is.                                                                                                                                                                                                                |
|                      Firewall poorten op de server waarop de-agent wordt uitgevoerd                       |                                                                                                                                                                                                          Om te communiceren met de Azure-service-eind punten, moeten de volgende firewall poorten zijn geopend voor de agent:<ul><li>TCP-poort 443</li><li>TCP-poort 5671</li></ul>                                                                                                                                                                                                          |
|          Bepaalde websites toestaan als verbeterde beveiliging van Internet Explorer is ingeschakeld           |                                                                                Als verbeterde beveiliging van Internet Explorer is ingeschakeld, moeten de volgende websites worden toegestaan op de server waarop de agent is geïnstalleerd:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>De Federatie server voor uw organisatie die wordt vertrouwd door Azure Active Directory (bijvoorbeeld <https://sts.contoso.com>).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Identity Manager-rapportage agent installeren in azure AD
Nadat de rapportage agent is geïnstalleerd, worden de gegevens uit Identity Manager-activiteiten geëxporteerd van Identity Manager naar Windows-gebeurtenis logboek. Identity Manager-rapportage agent verwerkt de gebeurtenissen en uploadt deze vervolgens naar Azure. In Azure worden de gebeurtenissen geparseerd, ontsleuteld en gefilterd voor de vereiste rapporten.

1.  Installeer Identity Manager 2016.

2.  Down load Identity Manager-rapportage agent en Ga als volgt te werk:

    a. Meld u aan bij de Azure AD-beheer Portal en selecteer vervolgens **Active Directory**.

    b. Dubbel klik op de map waarvoor u een globale beheerder bent en een Azure AD Premium-abonnement hebt.

    c. Selecteer **configuratie**en down load Reporting agent.

3.  Installeer Reporting agent door het volgende te doen:

    a.  Down load het [bestand MIMHReportingAgentSetup. exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) voor de service Server voor identiteits beheer.

    b.  Voer `MIMHReportingAgentSetup.exe` uit. 

    c.  Voer het installatieprogramma van de agent uit.

    d.  Zorg ervoor dat de service Identity Manager Reporting agent wordt uitgevoerd.

    e.  Start de Identity Manager-service opnieuw.

4.  Controleer of de Reporting agent voor Identity Manager werkt in Azure.

    U kunt rapport gegevens maken met behulp van de portal self-service voor wachtwoord herstel van Identity Manager om het wacht woord van een gebruiker opnieuw in te stellen. Zorg ervoor dat het wacht woord opnieuw is ingesteld en controleer vervolgens of de gegevens worden weer gegeven in de Azure AD-beheer Portal.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Hybride rapporten weer geven in de Azure Portal

1.  Meld u aan bij de [Azure Portal](https://portal.azure.com/) met het account van uw globale beheerder voor de Tenant.

2.  Selecteer **Azure Active Directory**.

3.  Selecteer in de lijst met beschik bare mappen voor uw abonnement de map Tenant.

4.  Selecteer **audit logboeken**.

5.  Zorg ervoor dat de **MIM-service** is geselecteerd in de vervolg keuzelijst **categorie** .

> [!IMPORTANT]
> Het kan enige tijd duren voordat de audit gegevens van Identity Manager in de Azure Portal worden weer gegeven.

## <a name="stop-creating-hybrid-reports"></a>Stoppen met het maken van hybride rapporten
Als u wilt stoppen met het uploaden van rapport controle gegevens van Identity Manager naar Azure AD, verwijdert u hybride Reporting agent. Gebruik het Windows-hulp programma software om hybride rapportage van Identity Manager te verwijderen.

## <a name="windows-events-used-for-hybrid-reporting"></a>Windows-gebeurtenissen die worden gebruikt voor hybride rapportage
Gebeurtenissen die door Identity Manager worden gegenereerd, worden opgeslagen in het Windows-gebeurtenis logboek. U kunt de gebeurtenissen in de **Logboeken** weer geven door **toepassings-en service logboeken** > van het**aanvraag logboek van Identity Manager**te selecteren. Elke aanvraag voor Identity Manager wordt geëxporteerd als een gebeurtenis in het Windows-gebeurtenis logboek in de JSON-structuur. U kunt het resultaat exporteren naar uw SIEM-systeem (Security Information and Event Management).

|Gebeurtenistype|Id|Details van gebeurtenis|
|--------------|------|-----------------|
|Informatie|4121|De identiteits Manager-gebeurtenis gegevens die alle gegevens van de aanvraag bevatten.|
|Informatie|4137|De id-extensie van het Identity Manager-gebeurtenis 4121 als er te veel gegevens zijn voor één gebeurtenis. De koptekst in deze gebeurtenis wordt weer gegeven in de volgende indeling `"Request: <GUID> , message <xxx> out of <xxx>`:.|
