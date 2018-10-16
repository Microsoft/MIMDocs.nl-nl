---
title: Werken met hybride rapportage in Azure met behulp van Identity Manager 2016 | Microsoft Docs
description: Informatie over het combineren van on-premises en cloudgegevens in hybride rapporten in Azure en het beheren en weergeven van deze rapporten.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 2/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: ddccc9acaafe06017178e343d7cd675e3c3f8dec
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333306"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Werken met hybride rapportage in Identity Manager

In dit artikel wordt beschreven hoe combineer on-premises en cloudgegevens in hybride rapporten in Azure, en over het beheren en weergeven van deze rapporten.

## <a name="available-hybrid-reports"></a>Beschikbare hybride rapporten
De eerste drie Microsoft Identity Manager-rapporten beschikbaar zijn in Azure Active Directory (Azure AD) zijn als volgt:

- **Activiteit voor wachtwoord opnieuw instellen**: wordt elke instantie weergegeven als een gebruiker uitgevoerd voor wachtwoord opnieuw instellen met behulp van de selfservice voor wachtwoordherstel (SSPR) en biedt de poorten of methoden die worden gebruikt voor verificatie.

- **Registratie voor wachtwoordherstel**: wordt weergegeven wanneer een gebruiker zich registreert voor SSPR en de methoden gebruikt om te verifiëren. Voorbeelden van methoden zijn een mobieletelefoonnummer of vragen en antwoorden.
   > [!NOTE]
   > Voor *registratie voor wachtwoordherstel* rapporteert, geen onderscheid wordt gemaakt tussen de SMS-gate en de MFA-gate. Beide worden beschouwd als methoden van de mobiele telefoon.

