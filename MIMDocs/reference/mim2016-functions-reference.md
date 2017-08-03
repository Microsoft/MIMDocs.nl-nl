---
title: Documentatie voor Microsoft Identity Manager 2016 werkt | Microsoft Docs
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 8f36cf981971db0d6c55fc17cce874a8faf0ecaf
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2017
---
# <a name="functions-reference-for-microsoft-identity-manager-2016"></a>Naslaginformatie over functions voor Microsoft Identity Manager 2016


In Microsoft Identity Manager (MIM) 2016, met functies kunt u wijzigen kenmerkwaarden voorafgaand aan dat ze aan een doel in een functieactiviteit of declaratieve inrichting. Het doel van dit document is om u te bieden een overzicht van de beschikbare functies en een beschrijving van hoe u ze kunt gebruiken.

Kenmerkstroomtoewijzingen configureren is een elementaire taak bij het configureren van synchronisatieregels. De eenvoudigste vorm van een kenmerk stroomtoewijzing is een rechtstreekse koppeling. Zoals wordt aangegeven door de naam, wordt een rechtstreekse koppeling heeft de waarde van een kenmerk van de gegevensbron en toegepast op het kenmerk geconfigureerde bestemming. Er zijn ook gevallen waarbij u ofwel moet bestaande kenmerkwaarden worden gewijzigd of nieuwe kenmerkwaarden moet worden berekend voordat het systeem op een doel toegepast.

Functies zijn ingebouwde methode gebruikt voor het definiëren van het type wijziging u de synchronisatie-engine moet worden toegepast moet wanneer een kenmerkwaarde voor een doel te genereren.

In MIM, kunt u de bestaande functies groeperen in de volgende categorieën:

-   **Functies voor het bewerken van gegevens**. Functies uit te voeren van tal van bewerkingen voor het manipuleren van tekenreeksen.

-   **Functies voor het ophalen van gegevens**. Functies om gegevens te extraheren uit de kenmerkwaarden.

-   **Functies voor het genereren van gegevens**. Functies om waarden te genereren.

-   **Bedrijfslogica-functies**. De functies op basis van voorwaarden bewerkingen uit te voeren.

De volgende secties vindt u meer informatie over de functies in elke categorie.

## <a name="data-manipulation-functions"></a>Functies voor het bewerken van gegevens

Functies voor het bewerken van gegevens worden gebruikt voor het uitvoeren van bewerkingen voor het manipuleren van tekenreeksen allerlei.

| Samenvoegen        |   |
|--------------------|-------------------------|
| Beschrijving        | De functie wordt gebruikt om twee of meer tekenreeksen samen te voegen.                                                                                                       |
| Functiehandtekening | tekenreeks1 + tekenreeks2...                                                                                                                                                     |
| Invoer             | Twee of meer tekenreeksen                                                                                                                                                        |
| Bewerkingen         | Alle invoerreeks parameters worden samengevoegd met elkaar.                                                                                                              |
| Uitvoer             | Een tekenreeks        |


| Hoofdletters         |         |
|-------------------|---------|
| Beschrijving        | De functie HOOFDLETTERS converteert alle tekens in een tekenreeks naar hoofdletters.         |
| Functiehandtekening | Tekenreeks UpperCase(string)                                                                                                                                                   |
| Invoer             | Een tekenreeks                                                                                                                                                                 |
| Bewerkingen         | Alle kleine letters van de invoerparameter worden geconverteerd naar hoofdletters. Voorbeeld: UpperCase("test") resulteert in 'TEST'.                                     |
| Uitvoer             | Een tekenreeks                                                              |


| Kleine letters          |                                 |
|--------------------|---------------------------------|
| Beschrijving        | De functie kleine converteert alle tekens in een tekenreeks naar kleine letters.                                                                                                  |
| Functiehandtekening | Tekenreeks LowerCase(string)                                                                                                                                                   |
| Invoer             | Een tekenreeks                                                                                                                                                                 |
| Bewerkingen         | Alle hoofdletters van de invoerparameter worden geconverteerd naar kleine letters. Voorbeeld: LowerCase("TeSt") resulteert in 'test'.                                     |
| Uitvoer             | Een tekenreeks               |


