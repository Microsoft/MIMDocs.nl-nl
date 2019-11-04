---
title: Licenties en down loads Microsoft Identity Manager | Microsoft Docs
description: In dit artikel vindt u een overzicht van de benaderingen voor licentie Microsoft Identity Manager (MIM) 2016, met verwijzingen naar waar de software kan worden gedownload.
keywords: ''
author: markwahl-msft
ms.author: mwahl
manager: femila
ms.date: 10/18/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: e0bfd868345b8e7dcc6a02e745d3ccbf632a6c58
ms.sourcegitcommit: b09a8c93983d9d92ca4871054650b994e9996ecf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2019
ms.locfileid: "73329289"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Microsoft Identity Manager 2016-licenties en-down loads

In dit artikel vindt u een overzicht van de benaderingen voor licentie Microsoft Identity Manager (MIM) 2016, met verwijzingen naar waar de software kan worden gedownload.

## <a name="licensing-mim-for-your-organization"></a>Licentie MIM voor uw organisatie

Microsoft Identity Manager 2016 wordt per gebruiker in licentie gegeven.  De details van licenties zijn opgenomen in de product termen en gerelateerde documenten, die kunnen worden gedownload via de pagina [licentie voorwaarden](https://www.microsoft.com/licensing/product-licensing/products.aspx) .

### <a name="licensing-for-azure-ad-premium-customers"></a>Licentie verlening voor Azure AD Premium klanten

Microsoft Identity Manager 2016 is opgenomen in Azure Active Directory Premium (P1 en P2), die deel uitmaakt van Enterprise Mobility + Security.

Azure AD Premium is beschikbaar via een [micro soft-Enterprise Agreement](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx), het [Open Volume License-programma](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx)en het programma [Cloud Solution Providers](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) . Abonnees van Azure en Office 365 kunnen Azure Active Directory Premium P1 en P2 ook online aanschaffen.  Meer informatie over [Azure Active Directory prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="mim-cals"></a>MIM-Cal's

Als u geen Azure Active Directory Premium-abonnementen voor uw gebruikers hebt en u meer MIM-mogelijkheden dan synchronisatie gebruikt, is een [CAL (Client Access License)](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx) vereist voor elke gebruiker waarvan de identiteit wordt beheerd in mim. Als u wilt dat externe gebruikers, zoals zakelijke partners, externe contract ANTEN of klanten, toegang krijgen tot MIM, kunt u Cal's verkrijgen voor elk van uw externe gebruikers of licenties voor externe connector (EC) aanschaffen. Microsoft Identity Manager 2016 Cal's zijn niet vereist voor gebruikers van wie de identiteit alleen in de Microsoft Identity Manager-synchronisatie service is en wordt niet beheerd in een ander MIM-onderdeel.

### <a name="licenses-for-platform-components"></a>Licenties voor platform onderdelen

Een Windows Server-licentie is vereist voor het gebruik van de server software van Microsoft Identity Manager 2016 als een Windows Server-invoeg toepassing. En een MIM-implementatie vereist ook een SQL Server installatie.  Windows Server-en SQL Server-licenties zijn niet opgenomen in MIM.

## <a name="obtaining-mim-software"></a>MIM-software verkrijgen

Voordat u een nieuwe installatie van MIM of een upgrade van een eerdere versie start, moet u ervoor zorgen dat u de nieuwste versies hebt.

Als u een nieuwe installatie start, moet u de installatie bestanden downloaden voor elk MIM-onderdeel dat relevant is voor uw scenario. Down load vervolgens alle updates voor deze bestanden en down load vervolgens alle extra onderdelen die afzonderlijk worden gedownload via het Download centrum.


| Scenario | Onderdeel | Vereist voor scenario? | Naam van DVD ISO-map | Opmerkingen |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Synchronisatie| Synchronisatie service (inclusief connector naar AD) | Yes | `Synchronization Service` | |
| Synchronisatie | PCNS | Nee | `Password Change Notification Service` |  Te worden geïnstalleerd op domein controllers |
| Synchronisatie | Connectors voor LDAP, SQL, Web Services, Power shell, Lotus Domino, Graph | Nee | N/A | Gedistribueerd via download centrum |
| Privileged Access Management | MIM-service | Yes | `Service and Portal` | |
| Selfservice | MIM-service, MIM-Portal | Yes | `Service and Portal` | |
| Selfservice | Invoeg toepassingen en extensies | Nee | `Add-ins and extensions` | Geïnstalleerd op computers van eind gebruikers |
| Selfservice | SCSM rapportage | Nee | `Data Warehouse Support Scripts` | |
| Selfservice | Hybrid Reporting agent | Nee | N/A | Gedistribueerd via download centrum |
| Selfservice | Taal pakketten | Nee | `LANGUAGE Packs` | |
| Certificaatbeheer | BEDRAAGT | Yes | `Certificate Management` | |
| Certificaatbeheer | CM bulk client | Nee | `CM Bulk Client` | |
| Certificaatbeheer | CM Client | Nee | `CM Client`  | |
| Certificaatbeheer | CM-app voor Windows | Nee | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Windows Installer-pakketten verkrijgen

Voor een nieuwe installatie downloadt de meeste organisaties de MIM-installatie pakketten van het [Volume Licensing Service Center](https://www.microsoft.com/licensing/servicecenter/default.aspx). 


Het ISO-bestand van de DVD bevat één map voor elk MIM-onderdeel: de synchronisatie service, de service en de portal, enzovoort. Als u de software wilt installeren op een andere computer dan waar u deze hebt gedownload, kopieert u het hele ISO-bestand of de map voor het onderdeel: Kopieer alleen maar een MSI-bestand uit een map zonder de rest van de bestanden en submappen.

Als u geen toegang hebt tot het Volume Licensing Service Center, kunnen klanten met een geschikt ontwikkelaars abonnement ook MIM 2016 SP2 downloaden als een ISO-bestand van [de Visual Studio mijn voor delen-down loads](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202&pgroup=).  Zoek naar ' Microsoft Identity Manager 2016 met Service Pack 2 '.  

Als u geen toegang hebt tot het service centrum voor volume licenties en u de MIM-software gedurende een beperkte periode wilt uitproberen, kunt u een [evaluatie versie van MIM 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244)downloaden. Deze software is niet bedoeld voor gebruik in productie omgevingen en zal niet langer dan 180 dagen na de eerste installatie worden uitgevoerd en kan niet worden bijgewerkt. Voor de evaluatie versie is Windows Server 2008 R2, Windows Server 2012 of Windows Server 2012 R2 voor installatie vereist.  Als u niet bekend bent met MIM en de technologie wilt leren gebruiken, moet u er rekening mee houdt dat voor alle MIM-scenario's een Active Directory domein, een Windows-Server en SQL Server aanwezig moeten zijn. Als u Windows Server of SQL Server al niet hebt, kunt u proberen om [een VM in te richten met SQL Server 2016 en Windows Server 2016](https://azure.microsoft.com/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### <a name="obtaining-updates"></a>Updates verkrijgen

Na de installatie van MIM vanuit MSI, moet u eerst de benodigde hotfixes installeren.

Controleer de [release geschiedenis van de versie Manager](./reference/version-history.md) voor de meest recente update versie, die een koppeling bevat naar de download site voor de Installer-patch bestanden.

Om te bepalen welke update bestanden nodig zijn, bevat deze tabel de onderdelen en de naam van het bijbehorende patch bestand (MSP) in een update.

| Scenario | Onderdeel | Naam van DVD ISO-map | Bestands naam van de bijbehorende update patch |
|----------|-----------|-   |-------------------|----------|--------------|
|Synchronisatie| Synchronisatie service | `Synchronization Service` | `MIMSyncService_x64*.msp` |
| Selfservice | MIM-service, MIM-Portal | `Service and Portal` | `MIMService_x64*msp` |
| Selfservice | Invoeg toepassingen en extensies | `Add-ins and extensions` | `MIMAddinsExtensions*msp` |
| Selfservice | Taal pakketten | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Toegangs beheer (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Certificaatbeheer | BEDRAAGT |  `Certificate Management` | `MIMCM*.msp` |
| Certificaatbeheer | CM bulk client |  `CM Bulk Client` |`MIMCMBulkClient*msp` |
| Certificaatbeheer | CM Client | `CM Client` |`MIMCMClient*msp` |

Lees vóór de installatie van het MSP-bestand alle release opmerkingen bij de update.

Updates voor [BHOLD](https://www.microsoft.com/download/details.aspx?id=55950) worden niet gedistribueerd als msp-bestanden, alleen als MSI-installatie Programma's.

### <a name="additional-downloads"></a>Aanvullende down loads

De volgende down loads kunnen ook relevant zijn:

- [MIM Hybrid Reporting agent](https://www.microsoft.com/download/details.aspx?id=55112)

- [Algemene LDAP-connector, algemene SQL-connector, Graph connector, Lotus Domino-connector, Power shell-connector, Web Services-connector](http://go.microsoft.com/fwlink/?LinkId=717495)

- [Connector voor share point-gebruikers profiel archief](https://www.microsoft.com/download/details.aspx?id=41164)

- Als u nog geen Active Directory domein hebt en een PAM-scenario instelt voor experimenten, raadpleegt u de hand Schriften voor de [pam-implementatie van MIM 2016 SP1](sp1-deployment-scripts.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de scenario's die worden geleverd in [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Lees de [hand leiding voor capaciteits planning](capacity-planning-guide.md).
- Implementeer MIM voor een [synchronisatie scenario](microsoft-identity-manager-deploy.md) of het [scenario privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

