---
title: Resource besturingselement weergave Configuration XML-referentie | Microsoft Docs
description: 
keywords: 
author: fimguy
ms.author: fimguy
manager: mbaldwin
ms.date: 05/1/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c6ed843fb15b150fc934062945ab76ba32d72d33
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# <a name="resource-control-display-configuration-xml-reference"></a>Resource besturingselement weergave Configuration XML-referentie

Resource besturingselement weergave configuratie (RCDC) resources zijn gebruiker gedefinieerde resources die u gebruiken kunt om te bepalen hoe de andere bronnen in de gegevensopslag van Microsoft Identity Manager 2016 SP1 (MIM) worden weergegeven in de gebruikersinterface (UI) voor de eindgebruiker. Elke RCDC-bron bevat een XML-configuratiebestand die u wijzigen kunt als u wilt toevoegen, wijzigen of verwijderen van de tekst van de gebruikersinterface en UI-besturingselementen. Hoewel MIM 2016 SP1 verschillende standaard RCDC-bronnen biedt, kunt u ook aangepaste RCDC-bronnen voor aangepaste bronnen maken. Zie voor meer informatie over het gebruik van de UI RCDC in de FIM-Portal [Inleiding tot het configureren en aanpassen van de FIM-Portal](http://go.microsoft.com/fwlink/?LinkID=165848) in de FIM-documentatie.


## <a name="known-issues"></a>Bekende problemen

De standaardwaarde in veel Resource voor Bronbesturingselementen besturingselementen wordt niet ondersteund.

In deze release, instellen van standaardwaarden in besturingselementen in een besturingselement Resource wordt niet ondersteund, met uitzondering van het besturingselement keuzerondje. U kunt dit probleem op voor een vervolgkeuzelijst omzeilen door te geven van een standaardwaarde die niet is gekoppeld aan een waarde en de gebruiker te dwingen de selectie te wijzigen. U kunt dit probleem is met andere besturingselementen omzeilen, moet u een werkstroom autorisatie gebruiken om op te geven van een standaardwaarde tijdens het indienen van de aanvraag.

## <a name="basic-structure"></a>Algemene structuur

De XML-gegevens voor een resource RCDC bestaat uit één XML-element **ObjectControlConfiguration.**

>[!NOTE]
Zie bijlage A: standaard XSD-Schema verderop in dit document voor het volledige XSD-schema.

Hier volgt het XSD-schema voor het element ObjectControlConfiguration:

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```



De **ObjectControlConfiguration** element bevat het volgende:

1.  **ObjectDataSource**: dit element bevat de TypeName van een data source-klasse die gebruikmaakt van de Resource-besturingselement (RC). Zie het volgende gedeelte met gegevens in dit document voor een beschrijving en de schemadefinitie. Een **ObjectControlConfiguration** element mag maximaal 32 knooppunten van de **ObjectDataSource** element.

2.  **XmlDataSource**: dit is een eenvoudige gegevensbron die meestal wordt gebruikt om het ontwerp van een overzichtspagina geven. Zie het volgende gedeelte met gegevens in dit document voor een beschrijving en de schemadefinitie. Een **ObjectControlConfiguration**: element mag maximaal 32 knooppunten van de **XmlDataSource** element.

3.  **Deelvenster**: beheerder kan de indeling van de pagina RCDC aanpassen door het wijzigen van elementen in de elementen van het Configuratiescherm. Zie de sectie Configuratiescherm verderop in dit document voor meer informatie. Een **ObjectControlConfiguration** element moet hebben slechts één element van het Configuratiescherm.

4.  **Gebeurtenissen**: beheerders aangepaste code achter niet opgeven, is deze functie beperkt. Dit is de gebeurtenis die een deelvenster of een besturingselement verzenden kunt, op basis van een statuswijziging. Zie de sectie gebeurtenissen verderop in dit document voor meer informatie. Een **ObjectControlConfiguration** element kan desgewenst een bevatten **gebeurtenis** element. In het algemeen is het gebruik van aangepaste **gebeurtenissen** wordt niet ondersteund tenzij specifiek ontwikkeld in latere verbeteringen.

## <a name="data-sources"></a>Gegevensbronnen

Microsoft Identity Manager maakt gebruik van gegevensbronnen als een manier om gegevens binden aan UI-onderdelen. Deze vergemakkelijkt scheiding van de gegevens uit de laag voor presentatie. Er zijn twee soorten gegevensbronnen in de configuratiegegevens voor RCDC-resource: **ObjectDataSource** en **XmlDataSource**.

-   **ObjectDataSources** een Microsoft .NET-klasse die de gegevens naar de RC opgeven. Er is een vaste set van de beschikbare typen ObjectDataSources voorwaarde dat de beheerder kiezen kan om te gebruiken bij het ontwerpen van RCDCs.

-   **XMLDataSources** bieden een eenvoudige manier om gegevens op basis van een XML-structuur en ze kunnen worden gebruikt door beheerders om aangepaste gegevens te leveren. De XML-gegevens moet worden opgegeven rechtstreeks in de Weergaveconfiguratie, tenzij u de ingebouwde, vooraf gedefinieerde XML-structuur. De ingebouwde XML-structuur wordt gebruikt voor het genereren van samenvattende pagina's in de RC.

U kunt deze gegevensbronnen in de Weergaveconfiguratie binden op kenmerken van de UI-besturingselementen die zijn opgegeven in de Weergaveconfiguratie voor het genereren van de gebruikersinterface.

### <a name="objectdatasource"></a>ObjectDataSource

Microsoft Identity manager biedt de algemene typen gegevensbronnen in de volgende tabel die beschikbaar voor alle brontypen zijn (tenzij anders is aangegeven).

| TypeName                        | Beschrijving     | Binding in twee richtingen ondersteunt | Syntaxis van de binding wordt ondersteund        |
|----------------|-----------|-------------------------|--------------|
| PrimaryResourceObjectDataSource | Hiermee wordt de FIM 2010-resource die wordt gemaakt, bewerkt of bekeken. Het pad in de bindingstekenreeks is de naam van het kenmerk. Houd er rekening mee dat het resourcetype is opgegeven door het kenmerk TargetObjectType van de RCDC in plaats van in de Weergaveconfiguratie. ConfigurationData-kenmerk. | Ja                     | [AttributeName] De waarde van het objectkenmerk opgegeven door de naam ervan.    |
| PrimaryResourceDeltaDataSource  | Deze gegevensbron bouwt de delta XML-bestand dat de oorspronkelijke status en de huidige status van de FIM 2010-resource worden vergeleken. De gegenereerde delta XML wordt verbruikt door het besturingselement voor RC samenvatting weergeven van de gebruikersinterface voor het verzenden van de gebruiker een aanvraag.                                    | Nee                      | DeltaXml: </br> Dit wordt gebruikt bij het besturingselement samenvatting om de verschillen weer te geven.                                                 |
| PrimaryResourceRightsDataSource | Deze gegevensbron biedt de inline-rechten voor elk kenmerk van de FIM 2010-resource. Hierdoor kunnen de RC om te bepalen wanneer er nog verzending van welke machtigingen die de gebruiker op dit kenmerk heeft en klik vervolgens op de juiste wijze genereren de gebruikersinterface voor dit kenmerk.                     | Nee                      | [AttributeName]                                                                                         |
| SchemaDataSource                | Deze gegevensbron kan worden gebruikt voor toegang tot de schema-gerelateerde informatie, zoals weergegeven naam, beschrijving, ongeacht of het kenmerk is vereist, evenals resourcegegevens type.                                                                                             | Nee                      | [AttributeName]. **Vereist** Booleaanse waarde die aangeeft of het kenmerk moet een waarde is alleen geldig hebben. <br/> [AttributeName]. **DisplayNameString** waarde die aangeeft van de binding weergavenaam <br/>[AttributeName]. **DescriptionString** waarde die aangeeft van de binding-beschrijving <br/>[AttributeName]. StringRegexString-waarde die Regex tekenreeks van een binding aangeeft. <br/>[AttributeName]. **Weergavenaam** <br/> [AttributeName]. **Beschrijving** <br/> [AttributeName]. [AttributeName]. **IntergerValueMinimum** <br/>[AttributeName]. **IntergerValueMaximum** <br/>[AttributeName]. **LocalizedAllowedValues**|
| DomainDataSource                | Deze gegevensbron bevat een opsomming van domeinen, op basis van de configuratie domeinbronnen. Houd er rekening mee dat deze gegevensbron kan alleen worden gebruikt in RCDCs die voor de groepbronnen en Gebruikersbronnen.                                                                           | Ja                     | Domein           |

Hier volgt een voorbeeld RCDC codefragment die drie gegevensbronnen koppelt aan het besturingselement UocTextBox om het kenmerk Beschrijving van een groep te bewerken:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource"></a>XMLDataSource

Met behulp van een **XMLDataSource**, kunt u aangepaste gegevens die de RCDC voor een bepaalde bron gebruiken kan opgeven. In dit geval moet de XML-gegevens worden opgegeven in de Weergaveconfiguratie. Als alternatief kan deze gegevensbron worden gebruikt om te verwijzen naar een ingebouwde XML-gegevensstructuur zodat de gebruikersinterface voor samenvattende pagina's worden weergegeven. U bepaalt welk type **XMLDataSource** moet worden gebruikt wanneer u deze in de Weergaveconfiguratie definiëren.


| TypeName                 | Beschrijving   | | |
|--------------------------|------------|
| **XMLDataSource**            | De gegevensbron vertegenwoordigt XML-gegevens. Het kan XSL-of ingesloten XSL-indelingen zijn: <br/>**XSL-indeling:** <br/> Microsoft.IdentityManagement.WebUI.Controls.dll < Mijn: XmlDataSource mijn: naam = ' <br/>summaryTransformXsl"my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl' >< / Mijn: XmlDataSource ><br/> **Ingesloten XSL-indeling:** <br/> < Mijn: XmlDataSource mijn: naam = "RequestStatusTransformXsl" > <br/> < xsl: stylesheet versie = "1.0" xmlns:xsl http://www.w3.org/1999/XSL/Transform = <br/> xmlns:msxsl = ' urn: schemas-microsoft-com:xslt "><br/>< / xsl: stylesheet >< / Mijn: XmlDataSource >                       |Nee | ```Xpath[;namespaces]``` <br/> Waar: Xpath is een geldig XML-xpath het vaakst selecteert u de vereiste Opmerking ' / ' (root) <br/>Naamruimten is een optionele lijst met voorvoegsel = URI tekenreeksen, gescheiden door puntkomma's, indien vereist voor het xpath werken tegen namespaced XML. |
| **ReferenceDeltaDataSource** | De gegevensbron vertegenwoordigt delta's van verwijzingskenmerken met meerdere waarden. Dit wordt alleen gebruikt voor RCDC voor de groep en Set. <br/> Hoewel de gegevensbron niet beperkt tot groepen of Sets is, vereist deze codewijzigingen in de hosteigenschappen RCDC dergelijke delta's verzenden. Groep en stel zijn momenteel de enige hosts die deze gegevensbron wordt herkend.  | Ja                      | [AttributeName]. Toevoegen waarbij [AttributeName] Hiermee geeft u een verwijzingskenmerk en de geretourneerde gegevens voor de delta-toevoegingen. <br/> Voorbeeld: [ReferenceAttribute]. Toevoegen <br/>Voorbeeld: ```<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}" />``` <br/>[AttributeName]. Verwijder waarbij [AttributeName] Hiermee geeft u een verwijzingskenmerk en de geretourneerde gegevens voor de delta-verwijderingen. <br/> DeltaXml |
|**RequestDetailsDataSource**| De gegevensbron vertegenwoordigt het kenmerk RequestParameter van objecten van de aanvraag. De parameter wordt het maximum aantal kenmerkwaarden moet worden weergegeven per kenmerk met meerdere waarden die alleen in de Weergaveconfiguratie voor de aanvraag gebruikt wordt ingesteld. ```<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />```| Nee | DeltaXml |
|**RequestStatusDataSource**| De gegevensbron vertegenwoordigt de **RequestStatusDetails** kenmerk van de aanvraag-objecten. <br/>Dit wordt alleen gebruikt in RCDC voor aanvraag.  | Nee | DeltaXml |

