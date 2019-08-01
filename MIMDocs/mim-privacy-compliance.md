---
title: Microsoft Identity Manager gegevens verwerking | Microsoft Docs
description: Meer informatie over het verwerken van Microsoft Identity Manager gegevens voor herkennen en het rapporteren van gegevens binnen de omgeving, het uitvoeren van een actie in een bepaald systeem op basis van operationele functies en vereisten.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 12/02/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: 6f861c5b1984de70a91edcac89276402f289e355
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701484"
---
# <a name="microsoft-identity-manager-data-handling"></a>Verwerking van Microsoft Identity Manager gegevens 

In dit artikel vindt u richt lijnen voor de manier waarop organisaties beslissingen kunnen nemen die kunnen worden toegepast op veel verbonden gegevens bronnen.  Dit kan worden bereikt via de bewerkingen voor zoeken, verwijderen, bijwerken en rapporteren.  Voordat u besluit om te verwijderen of bij te werken, is het belang rijk dat u een goed idee hebt van het huidige ontwerp en de configuratie van uw Identity Manager-systeem (MIM). 

Hieronder volgen enkele scenario's die klanten moeten overwegen en de volgende vragen kunnen beantwoorden: 

- Welke gegevens hebt u nodig voor identiteits beheer om te helpen bij het bedrijfs proces?
- Waar worden huidige gegevens opgeslagen in MIM?
- Hoe gaat u deze gegevens in het systeem gebruiken?
- Deelt u deze gegevens met gegevens bronnen voor externe partners (exporteren)
- Wat is de gezaghebbende bron voor de gegevens en de verwerking ervan?
- Wat gebeurt er met het Bewaar plan voor gegevens en het verwijderen van gegevens?
- Hebt u alle technologie geïdentificeerd die u nodig hebt om gegevens te verwerken en te beheren?