| ProperCase        |                                                          |
|-------------------|----------------------------------------------------------|
| Beschrijving        | De ProperCase functioneren zet het eerste teken van elk woord door spaties gescheiden in een tekenreeks naar hoofdletters en alle overige tekens worden geconverteerd naar kleine letters.           |
| Functiehandtekening | Tekenreeks ProperCase(string)                                                                                                                                                  |
| Invoer             | Een tekenreeks                                                                                                                                                                 |
| Bewerkingen         | Het eerste teken van elk woord door spaties gescheiden in de invoerparameter is omgezet in hoofdletters en alle hoofdletters worden geconverteerd naar kleine letters. Als een woord in de invoerparameter met een niet-karakters begint, wordt het eerste teken van het woord niet geconverteerd naar hoofdletters. <br/> Voorbeelden: <br/> -ProperCase("TEsT") resulteert in 'Test'. <br/> -ProperCase("britta simon") resulteert in 'Britta Simon'. <br/>-ProperCase("TEsT") resulteert in 'Test'. <br/> -ProperCase("\$TEsT") resulteert in '\$Test '.|
| Uitvoer             | Een tekenreeks      |


| LTrim              |      |
|--------------------|------|
| Beschrijving        | De functie LTrim verwijdert voorloopspaties wit uit een tekenreeks.                                                                                                             |
| Functiehandtekening | Tekenreeks LTrim(string)                                                                                                                                                       |
| Invoer             | Een tekenreeks                                                                                                                                                                 |
| Bewerkingen         | De toonaangevende spatietekens opgenomen in de invoerparameter worden verwijderd. <br/><br/>Voorbeeld: LTrim ('Test') resulteert in 'Test'.                                              |
| Uitvoer             | Een tekenreeks      |



| RTrim              |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Beschrijving        | De functie RTrim verwijdert de afsluitende spaties uit een tekenreeks.                                                                 |
| Functiehandtekening | Tekenreeks RTrim(string)                                                                                                            |
| Invoer             | Een tekenreeks                                                                                                                      |
| Bewerkingen         | De afsluitende spaties in de invoerparameter is opgenomen, worden verwijderd. Voorbeeld: RTrim ('Test') resulteert in 'Test'.  |
| Uitvoer             | Een tekenreeks                                                                                                                      |


| Trim               |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Beschrijving        | De functie spaties verwijdert voorloopspaties en afsluitende spaties uit een tekenreeks.                                                      |
| Functiehandtekening | Tekenreeks Trim(string)                                                                                                             |
| Invoer             | Een tekenreeks                                                                                                                      |
| Bewerkingen         | De voorloopspaties en afsluitende spaties opgenomen in de tekenreeks worden verwijderd. Voorbeeld: Trim ('Test') resulteert in 'Test'. |
| Uitvoer             | Een tekenreeks                                                                                                                      |




| RightPad           |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Beschrijving        | De RightPad functie rechts-pad een tekenreeks aan een opgegeven lengte met behulp van een opgegeven opvulteken.                          |
| Functiehandtekening | Tekenreeks RightPad (tekenreeks, lengte, padCharacter)                                                                                   |
| Bewerkingen         | Als de lengte van tekenreeks kleiner dan de lengte is, vervolgens padCharacter herhaaldelijk toegevoegd aan het einde van tekenreeks totdat er een lengte die gelijk zijn aan lengte. <br/> Voorbeelden: <br/> -RightPad("User", 10, "0") zou leiden tot 'User000000'. <br/> -RightPad(RandomNum(1,10), 5, "0") kan leiden tot '9000'.   |
| Uitvoer                                                                                                                                                          | Als de tekenreeks een lengte groter dan of gelijk zijn aan lengte heeft, wordt een tekenreeks die identiek is aan de tekenreeks geretourneerd. Als de lengte van tekenreeks kleiner dan de lengte is, wordt een nieuwe tekenreeks van de gewenste lengte met tekenreeks opgevuld met een padCharacter geretourneerd. Als null-tekenreeks is, retourneert de functie een lege tekenreeks. |   |   |
>[!NOTE]
**padCharacter** kan een spatie, maar dit kan een null-waarde niet. Als de lengte van **tekenreeks** is gelijk aan of groter zijn dan **lengte**, **tekenreeks** ongewijzigd geretourneerd.


