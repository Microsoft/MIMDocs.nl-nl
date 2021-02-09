---
title: Power shell-connector | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de Windows Power shell-connector van micro soft configureert.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6dba8e34-a874-4ff0-90bc-bd2b0a4199b5
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/02/2018
ms.author: billmath
ms.openlocfilehash: 0dcba300f70756dbfa7a29011a37839247e6bf8a
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835927"
---
# <a name="windows-powershell-connector-technical-reference"></a>Technische documentatie voor Windows PowerShell-connector
In dit artikel wordt de Windows Power shell-connector beschreven. Het artikel is van toepassing op de volgende producten:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * U moet hotfix 4.1.3671.0 of hoger [KB3092178](https://support.microsoft.com/kb/3092178)gebruiken.

Voor MIM2016 en FIM2010R2 is de connector beschikbaar als down load vanuit het [micro soft Download centrum](https://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Overzicht van de Power shell-connector
Met de Power shell-connector kunt u de synchronisatie service integreren met externe systemen die Api's op basis van Windows Power shell bieden. De connector biedt een brug tussen de mogelijkheden van het ECMA2-Framework (Extensible Connectivity Management Agent 2) en Windows Power shell. Zie voor meer informatie over het ECMA-Framework de [referentie voor de Extensible Connectivity 2,2 Management Agent](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Vereisten
Voordat u de connector gebruikt, moet u controleren of u over het volgende beschikt op de synchronisatie server:

* Microsoft .NET 4.5.2-Framework of hoger
* Windows Power Shell 2,0, 3,0 of 4,0

Het uitvoerings beleid op de synchronisatie service-Server moet zo worden geconfigureerd dat de connector Windows Power shell-scripts kan uitvoeren. Tenzij de scripts die de connector uitvoert, digitaal zijn ondertekend, moet u het uitvoerings beleid configureren door deze opdracht uit te voeren:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Een nieuwe connector maken
Als u een Windows Power shell-connector in de synchronisatie service wilt maken, moet u een reeks Windows Power shell-scripts opgeven waarmee de stappen worden uitgevoerd die door de synchronisatie service zijn aangevraagd. Afhankelijk van de gegevens bron waarmee u verbinding maakt en de functionaliteit die u nodig hebt, varieert de scripts die u moet implementeren. Deze sectie bevat een overzicht van alle scripts die kunnen worden geïmplementeerd en wanneer ze vereist zijn.

De Windows Power shell-connector is ontworpen voor het opslaan van elk van de scripts in de data base van de synchronisatie service. Hoewel het mogelijk is om scripts uit te voeren die zijn opgeslagen op het bestands systeem, is het eenvoudiger om de hoofd tekst van elk script rechtstreeks in te voegen in de configuratie van de connector.

Als u een Power shell-connector wilt maken, selecteert u in de **synchronisatie service** **beheer agent** selecteren en **maakt** u deze. Selecteer de **Power shell-connector (micro soft)** .

![Connector maken](./media/microsoft-identity-manager-2016-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Connectiviteit
Geef configuratie parameters op om verbinding te maken met een extern systeem. Deze waarden worden veilig opgeslagen door de synchronisatie service en beschikbaar gesteld aan uw Windows Power shell-scripts wanneer de connector wordt uitgevoerd.

![Connectiviteit](./media/microsoft-identity-manager-2016-connector-powershell/connectivity.png)

U kunt de volgende connectiviteits parameters configureren:

**Connectiviteit**


|              Parameter               | Standaardwaarde |                                                                                                                                                                                                                  Doel                                                                                                                                                                                                                   |
|--------------------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                Server                |    <Blank>    |                                                                                                                                                                                             De server naam waarmee de connector moet worden verbonden.                                                                                                                                                                                              |
|                Domain                |    <Blank>    |                                                                                                                                                                                    Domein van de referentie die moet worden opgeslagen voor gebruik wanneer de connector wordt uitgevoerd.                                                                                                                                                                                    |
|                 Gebruiker                 |    <Blank>    |                                                                                                                                                                                   De gebruikers naam van de referentie die moet worden opgeslagen voor gebruik wanneer de connector wordt uitgevoerd.                                                                                                                                                                                   |
|               Wachtwoord               |    <Blank>    |                                                                                                                                                                                   Wacht woord van de referentie die moet worden opgeslagen voor gebruik wanneer de connector wordt uitgevoerd.                                                                                                                                                                                   |
|    Connector account imiteren     |     Niet waar     | Als deze eigenschap waar is, voert de synchronisatie service de Windows Power shell-scripts uit in de context van de opgegeven referenties. Als dat mogelijk is, wordt aangeraden dat de **$credentials** para meter wordt door gegeven aan elk script wordt gebruikt in plaats van imitatie. Zie [aanvullende configuratie voor imitatie](#additional-configuration-for-impersonation)voor meer informatie over aanvullende machtigingen die nodig zijn voor het gebruik van deze optie. |
| Gebruikers profiel laden bij imiteren |     Niet waar     |                          Hiermee wordt aangegeven dat Windows het gebruikers profiel van de connector referenties tijdens imitatie moet laden. Als de geïmiteerde gebruiker een zwervend profiel heeft, laadt de connector het zwervende profiel niet. Zie [aanvullende configuratie voor imitatie](#additional-configuration-for-impersonation)voor meer informatie over aanvullende machtigingen die nodig zijn voor het gebruik van deze para meter.                           |
|    Aanmeldings type tijdens het imiteren     |     Geen      |                                                                                                                                                                      Aanmeldings type tijdens imitatie. Zie de [dwLogonType][dw] -documentatie voor meer informatie.                                                                                                                                                                       |
|         Alleen ondertekende scripts          |     Niet waar     |                                                                                                    Indien true, wordt in de Windows Power shell-connector gecontroleerd of elk script een geldige digitale hand tekening heeft. Indien onwaar, moet u ervoor zorgen dat het Windows Power shell-uitvoerings beleid van de synchronisatie service RemoteSigned of onbeperkt is.                                                                                                     |

**Algemene module**  
Met de connector kunt u een gedeelde Windows Power shell-module opslaan in de configuratie. Wanneer de connector een script uitvoert, wordt de Windows Power shell-module uitgepakt naar het bestands systeem zodat deze kan worden geïmporteerd door elk script.

Voor import-, export-en wachtwoord synchronisatie scripts wordt de algemene module geëxtraheerd naar de map MAData van de connector. Voor schema-, validatie-, hiërarchie-en partitie detectie scripts wordt de gemeen schappelijke module geëxtraheerd naar de map% TEMP%. In beide gevallen wordt het geëxtraheerde common module-script benoemd volgens de script naam instelling common module.

Als u een module met de naam FIMPowerShellConnectorModule. psm1 uit de map MAData wilt laden, gebruikt u de volgende instructie: `Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Als u een module met de naam FIMPowerShellConnectorModule. psm1 in de map% TEMP% wilt laden, gebruikt u de volgende instructie: `Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Validatie van de para meter**  
Het validatie script is een optioneel Windows Power shell-script dat kan worden gebruikt om ervoor te zorgen dat connector configuratie parameters die door de beheerder worden opgegeven, geldig zijn. Validatie van de server-, verbindings referenties-en verbindings parameters zijn veelvoorkomende toepassingen van het validatie script. Het validatie script wordt aangeroepen nadat de volgende tabbladen en dialoog vensters zijn gewijzigd:

* Connectiviteit
* Globale parameters
* Partitie configuratie

Het validatie script ontvangt de volgende para meters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameterPage |[ConfigParameterPage][cpp] |Het configuratie tabblad of dialoog venster dat de validatie aanvraag heeft geactiveerd. |
| ConfigParameters |[KeyedCollection][keyk] [teken reeks, [ConfigParameter][cp]] |Tabel met configuratie parameters voor de connector. |
| Referentie |[PSCredential][pscred] |Bevat de referenties die door de beheerder zijn ingevoerd op het tabblad connectiviteit. |

Het validatie script moet één ParameterValidationResult-object retour neren naar de pijp lijn.

**Schema detectie**  
Het schema detectie script is verplicht. Dit script retourneert de object typen, kenmerken en kenmerk beperkingen die de synchronisatie service gebruikt bij het configureren van kenmerk stroom regels. Het schema detectie script wordt uitgevoerd tijdens het maken van de connector en vult het schema van de connector. Het wordt ook gebruikt door de actie schema vernieuwen in de Synchronization Service Manager.

Het schema detectie script ontvangt de volgende para meters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk] [teken reeks, [ConfigParameter][cp]] |Tabel met configuratie parameters voor de connector. |
| Referentie |[PSCredential][pscred] |Bevat de referenties die door de beheerder zijn ingevoerd op het tabblad connectiviteit. |

Het script moet één [schema][schema] object retour neren naar de pijp lijn. Het schema object bestaat uit [schema type][schemaT] -objecten die object typen vertegenwoordigen (bijvoorbeeld: gebruikers en groepen). Het schema type-object bevat een verzameling [SchemaAttribute][schemaA] -objecten die de kenmerken vertegenwoordigen (bijvoorbeeld: naam, voor-en post adres) van het type.

**Aanvullende para meters**  
Naast de standaard configuratie-instellingen kunt u aanvullende aangepaste configuratie-instellingen definiëren die specifiek zijn voor het exemplaar van de connector. Deze para meters kunnen worden opgegeven op de connector, partition of run Step levels en toegankelijk vanuit het relevante Windows Power shell-script. Aangepaste configuratie-instellingen kunnen worden opgeslagen in de data base van de synchronisatie service als tekst zonder opmaak of ze kunnen worden versleuteld. Met de synchronisatie service worden beveiligde configuratie-instellingen automatisch versleuteld en ontsleuteld wanneer dit nodig is.

Als u aangepaste configuratie-instellingen wilt opgeven, moet u de naam van elke para meter scheiden met een komma (,).

Voor toegang tot aangepaste configuratie-instellingen vanuit een script moet u de naam achtervoegsel met een onderstrepings teken ( \_ ) en het bereik van de para meter (Global, partition of RunStep). Gebruik bijvoorbeeld het volgende code fragment om toegang te krijgen tot de globale FileName-para meter: `$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Functies
Het tabblad mogelijkheden van de beheer agent Designer definieert het gedrag en de functionaliteit van de connector. De selecties die op dit tabblad zijn gemaakt, kunnen niet worden gewijzigd wanneer de connector is gemaakt. Deze tabel bevat een lijst met de instellingen voor de mogelijkheden.

![Functies](./media/microsoft-identity-manager-2016-connector-powershell/capabilities.png)

| Mogelijkheid | Beschrijving |
| --- | --- |
| [Stijl van DN-naam][dnstyle] |Hiermee wordt aangegeven of de connector distinguished names ondersteunt, en welke stijl. |
| [Export type][exportT] |Bepaalt het type objecten die aan het export script worden gepresenteerd. <li>AttributeReplace – bevat de volledige set waarden voor een kenmerk met meerdere waarden wanneer het kenmerk wordt gewijzigd.</li><li>AttributeUpdate – bevat alleen de Deltas naar een kenmerk met meerdere waarden wanneer het kenmerk wordt gewijzigd.</li><li>MultivaluedReferenceAttributeUpdate-bevat een volledige set waarden voor niet-referentie kenmerken met meerdere waarden en alleen Deltas voor referentie kenmerken met meerdere waarden.</li><li>ObjectReplace: bevat alle kenmerken voor een object wanneer een kenmerk wordt gewijzigd</li> |
| [Gegevens normalisatie][DataNorm] |Hiermee wordt de synchronisatie service zo ingesteld dat anker kenmerken worden genormaliseerd voordat ze aan scripts worden door gegeven. |
| [Object bevestiging][oconf] |Hiermee configureert u het import gedrag in behandeling in de synchronisatie service. <li>Normaal: standaard gedrag waarbij alle geëxporteerde wijzigingen moeten worden bevestigd via importeren</li><li>NoDeleteConfirmation: wanneer een object wordt verwijderd, wordt er geen import bewerking in behandeling gegenereerd.</li><li>NoAddAndDeleteConfirmation: wanneer een object wordt gemaakt of verwijderd, wordt er geen import bewerking in behandeling gegenereerd.</li> |
| DN als anker gebruiken |Als de DN-naam stijl is ingesteld op LDAP, is het anker kenmerk voor de connector ruimte ook de DN-naam. |
| Gelijktijdige bewerkingen van verschillende connectors |Wanneer dit selectie vakje is ingeschakeld, kunnen meerdere Windows Power shell-connectors gelijktijdig worden uitgevoerd. |
| Partities |Als u dit selectie vakje inschakelt, ondersteunt de connector meerdere partities en partitie detectie. |
| Hiërarchie |Als u dit selectie vakje inschakelt, ondersteunt de connector een hiërarchische structuur van LDAP-stijl. |
| Importeren inschakelen |Als u dit selectie vakje inschakelt, worden gegevens door de connector geïmporteerd via import scripts. |
| Delta-import inschakelen |Als u dit selectie vakje inschakelt, kan de connector Deltas aanvragen uit de import scripts. |
| Exporteren inschakelen |Als u dit selectie vakje inschakelt, exporteert de connector gegevens via export scripts. |
| Volledige export inschakelen |Als u dit selectie vakje inschakelt, ondersteunen de export scripts de gehele connector ruimte. Als u deze optie wilt gebruiken, moet u het selectie vakje exporteren ook inschakelen. |
| Er zijn geen referentie waarden in de eerste export fase |Als u dit selectie vakje inschakelt, worden referentie kenmerken geëxporteerd tijdens een tweede export fase. |
| Object naam wijzigen toestaan |Als u dit selectie vakje inschakelt, kunnen DN-namen worden gewijzigd. |
| Delete-Add als vervangen |Als u dit selectie vakje inschakelt, worden de bewerkingen voor verwijderen en toevoegen geëxporteerd als één vervanging. |
| Wachtwoord bewerkingen inschakelen |Als u dit selectie vakje inschakelt, worden wachtwoord synchronisatie scripts ondersteund. |
| Wacht woord voor exporteren in eerste fase inschakelen |Als u dit selectie vakje inschakelt, worden wacht woorden die tijdens het inrichten worden ingesteld, geëxporteerd wanneer het object wordt gemaakt. |

### <a name="global-parameters"></a>Globale parameters
Op het tabblad globale para meters in de beheer agent Designer kunt u de Windows Power shell-scripts configureren die worden uitgevoerd door de connector. U kunt ook globale waarden configureren voor aangepaste configuratie-instellingen die zijn gedefinieerd op het tabblad connectiviteit.

**Partitie detectie**  
Een partitie is een afzonderlijke naam ruimte binnen één gedeeld schema. In Active Directory elk domein bijvoorbeeld een partitie binnen één forest. Een partitie is de logische groepering voor import-en export bewerkingen. Importeren en exporteren hebben een partitie als context en alle bewerkingen vindt plaats in deze context. Partities moeten een hiërarchie in LDAP vertegenwoordigen. De DN-naam van een partitie wordt gebruikt bij het importeren om te controleren of alle geretourneerde objecten binnen het bereik van een partitie vallen. De DN-naam van de partitie wordt ook gebruikt tijdens het inrichten van de inverse van de connector ruimte om te bepalen voor welke partitie een object tijdens het exporteren moet worden gekoppeld.

Het partitie detectie script ontvangt de volgende para meters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][teken reeks, [ConfigParameter][cp]] |Tabel met configuratie parameters voor de connector. |
| Referentie |[PSCredential][pscred] |Bevat de referenties die door de beheerder zijn ingevoerd op het tabblad connectiviteit. |

Het script moet een enkel [partitie][part] object of een lijst [T] van partitie-objecten retour neren naar de pijp lijn.

**Hiërarchie detectie**  
Het hiërarchie detectie script wordt alleen gebruikt wanneer de Distinguished name Style is ingesteld op LDAP. Het script wordt gebruikt om te bladeren door een set containers die in of buiten het bereik van de import-en export bewerkingen worden opgenomen. Het script mag alleen een lijst met knoop punten bevatten die directe onderliggende elementen zijn van het hoofd knooppunt dat is opgegeven voor het script.

Het hiërarchie detectie script ontvangt de volgende para meters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][teken reeks, [ConfigParameter][cp]] |Tabel met configuratie parameters voor de connector. |
| Referentie |[PSCredential][pscred] |Bevat de referenties die door de beheerder zijn ingevoerd op het tabblad connectiviteit. |
| ParentNode |[HierarchyNode][hn] |Het hoofd knooppunt van de hiërarchie waaronder het script directe onderliggende elementen moet retour neren. |

Het script moet een enkel onderliggend HierarchyNode-object of een lijst [T] van onderliggende HierarchyNode-objecten retour neren naar de pijp lijn.

#### <a name="import"></a>Importeren
Connectors die ondersteuning bieden voor import bewerkingen, moeten drie scripts implementeren.

**Beginnen met importeren**  
Het begin van het import script wordt uitgevoerd aan het begin van een stap voor het uitvoeren van een import bewerking. Tijdens deze stap kunt u een verbinding met het bron systeem tot stand brengen en voorbereidende stappen uitvoeren voordat u gegevens uit het verbonden systeem importeert.

Het begin script voor importeren ontvangt de volgende para meters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][teken reeks, [ConfigParameter][cp]] |Tabel met configuratie parameters voor de connector. |
| Referentie |[PSCredential][pscred] |Bevat de referenties die door de beheerder zijn ingevoerd op het tabblad connectiviteit. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Hiermee wordt het script geïnformeerd over het type import uitvoering (Delta of volledig), partitie, hiërarchie, water merk en verwachte pagina grootte. |
| Typen |[Schema][schema] |Schema voor de geïmporteerde connector ruimte. |

Het script moet één [OpenImportConnectionResults][oicres] -object retour neren naar de pijp lijn, bijvoorbeeld: `Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Gegevens importeren**  
Het gegevens script voor importeren wordt aangeroepen door de connector totdat het script aangeeft dat er geen gegevens meer kunnen worden geïmporteerd. De Windows Power shell-connector heeft een pagina formaat van 9.999 objecten. Als uw script meer dan 9.999 objecten retourneert voor import, moet u paginering ondersteunen. De connector geeft een aangepaste gegevens eigenschap weer die u kunt gebruiken om een water merk op te slaan, zodat telkens wanneer het gegevens bestand voor importeren wordt aangeroepen, het importeren van objecten door het script wordt hervat.

Het script gegevens importeren ontvangt de volgende para meters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][teken reeks, [ConfigParameter][cp]] |Tabel met configuratie parameters voor de connector. |
| Referentie |[PSCredential][pscred] |Bevat de referenties die door de beheerder zijn ingevoerd op het tabblad connectiviteit. |
| GetImportEntriesRunStep |[ImportRunStep][irs] |Bevat het water merk (CustomData) dat kan worden gebruikt tijdens het importeren van pagina's en Delta-import bewerkingen. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Hiermee wordt het script geïnformeerd over het type import uitvoering (Delta of volledig), partitie, hiërarchie, water merk en verwachte pagina grootte. |
| Typen |[Schema][schema] |Schema voor de geïmporteerde connector ruimte. |

Het gegevens script voor importeren moet een lijst [[CSEntryChange][csec]]-object naar de pijp lijn schrijven. Deze verzameling bestaat uit CSEntryChange-kenmerken die elk geïmporteerde object vertegenwoordigen. Tijdens een volledige import bewerking moet deze verzameling een volledige set CSEntryChange-objecten hebben met alle kenmerken voor elk object. Tijdens een Delta-import moet het object CSEntryChange de Delta kenmerken van het kenmerk niveau bevatten voor elk object dat moet worden geïmporteerd of een volledige weer gave van de objecten die zijn gewijzigd (vervangen modus).

**Importeren beëindigen**  
Aan het einde van de import uitvoering wordt het script voor het beëindigen van de import uitgevoerd. Dit script moet alle benodigde opschoon taken uitvoeren (bijvoorbeeld verbindingen met systemen sluiten en reageren op fouten).

Het einde van het import script ontvangt de volgende para meters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][teken reeks, [ConfigParameter][cp]] |Tabel met configuratie parameters voor de connector. |
| Referentie |[PSCredential][pscred] |Bevat de referenties die door de beheerder zijn ingevoerd op het tabblad connectiviteit. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Hiermee wordt het script geïnformeerd over het type import uitvoering (Delta of volledig), partitie, hiërarchie, water merk en verwachte pagina grootte. |
| CloseImportConnectionRunStep |[CloseImportConnectionRunStep][cecrs] |Informeert het script over de reden waarom het importeren is beëindigd. |

Het script moet één [CloseImportConnectionResults][cicres] -object retour neren naar de pijp lijn, bijvoorbeeld: `Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Exporteren
De connectors die ondersteuning bieden voor het exporteren, moeten drie scripts implementeren voor de import architectuur van de connector.

**Beginnen met exporteren**  
Het begin script voor het exporteren wordt uitgevoerd aan het begin van een stap voor het uitvoeren van een export. Tijdens deze stap kunt u een verbinding met het bron systeem tot stand brengen en eventuele voorbereidende stappen uitvoeren voordat u gegevens naar het verbonden systeem exporteert.

Het begin script voor het exporteren ontvangt de volgende para meters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][teken reeks, [ConfigParameter][cp]] |Tabel met configuratie parameters voor de connector. |
| Referentie |[PSCredential][pscred] |Bevat de referenties die door de beheerder zijn ingevoerd op het tabblad connectiviteit. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Hiermee wordt het script geïnformeerd over het type export uitvoering (Delta of volledig), partitie, hiërarchie en verwachte pagina grootte. |
| Typen |[Schema][schema] |Schema voor de te exporteren connector ruimte. |

Het script mag geen uitvoer naar de pijp lijn retour neren.

**Gegevens exporteren**  
De synchronisatie service roept het script voor het exporteren van gegevens zo vaak aan als nodig is voor het verwerken van alle in behandeling zijnde export bewerkingen. Als de ruimte van de connector in afwachting is van de export van de pagina grootte van de connector, kan het gegevens script voor exporteren meerdere keren worden aangeroepen en mogelijk meerdere keren voor hetzelfde object.

Het script gegevens exporteren ontvangt de volgende para meters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][teken reeks, [ConfigParameter][cp]] |Tabel met configuratie parameters voor de connector. |
| Referentie |[PSCredential][pscred] |Bevat de referenties die door de beheerder zijn ingevoerd op het tabblad connectiviteit. |
| CSEntries |IList[CSEntryChange][csec] |Lijst met alle connector ruimte objecten met in behandeling zijnde exports die moeten worden verwerkt tijdens deze fase. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Hiermee wordt het script geïnformeerd over het type export uitvoering (Delta of volledig), partitie, hiërarchie en verwachte pagina grootte. |
| Typen |[Schema][schema] |Schema voor de te exporteren connector ruimte. |

Het export gegevens script moet een [PutExportEntriesResults][peeres] -object retour neren aan de pijp lijn. Dit object hoeft geen resultaat informatie te bevatten voor elke geëxporteerde connector, tenzij er een fout optreedt of een wijziging in het anker kenmerk voordoet. Als u bijvoorbeeld een PutExportEntriesResults-object wilt retour neren naar de pijp lijn: `Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Export beëindigen**  
Aan het einde van de export uitvoering wordt het export script beëindigen uitgevoerd. Dit script moet alle benodigde opschoon taken uitvoeren (bijvoorbeeld verbindingen met systemen sluiten en reageren op fouten).

Het end-export script ontvangt de volgende para meters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][teken reeks, [ConfigParameter][cp]] |Tabel met configuratie parameters voor de connector. |
| Referentie |[PSCredential][pscred] |Bevat de referenties die door de beheerder zijn ingevoerd op het tabblad connectiviteit. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Hiermee wordt het script geïnformeerd over het type export uitvoering (Delta of volledig), partitie, hiërarchie en verwachte pagina grootte. |
| CloseExportConnectionRunStep |[CloseExportConnectionRunStep][cecrs] |Informeert het script over de reden waarom het exporteren is beëindigd. |

Het script mag geen uitvoer naar de pijp lijn retour neren.

#### <a name="password-synchronization"></a>Wachtwoordsynchronisatie
Windows Power shell-Connectors kunnen worden gebruikt als doel voor het wijzigen van wacht woorden/opnieuw instellen.

Het wachtwoord script ontvangt de volgende para meters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][teken reeks, [ConfigParameter][cp]] |Tabel met configuratie parameters voor de connector. |
| Referentie |[PSCredential][pscred] |Bevat de referenties die door de beheerder zijn ingevoerd op het tabblad connectiviteit. |
| Partitie |[Partitie][part] |De mappartitie waarin de CSEntry zich bevindt. |
| CSEntry |[CSEntry][cse] |Vermelding van connector ruimte voor het object dat is ontvangen van een wacht woord wijzigen of opnieuw instellen. |
| OperationType |Tekenreeks |Hiermee wordt aangegeven of de bewerking opnieuw instellen (**SetPassword**) of een wijziging (**ChangePassword**) is. |
| PasswordOptions |[PasswordOptions][pwdopt] |Vlaggen die het beoogde gedrag van het wacht woord opnieuw instellen aangeven. Deze para meter is alleen beschikbaar als OperationType **SetPassword** is. |
| OldPassword |Tekenreeks |Ingevuld met het oude wacht woord van het object voor wachtwoord wijzigingen. Deze para meter is alleen beschikbaar als OperationType is van **ChangePassword**. |
| NieuwWachtwoord |Tekenreeks |Ingevuld met het nieuwe wacht woord van het object dat door het script moet worden ingesteld. |

