---
title: De verwerking van de Microsoft Identity Manager-gegevens | Microsoft Docs
description: Begrijpen om te herkennen en rapporteren over de gegevens binnen de omgeving maatregelen nemen de afhandeling van Microsoft Identity Manager-gegevens in de opgegeven systeem op basis van de operationele taken en vereiste.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldiwn
ms.date: 05/22/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: 6bcf9ab26ba38f3c6eefbdb315d4975320a597b9
ms.sourcegitcommit: 66db63fe2813130764e52381f4f9c8e549d77d39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2018
ms.locfileid: "34449697"
---
# <a name="microsoft-identity-manager-data-handling"></a>De verwerking van de Microsoft Identity Manager-gegevens 

In dit artikel vindt u richtlijnen op hoe organisatie kan via de search, verwijderen, bijwerken en rapporteren operations beslissingen voor die uw organisatie moet in de praktijk of in veel verbonden gegevensbronnen te implementeren. Het huidige ontwerp en de configuratie van uw systeem van identity manager (MIM) is essentieel bij het kiezen van een benadering van het verwijderen of bijwerken van een goed begrip. Hieronder vindt u dat enkele scenario's voor klanten moet rekening houden en de volgende vragen beantwoorden: 

- Welke gegevens moet u voor u Identiteitsbeheer voor met een bedrijfsproces?
- Wanneer er gegevens van de huidige moeten worden opgeslagen in MIM
- Hoe wordt gebruikt u deze gegevens in het systeem?
- U wilt deze gegevens delen met een externe partners gegevens sources(Exporting)
- Wat is de gezaghebbende bron voor de gegevens en de verwerking van het?
- Wat wordt uw bewaren van gegevens en gegevens verwijderen plan geïmplementeerd?
- Hebt u de technologie die u wilt verwerken en gegevens beheren geïdentificeerd?