- **Activiteit voor selfservicegroepen**: elke poging wordt gedaan door iemand wilt toevoegen of verwijderen van zichzelf uit een groep en het maken van beveiligingsgroepen worden weergegeven.

    ![Afbeelding hybride rapportage van Azure - Activiteit wachtwoord opnieuw instellen](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * De rapporten die zich momenteel gegevens voor maximaal één maand van de activiteit.
> * De vorige hybride rapportage-Agent moet worden verwijderd.
> * Voor het verwijderen van hybride rapporten, verwijder de agent mimreportingagent.msi.

## <a name="prerequisites"></a>Vereisten

* Identity Manager 2016 SP1 Identity Manager-service, aanbevolen build [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* Een Azure AD Premium-tenant met een gelicentieerde beheerder in uw directory.

* Uitgaande internetverbinding vanaf de Identity Manager-server naar Azure.

## <a name="requirements"></a>Vereisten
De vereisten voor het gebruik van hybride rapportage van Identity Manager worden weergegeven in de volgende tabel:


|                                         Vereiste                                         |                                                                                                                                                                                                                                                                                    Beschrijving                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        Hybride rapportage is een functie van Azure AD Premium en Azure AD Premium vereist. </br>Zie voor meer informatie, [aan de slag met Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Krijgen een [gratis 30-daagse proefversie van Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     U moet een globale beheerder van uw Azure AD                     |                                                                   Standaard kunnen alleen globale beheerders kunnen installeren en configureren van de agenten die aan de slag, toegang tot de portal en bewerkingen uitvoeren in Azure. </br>**Belangrijke**: het account dat u gebruikt wanneer u de agents installeert moet een account voor werk- of schoolaccount. Dit mag geen Microsoft-account zijn. Zie voor meer informatie, [zich registreren voor Azure als een organisatie](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| Agent voor hybride Identity Manager is geïnstalleerd op elke doelserver van Identity Manager-Service |                                                                                                                                                                                                       Voor het ontvangen van de gegevens en bieden mogelijkheden voor bewaking en analyse, moet hybride rapportage de agents worden geïnstalleerd en geconfigureerd op de doelservers.  </br>                                                                                                                                                                                                       |
|                    Uitgaande verbinding met de service-eindpunten van Azure                     | Tijdens de installatie en runtime moet de agent verbinding hebben met service-eindpunten van Azure. Als u uitgaande connectiviteit is geblokkeerd door firewalls, zorg ervoor dat de volgende eindpunten wel zijn toegestaan:<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Uitgaande verbindingen op basis van IP-adressen                         |                                                                                                                                                                                                                      Voor IP-op basis van filteren op firewalls adressen, verwijzen naar de [IP-adresbereiken voor Azure](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 SSL-controle voor uitgaand verkeer is gefilterd of uitgeschakeld                 |                                                                                                                                                                                                               De agent stap of gegevens uploaden registratiebewerkingen kunnen mislukken als er SSL-inspectie of blokkering is voor uitgaand verkeer in de netwerklaag.                                                                                                                                                                                                                |
|                      Firewall-poorten op de server waarop de agent wordt uitgevoerd                       |                                                                                                                                                                                                          De agent vereist om te communiceren met de Azure service-eindpunten, de volgende firewallpoorten moeten open zijn:<ul><li>TCP-poort 443</li><li>TCP-poort 5671</li></ul>                                                                                                                                                                                                          |
|          Toestaan dat bepaalde websites als verbeterde beveiliging van Internet Explorer is ingeschakeld           |                                                                                Als Internet Explorer Verbeterde beveiliging is ingeschakeld, de volgende websites moeten worden toegestaan op de server waarop de agent geïnstalleerd is:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>De federation-server voor uw organisatie wordt vertrouwd door Azure Active Directory (bijvoorbeeld <https://sts.contoso.com>).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Identity Manager-Rapportageagent in Azure AD installeren
Nadat de Rapportageagent is geïnstalleerd, is de gegevens van Identity Manager-activiteit geëxporteerd van Identity Manager naar de Windows-gebeurtenislogboek. Identity Manager-rapportage-Agent de gebeurtenissen worden verwerkt en vervolgens geüpload naar Azure. In Azure worden de gebeurtenissen geparseerd, ontsleuteld en gefilterd voor de vereiste rapporten.

1.  Installeer Identity Manager 2016.

2.  Identity Manager-rapportage-Agent downloaden en vervolgens het volgende doen:

    a. Meld u aan bij de Azure AD-beheerportal en selecteer vervolgens **Active Directory**.

    b. Dubbelklik op de map waarvoor u een globale beheerder bent en een Azure AD Premium-abonnement hebt.

    c. Selecteer **configuratie**, en vervolgens Reporting-Agent downloaden.

3.  Reporting-Agent installeren door het volgende te doen:

    a.  Download de [MIMHReportingAgentSetup.exe bestand](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) voor de server Identity Manager-Service.

    b.  Voer `MIMHReportingAgentSetup.exe` uit. 

    c.  Voer het installatieprogramma van de agent uit.

    d.  Zorg ervoor dat de Agent voor Identity Manager-rapportage-service wordt uitgevoerd.

    e.  De Identity Manager-service opnieuw starten.

4.  Controleren of de Agent voor Identity Manager-rapportage is werkt in Azure.

    U kunt rapport gegevens met behulp van de Identity Manager Self-service voor wachtwoord opnieuw instellen van de portal om het wachtwoord van een gebruiker opnieuw in te maken. Zorg ervoor dat het wachtwoord opnieuw instellen is voltooid en vervolgens controleren om ervoor te zorgen dat de gegevens worden weergegeven in de Azure AD-beheerportal.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Hybride rapporten weergeven in de Azure-portal

1.  Aanmelden bij de [Azure-portal](https://portal.azure.com/) met uw globale beheerdersaccount voor de tenant.

2.  Selecteer **Azure Active Directory**.

3.  Selecteer in de lijst met beschikbare mappen voor uw abonnement, de tenant-directory.

4.  Selecteer **auditlogboeken**.

5.  In de **categorie** vervolgkeuzelijst lijst, zorg ervoor dat **MIM-Service** is geselecteerd.

> [!IMPORTANT]
> Het kan enige tijd duren voor Identity Manager controlegegevens worden weergeven in Azure portal.

## <a name="stop-creating-hybrid-reports"></a>Stoppen met het maken van hybride rapporten
Als u stoppen met het uploaden van controlegegevens van Identity Manager naar Azure AD wilt, verwijdert u de Agent voor hybride rapportage. Gebruik het hulpprogramma Windows verwijderen programma's installeren of verwijderen van de hybride rapportage van Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Windows-gebeurtenissen die worden gebruikt voor hybride rapportage
Gebeurtenissen die worden gegenereerd door Identity Manager worden opgeslagen in de Windows-gebeurtenislogboek. U vindt de gebeurtenissen in de **logboeken** door te selecteren **logboeken toepassingen en Services** > **Identity Manager aanvraag Log**. Elke aanvraag Identity Manager is geëxporteerd als een gebeurtenis in het Windows-gebeurtenislogboek in de JSON-structuur. U kunt de resultaten exporteren naar uw systeem security information en event management (SIEM).

|Gebeurtenistype|ID|Details van gebeurtenis|
|--------------|------|-----------------|
|Informatie|4121|De gegevens van de Identity Manager-gebeurtenis met alle aanvraaggegevens.|
|Informatie|4137|De Identity Manager gebeurtenis 4121 uitbreiding, als er te veel gegevens voor één gebeurtenis. De koptekst in deze gebeurtenis wordt weergegeven in de volgende indeling: `"Request: <GUID> , message <xxx> out of <xxx>`.|
