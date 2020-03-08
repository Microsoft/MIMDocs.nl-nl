---
title: Algemene LDAP-connector | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de algemene LDAP-connector van micro soft configureert.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.reviewer: davidste
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 06/26/2018
ms.author: billmath
ms.openlocfilehash: b8ed1b605b0a3c01ffd69a329f26583f1cb0a019
ms.sourcegitcommit: d98a76d933d4d7ecb02c72c30d57abe3e7f5d015
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853604"
---
# <a name="generic-ldap-connector-technical-reference"></a>Algemene technische Naslag informatie over LDAP-connectors
In dit artikel wordt de algemene LDAP-connector beschreven. Het artikel is van toepassing op de volgende producten:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * U moet hotfix 4.1.3671.0 of hoger [KB3092178](https://support.microsoft.com/kb/3092178)gebruiken.

Voor MIM2016 en FIM2010R2 is de connector beschikbaar als down load vanuit het [micro soft Download centrum](http://go.microsoft.com/fwlink/?LinkId=717495).

Als u verwijst naar IETF Rfc's, gebruikt dit document de indeling (RFC [RFC number]/[sectie in RFC-document]), bijvoorbeeld (RFC 4512/4.3).
U kunt meer informatie vinden op [https://tools.ietf.org/](https://tools.ietf.org/). Voer in het linkerdeel venster een RFC-nummer in het dialoog venster **documenten ophalen** in en test het om te controleren of dit geldig is.

## <a name="overview-of-the-generic-ldap-connector"></a>Overzicht van de algemene LDAP-connector
Met de algemene LDAP-connector kunt u de synchronisatie service integreren met een LDAP v3-server.

Bepaalde bewerkingen en schema-elementen, zoals de onderdelen die nodig zijn om Delta-import uit te voeren, zijn niet opgegeven in de IETF-Rfc's. Voor deze bewerkingen worden alleen expliciet opgegeven LDAP-directory's ondersteund.

Om verbinding te maken met de directory's, testen we met het account root/admin.  Als u een ander account wilt gebruiken om nauw keurige machtigingen toe te passen, moet u mogelijk het team van uw LDAP-Directory raadplegen.

Van een perspectief op hoog niveau worden de volgende functies ondersteund door de huidige versie van de connector:

| Onderdeel | Support |
| --- | --- |
| Verbonden gegevens bron |De connector wordt ondersteund met alle LDAP v3-servers (RFC 4510-compatibel). Het is getest met het volgende: <li>Micro soft Active Directory Lightweight Directory Services (AD LDS)</li><li>Micro soft Active Directory Global Catalog (AD GC)</li><li>389-adreslijst server</li><li>Apache-adreslijst server</li><li>IBM Tivoli DS</li><li>Isode Directory</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Open DJ</li><li>Open DS</li><li>Open LDAP (openldap.org)</li><li>Oracle (voorheen Sun) Directory server Enter prise Edition</li><li>RadiantOne Virtual Directory server (VDS)</li><li>Sun One-Directory server</li><li>Micro soft Active Directory Domain Services (AD DS)</li><ul><li>Voor de meeste scenario's moet u de ingebouwde Active Directory connector gebruiken in plaats van dat sommige functies niet werken</li></ul>**Bekende mappen of functies die niet worden ondersteund:**<li>Micro soft Active Directory Domain Services (AD DS)<ul><li>Meldings service voor wachtwoord wijzigingen (PCNS)</li><li>Exchange-inrichting</li><li>Verwijdering van actieve synchronisatie apparaten</li><li>Ondersteuning voor nTDescurityDescriptor</li></ul></li><li>Oracle Internet Directory (OID)</li> |
| Scenario's |<li>Beheer van object levenscyclus</li><li>Groeps beheer</li><li>Wachtwoord beheer</li> |
| Operationele activiteiten |De volgende bewerkingen worden ondersteund op alle LDAP-directory's: <li>Volledig importbewerking</li><li>Exporteren</li>De volgende bewerkingen worden alleen ondersteund in de opgegeven directory's:<li>Delta-import</li><li>Wacht woord instellen, wacht woord wijzigen</li> |
| Schema |<li>Het schema wordt gedetecteerd vanuit het LDAP-schema (RFC3673 en RFC4512/4.2)</li><li>Ondersteunt structurele klassen, aux-klassen en extensibleObject-object klasse (RFC4512/4.3)</li> |

### <a name="delta-import-and-password-management-support"></a>Ondersteuning voor Delta-import en wachtwoord beheer
Ondersteunde mappen voor Delta-import-en wachtwoord beheer:

* Micro soft Active Directory Lightweight Directory Services (AD LDS)
  * Ondersteunt alle bewerkingen voor het importeren van verschillen
  * Biedt ondersteuning voor wacht woord instellen
* Micro soft Active Directory Global Catalog (AD GC)
  * Ondersteunt alle bewerkingen voor het importeren van verschillen
  * Biedt ondersteuning voor wacht woord instellen
* 389-adreslijst server
  * Ondersteunt alle bewerkingen voor het importeren van verschillen
  * Ondersteunt wacht woord instellen en wacht woord wijzigen
* Apache-adreslijst server
  * Biedt geen ondersteuning voor het importeren van verschillen omdat deze directory geen persistent wijzigingen logboek heeft
  * Biedt ondersteuning voor wacht woord instellen
* IBM Tivoli DS
  * Ondersteunt alle bewerkingen voor het importeren van verschillen
  * Ondersteunt wacht woord instellen en wacht woord wijzigen
* Isode Directory
  * Ondersteunt alle bewerkingen voor het importeren van verschillen
  * Ondersteunt wacht woord instellen en wacht woord wijzigen
* Novell eDirectory en NetIQ eDirectory
  * Ondersteunt add-, update-en Rename-bewerkingen voor Delta-import
  * Biedt geen ondersteuning voor het verwijderen van een Delta-import bewerking
  * Ondersteunt wacht woord instellen en wacht woord wijzigen
* Open DJ
  * Ondersteunt alle bewerkingen voor het importeren van verschillen
  * Ondersteunt wacht woord instellen en wacht woord wijzigen
* Open DS
  * Ondersteunt alle bewerkingen voor het importeren van verschillen
  * Ondersteunt wacht woord instellen en wacht woord wijzigen
* Open LDAP (openldap.org)
  * Ondersteunt alle bewerkingen voor het importeren van verschillen
  * Biedt ondersteuning voor wacht woord instellen
  * Biedt geen ondersteuning voor het wijzigen van wacht woorden
* Oracle (voorheen Sun) Directory server Enter prise Edition
  * Ondersteunt alle bewerkingen voor het importeren van verschillen
  * Ondersteunt wacht woord instellen en wacht woord wijzigen
* RadiantOne Virtual Directory server (VDS)
  * Moet versie 7.1.1 of hoger gebruiken
  * Ondersteunt alle bewerkingen voor het importeren van verschillen
  * Ondersteunt wacht woord instellen en wacht woord wijzigen
* Sun One-Directory server
  * Ondersteunt alle bewerkingen voor het importeren van verschillen
  * Ondersteunt wacht woord instellen en wacht woord wijzigen

### <a name="prerequisites"></a>Vereisten
Voordat u de connector gebruikt, moet u controleren of u over het volgende beschikt op de synchronisatie server:

* Microsoft .NET 4.5.2-Framework of hoger

### <a name="detecting-the-ldap-server"></a>De LDAP-server detecteren
De connector is afhankelijk van verschillende technieken voor het detecteren en identificeren van de LDAP-server. De connector gebruikt de hoofd-DSE, de naam van de leverancier/versie en het schema wordt gecontroleerd om unieke objecten en kenmerken te vinden die in bepaalde LDAP-servers aanwezig zijn. Deze gegevens, indien gevonden, worden gebruikt om vooraf de configuratie opties in de connector in te vullen.

### <a name="connected-data-source-permissions"></a>Machtigingen voor verbonden gegevens bron
Voor het uitvoeren van import-en export bewerkingen voor de objecten in de verbonden Directory moet het Connector account voldoende machtigingen hebben. De connector heeft schrijf machtigingen nodig om te kunnen exporteren en lees machtigingen om te kunnen importeren. De machtigings configuratie wordt uitgevoerd in de beheer ervaring van de doel directory zelf.

### <a name="ports-and-protocols"></a>Poorten en protocollen
De connector gebruikt het poort nummer dat is opgegeven in de configuratie. Dit is standaard 389 voor LDAP en 636 voor LDAPS.

Voor LDAPS moet u SSL 3,0 of TLS gebruiken. SSL 2,0 wordt niet ondersteund en kan niet worden geactiveerd.

### <a name="required-controls-and-features"></a>Vereiste besturings elementen en functies
De volgende LDAP-besturings elementen/-functies moeten beschikbaar zijn op de LDAP-server om de connector goed te laten werken:  
`1.3.6.1.4.1.4203.1.5.3` waar/onwaar-filters

Het filter waar/onwaar wordt vaak niet gerapporteerd als ondersteund door LDAP-directory's en kan worden weer gegeven op de **pagina Global** onder **verplichte functies die niet zijn gevonden**. Het wordt gebruikt voor het maken **of** filteren van LDAP-query's, bijvoorbeeld bij het importeren van meerdere object typen. Als u meer dan één object type kunt importeren, ondersteunt uw LDAP-server deze functie.

Als u een map gebruikt waarin een unieke id het anker is, moet u ook de volgende gegevens beschikbaar hebben (Zie de sectie [ankers configureren](#configure-anchors) voor meer informatie):  
`1.3.6.1.4.1.4203.1.5.1` alle operationele kenmerken

Als de Directory meer objecten bevat dan kan worden aangepast aan de ene aanroep naar de Directory, is het raadzaam om paging te gebruiken. Voor een goede werking van paging moet u een van de volgende opties:

**Optie 1:**  
`1.2.840.113556.1.4.319` pagedResultsControl

**Optie 2:**  
`2.16.840.1.113730.3.4.9` VLVControl  
`1.2.840.113556.1.4.473` SortControl

Als beide opties zijn ingeschakeld in de connector configuratie, wordt pagedResultsControl gebruikt.

`1.2.840.113556.1.4.417` ShowDeletedControl

ShowDeletedControl wordt alleen gebruikt met de import methode USNChanged Delta om verwijderde objecten te kunnen zien.

De connector probeert de opties te detecteren die aanwezig zijn op de server. Als de opties niet kunnen worden gedetecteerd, wordt er een waarschuwing weer gegeven op de pagina Algemeen in de eigenschappen van de connector. Niet alle LDAP-servers bevatten alle besturings elementen/functies die ze ondersteunen en zelfs als deze waarschuwing aanwezig is, werkt de connector mogelijk zonder problemen.

### <a name="delta-import"></a>Delta-import
Delta-import is alleen beschikbaar als er een ondersteunings Directory is gedetecteerd. De volgende methoden worden momenteel gebruikt:

* LDAP-accesslog. Zie [http://www.openldap.org/doc/admin24/overlays.html#Access logboek registratie](http://www.openldap.org/doc/admin24/overlays.html#Access%20Logging)
* LDAP-wijzigingen logboek. Zie [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* Neem. Voor Novell/NetIQ eDirectory gebruikt de connector de laatste datum en tijd voor het ophalen en bijwerken van objecten. Novell/NetIQ eDirectory biedt geen gelijkwaardige manier om verwijderde objecten op te halen. Deze optie kan ook worden gebruikt als er geen andere import methode voor verschillen actief is op de LDAP-server. Met deze optie kunnen verwijderde objecten niet worden geïmporteerd.
* USNChanged. Zie: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Niet ondersteund
De volgende LDAP-functies worden niet ondersteund:

* LDAP-verwijzingen tussen servers (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Een nieuwe connector maken
Als u een algemene LDAP-connector wilt maken, selecteert u in de **synchronisatie service** **beheer agent** selecteren en **maakt**u deze. Selecteer de **algemene LDAP-connector (micro soft)** .

![CreateConnector](./media/microsoft-identity-manager-2016-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Connectiviteit
Op de pagina connectiviteit moet u de host-, poort-en bindings gegevens opgeven. Afhankelijk van welke binding is geselecteerd, kan aanvullende informatie worden opgegeven in de volgende secties.

![Connectiviteit](./media/microsoft-identity-manager-2016-connector-genericldap/connectivity.png)

* De time-outinstelling van de verbinding wordt alleen gebruikt voor de eerste verbinding met de server bij het detecteren van het schema.
* Als binding anoniem is, worden de gebruikers naam en het wacht woord of het certificaat gebruikt.
* Voor andere bindingen voert u de gegevens in gebruikers naam/wacht woord in of selecteert u een certificaat.
* Als u Kerberos gebruikt om te verifiëren, geeft u ook de realm of het domein van de gebruiker op.

Het tekstvak **kenmerk aliassen** wordt gebruikt voor kenmerken die zijn gedefinieerd in het schema met de syntaxis RFC4522. Deze kenmerken kunnen niet worden gedetecteerd tijdens de schema detectie en de connector heeft hulp nodig om deze kenmerken te identificeren. U moet bijvoorbeeld het volgende opgeven in het vak kenmerk aliassen om het kenmerk userCertificate als een binair kenmerk correct te identificeren:

`userCertificate;binary`

Hier volgt een voor beeld van hoe deze configuratie er als volgt uitziet:

![Connectiviteit](./media/microsoft-identity-manager-2016-connector-genericldap/connectivityattributes.png)

Schakel het selectie vakje **operationele kenmerken in schema toevoegen in** als u ook kenmerken wilt toevoegen die zijn gemaakt door de server. Dit zijn kenmerken zoals wanneer het object is gemaakt en de tijd van de laatste update.

Selecteer **uitbreid bare kenmerken insluiten in schema** als uitbreid bare objecten (RFC4512/4.3) worden gebruikt en als u deze optie inschakelt, kan elk kenmerk worden gebruikt voor alle objecten. Als u deze optie selecteert, maakt u het schema erg groot, tenzij de verbonden Directory deze functie gebruikt, is de optie selectief laten staan.

### <a name="global-parameters"></a>Algemene para meters
Op de pagina globale para meters configureert u de DN-namen voor het Delta wijzigings logboek en extra LDAP-functies. De pagina is vooraf ingevuld met de informatie die door de LDAP-server wordt verstrekt.

![Connectiviteit](./media/microsoft-identity-manager-2016-connector-genericldap/globalparameters.png)

In het bovenste gedeelte worden de gegevens weer gegeven die door de server zelf worden verstrekt, zoals de naam van de server. De connector controleert ook of de verplichte besturings elementen aanwezig zijn in de hoofdmap DSE. Als deze besturings elementen niet worden weer gegeven, wordt er een waarschuwing weer gegeven. Sommige LDAP-mappen bevatten niet alle functies in de hoofdmap DSE en het is mogelijk dat de connector zonder problemen werkt, zelfs niet als er een waarschuwing aanwezig is.

Het selectie vakje **ondersteunde besturings elementen** bepalen het gedrag voor bepaalde bewerkingen:

* Als structuur verwijderen is geselecteerd, wordt een hiërarchie met één LDAP-aanroep verwijderd. Als structuur verwijderen niet is geselecteerd, voert de connector een recursieve verwijdering uit als dat nodig is.
* Als de pagina met resultaten is geselecteerd, voert de connector een geïmporteerde pagina uit met de grootte die is opgegeven bij de stappen voor het uitvoeren.
* De VLVControl en SortControl is een alternatief voor de pagedResultsControl voor het lezen van gegevens uit de LDAP-adres lijst.
* Als alle drie de opties (pagedResultsControl, VLVControl en SortControl) zijn uitgeschakeld, wordt in de connector alle objecten in één bewerking geïmporteerd. Dit kan mislukken als het een grote map is.
* ShowDeletedControl wordt alleen gebruikt wanneer de Delta-import methode USNChanged is.

Het wijzigings logboek DN is de naamgevings context die wordt gebruikt door het Delta wijzigings logboek, bijvoorbeeld **CN = wijzigingen logboek**. Deze waarde moet worden opgegeven om Delta-import te kunnen uitvoeren.

Hier volgt een lijst met standaard-DNs-wijzigingen logboeken:

| Directory | Delta wijzigings logboek |
| --- | --- |
| Micro soft AD LDS en AD GC |Automatisch gedetecteerd. USNChanged. |
| Apache-adreslijst server |Niet beschikbaar. |
| Map 389 |Wijzigings logboek. Standaard waarde die moet worden gebruikt: **CN = wijzigingen logboek** |
| IBM Tivoli DS |Wijzigings logboek. Standaard waarde die moet worden gebruikt: **CN = wijzigingen logboek** |
| Isode Directory |Wijzigings logboek. Standaard waarde die moet worden gebruikt: **CN = wijzigingen logboek** |
| Novell/NetIQ eDirectory |Niet beschikbaar. Neem. De connector gebruikt de laatst bijgewerkte datum/tijd om records toe te voegen en op te halen. |
| DJ/DS openen |Wijzigings logboek.  Standaard waarde die moet worden gebruikt: **CN = wijzigingen logboek** |
| LDAP openen |Access-logboek. Standaard waarde die moet worden gebruikt: **CN = accesslog** |
| Oracle-DSEE |Wijzigings logboek. Standaard waarde die moet worden gebruikt: **CN = wijzigingen logboek** |
| RadiantOne VDS |Virtuele map. Is afhankelijk van de directory die is verbonden met VDS. |
| Sun One-Directory server |Wijzigings logboek. Standaard waarde die moet worden gebruikt: **CN = wijzigingen logboek** |

Het kenmerk Password is de naam van het kenmerk dat de connector moet gebruiken om het wacht woord in te stellen in het wacht woord voor wachtwoord wijzigingen en wacht woord instellen.
Deze waarde is standaard ingesteld op **userPassword** , maar kan worden gewijzigd wanneer dit nodig is voor een bepaald LDAP-systeem.

In de lijst extra partities is het mogelijk om extra naam ruimten toe te voegen die niet automatisch worden gedetecteerd. Deze instelling kan bijvoorbeeld worden gebruikt als verschillende servers een logisch cluster vormen, dat allemaal tegelijk moet worden geïmporteerd. Net zoals Active Directory meerdere domeinen in één forest kan hebben, maar alle domeinen één schema delen, kan hetzelfde worden gesimuleerd door de extra naam ruimten in te voeren in dit vak. Elke naam ruimte kan vanaf verschillende servers worden geïmporteerd en verder worden geconfigureerd op de pagina partities en hiërarchieën configureren. Gebruik CTRL + ENTER om een nieuwe regel te krijgen.

### <a name="configure-provisioning-hierarchy"></a>Inrichtings hiërarchie configureren
Deze pagina wordt gebruikt om het DN-onderdeel, bijvoorbeeld OE, toe te wijzen aan het object type dat moet worden ingericht, bijvoorbeeld organizationalUnit.

![Inrichtings hiërarchie](./media/microsoft-identity-manager-2016-connector-genericldap/provisioninghierarchy.png)

Als u de inrichtings hiërarchie configureert, kunt u de connector zo configureren dat deze automatisch een structuur maakt wanneer dat nodig is. Als er bijvoorbeeld een naam ruimte DC = contoso, DC = com en een nieuw object CN = Joe, ou = Seattle, c = US, DC = contoso, DC = com is ingericht, kan de connector een object van het type land maken voor US en een organizationalUnit voor Seattle als die nog niet aanwezig zijn in de map.

### <a name="configure-partitions-and-hierarchies"></a>Partities en hiërarchieën configureren
Selecteer op de pagina partities en hiërarchieën alle naam ruimten met objecten die u wilt importeren en exporteren.

![Partities](./media/microsoft-identity-manager-2016-connector-genericldap/partitions.png)

Voor elke naam ruimte is het ook mogelijk om connectiviteits instellingen te configureren waarmee de waarden die zijn opgegeven in het connectiviteits scherm, worden overschreven. Als deze waarden in de standaard lege waarde blijven, wordt de informatie uit het connectiviteits scherm gebruikt.

Het is ook mogelijk om te selecteren van welke containers en organisatie-eenheden de connector moet importeren en exporteren.

Bij het uitvoeren van een zoek actie worden deze uitgevoerd in alle containers in de partitie. Als er een groot aantal containers is, leidt dit tot prestatie vermindering.

> [!NOTE]
> Vanaf de update van maart 2017 van de algemene Zoek opdrachten voor LDAP-connectors kan het bereik worden beperkt tot alleen de geselecteerde containers. U kunt dit doen door het selectie vakje alleen zoeken in geselecteerde containers in te scha kelen, zoals wordt weer gegeven in de onderstaande afbeelding.

![Alleen geselecteerde containers doorzoeken](./media/microsoft-identity-manager-2016-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Ankers configureren
Deze pagina heeft altijd een vooraf geconfigureerde waarde en kan niet worden gewijzigd. Als de server leverancier is geïdentificeerd, kan het anker worden gevuld met een onveranderbaar kenmerk, bijvoorbeeld de GUID voor een object. Als het niet is gedetecteerd of als er geen onveranderbaar kenmerk is, gebruikt de connector DN (Distinguished Name) als het anker.

![ankers](./media/microsoft-identity-manager-2016-connector-genericldap/anchors.png)


Hier volgt een lijst met LDAP-servers en het gebruikte anker:

| Directory | Anker kenmerk |
| --- | --- |
| Micro soft AD LDS en AD GC |objectGUID |
| 389-adreslijst server |dn |
| Apache-map |dn |
| IBM Tivoli DS |dn |
| Isode Directory |dn |
| Novell/NetIQ eDirectory |GUID |
| DJ/DS openen |dn |
| LDAP openen |dn |
| Oracle-ODSEE |dn |
| RadiantOne VDS |dn |
| Sun One-Directory server |dn |

## <a name="other-notes"></a>Andere opmerkingen
Deze sectie bevat informatie over aspecten die specifiek zijn voor deze connector of om andere redenen belang rijk te weten zijn.

### <a name="delta-import"></a>Delta-import
Het verschil watermerk in open LDAP is UTC-datum/-tijd. Daarom moeten de klokken tussen de FIM-synchronisatie service en de open LDAP worden gesynchroniseerd. Als dat niet het geval is, kunnen sommige vermeldingen in het Delta wijzigings logboek worden wegge laten.

Voor de Novell-eDirectory wordt door de Delta-import geen object verwijderingen gedetecteerd. Daarom moet regel matig een volledige import bewerking worden uitgevoerd om alle verwijderde objecten te vinden.

Voor directory's met een Delta wijzigings logboek dat is gebaseerd op de datum/tijd, wordt het ten zeerste aanbevolen een volledige import uit te voeren op periodieke tijdstippen. Met dit proces kan de synchronisatie-engine zoeken naar en verschillen tussen de LDAP-server en wat zich op dit moment in de connector ruimte bevindt.

## <a name="troubleshooting"></a>Probleemoplossing
* Zie voor meer informatie over het inschakelen van logboek registratie voor het oplossen van problemen met de connector, het [inschakelen van etw-tracering voor connectors](http://go.microsoft.com/fwlink/?LinkId=335731).
