---
title: Naslag informatie voor BHOLD-ontwikkel aars voor Microsoft Identity Manager 2016 | Microsoft Docs
description: BHOLD-referentie voor ontwikkelaars
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: edf9f88a4d96d212cef4aa41cbad7d0b4446534f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758893"
---
# <a name="bhold-developer-reference-for-microsoft-identity-manager-2016"></a>Naslag informatie voor BHOLD-ontwikkel aars voor Microsoft Identity Manager 2016

De BHOLD-kern module kan script opdrachten verwerken. U kunt dit rechtstreeks doen met behulp van de bscript.dll in een .NET-project. Werkt ook met de interface b1scriptservice. asmx van de webservice. 

Voordat een script wordt uitgevoerd, moeten alle gegevens in het script worden verzameld om dit script op te stellen. Deze informatie kan worden verzameld uit de volgende bronnen:

   * Gebruikersinvoer
   * BHOLD-gegevens
   * Toepassingen
   * Overige

De BHOLD-gegevens kunnen worden opgehaald met behulp van de functie GetInfo van het script-object. Er is een volledige lijst met opdrachten waarmee u alle gegevens kunt presen teren die zijn opgeslagen in de BHOLD-data base. De weer gegeven gegevens zijn echter onderworpen aan de weergave machtigingen van de gebruiker die is aangemeld. Het resultaat bevindt zich in de vorm van een XML-document dat kan worden geparseerd.

Een andere bron voor informatie kan een van de toepassingen zijn die worden beheerd door BHOLD. De toepassings module heeft een speciale functie, de FunctionDispatch, die kan worden gebruikt om toepassingsspecifieke informatie te presen teren. Dit wordt ook weer gegeven als een XML-document.

Ten slotte, als er geen andere manier is, kan het script opdrachten rechtstreeks naar andere toepassingen of systemen bevatten. NoThenstallation van extra software op de BHOLD-server kan de beveiliging van het hele systeem ondermijnen.

Al deze gegevens worden in één XML-document geplaatst en toegewezen aan het BHOLD-script object. Het object combineert dit document met een vooraf gedefinieerde functie. De vooraf gedefinieerde functie is een XSL-document waarmee het script-invoer document wordt vertaald naar een BHOLD-opdracht document.

![Verwerking van BHOLD-script](media/mim2016-bhold-developer-reference/image001.png)

De opdrachten worden dezelfde volg orde uitgevoerd als in het document. Als een functie mislukt, worden alle uitgevoerde opdrachten teruggedraaid.

## <a name="script-object"></a>Script object
In deze sectie wordt beschreven hoe u het script object kunt gebruiken.

### <a name="retrieve-bhold-information"></a>BHOLD-gegevens ophalen
De functie **GetInfo** wordt gebruikt om informatie op te halen uit de beschik bare gegevens in het BHOLD-autorisatie systeem. De functie vereist een functie naam en uiteindelijk een of meer para meters. Als deze functie is geslaagd, wordt een BHOLD-object of-verzameling geretourneerd in de vorm van een XML-document.

Als de functie niet kan worden uitgevoerd, retourneert de functie GetInfo een lege teken reeks of een fout. De fout beschrijving en het nummer kunnen worden gebruikt om meer informatie over de fout te krijgen.

De functie GetInfo ' FunctionDispatch ' kan worden gebruikt om informatie op te halen uit een toepassing die wordt beheerd door het BHOLD-systeem. Deze functie vereist drie para meters: de ID van de toepassing, de functie Dispatch zoals deze is gedefinieerd in de ASI, en een XML-document met ondersteunende informatie voor de ASI. Als de functie slaagt, is het resultaat beschikbaar in XML-indeling in het resultaat object.

Het onderstaande fragment is een eenvoudig C#-voor beeld van GetInfo:

```C#
ScriptProcessor myScriptProcessor = new ScriptProcessor();
myScriptProcessor.Initializae("CORP\\b1user");
myScriptProcessor.GetInfo("OrgUnit", "1");
```

Op dezelfde manier is ook toegang tot het **bscript** -object via de webservice `b1scriptservice` . Dit doet u door een webverwijzing toe te voegen aan uw project met behulp van http:// <server> : 5151/BHOLD/core/b1scriptservice. asmx waarbij de- <server> server is waarop de BHOLD binaire bestanden zijn geïnstalleerd. Zie [een webservice-verwijzing toevoegen aan een Visual Studio-project](https://msdn.microsoft.com/library/d9w023sx(v=vs.71).aspx)voor meer informatie.

In het volgende voor beeld ziet u hoe u de functie **GetInfo** van een webservice gebruikt. Met deze code wordt de organisatie-eenheid met een OrgID van 1 opgehaald en wordt de naam van de organisatie-eenheid op het scherm weer gegeven.

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Xml;