Het wachtwoord script wordt niet verwacht om resultaten te retour neren naar de Windows Power shell-pijp lijn. Als er een fout optreedt in het wachtwoord script, moet het script een van de volgende uitzonde ringen genereren om de synchronisatie service op de hoogte te stellen van het probleem:

* [PasswordPolicyViolationException][pwdex1] : wordt gegenereerd als het wacht woord niet voldoet aan het wachtwoord beleid in het verbonden systeem.
* [PasswordIllFormedException][pwdex2] : wordt gegenereerd als het wacht woord niet acceptabel is voor het verbonden systeem.
* [PasswordExtension][pwdex3] : wordt gegenereerd voor alle andere fouten in het wachtwoord script.

## <a name="sample-connectors"></a>Voor beelden van connectors
Zie voor een volledig overzicht van de beschik bare voor beelden connectors verzameling voor beeld van een [Windows Power shell-connector][samp].

## <a name="other-notes"></a>Andere opmerkingen
### <a name="additional-configuration-for-impersonation"></a>Aanvullende configuratie voor imitatie
Verleen de gebruiker dat de volgende machtigingen op de synchronisatie service Server worden geïmiteerd:

Lees toegang tot de volgende register sleutels:

* HKEY_USERS \\ [SynchronizationServiceServiceAccountSID] \Software\Microsoft\PowerShell
* HKEY_USERS \\ [SynchronizationServiceServiceAccountSID] \Environment

Voer de volgende Power shell-opdrachten uit om de beveiligings-id (SID) van het service account van de synchronisatie service te bepalen:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Lees toegang tot de volgende bestandssysteem mappen:

* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\Extensions
* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\ExtensionsCache
* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\MaData \\ {connectornaam}

Vervang de naam van de Windows Power shell-connector voor de tijdelijke aanduiding {connector naam}.

## <a name="troubleshooting"></a>Problemen oplossen
* Zie voor meer informatie over het inschakelen van logboek registratie voor het oplossen van problemen met de connector, het [inschakelen van etw-tracering voor connectors](https://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: https://go.microsoft.com/fwlink/?LinkId=394291