-   Een aangepaste XML-gegevensbron definiëren:

 ```XML
   <my:XmlDataSource my:Name="MyCustomData" >
   %Insert custom, properly formatted XML data here%
   </my:XmlDataSource>
   ```

-   Definieer de gegevensbron als volgt voor het gebruik van de ingebouwde samenvatting besturingselementen XSL:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

 Als u een RCDC voor een aangepast brontype maakt, kunt u deze methode om automatisch een overzichtspagina voor de aangepaste resource weer te geven.

Hier volgt een voorbeeld van het maken van een samenvatting tabblad in de Weergaveconfiguratie, met de PrimaryResourceDeltaDataSource de XMLDataSource met behulp van de ingebouwde XSL:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

Als alternatief kunt het eerder opgegeven met de volgende notatie voor het definiëren van een aangepaste indeling van een overzichtspagina XmlDataSource-element worden vervangen door de gebruiker. De standaardwaarde FIM 2010 samenvatting XSL is als een verwijzing opgenomen in de bijlage B: standaard samenvatting XSL verderop in dit document.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Schema voor gegevensbronnen
Hier volgt het XSD-schema voor de twee soorten gegevensbronnen:

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="events"></a>Gebeurtenissen
Een gebeurtenis definieert de gewijzigde status van een besturingselement. De uitbreidbaarheid van deze functie is beperkt, omdat u een aangepaste functie (Handler) kan niet schrijven naar het definiëren van wat het gedrag is nadat een gebeurtenis wordt geactiveerd. Het element met dezelfde gebeurtenis kan worden gebruikt in het Configuratiescherm-element. Zie de sectie Configuratiescherm verderop in dit document voor meer informatie. Hier volgt het XSD-schema voor het element gebeurtenis:

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```


Een gebeurtenis is een leeg element en heeft twee kenmerken.

**Kenmerken:**

1.  **Naam**: dit is de unieke naam van een gebeurtenis. De enige ondersteunde gebeurtenis in de **ObjectControlConfiguration** is de Load-gebeurtenis. Deze gebeurtenis wordt geactiveerd wanneer de pagina voor het eerst wordt geladen.

2.  **Handler**: dit is de unieke naam van een handler. Wanneer de gebeurtenis wordt geactiveerd, meestal een methode wordt aangeroepen voor het afhandelen van de wijziging van de status van het besturingselement. de volgende gevallen worden niet ondersteund: verwijderen van een bestaande-handler van een bestaand besturingselement, een nieuwe handler maken en koppelen aan een bestaande of nieuwe besturingselement.

Voorbeeld:

Hier volgt een voorbeeld van een element gebeurtenissen.
```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

**Configuratiescherm**


Het Configuratiescherm-element is de core-element in een RCDC-indeling. Hier volgt het XSD-schema voor het element Configuratiescherm:

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

Dit element bevat een element terugkerende groeperen. Zie de sectie groepering in dit document voor meer informatie.

Het Configuratiescherm-element bevat vier kenmerken:

1.  **Naam**: de naam van het paneel. Dit is een vereist, type string-kenmerk.

2.  **DisplayAsWizard**: dit kenmerk op dat moment is afgeschaft. De bijbehorende VerbContext-kenmerk op de RCDC bepaalt als de indeling van de resource in de Wizard-modus of tabblad modus is. Als deze is ingesteld op 0 (aanmaakmodus), is het ook in de Wizard-modus. Anders is in de Tab-modus. Zie Inleiding tot het configureren en aanpassen van de FIM-Portal in de documentatie voor meer informatie.

3.  **Bijschrift**: dit kenmerk op dat moment is afgeschaft. De gebruiker kunt bijschriften voor een pagina opgeven door een groep die alleen de koptekst bevat. Zie de sectie groepering in dit document voor meer informatie.

4.  **AutoValidate**: dit is een optionele Boole-kenmerk. Wanneer deze is ingesteld op true, validatie wordt geactiveerd op basis van elk besturingselement in het huidige tabblad. Standaard als het kenmerk ontbreekt, wordt deze ingesteld op true. Het kan worden gebruikt in combinatie met de eigenschap RegularExpression. Zie 'RegularExpression' in verderop in dit document voor meer informatie.

## <a name="grouping"></a>Groeperen

Het element groepering bepaalt de algemene indeling van een paneel. Het fungeert als een container waarin de afzonderlijke besturingselementen gegroepeerd in andere secties en tabbladen. Hier volgt het XSD-schema voor het element groeperen:

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```


Er zijn drie soorten **groeperingen**:

1.  **Koptekst groepering**: A Header groepering is optioneel. Er kan niet meer dan één Header groeperen in een **Configuratiescherm**. Een groepering koptekst wordt weergegeven boven op een paneel als bijschrift.
    Slechts één UocCaptionControl is moet worden gebruikt in deze groepering toegestaan. Zie de sectie Voorbeeld voor een voorbeeld van een koptekst groepering.

2.  **Inhoud groeperingen**: ten minste één Inhoudsgroepering is vereist. Er kunnen zich meerdere inhoud groeperingen in een paneel. Een groepering van de inhoud wordt weergegeven als de belangrijkste inhoud van een pagina RCDC. Elke Inhoudsgroepering wordt weergegeven als een tabblad in hetzelfde deelvenster en van 1 naar 256 besturingselementen kan bevatten. Zie de sectie van het voorbeeld hieronder voor een voorbeeld van een **Inhoudsgroepering**.

3.  **Samenvatting groeperingen**: A samenvatting groepering is optioneel. Er kan alleen worden één samenvatting groeperen in een deelvenster. Een groepering van de samenvatting weergegeven als het laatste tabblad van een paneel. Slechts één **UocHtmlSummary** besturingselement kan worden gebruikt in een groepering samenvatting om de wijzigingen die de gebruiker heeft doorgevoerd vóór het verzenden van een aanvraag weer te geven. Zie de sectie voorbeeld hieronder voor een voorbeeld van een groepering samenvatting.

Elk type groepering bevat de volgende elementen:

1.  **Help**: dit element bevat Help-tekst in een tabblad. U kunt het ook een koppeling toevoegen aan een Help-bestand voor het tabblad gebruiken.

2.  **Besturingselementen**: Zie de sectie besturingselement in dit document voor informatie over dit element. Elke groep moet 1 tot 256 besturingselementen liggen, afhankelijk van het type van de groepering hebben.

3.  **Gebeurtenissen**: Zie de sectie gebeurtenissen in dit document voor informatie over dit element. Elke groep kunt, als een optie hebben één gebeurtenis. De gebeurtenissen die worden ondersteund in een element groepering zijn als volgt:

    - **BeforeLeave**: deze gebeurtenis wordt geactiveerd wanneer de gebruiker is gereed om te leiden dat een tabblad in een groepering van inhoud.
    - **AfterEnter**: deze gebeurtenis wordt geactiveerd wanneer de gebruiker gereed is voor het invoeren van een tabblad in een groepering van inhoud.

Kenmerken:

1.  **Naam**: dit is de vereiste naam van de groepering. De **naam** moet uniek zijn binnen de **Configuratiescherm**.

2.  **Bijschrift**: de **bijschrift** wordt weergegeven als het bijschrift van de header in een groepering-Header. Deze wordt weergegeven als het bijschrift van het tabblad van een inhoud of samenvatting groeperen.

3.  **Beschrijving**: een optionele tekenreekskenmerk **beschrijving** werkt alleen wanneer deze wordt gebruikt in een groepering van inhoud. Gebruik dit element zodat de eindgebruiker een aantal details over de gegevens binnen hetzelfde tabblad.

  >[!NOTE]
  Als dit kenmerk wordt gebruikt in een groepering van de samenvatting, kan het XML-bestand wordt beschouwd als ongeldig te zijn. Als dit kenmerk wordt gebruikt in een Header groepering, het XML-bestand wordt beschouwd als geldig, maar genegeerd.

4.  **Ingeschakeld**: een optionele Boole-kenmerk Enabled is ingesteld op true als deze ontbreekt. Als ingeschakeld is ingesteld op false, ziet de gebruiker een tabblad uitgeschakeld. Dit kenmerk is werken alleen in een groepering van inhoud.

  >[!NOTE]
  Als dit kenmerk wordt gebruikt in een groepering van de samenvatting, kan het XML-bestand wordt beschouwd als ongeldig te zijn. Als dit kenmerk wordt gebruikt in een Header groepering, het XML-bestand wordt beschouwd als geldig, maar genegeerd.

5.  **Zichtbaar**: U kunt een tabblad RCDC-pagina of de kop verbergen door dit kenmerk instelt op false. Standaard deze optionele, Boolean-type-kenmerk is ingesteld op true. Dit kenmerk is werkt alleen op een groepering van inhoud.

  >[!NOTE]
  Deze functie werkt niet wanneer er slechts één inhoud groeperen in een deelvenster. Wanneer er meer dan een groepering van inhoud in een deelvenster, gedraagt zich als hierboven beschreven.

6.  **IsHeader**: dit kenmerk is een optionele, Boolean-kenmerk die bepaalt of de groepering een groepering-Header. Als dit kenmerk niet is opgegeven, wordt deze ingesteld op false.

7.  **IsSummary**: dit is een optionele, Boole-kenmerk die bepaalt of de groepering een samenvatting groepering. Als dit kenmerk niet is opgegeven, wordt deze ingesteld op false.

![Ontv configuratie-XML](media/rcd-configuration-xml-reference/image005.jpg)

De volgende XML-code genereert de vorige Header groepering. De groepering Header is het gebied met de tekst voorbeeld Header groepering.

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

![Ontv configuratie-XML](media\rcd-configuration-xml-reference/image007.jpg)

De volgende XML-code genereert de vorige Inhoudsgroepering. De inhoud groepering is het meest linkse tabblad met de tekst **voorbeeld inhoud groepering**.

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

![Ontv configuratie-XML](media/rcd-configuration-xml-reference/image010.jpg)

De volgende XML-code genereert de vorige samenvatting groepering. De groepering samenvatting is de meest rechtse tab met de tekst **samenvatting**.

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```
### <a name="help"></a>Help

Het Help-element, kan als een optie worden opgenomen in een groep of een controle-element. Als het wordt gebruikt in een groep, moet het eerste element gebruikt. Het biedt tekstuele Help voor de eindgebruikers zodat deze nauwkeurige informatie bieden. Hier volgt het XSD-schema voor het Help-element:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```
Hier volgt een voorbeeld van het Help-element:
```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control"></a>Beheer

Een groepering-element bevat een of meer elementen van het besturingselement. Besturingselementen zijn de belangrijkste elementen in een RCDC. U kunt het element groepering aanpassen met het definiëren van de verschillende besturingselement-elementen die deze bevat. Hier volgt het XSD-schema voor het besturingselement element:

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

Een besturingselement bevat volgende elementen:

1.  **Help**: dit element wordt genegeerd. Dit werkt alleen in groepering.

2.  **CustomProperties**: dit element wordt niet ondersteund.

3.  **Opties**: dit element wordt alleen gebruikt in combinatie met de **UocDropDownList** of **UocRadioButtonList** besturingselementen. Deze werkt niet met andere besturingselementen. Zie de sectie opties in dit document voor de structuur van dit element. Zie de afzonderlijke besturingselement om te zien hoe deze wordt gebruikt in de context van een besturingselement.