namespace bhold_console
{
    class Program
    {
        static void Main(string[] args)
        {
             var bholdService = new BHOLDCORE.B1ScriptService();
             bholdService.Url = "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx";
             string orgname= "";

             if (args.Length == 3)
             {
                 //Use explicit credentials from command line
                 bholdService.UseDefaultCredentials = false;
                 bholdService.Credentials = new System.Net.NetworkCredential(args[0], args[1], args[2]);
                 bholdService.PreAuthenticate = true;
             }
             else
             {
                 bholdService.UseDefaultCredentials = true;
                 bholdService.PreAuthenticate = true;
             }

             //Load BHOLD information into an xml document and loop through document to find the bholdDescription value
             var myOrgUnit = new System.Xml.XmlDocument();
             myOrgUnit.LoadXml(bholdService.GetInfo("OrgUnit","1","","");

            XmlNodeList myList = myOrgUnit.SelectNodes(("//item");

            foreach (XmlNode myNode in myList)
            {
                for (int i = 0; i < myNode.ChildNodes.Count; i++)
                {
                    if (myNode.ChildNodes[i].InnerText.ToString() == "bholdDescription")
                    {
                        orgname = myNode.ChildNodes[i + 1].InnerText.ToString();
                    }
                }
            }

            System.Console.WriteLine("The Organizational Unit Name is: " + orgname);

        }
    }
}
```

In het volgende VBScript-voor beeld wordt de webservice gebruikt via SOAP en GetInfo. Zie de sectie beheerde Naslag informatie over BHOLD voor meer voor beelden van SOAP 1,1, SOAP 1,2 en HTTP POST, of u kunt rechtstreeks vanuit een browser naar de webservice navigeren en deze hier weer geven.

```VB
Dim SOAPRequest
Dim SOAPParameters
Dim SOAPResponse
Dim xmlhttp

Set xmlhttp = CreateObject("Microsoft.XMLHTTP")

xmlhttp.open "POST", "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx", False, "CORP\Administrator", "abc123*2k"

xmlhttp.setRequestHeader "Content-type", "text/xml; charset=utf-8"
xmlhttp.setRequestHeader "SOAPAction", "http://B1/B1ScriptService/GetInfo"

SOAPRequest = "<?xml version='1.0' encoding='utf-8'?> <soap:Envelope" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:xsi=""http://" & vbCRLF
SOAPRequest = SOAPRequest & " www.w3.org/2001/XMLSchema-instance""" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:xsd=""http://www.w3.org/2001/XMLSchema""" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:soap=""http://schemas.xmlsoap.org/soap/envelope/"">" & vbCRLF
SOAPRequest = SOAPRequest & " <soap:Body>" & vbCRLF
SOAPRequest = SOAPRequest & " <GetInfo xmlns=""http://B1/B1ScriptService"">" & vbCRLF
SOAPRequest = SOAPRequest & " <functionName>OrgUnit</functionName>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter1>1</parameter1>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter2></parameter2>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter3></parameter3>" & vbCRLF
SOAPRequest = SOAPRequest & " </GetInfo>" & vbCRLF
SOAPRequest = SOAPRequest & " </soap:Body>" & vbCRLF
SOAPRequest = SOAPRequest & " </soap:Envelope>"
MsgBox SOAPRequest

xmlhttp.send SOAPRequest 

SOAPResponse = xmlhttp.responseText

MsgBox SOAPResponse
```

## <a name="execute-scripts"></a>Scripts uitvoeren

De functie **ExecuteScript** van het object **bscript** kan worden gebruikt voor het uitvoeren van scripts. Voor deze functie zijn twee para meters vereist. De eerste para meter is het XML-document dat de aangepaste informatie bevat die door het script moet worden gebruikt. De tweede para meter is de naam van het vooraf gedefinieerde script dat moet worden gebruikt. InIn de BHOLD vooraf gedefinieerde scripts directory moet hier een XSL-document zijn met dezelfde naam als de functie, maar met de extensie. xsl.

Als de functie niet kan worden uitgevoerd, retourneert de functie ExecuteScript de waarde false. De fout beschrijving en het nummer kunnen worden gebruikt om te weten wat er mis is gegaan. Hier volgt een voor beeld van het gebruik van de ExecuteXML-webmethode. Met deze methode wordt ExecuteScript aangeroepen.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Sample
{
    class Program
    {
        static void Main(string[] args)
        {
            var bholdService = new BHOLDCORE.B1ScriptService();
            bholdService.Url = "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx";

            if (args.Length == 3)
            {
                //Use explicit credentials from command line
                bholdService.UseDefaultCredentials = false;
                bholdService.Credentials = new System.Net.NetworkCredential(args[0], args[1], args[2]);
                bholdService.PreAuthenticate = true;
            }
            else
            {
                bholdService.UseDefaultCredentials = true;
                bholdService.PreAuthenticate = true;
            }
            System.Console.WriteLine( "Add user #3 to role #44, result: {0}", bholdService.ExecuteXml(roleAddUser("44", "3")) );
            System.Console.WriteLine("Add user D1 to role 'MR-OU2 No Users', result: {0}", bholdService.ExecuteXml(roleAddUser("MR-OU2 No Users", "D1")));

        }

        private static System.Xml.XmlNode roleAddUser(string roleId, string userId)
        {
            var script = new System.Xml.XmlDocument();
            script.LoadXml(string.Format("<functions>"
                                        +"  <function name='roleadduser' roleid='{0}' userid='{1}' />"
                                        +"</functions>",
                                        roleId,
                                        userId)
                           );
            return script.DocumentElement;
```

## <a name="bholdscriptresult"></a>BholdScriptResult

Deze GetInfo-functie is beschikbaar nadat de functie **executescript** is uitgevoerd. De functie retourneert een XML-indelings teken reeks die het volledige uitvoerings rapport bevat. Het script knooppunt bevat de XML-structuur van het script dat wordt uitgevoerd.

Voor elke functie die mislukt tijdens het uitvoeren van het script, wordt een knooppunt functie toegevoegd met de naam van de knoop punten. ExecuteXML en error worden toegevoegd aan het einde van het document alle gegenereerde Id's worden toegevoegd.

U ziet dat alleen de functies, die een fout bevatten, worden toegevoegd. Het fout nummer ' 0 ' betekent dat de functie niet wordt uitgevoerd. 

```
<Bhold>
  <Script>
    <functions>
      <function name="OrgUnitadd" description="OrgUnit1" parentid="root" orgtypeid="root" return="@ID@"/>     
      <function name="roleaddOrgUnit" OrgUnitid="OrgUnit1" roleid="Role1_OrgUnit1" return="@ID@"/>      
    </functions>
  </Script>
  <Function>
    <Name>OrgUnitadd</Name>
    <ExecutedXML>
      <function name="OrgUnitadd" description="OrgUnit1" parentid="root" orgtypeid="root" return="@ID@"/>
    </ExecutedXML>
    <Error Number="5" Description="Violation of UNIQUE KEY constraint 'IX_OrgUnits'. Cannot insert duplicate key in object 'dbo.OrgUnits'.
The statement has been terminated."/>
  </Function>
  <Function>
    <Name>roleaddOrgUnit</Name>
    <ExecutedXML>
      <function name="roleaddOrgUnit" OrgUnitid="OrgUnit1" roleid="Role1_OrgUnit1" return="@ID@"/>
    </ExecutedXML>
    <Error Number="0" Description=""/>
  </Function>
  <IDS>
    <ID name="@ID@">35</ID>
  </IDS>
</Bhold>
```

## <a name="id-parameters"></a>ID-para meters
Met ID-para meters wordt een speciale behandeling verkregen. Niet-numerieke waarden worden gebruikt als Zoek waarde voor het zoeken naar de overeenkomende entiteiten in de BHOLD-gegevens opslag. Als de zoek waarde niet uniek is, wordt de eerste entiteit geretourneerd die voldoet aan de zoek waarde.

Als u numerieke Zoek waarden wilt onderscheiden van Id's, is het mogelijk om een voor voegsel te gebruiken. Wanneer de eerste zes tekens van de zoek waarde gelijk zijn aan ' no_id: ', worden deze tekens verwijderd voordat de waarde wordt gebruikt voor het zoeken. SQL-joker tekens% kunnen worden gebruikt.

De volgende velden worden gebruikt in combi natie met de zoek waarde:

| ID-type   | Zoekveld |
|---|---|
| OrgUnitID | Beschrijving  |
| roleID    | Beschrijving  |
| taskID    | Beschrijving  |
| userID    | DefaultAlias |

## <a name="script-access-and-permissions"></a>Toegang tot scripts en machtigingen
Code aan de server zijde in de Active Server pagina's worden gebruikt voor het uitvoeren van de scripts. Toegang tot het script betekent daarom toegang tot deze pagina's. Het BHOLD-systeem houdt informatie bij over de toegangs punten van de aangepaste pagina's. Deze informatie omvat start pagina en functie beschrijving (er moeten meerdere talen worden ondersteund).

Een gebruiker is gemachtigd om de aangepaste pagina's in te voeren en een script uit te voeren. Elk toegangs punt wordt weer gegeven als een taak. Elke gebruiker die deze taak heeft verkregen via een rol of een eenheid, kan de bijbehorende functie uitvoeren.

Een nieuwe functie in het menu bevat alle aangepaste functies die door de gebruiker kunnen worden uitgevoerd. Omdat een script acties in het BHOLD systeem kan uitvoeren onder een andere identiteit dan de gebruiker die is aangemeld. Het is mogelijk om toestemming te geven om één specifieke actie uit te voeren zonder dat er toezicht op een object hoeft te worden uitgevoerd. Dit kan bijvoorbeeld nuttig zijn voor een werk nemer die alleen nieuwe klanten voor het bedrijf mag invoeren. Deze scripts kunnen ook worden gebruikt om zelf registratie pagina's te maken.

## <a name="command-script"></a>Opdracht script
Het opdracht script bevat een lijst met functies die worden uitgevoerd door het BHOLD-systeem. De lijst wordt geschreven in een XML-document dat voldoet aan de volgende definities:


|   Opdracht script   |              `<functions>functions</functions>`               |
|--------------------|---------------------------------------------------------------|
|     vervullen      |                      functie {Function}                      |
|      functieassembly      | <functie name = "functie naam" functionParameters [retour] (/> \| > parameterList </function>) |
|    functionName    | Een geldige functie naam zoals beschreven in de volgende secties. |
| functionParameters |                     { functionParameter }                     |
| functionParameter  |               para meternaam = "parameterValue"                |
|   parameterName    |                    Een geldige parameter naam.                    |
|   parameterValue   |                      @variable@- \| waarde                      |
|       waarde        |                   Een geldige parameter waarde.                    |
|   parameterList    |          <parameters> Corpus  </parameters>          |
|   Corpus    | <parameter name="parameterName"> parameterValue </parameter>  |
|       terugkeren       |                      retour = " @variable @"                      |
|      variabeletype      |                    De naam van een aangepaste variabele.                    |

XML bevat de volgende vertalingen van speciale tekens:

| XML | Teken |
|---|---|
| ```&amp;```  | & |
| ```&lt;```   | < |
| ```&gt;```   | > |
| ```&quot;``` | " |
| ```&apos;``` | ' |

Deze XML-tekens kunnen worden gebruikt in id's, maar ze worden niet aanbevolen.

De volgende code toont een voor beeld van een geldig opdracht document met drie functies:

```
<functions>

   <functionname="OrgUnitAdd" parentID="34" description="Acme Inc." orgtypeID="5" return="@UnitID@" />

   <function name="UserAdd" description="John Doe" alias="jdoe" languageID="1" OrgUnitID="@UnitID@" />

   <function name="TaskAddFile" taskID="93" path="/customers/purchase">
      <parameters>
         <parameter name="history"> True</parameter>
      </parameters>
   </function>

</functions>
```

De functie **OrgUnitAdd** slaat de id van de gemaakte eenheid op in een variabele met de naam UnitID. Deze variabele wordt gebruikt als invoer voor de functie UserAdd. De geretourneerde waarde van deze functie wordt niet gebruikt. In de volgende secties worden alle beschik bare functies, de vereiste para meters en retour waarden beschreven.

## <a name="execute-functions"></a>Functies uitvoeren
In deze sectie wordt beschreven hoe u de Execute-functies gebruikt.

### <a name="abaattributeruleadd"></a>ABAAttributeRuleAdd
Maak een nieuwe kenmerk regel voor een specifiek kenmerk type. Kenmerk regels kunnen alleen worden gekoppeld aan één kenmerk type.

De opgegeven kenmerk regel kan worden gekoppeld aan alle mogelijke kenmerk typen. 

De RuleType kan niet worden gewijzigd met de opdracht ' ABAattributeruletypeupdate '. Vereist dat de beschrijving van het kenmerk uniek is.


|    Argumenten    |                                                                                                                                                                                                                  Type                                                                                                                                                                                                                  |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   Beschrijving   |                                                                                                                                                                                                                  Tekst                                                                                                                                                                                                                  |
|    RuleType     | Geef het soort kenmerk regel op. Afhankelijk van het soort kenmerk regel type andere argumenten moeten worden opgenomen. De volgende regel type waarden zijn geldig:<ul><li>0: reguliere expressie (argument waarde toevoegen).</li><li>1: waarde (Voeg argumenten toe, operator en waarde).</li><li>2: lijst met waarden.</li><li>3: Range (argumenten toevoegen "rangemin wordt" en "RangeMax").</li><li>4: leeftijd (argumenten toevoegen, operator en waarde).</li></ul> |
|  InvertResult   | ```["0"|"1"|"N"|"Y"]``` |
| AttributeTypeID |                                                                                                                                                                                                                  Tekst                                                                                                                                                                                                                  |

| Optionele argumenten | Type |
|---|---|
| Operator | Tekst <br>**Opmerking** : dit argument is verplicht als **RuleType** 1 of 4 is. De mogelijke waarden zijn ' = ', ' < ' of ' > '. XML-labels moeten worden gebruikt &gt; voor ' > ' en ' &lt; ' voor ' < '. |
| Rangemin wordt | Getal <br>**Opmerking** : dit argument is verplicht als **RuleType** 3 is. |
| RangeMax | Getal <br>**Opmerking** : dit argument is verplicht als **RuleType** 3 is. |
| Waarde    | Tekst <br>**Opmerking** : dit argument is verplicht als **RuleType** 0, 1 of 4 is. Het argument moet een numerieke of alfanumerieke waarde zijn. |
| Retour type AttributeRuleID | Tekst   |


### <a name="applicationadd"></a>applicationadd
Hiermee wordt een nieuwe toepassing gemaakt en wordt de ID van de nieuwe toepassing geretourneerd.

| Argumenten | Type |
|---|---|
| beschrijving |      |
| machine     |      |
| programma      |      |
| parameter   |      |
| protocol    |      |
| gebruikersnaam    |      |
| wachtwoord    |      |
| svroleID (optioneel)                | Als dit argument niet aanwezig is, wordt er een supervisor functie van de huidige gebruiker gebruikt. |
| Applicationaliasformula (optioneel) | De alias formule wordt gebruikt voor het maken van een alias voor een gebruiker wanneer deze is toegewezen aan een machtiging van de toepassing. De alias wordt gemaakt als de gebruiker nog geen alias heeft voor deze toepassing. Als er geen waarde wordt opgegeven, wordt de standaard alias van de gebruiker gebruikt als alias voor de toepassing. De formule wordt opgemaakt als ` [<<objecttype>>.<<nameofobjecttypeattribute>>(startindexoffset,length offset)]` . De offset is optioneel. Alleen kenmerken van gebruikers en toepassingen kunnen worden gebruikt. Vrije tekst kan worden gebruikt. De gereserveerde tekens zijn vier kant haakje openen ([) en rechter rechte haak (]). Bijvoorbeeld: `[Application.bholdDescription]\[User.bholdDefAlias(1,5)]`. |
| Retour type | De ID van de nieuwe toepassing. |

### <a name="attributesetvalue"></a>AttributeSetValue
Hiermee stelt u de waarde van een kenmerk type dat is verbonden met het object type. Vereist dat de beschrijvingen van het object type en het kenmerk type uniek zijn.

| Argumenten | Type |
|---|---|
| ObjectTypeID     | Tekst |
| ObjectID         | Tekst |
| AttributeTypeID  | Tekst |
| Waarde            | Tekst |
| Retourtype      | Type |

### <a name="attributetypeadd"></a>AttributeTypeAdd
Hiermee wordt een nieuw type kenmerk type/eigenschap ingevoegd.


|        Argumenten        |                                               Type                                                |
|-------------------------|---------------------------------------------------------------------------------------------------|
|       DataTypeID        |                                               Tekst                                                |
| Beschrijving (= identiteit) | Tekst <br>**Opmerking** : gereserveerde woorden kunnen niet worden gebruikt, waaronder ' a ', ' frm ', ' id ', ' usr ' en ' bhold '. |
|        Lengte        |                                       Nummer in [1,.., 255]                                        |
| ListOfValues (Booleaans)  |                                          ```["0"|"1"|"N"|"Y"]```                               |
|      Standaard       |                                               Tekst                                                |
|       Retourtype       |                                               Type                                                |
|     AttributeTypeID     |                                               Tekst                                                |

### <a name="attributetypesetadd"></a>AttributeTypeSetAdd
Hiermee wordt een nieuw kenmerk type ingesteld. Vereist dat de beschrijving van het kenmerk type is ingesteld op uniek.

| Argumenten | Type |
|---|---|
| Beschrijving (= identiteit) |Tekst |
| Retourtype | Type |
| AttributeTypeSetID | Tekst |

### <a name="attributetypesetaddattributetype"></a>AttributeTypeSetAddAttributeType
Hiermee wordt een nieuw kenmerk type ingevoegd in een bestaand kenmerk type dat is ingesteld. Vereist dat de beschrijvingen van het kenmerk type set en het kenmerk type uniek zijn.


|     Argumenten      |                              Type                              |
|--------------------|----------------------------------------------------------------|
| AttributeTypeSetID |                              Tekst                              |
|  AttributeTypeID   |                              Tekst                              |
|       Bestellen        |                             Getal                             |
|     Locatie     | Tekst <br>**Opmerking** : de locatie is groep of één voor waarde. |
|     Verplicht      |                    ```["0"|"1"|"N"|"Y"]```                  |
|    Retourtype     |                              Type                              |

### <a name="objecttypeaddattributetypeset"></a>ObjectTypeAddAttributeTypeSet
Hiermee wordt een kenmerk type ingesteld op een object type. Vereist dat de beschrijving van het object type en het kenmerk type uniek zijn. De object typen zijn: systeem, OrgUnit, gebruiker, taak.

| Argumenten | Type |
|---|---|
| ObjectTypeID | Tekst | 
| AttributeTypeSetID | Tekst | 
| Bestellen | Getal | 
| Zichtbaar | <ul><li>0: het kenmerk type is ingesteld op zichtbaar.</li><li>2: het kenmerk type is ingesteld op zichtbaar wanneer de knop **meer informatie** is geselecteerd.</li><li>1: het kenmerk type is ingesteld op onzichtbaar.</li></ul> | 
| Retourtype | Type | 

### <a name="orgunitadd"></a>OrgUnitadd
Hiermee wordt een nieuwe organisatie-eenheid gemaakt en wordt de ID van de nieuwe organisatie-eenheid geretourneerd.

| Argumenten | Type |
|---|---|
| beschrijving | | 
| orgtypeID | | 
| parentID | | 
| OrgUnitinheritedroles (optioneel) | | 
| Retourtype | Type | 
| ID van de nieuwe eenheid | De para meter OrgUnitinheritedroles <br>heeft de waarde Ja of Nee. | 

### <a name="orgunitaddsupervisor"></a>OrgUnitaddsupervisor
Een gebruiker een super Visor van een organisatie-eenheid maken.

| Argumenten | Type |
|---|---|
| svroleID | De argument-userID kan ook worden gebruikt. In dit geval is de standaard Supervisor functie geselecteerd. Een standaard supervisor Role heeft een naam zoals **__svrole** gevolgd door een getal. De argument-userID kan worden gebruikt voor achterwaartse compatibiliteit. | 
| OrgUnitID | | 

### <a name="orgunitadduser"></a>OrgUnitadduser
Een gebruiker lid maken van een organisatie-eenheid.

| Argumenten | Type |
|---|---|
| userID | | 
| OrgUnitID | | 

### <a name="orgunitdelete"></a>OrgUnitdelete
Hiermee verwijdert u een organisatie-eenheid.

| Argumenten | Type |
|---|---|
| OrgUnitID | | 

### <a name="orgunitdeleteuser"></a>OrgUnitdeleteuser
Hiermee wordt een gebruiker verwijderd als lid van een organisatie-eenheid.

| Argumenten | Type |
|---|---|
| userID | | 
| OrgUnitID | | 

### <a name="roleadd"></a>roleadd
Hiermee maakt u een nieuwe rol.

| Argumenten | Type |
|---|---|
| Beschrijving | | 
| svrole | | 
| svroleID (optioneel) | Als dit argument niet aanwezig is, wordt er een supervisor rol van de huidige gebruiker gebruikt. | 
| ContextAdaptable (optioneel) | ```["0","1","N","Y"]``` | 
| MaxPermissions (optioneel) | Geheel getal | 
| MaxRoles (optioneel) | Geheel getal | 
| MaxUsers (optioneel) | Geheel getal | 
| Retourtype | Type | 
| ID van de nieuwe rol | | 

### <a name="roleaddorgunit"></a>roleaddOrgUnit
Hiermee wijst u een rol toe aan een organisatie-eenheid.


|    Argumenten    |                                      Type                                      |
|-----------------|--------------------------------------------------------------------------------|
|    OrgUnitID    |                                     roleID                                     |
| inheritThisRole | ' waar ' of ' onwaar ' geeft aan of de rol wordt voorgesteld voor onderliggende eenheden. |

### <a name="roleaddrole"></a>roleaddrole
Hiermee wijst u een rol toe als een subrol van een andere rol.

| Argumenten | Type |
|---|---|
| roleID | | 
| subRoleID | | 

### <a name="roleaddsupervisor"></a>roleaddsupervisor
Maak een gebruiker een super Visor van een rol.

| Argumenten | Type |
|---|---|
| svroleID | De argument-userID kan ook worden gebruikt. In dit geval is de standaard Supervisor functie geselecteerd. Een standaard supervisor Role heeft een naam zoals **__svrole** gevolgd door een getal. De argument-userID kan worden gebruikt voor achterwaartse compatibiliteit. | 
| roleID | | 

### <a name="roleadduser"></a>roleadduser
Hiermee wijst u een rol toe aan een gebruiker. De rol kan geen context aanpas bare rol hebben als er geen Contextid mag is opgegeven.

| Argumenten | Type |
|---|---|
| userID | | 
| roleID | | 
| durationType (optioneel) | Kan de waarden ' Free ', ' hours ' en ' Days ' bevatten. | 
| durationLength (optioneel) | Vereist wanneer durationType ' hours ' of ' Days ' is. moet de integerwaarde bevatten voor het aantal uren of dagen dat de rol is toegewezen aan een gebruiker. | 
| starten (optioneel) | De datum en tijd waarop de rol is toegewezen. Als dit kenmerk wordt wegge laten, wordt de rol onmiddellijk toegewezen. De datum notatie is ' JJJJ-MM-DDTuu: nn: SS ', waarbij alleen jaar, maand en dag zijn vereist. bijvoorbeeld "2004-12-11" en "2004-11-28T08:00" zijn geldige waarden. | 
| End (optioneel) | De datum en tijd waarop de functie is ingetrokken. Wanneer durationType en durationLength worden opgegeven, wordt deze waarde genegeerd. De datum notatie is ' JJJJ-MM-DDTuu: nn: SS ', waarbij alleen jaar, maand en dag zijn vereist. bijvoorbeeld "2004-12-11" en "2004-11-28T08:00" zijn geldige waarden. | 
| linkreason | Vereist wanneer start, end of duration wordt opgegeven, anders genegeerd. | 
| Contextid mag (optioneel) | ID van de organisatie-eenheid, alleen vereist voor context aanpas bare rollen. | 

### <a name="roledelete"></a>roledelete
Hiermee verwijdert u een rol.

| Argumenten | Type |
|---|---|
| roleID | | 

### <a name="roledeleteuser"></a>roledeleteuser
Hiermee verwijdert u de roltoewijzing aan een gebruiker. Overgenomen rollen door de gebruiker worden ingetrokken door deze opdracht.

| Argumenten | Type |
|---|---|
| userID | | 
| roleID | | 
| Contextid mag (optioneel) |  | 

### <a name="roleproposeorgunit"></a>roleproposeOrgUnit
Hiermee wordt een rol voorgesteld om deze toe te wijzen aan de leden en de OrgUnits van een OrgUnit.

| Argumenten | Type |
|---|---|
| OrgUnitID | | 
| roleID | | 
| durationType (optioneel) | Kan waarden ' Free ', ' hours ' en ' Days ' bevatten. | 
| durationLength | Vereist wanneer durationType ' hours ' of ' Days ' is, moet de gehele waarde bevatten voor het aantal uren of dagen dat de rol is toegewezen aan een gebruiker. | 
| durationFixed | ' waar ' of ' onwaar ' geeft aan of de toewijzing van deze rol aan een gebruiker gelijk moet zijn aan durationLength. | 
| inheritThisRole | ' waar ' of ' onwaar ' geeft aan of de rol wordt voorgesteld voor onderliggende eenheden. | 

### <a name="taskadd"></a>taskadd
Hiermee wordt een nieuwe taak gemaakt en wordt de ID van de nieuwe taak geretourneerd.

| Argumenten | Type |
|---|---|
| applicationID | | 
| beschrijving | Tekst met een maximum van 254 tekens. | 
| TaskName | Tekst met een maximum van 254 tekens. | 
| tokenGroupID | | 
| svroleID (optioneel) | Als dit argument niet aanwezig is, wordt er een supervisor rol van de huidige gebruiker gebruikt. | 
| contextAdaptable (optioneel) | ```["0","1","N","Y"]``` | 
| onderbouw (optioneel) | ```["0","1","N","Y"]``` | 
| auditaction (optioneel) | <ul><li>0: onbekend (standaard)</li><li>1: ReportOnly</li><li>2: AlertAppAll</li><li>3: AlertAppObsolete</li><li>4: AlertAppMissing</li><li>5: EnforceAppAll</li><li>6: EnforceAppObsolete</li><li>7: EnforceAppMissing</li><li>8: AlertEnforceAppAll</li><li>9: AlertEnforceAppObsolete</li><li>10: AlertEnforceAppMissing</li><li>11: importal</li></ul> | 
| auditalertmail (optioneel) | Het e-mail adres om waarschuwingen over deze machtiging te ontvangen, wordt door de auditor verzonden. Als dit argument niet aanwezig is, wordt het e-mail adres van de waarschuwing van de auditor gebruikt. | 
| MaxRoles (optioneel) | Geheel getal | 
| MaxUsers (optioneel) | Geheel getal | 
| Retourtype | De ID van de nieuwe taak. | 

### <a name="taskadditask"></a>taskadditask
Aangeven dat twee taken incompatibel zijn.

| Argumenten | Type |
|---|---|
| taskID | | 
| taskID2 | | 

### <a name="taskaddrole"></a>taskaddrole
Hiermee wijst u een taak toe aan een rol.

| Argumenten | Type |
|---|---|
| roleID | | 
| taskID | | 

### <a name="taskaddsupervisor"></a>taskaddsupervisor
Een gebruiker een super Visor van een taak maken.

| Argumenten | Type |
|---|---|
| svroleID | De argument-userID kan ook worden gebruikt. In dit geval is de standaard Supervisor functie geselecteerd. Een standaard supervisor Role heeft een naam zoals **__svrole** gevolgd door een getal. De argument-userID kan worden gebruikt voor achterwaartse compatibiliteit. | 
| taskID | | 

### <a name="useradd"></a>useradd
Hiermee wordt een nieuwe gebruiker gemaakt. Hiermee wordt de ID van de nieuwe gebruiker geretourneerd.

| Argumenten | Type |
|---|---|
| beschrijving | | 
| alias | | 
| languageID | <ul><li>1: Engels</li><li>2: Nederlands</li></ul> | 
| OrgUnitID | | 
eind datum (optioneel) |De datum notatie is ' JJJJ-MM-DDTuu: nn: SS ', waarbij alleen jaar, maand en dag zijn vereist. bijvoorbeeld "2004-12-11" en "2004-11-28T08:00" zijn geldige waarden. | 
| uitgeschakeld (optioneel) | <ul><li>0: ingeschakeld</li><li>1: uitgeschakeld</li></ul> | 
| MaxPermissions (optioneel) | Geheel getal | 
| MaxRoles (optioneel) | Geheel getal | 
| Retourtype | ID van de nieuwe gebruiker. | 

### <a name="useraddrole"></a>UserAddRole
Hiermee voegt u een gebruikersrol toe.
<!--- missing content -->

| Argumenten | Type |
|---|---|
|  | | 

### <a name="userdeleterole"></a>UserDeleteRole 
Hiermee verwijdert u een gebruikersrol.
<!--- missing content -->

| Argumenten | Type |
|---|---|
|  | | 

### <a name="userupdate"></a>Userupdate
Hiermee wordt een gebruiker bijgewerkt.

| Argumenten | Type |
|---|---|
| UserID | | 
| Beschrijving (optioneel)|  | 
| language | <ul><li>1: Engels</li><li>2: Nederlands</li></ul> | 
| userDisabled (optioneel) |<ul><li>0: ingeschakeld</li><li>1: uitgeschakeld</li></ul> | 
| UserEndDate (optioneel) | De datum notatie is ' JJJJ-MM-DDTuu: nn: SS ', waarbij alleen jaar, maand en dag zijn vereist. bijvoorbeeld "2004-12-11" en "2004-11-28T08:00" zijn geldige waarden. | 
| voor naam (optioneel) | | 
| middelste naam (optioneel) | | 
| Achternaam (optioneel) | | 
| maxPermissions (optioneel) | Geheel getal |
| maxRoles (optioneel) | Geheel getal | 


## <a name="getinfo-functions"></a>GetInfo-functies
De set functies die in deze sectie worden beschreven, kan worden gebruikt om gegevens op te halen die zijn opgeslagen in het BHOLD systeem. Elke functie kan worden aangeroepen met behulp van de functie GetInfo van het object BScript. Voor sommige objecten zijn para meters vereist. De gegevens die worden geretourneerd, zijn onderworpen aan de weergave machtigingen en de objecten onder Super visie van de gebruiker die is aangemeld.

### <a name="getinfo-arguments"></a>GetInfo argumenten

| Naam | Beschrijving |
|---|---|
| toepassingen | Retourneert een lijst met toepassingen. | 
| attributetypes | Hiermee wordt een lijst met kenmerk typen geretourneerd. | 
| orgtypes | Retourneert een lijst met typen organisatie-eenheden. | 
| OrgUnits | Retourneert een lijst met organisatie-eenheden zonder de kenmerken van de organisatie-eenheden. | 
| OrgUnitproposedroles | Retourneert een lijst met voorgestelde rollen die zijn gekoppeld aan de organisatie-eenheid. | 
| OrgUnitroles | Retourneert een lijst met rechtstreeks gekoppelde rollen van de opgegeven organisatie-eenheid | 
| Objecttypeattributetypes | | 
| permissions | | 
| permissionusers | | 
| rollen | Retourneert een lijst met rollen. | 
| roletasks | Retourneert een lijst met taken van de betreffende rol. | 
| taken | Retourneert alle taken die bekend zijn bij BHOLD. | 
| gebruikers | Retourneert een lijst met gebruikers. | 
| usersroles | Hiermee wordt de lijst met gekoppelde supervisor rollen van de opgegeven gebruiker geretourneerd. | 
| userpermissions | Hiermee wordt de lijst met machtigingen van de opgegeven gebruiker geretourneerd. | 

### <a name="orgunit-info"></a>Informatie over OrgUnit

| Naam | Parameters | Retourtype |
|---|---|---|
| OrgUnit | OrgUnitID | OrgUnit | 
| OrgUnitasiattributes | OrgUnitID | Verzameling | 
| OrgUnits| filter (optioneel), proptypeid (optioneel)<br> Hiermee wordt gezocht naar eenheden die de teken reeks bevatten die wordt beschreven in **filter** in de proptype die wordt beschreven in **proptypeid** . Als deze ID wordt wegge laten, wordt het filter toegepast op de beschrijving van de eenheid. Als er geen filter is opgegeven, worden alle zicht bare eenheden geretourneerd. | Verzameling | 
| OrgUnitOrgUnits | OrgUnitID | Verzameling | 
| OrgUnitparents | OrgUnitID | Verzameling | 
| OrgUnitpropertyvalues | OrgUnitID | Verzameling | 
| OrgUnitproptypes |  | Verzameling | 
| OrgUnitusers | OrgUnitID | Verzameling | 
| OrgUnitproposedroles | OrgUnitID | Verzameling | 
| OrgUnitroles | OrgUnitID | Verzameling | 
| OrgUnitinheritedroles | OrgUnitID | Verzameling | 
| OrgUnitsupervisors | OrgUnitID | Verzameling | 
| OrgUnitinheritedsupervisors| OrgUnitID | Verzameling | 
| OrgUnitsupervisorroles | OrgUnitID | Verzameling | 

### <a name="role-information"></a>Informatie over rol

| Naam | Parameters | Retourtype |
|---|---|---|
| role | roleID | Object | 
| rollen | filter (optioneel) | Verzameling | 
| roleasiattributes | roleID | Verzameling | 
| roleOrgUnits | roleID | Verzameling | 
| roleparentroles | roleID | Verzameling | 
| rolesubroles | roleID | Verzameling | 
| rolesupervisors | roleID| Verzameling | 
| rolesupervisorroles | roleID | Verzameling | 
| roletasks | roleID | Verzameling | 
| roleusers | roleID | Verzameling | 
| rolesupervisorroles | roleID | Verzameling | 
| proposedroleOrgUnits | roleID | Verzameling | 
| proposedroleusers | roleID | Verzameling | 

### <a name="permission---task-information"></a>Machtiging-taak gegevens

| Naam | Parameters | Retourtype |
|---|---|---|
| hebt | TaskID | Machtiging | 
| permissions | filter (optioneel) | Verzameling | 
| permissionasiattributes | TaskID | Verzameling | 
| permissionattachments | TaskID | Verzameling | 
| permissionattributetypes | - | Verzameling | 
| permissionparams | TaskID | Verzameling | 
| permissionroles | TaskID | Verzameling | 
| permissionsupervisors | TaskID | Verzameling | 
| permissionsupervisorroles | TaskID | Verzameling | 
| permissionusers | TaskID | Verzameling | 
| taak | TaskID | Taak | 
| taken| filter (optioneel) | Verzameling | 
| taskattachments | TaskID | Verzameling | 
| taskparams | TaskID | Verzameling | 
| taskroles | TaskID | Verzameling | 
| tasksupervisors | TaskID | Verzameling | 
| tasksupervisorroles | TaskID | Verzameling | 
| taskusers | TaskID | Verzameling | 

### <a name="user-information"></a>Gebruikersgegevens

| Naam | Parameters | Retourtype |
|---|---|---|
| gebruiker | UserID | Gebruiker | 
| gebruikers | filter (optioneel), attributetypeid (optioneel) <br>Zoekt naar gebruikers die zijn opgenomen in de attributetype die is opgegeven door attributetypeid de teken reeks die is opgegeven met het filter. Als deze ID wordt wegge laten, wordt het filter toegepast op de standaard alias van de gebruiker. Als er geen filter is opgegeven, worden alle zicht bare gebruikers geretourneerd. Bijvoorbeeld:<ul><li>`GetInfo("users")` retourneert alle gebruikers.</li><li>`GetInfo("users", "%dmin%")` retourneert alle gebruikers met de teken reeks "DMin" in de standaard alias.<br></li><li>Stel dat gebruikers een extra kenmerk met de naam hebben `"City".GetInfo("users", "%msterda%", "City")` . Deze aanroep retourneert alle gebruikers met de teken reeks "msterda" in het kenmerk City.</li></ul> | UserCollection | 
| usersapplications | UserID | Verzameling | 
| Userpermissions | UserID | Verzameling | 
| userroles | UserID | Verzameling | 
| usersroles | UserID | Verzameling | 
| userstasks | UserID | Verzameling | 
| usersunits | UserID | Verzameling | 
| usertasks | UserID | Verzameling | 
| userunits | UserID | Verzameling | 

### <a name="return-types"></a>Retour typen
In deze sectie worden de retour typen van de functie GetInfo beschreven.

| Naam | Retourtype |
|---|---|
| Verzameling |```=<ITEMS>{<ITEM description="..." id="..." />}</ITEMS>``` | 
| Object |```=<ITEM type="…" description="..." />``` | 
| OrgUnit |```= <ITEM id="…" description="..." orgtype="..." parent="..."> <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Machtiging |```= <ITEM id="…" description="…" name="…" tokengroup="…" application="…" > <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Rollen |```= <ITEMS> {<ITEM id="…" description="…" />} </ITEMS>``` | 
| Rol |```= <ITEM id="…" description="… " > <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Taak | Machtiging weer geven | 
| Gebruikers | ```= <ITEMS> {<ITEM description="…" id="…" alias="…" />} </ITEMS>``` | 
| Gebruiker |```= <ITEM id="…" description="…" alias="…" firstname="…" lastname="…" uuid="…" language="…"> <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 

## <a name="script-sample"></a>Voorbeeld script

Een bedrijf heeft een BHOLD-server en wil een geautomatiseerd script waarmee nieuwe klanten worden gemaakt. De informatie over het bedrijf en zijn/haar aankoop Manager worden op een aangepaste webpagina ingevoerd. Elke klant wordt in het model gepresenteerd als een eenheid onder de eenheid klanten. De aankoop Manager is ook lid van een super Visor van deze eenheid. Er wordt een rol gemaakt waarmee de eigen aren het recht krijgen om in naam van de nieuwe klant te kopen.

Deze klant bestaat echter niet in de toepassing. Er is een speciale functie geïmplementeerd in de ASI-FunctionDispatch die een nieuw klant account in de aankoop toepassing maakt. Elke klant heeft een klant type.

De mogelijke typen kunnen ook worden weer gegeven door de FunctionDispatch-functie. AA kiest voor de nieuwe klant het juiste type.

Maak een rol en taak om de aankoop rechten te presen teren. De bevoegdheid voor echte aankoop wordt door de ASI als een bestand gepresenteerd `/customers/customer id/purchase` . Dit bestand moet worden gekoppeld aan de nieuwe taak.

De pagina Active Server die de informatie verzamelt, ziet er als volgt uit:

```VB
<%@ Language=VBScript %>
<% Option Explicit %>
<html>
<body>
<form action="MySubmit.asp" method=post>
<input type="hidden" name="OrgUnitID" 
     value="<% = Request("ID") %>">
Company <input type="text" name="Description"> <br>
Type <select name="OrgType">
<%Dim oOrgType
For Each oOrgType on bscript.getinfo("Orgtypes") %>
<option value="<% = oOrgType.OrgTypeID %>">
<% = oOrgType.Description %>
</option> <%
Next %>
</select>  <br>
Manager <input type="text" name=" manager"> <br>
Alias <input type=" text" name=" alias"> <br>
e-mail <input type=" text" name=" email"> <br>
<input type="submit">
</form>
</body>
</html>
```

Alle aangepaste pagina's zouden nodig moeten zijn om de juiste informatie op te vragen en een XML-document te maken met de aangevraagde informatie. In dit voor beeld transformeert de MySubmit-pagina de gegevens in het XML-document en wijst deze toe aan de **b1script. Met het object para meters** en wordt de `b1script.ExecuteScript("MyScript")` functie aangeroepen.

In het volgende invoer script wordt dit voor beeld weer gegeven:

```
<customer>
<description>ACME inc.</description>
<orgtype>5<orgtype>
<name>John Doe</name>
<alias>jdoe</alias>
<email>jdoe@acme.com</email>
</customer>
```

Dit invoer script bevat geen opdrachten voor BHOLD. Dit is omdat dit script niet rechtstreeks door BHOLD wordt uitgevoerd. in plaats daarvan is dit de invoer voor een vooraf gedefinieerde functie. Deze vooraf gedefinieerde functie zet dit object om in een XML-document met BHOLD-opdrachten. Dit mechanisme inhoudt dat de gebruiker scripts naar het BHOLD-systeem kan verzenden die functies bevatten die de gebruiker niet mag uitvoeren, zoals setUser en functie-verzen ding naar een ASI.

```
<?xml version="1.0" encoding="utf-8" ?> 
- <functions xmlns="http://tempuri.org/BscriptFunctions.xsd">

  <function name="roleadduser" roleid="" userid="" /> 
  <function name="roledeleteuser" roleid="" userid="" /> 
  </functions>
```

## <a name="next-steps"></a>Volgende stappen
- [Handleiding voor BHOLD-concepten](../bhold/bhold-concepts-guide.md)
- [Versiegeschiedenis van BHOLD](version-bhold-history.md)