| LeftPad      |     |
|----|-------|
| Beschrijving  | De LeftPad functie links-pad een tekenreeks aan een opgegeven lengte met behulp van een opgegeven opvulteken.    |
| Functiehandtekening      | Tekenreeks LeftPad (tekenreeks, lengte, padCharacter)     |
| Invoer |  - **De tekenreeks.** De tekenreeks worden opgevuld. <br/> - **lengte.** Een geheel getal dat de gewenste lengte van tekenreeks. <br/> - **padCharacter.** Een tekenreeks die bestaat uit een enkel teken moet worden gebruikt als een teken pad. |
| Bewerkingen  | Als de lengte van tekenreeks kleiner dan de lengte is, vervolgens padCharacter herhaaldelijk toegevoegd aan het begin van tekenreeks totdat er een lengte die gelijk zijn aan lengte. <br/> Voorbeelden: <br/> -LeftPad("User", 10, "0") zou leiden tot '000000User'. <br/> LeftPad(RandomNum(1,10), 5, '0') kan leiden tot '0009'. |  
|Uitvoer | Als de tekenreeks een lengte groter dan of gelijk zijn aan lengte heeft, wordt een tekenreeks die identiek is aan de tekenreeks geretourneerd. <br/> Als de lengte van tekenreeks kleiner dan de lengte is, wordt een nieuwe tekenreeks van de gewenste lengte met tekenreeks opgevuld met een padCharacter geretourneerd. <br/>  Als **tekenreeks** is null, de functie retourneert een lege tekenreeks.                                                   |

<[!NOTE]
**padCharacter** kan een spatie, maar dit kan een null-waarde niet. Als de lengte van **tekenreeks** is gelijk aan of groter zijn dan **lengte**, **tekenreeks** ongewijzigd geretourneerd.

| BitOr    |  |
|----- |------|
| Beschrijving  | De functie BitOr wordt een opgegeven bits ingesteld op een markering op 1.     |
| Functiehandtekening  | Int BitOr(mask, flag)       |  
| Invoer     | 1. **masker.** Een hexadecimale waarde waarmee de bits op de vlag instellen. <br/> 2. **vlag.** Een hexadecimale waarde die is dat een specifieke bits gewijzigd.    |   
| Bewerkingen         | Deze functie worden beide parameters geconverteerd naar binaire voorstelling en vergelijkt ze: <br/> -Instellen iets op 1 als een of beide van de bijbehorende bits in masker en markering tussen 1 liggen en 0 als de bijbehorende bits 0 zijn. <br/> -Deze retourneert 1 in alle gevallen behalve wanneer de bijbehorende bits van beide parameters 0. <br/> -De resulterende bitpatroon 'instellen' (1 of true) zijn de bits is van een van de twee operanden. Meerdere vlaggenbits kunnen worden ingesteld als meerdere bits de waarde 1 in het masker hebben.  |
| Uitvoer             | Een nieuwe versie van **vlag**, met de opgegeven in bits **masker** ingesteld op 1.                   |


| BitAnd             |                                                                                    |
|--------------------|------------------------------------------------------------------------------------|
| Beschrijving        | De functie BitAnd wordt een opgegeven bits op een markering op 0 ingesteld.                           |
| Functiehandtekening | Int BitOr(mask, flag)                                                              |
| Invoer             | 1. **masker.** Een hexadecimale waarde die aangeeft van de bit te wijzigen voor de vlag. <br/> 2. **vlag.** Een hexadecimale waarde die een specifieke bits gewijzigd   |
| Bewerkingen         | Deze functie worden beide parameters geconverteerd naar binaire voorstelling en vergelijkt ze: <br/> -Instellen iets op 0 als één of beide van de bijbehorende bits in **masker** en **vlag** 0 en 1 als beide van de bijbehorende bits 1 zijn. <br/> -Het resultaat 0 in alle gevallen behalve wanneer de bijbehorende bits van beide parameters 1. Meerdere vlaggenbits kunnen worden ingesteld op 0 als meerdere bits hebben de waarde 0 in **masker.** |
| Uitvoer             | Een nieuwe versie van **vlag** met de opgegeven in bits **masker** ingesteld op 0.                    |