4.  **Knoppen**: dit element wordt alleen gebruikt in combinatie met de **UocListView** besturingselement. Deze werkt niet voor andere besturingselementen. Zie de sectie UocListView in dit document voor meer informatie.

5.  Eigenschappen: Dit element wordt gebruikt in alle besturingselementen aanvullende gedrag van een besturingselement opgeven. Zie de sectie met eigenschappen in dit document voor informatie over dit element.

6.  **Gebeurtenissen**: Zie de sectie gebeurtenissen eerder in dit document voor de structuur van dit element. Zie de definitie van de afzonderlijke besturingselement om te bepalen welke gebeurtenis wordt in dat besturingselement gebruikt.

Een besturingselement bevat de volgende kenmerken:

1.  **Naam**: dit is de naam van het besturingselement. De naam van een besturingselement moet uniek zijn binnen elke Configuratiescherm. Dit is een vereist, type string-kenmerk.

2.  **TypeName**: dit kenmerk wordt opgegeven welk type besturingselement is. Dit is een vereist, type string-kenmerk. Zie de sectie afzonderlijke besturingselementen in dit document voor de naam van elk besturingselement.

3.  **Bijschrift**: U kunt dit kenmerk een bijschrift voor het besturingselement moet worden opgenomen.
    Het bijschrift is meestal de weergavenaam van de gegevens die door het besturingselement wordt weergegeven of invoeren. U kunt expliciet een waarde opgeven voor het bijschrift of binding tot stand brengen met schema-kenmerk weergave naam-informatie. Het bijschrift wordt weergegeven op de meest linkse zijde van een besturingselement normale grootte. Als een besturingselement wordt het volledige scherm spanning die het bijschrift wordt weergegeven boven het besturingselement. Dit is een optioneel, type string-kenmerk. Zie de sectie met eigenschappen voor informatie over het koppelen van een gegevensbron via een kenmerk of een waarde van eigenschap.

   Het volgende voorbeeld ziet u hoe een bijschrift expliciet kan worden gebruikt:

   ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
   ```
     In het volgende voorbeeld wordt getoond hoe een bijschrift kan worden gebruikt met een gegevensbron. Als u de sjabloon hebt gebruikt voor een gegevensbron die u eerder in dit document wordt weergegeven, is uw gegevensbron schema. Het is raadzaam dat u de weergavenaam van het kenmerk met een bijschriftkenmerk binden.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```
4.  Ingeschakeld: Dit is een optionele, Boolean-type-kenmerk. De waarde van dit kenmerk instelt op false, kan de gebruiker een besturingselement uitschakelen. De standaardwaarde is ingesteld op true.

5.  Zichtbaar: Dit is een optionele, Boolean-type-kenmerk. U kunt dit kenmerk gebruiken voor het verbergen van het gehele besturingselement. De standaardwaarde is ingesteld op true.

6.  Beschrijving: Gebruik dit optioneel, type string-kenmerk Neem een beschrijving zodat de eindgebruiker begrijpen wat ze moeten geplaatst in het besturingselement of wat het doet. U kunt expliciet een waarde opgeven voor de beschrijving of binding tot stand brengen met de schemagegevens voor de kenmerk-Beschrijving. <br/>De beschrijving wordt weergegeven op de meest linkse zijde van een besturingselement normale grootte onder het bijschrift. Als een besturingselement wordt het volledige scherm spanning, de beschrijving wordt weergegeven boven aan het besturingselement onder het bijschrift. Zie de sectie met eigenschappen in dit document voor informatie over het koppelen van een gegevensbron via een kenmerk of een waarde van eigenschap.

7.  Het volgende voorbeeld ziet u hoe een beschrijving expliciet kan worden gebruikt:
  ```XML
  <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
  ```
  Dit voorbeeld toont hoe een beschrijving met een gegevensbron kan worden gebruikt. Als u de sjabloon hebt gebruikt voor een gegevensbron die u eerder in dit document wordt weergegeven, is het uw gegevensbron **schema**. Het is raadzaam dat u het kenmerk binden **beschrijving** met een het beschrijvingskenmerk.
  ```XML
  <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
  ````
8. ExpandArea: Voor dit kenmerk geeft aan of het besturingselement het volledige scherm omvat. Dit is een optionele, Boolean-type-kenmerk. De standaardwaarde is ingesteld op false.

    >[!NOTE]
    De kenmerken bijschrift en een beschrijving worden uitgeschakeld als dit kenmerk is ingesteld op true. Het besturingselement UocLabel moet u een bijschrift voor een uitgebreide controle bieden.
9. **Hint**: dit is een optioneel, type string-kenmerk. De tekst in het kenmerk Hint helpt de eindgebruiker bepalen wat is er een geldige invoer voor het besturingselement. De Hint wordt weergegeven onder het besturingselement.

10.  **AutoPostback**: dit is een optionele, Boolean-type-kenmerk. De standaardwaarde is ingesteld op false. Indien ingesteld op false, de pagina te vernieuwen wordt niet vernieuwd in het besturingselement. Voor informatie over AutoPostback, zoekt u de eigenschap van het Microsoft ASP.NET UI-besturingselement met dezelfde naam.

11. **RightsLevel**: dit is een optioneel, type string-kenmerk. U kunt dit kenmerk alleen met inline-rechten aan een gegevensbron binden. Het besturingselement wordt dynamisch ingeschakeld of uitgeschakeld, op basis van de rechten van de gebruiker. Zie de sectie met eigenschappen in dit document voor informatie over het koppelen van gegevensbronnen met een kenmerk of een waarde van eigenschap.

    Dit voorbeeld ziet u hoe een **RightsLevel** kenmerk kan worden gebruikt met een gegevensbron. Als u de sjabloon hebt gebruikt voor een gegevensbron die u eerder in dit document wordt weergegeven, is het uw gegevensbron **rechten**. Naam van het kenmerk gebruiken als het pad.

### <a name="properties"></a>Eigenschappen

U kunt een eigenschap gebruiken voor het verder aanpassen van het gedrag van elk besturingselement. Een eigenschap is een leeg element. Hier volgt het XSD-schema voor de eigenschapselement:
```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```



Elke eigenschap heeft twee vereiste kenmerken:

1.  **Naam**: dit kenmerk van het type tekenreeks is de unieke naam van de eigenschap.
    Andere besturingselementen hebben andere eigenschappen. Er zijn enkele algemene eigenschappen die kunnen worden gebruikt door alle besturingselementen. Zie voor meer informatie over welke namen beschikbaar voor een bepaald besturingselement zijn de algemene eigenschappen en de afzonderlijke besturingselementen sectie.

2.  **Waarde**: dit is de waarde van de eigenschap. Het gegevenstype van de waarde afhankelijke services op waarvan de eigenschap is toegewezen aan. Zie de volgende sectie voor de indeling van de toegestane waarde voor specifieke eigenschappen.

Sommige eigenschappen kunnen met informatie van een gegevensbron worden gebonden. U moet de indeling van de volgende tekenreeks gebruiken om dit te doen. Zie de afzonderlijke eigenschappen in de sectie afzonderlijke besturingselementen wilt weten hoe ze verbinding maken met een gegevensbron.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}”

SourceExpression:= “Source=” + [ObjectDataSourceName]

PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

ModeExpression:= “Mode=” + [ModeChoice]

ModeChoice:= “OneWay”|”TwoWay”

ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

AttributeName:= valid schema attribute name from the data source.

AttributePropertyName:= valid property name of a schema attribute from the data source.

````
**VOORBEELD:**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>

```



<a name="common-properties"></a>Algemene eigenschappen
-----------------

Alle RCDC-besturingselementen die zijn opgegeven in dit document kunnen de volgende algemene eigenschappen hebben. U kunt deze eigenschappen samen met andere eigenschappen die specifiek voor andere besturingselementen zijn.

1.  Vereist: Deze eigenschap geeft aan dat het veld een vereist veld of een optioneel veld. Een vereist veld moet gevuld met een waarde. Een lege waarde wordt niet ondersteund in het geval van een tekenreeksinvoer. Een optioneel veld kan leeg zijn. Als dit veld een verplicht veld zonder waarde ingevuld is, wordt een foutbericht weergegeven boven het besturingselement voor invoer op. U kunt expliciet opgeven of een veld vereist of optioneel is. U kunt ook het veld met de schema-informatie van een bepaalde binding tussen een kenmerk en een resourcetype binden. Standaard, als deze eigenschap niet weergegeven wordt, betekent dit dat het besturingselement is een optionele invoer.

   Hier volgt een voorbeeld met een expliciete waarde op voor deze eigenschap:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```
   Dit is een voorbeeld die gebruikmaakt van een dynamische gegevensbron voor deze eigenschap. Als u de sjabloon voor een gegevensbron die wordt weergegeven in de vorige sectie van dit document hebt gebruikt, is uw gegevensbron schema. Gebruik \<kenmerknaam\>. Vereist als het pad.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```
2. **Alleen-lezen**: met deze eigenschap instelt op true, de eindgebruiker optreedt in het besturingselement in een alleen-lezen-modus. Dit is een optionele, Boolean-type-kenmerk.
    De standaardwaarde is ingesteld op false. Echter wordt ook het gedrag van deze eigenschap overschreven door het type van de rechten die van een persoon op de gegevens worden gebonden aan het besturingselement heeft. Als een gebruiker geen rechten voor het bijwerken van een veld en het veld is gekoppeld met inline rechten, ziet de gebruiker de gegevens in een alleen-lezenmodus zelfs die deze eigenschap is ingesteld op false.

