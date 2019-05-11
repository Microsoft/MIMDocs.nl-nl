---
title: Microsoft Identity Manager-licentieverlening en downloads | Microsoft Docs
description: In dit artikel bevat een overzicht van de methoden voor het Microsoft Identity Manager (MIM) 2016, met verwijzingen over het downloaden van de software licensing.
keywords: ''
author: markwahl-msft
ms.author: markwahl-msft
manager: femila
ms.date: 02/25/2019
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: 7c5ab987801c63dca2457025442a35560dca3b86
ms.sourcegitcommit: 225fca802d6b59bdc98d50020b297eb6393f70eb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/26/2019
ms.locfileid: "64518737"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Microsoft Identity Manager 2016-licentieverlening en downloads

In dit artikel bevat een overzicht van de methoden voor het Microsoft Identity Manager (MIM) 2016, met verwijzingen over het downloaden van de software licensing.

## <a name="licensing-mim-for-your-organization"></a>MIM-licentieverlening voor uw organisatie

Microsoft Identity Manager 2016 heeft een licentie op basis van per gebruiker.  De informatie over licenties zijn opgenomen in de productvoorwaarden van de en gerelateerde documenten, waarop kunnen worden gedownload vanaf de [licentievoorwaarden](https://www.microsoft.com/en-us/licensing/product-licensing/products.aspx) pagina.

### <a name="licensing-for-azure-ad-premium-customers"></a>Licenties voor klanten van Azure AD Premium

Microsoft Identity Manager 2016 wordt geleverd bij Azure Active Directory Premium (P1 en P2), die deel uitmaakt van Enterprise Mobility + Security.

Azure AD Premium is beschikbaar via een [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), wordt de [Open Volume License-programma](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx), en de [Cloud Solution Providers](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) programma. Azure en Office 365-abonnees kunnen ook Azure Active Directory Premium P1 en P2 online aanschaffen.  Meer informatie [prijzen van Azure Active Directory](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

### <a name="mim-cals"></a>MIM-CAL 's

Als u nog geen Azure Active Directory Premium-abonnementen voor uw gebruikers, en meer mogelijkheden die MIM niet synchronisatie, een [Client Access License (CAL)](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx) is vereist voor elke gebruiker waarvan de identiteit wordt beheerd in MIM. Als u wilt dat externe gebruikers, zoals zakelijke partners, aannemers externe of klanten — als u toegang tot MIM, kunt u CAL's voor elk van uw externe gebruikers aanschaffen of externe Connector (EG) licenties aanschaft. Microsoft Identity Manager 2016-CAL's zijn niet vereist voor gebruikers wiens identiteit is alleen in de Microsoft Identity Manager-synchronisatieservice en niet wordt beheerd in een andere MIM-onderdeel.

### <a name="licenses-for-platform-components"></a>Licenties voor platformonderdelen

Een Windows Server-licentie is vereist voor software van Microsoft Identity Manager 2016-server gebruiken als een invoegtoepassing voor de Windows Server. En een MIM-implementatie vereist ook installatie van SQL Server.  Windows Server en SQL Server-licenties zijn niet opgenomen in MIM.

## <a name="obtaining-mim-software"></a>Het ophalen van de MIM-software

Voordat u begint met een nieuwe installatie van MIM of een upgrade van een eerdere versie, zorg ervoor dat u hebt de meest recente versies.

Als u een nieuwe installatie begint, moet u voor het downloaden van de installatiebestanden voor elke MIM-onderdelen die relevant is voor uw scenario. Vervolgens eventuele updates voor deze bestanden downloaden en download vervolgens extra onderdelen die afzonderlijk worden gedownload via het Downloadcentrum.


| Scenario | Onderdeel | Vereist voor scenario? | De naam van de DVD ISO-map | Opmerkingen |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Synchronisatie| Synchronisatieservice (met inbegrip van AD-connector) | Ja | `Synchronization Service` | |
| Synchronisatie | PCNS | Nee | `Password Change Notification Service` |  Om te worden geïnstalleerd op domeincontrollers |
| Synchronisatie | Graph-connectors voor LDAP, SQL, Web Services, PowerShell, Lotus Domino | Nee | N/A | Gedistribueerd via het Downloadcentrum |
| Privileged Access Management | MIM-service | Ja | `Service and Portal` | |
| Selfservice | MIM-Service, MIM-Portal | Ja | `Service and Portal` | |
| Selfservice | -Invoegtoepassingen en -extensies | Nee | `Add-ins and extensions` | Om te worden geïnstalleerd op computers van eindgebruikers |
| Selfservice | SCSM-rapportage | Nee | `Data Warehouse Support Scripts` | |
| Selfservice | Agent voor hybride rapportage | Nee | N/A | Gedistribueerd via het Downloadcentrum |
| Selfservice | Taalpakketten | Nee | `LANGUAGE Packs` | |
| Certificaatbeheer | CM | Ja | `Certificate Management` | |
| Certificaatbeheer | CM-Bulkclient | Nee | `CM Bulk Client` | |
| Certificaatbeheer | CM-Client | Nee | `CM Client`  | |
| Certificaatbeheer | CM-App voor Windows | Nee | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Het ophalen van Windows installer-pakketten

De meeste organisaties download voor een nieuwe installatie, de MIM-installatiepakketten van de [Volume Licensing Service Center](https://www.microsoft.com/licensing/servicecenter/default.aspx). 


De DVD ISO-bestand bevat een map voor elk MIM-onderdeel: Synchronisatie-Service, Service en -Portal, enzovoort. Als u de software te installeren op een andere computer van waaruit u het hebt gedownload, zorg ervoor dat het hele ISO-bestand of de map voor het onderdeel kopiëren: alleen niet alleen een MSI-bestand uit een map zonder de rest van de bestanden en submappen van de map kopiëren.

Als u geen toegang met Volume Licensing Service Center hebt, klanten met een juiste developer-abonnement ook MIM 2016 SP1 kunnen downloaden als een ISO-bestand van [Visual Studio mijn voordelen Downloads](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%201&pgroup=).  Zoek naar 'Microsoft Identity Manager 2016 Service Pack 1'.  

Als u geen toegang tot het Volume Licensing Service Center en alleen wilt uitproberen van de MIM-software voor een beperkte periode, kunt u downloaden een [evaluatieversie van MIM 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244). Deze software is niet bedoeld voor gebruik in productieomgevingen en kan niet meer functioneren van 180 dagen na de eerste installatie en kan niet worden bijgewerkt. De evaluatieversie vereist Windows Server 2008 R2, Windows Server 2012 of Windows Server 2012 R2 voor installatie.  Als u niet bekend bent met MIM en leren van de technologie, houd er rekening mee dat alle MIM-scenario's vereist voor Active Directory-domein, een Windows Server en SQL Server aanwezig zijn. Als u geen Windows Server of SQL Server al aanwezig is, kunt u proberen [inrichten van een virtuele machine met SQL Server 2016 en Windows Server 2016](https://azure.microsoft.com/en-us/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### <a name="obtaining-updates"></a>Het ophalen van updates

Na de installatie van MIM van MSI-bestand, moet u naast de vereiste hotfixes installeren.

Controleer de [Identity Manager versiegeschiedenis van release](./reference/version-history.md) voor de meest recente release van update, die een koppeling naar de site voor het downloaden van de installer patch-bestanden bevat.

Om te bepalen welke update-bestanden die nodig zijn, worden deze tabel bevat de onderdelen en de naam van het bijbehorende (MSP)-patchbestand in een update.

| Scenario | Onderdeel | De naam van de DVD ISO-map | Bijbehorende bestandsnaam van de update-patch |
|----------|-----------|-   |-------------------|----------|--------------|
|Synchronisatie| Sync-Service | `Synchronization Service` | `FIMSyncService_x64*.msp` |
| Selfservice | MIM-Service, MIM-Portal | `Service and Portal` | `FIMService_x64*msp` |
| Selfservice | -Invoegtoepassingen en -extensies | `Add-ins and extensions` | `FIMAddinsExtensions*msp` |
| Selfservice | Taalpakketten | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Access management (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Certificaatbeheer | CM |  `Certificate Management` | `FIMCM*.msp` |
| Certificaatbeheer | CM-Bulkclient |  `CM Bulk Client` |`FIMCMBulkClient*msp` |
| Certificaatbeheer | CM-Client | CM-Client |`FIMCMClient*msp` |

Zorg ervoor dat eventuele opmerkingen bij de release lezen gekoppeld met de update voor de installatie van het MSP-bestand.

Updates voor [BHOLD](https://www.microsoft.com/en-us/download/details.aspx?id=55950) niet worden gedistribueerd als MSP-bestanden, alleen als de MSI-installatieprogramma's.

### <a name="additional-downloads"></a>Meer downloads

De volgende downloads mogelijk ook relevant zijn:

- [Agent voor MIM hybride rapportage](https://www.microsoft.com/download/details.aspx?id=55112)

- [Algemene LDAP-Connector, algemene SQL Connector, Graph-Connector, Lotus Domino-Connector, PowerShell-Connector, Web Services-Connector](http://go.microsoft.com/fwlink/?LinkId=717495)

- [Connector voor SharePoint gebruiker profiel Store](https://www.microsoft.com/en-us/download/details.aspx?id=41164)

- Als u nog geen Active Directory-domein en het instellen van een PAM-scenario voor experimenten, Zie de [MIM 2016 SP1 PAM-implementatiescripts](sp1-deployment-scripts.md).

## <a name="next-steps"></a>Volgende stappen

- Leer meer over scenario's die worden aangeboden in [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Lees de [handleiding voor capaciteitsplanning](capacity-planning-guide.md).
- Implementeren van MIM voor een [synchronisatiescenario](microsoft-identity-manager-deploy.md) of de [scenario voor het beheer van bevoegde toegang](./pam/privileged-identity-management-for-active-directory-domain-services.md).

