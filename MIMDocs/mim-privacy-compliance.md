---
title: Verwerking van gegevens van Microsoft Identity Manager | Microsoft Docs
description: Informatie over Microsoft Identity Manager-gegevens verwerken om te herkennen en rapporteren van gegevens binnen de omgeving, actie ondernemen in de opgegeven systeem op basis van operationele taken en vereiste.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 12/02/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: f75eb69360852c9f629b60d4900638c8b51e068a
ms.sourcegitcommit: 9e420840815adb133ac014a8694de9af4d307815
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/03/2018
ms.locfileid: "52825787"
---
# <a name="microsoft-identity-manager-data-handling"></a>Verwerking van gegevens van Microsoft Identity Manager 

In dit artikel vindt u richtlijnen voor hoe organisaties de beslissingen die kunnen worden toegepast op veel verbonden gegevensbronnen kunnen maken.  Dit kan worden bereikt via het zoeken, verwijderen, bijwerken en bewerkingen.  Voordat u besluit uw benadering van het verwijderen of bijwerken, is een goed begrip van het huidige ontwerp en de configuratie van het systeem in uw identity manager (MIM) essentieel. 

Hieronder vindt u dat enkele scenario's-klanten hoeven te bespreken en de volgende vragen beantwoorden: 

- Welke gegevens moet u voor u Identiteitsbeheer met een bedrijfsproces?
- Waar gaat actuele gegevens worden opgeslagen in MIM?
- Hoe wordt gebruikt u deze gegevens in het systeem?
- Deelt u deze gegevens met een externe partners gegevens sources(Exporting)
- Wat is de gezaghebbende bron voor de gegevens en de verwerking van?
- Wat wordt uw bewaren van gegevens en het verwijderen van gegevens plannen in plaats?
- Hebt u geïdentificeerd met de technologie die u nodig hebt om te verwerken en beheren van gegevens?

