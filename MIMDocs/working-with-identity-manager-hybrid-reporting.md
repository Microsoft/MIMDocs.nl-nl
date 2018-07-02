---
title: Werken met hybride rapportage in Azure met behulp van Identity Manager 2016 | Microsoft Docs
description: Informatie over het combineren van on-premises en cloudgegevens in hybride rapporten in Azure en het beheren en weergeven van deze rapporten.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 2/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: 3c9e8c0fa0a0de3cf9710003d4d7f4ed9c0b03bd
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289642"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Werken met hybride rapportage in Identity Manager

In dit artikel wordt beschreven voor het combineren van on-premises en cloudgegevens in hybride rapporten in Azure en het beheren en weergeven van deze rapporten.

## <a name="available-hybrid-reports"></a>Beschikbare hybride rapporten
De eerste drie Microsoft Identity Manager-rapporten in Azure Active Directory (Azure AD) beschikbaar zijn als volgt:

- **Activiteit voor wachtwoordherstel**: wordt elke instantie weergegeven waarbij een gebruiker wachtwoordherstel met selfservice voor wachtwoordherstel (SSPR) uitgevoerd en biedt de poorten of methoden die worden gebruikt voor verificatie.

- **Registratie voor wachtwoord opnieuw instellen**: wordt weergegeven wanneer een gebruiker zich registreert voor de SSPR en de methoden gebruikt om te verifiëren. Voorbeelden van methoden mogelijk een mobieletelefoonnummer of vragen en antwoorden.
   > [!NOTE]
   > Voor *registratie voor wachtwoord opnieuw instellen* rapporteert, wordt geen onderscheid wordt gemaakt tussen de SMS-gate en de MFA-gate. Beide worden beschouwd als mobiele telefoon methoden.