Om u te helpen inzicht te krijgen in een huidige MIM-omgeving, kunt u het volgende hulp programma gebruiken om uw MIM-omgeving te documenteren of uw ontwerp documenten voor de implementatie uit te stellen.
- [MIM-document functie: Hiermee staat u de huidige configuratie toe](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Zoeken naar en identificeren van persoons gegevens
Het zoeken van gegevens binnen MIM is afhankelijk van de configuratie en de installatie. De meeste omgevingen hebben een onderlinge verbinding, maar voor de duidelijkheid zijn deze op hoog niveau afgebroken.

### <a name="synchronization-service"></a>Synchronisatieservice

Alle gegevens in MIM die betrekking hebben op gebruikers, zijn afgeleid van Active Directory (AD) en HR-gegevens bronnen. Wanneer u zoekt naar persoons gegevens, kunt u het beste zoeken in AD of verbonden gegevens bronnen. 

Als u niet zeker weet dat u deze gebruiker kunt volgen vanuit de MIM Synchronization Service Manager-console, klikt u op de omgekeerde zoek balk om de Identificeer bare persoons gegevens weer te geven die zijn opgeslagen in de-data base. Gebruikers kunnen zoeken naar een specifieke gebruiker of een specifiek kenmerk.

- Gegevens van gebruikers objecten controleren of zoeken
    - De client voor de synchronisatie service openen
        - U kunt met behulp van de functie voor inverse-invoer kenmerk stroom importeren en prioriteiten weer geven.
![MIM-privacy-compliance_1. PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Met behulp van de omgekeerde zoek opdracht kunt u zoeken naar een wille keurig object ![en kenmerk in de data base MIM-privacy-compliance_2. png](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Nadat u het object hebt gevonden, wordt de pagina gebruikers profiel geopend wanneer u op het object klikt. De object Details bieden u de uitgebreide details over het object, de kenmerken, het laatst gewijzigde en de bron van de instantie en gerelateerde verbonden gegevens bron afgeleid van de beheer agent configuratie voor beeld hieronder.

![MIM-privacy-naleving. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Service en Portal/PAM
Als er een exemplaar van de service en de portal of PAM-module zijn geïnstalleerd, is het belang rijk dat u gebruikers kunt zoeken. 

Als u de portal hebt geïnstalleerd, kunt u de gebruikers interface gebruiken om te zoeken naar een wille keurig kenmerk of query voor een bepaalde gebruiker.

Als u alleen de service Server (zonder de portal-gebruikers interface) hebt geïnstalleerd, kunt u een zoek syntaxis uitvoeren op basis van de [FIMAutomation PSSnapin], zoals [hier](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx)wordt gevonden.

PAM kan dezelfde syntaxis gebruiken of u kunt de [MIMPAM-module](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) gebruiken met name de Get-pamuser-cmdlet om te zoeken naar de gebruiker in de pam-omgeving.

Andere rapportage opties voor het zoeken naar beschik bare gegevens bevinden zich in de service en de portal.
- [Hybride rapportage](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Rapportage met SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
De Bhold core-service heeft een gebruikers interface waarmee u naar een gebruiker of kenmerken kunt zoeken. 

![bhold zoeken](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Als u BHOLD synchroniseert met [Access Management connector](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) voor synchronisatie service, kunt u de verbonden gebruikers objecten en de kenmerken die uw verzenden naar BHOLD core zien.

U kunt ook de BHOLD-rapportage module laden.

- [BHOLD rapportage](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Certificaatbeheer
Zoek opdracht van de certificaat beheer service is ingebouwd in de gebruikers interface. De beheerder wordt gestart en selecteert de informatie gebruiker zoeken en weer geven of beheren  

![cm zoeken](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exporteren van persoonlijke gegevens
Omdat de gegevens met betrekking tot entiteiten in MIM worden afgeleid van meerdere bronnen, worden de meeste gegevens opgeslagen in de data base van de synchronisatie service. Daarom moet u aan object gerelateerde gegevens van MIM Sync exporteren of kunt u de eigenaar van deze gegevens bepalen.

### <a name="synchronization-service"></a>Synchronisatieservice
Synchronisatie Services voor het exporteren van gegevens selecteert u de gegevens in de zoek GEBRUIKERSINTERFACE en kopieert en plakt u in een CSV-of voorkeurs indeling. Een andere manier om deze gegevens te exporteren, is door een op bestanden gebaseerde MA te maken om de huidige gegevens te verwijderen die nodig zijn voor een gemarkeerde gebruiker van belang. [Hier](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/)vindt u een voor beeld van het gebruik van op bestanden gebaseerde ma.


### <a name="service-and-portal--pam"></a>Service en Portal/PAM
Service en Portal samen met PAM u kunt deze gegevens exporteren een zoek syntaxis uitvoeren op basis van de [FIMAutomation PSSnapin], voor beeld dat [hier](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) is gevonden en deze naar [CSV](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6)door sluizen.

PAM kan dezelfde syntaxis gebruiken of u kunt de [MIMPAM-module](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) gebruiken met name de Get-pamuser om de gebruiker te zoeken in de pam-omgeving en deze naar een CSV te pipeen.

- [Voor beeld van het uitvoeren van Query's in de MIM-service met Power shell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Bhold gegevens kunnen worden geëxporteerd met behulp van de Bhold-rapportage module naar de gewenste indeling.

### <a name="certificate-management"></a>Certificaatbeheer
Certificaat beheer gegevens met betrekking tot persoonlijke gegevens zijn verbonden met Active Directory. Een beheerder kan deze gegevens exporteren met behulp van Active Directory Power shell.

## <a name="updating-personal-data"></a>Het bijwerken van persoonlijke gegevens

Persoonlijke gegevens over gebruikers of objecten in MIM-oplossingen worden doorgaans afgeleid van het object van de gebruiker in de verbonden gegevens bronnen van uw organisatie. Omdat wijzigingen in het gebruikers profiel in de HR-bron of een ander gezaghebbend systeem van record, zoals AD, vervolgens worden weer gegeven in de MIM-synchronisatie service.

### <a name="synchronization-service"></a>Synchronisatieservice

Beheerders moeten deel uitmaken van synchronisatie bewerkingen of de door de [beheerder gedefinieerde definitie](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10))om beheer bewerkingen uit te voeren.

Het bijwerken van gegevens wordt uitgevoerd door regels te definiëren uit de bron van de autoriteit. De beheer console helpt bij het identificeren van de bron van de autoriteit om deze bij de bron bij te werken. Een andere optie is het maken van een synchronisatie regel of regel uitbrei ding voor het beheren van de gegevens die moeten worden bijgewerkt als de bron, zoals HR-gegevens, nog steeds moet blijven. Dit zijn avialible ondersteunde opties.

Zie hieronder voor meer informatie over verschillende manieren om het kenmerk bij te werken. 

- [Regel uitbreidingen gebruiken](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Meer informatie over gegevenssynchronisatie met externe systemen](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Service en Portal/PAM

Service en portal voor het toevoegen van PAM-gegevens kunnen worden bijgewerkt met behulp van de FIMAutomation-of PAM-cmdlets. Als u de portal hebt, kunt u ook rechtstreeks bijwerken door het object te zoeken en te wijzigen. Het is een goed om te noteren en afhankelijk van de configuratie die u gewoon bijwerkt vanuit de portal, heeft dit geen effect. Als bron van de autoriteit is zeer afhankelijk van de algehele configuratie.

### <a name="bhold"></a>BHOLD

Gebruikers kunnen rechtstreeks worden bijgewerkt met de BHOLD-kern gebruikers interface of de toegangs beheer connector.

### <a name="certificate-management"></a>Certificaatbeheer

Gebruikers in de certificaat beheer service zijn allemaal een reflectie van Active Directory. Gebruik Active Directory om de object gegevens te wijzigen.

## <a name="deleting-personal-data"></a>Het verwijderen van persoonlijke gegevens

>[!Note] 
> Dit artikel bevat richt lijnen voor het verwijderen van persoonlijke gegevens uit Microsoft Identity Manager en kan worden gebruikt ter ondersteuning van uw verplichtingen onder het AVG. Zie de [AVG-sectie van Service Trust Portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted) voor algemene informatie over de AVG.

Gegevens in MIM worden gesynchroniseerd en worden altijd bijgewerkt vanuit de gekoppelde gegevens bron. Wanneer een object in het doel wordt verwijderd, kunnen de gegevens van het object in MIM worden onderhouden voor beveiligings onderzoek. Het verwijderen van objecten is geconfigureerd per verbonden gegevens bron regels of regel extensie (code) en/of verwijderings regels voor objecten.

### <a name="synchronization-service"></a>Synchronisatieservice
Synchronisatie service zoveel manieren om gegevens af te handelen of gegevens te verwijderen, afhankelijk van de bedrijfs processen. Hieronder vindt u een aantal artikelen voor meer informatie over de opties voor het verwijderen en bijwerken van kenmerken: 

- [Meer informatie over ongedaan maken van de inrichting](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Regel uitbreidingen gebruiken](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Aanbevolen procedures voor MIM](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Service en Portal/PAM

Het wordt aanbevolen voor de service & Portal dat u de standaard configuratie voor de Bewaar periode van 30 dagen voor systeem bronnen behoudt. Dit vertelt de service wanneer deze wordt verwijderd, niet alleen gegevens aanvragen, maar ook voor objecten die moeten worden gewist uit het systeem. Zodra het proces is uitgevoerd, worden alle gegevens die aan dit object zijn gekoppeld, verwijderd, inclusief alle SSPR registratie gegevens. Hiermee wordt de configuratie voor object verwijdering hierboven afgespeeld. We hebben één tabel die de GUID van de objecten opslaat. Als u de totale grootte van de tabel in Build 4.4.1459 wilt reduceren, kunt u [hier](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden)een proces toevoegen met de naam FIM_DeleteExpiredSystemObjectsJob Details van dit proces.

![MIM-privacy-naleving-srrc. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold als de meeste systemen die zijn verbonden met de synchronisatie service, kunnen worden geconfigureerd om te verwijderen zodra het bron object, zoals HR, wordt verwijderd. Dit wordt geconfigureerd in de beheer agent. en worden beheerd door de regels voor het verwijderen van objecten zoals beschreven onder de functies synchronisaties service.

Een andere mogelijkheid is om het gebruikers object rechtstreeks te verwijderen uit de BHOLD core-gebruikers interface. Afhankelijk van de installatie kan dit goed werken, maar Opmerking inrichten logica kan deze gebruiker opnieuw maken als deze niet wordt verwijderd uit de bron.
![MIM-privacy-naleving-bholdr. PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Certificaatbeheer
Als u een gebruiker uit CM wilt verwijderen, moet u de gebruiker in Active Directory verwijderen.

Met certificaat beheer wordt alleen de profiel-UID van Certificate Services met het domein sAMAccountName opgeslagen. Zodra de gebruiker uit AD is verwijderd, is de gebruikers cache alleen aanwezig voor de certificaten, veren deze zijn Inge schreven. Het is niet raadzaam om niets in de data base te verwijderen, omdat dit kan leiden tot een algehele beschadiging van de werking van de omgeving.

## <a name="opt-out-of-telemetry"></a>Niet-telemetrie
Eerdere versies van FIM/MIM worden gebruikt voor het verzamelen van geanonimiseerd-telemetrie over elke implementatie en verzendt deze gegevens via HTTPS naar micro soft-servers. Deze gegevens zijn door micro soft gebruikt om toekomstige versies van FIM/MIM in het verleden te helpen verbeteren.

>[!Note] 
> In latere releases van versie 4.5. x. x-of meer gegevens verzameling wordt uitgeschakeld.

Als u het verzamelen van gegevens in de vorige versie wilt uitschakelen, voert u de modus wijzigen uit en schakelt u de volgende prompt uit:

![MIM-privacy-naleving-CEIP. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

of bewerk het REGI ster en stel de waarde in op 0: Onderdeel CEIP HKLM\SOFTWARE\Microsoft\Forefront identiteit Manager\2010

![MIM-privacy-naleving-ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Volgende stappen 
- [Voor aan SQL gerelateerde privacy-richt lijnen](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [De sectie AVG van de service Trust-Portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010-archief: Een platform voor het implementeren van Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