U kunt het volgende uit om te documenteren van uw MIM-omgeving of delegeren aan uw implementatie Ontwerpdocumenten gebruiken zodat u inzicht in de huidige MIM-omgeving.
- [MIM Documentor - kunnen aan de huidige configuratie exporteren](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Zoeken naar en het identificeren van persoonlijke gegevens
Zoeken naar gegevens in MIM zijn afhankelijk van de configuratie en installatie. De meeste omgevingen onderling verbonden zijn, maar voor de duidelijkheid we ze heeft overtreden uit door op hoog niveau onderdeel.

### <a name="synchronization-service"></a>Synchronisatieservice

Alle gegevens in MIM die is gekoppeld aan gebruikers wordt afgeleid van Active Directory (AD) en HR-gegevensbronnen. Bij het zoeken naar persoonlijke gegevens, is de eerste plaats waar u rekening met het zoeken naar houden moet AD of verbonden gegevensbronnen. 

Als u niet zeker weet de bron van de instantie kunt u deze gebruiker bijhouden van de MIM Synchronization Service Manager-console, klik op de balk Metaverse zoeken om de persoonlijke gegevens worden opgeslagen in de database weer te geven. Gebruikers kunnen zoeken voor een specifieke gebruiker of het kenmerk.

- Om uit te voeren een controle of zoekopdracht van gegevens van de objecten gebruiker
    - Open de service-client voor synchronisatie
        - Met behulp van de metaverse designer kunt u om te zien van de kenmerkstroom worden geïmporteerd en prioriteit.
![Mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Met behulp van de metaverse search kunt u zoeken op een object en het kenmerk in de database ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Nadat het object zoeken, wordt te klikken op het object geopend de gebruikersprofielpagina. De objectdetails van het biedt u met de uitgebreide informatie over het object, de kenmerken ervan, het laatst is gewijzigd en autoriteitsbron en gerelateerde verbonden gegevensbron afgeleid van management agent configuratievoorbeeld hieronder.

![Mim-privacy-naleving. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Service en -Portal / PAM
Als u een exemplaar van de Service en Portal of de PAM wordt geïnstalleerd is kunnen zoeken naar gebruikers het belangrijk. 

Als u de Portal hebt geïnstalleerd, kunt u de gebruikersinterface om op een kenmerk of de query voor een bepaalde gebruiker te zoeken.

Als u alleen de-server (zonder de gebruikersinterface van Portal) geïnstalleerd hebt u een search-syntaxis op basis van de [FIMAutomation PSSnapin] kunt uitvoeren, voorbeeld gevonden [hier](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

PAM bovenstaande dezelfde syntaxis kunt gebruiken of kunt u de [MIMPAM Module](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specifiek de cmdlet get-pamuser om te zoeken naar de gebruiker in de PAM-omgeving.

Er is een andere reporting opties om te zoeken naar beschikbare gegevens in de service en -portal.
- [Hybride rapportage](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Rapportage met SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
Bhold-Core-service heeft een gebruikersinterface waarmee u wilt zoeken voor een gebruiker of kenmerken. 

![bhold zoeken](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Als u met BHOLD met synchroniseert [toegang management-connector](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) voor synchronisatieservice kunt u zich om te zien dat de objecten gebruiker verbinding en de kenmerken uw verzenden naar BHOLD-core.

U kunt ook de rapportage van BHOLD-module laden.

- [BHOLD-rapportage](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Certificaatbeheer
Zoeken in Certificate management-service is ingebouwd in de gebruikersinterface. De beheerder wordt starten en selecteer de 'gebruiker en de weergave of het beheren van gegevens'  

![cm zoeken](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exporteren van persoonlijke gegevens
Omdat de gegevens die betrekking hebben op entiteiten in MIM is afgeleid van meerdere bronnen, worden de meeste gegevens worden opgeslagen in de Synchronization Service-database. Daarom moet u de object-gerelateerde gegevens exporteren uit MIM Sync of kunt u de eigenaar van deze gegevens bepalen.

### <a name="synchronization-service"></a>Synchronisatieservice
Synchronisatieservices voor het exporteren van gegevens selecteert u de gegevens van de gebruikersinterface van de zoekopdracht en kopieer en plak in een CSV- of een door u gewenste indeling. Een andere manier om deze gegevens te exporteren is het maken van een MA op basis van bestanden om te verwijderen van huidige gegevens die nodig zijn over een gemarkeerde gebruiker van belang zijn. Een voorbeeld van het gebruik van MA op basis van bestanden vindt [hier](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Service en -Portal / PAM
Service en portal samen met PAM kunt u deze gegevens uitvoert een zoeksyntaxis op basis van de [FIMAutomation PSSnapin] exporteren, voorbeeld gevonden [hier](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) en doorgeven aan [csv](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

PAM bovenstaande dezelfde syntaxis kunt gebruiken of kunt u de [MIMPAM Module](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specifiek de get-pamuser om te zoeken voor de gebruiker in de PAM-omgeving en deze doorgeven aan een csv.

- [Voorbeeld van query's van de MIM-Service met behulp van PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Bhold-gegevens kunnen worden geëxporteerd met behulp van de bhold module rapporteren aan de door u gewenste indeling.

### <a name="certificate-management"></a>Certificaatbeheer
Certificate management-gegevens met betrekking tot persoonlijke gegevens is verbonden met active directory. Een beheerder kan deze gegevens met behulp van powershell voor Active Directory kunt exporteren.

## <a name="updating-personal-data"></a>Het bijwerken van persoonlijke gegevens

Persoonlijke gegevens over gebruikers- of objecten in de MIM-oplossingen is gewoonlijk afgeleid van het gebruikersobject in de verbonden gegevensbronnen van uw organisatie. Omdat alle wijzigingen aan het gebruikersprofiel in de HR-bron- of een andere gezaghebbende systeem van records, zoals AD worden vervolgens doorgevoerd in de MIM-synchronisatieservice.

### <a name="synchronization-service"></a>Synchronisatieservice

Beheerbewerkingen wilt uitvoeren, beheerders moeten deel uitmaken van synchronisatiebewerkingen of beheerder gedefinieerd [hier](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10)).

Bijwerken van gegevens wordt gedaan door het definiëren van regels uit de bron van de instantie. Beheerconsole helpt bij het identificeren van de bron van de instantie bij te werken op de bronlocatie. Een andere optie is synchronisatieregel of de extensie van de regel voor het beheren van de gegevens bijwerken als bron, zoals HR-gegevens nog steeds nodig heeft om te blijven maken. Dit zijn de opties voor avialible ondersteund.

Zie hieronder voor meer informatie over verschillende manieren om bij te werken kenmerk. 

- [Met behulp van Regeluitbreidingen](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Meer informatie over gegevenssynchronisatie met externe systemen](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Service en -Portal / PAM

Service en Portal voor het opnemen van PAM-gegevens kunnen worden bijgewerkt met de FIMAutomation of de PAM-cmdlets. Als u de Portal hebt, kunt u ook rechtstreeks bijwerken door te zoeken naar en het object wijzigen. Eén ding opmerking en, afhankelijk van de configuratie bij te werken vanuit de portal betekent niet dat blijft. Als de bron van de instantie is sterk afhankelijk van de algehele-configuratie.

### <a name="bhold"></a>BHOLD

Gebruikers kunnen rechtstreeks worden bijgewerkt met de gebruikersinterface van BHOLD-Core of de toegang management-connector.

### <a name="certificate-management"></a>Certificaatbeheer

Gebruikers in de certificate management-service zijn een weerspiegeling van active directory. Gebruik Active Directory voor het wijzigen van objectdetails bijwerken.

## <a name="deleting-personal-data"></a>Het verwijderen van persoonlijke gegevens

>[!Note] 
> In dit artikel bevat richtlijnen voor manieren om te verwijderen van persoonlijke gegevens van Microsoft Identity Manager en kan worden gebruikt voor de ondersteuning van uw verplichtingen onder de AVG. Zie de [AVG-sectie van Service Trust Portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted) voor algemene informatie over de AVG.

Gegevens in MIM is gesynchroniseerd en altijd bijgewerkt van de verbonden gegevensbron. Wanneer een object in het doel wordt verwijderd, kunnen de gegevens van het object in MIM ten behoeve van beveiligingsonderzoek worden beheerd. Verwijderen van objecten is geconfigureerd per met elkaar verbonden gegevensbron regels of regel extension(code) en/of regels voor Object verwijderen.

### <a name="synchronization-service"></a>Synchronisatieservice
Synchronisatie-Service zo veel manieren om te verwerken van gegevens of verwijder gegevens, afhankelijk van bedrijfsprocessen. Om te begrijpen, vindt hieronder u enkele artikelen voor meer informatie over opties voor verwijderen en bijwerken van kenmerken: 

- [Meer informatie over ongedaan maken van de inrichting](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Met behulp van Regeluitbreidingen](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Aanbevolen procedures voor MIM](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Service en -Portal / PAM

Het is raadzaam voor de Service en Portal dat u de standaard 30 dagen configuratie van systeembronbehoud houden. Hiermee wordt de service aangegeven wanneer deze wordt verwijderd, niet alleen gegevens van aanvragen, maar ook een willekeurig object dat moet worden gewist van het systeem. Zodra het proces optreedt, worden alle gegevens die zijn gekoppeld aan dit object is verwijderd dit omvat alle gegevens voor SSPR-registratie. Dit speelt in de bovenstaande voor configuratie van object verwijderen. We hebben wel één tabel zijn we slaan de guid van de objecten. Om te beperken van de totale grootte van de tabel in de build 4.4.1459 er is een proces genaamd FIM_DeleteExpiredSystemObjectsJob meer informatie over dit proces vindt toegevoegd [hier](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![Mim-privacy-naleving-srrc. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold, zoals de meeste systemen die zijn verbonden met de synchronisatieservice kan worden geconfigureerd als u wilt verwijderen nadat het bronobject zoals HR wordt verwijderd. Dit is geconfigureerd op de beheeragent. en beheerd door de regels van het verwijderen van objecten zoals beschreven onder de servicefuncties synchronisaties.

Een andere optie is om te verwijderen van het gebruikersobject rechtstreeks vanuit de gebruikersinterface van BHOLD Core. Afhankelijk van instellingen kan dit werkt goed, maar houd er rekening mee inrichting logica kan deze gebruiker opnieuw maken als dit niet verwijderd op de bronlocatie.
![Mim-privacy-naleving-bholdr. PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Certificaatbeheer
Als u wilt verwijderen van een gebruiker van CM, zich Verwijder de gebruiker in active directory.

Certificaatbeheer zoals deze wordt alleen de uid profiel van certificaatservices met domein sAMAccountName opslaan. Zodra de gebruiker wordt verwijderd uit de cache van de gebruiker geldt alleen voor de certificaten AD veren die ze hebben ingeschreven. We raden niet alles in de database te verwijderen, dit leiden algemene schade voor de werking van de omgeving tot kan.

## <a name="opt-out-of-telemetry"></a>Opt-out van telemetrie
Vorige builds FIM/MIM gebruikt voor het verzamelt geanonimiseerde telemetriegegevens over elke implementatie en verzendt deze gegevens via HTTPS met Microsoft-servers. Deze gegevens is door Microsoft gebruikt voor het verbeteren van toekomstige versies van FIM/MIM in het verleden.

>[!Note] 
> In latere releases van 4.5.x.x of meer gegevens wordt verzameling uitgeschakeld.

Om uit te schakelen van gegevens verzameling in de vorige versie uitvoeren modus wijzigen en schakel het volgende bericht:

![Mim-privacy-naleving-programma voor kwaliteitsverbetering. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

of het register te bewerken en de waarde instelt op 0: (onderdeel) programma voor Kwaliteitsverbetering HKLM\SOFTWARE\Microsoft\Forefront identiteit Manager\2010

![Mim-privacy-naleving-ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Volgende stappen 
- [Voor SQL-gerelateerde privacy richtlijnen](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [GDPR-sectie van de Service Trust-portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [Archiveren van FIM 2010: Ramp Up - implementatie van Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