- **Activiteit voor selfservicegroepen**: elke poging wordt gedaan door iemand toevoegen of verwijderen van zichzelf uit een groep en het maken van de groep wordt weergegeven.

    ![Afbeelding hybride rapportage van Azure - Activiteit wachtwoord opnieuw instellen](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * De rapporten die zich momenteel gegevens van de activiteit van één maand.
> * De vorige hybride rapportage-Agent moet worden verwijderd.
> * Voor het verwijderen van hybride rapporten, verwijdert u de agent mimreportingagent.msi.

## <a name="prerequisites"></a>Vereisten

* Identity Manager 2016 SP1 Identity Manager-service, aanbevolen build [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* Een Azure AD Premium-tenant met een gelicentieerde beheerder in uw directory.

* Uitgaande internetverbinding vanaf de Identity Manager-server naar Azure.

## <a name="requirements"></a>Vereisten
In de volgende tabel vindt u de vereisten voor het gebruik van hybride rapportage van Identity Manager:


|                                         Vereiste                                         |                                                                                                                                                                                                                                                                                    Description                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        Hybride rapportage is een Azure AD Premium-functie en Azure AD Premium vereist. </br>Zie voor meer informatie [aan de slag met Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Ophalen van een [gratis proefperiode van 30 dagen van Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     U moet een globale beheerder van uw Azure AD                     |                                                                   Standaard kunnen alleen globale beheerders kunnen installeren en configureren van de agents aan de slag, toegang tot de portal en bewerkingen uitvoeren in Azure. </br>**Belangrijke**: het account dat u gebruikt wanneer u de agents installeert moet een werk- of schoolaccount-account. Dit mag geen Microsoft-account zijn. Zie voor meer informatie [aanmelden voor Azure als een organisatie](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| Agent voor hybride Identity Manager is geïnstalleerd op elke doelserver Identity Manager-Service |                                                                                                                                                                                                       Voor het ontvangen van de gegevens en bieden mogelijkheden voor bewaking en analyses, moet hybride rapportage de agents worden geïnstalleerd en geconfigureerd op de doelservers.  </br>                                                                                                                                                                                                       |
|                    Uitgaande verbinding met de service-eindpunten van Azure                     | Tijdens de installatie en runtime moet de agent verbinding hebben met service-eindpunten van Azure. Als u uitgaande connectiviteit wordt geblokkeerd door firewalls, zorg ervoor dat de volgende eindpunten zijn toegevoegd aan de lijst met toegestane:<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Uitgaande verbinding op basis van IP-adressen                         |                                                                                                                                                                                                                      Voor IP-gebaseerde filteren op firewalls adressen, verwijzen naar de [Azure IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 SSL-inspectie voor uitgaand verkeer is gefilterd of uitgeschakeld                 |                                                                                                                                                                                                               De agent registratie stap of gegevens uploadbewerkingen mislukken als er SSL-inspectie of beëindiging voor uitgaand verkeer op de netwerklaag.                                                                                                                                                                                                                |
|                      Firewall-poorten op de server waarop de agent wordt uitgevoerd                       |                                                                                                                                                                                                          De agent vereist om te communiceren met de Azure-service-eindpunten, de volgende firewallpoorten moeten open zijn:<ul><li>TCP-poort 443</li><li>TCP-poort 5671</li></ul>                                                                                                                                                                                                          |
|          Toestaan dat bepaalde websites als verbeterde beveiliging van Internet Explorer is ingeschakeld           |                                                                                Als Internet Explorer Verbeterde beveiliging is ingeschakeld, moeten de volgende websites zijn toegestaan op de server waarop de agent geïnstalleerd is:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>De federation-server voor uw organisatie worden vertrouwd door Azure Active Directory (bijvoorbeeld <https://sts.contoso.com>).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Identity Manager-Agent voor rapportage in Azure AD installeren
Nadat de Reporting-Agent is geïnstalleerd, de gegevens van Identity Manager-activiteit is geëxporteerd van Identity Manager naar het Windows-gebeurtenislogboek. Identity Manager-rapportage-Agent verwerkt de gebeurtenissen en geüpload naar Azure. In Azure worden de gebeurtenissen geparseerd, ontsleuteld en gefilterd voor de vereiste rapporten.

1.  Installeer Identity Manager 2016.

2.  Identity Manager-rapportage-Agent downloaden en vervolgens het volgende doen:

    a. Aanmelden bij de Azure AD-beheerportal en selecteer vervolgens **Active Directory**.

    b. Dubbelklik op de map waarvoor u een globale beheerder bent en een Azure AD Premium-abonnement hebt.

    c. Selecteer **configuratie**, en vervolgens de Reporting-Agent downloaden.

3.  Rapportage-Agent installeren als volgt:

    a.  Download de [MIMHReportingAgentSetup.exe bestand](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) voor de server Identity Manager-Service.

    b.  Voer `MIMHReportingAgentSetup.exe` uit. 

    c.  Voer het installatieprogramma van de agent uit.

    d.  Zorg ervoor dat de Agent voor Identity Manager-rapportage-service wordt uitgevoerd.

    e.  Het Identity Manager-service opnieuw starten.

4.  Controleer of de Agent voor Identity Manager-rapportage is werkt in Azure.

    U kunt maken rapport gegevens met behulp van het wachtwoord van Identity Manager Self-service portal om in te stellen van een gebruiker-wachtwoord opnieuw instellen. Zorg ervoor dat het wachtwoord opnieuw instellen is voltooid, en vervolgens controleren om ervoor te zorgen dat de gegevens wordt weergegeven in de Azure AD-beheerportal.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Hybride-rapporten weergeven in de Azure portal

1.  Aanmelden bij de [Azure-portal](https://portal.azure.com/) met uw globale beheerdersaccount voor de tenant.

2.  Selecteer **Azure Active Directory**.

3.  Selecteer de tenantmap in de lijst met beschikbare mappen voor uw abonnement.

4.  Selecteer **controlelogboeken**.

5.  In de **categorie** vervolgkeuze-lijst, zorg ervoor dat **MIM-Service** is geselecteerd.

> [!IMPORTANT]
> Het kan enige tijd duren voor Identity Manager controlegegevens worden weergegeven in de Azure-portal.

## <a name="stop-creating-hybrid-reports"></a>Stoppen met het maken van hybride rapporten
Als u stoppen met het uploaden van audit rapportagegegevens van Identity Manager naar Azure AD wilt, verwijdert u de Agent voor hybride rapportage. Het hulpprogramma Windows verwijderen programma's installeren of verwijderen met hybride rapportage van Identity Manager gebruiken.

## <a name="windows-events-used-for-hybrid-reporting"></a>Windows-gebeurtenissen die worden gebruikt voor hybride rapportage
Gebeurtenissen die worden gegenereerd door Identity Manager worden opgeslagen in de Windows-gebeurtenislogboek. U vindt de gebeurtenissen in de **logboeken** door te selecteren **logboeken toepassingen en Services** > **logboek Identity Manager-aanvragen**. Elke Identity Manager-aanvraag wordt geëxporteerd als een gebeurtenis in het Windows-gebeurtenislogboek in JSON-structuur. U kunt het resultaat exporteren naar uw systeem security information en event management (SIEM).

|Gebeurtenistype|ID|Details van gebeurtenis|
|--------------|------|-----------------|
|Informatie|4121|De Identity Manager-gebeurtenisgegevens met alle aanvraaggegevens.|
|Informatie|4137|Het Identity Manager gebeurtenisextensie 4121, als er te veel gegevens voor één gebeurtenis. De koptekst in deze gebeurtenis wordt weergegeven in de volgende indeling: `"Request: <GUID> , message <xxx> out of <xxx>`.|