3.  **RegularExpression**: deze eigenschap geeft u beperkingen die zijn ingesteld op de waarde in het besturingselement. De indelingen van deze eigenschapswaarde zijn de indelingen die worden ondersteund in de standaard .NET-StringRegex. Zie voor meer informatie [.NET Framework Regular Expressions](http://go.microsoft.com/fwlink/?LinkId=165361). Als het besturingselement wordt gebruikt voor het invoeren van een waarde, de waarde wordt vergeleken met de beperking die is opgegeven in deze eigenschap wanneer de gebruiker atempts te verlaten van de huidige pagina.
    Het foutbericht wordt weergegeven boven op het besturingselement dat ongeldige invoer heeft. De gebruiker kan een reguliere expressie voor tekenreeksen expliciet opgeven. De gebruiker kan ook het binden met schema-informatie van een bepaald kenmerk. Standaard, als deze eigenschap niet weergegeven wordt, betekent dit dat het besturingselement wordt niet gecontroleerd voor beperkingen op invoer tekenreeksen.
    Hier volgt een voorbeeld met een expliciete waarde op voor deze eigenschap:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```
    Dit is een voorbeeld die gebruikmaakt van een dynamische gegevensbron voor deze eigenschap. Als u de sjabloon hebt gebruikt voor een gegevensbron die eerder in dit document wordt weergegeven, is uw gegevensbron schema. Gebruik de <attribute name>. StringRegex als het pad.
    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```
4.  Zichtbaar: Dit is een optionele, Boolean-type-kenmerk. U kunt dit kenmerk gebruiken voor het verbergen van het gehele besturingselement. De standaardwaarde is ingesteld op true.

### <a name="options"></a>Opties

De opties-element bevat een of meer optie subknooppunten. De opties-element wordt alleen gebruikt met de besturingselementen UocRadioButtonList en UocDropDownList. Zie de sectie besturingselementen niet afzonderlijk voor meer informatie over het gebruik ervan. Hier volgt het XSD-schema voor het element opties:

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```


Kenmerken:

1.  Waarde: Dit is een vereist kenmerk van het tekenreekstype. De kenmerkwaarde moet uniek zijn binnen hetzelfde besturingselement. Alleen A-Z, niet-hoofdlettergevoelige tekens kunnen worden gebruikt.

2.  Bijschrift: Dit vereist kenmerk is de weergavenaam van elke optie.

3.  Hint: Dit is een optioneel kenmerk. Dit kenmerk gebruiken voor meer informatie en tips voor de eindgebruiker.

### <a name="environment-variables"></a>Omgevingsvariabelen

De omgevingsvariabelen in de volgende tabel kunnen worden gebruikt in een willekeurige RCDC-configuratie.

| variabele | Beschrijving |
|--------|--------|
| `<LoginID>`       | Geeft de ID van de gebruiker die momenteel is aangemeld.           |
| `<LoginDomain>`   | Bevat het domein van de gebruiker die momenteel is aangemeld.       |
| `<Today>  `       | Geeft de huidige datum en tijd                                |
| `<FromToday_nnn>` | Geeft de huidige datum plus nnn en tijd. nnn is een geheel getal.  |
| `<ObjectID> `     | De RCDC primaire resource-ID.                                     |
| `<Attribute_xxx>` | Retourneert een opgegeven kenmerk xxx van de primaire RCDC-resource. |

### <a name="debugging-xml-configuration-files"></a>XML-configuratiebestanden foutopsporing


Als u ontwikkelt of configuratie-XML-bestanden voor een RCDC wijzigt, kunt u fouten reduceren door het valideren van de XML op basis van XSD-bestanden met een editor zoals Microsoft Visual Studio®. Zie voor meer informatie [een inleiding tot de XML-hulpprogramma's in Visual Studio 2005](http://go.microsoft.com/fwlink/?LinkID=74512).

### <a name="customizing-a-help-file"></a>Een Help-bestand aanpassen

Als u nieuwe resources en kenmerken maakt, kunt u de bestaande Help-bestanden in de FIM-Portal bijwerken met inhoud voor uw aangepaste resources. Help-bestanden in de FIM-Portal in .htm-indeling zijn en kunnen handmatig worden bewerkt.

>[!IMPORTANT]
Zie voor meer informatie over het maken van aangepaste kenmerken Inleiding tot de aangepaste Resource en het beheer van kenmerk in de documentatie van FIM 2010.

>[!IMPORTANT]
Deze sectie biedt geen informatie over de grondbeginselen van opmaak of HTML bewerken. Als u wilt wijzigen van de Help-bestanden moet al u bekend bent met het HTML bewerken


**Locatie van de Help-bestanden**: de Help-bestanden voor de Microsoft Identity Management 2016 SP1 Portal bevinden zich in de volgende map op de MIM-server:

  `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html`

**Hoe u de juiste Help-bestand vindt**: de Help-bestanden voor de FIM-Portal zijn met de naam met een globally unique identifier (GUID). Het juiste bestand voor uw aangepaste resource zoeken:

1.  Open het Help-bestand op de Portal-pagina die u wilt aanpassen in de FIM-Portal.

2.  Met de rechtermuisknop op het Help-bestand en klik vervolgens op **eigenschappen**.

3.  Selecteer en kopieer de `<GUID\>.htm` bestand in het veld URL-adres.

4.  Navigeer naar de map waar de Help-bestanden worden opgeslagen en zoek naar het bestand.

**Inhoud voor een nieuw kenmerk toe te voegen**: beschrijvende inhoud voor een nieuw kenmerk in een groepering-element (door tabs) toe te voegen:

1.  Het identificeren en het juiste Help-bestand gevonden.

2.  Open het bestand met een HTML-editor.

3.  Ga naar waar u de inhoud toevoegen. Dit is meestal een aanvullende lid, bijvoorbeeld:

`<p xmlns="">A new paragraph with customized information.</p>`

Mogelijk ook een item dat is ingevoegd in een bestaande lijst, bijvoorbeeld:
```
<li class="unordered"><b>First Name</b> – The first name of the User.<br>

<li class="unordered"><b>Last Name</b> - The last name of the User.<br>

<li class="unordered"><b>Added a new line</b><br>
```

**Inhoud voor een nieuwe groepering-element toe te voegen**: het merendeel van de FIM-Portal-pagina's hebben meerdere groepering elementen (of tabbladen) en de bijbehorende Help bestanden hebt bladwijzers secties die betrekking hebben op elk element in de groepering. Bladwijzers in de HTML-code zijn opgegeven in de secties. Dit is bijvoorbeeld de HTML-code voor het tabblad Contactgegevens werk van het Help-bestand voor de pagina gebruiker maken in de FIM-Portal:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

Hiernaar wordt verwezen door het element groepering **WorkInfo** in het gegevens-configuratie-XML-bestand voor de **configuratie voor aanmaak van de gebruiker** RCDC. Houd er rekening mee dat de `\<GUID\>.htm` bestandsnaam en de bladwijzer zijn opgegeven in de mijn: koppeling parameter:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

**Eenvoudig beheer-voorbeelden**

De volgende afbeelding toont een voorbeeld van andere eenvoudige tekstvak besturingselementen in verschillende modi.

Voorbeeld:

![](media/rcd-configuration-xml-reference/image016.gif)

Het volgende codesegment maakt het eerste besturingselement in het tekstvak, dat gebruikmaakt van expliciete tekst voor alle eigenschappen en kenmerken:

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->

```


Het volgende codesegment maakt het tweede besturingselement in het tekstvak, die de dynamische binding techniek gebruikt om te koppelen van het besturingselement met een andere gegevensbron:

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```


Het volgende codesegment maakt de derde uitgevouwen label en in het tekstvak controle:

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

Het volgende codesegment maakt het vierde uitgeschakelde tekstvak-besturingselement.
Hoewel dit besturingselement zichtbaar verschil tussen de status uitgeschakeld en de ingeschakelde status niet wordt weergegeven, kan de gebruiker niet meer gegevens invoeren in het tekstvak.

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```
## <a name="individual-controls"></a>Afzonderlijke besturingselementen

### <a name="uocbutton"></a>UocButton

**Naam**: UocButton

**Beschrijving**: dit is een eenvoudige knopbesturingselement die u gebruiken kunt voor het activeren van bepaalde acties. Het gebruik van dit besturingselement is echter beperkt omdat u uw eigen handler kunt opgeven.

**Eigenschappen**:

1.  **Algemene eigenschappen voor alle**: Zie voor informatie over deze eigenschap het algemene gedeelte van de eigenschappen van dit document.

2.  **Tekst**: deze eigenschap geeft u de tekst die wordt weergegeven op de knop. Dit is een optioneel, type string-kenmerk. De tekst neemt een expliciete tekenreekswaarde.

Gebeurtenis:

   • **OnButtonClicked**: de gebeurtenis wordt verzonden wanneer op de knop wordt geklikt.

Voorbeeld:

![](media/rcd-configuration-xml-reference/image017.png)


Het volgende XML-segment, wordt de knop eenvoudig:

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```

### <a name="uoccaptioncontrol"></a>UocCaptionControl

**Naam**: UocCaptionControl

**Beschrijving**: dit besturingselement wordt gebruikt voor het bijschrift van een pagina RCDC. Dit besturingselement is ontworpen om alleen worden gebruikt als een enkel besturingselement in een groepering header.
Gebruik deze in een andere context kan leiden tot problemen rendering of portal fouten.

**Modus**: lezen alleen (OneWay)

**Eigenschappen:**

1.  **Algemene eigenschappen voor alle:** voor meer informatie raadpleegt u het algemene gedeelte van de eigenschappen van dit document.

2.  **MaxHeight:** deze eigenschap geeft u de maximale hoogte van het pictogram in de bijschriftsectie. Deze eigenschap is optioneel. Deze eigenschap wordt een integer-waarde in pixels. De standaardwaarde is 32 pixels.

Voorbeeld:

![](media/rcd-configuration-xml-reference/image020.jpg)

Het volgende codesegment genereert de **Header bijschrift**:
```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->

```


Deze codesegment genereert de **expliciete inhoud bijschrift:**

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

Het volgende codesegment genereert de **weergavenaam** dynamische bijschrift:
```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```

### <a name="uoccheckbox"></a>UocCheckBox

**Naam**: UocCheckBox

Beschrijving: Dit is een eenvoudige selectievakje-besturingselement. We raden aan dat de gebruiker dit besturingselement met een Boolean-type gegevens binden. Dit besturingselement kan worden gebruikt als een alleen-lezen of een bij te werken besturingselement, op basis van de gegevens waaraan deze is gebonden.

>[!NOTE]
In deze release wanneer met behulp van het selectievakje beheren in de bewerkingsmodus om een Boole-kenmerk weer te geven als het kenmerk niet heeft een waarde die eerder zijn toegewezen, het bronbesturingselement voegt een waarde van **false** met het kenmerk wanneer **OK** wordt geklikt in de bewerkingsmodus. Het werk is ongeveer maken altijd een Boole-kenmerk dat wordt ervan uitgegaan dat die niet bestaan, is hetzelfde als **false**, of gebruik een andere besturingselementen zoals een keuzerondje voor Booleaanse kenmerken.

**Eigenschappen**:

1.  **Algemene eigenschappen voor alle**: Zie de sectie met algemene eigenschappen van dit document voor meer informatie.

2.  **DefaultValue**: dit is een optionele, Boolean-type-eigenschap. De standaardwaarde is ingesteld op false. Dit veld wordt het standaardgedrag van een selectievakje.
    Dit kan expliciet worden opgegeven.

3.  **Dit selectievakje inschakelt**: dit is een optionele, Boolean-type-eigenschap. De standaardwaarde is ingesteld op false. Deze waarde wordt overschreven door de eigenschap DefaultValue wanneer dit samen met DefaultValue aanwezig is. Dit veld wordt het gedrag van een selectievakje. Zoals DefaultValue, kan dit worden expliciet opgegeven of gebonden met gegevens van de server.

4.  **Tekst**: dit is een optioneel, type string-kenmerk. De tekst wordt weergegeven aan de rechterkant van het selectievakje in. U kunt deze eigenschap Geef tekst op die voor de eindgebruiker vindt u meer informatie.

**Gebeurtenissen**:

   • CheckedChanged: als het selectievakje de status verandert, deze gebeurtenis wordt verzonden.

In het volgende voorbeeld wordt een aangepaste binding wordt gemaakt tussen het type aangepaste resource en het kenmerk **IsConfigurationType**. Het XML-bestand wordt gebruikt in de Weergaveconfiguratie van een aangepast brontype.

Voorbeeld:

![](media/rcd-configuration-xml-reference/image022.png)

De volgende code segment produceert een **dynamische selectievakje**, zoals wordt weergegeven als dynamische selectievakje in de vorige afbeelding. Dit type binding wordt doorgaans veelzijdige en meer dan een expliciete selectievakje nuttig. Het kenmerk moet behoren tot het huidige brontype.

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**Beschrijving**: dit is een besturingselement van tekstvak met meerdere regels die ondersteuning biedt voor speciale tekenreeks opmaak. Elke waarde tussen de vermeldingen met meerdere waarden wordt van elkaar gescheiden door een puntkomma. of een regeleinde in het tekstvak. We raden aan dat dit besturingselement met gegevens van meerdere waarden, korte tekenreeks en geheel getal typen worden gebonden. Dit besturingselement ondersteunt alleen-lezen-modus en modus worden bijgewerkt.

**Eigenschappen**:

1.  **Algemene eigenschappen voor alle**: Zie de sectie met algemene eigenschappen van dit document voor meer informatie.

2.  **Gegevenstype**: dit is een vereist, type string-kenmerk. U kunt opgeven als een **tekenreeks, geheel getal**, of **DateTime** expliciet type. U kunt ook een kenmerk met het schemakenmerk binden **DataType** eigenschap. Een verwijzingstype met meerdere waarden moet worden verwerkt door **UOCListView** of **UOCIdentityPicker**. Met meerdere waarden Booleaanse waarde is niet een ondersteund gegevenstype.

3.  **Rijen**: dit is een optioneel, van het type geheel getal-kenmerk. U kunt de hoogte van het vak definiëren in aantal tekens. De standaardwaarde is ingesteld op 1.

4.  **Kolommen**: dit is een optioneel, van het type geheel getal-kenmerk. U kunt opgeven hoeveel breed is het aantal tekens. De standaardwaarde is ingesteld op
    20.

5.  **Waarde**: dit is een optioneel, type string-kenmerk. U kunt dit kenmerk met de gegevensbron binden.

Gebeurtenissen:

   • **ValueListChanged**: deze gebeurtenis wordt geactiveerd wanneer de huidige waarde in het besturingselement wordt gewijzigd.

In het volgende voorbeeld wordt een tekenreekskenmerk met meerdere waarden met de naam **AMultiValueString** wordt gemaakt en gekoppeld aan het aangepaste brontype. In dit voorbeeld werkt alleen als u deze binding wordt gemaakt.

**Voorbeeld:**

![](media/rcd-configuration-xml-reference/image024.jpg)

Het volgende codesegment wordt gegenereerd in de vorige afbeelding:

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```



### <a name="uocdatetimecontrol"></a>UocDateTimeControl


**Naam**: UocDateTimeControl

**Beschrijving**: dit is vergelijkbaar met een besturingselement in het tekstvak, maar **beschrijving** accepteert alleen een bepaalde opmaak. Deze weergegeven in de modus alleen-lezen, zoals een label. Zie voor de indeling van de invoerreeks die wordt ondersteund, de **DateTimeFormat** eigenschap in deze sectie.

**Eigenschappen**:

1.  **Algemene eigenschappen voor alle**: Zie de sectie met algemene eigenschappen van dit document voor meer informatie.

2.  **DateTimeFormat**: dit is een optioneel, type string-kenmerk. De ondersteunde indelingen zijn DateTime of DateOnly. De standaardwaarde is ingesteld op de datum/tijd-indeling.
    a. Datum-/ tijdindeling: dit kenmerk is geformatteerd als mm/dd/jjjj uu: mm: ss AM.

      <[!NOTE]
      Beide **DateTime** en **DateOnly** indeling worden ondersteund, ongeacht de gebruiker die het verschil is opgeven.
3.  **Waarde**: dit is een optioneel, type string-kenmerk. U koppelt dit kenmerk aan een resource-gegevensbron. De waarde van dit kenmerk is om te voldoen aan de juiste datum / tijdindeling.

Gebeurtenissen:

   • **DateTimeChanged**: wanneer de datum/tijd-waarde wijzigingen, de gebeurtenis plaatsvindt.

Voorbeeld:

![](media/rcd-configuration-xml-reference/image027.jpg)

Het volgende codesegment produceert de eerste **DateTime** besturingselement.

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```


Het volgende codesegment produceert de tweede **DateTime** besturingselement. Als u de voorbeeldcode hebt gebruikt in het gedeelte met gegevens, de **ExpirationTime** kenmerk tot alle brontypen is gebonden. Daarom kunt u met de volgende code:

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```



### <a name="uocdropdownlist"></a>UocDropDownList

**Naam**: UocDropDownList

Beschrijving: Dit is een eenvoudige vervolgkeuzelijst-besturingselement. Dit besturingselement wordt meestal gebruikt wanneer u opties wilt selecteren uit een gedefinieerde set opties. Gegevenstypen van de tekenreeks, geheel getal, datum/tijd en Booleaanse waarde zijn goede kandidaten voor dit besturingselement.

**Eigenschappen**:

1.  **Algemene eigenschappen voor alle**: Zie de sectie met algemene eigenschappen van dit document voor meer informatie.

2.  **ValuePath**: de eigenschap ophalen van het kenmerk Value van itemsource instelt. Wanneer ItemSource als aangepast wordt opgegeven, wordt het pad van de waarde is ingesteld op waarde. Deze verbinding heeft met het veld waarde uit de optie die verderop in dit document is gedefinieerd.

3.  **CaptionPath**: de eigenschap ophalen van het kenmerk Value van itemsource instelt. Wanneer ItemSource is opgegeven als aangepast, wordt het pad van de waarde ingesteld op bijschrift. Deze verbinding heeft met het veld Bijschrift van de optie-element dat verderop in dit document is gedefinieerd.

4.  **HintPath**: de eigenschap ophalen van het kenmerk Value van itemsource instelt. Wanneer ItemSource is opgegeven als aangepast, wordt het pad van de waarde ingesteld op Hint. Deze verbinding heeft met het veld Hint uit de optie die verderop in dit document is gedefinieerd.

5.  **ItemSource**: een verzameling van ListControlItems die definieert de opties in de lijst. De gebruiker kan expliciet dit is ingesteld op aangepast en het element optie gebruiken om op te geven van de tekenreekswaarde.

6.  **SelectedValue**: de waarde die momenteel is geselecteerd. Dit is een vereiste, type string-eigenschap. Deze eigenschap is gebonden aan tekenreeksgegevens uit de gegevensbron.

Gebeurtenissen:

  • SelectedIndexChanged: de gebeurtenis treedt op wanneer de selectie in de vervolgkeuzelijst wordt gewijzigd.

Opties:

Zie de sectie 'Optie' in dit document voor de structuur van een Option-element.

1.  **Waarde**: de waarde van één optie element kan worden ingesteld op een tekenreeks die is de geldige invoer van de gegevensbron waaraan het besturingselement is gebonden.

2.  **Bijschrift**: bijschrift kan een willekeurige tekenreekswaarde zijn.

3.  **Hint**: Hint kan een willekeurige tekenreekswaarde zijn.

Voorbeeld:

![](media/rcd-configuration-xml-reference/image030.jpg)


![](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
Als u het voorbeeld werken, moet u een bestaand, type string-kenmerk binden **bereik** met het type aangepaste resource die de RCDC is van toepassing op.


Deze codesegment genereert de vervolgkeuzelijst:

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

Naam: UocFileDownload

Beschrijving: Dit besturingselement bevat een hyperlink. Wanneer u de hyperlink klikt, verschijnt een pagina Windows-bestand opslaan. De gebruiker kan het bestand opslaan op hun lokale schijf.
De Open option wordt ook ondersteund als de bestandsindeling van Internet Explorer kunt weergegeven. De aanbevolen gegevenstypen gebruiken dit besturingselement met zijn geformatteerd tekenreeks (XML) en binaire typen.

>[!NOTE]
In deze release van Microsoft Identity Manager 2016 SP1, moet de gebruiker de Internet Explorer-venster waarin hij of zij het bestand hebt geopend en vernieuw de pagina sluiten. De gebruiker na het vernieuwen van het Internet Explorer-venster kunt vervolgens het downloaden als u wilt opslaan of hetzelfde bestand opnieuw te openen in het venster van de oorspronkelijke starten.


Eigenschappen:

1.  **Algemene eigenschappen voor alle**: Zie de sectie met algemene eigenschappen van dit document voor meer informatie.

2.  **Tekst**: dit is een optioneel, type string-kenmerk die definieert de tekst van de hyperlink. De gebruiker kan een expliciete tekenreeks opgeven voor deze eigenschap.

3.  **Waarde**: dit is een vereist kenmerk. Deze eigenschap de kenmerk-binding op de server waarvan de inhoud worden gedownload.

4.  **PromptedFileName**: dit is een optioneel, type string-kenmerk. Dit is de bestandsnaam die wordt voorgesteld voor de gebruiker wanneer ze het gedownloade bestand opslaan.

5.  **ContentType**: dit is een vereist, type string-kenmerk. Dit is het bestandstype dat de gegevens worden opgeslagen in. Tekst of binaire zijn twee Tekenreeksopties voor de ondersteunde. Als deze tekst is, wordt de retourwaarde wordt beschouwd als een lange tekenreeks.
    De retourwaarde wordt anders voor binair beschouwd als byte []. Als tekst is ingeschakeld, kunt, de gebruiker als een optie toevoegen wordt een achtervoegsel om op te geven van het type van de tekst-indeling. Bijvoorbeeld, is text/xml ongeldig.


>[!NOTE]
Wanneer de waarde die is gekoppeld aan dit besturingselement leeg is, ontbreekt in het besturingselement de hyperlink moet worden gebruikt voor het activeren van de downloadactie. Dit komt doordat er is niets om te downloaden.


**Gebeurtenissen**:

Er zijn geen gebeurtenissen voor dit besturingselement.

**Voorbeeld**:

![](media/rcd-configuration-xml-reference/image035.png)


>[!NOTE]
Voordat u dit voorbeeldbestand uploadt, moet de gebruiker een binding tussen een aangepast brontype en de bestaande ConfigurationData-kenmerk maken.


Het volgende codesegment genereert het besturingselement voor het downloaden van bestanden in de vorige afbeelding:

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**Naam**: UocFileUpload

**Beschrijving**: dit besturingselement bevat een tekstvak dat geeft de locatie van het lokale bestand te uploaden, een knop van het bestand bladeren en een knop uploaden. Wanneer de eindgebruiker op een bladerknop klikt, verschijnt een venster geopend bestand van Windows. De eindgebruiker kan één bestand op hun lokale schijf uploaden selecteren. Wanneer het bestand is geselecteerd, wordt de locatie van het bestand weergegeven in het tekstvak. Wanneer de knop uploaden wordt geklikt, wordt het bestand geüpload naar de client-side lokale gegevensbron. Inhoud van het bestand nog niet is verzonden naar de server. De aanbevolen gegevenstypen gebruiken dit besturingselement met zijn als volgt: indeling van tekenreeks (XML) of binaire typen.

>[!NOTE]
Er is geen aanwijzing van de uploadvoortgang of status. Wanneer het bestand is geüpload naar de lokale gegevensbron, wordt in het tekstvak gewist.


Eigenschappen:

1.  Algemene eigenschappen voor alle: Zie het algemene gedeelte van de eigenschappen van dit document voor informatie.

2.  Waarde: Dit is een vereist kenmerk. Deze geeft de schema-kenmerk-binding op de server waarop de gegevens zijn geüpload.

3.  ContentType: Dit is een optioneel, type string-kenmerk. Dit is het gegevenstype dat het bestand is opgeslagen in op de server. Dit kan worden ingesteld op tekst of binaire. Als de eigenschap ontbreekt, is de standaardwaarde Binary.

4.  MaxFileSize: Dit is een optioneel, type string-kenmerk. MaxFileSize definieert hoe groot de grootte van het geüploade bestand kan worden. Als de eigenschap ontbreekt, is de maximale grootte standaard 1 megabyte (MB).

5.  PromptedForNoValue: Dit is een optioneel, type string-kenmerk. Hiermee definieert u de tekst die wordt weergegeven voor de gebruiker wanneer een bestand wordt niet worden geüpload.

Gebeurtenissen:

   • FileUploaded: deze gebeurtenis wordt verzonden wanneer het bestand is geüpload.

Voorbeeld:

![](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
Als u de volgende voorbeeldcode werken, moet u een nieuw binair type kenmerk met de naam ABinaryAttribute maken en maakt u een nieuwe binding tussen een aangepast brontype en dit kenmerk.


Het volgende codesegment genereert besturingselement voor het uploaden in de vorige afbeelding:
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```

### <a name="uocfilterbuilder"></a>UocFilterBuilder

Naam: UocFilterBuilder

Beschrijving: Dit is een complex besturingselement waarmee de gebruiker het weergeven van een MIM 2016 XPath-expressie. Sommige XPath-expressies worden niet ondersteund. Voor informatie over het gebruik van de opbouwfunctie voor het filter, Zie de Help voor filter builder.

Eigenschappen:

1.  Algemene eigenschappen voor alle: Zie het algemene gedeelte van de eigenschappen van dit document voor meer informatie.

2.  PermittedObjectTypes: Hiermee definieert u een lijst met brontypen die moet worden weergegeven in de select-instructie van een filter builder. Zie voor informatie over het gebruik van filter builder filter builder Help. De tekenreeks is in de indeling van ResourceTypeA, ResourceTypeB, waarin elk resourcetype worden gescheiden door een komma (,).

3.  Waarde: Dit is de waarde waarmee de opbouwfunctie voor het filter wordt weergegeven.
    Alleen een binding met het type string-gegevens met een XPath-expressie wordt ondersteund. Het kenmerk Filter is een aanbevolen kenmerk voor het binden van dit besturingselement.

4.  PreviewButtonVisible: Dit is een optionele, Boolean-type-eigenschap. Wanneer deze eigenschap is ingesteld op false, ziet de gebruiker de knop voorbeeld. De standaardwaarde is ingesteld op true. Deze knop kan worden gebruikt in combinatie met een besturingselement voor lijstweergave voorbeeld van de resultaten van een XPath-expressie.

5.  ExcludeGroupMembership: Dit is een Boole-eigenschap. Als deze eigenschap is ingesteld op true, u kunt geen maken een filter met \<verwijzingskenmerk\> (bijvoorbeeld ResourceID) is lid van \<groepsobject\>. Met andere woorden, als deze eigenschap is ingesteld op true, u kunt geen maken een filter met de map voor het lidmaatschap van groep.

6.  PreviewButtonCaption: Dit is een optionele tekenreeks. Wanneer PreviewButtonVisible is ingesteld op true, kunt u deze eigenschap geeft de knop een aangepaste tekst. De tekst wordt weergegeven op de knop voorbeeld.

Gebeurtenissen:

   • OnFilterChanged: deze gebeurtenis wordt geactiveerd wanneer de filter builder inhoud wordt gewijzigd.

Voorbeeld:

![](media/rcd-configuration-xml-reference/image044.png)

>[!NOTE]
Voordat u deze voorbeeldcode gebruiken, maakt u een nieuwe binding tussen een bestaand filterkenmerk en een aangepast brontype.


Hieronder volgt de voorbeeldcode met een besturingselement UOCLabel, een eenvoudig filter builder met PermittedObjectTypes en een lijstweergave in Preview. U kunt de lijstweergave ListFilter eigenschap en filter builder eigenschap Value moet verwijzen naar het hetzelfde kenmerk van de gegevensbron te koppelen van de twee.

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


<a name="uochtmlsummary"></a>UocHtmlSummary
--------------

Naam: UocHtmlSummary

Beschrijving: U kunt dit besturingselement gebruiken voor het definiëren van een overzichtspagina in een RCDC-pagina.
Deze overzichtspagina wordt weergegeven nadat de gebruiker een aanvraag indient. Dit besturingselement kan alleen worden gebruikt in een groepering van de samenvatting en moet het enige besturingselement. Sterk aanbevolen dat u de voorbeeldcode die is opgegeven.

>[!NOTE]
Dit besturingselement is niet uitgebreid getest.


Eigenschappen:

1.  Algemene eigenschappen voor alle: Zie het algemene gedeelte van de eigenschappen van dit document voor informatie over deze eigenschap.

2.  ModificationsXml: Deze eigenschap moet worden opgemaakt als {Binding bron = delta, pad = DeltaXml}, waarbij delta is gedefinieerd in de configuratie-header ObjectDataSource.

3.  TransformXsl: Deze eigenschap wordt gewoonlijk ingedeeld als {Binding bron = summaryTransformXsl, pad = /}, waarbij summaryTransformXsl is gedefinieerd in de configuratie-header XmlDataSource.

Zie 'Samenvatting groepering' in de sectie groepering eerder in dit document voor een bestaande voorbeeld van dit besturingselement.

### <a name="uochyperlink"></a>UocHyperLink

Naam: UocHyperLink

Beschrijving: Dit is een eenvoudig hyperlinkbesturingselement. Informatie weergeven als een hyperlink kunt u dit besturingselement.

Eigenschappen:

1.  Algemene eigenschappen voor alle: Zie het algemene gedeelte van de eigenschappen van dit document voor informatie.

2.  ObjectReference: Dit is een optionele, reference-type-eigenschap. Als een geldige resource wordt verwezen door de GUID die in deze eigenschap is gedefinieerd, biedt de hyperlink de eindgebruiker een manier toegang tot de bron. Dit is een wederzijds uitsluitend met de eigenschap NavigateUrl (hieronder).

3.  Tekst: Dit is een optionele, type string-eigenschap. Deze eigenschap kunt u de tekst die wordt weergegeven als hyperlink definiëren.

4.  NavigateUrl: Dit is een optionele, type string-eigenschap. Deze eigenschap kunt u het volledige pad-URL die is gekoppeld aan de hyperlink definiëren. Dit is een wederzijds uitsluitend met de eigenschap ObjectReference (hierboven).

Voorbeeld:

![](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
U moet een geldige GUID van een resource met deze koppeling. In dit geval wordt de tweede hyperlink gegenereerd met een geldige GUID. Het eerste beheerpunt kan een website zijn.


Het volgende codesegment genereert een omleiding hyperlink:

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->

```

Het volgende codesegment genereert een hyperlink naar een resource. De expliciete verwijzing kan worden vervangen door de expressie {Binding bron object, pad = = Maker} dit met een gegevensbron binden. Dit kan zijn geldige alleen wanneer de resource manager bestaat en het type verwijzing waarde.

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```

### <a name="uocidentitypicker"></a>UocIdentityPicker

Naam: UocIdentityPicker

Beschrijving: Dit besturingselement bestaat uit een optionele oplossen en een bladervenster. Het optionele los vak bestaat uit een optionele tekstvak de identiteit, een knop oplossen omzetten van de identiteit en een bladerknop om een pop-upvenster voor bladeren in te voeren. Het bladervenster maakt het mogelijk dat de gebruiker te selecteren van identiteiten via een lijst met weergave-besturingselement. De geselecteerde identiteit van het bladervenster wordt weergegeven in het vak oplossen.

Eigenschappen:

1.  Algemene eigenschappen voor alle: Zie het algemene gedeelte van de eigenschappen van dit document voor informatie over deze eigenschap.

2.  UsageKeywords: Dit is een optionele tekenreekseigenschap. U kunt een lijst met zoekbereiken moet worden gebruikt in de Resource-kiezer door te geven een lijst met trefwoorden gebruik die worden ondersteund door de structuur SearchScopeConfiguration waarin elk trefwoord worden gescheiden door een (') definiëren.

3.  Filter: Dit is een optionele tekenreekseigenschap. De gebruiker biedt een XPath-expressie voor het bereik van de bron kiezen om alleen de items die binnen een gedefinieerde bereik past weer te geven. Deze eigenschap is wederzijds uitsluitend met de eigenschap UsageKeywords (hierboven). Wanneer het zoekbereik wordt toegepast, wordt deze eigenschap heeft geen effect.

4.  ResultObjectType: Dit is een optionele tekenreekseigenschap. Het brontype wordt gebruikt om resources in de lijst pop-dialoogvenster weer te geven. Dit is met het Filter gebruikt om u te helpen bij het kiezen van de identiteit bepalen welk resourcetype wordt geretourneerd door het Filter en dienovereenkomstig renderen van de gegevens. Deze eigenschap is wederzijds uitsluitend met de eigenschap UsageKeywords (Zie hierboven). Wanneer het zoekbereik wordt toegepast, heeft dit geen effect. De tekenreeks die wordt geaccepteerd voor deze eigenschap is een afzonderlijke, geldige, resourcetype-naam, bijvoorbeeld persoon.
    Wanneer u het filter om terug te keren meerdere brontypen wordt verwacht, wordt bron gebruikt.

5.  PreviewTitle: Dit is de preview-titel gebruikt op een lijstweergave. Zie de sectie UocListView voor informatie over deze eigenschap.

6.  ListViewTitle: Dit is een optionele tekenreekseigenschap. U kunt deze eigenschap gebruiken voor het definiëren van de tekst die wordt weergegeven boven de lijstweergave als een titel op.

7.  Waarde: Dit is een optionele tekenreekseigenschap. Het is raadzaam om dit te binden aan een schemakenmerk de waarde een verbinding maakt met een gegevensbron.

8.  Modus: Dit is een optionele tekenreekseigenschap. Deze eigenschap kunt u definiëren of één waarde kan worden ingeschakeld door de objectkiezer identiteit of meerdere identiteiten kunnen worden geselecteerd. SingleResult en MultipleResult zijn dan de toegestane waarden. Standaard wordt deze ingesteld op SingleResult.

9.  ObjectTypes: Dit is een optionele, type string-eigenschap. Een lijst met brontypen die de eindgebruiker vermeldingen tegen in het vak identiteit objectkiezer omzetten kunt oplossen, kunt u definiëren. De lijst bestaat uit een lijst met type resource namen gescheiden door een komma (,).

10. AttributesToSearch: Dit is een optioneel, string-typeproperty. U kunt een lijst met kenmerken die moeten worden gebruikt voor het omzetten van het item in de objectkiezer identiteit waarbij de lijst is een lijst met schema-kenmerk, gescheiden door een komma (,) definiëren. Bijvoorbeeld, als AttributesToSearch is ingesteld op DisplayName, Alias, betekent dit, kan die gebruiker om te zoeken items met DisplayName = \<waarde zoeken\> of Alias =\<waarde zoeken\>. U wordt aangeraden dat de namen van kenmerken die u hier invoert zijn geldige kenmerken op resource doeltypen van de gegevensbron die wordt geplaatst in waarde. De doel-brontypen vindt u in het veld ObjectTypes. Alle kenmerken moet geldig zijn op een opgegeven brontypen die worden beschreven in het veld ObjectTypes.

11. ColumnsToDisplay: Dit is een optionele, type string-eigenschap. De gebruiker geeft een lijst van schema kenmerknamen, gescheiden door een komma (,). De kenmerken die hier zijn gedefinieerd, vormen de kolom van de lijstweergave in de kiezer voor identiteit.

12. Rijen: Dit is optioneel, integer-eigenschap. Dit werkt alleen als de modus is ingesteld op MultipleResult. Gebruik deze eigenschap de hoogte van het tekstvak los instellen op een opgegeven grootte in tekeneenheden.

13. MainSearchScreenText: Dit is een optionele, type string-eigenschap. Dit is de aangepaste tekst die wordt weergegeven als de zoekopdracht wordt uitgevoerd in het venster Bladeren.

Gebeurtenissen:

 • SelectedObjectChanged: deze gebeurtenis wordt verzonden wanneer de gebruiker wijzigt de geselecteerde resources.

Voorbeeld:

![](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
Voor dit voorbeeld werken, moet u een nieuwe binding tussen het kenmerk Manager en eventuele aangepaste brontype dat deze XML is van toepassing op maken.


Het volgende codesegment wordt een identiteit objectkiezer in modus voor één gegenereerd met behulp van de eigenschappen Filter en ResultObjectType als onderdeel van de RCDC:

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```



Voorbeeld:

![](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
Als u deze voorbeeldcode werken, moet u het kenmerk ExplicitMember (een verwijzingskenmerk met meerdere waarden) binden aan het aangepaste brontype. U moet ook zoekbereiken maken met de eigenschap UsageKeyword is ingesteld op persoon en groep.


Het volgende codesegment maakt het besturingselement in de vorige afbeelding:

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

Naam: UocLabel

Beschrijving: Dit is een eenvoudige, alleen-lezen, tekst label-besturingselement. Het is raadzaam dat dit besturingselement worden gebruikt om alleen-lezen gegevens weer te geven.

Eigenschappen:

1.  Algemene eigenschappen voor alle: Zie het algemene gedeelte van de eigenschappen van dit document voor informatie over deze eigenschap.

2.  Tekst: Dit is een kenmerk van het type string. Deze eigenschap wordt gedefinieerd door een expliciete string-waarde op te geven of door deze binding met een gegevensbron. Een voorbeeld-binding die de walue van deze eigenschap wijst is {Binding bron = object, pad =\<geldig kenmerknaam\>.

Zie voor een voorbeeld van het besturingselement UocLabel eenvoudig besturingselement in de sectie eenvoudige control voorbeelden.

### <a name="uoclistview"></a>UocListView

Naam: UocListView

Beschrijving: Dit is een geavanceerde lijstweergave-besturingselement. Bestaat uit een eenvoudige lijstweergave, een optionele eenvoudige zoekopdrachten, een besturingselement optionele Geavanceerd zoeken, een optionele selectievakje preview en een knop actiebalk. De optionele eenvoudige zoekopdrachten bestaat uit een zoekbereik en een eenvoudige zoekvak. Het besturingselement Geavanceerd zoeken is een filter builder. De lijstweergave toont een prerendered lijst met bronnen. Ook kan afkomstig zijn van de zoekinstellingen in dit besturingselement zoekresultaten weergegeven. De balk actieknop definieert welke actie kan worden uitgevoerd op basis van de selectie in de lijstweergave. In het vak selectie voorbeeld ziet u welke items zijn geselecteerd in de lijst.

>[!IMPORTANT]
UocListView werkt niet met één waarde verwijzingskenmerken. Deze kan alleen worden gebruikt met meerdere waarden verwijzingskenmerken. Zie voor één waarde verwijzingskenmerken UocIdentityPicker in dit document.


Eigenschappen:

1.  Algemene eigenschappen voor alle: Zie het algemene gedeelte van de eigenschappen van dit document voor informatie over deze eigenschap.

2.  SelectedValue: Dit is een optionele, type string-eigenschap die meestal wordt gebonden aan een met meerdere waarden verwijzingskenmerk accepteren van een lijst met tekenreeksen GUID-indeling.

3.  PageSize: Dit is optioneel integer-eigenschap. De gebruiker kunt opgeven hoeveel vermeldingen past in meerdere pagina's in een besturingselement voor lijstweergave. De standaardwaarde is 10 vermeldingen. Een positief geheel getal is ongeldig.

4.  UsageKeyword: Dit is een optionele, type string-eigenschap. De gebruiker kan een lijst met trefwoorden die bepalen welke zoekbereik wordt gebruikt in de lijst weergave zoekbesturing opgeven. Er zijn bereik zoeken in resources in FIM 2010-server. Het kenmerk van een structuur SearchScopeConfiguration, aangeroepen UsageKeyword, wordt gebruikt voor een aantal zoekbereiken groeperen. De lijstweergave verbruikt die lijst met trefwoorden. Elk trefwoord wordt gescheiden door een komma (,).
    Dit de usagekeyword gebruikt op het bijbehorende zoekbereik die u wilt weergeven in de lijstweergave van deze. Dit is alleen van kracht wanneer de eigenschap ShowSearchControl is ingesteld op true.

5.  SearchControlAutoPostback: Dit is een optionele Boole-eigenschap. De waarde van deze eigenschap ingesteld op true om uit te voeren autopostback wanneer een zoekopdracht wordt geactiveerd. SearchControlAutoPostback is standaard ingesteld op false.

6.  EmptyResultText: Dit is een optionele, type string-eigenschap. Standaard is ingesteld op geen items, maar deze kan worden ingesteld op een willekeurige tekenreekswaarde. Deze tekst wordt weergegeven wanneer een zoekresultaat leeg is.

7.  ButtonHeight: Dit is een optionele, geheel getal-type-eigenschap. De waarde van deze eigenschap ingesteld op een positief geheel getal. Deze eigenschap bepaalt de hoogte van de knoppen in de actiebalk in pixels. De standaardwaarde is 32 pixels.

8.  ButtonWidth: Dit is een optionele, geheel getal-type-eigenschap. De waarde van deze eigenschap ingesteld op een positief geheel getal. Deze eigenschap bepaalt de breedte van de knoppen in de actiebalk in pixels. De standaardwaarde is 32 pixels.

9.  CaptionImageMaxHeight: Dit is een optionele, geheel getal-type-eigenschap. De waarde van deze eigenschap ingesteld op een positief geheel getal. Deze eigenschap definieert een optionele bijschrift maximale pictogram hoogte. De standaardwaarde is 32 pixels.

10. CaptionImageMaxWidth: Dit is een optionele, geheel getal-type-eigenschap. De waarde van deze eigenschap ingesteld op een positief geheel getal. Deze eigenschap definieert een optionele bijschrift pictogram maximale breedte. De standaardwaarde is 32 pixels.

11. CaptionImageUrl: Dit is een optionele, type string-eigenschap. Deze eigenschap definieert een URL die verwijst naar een afbeelding die wordt weergegeven als de installatiekopie van het bijschrift.

12. PreviewTitle: Dit is een optionele, type string-eigenschap. Deze eigenschap kunt u definiëren van de tekst die boven op het preview selecteren verschijnt.

13. EnableSelection: Dit is een optionele, Boolean-type-eigenschap. Deze eigenschap kunt u aangeven of een lijstweergave in modus voor selectie. Als een lijstweergave in de selectiemodus is, wordt een kolom van selectievakjes weergegeven in de linkerkolom van de lijstweergave en het preview selectie wordt weergegeven aan de onderkant van de lijstweergave. De standaardwaarde van deze eigenschap is ingesteld op true.

14. SingleSelection: Dit is een optionele, Boolean-type-eigenschap. Als de selectiemodus is ingeschakeld voor de lijstweergave, als deze waarde instelt op true wordt beperkt door de gebruiker slechts één item in de lijst te selecteren. De waarde van deze eigenschap is standaard ingesteld op false. Dit betekent dat standaard de eindgebruiker kan meerdere items selecteren in de lijst.

15. URL omleiding: Dit is een optionele, type string-eigenschap. Gebruik deze eigenschap om de pagina een waarnaar wordt doorverwezen wanneer een hyperlink item in de lijst wordt geklikt. Deze URL kan de tijdelijke aanduidingen die tijdens de runtime worden vervangen door de werkelijke waarde bevatten. De tijdelijke aanduidingen zijn als volgt:

     ◦ {0} - objectType

     ◦ {1} - object-id

     ◦ {2} - weergavenaam

16.  ShowTitleBar: Dit is een optionele, Boolean-type-eigenschap. Gebruik deze eigenschap om op te geven of de titelbalk zichtbaar moet zijn. De standaardwaarde van deze eigenschap is ingesteld op false.

17.  ShowActionBar: Dit is een optionele, Boolean-type-eigenschap. Gebruik deze eigenschap om op te geven of het gebied van de actie weergegeven worden moet. De standaardwaarde van deze eigenschap is ingesteld op true.

18.  ShowPreview: Dit is een optionele, Boolean-type-eigenschap. Gebruik deze eigenschap om op te geven of het voorbeeldgebied zichtbaar moet zijn. De standaardwaarde van deze eigenschap is ingesteld op true.

19.  ShowSearchControl: Dit is een optionele, Boolean-type-eigenschap. Gebruik deze eigenschap om op te geven of de zoekbesturing weergegeven worden moet. De standaardwaarde van deze eigenschap is ingesteld op true.

20.  ResultObjectType: Dit is een optionele, type string-eigenschap. Gebruik deze eigenschap het type object wordt verwacht van de zoekresultaten te geven. De standaardwaarde van deze eigenschap is een Resource. Als het zoekresultaat meerdere brontypen bevat, moet deze waarde worden opgegeven als bron.

21.  ColumnsToDisplay: Dit is een optionele eigenschap. Gebruik deze eigenschap om aan te geven welke kenmerken de lijstweergave kolommen wordt weergegeven. De standaardwaarde van deze eigenschap is DisplayName, ResourceType. Elke kolom wordt vertegenwoordigd door de naam van een kenmerk. Elke kolom wordt gescheiden door een komma (,). U beschikt niet over naar een waarde opgeven voor deze eigenschap wanneer de lijstweergave wordt gebruikt in de modus voor selectie. In de selectiemodus, is de kolominstelling van de afkomstig van het kenmerk SearchScopeColumn van het zoekbereik die momenteel is geselecteerd.

22.  ListFilter: Dit is een optionele, type string-eigenschap. Dit is het xpath dat wordt gebruikt voor het weergeven van de lijstweergave en is alleen van kracht wanneer ShowSearchControl-eigenschap is ingesteld op false. Wanneer deze waarde wordt opgegeven, wordt de lijstweergave waarde van deze eigenschap gebruikt voor query's en wordt de lijstweergave is niet in de selectiemodus. Het filter kan ofwel worden gebonden aan een string-kenmerk van de bron als volgt:<br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>` <br/> <br/>
     of een tekenreeks met een aantal vooraf gedefinieerde omgevingsvariabele als volgt: <br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

23.  TargetAttribute: Dit is een verouderd eigenschap. De waarde moet de systeemnaam van een kenmerk waarnaar wordt verwezen. We raden aan dat deze eigenschap niet meer worden gebruikt. Bijvoorbeeld, in het groepsbeheer, in plaats van het volgende:

  `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>` <br/>

  Het moet zijn:`<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>` <br/><br/>
24.  ItemClickBehavior: Dit is een optionele, type string-eigenschap. Gebruik deze eigenschap om aan te geven of klik op een lijstweergave-item voor het activeren van een terugpostactie server of om details weer te geven van het item. Twee waarden worden ondersteund: ModelessDialog en de Server. De standaardwaarde is ModelessDialog.

25.  SearchOnLoad: Dit is een optionele, Boolean-type-eigenschap die aangeeft of het besturingselement voor lijstweergave query moet worden uitgevoerd bij het laden. Deze eigenschap geldt alleen wanneer de lijstweergave geselecteerd is. De standaardwaarde voor deze eigenschap is ingesteld op true. U kunt deze uitschakelen als u verwacht de gebruiker meestal tekst om in te typen zoeken dat om een zinvolle resultaat wordt verkregen. In dit geval toont de lijstweergave in eerste instantie een bericht naar de gebruiker zien hoe u een zoekopdracht uitvoert. De tekst kan worden aangepast door de volgende eigenschappen:

26.  MainSearchScreenText: Dit is optioneel, type string-eigenschap is van toepassing wanneer er op SearchOnload is ingesteld op true. Deze eigenschap kan worden gebruikt voor het aanpassen van de tekst die wordt weergegeven in het midden van de lijstweergave wanneer de lijstweergave niet automatisch te zoeken. De standaardwaarde voor deze eigenschap is zoeken naar de resources die u wilt met behulp van de bovenstaande zoekfunctie. U kunt een waarde als u de tekst meer relevant zijn voor uw scenario opgeven.

27.  SubSearchScreenText: Dit is optioneel, type string-eigenschap gebruikt voor het aanpassen van de tekst die wordt weergegeven onder MainSearchScreenText. U hebt normaal gesproken niet naar een waarde op voor deze eigenschap opgeeft, tenzij u wilt toevoegen van een aantal aanvullende instructies over het gebruik van de lijstweergave.

Zie voor voorbeelden van het lijstweergave samen met het besturingselement UocFilterBuilder gebruiken als een lijst preview UocFilterBuilder voorbeelden eerder in dit document. De UocListView kan ook worden gebruikt zonder filter builder.

### <a name="uocnumericbox"></a>UocNumericBox

Naam: UocNumericBox

Beschrijving: Dit is een eenvoudig tekstvak waarvoor alleen gehele waarden. Dit besturingselement ondersteunt alleen-lezen-modus en modus worden bijgewerkt.

Eigenschappen:

1.  Algemene eigenschappen voor alle: Zie het algemene gedeelte van de eigenschappen van dit document voor informatie over deze eigenschap.

2.  MaxValue: Dit is een optionele, geheel getal-type-eigenschap. Gebruik deze eigenschap voor het definiëren van een client-side '-validatie voor het besturingselement. De waarde die de eindgebruiker invoert, kan niet groter zijn dan deze waarde. U kunt een expliciete geheel getal invoeren of binden dit met een geheel getal van een gegevensbron met behulp van {Binding bron = schema, pad = IntegerMaximum}.

3.  MinValue: Dit is een optionele, geheel getal-type-eigenschap. Gebruik deze eigenschap voor het definiëren van een client-side '-validatie voor het besturingselement. De waarde die de eindgebruiker invoert mag niet lager is dan deze waarde. U kunt een expliciete geheel getal invoeren of binden dit met een geheel getal van een gegevensbron met behulp van de Binding bron = schema, pad = IntegerMinimum}.

4.  Standaardwaarde: Dit is een optionele, geheel getal-type-eigenschap. Gebruik deze eigenschap voor het definiëren van een standaardwaarde voor het besturingselement als het besturingselement wordt gebruikt voor het maken van nieuwe gegevens. Deze waarde kan alleen expliciet worden ingesteld op een statische geheel getal.

5.  Waarde: Dit is een optionele, geheel getal-type-eigenschap. Wanneer u dit aan een type geheel getal van een gegevensbron koppelt, wordt de waarde van dit kenmerk weergegeven wanneer de pagina wordt geladen en vervolgens wordt opgeslagen in de gegevensbron na het indienen.

Handler:

   • TextChanged: deze gebeurtenis wordt verzonden wanneer de huidige waarde in het besturingselement wordt gewijzigd.

Voorbeeld:

![](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
De volgende voorbeeldcode wordt het eerste vak numerieke gegenereerd. Het numerieke vak is niet verbonden met een gegevensbron of een schema-informatie.

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->

```

De volgende voorbeeldcode genereert het tweede vak numerieke.

>[!NOTE]
Voor dit voorbeeld werkt, eerst moet u een nieuwe, geheel type kenmerk aangeroepen AnIntegerAttribute en bindt dit met het type aangepaste resource.

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

Naam: UocPictureBox

Beschrijving: Dit besturingselement wordt gebruikt voor het genereren van afbeelding, binair type gegevens. Het is raadzaam dat dit besturingselement met type binaire gegevens worden gebruikt. De afbeelding kan worden weergegeven door een opgegeven afbeeldings-URL, binair type gegevens of de bron van het kenmerk dat type afbeelding gegevens bevat.

Eigenschappen:

1.  Algemene eigenschappen voor alle: Zie het algemene gedeelte van de eigenschappen van dit document voor informatie over deze eigenschap.

2.  ImageUrl: Dit is een optionele, type string-eigenschap. Voer de URL van de doel-afbeelding.

3.  MaxHeight: Dit is een optionele, string-type-eigenschap. Hiermee definieert u de maximale hoogte van de afbeelding die moet worden gerenderd in pixels.

4.  MaxWidth: Dit is een optionele, type string-eigenschap. Hiermee definieert u de maximale breedte van de afbeelding die moet worden gerenderd in pixels.

5.  Afbeeldingsgegevens: Dit is een eigenschap van het binaire type. Gebruik deze eigenschap binden van een gegevensbron met de installatiekopie van het weergegeven. De gekoppelde gegevensbron is van het binaire bestand.
    U kunt dit veld ook expliciet een afbeelding instellen door te geven gegevens in byte []-indeling gebruiken.

6.  ImageResource: Dit is een optionele, binary-type-eigenschap.

7.  AlternativeText: Dit is een optionele, type string-eigenschap. Deze eigenschap wordt weergegeven als alternatieve tekst wanneer de afbeelding kan niet worden weergegeven.

Voorbeeld:

>[!NOTE]
Als u wilt dit voorbeeld gebruiken, moet u een bestaande installatiekopie gegevens bij het besturingselement binden hebben.


Het volgende codesegment genereert een invoervak afbeelding waaraan een gegevensbron met het besturingselement is gebonden:

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

Het volgende codesegment genereert een invoervak afbeelding waaraan een URL-afbeelding met het besturingselement is gebonden:

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

### <a name="uocradiobuttonlist"></a>UocRadioButtonList

Naam: UocRadioButtonList

Beschrijving: Dit is een eenvoudige, keuzerondje lijst. De opties zijn sluiten elkaar wederzijds uit in deze lijst. Dit besturingselement wordt aanbevolen wanneer gebruikers kunt kiezen uit vijf of minder opties hebben. Anders wordt UOCListView aanbevolen.

Eigenschappen:

1.  Algemene eigenschappen voor alle: Zie het algemene gedeelte van de eigenschappen van dit document voor informatie over deze eigenschap.

2.  ValuePath: Het pad van de waarde is ingesteld op waarde. Deze verbinding heeft met het veld waarde van de optie-element dat is gedefinieerd in dit document.

3.  CaptionPath: Het pad van de waarde is ingesteld op bijschrift. Deze verbinding heeft met het veld Bijschrift van de optie-element dat is gedefinieerd in dit document.

4.  HintPath: Het pad van de waarde is ingesteld op Hint. Deze verbinding heeft met het veld Hint van de optie-element dat is gedefinieerd in dit document.

5.  SelectedValue: De waarde die momenteel is geselecteerd. Dit is een vereiste, type string-eigenschap. Deze eigenschap gebonden met tekenreeksgegevens uit de gegevensbron.

Gebeurtenissen:

1.  SelectedIndexChanged

2.  CheckedChanged

Opties:

Er kan alleen worden twee Option-elementen in de opties voor dit besturingselement.

1.  Waarde: Veld met de waarde in een enkel element van de optie is ingesteld op waar of ONWAAR.

2.  Bijschrift: Dit kan een willekeurige tekenreekswaarde zijn.

3.  Hint: Dit kan een willekeurige tekenreekswaarde zijn.

Voorbeeld:

![](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
Voor dit voorbeeld werken, moet u een nieuw Booleaanse kenmerk ABooleanAttribute, maken en binding tot stand brengen met het type van uw aangepaste resource.

Het volgende codesegment maakt de lijst van de knop Opties in de vorige afbeelding:

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton


Naam: UocSimpleRadioButton

Beschrijving: Dit is een eenvoudig, keuzerondje besturingselement. Het gebruik van dit besturingselement is vergelijkbaar met een eenvoudige selectievakje. Er zijn twee keuzerondjes, naast de tekst labels weergegeven. Het besturingselement binden aan een Boolean-type gegevens wordt aanbevolen.

Eigenschappen:

1.  Algemene eigenschappen voor alle: Zie voor informatie over deze eigenschap het algemene gedeelte van de eigenschappen van dit document

2.  TekstAlsWaar: Dit is een optionele, type string-eigenschap. Dit is de tekst die wordt weergegeven wanneer het keuzerondje is ingeschakeld.

3.  TekstAlsOnwaar: Dit is een optionele, type string-eigenschap. Dit is de tekst die wordt weergegeven wanneer het keuzerondje niet is ingeschakeld.

4.  SelectedItem: Dit is een optionele, Boolean-type-eigenschap. Deze waarde geeft aan dat het keuzerondje is ingeschakeld. Dit kan worden verbonden met een Boolean-type gegevens van een gegevensbron. De standaardwaarde is ingesteld op false.

Gebeurtenissen:

   • CheckedChanged: uitgeschakeld wanneer de wijzigingen van de knop optie status van geselecteerd of het tegenovergestelde, het signaal is verzonden.

Voorbeeld:

![](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
Als u het voorbeeld werken, moet u nieuwe Boole-kenmerk ABooleanAttribute maken en bindt dit aan uw aangepaste brontype. De gegevens RCDC wordt naar hetzelfde type aangepaste resource toegepast.

Het volgende codesegment genereert het keuzerondje in de vorige afbeelding:

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

Naam: UocTextBox

Beschrijving: Dit is een eenvoudig tekstvak die ondersteuning biedt voor invoer van type string. Het is raadzaam dat u dit besturingselement binden met gegevens van type string.

Eigenschappen:

1.  Algemene eigenschappen voor alle: Zie het algemene gedeelte van de eigenschappen van dit document voor informatie over deze eigenschap.

2.  MaxLength: Dit is een optioneel, van het type geheel getal-kenmerk. Deze eigenschap geeft u de maximale lengte voor een tekenreeks die invoer. De standaardwaarde voor deze eigenschap is 128 tekens.

3.  Tekst: Dit is een optionele, type string-eigenschap. Dit is de tekst die wordt weergegeven in het tekstvak. U kunt geen expliciete tekenreeks die wordt weergegeven in het tekstvak tijdens de eerste laden van het besturingselement of binden aan een schemakenmerk van het type string definiëren.

4.  Rijen: Dit is een optionele, geheel getal-type-eigenschap. Deze eigenschap bepaalt de hoogte van het tekstvak in tekens. De standaardwaarde is 1 teken.

5.  Kolommen: Dit is een optionele, geheel getal-type-eigenschap. Deze eigenschap bepaalt de breedte van het tekstvak in tekens. De standaardwaarde is 20 tekens bestaan.

6.  Wrap: Dit is een optionele, Boolean-type-eigenschap. De waarde van deze eigenschap instelt op true, kan de gebruiker de functie terugloop in het tekstvak. De standaardwaarde van deze eigenschap is ingesteld op true.

7.  UniquenessValidationXPath: Dit is een optionele, type string-eigenschap. Het duurt een ongeldig XPath voor FIM-filterexpressie en zorgt ervoor dat de waarde die door de gebruiker invoer uniek is binnen de resources in het bereik van het filter.
    Bijvoorbeeld, om ervoor te zorgen dat de gebruiker gevraagd weergavenaam is uniek binnen alle e-mailtoegang beveiligingsgroepen in de FIM-Servicedatabase, gebruikt u het XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’`. De actie validatie wordt uitgevoerd wanneer de gebruiker de pagina verlaat. Deze eigenschap wordt alleen ondersteund in de RCDC voor het maken van een resource.

8.  UniquenessErrorMessage: Dit is een optionele, type string-eigenschap. Deze tekenreeks wordt gebruikt om weer te geven van een foutbericht weergegeven als de UniquenessValidationXPath-validatie is mislukt, en kan expliciete tekst of een tekenreeks Resource-variabele. Als deze eigenschap niet is opgegeven, is het standaardfoutbericht voor een mislukte validatie "% VALUE % bestaat al. Probeer een andere naam."

Gebeurtenissen:

   • TextChanged: deze gebeurtenis wordt verzonden wanneer de tekst in het tekstvak wordt gewijzigd.

Zie de sectie eenvoudig besturingselement voorbeelden voor een compleet codevoorbeeld van dit besturingselement.

## <a name="appendix-a-default-xsd-schema"></a>Bijlage A: standaard XSD-Schema

Hieronder ziet u het volledige XSD-schema voor alle standaard RCDCs die worden geleverd met Microsoft Identity Manager 2016 SP1:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```



## <a name="appendix-b-default-summary-xsl"></a>Bijlage B: standaard samenvatting XSL

Hieronder ziet u de volledige samenvatting XSL die wordt geleverd met Microsoft Identity Manager 2016 SP1:
```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```