| DateTimeFormat                                 |    |
|---------------------------------------|------------|
| Beschrijving       | De functie DateTimeFormat gebruiken om een datetime-waarde in de vorm van een tekenreeks naar een specifieke indeling.     |
| Functiehandtekening   | Tekenreeks DateTimeFormat (dateTime, -indeling)      |
| Invoer   | 1. dateTime-waarde. Een tekenreeks die de datum/tijd opmaken vertegenwoordigt.  <br/> 2. **indeling.** Een tekenreeks die de indeling converteren naar vertegenwoordigt.  |   
| Bewerkingen           | De indelingstekenreeks die is opgegeven in de indeling wordt toegepast op de datum/tijd in de datum/tijd-tekenreeks. <br/> De tekenreeks die is opgegeven in de indeling moet een geldige datum / tijdindeling. Als dit niet het geval is, wordt er een fout geretourneerd dat de indeling niet een geldige datum / tijdindeling is. <br/> Voorbeeld: DateTime ("25-12/2007 ', ' jjjj-MM-dd ') resulteert in '2007-12-25'.|   
| Uitvoer     | Een tekenreeks die voortvloeit uit **indeling** naar **dateTime.**   |

>[!Note]                                                                                                                                                                             
Zie voor de tekens voor het maken van de gebruiker gedefinieerde indelingen worden geaccepteerd, [door de gebruiker gedefinieerde datum/tijd-indeling](http://go.microsoft.com/fwlink/?LinkId=195182)


| ConvertSidToString       |    |   
|--------------------------|----|
| Beschrijving       | De ConvertSidToString converteert een bytematrix die een beveiligings-id in een tekenreeks.         |
| Functiehandtekening      | Tekenreeks ConvertSidToString(ObjectSID)    |
| Invoer  | **ObjectSID.** Een bytematrix met een beveiligings-id (SID).   |
| Bewerkingen    | De opgegeven binaire SID is geconverteerd naar een tekenreeks.    |
| Uitvoer              | Een tekenreeksrepresentatie van de SID.   |  

| ConvertStringToGuid |        |
|---------------------|--------|
| Beschrijving         | **De ConvertStringToGuid** functie de tekenreeksweergave van een GUID converteert naar een binaire voorstelling van de GUID.      |
| Functie            | Byte [] ConvertStringToGuid(stringGuid)  |  
| Invoer              | **GUID.** Een tekenreeks in dit patroon worden opgemaakt: **xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx**, waarbij de waarde van de GUID wordt weergegeven als een reeks hexadecimale cijfers in groepen van 8, 4, 4, 4 en 12 cijfers en door koppeltekens gescheiden. Een voorbeeld van een geretourneerde waarde is '382c74c3-721d-4f34-80e557657b6cbc27'.  |
| Bewerkingen          | De tekenreeks **Guid** opgegeven in de parameter 1 wordt geconverteerd naar de binaire weergave. <br/> De tekenreeks is niet als een representatie van een geldige **Guid**, de functie wordt geweigerd voor het argument met de volgende fout: <br/> **De parameter voor de functie ConvertStringToGuid moet een tekenreeks voor een geldige Guid zijn.**  |
| Uitvoer              | Een binaire voorstelling van de Guid.           |                                                                           


| ReplaceString       |     |
|--------------------|-------|
| Beschrijving         | De functie ReplaceString vervangt alle instanties van een tekenreeks naar een andere tekenreeks.  |   
| Functie            | Tekenreeks ReplaceString (string, OldValue, NewValue)    |                                                                          
| Invoer              | 1. **De tekenreeks.** Een tekenreeks in de vervangende waarden. <br/> 2. **OldValue.** De tekenreeks om naar te zoeken en vervangen. <br/> 3. NewValue. De tekenreeks te vervangen. |
| Bewerkingen          | Alle instanties van OldValue in de tekenreeks worden vervangen door NewValue. De functie moet verwerken van de volgende speciale tekens bevatten: <br/> - **\n.** Nieuwe regel. <br/> - **\r.** Regelterugloop. <br/> - **\t.** Tabblad. <br/> Voorbeeld: ReplaceString ('One\n\rMicrosoft\n\r\Way', '\n\r',' ') retourneert 'One Microsoft Way'. |   
| Uitvoer              | Een tekenreeks zijn met alle instanties van **OldValue** in de tekenreeks die wordt vervangen door **NewValue.**      |

## <a name="data-retrieval-functions"></a>Functies voor het ophalen van gegevens

Functies voor het ophalen van gegevens worden gebruikt voor bewerkingen waarmee de gewenste tekens worden opgehaald uit een tekenreeks.

| Word       |        |
|--------------------|---------------|
| Beschrijving        | De functie woord retourneert een woord dat voorkomt in een tekenreeks, op basis van parameters met een beschrijving van de scheidingstekens moet gebruiken en het aantal word om terug te keren.                                                                |
| Functiehandtekening | Tekenreeks Word (tekenreeks, getal, scheidingstekens)                                                                                                                                                                        |
| Invoer             | 1. **tekenreeks.** De tekenreeks waaruit een woord. <br/> 2. **getal.** Een getal dat aangeeft welke word-nummer moet worden geretourneerd. <br/> 3. **delimeters.** Een tekenreeks die de delimeters die moeten worden gebruikt om te identificeren woorden vertegenwoordigt. |
| Bewerkingen         | Elke reeks tekens in een tekenreeks die is gescheiden door een van de tekens in scheidingstekens wordt geïdentificeerd als een woord. Het woord dat is gevonden op de positie die is opgegeven in de parameter 3 (number) wordt geretourneerd: <br/> -Als nummer < 1, kunt u een lege tekenreeks retourneren. <br/> -Als null-tekenreeks is, retourneert een lege tekenreeks. <br/><br/> Voorbeelden: <br/> 1. Word (' testen; van de functie %; ', 3,'; $& %") retourneert 'functie'. <br/> 2. Word (' testen; De functie', 2, ';') retourneert ' ' (een lege tekenreeks). 3. Word (' testen; van de functie %; ", 0,"; $& %") retourneert ' '(een lege tekenreeks).
| Uitvoer             | Een tekenreeks met het woord op de positie van de gebruiker gevraagd. Als **tekenreeks** bevat minder dan het aantal woorden of **tekenreeks** bevat niet alle woorden die zijn geïdentificeerd door **delimeters,** een lege tekenreeks geretourneerd. |  


| Links               |   |
|-------|-------|
| Beschrijving        | De functie links retourneert een opgegeven aantal tekens vanaf de linkerkant van een tekenreeks.       |
| Functiehandtekening | Tekenreeks Left (tekenreeks, numChars)     |
| Invoer             | 1. **tekenreeks.** De tekenreeks waaruit tekens. 2. **numChars.** Een nummer van het aantal tekens retourneren vanaf het begin van een tekenreeks.         |
| Bewerkingen         | **numChars** tekens uit de eerste positie van de tekenreeks worden geretourneerd. <br/> Voorbeeld: Left ('Britta Simon', 3) retourneert 'Bri'.   |
| Uitvoer             | Een tekenreeks met de eerste numChars tekens in de tekenreeks.  <br/> -Als numChars = 0, een lege tekenreeks retourneren. <br/> -Als numChars < 0, retourneren een invoertekenreeks. <br/> -Als null-tekenreeks is, retourneert een lege tekenreeks. |




| Rechts       |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Beschrijving | De juiste functie retourneert een opgegeven aantal tekens vanaf de rechterkant (end) van een tekenreeks.                                 |
| Functiehandtekening   | Tekenreeks Right (tekenreeks, numChars)   |
| Invoer      | 1. **De tekenreeks.** De tekenreeks waaruit tekens. <br/> 2. **numChars.** Een nummer van het aantal tekens retourneren vanaf het einde van een tekenreeks.  |
| Bewerkingen  | **numChars.** Tekens vanaf het einde van een tekenreeks geretourneerd. <br/> Voorbeeld: Rechts ('Britta Simon', 3) retourneert 'ma'.                  |
| Uitvoer      | Een tekenreeks met de laatste numChars tekens in een tekenreeks. Als numChars = 0, een lege tekenreeks retourneren. <br/> -Als **numChars** < 0, retourneren een invoertekenreeks. <br/> -Als de tekenreeks null is, retourneert een lege tekenreeks. <br/> -Als de tekenreeks minder tekens dan het aantal opgegeven in numChars bevat, wordt een tekenreeks die identiek is aan de tekenreeks geretourneerd. |




| Mid         |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Beschrijving | De functie deel retourneert een opgegeven aantal tekens van een opgegeven positie in een tekenreeks.                              |
| Functiehandtekening    | Tekenreeks Mid(string, pos, numChars)                                                                                             |
| Invoer      | 1. **tekenreeks.** De tekenreeks waaruit tekens.   <br/> 2. **pos.** Een nummer van de beginpositie in een tekenreeks voor het retourneren van tekens. <br/> 3. **numChars.** Een nummer van het aantal tekens retourneren vanaf een positie in de tekenreeks.  |
| Bewerkingen  | Retourneren **numChars** tekens vanaf positie **pos** in de tekenreeks. <br/>Voorbeeld: Mid ('Britta Simon', 3, 5) retourneert 'itta'. |
| Uitvoer      | Een tekenreeks met **numChars** tekens vanaf positie **pos** in een tekenreeks: <br/> -Als **numChars** = 0, een lege tekenreeks retourneren. <br/> -Als **numChars** < 0, een lege tekenreeks retourneren. <br/> -Als **pos** > de lengte van tekenreeks, een invoertekenreeks retourneren. <br/> -Als **pos** ≤ 0, retourneren een invoertekenreeks. <br/> -Als **tekenreeks** null is, een lege tekenreeks retourneren. <br/> Als er geen **numChar** tekens resterend in **tekenreeks** vanaf positie **pos**, zoals het aantal tekens als kan worden geretourneerd worden geretourneerd.

## <a name="data-generation-functions"></a>Functies voor het genereren van gegevens

Functies voor het genereren van gegevens worden gebruikt om waarden voor specifieke gegevenstypen te genereren.

| CRLF               |                                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| Beschrijving        | De CRLF genereert een regeleinde Return/Line Feed. Deze functie kunt u een nieuwe regel toevoegen. |
| Functiehandtekening | Tekenreeks CRLF                                                                              |
| Invoer             | Geen parameters                                                                            |
| Bewerkingen         | Een CRLF wordt geretourneerd.                                                                      |
|                    | Voorbeeld: AddressLine1 + CRLF() + AddressLine2 resulteert in: <br/> -AddressLine1 <br/> -AddressLine2 |
| Uitvoer             | Een CRLF is de uitvoer.                                                                                             |

| RandomNum          |                                                                                                                   |  
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Beschrijving        | De functie RandomNum retourneert een willekeurig getal binnen een opgegeven interval.                                       |   
| Functiehandtekening | Int RandomNum(start, end)                                                                                         |   
| Invoer             | - **Start**. Een nummer van de ondergrens van een willekeurige waarde om te genereren.   <br/> - **end**. Een nummer van de bovengrens van een willekeurige waarde om te genereren.  |
| Bewerkingen         | Een willekeurig getal groter dan of gelijk zijn aan **start** en kleiner dan of gelijk zijn aan **end** wordt gegenereerd. <br/>  Voorbeeld: Random(0,999) kan 100 retourneren.                      |
| Uitvoer             | Een willekeurig getal binnen het bereik van **start** en **end**.                                                      |  

| EscapeDNComponent  |                                                                                                                   |   
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Beschrijving        | De *EscapeDNComponent* methode verwerkt de invoerreeks op basis van het type van de management-agent die wordt gebruikt. |
| Functiehandtekening | Tekenreeks EscapeDNComponent(string)                                                                                  |
| Invoer     | **tekenreeks**. Een tekenreeks die wordt gebruikt voor het verwerken van een DN-naam. De tekenreeks mag geen escape-teken tekens bevatten. |
| Bewerkingen | De methode EscapeDNComponent MIISUtils wordt gebruikt voor deze bewerking niet uitvoeren. Deze methode verwerkt de invoerreeks op basis van het type van de management-agent die wordt gebruikt. <br/> Omdat verschillende beheeragents verschillende DN-Naamindelingen vereisen, verwerkt deze methode de invoer tekenreeksen op basis van het type van de beheeragent. De typen zijn Lightweight Directory Access Protocol LDAP) DN-naam, zoals asActive Directory® Domain Services, Sun Directory Server (voorheen iPlanet Directory-Server), Microsoft Exchange-Server. hiërarchische nonLDAP, zoals Microsoft Lotus Notes; en extrinsic, zoals de database en XML zonder LDAP DN-namen. <br/> ** LDAP DN-naam: ** <br/> -Ongeldige XML-tekens in het gedeelte van de waarde van een bepaald gedeelte zijn hexadecimaal gecodeerde. <br/>-Alle ongeldige tekens in het naamgedeelte van een bepaald gedeelte (inclusief ongeldige XML-tekens) is een fout gegenereerd. <br/> -De volgende tekens zijn escape-teken: <br/> &nbsp;&nbsp;&nbsp;-Met door komma's (',') <br/> &nbsp;&nbsp;&nbsp;-Is gelijk-teken ('=') <br/> &nbsp;&nbsp;&nbsp;-Plusteken ('+') <br/> &nbsp;&nbsp;&nbsp;-Minder-dan-teken ('< ') <br/> &nbsp;&nbsp;&nbsp;-Groter-dan-teken ('> ') <br/> &nbsp;&nbsp;&nbsp;-Hekje (#) <br/> &nbsp;&nbsp;&nbsp;-Puntkomma (';') <br/> &nbsp;&nbsp;&nbsp;-Backslash ('\') <br/> &nbsp;&nbsp;&nbsp;-Aanhalingsteken (' ' ") <br/> -Als het laatste teken in de tekenreeks een spatie is, is dat het ruimtegebruik ontsnapt. <br/> -Alle overbodige voorloop- of afsluitende spaties rond een onderdeelnaam worden verwijderd. <br/> -Voor de XML-beheeragent als er meerdere delen, klikt u vervolgens de onderdelen in alfabetische volgorde staan. <br/> -Als meerdere onderdelen zijn opgegeven, is de tekenreeks samengestelde DN-naam van de samenvoeging van de afzonderlijke tekenreeksen, gescheiden door het plusteken. <br/> -Er is een fout wordt gegenereerd als de invoertekenreeks is geen goed ingedeelde, LDAP-DN-naam. <br/><br/> **Hiërarchische niet LDAP** <br/> -Deze beheeragents ondersteunen geen meerdelige onderdelen. Als meerdere tekenreeksen worden doorgegeven aan EscapeDNComponent, wordt een ArgumentException gegenereerd. <br/> -Als een van de tekens in de invoerreeks bevat ongeldige XML-tekens, wordt een ArgumentException gegenereerd. <br/> -Alle komma's en backslashes in de invoerreeks zijn escape-teken. <br/> -Als het laatste teken in de tekenreeks een spatie is, is dat het ruimtegebruik ontsnapt. <br/><br/> **Extrinsic:** <br/> 1. Als een deel binair of een ongeldig XML-teken bevat, wordt dat onderdeel als een hexadecimaal gecodeerde versie van de onbewerkte gegevens opgeslagen met een voorvoegsel aan het begin van de tekenreeks '#'-teken. Bijvoorbeeld, als een onderdeel is AxC (waarbij x staat voor een ongeldig XML-teken, zoals '0x10'), is dat onderdeel gecodeerd als '#410010004300'. <br/> 2. Anders wordt alle exemplaren van de volgende tekens zijn escape-teken: <br/> &nbsp;&nbsp;&nbsp;-Backslash ('\') <br/> &nbsp;&nbsp;&nbsp;-Met door komma's (',') <br/> &nbsp;&nbsp;&nbsp;-Plusteken ('+') <br/> &nbsp;&nbsp;&nbsp;-Hekje (#) <br/> 3. Als het laatste teken in een bepaald gedeelte-tekenreeks een spatie is, is die ruimte ontsnapt. <br/> 4. Als meerdere onderdelen worden opgegeven, is de tekenreeks samengestelde DN-naam van de samenvoeging van de afzonderlijke tekenreeksen gescheiden door het plusteken.
| Uitvoer      | Een tekenreeks met een geldige domeinnaam in.                                                                                                                  |   

>[!NOTE]
De validatie van DN-namen is minder strikt dan de syntaxis die is gedefinieerd in de LDAP-specificaties. EscapeDNComponent(String[]) kunt u de onderdeelnaam van een bestaan uit een combinatie van een of meer van de tekens 'a'-'z', 'A'-'Z', '0'-'9', '-', en '.'. <br/>
Het is niet mogelijk om op te geven van een binair deel met deze methode. Het is echter mogelijk dat een binair deel **CommitNewConnector** als de DN-naam is samengesteld uit anker kenmerken en een van de kenmerken van het anker binair type.


| Null        |                                                                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Beschrijving | De Null-functie wordt gebruikt voor het definiëren van deze MA heeft geen een kenmerk bij te dragen en dat met de volgende MA Kenmerkprioriteit moet worden voortgezet. |   
| Functiehandtekening    | Tekenreeks Null    |
| Invoer      | Geen parameters                                                                                                                                             |   
| Bewerkingen  | Er is een Null geretourneerd. <br/> Voorbeeld: IIF(Eq(domain), 'Onbekend', Null())                                                                                           |   
| Uitvoer      | Een null-waarde wordt uitgevoerd.                                                                                                                                         |   |   |


## <a name="logic-functions"></a>Bedrijfslogica-functies
Bedrijfslogica-functies worden gebruikt voor het uitvoeren van een bewerking op basis van voorwaarden die worden geëvalueerd door het systeem.

| IIF        |  |
|-------------|---|
| Beschrijving | De functie IIF retourneert een van de mogelijke waarden op basis van een opgegeven voorwaarde.    |
| Functiehandtekening   | Object IIF (voorwaarde, waardeindienwaar, WaardeAlsOnwaar)   |                                                 |
| Invoer      | 1. **Voorwaarde**. Een waarde of expressie die kan worden geëvalueerd op waar of ONWAAR. 2. **waardeindienwaar** een waarde die wordt geretourneerd als de voorwaarde wordt geëvalueerd op true. <br/> 3. **WaardeAlsOnwaar** een waarde die wordt geretourneerd als de voorwaarde wordt geëvalueerd op false. <br/><br/> De volgende functies beschikbaar zijn voor gebruik als expressies in de functie IIF zijn **voorwaarde:** <br/> **EQ.** Deze functie vergelijkt twee argumenten op gelijkheid. <br/> **NotEquals.** Deze functie vergelijkt twee argumenten voor ongelijk, retourneert true als ze niet gelijk is en false als ze gelijk zijn.<br/> Voorbeeld: NotEquals (EmployeeType, 'Contractor')<br/> **LessThan.** Deze functie vergelijkt twee getallen, anders waar als de eerste kleiner is dan de tweede en false geretourneerd.<br/>Voorbeeld: LessThan (salaris, 100000) <br/>**Groter dan.** Deze functie vergelijkt twee getallen, retourneert true als de eerste anders groter dan de tweede en false is.<br/> Voorbeeld: Groter dan (salaris, 100000) <br/> **LessThanOrEquals.** Deze functie vergelijkt twee getallen, retourneert true als de eerste anders kleiner dan of gelijk aan de tweede en false is.<br/>Voorbeeld: LessThanOrEquals (salaris, 100000) <br/> **GreaterThanOrEquals.* Deze functie vergelijkt twee getallen, retourneert true als de eerste groter dan is of gelijk aan de tweede en false anders is. <br/>Voorbeeld: GreaterThanOrEquals (salaris, 100000)<br/> IsPresent. Deze functie wordt als een kenmerk in het schema ILM invoer en retourneert true als het kenmerk niet null is en false als het kenmerk null is.|
| Bewerkingen  | Als **voorwaarde** evalueert op true, retourneren **waardeindienwaar.** Anders retourneren **WaardeAlsOnwaar.** <br/>Voorbeeld: IIF (Eq (EmployeeType, 'Kandidaatbestanden'), 't-' + Alias, Alias) retourneert de alias van een gebruiker met "t-' toegevoegd aan het begin van deze als de gebruiker een intern is. Anders retourneert het alias van de gebruiker is. |
| Uitvoer      | De uitvoer is **waardeindienwaar** als de voorwaarde waar is of **WaardeAlsOnwaar** als de voorwaarde onwaar is. |      
