---
title: Algemene SQL-connector | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de algemene SQL-connector van micro soft kunt configureren.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: fd8ccef3-6605-47ba-9219-e0c74ffc0ec9
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/02/2018
ms.author: billmath
ms.openlocfilehash: 862ea1ed910905daa0f2e0be97838f3da2423661
ms.sourcegitcommit: 2950ef38055bf78e3b98e4cd84771b04c2aa5584
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/08/2020
ms.locfileid: "92759022"
---
# <a name="generic-sql-connector-technical-reference"></a>Technische Naslag informatie over algemene SQL-connector
In dit artikel wordt de algemene SQL-connector beschreven. Het artikel is van toepassing op de volgende producten:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * U moet hotfix 4.1.3671.0 of hoger [KB3092178](https://support.microsoft.com/kb/3092178)gebruiken.

Voor MIM2016 en FIM2010R2 is de connector beschikbaar als down load vanuit het [micro soft Download centrum](https://go.microsoft.com/fwlink/?LinkId=717495).

Zie het stapsgewijze artikel over de [algemene SQL-connector](microsoft-identity-manager-2016-connector-genericsql-step-by-step.md) om deze connector in actie te zien.

## <a name="overview-of-the-generic-sql-connector"></a>Overzicht van de algemene SQL-connector
Met de algemene SQL-connector kunt u de synchronisatie service integreren met een database systeem dat ODBC-connectiviteit biedt.  

Van een perspectief op hoog niveau worden de volgende functies ondersteund door de huidige versie van de connector:

| Functie | Ondersteuning |
| --- | --- |
| Verbonden gegevens bron |De connector wordt ondersteund met alle 64-bits ODBC-stuur Programma's *. Het is getest met het volgende: <li>Microsoft SQL Server & SQL Azure</li><li>IBM DB2 10. x</li><li>IBM DB2 9. x</li><li>Oracle 10 & 11g</li><li>Oracle-12c en-18c</li><li>MySQL 5. x</li> |
| Scenario's |<li>Beheer van object levenscyclus</li><li>Wachtwoordbeheer</li> |
| Bewerkingen |<li>Volledige import-en Delta-import, exporteren</li><li>Voor exporteren: toevoegen, verwijderen, bijwerken en vervangen</li><li>Wacht woord instellen, wacht woord wijzigen</li> |
| Schema |<li>Dynamische detectie van objecten en kenmerken</li> |

> [!NOTE]
> * Verbindingen met gegevens bronnen die niet hierboven worden vermeld, zijn momenteel beperkt tot alleen-lezen scenario's.

### <a name="prerequisites"></a>Vereisten
Voordat u de connector gebruikt, moet u controleren of u over het volgende beschikt op de synchronisatie server:

* Microsoft .NET 4.5.2-Framework of hoger
* 64-bits ODBC-client Stuur Programma's
* Als u de connector gebruikt om te communiceren met Oracle 12c, hebt u Oracle Instant client 12.2.0.1 of nieuwer nodig met het ODBC-pakket.
* Als u de connector gebruikt om met Oracle 18c te communiceren, hebt u Oracle Instant client 18.3.0.0 of nieuwer nodig met het ODBC-pakket en moet de NLS_LANG systeem variabele worden ingesteld op ondersteuning van UTF8-tekens.

### <a name="permissions-in-connected-data-source"></a>Machtigingen in verbonden gegevens bron
Als u een van de ondersteunde taken in een algemene SQL-connector wilt maken of uitvoeren, hebt u het volgende nodig:

* db_datareader
* db_datawriter

### <a name="ports-and-protocols"></a>Poorten en protocollen
Raadpleeg de documentatie van de leverancier van de Data Base voor de poorten die vereist zijn voor het werken met het ODBC-stuur programma.

## <a name="create-a-new-connector"></a>Een nieuwe connector maken
Als u een algemene SQL-connector wilt maken, selecteert u in de **synchronisatie service** **beheer agent** selecteren en **maakt** u deze. Selecteer de **algemene SQL-connector (micro soft)** .

![CreateConnector](./media/microsoft-identity-manager-2016-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Connectiviteit
De connector gebruikt een ODBC DSN-bestand voor connectiviteit. Maak het DSN-bestand met behulp van **ODBC-gegevens bronnen** die worden gevonden in het menu Start onder **systeem beheer** . Maak in het beheer programma een **Bestands-DSN** zodat deze kan worden ingesteld voor de connector.

![CreateConnector](./media/microsoft-identity-manager-2016-connector-genericsql/connectivity.png)

Het scherm connectiviteit is de eerste wanneer u een nieuwe algemene SQL-connector maakt. U moet eerst de volgende informatie opgeven:

* Pad naar DSN-bestand
* Verificatie
  * Gebruikersnaam
  * Wachtwoord

De data base moet een van de volgende verificatie methoden ondersteunen:

* **Windows-verificatie** : de verificatie database gebruikt de Windows-referenties om de gebruiker te verifiëren. De opgegeven gebruikers naam en het wacht woord worden gebruikt voor verificatie bij de data base. Dit account heeft machtigingen nodig voor de data base.
* **SQL-verificatie** : de verificatie database maakt gebruik van de gebruikers naam en het wacht woord die zijn gedefinieerd voor het connectiviteits scherm om verbinding te maken met de data base. Als u de gebruikers naam-wachtwoord in het DSN-bestand opslaat, hebben de referenties die op het connectiviteits scherm worden gegeven prioriteit.
* **Azure SQL database-verificatie** : Zie [verbinding maken met SQL database met behulp van Azure Active Directory-verificatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)voor meer informatie.

**DN is anker** : als u deze optie selecteert, wordt de DN ook gebruikt als het anker kenmerk. Het kan worden gebruikt voor een eenvoudige implementatie, maar heeft ook de volgende beperking:

* Connector ondersteunt slechts één object type. Verwijzings kenmerken kunnen daarom alleen verwijzen naar hetzelfde object type.

**Export type: object vervangen** : tijdens het exporteren, wanneer slechts enkele kenmerken zijn gewijzigd, wordt het volledige object met alle kenmerken geëxporteerd en wordt het bestaande object vervangen.

### <a name="schema-1-detect-object-types"></a>Schema 1 (object typen detecteren)
Op deze pagina gaat u configureren hoe de connector de verschillende object typen in de data base gaat vinden.

Elk object type wordt weer gegeven als een partitie en is verder geconfigureerd op het **configureren van partities en hiërarchieën** .

![schema1a](./media/microsoft-identity-manager-2016-connector-genericsql/schema1a.png)

**Detectie methode voor object type** : de connector ondersteunt deze detectie methoden voor object typen.

* **Vaste waarde** : u geeft de lijst met object typen met een door komma's gescheiden lijst. Bijvoorbeeld: `User,Group,Department`.  
  ![schema1b](./media/microsoft-identity-manager-2016-connector-genericsql/schema1b.png)
* **Tabel/weer gave/opgeslagen procedure** : Geef de naam op van de tabel/weer gave/opgeslagen procedure en vervolgens de naam van de kolom die de lijst met object typen levert. Als u een opgeslagen procedure gebruikt, geeft u in de indeling **[naam]: [Direction]: [waarde]** ook para meters op. Geef elke para meter op een afzonderlijke regel (gebruik CTRL + ENTER om een nieuwe regel op te halen).  
  ![schema1c](./media/microsoft-identity-manager-2016-connector-genericsql/schema1c.png)
* **SQL-query** : met deze optie kunt u een SQL-query opgeven die één kolom met object typen retourneert, bijvoorbeeld `SELECT [Column Name] FROM TABLENAME` . De geretourneerde kolom moet van het type String (varchar) zijn.

### <a name="schema-2-detect-attribute-types"></a>Schema 2 (kenmerk typen detecteren)
Op deze pagina gaat u configureren hoe de kenmerk namen en typen worden gedetecteerd. De configuratie opties worden weer gegeven voor elk object type dat op de vorige pagina is gedetecteerd.

![schema2a](./media/microsoft-identity-manager-2016-connector-genericsql/schema2a.png)

**Detectie methode voor kenmerk type** : de connector ondersteunt deze detectie methoden voor kenmerk typen met elk gedetecteerd object type in het scherm schema 1.

* **Tabel/weer gave/opgeslagen procedure** : Geef de naam op van de tabel/weer gave/opgeslagen procedure die moet worden gebruikt om de kenmerk namen te vinden. Als u een opgeslagen procedure gebruikt, geeft u in de indeling **[naam]: [Direction]: [waarde]** ook para meters op. Geef elke para meter op een afzonderlijke regel (gebruik CTRL + ENTER om een nieuwe regel op te halen). Als u de kenmerk namen in een kenmerk met meerdere waarden wilt detecteren, geeft u een door komma's gescheiden lijst met tabellen of weer gaven op. Scenario's met meerdere waarden worden niet ondersteund wanneer bovenliggende en onderliggende tabellen dezelfde kolom namen hebben.
* **SQL-query** : met deze optie kunt u een SQL-query opgeven waarmee één kolom met kenmerk namen wordt geretourneerd `SELECT [Column Name] FROM TABLENAME` . De geretourneerde kolom moet van het type String (varchar) zijn.

### <a name="schema-3-define-anchor-and-dn"></a>Schema 3 (anker en DN definiëren)
Op deze pagina kunt u het kenmerk Anchor en DN configureren voor elk gedetecteerd object type. U kunt meerdere kenmerken selecteren om het anker uniek te maken.

![schema3a](./media/microsoft-identity-manager-2016-connector-genericsql/schema3a.png)

* Kenmerken met meerdere waarden en Booleaanse waarde worden niet weer gegeven.
* Hetzelfde kenmerk kan niet worden gebruikt voor DN en anker, tenzij **DN-anker is** geselecteerd op de pagina connectiviteit.
* Als **DN-anker** is geselecteerd op de pagina connectiviteit, vereist deze pagina alleen het kenmerk DN. Dit kenmerk wordt ook gebruikt als het anker kenmerk.

  ![schema3b](./media/microsoft-identity-manager-2016-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Schema 4 (kenmerk type, verwijzing en richting definiëren)
Op deze pagina kunt u het kenmerk type configureren, zoals geheel getal, binair, of Booleaans, en richting voor elk kenmerk. Alle kenmerken van pagina **schema 2** worden vermeld, inclusief kenmerken met meerdere waarden.

![schema4a](./media/microsoft-identity-manager-2016-connector-genericsql/schema4a.png)

* **Data type** : wordt gebruikt om het kenmerk type toe te wijzen aan de typen die bekend zijn bij de synchronisatie-engine. De standaard instelling is het gebruik van hetzelfde type zoals gedetecteerd in het SQL-schema, maar DateTime en references zijn niet eenvoudig te detecteren. Voor die, moet u **DateTime** of **Reference** opgeven.
* **Richting** : u kunt de kenmerk richting instellen op importeren, exporteren of ImportExport. ImportExport is standaard.

![schema4b](./media/microsoft-identity-manager-2016-connector-genericsql/schema4b.png)

Opmerkingen:

* Als een kenmerk type niet kan worden gedetecteerd door de connector, wordt het gegevens type teken reeks gebruikt.
* **Geneste tabellen** kunnen worden beschouwd als database tabellen met één kolom. Oracle slaat de rijen van een geneste tabel in een bepaalde volg orde op. Wanneer u de geneste tabel echter ophaalt in een PL/SQL-variabele, krijgen de rijen opeenvolgende subscripten, te beginnen bij 1. Hiermee krijgt u matrix-achtige toegang tot afzonderlijke rijen.
* **VARRYS** worden niet ondersteund in de connector.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schema 5 (partitie voor referentie kenmerken definiëren)
Op deze pagina configureert u alle referentie kenmerken van de partitie (object type) waarnaar een kenmerk verwijst.

![schema5](./media/microsoft-identity-manager-2016-connector-genericsql/schema5.png)

Als u **DN-anker** gebruikt, moet u hetzelfde object type gebruiken als de account die u wilt verwijzen. U kunt niet verwijzen naar een ander object type.

> [!NOTE]
> Vanaf de update van maart 2017 is er nu een optie voor ' * ' wanneer u deze optie kiest, worden alle mogelijke typen leden geïmporteerd.

![globalparameters3](./media/microsoft-identity-manager-2016-connector-genericsql/any-option.png)

> [!IMPORTANT]
>  Vanaf mei 2017 is de ' \* ' ook wel **een optie** is gewijzigd om de import-en export stroom te ondersteunen. Als u deze optie wilt gebruiken, moet de tabel of weer gave met meerdere waarden een kenmerk met het object type bevatten.

![](./media/microsoft-identity-manager-2016-connector-genericsql/any-02.png)

 </br> Als "*" is geselecteerd, moet de naam van de kolom met het object type ook worden opgegeven.</br> ![](./media/microsoft-identity-manager-2016-connector-genericsql/any-03.png)

Na het importeren ziet u iets dat lijkt op de onderstaande afbeelding:

  ![globalparameters3](./media/microsoft-identity-manager-2016-connector-genericsql/after-import.png)



### <a name="global-parameters"></a>Globale parameters
De pagina globale para meters wordt gebruikt voor het configureren van de Delta-import, de datum-en tijd notatie en de wachtwoord methode.

![globalparameters1](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters1.png)



De algemene SQL-connector ondersteunt de volgende methoden voor het importeren van verschillen:

* **Trigger** : Zie voor het [genereren van Delta weergaven met triggers](https://technet.microsoft.com/library/cc708665.aspx).
* **Water merk** : een algemene benadering die met elke Data Base kan worden gebruikt. De watermerk query is vooraf ingevuld op basis van de leverancier van de data base. Er moet een watermerk kolom aanwezig zijn op elke gebruikte tabel/weer gave. In deze kolom moeten de ingevoegde en updates van de tabellen worden gevolgd als de afhankelijke tabellen (meerdere waarden of onderliggende). De klokken tussen de synchronisatie service en de database server moeten worden gesynchroniseerd. Als dat niet het geval is, kunnen sommige vermeldingen in de Delta-import worden wegge laten.  
  Beperkt
  * De watermerk strategie biedt geen ondersteuning voor verwijderde objecten.
* **Moment opname** : (werkt alleen met Microsoft SQL Server) [Delta weergaven genereren met behulp van moment opnamen](https://technet.microsoft.com/library/cc720640.aspx)
* **Wijzigingen bijhouden** : (werkt alleen met Microsoft SQL Server) [over wijzigingen bijhouden](https://msdn.microsoft.com/library/bb933875.aspx)  
  Beperkingen:
  * Het kenmerk Anchor & DN moet deel uitmaken van de primaire sleutel voor het geselecteerde object in de tabel.
  * SQL-query wordt niet ondersteund tijdens het importeren en exporteren met Wijzigingen bijhouden.

**Aanvullende para meters** : Geef de tijd zone van de database server op die aangeeft waar de database server zich bevindt. Deze waarde wordt gebruikt ter ondersteuning van de verschillende notaties voor datum-& tijd kenmerken.

De connector slaat de datum en tijd in UTC-notatie altijd op. De tijd zone van de database server en de gebruikte indeling moet worden opgegeven om de datum en tijd correct te kunnen converteren. De notatie moet worden uitgedrukt in de .net-indeling.

Elk datum tijd kenmerk dat tijdens het exporteren moet worden toegekend aan de connector in UTC-tijd notatie.

![globalparameters2](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters2.png)

**Wachtwoord configuratie** : de connector biedt mogelijkheden voor wachtwoord synchronisatie en ondersteunt het instellen en wijzigen van het wacht woord.

De connector biedt twee methoden om wachtwoord synchronisatie te ondersteunen:

* **Opgeslagen procedure** : voor deze methode zijn twee opgeslagen procedures vereist ter ondersteuning van set & wacht woord wijzigen. Typ alle para meters voor toevoegen en wijzigen van de wachtwoord bewerking in **set Password SP** en wijzig de para meters van het **wacht woord** in respectievelijk volgens onderstaande voor beeld.
  ![globalparameters3](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters3.png)
* **Wachtwoord extensie** : voor deze methode is een wachtwoord extensie-DLL vereist (u moet de naam van de extensie-DLL opgeven die de [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) -interface implementeert). De assembly voor wachtwoord uitbreiding moet in een extensie map worden geplaatst, zodat de connector de DLL kan laden tijdens runtime.
  ![globalparameters4](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters4.png)

U moet ook het wachtwoord beheer inschakelen op de pagina **uitbrei ding configureren** .
![globalparameters5](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Partities en hiërarchieën configureren
Selecteer alle object typen op de pagina partities en hiërarchieën. Elk object type is een eigen partitie.

![partitions1](./media/microsoft-identity-manager-2016-connector-genericsql/partitions1.png)

U kunt ook de waarden overschrijven die zijn gedefinieerd op de pagina **connectiviteit** of **globale para meters** .

![partitions2](./media/microsoft-identity-manager-2016-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Ankers configureren
Deze pagina is alleen-lezen omdat het anker al is gedefinieerd. Het geselecteerde anker kenmerk wordt altijd toegevoegd met het object type om ervoor te zorgen dat het uniek blijft in verschillende object typen.

![ankers](./media/microsoft-identity-manager-2016-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Para meter run Step configureren
Deze stappen worden geconfigureerd op de uitvoerings profielen op de connector. Deze configuraties doen het werkelijke werk van het importeren en exporteren van gegevens.

### <a name="full-and-delta-import"></a>Volledige en Delta-import
Algemene SQL-connector biedt ondersteuning voor volledige en Delta-import met behulp van de volgende methoden:

* Tabel
* Weergave
* Opgeslagen procedure
* SQL-query

![runstep1](./media/microsoft-identity-manager-2016-connector-genericsql/runstep1.png)

**Tabel/weer gave**  
Als u kenmerken met meerdere waarden voor een object wilt importeren, moet u de naam van de tabel/weer gave opgeven in de **naam van de tabel of views met meerdere waarden** en de bijbehorende voor waarden in de **voor waarde voor samen** voegen met de bovenliggende tabel. Als er meer dan één tabel met meerdere waarden in de gegevens bron is, kunt u deze samen voegen tot één weer gave.

> [!IMPORTANT]
> De algemene SQL-beheer agent kan worden gebruikt met één tabel met meerdere waarden. De naam van een tabel of weer gave met meerdere waarden niet meer dan één naam van de tabel. Dit is de beperking van algemene SQL.


Voor beeld: u wilt het werknemers object en alle kenmerken met meerdere waarden importeren. Er zijn twee tabellen: werk nemer (hoofd tabel) en afdeling (meerdere waarden).
Ga als volgt te werk:

* Typ **werk nemer** in **tabel/weer gave/SP** .
* Typ afdeling in **naam van tabel/weer gaven met meerdere waarden** .
* Typ bijvoorbeeld de samenvoegings voorwaarde tussen werk nemer & afdeling in de **samenvoeg voorwaarde** `Employee.DEPTID=Department.DepartmentID` .
  ![runstep2](./media/microsoft-identity-manager-2016-connector-genericsql/runstep2.png)

**Opgeslagen procedures**  
![runstep3](./media/microsoft-identity-manager-2016-connector-genericsql/runstep3.png)

* Als u veel gegevens hebt, is het raadzaam om paginering te implementeren met uw opgeslagen procedures.
* Voor de opgeslagen procedure om paginering te ondersteunen, moet u begin index en eind index opgeven. Zie: [efficiënt pagingen door grote hoeveel heden gegevens](https://msdn.microsoft.com/library/bb445504.aspx).
* @StartIndex en @EndIndex worden tijdens uitvoerings tijd vervangen door de waarde voor de bijbehorende pagina grootte die is geconfigureerd op de pagina **stap configureren** . Als de connector bijvoorbeeld de eerste pagina ophaalt en de pagina grootte is ingesteld op 500, is dit in een dergelijke situatie @StartIndex 1 en @EndIndex 500. Deze waarden nemen toe wanneer de connector volgende pagina's ophaalt en de @StartIndex & @EndIndex waarde wijzigt.
* Als u een opgeslagen procedure met para meters wilt uitvoeren, geeft u de para meters op in de `[Name]:[Direction]:[Value]` indeling. Voer elke para meter op een afzonderlijke regel in (gebruik CTRL + ENTER om een nieuwe regel op te halen).
* Algemene SQL-connector biedt ook ondersteuning voor de import bewerking van gekoppelde servers in Microsoft SQL Server. Als informatie moet worden opgehaald uit een tabel in een gekoppelde server, moet de tabel worden verstrekt in de volgende indeling: `[ServerName].[Database].[Schema].[TableName]`
* Algemene SQL-connector ondersteunt alleen objecten met een vergelijk bare structuur (zowel de naam van de alias als het gegevens type) tussen het uitvoeren van stappen en de detectie van schema's. Als het geselecteerde object uit het schema en de opgegeven informatie tijdens de uitvoerings stap verschilt, kan SQL connector dit type scenario's niet ondersteunen.

**SQL-query**  
![runstep4](./media/microsoft-identity-manager-2016-connector-genericsql/runstep4.png)

![runstep5](./media/microsoft-identity-manager-2016-connector-genericsql/runstep5.png)

* Query's met meerdere resultaten sets worden niet ondersteund.
* SQL query ondersteunt de paginering en biedt start index en eind index als een variabele ter ondersteuning van paginering.

### <a name="delta-import"></a>Delta-Import
![runstep6](./media/microsoft-identity-manager-2016-connector-genericsql/runstep6.png)

De Delta-Import configuratie vereist een aantal meer configuratie vergeleken met volledige import.

* Als u de aanpak van de trigger of de moment opname kiest voor het bijhouden van wijzigingen in de Delta, geeft u de geschiedenis tabel of momentopname database op in de **geschiedenis tabel of in de database naam van de moment opname** .
* U moet ook een samenvoegings voorwaarde opgeven tussen de geschiedenis tabel en de bovenliggende tabel, bijvoorbeeld `Employee.ID=History.EmployeeID`
* Als u de trans actie voor de bovenliggende tabel uit de geschiedenis tabel wilt bijhouden, moet u de naam van de kolom opgeven die de bewerkings gegevens bevat (toevoegen/bijwerken/verwijderen).
* Als u water merk kiest om Delta wijzigingen bij te houden, geeft u de naam van de kolom op die de bewerkings gegevens in de **kolom naam water Mark** bevat.
* De kenmerk kolom van het **type wijzigen** is vereist voor het wijzigings type. Deze kolom wijst een wijziging die in de primaire tabel of tabel met meerdere waarden voor komt, toe aan een wijzigings type in de weer gave Delta. Deze kolom kan het Modify_Attribute wijzigings type voor wijziging op kenmerk niveau bevatten of een wijzigings type toevoegen, wijzigen of verwijderen voor een wijzigings type op object niveau. Als het iets anders is dan de standaard waarde toevoegen, wijzigen of verwijderen, kunt u deze waarden definiëren met deze optie.

### <a name="export"></a>Exporteren
![runstep7](./media/microsoft-identity-manager-2016-connector-genericsql/runstep7.png)

Ondersteuning voor het exporteren van algemene SQL-connectors met vier ondersteunde methoden, zoals:

* Tabel
* Weergave
* Opgeslagen procedure
* SQL-query

**Tabel/weer gave**  
Als u de optie tabel/weer gave kiest, genereert de connector de respectievelijke query's om de export uit te voeren.

**Opgeslagen procedures**  
![runstep8](./media/microsoft-identity-manager-2016-connector-genericsql/runstep8.png)

Als u kiest voor de optie opgeslagen procedure, zijn voor het exporteren drie verschillende opgeslagen procedures vereist om bewerkingen voor invoegen/bijwerken/verwijderen uit te voeren.

* **SP-naam toevoegen** : deze SP wordt uitgevoerd als een van de objecten in de betreffende tabel wordt geconnectord voor invoeging.
* **SP-naam bijwerken** : deze SP wordt uitgevoerd als een van de objecten in de betreffende tabel wordt bijgewerkt naar de connector.
* **SP-naam verwijderen** : deze SP wordt uitgevoerd als een van de objecten in de betreffende tabel worden verwijderd.
* Het kenmerk dat is geselecteerd in het schema dat wordt gebruikt als parameter waarde voor de opgeslagen procedure. Bijvoorbeeld `@EmployeeName: INPUT: EmployeeName` (Werknemersnaam is geselecteerd in het connector schema en de connector vervangt de respectieve waarde tijdens het exporteren)
* Als u een opgeslagen procedure met para meters wilt uitvoeren, geeft u para meters op in `[Name]:[Direction]:[Value]` indeling. Voer elke para meter op een afzonderlijke regel in (gebruik CTRL + ENTER om een nieuwe regel op te halen).

**SQL-query**  
![runstep9](./media/microsoft-identity-manager-2016-connector-genericsql/runstep9.png)

Als u de SQL-query optie kiest, zijn voor export drie verschillende query's vereist om bewerkingen voor invoegen/bijwerken/verwijderen uit te voeren.

* **Invoeg query** : deze query wordt uitgevoerd als een object naar de connector komt voor invoeging in de desbetreffende tabel.
* **Update query** : deze query wordt uitgevoerd als een van de objecten in de betreffende tabel wordt bijgewerkt naar de connector.
* **Query verwijderen** : deze query wordt uitgevoerd als een wille keurig object in de desbetreffende tabel wordt verwijderd.
* Het kenmerk dat is geselecteerd in het schema dat wordt gebruikt als parameter waarde voor de query, bijvoorbeeld `Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Problemen oplossen
* Zie voor meer informatie over het inschakelen van logboek registratie voor het oplossen van problemen met de connector, het [inschakelen van etw-tracering voor connectors](https://go.microsoft.com/fwlink/?LinkId=335731).