Meer informatie over een huidige MIM-omgeving kunt u het volgende uit om te documenteren van uw MIM-omgeving of delegeren aan uw implementatie Ontwerpdocumenten gebruikmaken.
- [Documentor MIM - kan naar de huidige configuratie exporteren](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Zoeken naar en persoonlijke gegevens worden geïdentificeerd
Zoeken naar gegevens in MIM zijn afhankelijk van de configuratie en installatie. De meeste omgevingen onderling verbonden, maar voor de duidelijkheid we ze heeft overtreden uit door op hoog niveau onderdeel.

### <a name="synchronization-service"></a>Synchronisatieservice

Alle gegevens in MIM die is gekoppeld aan gebruikers is afgeleid van Active Directory (AD) en HR-gegevensbronnen. Bij het zoeken naar persoonlijke gegevens, is voor de eerste plaats waar u rekening met het zoeken naar houden moet AD of verbonden gegevensbronnen. 

Als u niet zeker weet de bron van autorisaties, kunt u deze gebruiker bijhouden van de MIM Synchronization Service Manager-console, klik op de balk Metaverse zoeken om de persoonlijke gegevens worden opgeslagen in de database weer te geven. Gebruikers kunnen zoeken naar een specifieke gebruiker of het kenmerk.

- Om uit te voeren een controle of zoekopdracht in de gegevens van de objecten gebruiker
    - Open de service-client voor synchronisatie
        - Met behulp van de metaverse designer kunt u zien kenmerkstroom importeert en prioriteit.
![Mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Het metaverse zoeken kunt u zoeken op een object en het kenmerk in de database ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Na het vinden van het object wordt te klikken op het object geopend de pagina gebruikersprofiel. De objectdetails biedt u met de uitgebreide informatie over het object, de kenmerken ervan het laatst is gewijzigd en bron van de instantie van en verwante verbonden gegevensbron afgeleid van management agent configuratievoorbeeld hieronder.

![Mim-privacy-naleving. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Service en Portal / PAM
Als u een exemplaar van de Service en Portal of PAM geïnstalleerd wordt is kunnen zoeken naar gebruikers het belangrijk. 

Als u de Portal hebt geïnstalleerd, kunt u de gebruikersinterface kunt gebruiken om te zoeken op een kenmerk of de query voor een bepaalde gebruiker.

Als u alleen de-server (zonder gebruikersinterface Portal) geïnstalleerd hebt kunt u de syntaxis van een zoekopdracht op basis van de [FIMAutomation PSSnapin] uitvoeren voorbeeld gevonden [hier](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

PAM dezelfde bovenstaande syntaxis kunt gebruiken of kunt u de [MIMPAM Module](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specifiek de cmdlet get-pamuser om te zoeken naar de gebruiker binnen de PAM-omgeving.

Andere reporting opties voor het zoeken van beschikbare gegevens, wordt de service en -portal.
- [Hybride rapportage](https://docs.microsoft.com/en-us/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Rapportage met SCSM](https://docs.microsoft.com/en-us/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
Bhold-Core-service heeft een gebruikersinterface waarmee u wilt zoeken voor een gebruiker of kenmerken. 

![bhold zoeken](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Als u BHOLD met synchroniseert [management-connector toegang](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-access-management-connector-install) voor synchronisatieservice kunt u zich kunt zien dat de objecten die de gebruiker verbinding en de kenmerken uw verzenden naar BHOLD-core.

U kunt ook de rapportage van BHOLD-module laden.

- [BHOLD-rapportage](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Certificaatbeheer
Certificate management-service zoeken is ingebouwd in de gebruikersinterface. De beheerder wordt Start en selecteer de 'vinden voor gebruiker en bekijk of beheren van hun gegevens'  

![cm zoeken](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exporteren van persoonlijke gegevens
Omdat de gegevens met betrekking tot entiteiten in MIM is afgeleid van meerdere bronnen, worden de meeste gegevens worden opgeslagen in de database van de synchronisatieservice. Daarom moet u de object-gerelateerde gegevens uit MIM Sync exporteren of u de eigenaar van deze gegevens kunt bepalen.

### <a name="synchronization-service"></a>Synchronisatieservice
Synchronisatieservices voor het exporteren van gegevens gewoon Selecteer de gegevens van de gebruikersinterface van de zoekopdracht en kopiëren en plakken in een CSV-bestand of de gewenste indeling. Deze gegevens exporteren op een andere manier is het maken van een MA op basis van bestanden voor het verwijderen van huidige gegevens die nodig zijn over een gemarkeerde gebruiker van belang. Een voorbeeld van het gebruik van MA op basis van bestanden vindt u [hier](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Service en Portal / PAM
Service en portal samen met PAM kunt u deze gegevens uitvoeren van de syntaxis van een zoekopdracht op basis van de [FIMAutomation PSSnapin] exporteren voorbeeld gevonden [hier](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) en doorgeven aan [csv](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

PAM dezelfde bovenstaande syntaxis kunt gebruiken of kunt u de [MIMPAM Module](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specifiek de get-pamuser zoeken naar de gebruiker binnen de PAM-omgeving en doorgeven aan een csv.

- [Voorbeeld voor het opvragen van de MIM-Service met behulp van PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Bhold-gegevens kunnen met behulp van de module rapporteren aan uw favoriete notatie bhold worden geëxporteerd.

### <a name="certificate-management"></a>Certificaatbeheer
Certificate management gegevens met betrekking tot persoonlijke gegevens is naar active directory verbonden. Een beheerder kan deze gegevens met behulp van Active Directory powershell exporteren.

## <a name="updating-personal-data"></a>Bijwerken van persoonlijke gegevens

Persoonlijke gegevens over gebruikers- of objecten in de MIM-oplossingen wordt doorgaans afgeleid van het gebruikersobject in uw organisatie verbonden gegevensbronnen. Omdat wijzigingen in het gebruikersprofiel in HR-bron- of een ander gezaghebbend systeem van record, zoals AD worden vervolgens doorgevoerd in de MIM-synchronisatieservice.

### <a name="synchronization-service"></a>Synchronisatieservice

Beheerders moeten beheerbewerkingen wilt uitvoeren, deel uitmaken van de synchronisatiebewerkingen of beheerder gedefinieerd [hier](https://docs.microsoft.com/en-us/previous-versions/mim/jj590183(v%3dws.10)).

Bijwerken van gegevens wordt gedaan door het definiëren van regels van de bron van de instantie. Beheerconsole kunt identificeren van de bron van de instantie bij te werken op de bronlocatie. Er is een andere optie synchronisatieregel of extensie regel waarmee de gegevens bijwerken als bron, zoals HR-gegevens nog steeds moet blijven maken. Dit zijn de opties avialible ondersteund.

Zie hieronder voor meer informatie over verschillende manieren om bij te werken kenmerk. 

- [Met behulp van regels-extensies](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Meer informatie over gegevenssynchronisatie met externe systemen](https://docs.microsoft.com/en-us/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Service en Portal / PAM

Service en Portal PAM gegevens kunnen worden bijgewerkt met de FIMAutomation of de PAM-cmdlets. Als u de Portal hebt, kunt u ook rechtstreeks bijwerken door te zoeken en het object niet wijzigen. Één ding opmerking en afhankelijk van configuratie gewoon bijwerken via de portal betekent niet dat blijft. Als de bron van de instantie is sterk afhankelijk is van de algehele configuratie.

### <a name="bhold"></a>BHOLD

Gebruikers kunnen rechtstreeks worden bijgewerkt met Core BHOLD-gebruikersinterface of de access management-connector.

### <a name="certificate-management"></a>Certificaatbeheer

Gebruikers in de certificate management-service zijn een reflectie uit active directory. Gebruik Active Directory om de objectdetails van het wijzigen bijwerken.

## <a name="deleting-personal-data"></a>Verwijderen van persoonlijke gegevens

>[!Note] 
> Dit artikel biedt richtlijnen voor manieren om te verwijderen van persoonlijke gegevens van Microsoft Identity Manager en kan worden gebruikt ter ondersteuning van uw verplichtingen onder de GDPR. Als u algemene informatie over GDPR zoekt, raadpleegt u de [GDPR sectie van de portal Service vertrouwen](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Gegevens in MIM is gesynchroniseerd en altijd vanuit een verbonden gegevensbron zijn bijgewerkt. Wanneer een object in het doel is verwijderd, kunnen de gegevens van het object in MIM ten behoeve van beveiliging onderzoek worden onderhouden. Verwijderen van objecten is geconfigureerd per verbonden gegevens bronregels of regel extension(code) en/of regels voor Object verwijderen.

### <a name="synchronization-service"></a>Synchronisatieservice
Synchronisatie-Service zo veel manieren om te verwerken van gegevens of verwijder gegevens, afhankelijk van bedrijfsprocessen. Om te begrijpen, volgen hieronder enkele artikelen voor meer informatie over opties voor verwijderen en bijwerken van kenmerken: 

- [Meer informatie over ongedaan maken van de inrichting](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Met behulp van regels-extensies](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Aanbevolen procedures voor MIM](https://docs.microsoft.com/en-us/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Service en Portal / PAM

Het wordt aanbevolen voor de Service en Portal dat u de standaard 30 dagen resource bewaren systeemconfiguratie behouden. Hiermee wordt de service schrijfopdrachten wanneer deze worden verwijderd, niet alleen gegevens van aanvragen, maar ook voor elk object dat moet worden gewist uit het systeem. Zodra het proces plaatsvindt, worden alle gegevens die zijn gekoppeld aan dit object is verwijderd dit omvat alle gegevens van de SSPR-registratie. Dit speelt in de bovenstaande voor configuratie van object verwijderen. We hebt één tabel zijn zo bewaren wij de guid van de objecten. Om te beperken van de totale grootte van de tabel in de build 4.4.1459 hebben we een proces genaamd FIM_DeleteExpiredSystemObjectsJob meer informatie over dit proces vindt toegevoegd [hier](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![Mim-privacy-naleving-srrc. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold zoals de meeste systemen die zijn verbonden met de synchronisatieservice kan worden geconfigureerd om te verwijderen eenmaal het bronobject zoals HR wordt verwijderd. Dit is geconfigureerd op de beheeragent. en beheerd door de regels van het verwijderen van objecten zoals omschreven in de functies van de service synchronisaties.

Een andere optie is het verwijderen van het gebruikersobject rechtstreeks vanuit de gebruikersinterface van BHOLD Core. Afhankelijk van het installatieprogramma kan dit goed werken maar houd er rekening mee inrichting logica kan deze gebruiker opnieuw maken als dit niet verwijderd op de bronlocatie.
![Mim-privacy-naleving-bholdr. PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Certificaatbeheer
Als u wilt een gebruiker verwijderen uit CM, zich Verwijder de gebruiker in active directory.

Certificaatbeheer zoals deze wordt alleen opgeslagen voor de uid profiel van certificaatservices met sAMAccountName van domein. Zodra de gebruiker wordt verwijderd uit de cache van de gebruiker is alleen aanwezig voor de certificaten AD rschakelen ze hebt ingeschreven. We raden niet alles in de database verwijderen omdat dit ertoe leiden algemene schade voor de werking van de omgeving dat kan.

## <a name="opt-out-of-telemetry"></a>Opt-out van telemetrie
In vorige builds FIM/MIM gebruikt voor het verzamelt geanonimiseerde telemetriegegevens over elke implementatie en verzendt deze gegevens via HTTPS met Microsoft-servers. Deze gegevens is gebruikt door Microsoft te verbeteren toekomstige versies van FIM/MIM in het verleden.

>[!Note] 
> In latere versies van 4.5.x.x of groter gegevens wordt verzameling uitgeschakeld.

Gegevens uitschakelen verzameling in de vorige versie uitvoeren modus wijzigen en schakel het volgende bericht:

![Mim-privacy-naleving-programma voor kwaliteitsverbetering. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

Bewerk het register en de waarde instelt op 0: (onderdeel) CEIP HKLM\SOFTWARE\Microsoft\Forefront identiteit Manager\2010

![Mim-privacy-naleving-ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Volgende stappen 
- [Voor SQL-gerelateerde privacy richtlijnen](https://docs.microsoft.com/en-us/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [GDPR sectie van de portal Service vertrouwen](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [Archiveren van FIM 2010: Extra - implementatie van Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)