---
title: Bron besturings element weergave configuratie XML-verwijzing | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 0eedee975a785bd20ee37c85262a0c5f678b09b5
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010707"
---
# <a name="resource-control-display-configuration-xml-reference"></a>XML-referentie voor weergave configuratie voor bron besturings elementen

Resource control display Configuration (RCDC) resources zijn door de gebruiker gedefinieerde resources die u kunt gebruiken om te bepalen hoe andere resources in het gegevens archief van de Microsoft Identity Manager 2016 SP1 (MIM) in de gebruikers interface (UI) worden weer gegeven voor de eind gebruiker. Elke RCDC-resource bevat een XML-configuratie bestand dat u kunt wijzigen om UI-tekst en UI-besturings elementen toe te voegen, te wijzigen of te verwijderen. Hoewel MIM 2016 SP1 verschillende standaard RCDC-resources biedt, kunt u ook aangepaste RCDC-resources maken voor aangepaste resources. Zie [Inleiding tot het configureren en aanpassen van de FIM-Portal](/previous-versions/mim/ee534913(v=ws.10)) in de FIM-documentatie voor meer informatie over het gebruik van de RCDC-gebruikers interface in de FIM-Portal.


## <a name="known-issues"></a>Bekende problemen

De standaard waarde in veel RCDC-besturings elementen wordt niet ondersteund.

In deze release wordt het instellen van standaard waarden in besturings elementen in een resource besturings element niet ondersteund, behalve voor het besturings element keuze rondje. U kunt dit probleem omzeilen voor een vervolg keuzelijst door een standaard waarde op te geven die niet is gekoppeld aan een waarde om ervoor te zorgen dat de gebruiker de selectie wijzigt. U kunt dit probleem omzeilen met andere besturings elementen door een autorisatie werk stroom te gebruiken om een standaard waarde op te geven tijdens het verzenden van de aanvraag.

## <a name="basic-structure"></a>Basis structuur

De XML-gegevens voor een RCDC-resource bestaan uit één **ObjectControlConfiguration** XML-element.

>[!NOTE]
>Zie voor het volledige XSD-schema <a href="#appendix-a">bijlage A: standaard XSD-schema</a>.

Hier volgt het XSD-schema voor het element **ObjectControlConfiguration** :

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

Het **ObjectControlConfiguration** -element bevat de volgende elementen:

- **ObjectDataSource**: in dit element wordt de TypeName opgegeven van een gegevens bron klasse die door het resource control (RC) wordt gebruikt. Zie de sectie over de volgende gegevens bronnen in dit document voor een beschrijving en de schema definitie. Een **ObjectControlConfiguration** -element kan maxi maal 32 knoop punten van het element **ObjectDataSource** bevatten.

- **XmlDataSource**: dit is een eenvoudige gegevens bron die meestal wordt gebruikt om het ontwerp van een overzichts pagina op te geven. Zie de sectie over de volgende gegevens bronnen in dit document voor een beschrijving en de schema definitie. Een **ObjectControlConfiguration**:-element kan maxi maal 32 knoop punten van het **XmlDataSource** -element bevatten.

- **Paneel**: de beheerder kan de lay-out van de RCDC pagina aanpassen door elementen in de deel venster elementen te wijzigen. Zie de sectie van het deel venster verderop in dit document voor meer informatie. Een **ObjectControlConfiguration** -element mag slechts één panel element hebben.

- **Gebeurtenissen**: beheerders kunnen geen aangepaste code opgeven achter deze functie is beperkt. Dit is de gebeurtenis die een paneel of een besturings element kan verzenden, op basis van een status wijziging. Zie de sectie gebeurtenissen verderop in dit document voor meer informatie. Een **ObjectControlConfiguration** -element kan optioneel één **gebeurtenis** element bevatten. In het algemeen wordt het gebruik van aangepaste **gebeurtenissen** niet ondersteund, tenzij specifiek is ontwikkeld in latere verbeteringen.

## <a name="data-sources"></a>Gegevensbronnen

Microsoft Identity Manager gebruikt gegevens bronnen als een manier om gegevens te binden aan UI-onderdelen. Dit helpt de schei ding van de gegevens van de presentatielaag te vereenvoudigen. Er zijn twee soorten gegevens bronnen in de configuratie gegevens van de RCDC-Bron: **ObjectDataSource** en **XmlDataSource**.

-   **ObjectDataSources** geef een Microsoft .net klasse op die de gegevens aan de RC levert. Er is een vaste set beschik bare soorten ObjectDataSources die de beheerder kan kiezen om te gebruiken bij het ontwerpen van RCDCs.

-   **XMLDataSources** bieden een eenvoudige manier om XML-gegevens te structureren en ze kunnen door beheerders worden gebruikt om aangepaste gegevens op te geven. De XML-gegevens moeten rechtstreeks worden opgegeven in de RCDC, tenzij u de ingebouwde, vooraf gedefinieerde XML-structuur gebruikt. De ingebouwde XML-structuur wordt gebruikt voor het genereren van overzichts pagina's in de RC.

In de RCDC kunt u deze gegevens bronnen binden aan kenmerken van de UI-besturings elementen die zijn opgegeven in de RCDC om de gebruikers interface te genereren.

### <a name="objectdatasource-elements"></a>ObjectDataSource-elementen

Micro soft Identity Manager biedt de algemene gegevens bron typen in de volgende tabel die beschikbaar zijn voor alle resource typen (tenzij anders vermeld).

| TypeName | Beschrijving | Twee richtings binding | Bindings syntaxis |
|---|---|---|---|
| PrimaryResourceObjectDataSource | Dit vertegenwoordigt de FIM 2010-resource die wordt gemaakt, bewerkt of weer gegeven. Het pad in de bindings teken reeks is de kenmerk naam. Het bron type wordt opgegeven door het kenmerk TargetObjectType van de RCDC in plaats van in het kenmerk RCDC.ConfigurationData. | Ja | `[AttributeName]` waarde van het object kenmerk dat is opgegeven met de naam. |
| PrimaryResourceDeltaDataSource  | Deze gegevens bron bouwt de Delta-XML die de oorspronkelijke status en de huidige status van de FIM 2010-resource vergelijkt. De gegenereerde Delta-XML wordt gebruikt door het besturings element RC-samen vatting om de gebruikers interface weer te geven voor aanvragen die de gebruiker verzendt. | Nee | `DeltaXml` Dit wordt gebruikt in combi natie met het besturings element samen vatting om de Delta weer te geven. |
| PrimaryResourceRightsDataSource | Deze gegevens bron biedt de inline rechten voor elk kenmerk van de FIM 2010-resource. Hierdoor kan de RC vooraf bepalen van de verzen ding welke machtigingen de gebruiker heeft voor dat kenmerk en vervolgens de gebruikers interface voor dat kenmerk op de juiste manier weer geven. | Nee | `[AttributeName]` |
| SchemaDataSource                | Deze gegevens bron kan worden gebruikt om toegang te krijgen tot schema-informatie, zoals weergave naam, beschrijving, ongeacht of het kenmerk vereist is, evenals informatie over het resource type. | Nee | `[AttributeName].Required` Booleaanse waarde waarmee wordt aangegeven of het kenmerk een geldige waarde moet hebben. <br/> `[AttributeName].DisplayNameString` Waarde die de weergave naam van de binding aangeeft. <br/> `[AttributeName].DescriptionString` Waarde die de beschrijving van de binding aangeeft. <br/>`[AttributeName].StringRegexString` waarde die de regex-expressie van een binding aangeeft. <br/> `[AttributeName].DisplayName` <br/> `[AttributeName].Description` <br/> `[AttributeName].IntegerValueMinimum` <br/>`[AttributeName].IntegerValueMaximum` <br/>`[AttributeName].LocalizedAllowedValues` |
| DomainDataSource                | Deze gegevens bron bevat een inventarisatie van domeinen, op basis van de domein configuratie bronnen. Deze gegevens bron kan alleen worden gebruikt in RCDCs die voor groeps resources en gebruikers resources zijn. | Ja | Domain |

Hier volgt een voor beeld van een RCDC-fragment dat drie gegevens bronnen verbindt met het besturings element UocTextBox om het kenmerk Description van een groep te bewerken:

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

### <a name="xmldatasource-element"></a>XMLDataSource-element

U kunt met behulp van een **XMLDataSource** -element aangepaste gegevens opgeven die door de RCDC kunnen worden gebruikt voor een bepaalde resource. In dit geval moeten de XML-gegevens worden opgegeven in de RCDC. Als alternatief kunt u deze gegevens bron gebruiken om te verwijzen naar een ingebouwde XML-gegevens structuur om de gebruikers interface voor overzichts pagina's weer te geven. U bepaalt welk type **XMLDataSource** moet worden gebruikt wanneer u deze definieert in het RCDC.


| TypeName | Beschrijving | Twee richtings binding | Bindings syntaxis |
|---|---|---|---|
| **XMLDataSource**            | De gegevens bron bevat XML-gegevens. De gegevens kunnen in XSL-of Inge sloten XSL-indeling zijn:<ul><li>XSL-indeling in Microsoft.IdentityManagement.WebUI.Controls.dll:<br/>```<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement. WebUI.Controls.Resources.DefaultSummary.xsl"> </my:XmlDataSource>```</li><li>Inge sloten XSL-indeling:<br/>```<my:XmlDataSource my:Name="RequestStatusTransformXsl"><xsl:stylesheet version="1.0" xmlns:xsl=http://www.w3.org/1999/XSL/Transform xmlns:msxsl="urn:schemas-microsoft-com:xslt"></xsl:stylesheet></my:XmlDataSource>```</li></ul>  | Nee | `Xpath[;namespaces]`waar `Xpath` is een geldig XML-XPath om de vereiste notitie te selecteren, meestal '/' (root). `namespaces` is een optionele lijst met voor voegsel = URI-teken reeksen. De teken reeks wordt gescheiden door punt komma's als vereist om het XPath te laten werken aan de naam ruimte-XML. |
| **ReferenceDeltaDataSource** | De gegevens bron vertegenwoordigt Deltas van verwijzings kenmerken met meerdere waarden. Het wordt alleen gebruikt voor de groep en het instellen van RCDC. <br/> Hoewel de gegevens bron niet beperkt is tot groepen of sets, is het vereist dat code wijzigingen in de RCDC-host deze verschillen indienen. Op dit moment zijn groep en set de enige hosts die deze gegevens bron herkennen.  | Ja | `[AttributeName].Add` waar `[AttributeName]` staat voor een referentie kenmerk en de geretourneerde gegevens zijn de Delta toevoegingen.<ul><li>Voorbeeld: `[ReferenceAttribute].Add`</li><li>Voorbeeld: `<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}"/>`</li></ul>`[AttributeName].Remove` waar `[AttributeName]` staat voor een verwijzings kenmerk en de geretourneerde gegevens zijn de Delta-verwijderingen. <br/> DeltaXml <!-- Is bold formatting needed for DeltaXml? --> |
|**RequestDetailsDataSource**| De gegevens bron vertegenwoordigt het RequestParameter-kenmerk van aanvraag objecten. Met de para meter stelt u het maximum aantal kenmerk waarden in dat moet worden weer gegeven per kenmerk met meerdere waarden. Het wordt alleen gebruikt in RCDC voor request. `<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />`| Nee | DeltaXml |
|**RequestStatusDataSource**| De gegevens bron vertegenwoordigt het **RequestStatusDetails** -kenmerk van aanvraag objecten. Het wordt alleen gebruikt in RCDC voor request. | Nee | DeltaXml |

Als u een aangepaste XML-gegevens bron wilt definiëren, gebruikt u de volgende XML:

 ```XML
<my:XmlDataSource my:Name="MyCustomData" >
     %Insert custom, properly formatted XML data here%
</my:XmlDataSource>
```

Als u de ingebouwde samenvattings controle-XSL wilt gebruiken, definieert u de gegevens bron als volgt:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

Als u een RCDC maakt voor een aangepast resource type, kunt u deze methode gebruiken om automatisch een overzichts pagina voor die aangepaste resource weer te geven.

Hieronder volgt een voor beeld van het maken van een overzichts tabblad in het RCDC met behulp van het **PrimaryResourceDeltaDataSource** -element met het **XMLDataSource** -element met behulp van de ingebouwde xsl:

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

Als alternatief kan de gebruiker het XmlDataSource-element vervangen dat eerder is opgegeven met de volgende indeling om een aangepaste indeling van een overzichts pagina te definiëren. Als referentie is de standaard-XSL 2010-samen vatting opgenomen in bijlage B: standaard-XSL-overzicht, verderop in dit document.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Schema voor gegevens bronnen
In het volgende XSD-schema worden de twee typen gegevens bronnen gegenereerd:

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

## <a name="event-element"></a>Gebeurtenis element
Een **gebeurtenis** element definieert de wijzigings status van een besturings element. De uitbreid baarheid van deze functie is beperkt omdat u geen aangepaste functie (handler) kunt schrijven om te definiëren wat het gedrag is nadat een gebeurtenis is geactiveerd. Hetzelfde gebeurtenis element kan in het deel venster element worden gebruikt. Zie de sectie van het deel venster verderop in dit document voor meer informatie.

Hier volgt het XSD-schema voor het gebeurtenis element:

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

Een **gebeurtenis** is een leeg element en heeft de volgende kenmerken:

- **Naam**: dit is de unieke naam van een gebeurtenis. De enige ondersteunde gebeurtenis in **ObjectControlConfiguration** is de gebeurtenis Load. Deze gebeurtenis wordt geactiveerd wanneer de pagina voor het eerst wordt geladen.

- **Handler**: dit is de unieke naam van een handler. Wanneer de gebeurtenis wordt geactiveerd, wordt meestal een programma methode aangeroepen om de wijziging van de status van het besturings element te verwerken. De volgende gevallen worden niet ondersteund:

   - Een bestaande handler verwijderen uit een bestaand besturings element.
   - Er wordt een nieuwe handler gemaakt.
   - Een handler koppelen aan een bestaand of nieuw besturings element.

Hier volgt een voor beeld van een-element **Events** :

```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

## <a name="panel-element"></a>Paneel element
Het **deel venster** element is het kern element in een RCDC-indeling. Hier volgt het XSD-schema voor het element panel:

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

Het **deel venster** element bevat een terugkerend element, **groeperen**. Zie de sectie groepering in dit document voor meer informatie.

Het element panel heeft de volgende kenmerken:

- **Name**: de naam van het paneel. Dit is een vereist kenmerk van het type teken reeks.

- **DisplayAsWizard**: dit kenmerk is momenteel afgeschaft. Het bijbehorende VerbContext-kenmerk in de RCDC bepaalt of de resource-indeling zich in de wizard modus of de tabblad modus bevindt. Als deze is ingesteld op 0 (aanmaak modus), is het ook in de modus Wizard. Anders is het de modus voor het tabblad. Zie Inleiding tot het configureren en aanpassen van de FIM-Portal in de documentatie voor meer informatie.

- **Bijschrift**: dit kenmerk is momenteel afgeschaft. De gebruiker kan bijschriften opgeven voor een pagina door een groep op te nemen die alleen koptekst informatie bevat. Zie de sectie groepering in dit document voor meer informatie.

- **Autovalidate**: dit is een optioneel Booleaans kenmerk. Wanneer deze eigenschap is ingesteld op True, wordt de validatie geactiveerd voor elk besturings element op het huidige tabblad. Als het kenmerk ontbreekt, wordt het standaard ingesteld op True. Deze kan worden gebruikt in combi natie met de eigenschap RegularExpression. Zie ' RegularExpression ' in een latere sectie van dit document voor meer informatie.

## <a name="grouping-element"></a>Groepeer element
Het **Groepeer** element definieert de algemene indeling van een paneel. Het fungeert als een container waarmee afzonderlijke besturings elementen worden gegroepeerd in verschillende secties en tabbladen. Hier volgt het XSD-schema voor het Groepeer element:

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

Er zijn drie typen **Groepeer** element:

- **Groeperen in koptekst**: een header groepering is optioneel. Er kan slechts één header-groepering in een **deel venster** zijn. Een koptekst groepering wordt weer gegeven boven op een deel venster als bijschrift. Er kan slechts één UocCaptionControl worden gebruikt in deze groepering. Zie de sectie voor beeld voor een voor beeld van het groeperen van een header.

- **Inhouds groepering**: er is ten minste één inhouds groepering vereist. Er kunnen meerdere inhouds groepen in een paneel zijn. Een inhouds groepering wordt weer gegeven als de hoofd inhoud van een RCDC-pagina. Elke inhouds groepering wordt weer gegeven als een tabblad in hetzelfde deel venster en kan 1 tot 256 besturings elementen bevatten. Zie de sectie voor beelden voor een voor beeld van het **groeperen van inhoud**.

- **Samen vatting groeperen**: een overzichts groepering is optioneel. Er kan slechts één samen vatting worden gegroepeerd in een deel venster. Een overzichts groepering wordt weer gegeven als het laatste tabblad van een deel venster. Er kan slechts één **UocHtmlSummary** -besturings element worden gebruikt in een overzichts groepering om de wijzigingen weer te geven die de gebruiker heeft aangebracht voordat een aanvraag wordt ingediend. Zie de sectie voor beelden voor een voor beeld van het groeperen van een samen vatting.

Elk groeperings type bevat de volgende elementen:

- **Help**: dit element biedt Help-tekst op een tabblad. U kunt dit ook gebruiken om een koppeling toe te voegen aan een Help-bestand voor het tabblad.

- **Besturings elementen**: Zie de sectie besturings element in dit document voor meer informatie over dit element. Elke groepering moet 1 tot 256 besturings elementen omvatten, afhankelijk van het type groepering.

- **Gebeurtenissen**: Zie de sectie gebeurtenissen in dit document voor meer informatie over dit element. Elke groepering kan als optie één gebeurtenis hebben. De gebeurtenissen die in een groepeer element worden ondersteund, zijn als volgt:

    - **BeforeLeave**: deze gebeurtenis wordt geactiveerd wanneer de gebruiker klaar is om een tabblad in een inhouds groepering te verlaten.
    - **AfterEnter**: deze gebeurtenis wordt geactiveerd wanneer de gebruiker klaar is om een tabblad in een inhouds groepering in te voeren.

Een groepering kan de volgende zeven kenmerken bevatten:

- **Naam**: dit is de vereiste naam van de groepering. De **naam** moet uniek zijn binnen het **paneel**.

- **Bijschrift**: het **Bijschrift** wordt weer gegeven als koptekst bijschrift in een koptekst groepering. Deze wordt weer gegeven als het bijschrift van een inhoud of samen vatting van een groepering.

- **Beschrijving**: een optioneel teken reeks kenmerk, **Beschrijving** is alleen functioneel wanneer het wordt gebruikt in een inhouds groepering. Gebruik dit element om de eind gebruiker enige details over de informatie op hetzelfde tabblad te geven.

    >[!NOTE]
    >Als dit kenmerk wordt gebruikt in een overzichts groepering, wordt de XML als ongeldig beschouwd. Als dit kenmerk wordt gebruikt in een koptekst groepering, wordt de XML beschouwd als geldig, maar genegeerd.
    >

- **Ingeschakeld**: een optioneel Booleaans kenmerk, dat is ingeschakeld, wordt ingesteld op waar als dit ontbreekt. Als ingeschakeld is ingesteld op ONWAAR, ziet de eind gebruiker een uitgeschakeld tabblad. Dit kenmerk werkt alleen in een groepering van inhoud.

    >[!NOTE]
    >Als dit kenmerk wordt gebruikt in een overzichts groepering, wordt de XML als ongeldig beschouwd. Als dit kenmerk wordt gebruikt in een koptekst groepering, wordt de XML beschouwd als geldig, maar genegeerd.
    >

- **Zichtbaar**: u kunt een RCDC-pagina of een kop verbergen door dit kenmerk op ONWAAR in te stellen. Standaard is dit optioneel, het kenmerk Boolean-type ingesteld op True. Dit kenmerk werkt alleen voor het groeperen van inhoud.

    >[!NOTE]
    >Als er slechts één inhouds groepering in een deel venster is, werkt deze functie niet. Wanneer er meer dan één inhouds groepering in een paneel is, gedraagt het zich zoals eerder is beschreven.

- **IsHeader**: dit kenmerk is een optioneel, Booleaans kenmerk dat bepaalt of de groepering een koptekst groepering is. Als dit kenmerk niet is opgegeven, wordt het ingesteld op ONWAAR.

- **IsSummary**: dit is een optioneel, Booleaans kenmerk dat bepaalt of de groepering een overzichts groepering is. Als dit kenmerk niet is opgegeven, wordt het ingesteld op ONWAAR.

### <a name="examples-for-types-of-grouping-elements"></a>Voor beelden voor typen Groepeer elementen
Deze sectie bevat voor beelden voor het element groups.

#### <a name="example-header-grouping"></a>Voor beeld: koptekst groepering
In de volgende afbeelding ziet u een voor beeld van het groeperen van kopteksten:

![Koptekst groepering](media/rcd-configuration-xml-reference/image005.jpg)

Met de volgende XML wordt een voor beeld van een header groepering gegenereerd. In het XML-bestand is de koptekst groepering het gebied met het bijschrift ' voorbeeld header-groepering '.

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

#### <a name="example-content-grouping"></a>Voor beeld: inhouds groepering
In de volgende afbeelding ziet u een voor beeld van het groeperen van inhoud:

![Inhouds groepering](media/rcd-configuration-xml-reference/image007.jpg)

Met de volgende XML wordt een voor beeld van een inhouds groepering gegenereerd. In het XML-bestand is de groepering van inhoud het gebied met de tekst ' voorbeeld groepering van inhoud '.

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

#### <a name="example-summary-grouping"></a>Voor beeld: overzicht groeperen

In de volgende afbeelding ziet u een voor beeld van een groepering van een samen vatting:

![Samen vatting groeperen](media/rcd-configuration-xml-reference/image010.jpg)

In de volgende XML wordt een voor beeld van een samen vatting van de groepering gegenereerd. In het XML-bestand wordt de samen vatting gegroepeerd op het gebied met het bijschrift ' voorbeeld groepering van samen vatting '.

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

### <a name="help-element"></a>Help-element
Het **Help** -element kan worden opgenomen in een groepeer-of Control-element als een optioneel element. Als deze wordt gebruikt in een groepering, moet dit het eerste element zijn dat wordt gebruikt. Het bevat tekstuele hulp voor de eind gebruikers om ze te helpen nauw keurige informatie te bieden. Het volgende XSD-schema is voor het Help-element:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```

Met de volgende XML-voorbeeld code wordt een Help-element gegenereerd:

```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control-element"></a>Control-element
Een groepeer element bevat een of meer **besturings** elementen. Besturings elementen zijn de belangrijkste elementen in een RCDC. U kunt het Groepeer element aanpassen door de verschillende besturings elementen te definiëren die deze bevat. Het volgende XSD-schema is voor het element Control:

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

Een Control-element bevat de volgende elementen:

- **Help**: dit element wordt genegeerd. De functie werkt alleen in groeperingen.

- **CustomProperties**: dit element wordt niet ondersteund.

- **Opties**: dit element wordt alleen gebruikt in combi natie met de besturings elementen **UocDropDownList** of **UocRadioButtonList** . Deze functie werkt niet met andere besturings elementen. Zie de sectie opties in dit document voor de structuur van dit element. Zie de sectie afzonderlijke besturings elementen van dit document voor meer informatie over de opties die door een besturings element worden gebruikt.

- **Knoppen**: dit element wordt alleen gebruikt in combi natie met het besturings element **UocListView** . Deze functie werkt niet voor andere besturings elementen. Zie de sectie UocListView in dit document voor meer informatie.

- **Eigenschappen**: dit element wordt in alle besturings elementen gebruikt om extra gedrag van een besturings element op te geven. Zie de sectie eigenschappen in dit document voor meer informatie over dit element.

- **Gebeurtenissen**: Zie de sectie evenementen eerder in dit document voor de structuur van dit element. Zie de sectie afzonderlijke besturings elementen van dit document voor een overzicht van de gebeurtenissen die in een besturings element worden gebruikt.

Een Control-element kan de volgende 10 kenmerken bevatten:

- **Naam**: dit is de naam van het besturings element. De naam van een besturings element moet uniek zijn binnen elk paneel. Dit is een vereist kenmerk van het type teken reeks.

- **TypeName**: dit kenmerk geeft aan welk type besturings element het is. Dit is een vereist kenmerk van het type teken reeks. Zie de sectie afzonderlijke besturings elementen in dit document voor elke naam van het besturings element.

- **Bijschrift**: u kunt dit kenmerk gebruiken voor het toevoegen van een bijschrift voor het besturings element. Het bijschrift is doorgaans de weergave naam van de gegevens die door het besturings element worden weer gegeven of in het bedrijf worden geplaatst. U kunt expliciet een waarde voor het bijschrift opgeven of deze binden met informatie over de weergave naam van het schema kenmerk. Het bijschrift wordt weer gegeven aan de linkerkant van een besturings element met een normale grootte. Als een besturings element het volledige scherm beslaat, wordt het bijschrift over het besturings element weer gegeven. Dit is een optioneel kenmerk van het type teken reeks. Zie de sectie Properties (eigenschappen) voor meer informatie over het binden van een gegevens bron met een kenmerk of een eigenschaps waarde.

    In het volgende voor beeld ziet u hoe een bijschrift expliciet kan worden gebruikt:

    ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
    ```

    In het volgende voor beeld ziet u hoe een bijschrift kan worden gebruikt met een gegevens bron. Als u de sjabloon hebt gebruikt voor een gegevens bron die eerder in dit document is weer gegeven, is de gegevens bron schema. U wordt aangeraden de DisplayName van het kenmerk te binden met een bijschrift kenmerk.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```

- **Ingeschakeld**: dit is een optioneel, Booleaans type-kenmerk. Door deze kenmerk waarde op ONWAAR in te stellen, kan de gebruiker een besturings element uitschakelen. De standaard waarde is ingesteld op True.

- **Visible**: dit is een optioneel, Booleaans type-kenmerk. U kunt dit kenmerk gebruiken om het hele besturings element te verbergen. De standaard waarde is ingesteld op True.

- **Beschrijving**: gebruik dit optionele kenmerk voor het type teken reeks om een beschrijving op te nemen om de eind gebruiker te helpen begrijpen wat ze in het besturings element moeten plaatsen of wat het besturings element doet. U kunt expliciet een waarde opgeven voor de beschrijving of deze binden met de informatie over de beschrijving van het schema kenmerk.

    De beschrijving wordt weer gegeven aan de linkerkant van een besturings element met een normale grootte onder het bijschrift. Als een besturings element het volledige scherm beslaat, wordt de beschrijving boven aan het besturings element onder het bijschrift weer gegeven. Zie de sectie eigenschappen in dit document voor informatie over het binden van een gegevens bron met een kenmerk of een eigenschaps waarde.

    In het volgende voor beeld ziet u hoe een beschrijving expliciet kan worden gebruikt:

    ```XML
    <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
    ```

    In dit voor beeld ziet u hoe een beschrijving kan worden gebruikt met een gegevens bron. Als u de sjabloon hebt gebruikt voor een gegevens bron die eerder in dit document is weer gegeven, is de gegevens bron **schema**. We raden aan dat u de **Beschrijving** van het kenmerk verbindt met een beschrijvings kenmerk.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
    ```

- **ExpandArea**: dit kenmerk geeft aan of het besturings element het volledige scherm omspant. Dit is een optioneel, Booleaans type kenmerk. De standaard waarde is ingesteld op ONWAAR.

    >[!NOTE]
    >De kenmerken bijschrift en beschrijving zijn uitgeschakeld wanneer dit kenmerk is ingesteld op True. Gebruik het besturings element UocLabel om een bijschrift voor een uitgebreid besturings element op te geven.
    >

- **Hint**: dit is een optioneel kenmerk van het type teken reeks. De tekst in het Hint kenmerk helpt de eind gebruiker te bepalen wat een geldige invoer is voor het besturings element. De hint wordt onder het besturings element weer gegeven.

- **Auto post back**: dit is een optioneel, Booleaans type-kenmerk. De standaardwaarde is false. Als de waarde is ingesteld op ONWAAR, wordt het besturings element mogelijk niet vernieuwd als u de pagina vernieuwt. Zoek voor meer informatie over auto posting naar de micro soft ASP.NET UI Control-eigenschap met dezelfde naam.

- **RightsLevel**: dit is een optioneel kenmerk van het type String. U kunt dit kenmerk alleen binden met inline-rechten met een gegevens bron. Het besturings element wordt dynamisch ingeschakeld of uitgeschakeld, op basis van de rechten van de gebruiker. Zie de sectie eigenschappen in dit document voor informatie over het binden van gegevens bronnen met een kenmerk of een eigenschaps waarde.

    In dit voor beeld ziet u hoe een **RightsLevel** -kenmerk kan worden gebruikt met een gegevens bron. Als u de sjabloon hebt gebruikt voor een gegevens bron die eerder in dit document is weer gegeven, hebt u de **toegang** tot de gegevens bron. Gebruik de naam van het kenmerk als het pad.
    <!--- no example provided -->

### <a name="property-element"></a>Eigenschaps element
U kunt een **eigenschaps** element gebruiken om het gedrag van elk besturings element verder aan te passen. Een eigenschap is een leeg element. Het volgende XSD-schema is voor het eigenschaps element:

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

Elke eigenschap heeft de volgende twee vereiste kenmerken:

- **Naam**: dit kenmerk van het type teken reeks is de unieke naam van de eigenschap. Verschillende besturings elementen hebben andere eigenschappen. Er zijn enkele algemene eigenschappen die kunnen worden gebruikt door alle besturings elementen. Zie de secties algemene eigenschappen en afzonderlijke besturings elementen van dit document voor meer informatie over welke namen beschikbaar zijn voor een bepaald besturings element.

- **Waarde**: dit is de waarde van de eigenschap. Het gegevens type van de waarde is afhankelijk van de eigenschap waaraan deze is toegewezen. Raadpleeg de volgende sectie voor de indeling toegestane waarde voor specifieke eigenschappen.


#### <a name="bind-property-with-data-source-content"></a>Eigenschap binden met gegevens bron inhoud
Sommige eigenschappen kunnen worden gebonden aan gegevens uit een gegevens bron. Gebruik de volgende teken reeks indeling om deze binding te maken. Zie de beschrijving voor de afzonderlijke eigenschappen in de sectie afzonderlijke besturings elementen van dit document voor meer informatie over het koppelen van eigenschappen aan een gegevens bron.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

   Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}

   SourceExpression:= “Source=” + [ObjectDataSourceName]

   PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

   ModeExpression:= “Mode=” + [ModeChoice]

   ModeChoice:= “OneWay”|”TwoWay”

   ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

   AttributeName:= valid schema attribute name from the data source.

   AttributePropertyName:= valid property name of a schema attribute from the data source.
```

In het volgende XML-bestand ziet u hoe u een gegevens bron verbindt met een **eigenschaps** element:

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
```


<h2 id="common-properties">Algemene eigenschappen</h2>

Alle RCDC-besturings elementen die in dit document zijn opgegeven, kunnen de algemene eigenschappen hebben die in deze sectie worden beschreven. U kunt deze eigenschappen gebruiken samen met andere eigenschappen die specifiek zijn voor verschillende besturings elementen.

- **Vereist**: deze eigenschap geeft aan dat het veld een verplicht veld of een optioneel veld is. Een vereist veld moet worden gevuld met een waarde. Een lege waarde wordt niet ondersteund voor teken reeks invoer. Een optioneel veld kan leeg blijven. Als dit veld een verplicht veld is waarin geen waarde is ingevuld, wordt een fout bericht weer gegeven boven op het besturings element voor invoer. U kunt expliciet opgeven of een veld vereist of optioneel is. U kunt het veld ook binden met de schema gegevens van een bepaalde binding tussen een kenmerk en een resource type. Als deze eigenschap ontbreekt, betekent dit dat het besturings element een optioneel invoer besturings element is.

    In het volgende voor beeld wordt een expliciete waarde voor deze eigenschap gebruikt:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```

    Dit is een voor beeld waarbij een dynamische gegevens bron wordt gebruikt voor deze eigenschap. Als u de sjabloon hebt gebruikt voor een gegevens bron die wordt weer gegeven in de vorige sectie van dit document, is de gegevens bron schema. Gebruiken `<attribute name>.Required` als pad.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```

- **ReadOnly**: door deze eigenschap in te stellen op waar, heeft de eind gebruiker het besturings element in een alleen-lezen modus. Dit is een optioneel, Booleaans type kenmerk. De standaard waarde is ingesteld op ONWAAR. Soms wordt het gedrag van deze eigenschap echter overschreven door het type rechten dat een persoon heeft op de gegevens binding met het besturings element. Als een gebruiker bijvoorbeeld geen rechten heeft om een veld bij te werken en het veld is gebonden aan inline-rechten, ziet de gebruiker de gegevens in een alleen-lezen modus, zelfs als deze eigenschap is ingesteld op false.

- **RegularExpression**: met deze eigenschap worden de beperkingen opgegeven die worden opgelegd voor de waarde in het besturings element. De notaties van deze eigenschaps waarde zijn de indelingen die worden ondersteund in de .NET StringRegex-standaard. Zie [.NET Framework reguliere expressies](https://go.microsoft.com/fwlink/?LinkId=165361)voor meer informatie. Als het besturings element wordt gebruikt om een waarde in te voeren, wordt de waarde gecontroleerd op basis van de beperking die is opgegeven in deze eigenschap wanneer de gebruiker de huidige pagina probeert te verlaten. Het fout bericht wordt weer gegeven boven op het besturings element met ongeldige invoer. De gebruiker kan expliciet een teken reeks reguliere expressie opgeven. De gebruiker kan deze ook binden met schema-informatie van een bepaald kenmerk. Als deze eigenschap ontbreekt, betekent dit dat het besturings element niet controleert op eventuele beperkingen van invoer teken reeksen.

    In het volgende voor beeld wordt een expliciete waarde voor deze eigenschap gebruikt:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```

    Dit is een voor beeld waarbij een dynamische gegevens bron wordt gebruikt voor deze eigenschap. Als u de sjabloon hebt gebruikt voor een gegevens bron die eerder in dit document is weer gegeven, is de gegevens bron schema. Gebruik `<attribute name>.StringRegex` als pad.

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```

- **Visible**: dit is een optioneel, Booleaans type-kenmerk. U kunt dit kenmerk gebruiken om het hele besturings element te verbergen. De standaard waarde is ingesteld op True.


<h3 id="options-element">Opties-element</h3>

Het element Options bevat een of meer **optie** subknooppunten. Het element **Options** wordt alleen gebruikt met de **UocRadioButtonList** -en **UocDropDownList** -besturings elementen. Zie de sectie afzonderlijke besturings elementen van dit document voor meer informatie over het gebruik van deze besturings elementen.

Het volgende XSD-schema is voor het element Options:

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

Het element **Options** heeft de volgende kenmerken:

- **Waarde**: dit is een vereist kenmerk van het teken reeks type. Het kenmerk value moet uniek zijn binnen hetzelfde besturings element. Alleen A t/m Z, hoofdletter gevoelige tekens kunnen worden gebruikt.

- **Bijschrift**: dit vereiste kenmerk is de weergave naam van elke optie.

- **Hint**: dit is een optioneel kenmerk. Gebruik dit kenmerk om meer informatie en hints voor de eind gebruiker op te geven.


## <a name="environment-variables"></a>Omgevingsvariabelen

De volgende omgevings variabelen kunnen worden gebruikt in elke RCDC-configuratie:

| Variabele | Beschrijving |
|---|---|
| `<LoginID>`       | Hiermee wordt de ID weer gegeven van de gebruiker die momenteel is aangemeld.           |
| `<LoginDomain>`   | Hier wordt het domein weer gegeven van de gebruiker die momenteel is aangemeld.       |
| `<Today>  `       | De huidige datum en tijd weer geven                                |
| `<FromToday_nnn>` | Hier worden de huidige datum, plus `nnn` en tijd weer gegeven, waarbij `nnn` een geheel getal is.  |
| `<ObjectID> `     | De primaire Resource-ID van de RCDC.                                     |
| `<Attribute_xxx>` | Retourneert een opgegeven kenmerk, xxx, van de primaire resource RCDC. |


## <a name="debug-xml-configuration-files"></a>Fouten opsporen in XML-configuratie bestanden

Wanneer u XML-configuratie bestanden ontwikkelt of wijzigt voor een RCDC, kunt u fouten verminderen door de XML te valideren op XSD-bestanden met behulp van een editor zoals micro soft Visual Studio. Zie voor meer informatie [een inleiding tot de XML-Hulpprogram ma's in Visual Studio 2005](https://go.microsoft.com/fwlink/?LinkID=74512).


## <a name="customize-help-files"></a>Help-bestanden aanpassen

Als u nieuwe resources en kenmerken maakt, wilt u mogelijk de bestaande Help-bestanden in de FIM-Portal bijwerken met inhoud voor uw aangepaste resources. Help-bestanden in de FIM-Portal hebben de indeling. htm en kunnen hand matig worden bewerkt. Zie voor meer informatie over het maken van aangepaste kenmerken Inleiding tot aangepast resource-en kenmerk beheer in de FIM 2010-documentatie.

>[!IMPORTANT]
>Informatie over de basis principes van het format teren of bewerken van HTML is niet opgenomen in dit artikel. Gebruikers worden verwacht hoe ze HTML-bestanden kunnen bewerken.

### <a name="location-of-help-files"></a>Locatie van Help-bestanden
Alle Help-bestanden voor de Microsoft Identity Manager 2016 SP1-Portal bevinden zich in de map `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html` op de server van de MIM-service.

### <a name="locate-a-specific-help-file"></a>Een specifiek Help-bestand zoeken
Alle Help-bestanden voor de FIM-Portal hebben de naam een Globally Unique Identifier (GUID). Het juiste bestand voor uw aangepaste resource zoeken:

1. Open in de FIM-Portal het Help-bestand op de portal pagina die u wilt aanpassen.

2. Klik met de rechter muisknop op het Help-bestand en selecteer **Eigenschappen**.

3. Markeer en kopieer het `<GUID\>.htm` bestand in het **URL-adres** veld.

4. Blader naar de map waarin de Help-bestanden worden opgeslagen en zoek naar het bestand.


## <a name="add-content-for-attribute-in-existing-grouping-element"></a>Inhoud voor kenmerk toevoegen in bestaand Groepeer element
Beschrijvende inhoud toevoegen voor een nieuw kenmerk binnen een bestaand Groepeer element (tab):

1. Identificeer en zoek het juiste Help-bestand.

2. Open het bestand met behulp van een HTML-editor.

3. Ga naar de locatie waar u de inhoud wilt toevoegen. Dit is normaal gesp roken een extra alinea, bijvoorbeeld:

    `<p xmlns="">A new paragraph with customized information.</p>`

    Het kan ook een item zijn dat in een bestaande lijst is ingevoegd, bijvoorbeeld:

    ```
    <li class="unordered"><b>First Name</b> – The first name of the User.<br>
    <li class="unordered"><b>Last Name</b> - The last name of the User.<br>
    <li class="unordered"><b>Added a new line</b><br>
    ```

## <a name="add-content-for-existing-grouping-element"></a>Inhoud toevoegen voor een bestaand Groepeer element
Het meren deel van de FIM-Portal pagina's heeft meerdere groeperings elementen (of tabbladen) en de bijbehorende Help-bestanden hebben Blade secties die betrekking hebben op elk Groepeer element. De blad wijzers in de HTML worden opgegeven in de secties. Dit is bijvoorbeeld de HTML voor het tabblad werk gegevens uit het Help-bestand voor de pagina gebruiker maken in de FIM-Portal:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

Ernaar wordt verwezen door het Groepeer element **WorkInfo** in het XML-configuratie gegevens bestand voor de **configuratie voor het maken van** RCDC door de gebruiker. De `\<GUID\>.htm` Bestands naam en de blad wijzer worden opgegeven in de `my:Link` para meter:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

### <a name="simple-control-samples"></a>Eenvoudige controle voorbeelden
Deze sectie bevat voor beelden voor het maken van verschillende eenvoudige besturings elementen voor tekst vakken.

In de volgende afbeelding ziet u enkele eenvoudige besturings elementen voor tekst vakken in verschillende modi:

![Eenvoudige besturings elementen voor tekst vakken](media/rcd-configuration-xml-reference/image016.gif)

Met het volgende code segment wordt het eerste besturings element voor een tekstvak gemaakt, dat gebruikmaakt van expliciete tekst voor alle kenmerken en eigenschappen:

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

In het volgende code segment wordt het tweede besturings element tekstvak gemaakt, dat gebruikmaakt van dynamische binding techniek om het besturings element te koppelen aan een andere gegevens Bron:

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

Met het volgende code segment wordt het derde uitgebreide label en het tekstvak besturings element gemaakt:

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

Met het volgende code segment wordt het vierde uitgeschakelde tekstvak besturings element gemaakt. Hoewel dit besturings element geen zichtbaar verschil weergeeft tussen de uitgeschakelde status en de ingeschakelde status, kan de gebruiker geen gegevens meer invoeren in het tekstvak.

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


## <a name="individual-controls"></a>Afzonderlijke besturings elementen

In deze sectie worden de afzonderlijke besturings elementen gedocumenteerd die worden meegeleverd met Microsoft Identity Manager 2016 SP1.

### <a name="uocbutton"></a>UocButton

**Naam**: UocButton

**Beschrijving**: dit is een eenvoudig besturings element voor knoppen dat u kunt gebruiken om bepaalde acties te activeren. Omdat u echter geen eigen handler kunt opgeven, is het gebruik van dit besturings element beperkt.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **Tekst**: met deze eigenschap geeft u de tekst op die op de knop wordt weer gegeven. Dit is een optioneel kenmerk van het type teken reeks. De tekst neemt een expliciete teken reeks waarde.

**Gebeurtenissen**:

- **OnButtonClicked**: de gebeurtenis wordt verzonden wanneer op de knop wordt geklikt.

**Voorbeeld**:

![Besturings element UocButton](media/rcd-configuration-xml-reference/image017.png)

Het volgende XML-segment produceert een eenvoudige UocButton-beheer knop:

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

**Beschrijving**: dit besturings element wordt gebruikt om het bijschrift van een RCDC-pagina weer te geven. Dit besturings element is ontworpen om alleen te worden gebruikt als één besturings element in een koptekst groepering. Het gebruik ervan in een andere context kan weergave problemen of portal fouten veroorzaken.

**Modus**: alleen-lezen (ONEway)

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **MaxHeight:** Met deze eigenschap geeft u de maximum hoogte van het pictogram op in de sectie bijschrift. Deze eigenschap is optioneel. Deze eigenschap neemt een geheel getal in pixels. De standaard waarde is 32 pixels.

**Gebeurtenissen**:

- Er zijn geen gebeurtenissen voor dit besturings element.

**Voorbeeld**:

![Besturings element UocCaptionControl](media/rcd-configuration-xml-reference/image020.jpg)

In het volgende code segment wordt een **koptekst bijschrift** gegenereerd:

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

In het volgende code segment wordt een **Bijschrift voor expliciete inhoud** gegenereerd:

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

Het volgende code segment genereert een dynamisch bijschrift voor een **weergave naam** :

```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```


### <a name="uoccheckbox"></a>UocCheckBox

**Naam**: UocCheckBox

**Beschrijving**: dit is een eenvoudig besturings element voor selectie vakjes. We raden aan dat de gebruiker dit besturings element verbindt met gegevens van het type Booleaans. Dit besturings element kan worden gebruikt als een alleen-lezen besturings element of een besturings element dat kan worden bijgewerkt op basis van de gegevens waaraan het is gekoppeld.

>[!NOTE]
>Als in deze release het besturings element selectie vakje in de bewerkings modus wordt gebruikt om een Boole-kenmerk weer te geven als aan het kenmerk geen waarde is toegewezen, wordt de waarde **False** aan het kenmerk toegevoegd wanneer u op **OK** klikt in de bewerkings modus. De tijdelijke oplossing is om altijd een Boole-kenmerk te maken dat ervan uitgaat dat niet-bestaan hetzelfde is als **Onwaar**, of gebruik andere besturings elementen, zoals een keuze rondje voor Booleaanse kenmerken.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **DefaultValue**: dit is een optionele, Booleaanse-type eigenschap. De standaard waarde is ingesteld op ONWAAR. In dit veld wordt het standaard gedrag van een selectie vakje opgegeven. Dit kan expliciet worden opgegeven.

- **Gecontroleerd**: dit is een optionele, Booleaanse-type eigenschap. De standaard waarde is ingesteld op ONWAAR. Deze waarde overschrijft de eigenschap DefaultValue wanneer deze wordt weer gegeven, samen met DefaultValue. In dit veld wordt het gedrag van een selectie vakje opgegeven. Dit kan net als DefaultValue expliciet worden opgegeven of gebonden zijn aan de gegevens van de server.

- **Tekst**: dit is een optioneel kenmerk van het type teken reeks. De tekst wordt aan de rechter kant van het selectie vakje weer gegeven. U kunt deze eigenschap gebruiken om tekst op te geven die meer informatie geeft aan de eind gebruiker.

**Gebeurtenissen**:

- **CheckedChanged**: wanneer het selectie vakje de status wijzigt, wordt deze gebeurtenis verzonden.

**Voorbeeld**:

In het volgende voor beeld wordt een aangepaste binding gemaakt tussen het aangepaste resource type en het kenmerk **IsConfigurationType**. Het XML-bestand wordt gebruikt in de RCDC van een aangepast resource type.

![Besturings element UocCheckBox](media/rcd-configuration-xml-reference/image022.png)

Het volgende code segment produceert een **dynamisch selectie** vakje, zoals in de vorige afbeelding het selectie vakje dynamisch wordt weer gegeven. Dit type binding is veelzijdiger en nuttiger dan een expliciet selectie vakje. Het kenmerk moet behoren tot het huidige bron type.

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

**Naam**: UocCommonMultiValueControl

**Beschrijving**: dit is een besturings element voor tekst vakken met meerdere regels dat speciale teken reeks opmaak ondersteunt. Elke waarde tussen de vermeldingen met meerdere waarden wordt van elkaar gescheiden door een punt komma (;) of een regel afbreek teken in het tekstvak. U kunt dit besturings element het beste binden met gegevens van de typen met meerdere waarden, korte teken reeksen en gehele getallen. Dit besturings element ondersteunt zowel de modus alleen-lezen als de bij te werken modus.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **Data type**: dit is een vereist kenmerk van het type String. U kunt deze waarde opgeven als een **teken reeks, een geheel getal** of een **DateTime** -type. U kunt het kenmerk ook binden met de eigenschap **Data type** van het schema kenmerk. Een verwijzing naar meerdere waarden moet worden afgehandeld door **UOCListView** of **UOCIdentityPicker**. Een Booleaanse waarde met meerdere waarden is geen ondersteund gegevens type.

- **Rows**: dit is een optioneel, geheel getal-type kenmerk. U kunt de hoogte van het vak definiëren in het aantal tekens. De standaard waarde is ingesteld op 1.

- **Columns**: dit is een optioneel kenmerk van het type integer. U kunt definiëren hoeveel breed het vak is in het aantal tekens. De standaard waarde is ingesteld op 20.

- **Waarde**: dit is een optioneel kenmerk van het type teken reeks. U kunt dit kenmerk alleen binden met de gegevens bron.

**Gebeurtenissen**:

- **ValueListChanged**: deze gebeurtenis wordt geactiveerd wanneer de huidige waarde in het besturings element wordt gewijzigd.

**Voorbeeld:**

In het volgende voor beeld wordt een teken reeks kenmerk met meerdere waarden met de naam **AMultiValueString** gemaakt en gekoppeld aan het aangepaste resource type. Dit voor beeld werkt pas nadat deze binding is gemaakt.

![Besturings element UocCommonMultiValueControl](media/rcd-configuration-xml-reference/image024.jpg)

Met het volgende code segment wordt een **UocCommonMultiValueControl** -besturings element gegenereerd:

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

**Beschrijving**: dit is vergelijkbaar met een besturings element voor een tekstvak, maar de **Beschrijving** accepteert alleen een bepaalde indeling. In de modus alleen-lezen wordt het weer gegeven als een label. Zie de eigenschap **date time format** in deze sectie voor de indeling van de invoer teken reeks die wordt ondersteund.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **Date time format**: dit is een optioneel kenmerk van het type String. De ondersteunde indelingen zijn **DateTime** en **DateOnly**. De standaard waarde is ingesteld op de **datum/tijd** -indeling.

  - **Datum tijd**: het kenmerk is opgemaakt als mm/dd/jjjj uu: mm: SS am.
  - **DateOnly**: het kenmerk wordt opgemaakt als mm/dd/jjjj.

    >[!NOTE]
    >De notaties **DateTime** en **DateOnly** worden ondersteund, ongeacht de gebruiker die het verschil opgeeft.
    >

- **Waarde**: dit is een optioneel kenmerk van het type teken reeks. U koppelt dit kenmerk aan een bron gegevens bron. De waarde van dit kenmerk moet voldoen aan de juiste datum/tijd notatie.

**Gebeurtenissen**:

- **DateTimeChanged**: de gebeurtenis treedt op wanneer de waarde voor datum/tijd wordt gewijzigd.

**Voorbeeld**:

![Besturings element UocDateTimeControl](media/rcd-configuration-xml-reference/image027.jpg)

Het volgende code segment produceert het eerste **DateTime** -besturings element.

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

Het volgende code segment produceert het tweede **DateTime** -besturings element. Als u de voorbeeld code hebt gebruikt in de sectie gegevens bronnen, is het kenmerk **ExpirationTime** gebonden aan alle resource typen. Daarom kunt u het gebruiken met de volgende code:

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

**Beschrijving**: dit is een eenvoudig vervolg keuzelijst besturings element. Dit besturings element wordt gebruikt om opties uit een gedefinieerde set opties te selecteren. Gegevens typen van string, integer, DateTime en Boolean zijn goede kandidaten voor dit besturings element.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **ValuePath**: de eigenschap voor het ophalen van het kenmerk value van item source instelt. Wanneer item source instelt is opgegeven als aangepast, wordt het pad naar de waarde ingesteld op waarde. Het wordt gebonden aan het veld waarde van het element Option, zoals beschreven in deze sectie.

- **CaptionPath**: de eigenschap voor het ophalen van het kenmerk value van item source instelt. Wanneer item source instelt is opgegeven als aangepast, wordt het pad naar de waarde ingesteld op Caption. Het wordt gebonden aan het bijschrift veld van het optie-element, zoals beschreven in deze sectie.

- **HintPath**: de eigenschap voor het ophalen van het kenmerk value van item source instelt. Wanneer item source instelt is opgegeven als aangepast, wordt het pad naar de waarde ingesteld op Hint. Het wordt gebonden aan het Hint veld van het optie-element, zoals beschreven in deze sectie.

- **Item source instelt**: een verzameling ListControlItems die de keuzen in de lijst definieert. De gebruiker kan dit expliciet instellen op aangepast en het optie-element gebruiken, zoals beschreven in deze sectie, om de teken reeks waarde op te geven.

- **SelectedValue**: de waarde die momenteel is geselecteerd. Dit is een vereiste eigenschap van het type String. Deze eigenschap is gebonden aan teken reeks gegevens uit de gegevens bron.

**Gebeurtenissen**:

- **SelectedIndexChanged**: de gebeurtenis treedt op wanneer de selectie in de vervolg keuzelijst verandert.

**Opties**:

Zie <a href="#options-element">Opties element</a>voor de structuur van een **Options** -element.

- **Waarde**: de waarde van één element Options kan worden ingesteld op een wille keurige teken reeks die de geldige invoer is van de gegevens bron waaraan het besturings element is gekoppeld.

- **Bijschrift**: bijschrift kan een wille keurige teken reeks waarde zijn.

- **Hint**: hint kan een wille keurige teken reeks waarde zijn.

**Voorbeeld**:

![Besturings element UocDropDownList](media/rcd-configuration-xml-reference/image030.jpg)


![Opties in een UocDropDownList-besturings element](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
>Als u het voor beeld wilt laten werken, moet u een bestaand **bereik** van het type teken reeks kenmerken binden met het aangepaste resource type waarop de RCDC van toepassing is.


Met het volgende code segment wordt een vervolg keuzelijst gegenereerd:

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

**Naam**: UocFileDownload

**Beschrijving**: dit besturings element bevat een Hyper link. Wanneer op de Hyper link wordt geklikt, wordt de pagina Windows-bestand opslaan weer gegeven. De gebruiker kan het bestand opslaan op de lokale schijf. De optie openen wordt ook ondersteund als Internet Explorer de bestands indeling kan weer geven. De aanbevolen gegevens typen voor het gebruik van dit besturings element met zijn opgemaakte teken reeks (XML) en binaire typen.

>[!NOTE]
>In deze versie van Microsoft Identity Manager 2016 SP1 moet de gebruiker het Internet Explorer-venster sluiten waarin het bestand is geopend en de pagina vervolgens vernieuwen. Nadat het venster van Internet Explorer is vernieuwd, kan de gebruiker de down load starten om het bestand opnieuw op te slaan of te openen in het oorspronkelijke venster.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **Tekst**: dit is een optioneel kenmerk van het type teken reeks waarmee de hyperlink tekst wordt gedefinieerd. De gebruiker kan een expliciete teken reeks opgeven voor deze eigenschap.

- **Waarde**: dit is een vereist kenmerk. Hiermee geeft u de kenmerk binding op de server waarvan de inhoud moet worden gedownload.

- **PromptedFileName**: dit is een optioneel kenmerk van het type String. Dit is de bestands naam die voor de gebruiker wordt voorgesteld wanneer het gedownloade bestand wordt opgeslagen.

- **Content type**: dit is een vereist kenmerk van het type String. Dit is het bestands type waarin de gegevens worden opgeslagen. Tekst of binair zijn de twee ondersteunde teken reeks opties. Als het tekst is, wordt de geretourneerde waarde beschouwd als een lange teken reeks. Anders wordt de geretourneerde waarde voor binair beschouwd als byte []. Als tekst is geselecteerd, kan de gebruiker, als optie, een achtervoegsel toevoegen om het type indeling op te geven waarin de tekst zich bevindt. Zo is tekst/XML geldig.

>[!NOTE]
>Wanneer de waarde die aan dit besturings element is gekoppeld, leeg is, ontbreekt het besturings element de Hyper link die moet worden gebruikt om de download actie te activeren. Dit komt doordat er niets kan worden gedownload.

**Gebeurtenissen**:

- Er zijn geen gebeurtenissen voor dit besturings element.

**Voorbeeld**:

![Besturings element UocFileDownload](media/rcd-configuration-xml-reference/image035.png)

>[!NOTE]
>Voordat u dit voorbeeld bestand uploadt, moet de gebruiker een binding maken tussen een aangepast resource type en het bestaande ConfigurationData-kenmerk.

Het volgende code segment genereert een besturings element voor het downloaden van bestanden:

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

**Beschrijving**: dit besturings element bevat een tekstvak waarin de locatie wordt weer gegeven van het lokale bestand dat moet worden geüpload, een knop voor een Blader bestand en een knop uploaden. Wanneer de eind gebruiker op een knop Bladeren klikt, wordt het venster Windows geopend. De eind gebruiker kan één bestand op het lokale station selecteren dat u wilt uploaden. Wanneer het bestand is geselecteerd, wordt de locatie van het bestand weer gegeven in het tekstvak. Wanneer u op de knop Uploaden klikt, wordt het bestand geüpload naar de lokale gegevens bron aan de client zijde. De bestands inhoud is nog niet verzonden naar de server. De aanbevolen gegevens typen voor het gebruik van dit besturings element met zijn de volgende: opgemaakte teken reeks (XML) of binaire typen.

>[!NOTE]
>Er is geen indicatie van de voortgang of status van het uploaden. Wanneer het bestand wordt geüpload naar de lokale gegevens bron, wordt het tekstvak gewist.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **Waarde**: dit is een vereist kenmerk. Hiermee wordt het schema kenmerk binding opgegeven op de server waarnaar de gegevens worden geüpload.

- **Content type**: dit is een optioneel kenmerk van het type teken reeks. Dit is het gegevens type waarop het bestand is opgeslagen op de server. Dit kan worden ingesteld op text of binary. Wanneer de eigenschap ontbreekt, is de standaard waarde binair.

- **MaxFileSize**: dit is een optioneel kenmerk van het type teken reeks. MaxFileSize definieert hoe groot de geüploade bestands grootte kan zijn. Als de eigenschap ontbreekt, is de maximale grootte standaard 1 Mega byte (MB).

- **PromptedForNoValue**: dit is een optioneel kenmerk van het type String. Hiermee wordt de tekst gedefinieerd die wordt weer gegeven aan de gebruiker wanneer er geen bestand wordt geüpload.

**Gebeurtenissen**:

- **Fileupload**: deze gebeurtenis wordt verzonden wanneer het bestand is geüpload.

**Voorbeeld**:

![Besturings element UocFileUpload](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
>Als u de volgende voorbeeld code wilt gebruiken, moet u een nieuw kenmerk binary-type met de naam ABinaryAttribute maken en vervolgens een nieuwe binding maken tussen een aangepast resource type en dit kenmerk.

Het volgende code segment genereert een upload besturings element:
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

**Naam**: UocFilterBuilder

**Beschrijving**: dit is een complex besturings element waarmee de gebruiker een MIM 2016 XPath-expressie kan weer geven. Sommige XPath-expressies worden niet ondersteund. Zie de Help voor de filter functie voor meer informatie over het gebruik van de opbouw functie voor filters.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **PermittedObjectTypes**: Hiermee definieert u een lijst met resource typen die moeten worden weer gegeven in de instructie SELECT van een filter Builder. Zie de Help van filter Builder voor meer informatie over het gebruik van de filter functie voor filters. De teken reeks heeft de indeling ResourceTypeA, ResourceTypeB, waarbij elk resource type wordt gescheiden door een komma ', '.

- **Waarde**: dit is de waarde waarmee de filter functie wordt gerenderd. Alleen een binding met gegevens van het type teken reeks die een XPath-expressie bevat, wordt ondersteund. Het filter kenmerk is een aanbevolen kenmerk voor het binden van dit besturings element.

- **PreviewButtonVisible**: dit is een optionele, Booleaanse-type eigenschap. Wanneer deze eigenschap is ingesteld op False, ziet de gebruiker geen knop Preview. De standaard waarde is ingesteld op True. Deze knop kan worden gebruikt in combi natie met een besturings element lijst-weer gave om de resultaten van een XPath-expressie te bekijken.

- **ExcludeGroupMembership**: dit is een Booleaanse eigenschap. Als deze eigenschap is ingesteld op True, kunt u geen filter maken dat gebruikmaakt \<Reference Attribute\> van (bijvoorbeeld ResourceID) is lid van \<Group object\> . Met andere woorden, wanneer deze eigenschap is ingesteld op True, kunt u geen filter maken dat de map groepslid maatschap gebruikt.

- **PreviewButtonCaption**: dit is een optionele teken reeks. Wanneer PreviewButtonVisible is ingesteld op True, kunt u deze eigenschap gebruiken om de knop een aangepaste tekst te geven. De tekst wordt weer gegeven op de knop Preview.

**Gebeurtenissen**:

- **OnFilterChanged**: deze gebeurtenis wordt geactiveerd wanneer de inhoud van de filter Builder wordt gewijzigd.

**Voorbeeld**:

![Besturings element UocFilterBuilder](media/rcd-configuration-xml-reference/image044.png)

De volgende voorbeeld code bevat een UOCLabel-besturings element, een eenvoudige filter functie met PermittedObjectTypes en een preview-lijst weergave. Wijs de eigenschap ListFilter van de lijst weergave en de eigenschap waarde filter Builder naar hetzelfde gegevens bron kenmerk om de twee te koppelen.

>[!NOTE]
>Voordat u deze voorbeeld code gebruikt, maakt u een nieuwe binding tussen een bestaand filter kenmerk en een aangepast resource type.

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


### <a name="uochtmlsummary"></a>UocHtmlSummary

**Naam**: UocHtmlSummary

**Beschrijving**: u kunt dit besturings element gebruiken om een overzichts pagina te definiëren in een RCDC-pagina. Deze overzichts pagina wordt weer gegeven nadat de eind gebruiker een aanvraag indient. Dit besturings element kan alleen worden gebruikt in een groepering samen vatting en het moet het enige besturings element zijn. We raden u ten zeerste aan de voorbeeld code te gebruiken die wordt vermeld.

>[!NOTE]
>Dit besturings element is niet uitgebreid getest.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **ModificationsXml**: deze eigenschap moet zijn opgemaakt als {binding source = Delta, Path = DeltaXml}, waarbij Delta is gedefinieerd in de configuratie header ObjectDataSource.

- **TransformXsl**: deze eigenschap is ingedeeld als {binding source = SummaryTransformXsl, Path =/}, waarbij summaryTransformXsl is gedefinieerd in de configuratie header XmlDataSource.

**Gebeurtenissen**:

- Er zijn geen gebeurtenissen voor dit besturings element.

**Voorbeeld**:

Voor een voor beeld van dit besturings element, zie het voor beeld voor een samen vatting van groepen in de sectie Groepeer element van dit document.


### <a name="uochyperlink"></a>UocHyperLink

**Naam**: UocHyperLink

**Beschrijving**: dit is een eenvoudig besturings element voor Hyper links. U kunt dit besturings element gebruiken om informatie weer te geven als een Hyper link.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **ObjectReference**: dit is een optionele eigenschap van het type verwijzing. Als naar een geldige resource wordt verwezen door de GUID die is gedefinieerd in deze eigenschap, biedt de Hyper Link de eind gebruiker een manier om toegang te krijgen tot de resource. Deze sluiten elkaar wederzijds uit met de eigenschap NavigateUrl.

- **Tekst**: dit is een optionele eigenschap van het type teken reeks. U kunt deze eigenschap gebruiken om de tekst te definiëren die wordt weer gegeven als de Hyper link.

- **NavigateUrl**: dit is een optionele eigenschap van het type teken reeks. Met deze eigenschap kunt u de URL voor het volledige pad definiëren waarnaar de Hyper link verwijst. Dit is wederzijds exclusief met de eigenschap ObjectReference.

**Gebeurtenissen**:

- Er zijn geen gebeurtenissen voor dit besturings element.

**Voorbeeld**:

![Besturings element UocHyperLink](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
>U hebt een geldige GUID van een resource nodig om dit te koppelen aan. In dit geval wordt de tweede Hyper link gegenereerd met een geldige GUID. De eerste kan een wille keurige website zijn.

In het volgende code segment wordt een omleidings hyperlink gegenereerd:

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

Het volgende code segment genereert een Hyper link die verwijst naar een resource. De expliciete verwijzing kan worden vervangen door de expressie {binding source = object, Path = Creator} om dit te binden met een gegevens bron. Dit kan alleen geldig zijn wanneer de Manager van de resource bestaat en de waarde van het verwijzings type is.

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

**Naam**: UocIdentityPicker

**Beschrijving**: dit besturings element bestaat uit een optioneel vak voor omzetten en een Blader venster. Het optionele vak voor de oplossing bestaat uit een optioneel tekstvak voor het invoeren van de identiteit, een knop voor het oplossen van de identiteit en een Blader knop om een pop-upvenster te openen. In het venster bladeren kunnen gebruikers identiteiten selecteren via een besturings element lijst-weer gave. De geselecteerde identiteit uit het venster Bladeren wordt weer gegeven in het vak oplossen.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **UsageKeywords**: dit is een optionele teken reeks eigenschap. U kunt een lijst met zoekbereiken definiëren die moeten worden gebruikt in de resource kiezer door een lijst op te geven met de gebruiks trefwoorden die worden ondersteund door de SearchScopeConfiguration-structuur, waarbij elk sleutel woord wordt gescheiden door een apostrof (').

- **Filter**: dit is een optionele teken reeks eigenschap. De gebruiker geeft een XPath-expressie om de resource kiezer te bereiken om alleen de items weer te geven die binnen een gedefinieerd bereik passen. Deze eigenschap is wederzijds exclusief met de eigenschap UsageKeywords. Wanneer het zoek bereik wordt toegepast, heeft deze eigenschap geen effect.

- **ResultObjectType**: dit is een optionele teken reeks eigenschap. Het bron type wordt gebruikt om resources weer te geven in de lijst met pop-updialoogvensters. Dit wordt gebruikt in combi natie met het filter om te helpen bij het identificeren van de identiteits kiezer welk resource type wordt geretourneerd door het filter en de gegevens dienovereenkomstig te renderen. Deze eigenschap is wederzijds exclusief met de eigenschap UsageKeywords. Wanneer het zoek bereik wordt toegepast, heeft dit geen effect. De teken reeks die voor deze eigenschap wordt geaccepteerd, is een wille keurige, geldige resource-type naam, bijvoorbeeld persoon. Wanneer het filter naar verwachting meerdere resource typen retourneert, wordt de resource gebruikt.

- **PreviewTitle**: dit is de preview-titel die wordt gebruikt in een lijst weergave. Zie de sectie UocListView voor meer informatie over deze eigenschap.

- **ListViewTitle**: dit is een optionele teken reeks eigenschap. U kunt deze eigenschap gebruiken om de tekst die boven op de lijst weergave wordt weer gegeven als titel te definiëren.

- **Waarde**: dit is een optionele teken reeks eigenschap. U kunt dit het beste binden met een schema kenmerk om de waarde te verbinden met een gegevens bron.

- **Modus**: dit is een optionele teken reeks eigenschap. U gebruikt deze eigenschap om te definiëren of een waarde kan worden geselecteerd door de identiteits kiezer of meerdere identiteiten kan worden geselecteerd. SingleResult en MultipleResult zijn de toegestane waarden. Standaard is deze ingesteld op SingleResult.

- **Object** typen: dit is een optionele eigenschap van het type teken reeks. U kunt een lijst met resource typen definiëren waarmee de eind gebruiker vermeldingen kan oplossen in het vak identiteits kiezer oplossen. De lijst bevat een lijst met resource-type namen gescheiden door een komma ' ', '.

- **AttributesToSearch**: dit is een optionele eigenschap van het type String. U kunt een lijst met kenmerken definiëren die moeten worden gebruikt voor het omzetten van het item in de identiteits kiezer, waarbij de lijst een lijst is met schema kenmerken, gescheiden door een komma ', '. Als AttributesToSearch bijvoorbeeld is ingesteld op `DisplayName, Alias` , kan de gebruiker de items zoeken met `DisplayName = \<search value\>` of `Alias=\<search value\>` . Kenmerk namen die hier worden ingevoerd, moeten geldige kenmerken zijn voor doel resource typen van de gegevens bron die is opgegeven in de eigenschap Value. De doel resource typen vindt u in het veld object type. Alle kenmerken moeten geldig zijn voor de resource typen die worden vermeld in het veld object types.

- **ColumnsToDisplay**: dit is een optionele eigenschap van het type String. De gebruiker geeft een lijst met namen van schema kenmerken, gescheiden door een komma ', '. De kenmerken die hier worden gedefinieerd, vormen de kolom van de lijst weergave in de identiteits kiezer.

- **Rows**: dit is een optionele, integer-eigenschap. Het werkt alleen als de modus is ingesteld op MultipleResult. Gebruik deze eigenschap om de hoogte van het tekstvak voor omzetten in te stellen op een bepaalde grootte in teken eenheden.

- **MainSearchScreenText**: dit is een optionele eigenschap van het type String. Dit is de aangepaste tekst die wordt weer gegeven terwijl de zoek opdracht wordt uitgevoerd in het venster Bladeren.

**Gebeurtenissen**:

- **SelectedObjectChanged**: deze gebeurtenis wordt verzonden wanneer de gebruiker de geselecteerde resources wijzigt.

**Voorbeeld**:

![UocIdentityPicker-besturings element in SingleResult-modus](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
>Dit voor beeld werkt alleen als u een nieuwe binding maakt tussen het kenmerk Manager en een aangepast resource type waarop deze XML van toepassing is.

Met het volgende code segment wordt een identiteits kiezer gegenereerd in de SingleResult-modus met behulp van de eigenschappen Filter en ResultObjectType als onderdeel van de RCDC:

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

In de volgende afbeelding ziet u een identiteits kiezer in de modus MultipleResult:

![UocIdentityPicker-besturings element in MultipleResult-modus](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
>Als u deze voorbeeld code wilt gebruiken, moet u het kenmerk ExplicitMember (een verwijzing naar meerdere waarden) binden aan het aangepaste resource type. Zoek bereiken maken waarvan de eigenschap UsageKeyword is ingesteld op persoon en groep.

Met het volgende code segment wordt een identiteits kiezer gemaakt in de MultipleResult-modus:

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

**Naam**: UocLabel

**Beschrijving**: dit is een eenvoudig, alleen-lezen besturings element voor tekst label. U wordt aangeraden dit besturings element te gebruiken om alleen-lezen gegevens weer te geven.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **Tekst**: dit is een kenmerk van het type String. U definieert deze eigenschap door een expliciete teken reeks waarde op te geven of door deze te binden aan een gegevens bron. Een voor beeld-binding waarmee de waarde van deze eigenschap wordt toegewezen, is {binding source = object, Path = \<valid attribute name\> .

Zie voor een voor beeld van het besturings element UocLabel eenvoudige controle in de sectie voor beelden van eenvoudige besturings elementen.


### <a name="uoclistview"></a>UocListView

**Naam**: UocListView

**Beschrijving**: dit is een geavanceerd lijst-Weergave besturings element. Het bestaat uit een eenvoudige lijst weergave, een optionele eenvoudige zoek opdracht, een optioneel Geavanceerd zoek element, een optioneel selectie vakje voor selecties en een actie knop balk. De optionele eenvoudige zoek opdracht bestaat uit een zoek bereik en een eenvoudig zoek tekstvak. Het besturings element Geavanceerd zoeken is een filter functie voor filters. In de lijst weergave ziet u een voor beeld van een lijst met resources. Het kan ook Zoek resultaten weer geven die afkomstig zijn uit de zoek besturings elementen in dit besturings element. De actie knop balk definieert welke actie kan worden uitgevoerd op basis van de selectie in de lijst weergave. In het vak selectie voorbeeld ziet u welke items zijn geselecteerd in de lijst weergave.

>[!IMPORTANT]
>UocListView werkt niet met referentie kenmerken met één waarde. Deze kan alleen worden gebruikt met referentie kenmerken met meerdere waarden. Zie UocIdentityPicker in dit document voor referentie kenmerken met één waarde.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **SelectedValue**: dit is een optionele eigenschap van het type teken reeks die is gekoppeld aan een verwijzings kenmerk met meerdere waarden en die een lijst met door de GUID geformatteerde teken reeksen accepteert.

- **PageSize**: dit is een optionele integer-eigenschap. De gebruiker kan opgeven hoeveel items op één pagina in een besturings element lijst weergave passen. De standaard waarde is 10 vermeldingen. Een positief geheel getal is geldig.

- **UsageKeyword**: dit is een optionele eigenschap van het type String. De gebruiker kan een lijst met tref woorden opgeven die definiëren welk zoek bereik wordt gebruikt in het besturings element voor het zoeken in de lijst. Er zijn zoek bereik bronnen in de FIM 2010-server. Het kenmerk in een SearchScopeConfiguration-structuur, genaamd UsageKeyword, wordt gebruikt om een set zoek bereik te groeperen. De lijst weergave gebruikt de lijst met tref woorden. Elk tref woord wordt gescheiden door een komma (,). Dit is het sleutel woord Usage dat wordt gebruikt voor het bijbehorende zoek bereik dat u wilt weer geven in deze lijst weergave. Dit is alleen van toepassing wanneer de eigenschap ShowSearchControl is ingesteld op True.

- **SearchControlAutoPostback**: dit is een optionele Booleaanse eigenschap. Stel de waarde van deze eigenschap in op True als u auto post back wilt uitvoeren wanneer een zoek opdracht wordt geactiveerd. SearchControlAutoPostback is standaard ingesteld op false.

- **EmptyResultText**: dit is een optionele eigenschap van het type String. Standaard is deze ingesteld op geen items, maar dit kan worden ingesteld op een wille keurige teken reeks waarde. Deze tekst wordt weer gegeven wanneer een Zoek resultaat leeg is.

- **ButtonHeight**: dit is een optionele eigenschap van het type integer. Stel de waarde van deze eigenschap in op een wille keurige positieve integerwaarde. Met deze eigenschap wordt de hoogte van knoppen in de actie balk in pixels gedefinieerd. De standaard waarde is 32 pixels.

- **ButtonWidth**: dit is een optionele eigenschap van het type integer. Stel de waarde van deze eigenschap in op een wille keurige positieve integerwaarde. Met deze eigenschap wordt de breedte van knoppen in de actie balk in pixels gedefinieerd. De standaard waarde is 32 pixels.

- **CaptionImageMaxHeight**: dit is een optionele eigenschap van het type integer. Stel de waarde van deze eigenschap in op een wille keurig positief geheel getal. Met deze eigenschap wordt de maximum hoogte van het tijdelijke bijschrift gedefinieerd. De standaard waarde is 32 pixels.

- **CaptionImageMaxWidth**: dit is een optionele eigenschap van het type integer. Stel de waarde van deze eigenschap in op een wille keurig positief geheel getal. Met deze eigenschap wordt de maximale pictogram breedte van een bijschrift gedefinieerd. De standaard waarde is 32 pixels.

- **CaptionImageUrl**: dit is een optionele eigenschap van het type String. Deze eigenschap definieert een URL die is gekoppeld aan een afbeelding die wordt weer gegeven als de titel afbeelding.

- **PreviewTitle**: dit is een optionele eigenschap van het type String. U kunt deze eigenschap gebruiken om de tekst te definiëren die boven op het selectie voorbeeld venster wordt weer gegeven.

- **EnableSelection**: dit is een optionele, Booleaanse-type eigenschap. U gebruikt deze eigenschap om te definiëren of een lijst weergave in de selectie modus is. Als een lijst weergave in de selectie modus is, wordt een kolom met selectie vakjes weer gegeven in de kolom uiterst links van de lijst weergave en wordt er onder in de lijst een selectie voorbeeld venster weer gegeven. De standaard waarde van deze eigenschap is ingesteld op True.

- **SingleSelection**: dit is een optionele, Booleaanse-type eigenschap. Als de selectie modus is ingeschakeld voor de lijst weergave en u deze waarde instelt op True, wordt de eind gebruiker beperkt om slechts één item uit de lijst te selecteren. De waarde van deze eigenschap is standaard ingesteld op ONWAAR. Dit betekent dat de eind gebruiker in de lijst standaard meerdere items kan selecteren.

- **RedirectUrl**: dit is een optionele eigenschap van het type String. Gebruik deze eigenschap om een pagina op te geven waarnaar moet worden omgeleid wanneer een item in de lijst met Hyper links wordt geklikt. Deze URL kan tijdelijke aanduidingen bevatten die worden vervangen door de werkelijke waarde tijdens runtime. De tijdelijke aanduidingen zijn als volgt:

    - {0} Later
    - {1} Id
    - {2} displayName

- **ShowTitleBar**: dit is een optionele, Booleaanse-type eigenschap. Gebruik deze eigenschap om op te geven of de titel balk zichtbaar moet zijn. De standaard waarde van deze eigenschap is onwaar.

- **ShowActionBar**: dit is een optionele, Booleaanse-type eigenschap. Gebruik deze eigenschap om op te geven of het actie balk gebied zichtbaar moet zijn. De standaard waarde van deze eigenschap is waar.

- **ShowPreview**: dit is een optionele, Booleaanse-type eigenschap. Gebruik deze eigenschap om op te geven of het voorvertonings gebied zichtbaar moet zijn. De standaard waarde van deze eigenschap is waar.

- **ShowSearchControl**: dit is een optionele, Booleaanse-type eigenschap. Met deze eigenschap geeft u op of het besturings element voor zoeken moet worden weer gegeven. De standaard waarde van deze eigenschap is waar.

- **ResultObjectType**: dit is een optionele eigenschap van het type String. Gebruik deze eigenschap om het verwachte object type van de zoek resultaten op te geven. De standaard waarde van deze eigenschap is resource. Als het Zoek resultaat meerdere bron typen bevat, moet deze waarde als resource worden opgegeven.

- **ColumnsToDisplay**: dit is een optionele eigenschap. Gebruik deze eigenschap om op te geven welke kenmerken u wilt weer geven in de lijst weergave als kolommen. De standaard waarde van deze eigenschap is DisplayName, resource type. Elke kolom wordt vertegenwoordigd door de systeem naam van een kenmerk. Elke kolom wordt gescheiden door een komma ', '. U hoeft geen waarde op te geven voor deze eigenschap wanneer de lijst weergave wordt gebruikt in de selectie modus. In de selectie modus is de kolom instelling afkomstig van het kenmerk SearchScopeColumn van het zoek bereik dat momenteel is geselecteerd.

- **ListFilter**: dit is een optionele eigenschap van het type String. Dit is het XPath dat wordt gebruikt om de lijst weergave weer te geven. Dit is alleen van toepassing wanneer de eigenschap ShowSearchControl is ingesteld op false. Wanneer deze waarde is opgegeven, gebruikt de lijst weergave deze eigenschaps waarde voor query's en de lijst weergave niet in de selectie modus. Het filter kan worden gebonden aan een teken reeks kenmerk van de resource:

    `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>`

    of een teken reeks zijn die een bepaalde vooraf gedefinieerde omgevings variabele bevat:

    `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

- **TargetAttribute**: dit is een verouderde eigenschap. De waarde moet de systeem naam zijn van een kenmerk met meerdere waarden waarnaar wordt verwezen. U wordt aangeraden deze eigenschap niet meer te gebruiken. In groeps beheer in plaats van:

    `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>`

    Gebruiken

    `<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>`

- **ItemClickBehavior**: dit is een optionele eigenschap van het type String. Met deze eigenschap geeft u op of u wilt dat het item klikken op een lijst weergave een server terugzet of een detail weergave van het item weergeeft. Er worden twee optie waarden ondersteund: ModelessDialog en server. De standaard waarde is ModelessDialog.

- **SearchOnLoad**: dit is een optionele, Booleaanse-type eigenschap die aangeeft of het besturings element van de lijst moet worden doorzocht op belasting. Deze eigenschap is alleen van toepassing als de lijst weergave de selectie modus heeft. De standaard waarde voor deze eigenschap is waar. U kunt deze uitschakelen als u verwacht dat de gebruiker meestal tekst in de zoek opdracht typt om een zinvol resultaat te krijgen. In dit geval toont de lijst weergave eerst een bericht om de gebruiker te laten weten hoe een zoek opdracht moet worden uitgevoerd. De tekst kan worden aangepast door de volgende eigenschappen:

- **MainSearchScreenText**: deze optionele, String-type-eigenschap is alleen van toepassing als SearchOnload is ingesteld op True. Deze eigenschap kan worden gebruikt om tekst aan te passen die in het midden van de lijst weergave wordt weer gegeven wanneer de lijst weergave niet automatisch wordt doorzocht. De standaard waarde voor deze eigenschap is het vinden van de bronnen met behulp van de zoek opdracht, zoals eerder is beschreven. U kunt een waarde opgeven om de tekst relevanter te maken voor uw scenario.

- **SubSearchScreenText**: deze optionele, String-type-eigenschap wordt gebruikt voor het aanpassen van de tekst die wordt weer gegeven na de eigenschap **MainSearchScreenText** . Normaal gesp roken hoeft u geen waarde op te geven voor deze eigenschap, tenzij u een extra instructie wilt toevoegen over het gebruik van de lijst weergave.

**Gebeurtenissen**:

- Er zijn geen gebeurtenissen voor dit besturings element.

**Voorbeeld**:

Zie voor voor beelden van het gebruik van de lijst weergave samen met het besturings element UocFilterBuilder als een preview-lijst de UocFilterBuilder-voor beelden eerder in dit document. De UocListView kan ook worden gebruikt zonder de filter functie Builder.


### <a name="uocnumericbox"></a>UocNumericBox

**Naam**: UocNumericBox

**Beschrijving**: dit is een eenvoudig tekstvak waarin alleen gehele waarden worden gebruikt. Dit besturings element ondersteunt zowel de modus alleen-lezen als de bij te werken modus.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **MaxValue**: dit is een optionele, integer-type-eigenschap. Gebruik deze eigenschap om een validatie aan de client zijde voor het besturings element te definiëren. De waarde die de eind gebruiker invoert, mag deze waarde niet overschrijden. U kunt een expliciet geheel getal invoeren of dit binden met gehele gegevens uit een gegevens bron met behulp van {binding source = schema, Path = IntegerMaximum}.

- **MinValue**: dit is een optionele, integer-type-eigenschap. Gebruik deze eigenschap om een validatie aan de client zijde voor het besturings element te definiëren. De waarde die de eind gebruiker invoert, mag niet lager zijn dan deze waarde. U kunt een expliciet geheel getal invoeren of dit binden met gehele gegevens uit een gegevens bron met behulp van {binding source = schema, Path = IntegerMinimum}.

- **DefaultValue**: dit is een optionele, integer-type-eigenschap. Gebruik deze eigenschap om een standaard waarde voor het besturings element te definiëren als het besturings element wordt gebruikt om nieuwe gegevens te maken. Deze waarde kan alleen expliciet worden ingesteld op een statisch geheel getal.

- **Waarde**: dit is een optionele eigenschap van het type integer. Wanneer u dit koppelt aan een gegevens bron met een geheel getal, wordt de waarde van dat kenmerk weer gegeven wanneer de pagina wordt geladen en vervolgens wordt deze opgeslagen in de gegevens bron nadat deze is verzonden.

**Gebeurtenissen**:

- **TextChanged**: deze gebeurtenis wordt verzonden als de huidige waarde in het besturings element wordt gewijzigd.

**Voorbeeld**:

![Besturings element UocNumericBox](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
>Met de volgende voorbeeld code wordt het eerste numerieke vak gegenereerd. Het numerieke vak is niet verbonden met een gegevens bron of schema-informatie.

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

Met de volgende voorbeeld code wordt het tweede numerieke vak gegenereerd.

>[!NOTE]
>Dit voor beeld werkt alleen als u eerst een nieuw, integer-type kenmerk met de naam AnIntegerAttribute maakt en dit verbindt met het aangepaste resource type.

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

**Naam**: UocPictureBox

**Beschrijving**: dit besturings element wordt gebruikt voor het weer geven van de gegevens van een binair type. U wordt aangeraden dit besturings element te gebruiken met gegevens van een binair type. De afbeelding kan worden weer gegeven op basis van een afbeeldings-URL, gegevens van een binair type of de kenmerk bron met gegevens van het type afbeelding.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **ImageUrl**: dit is een optionele eigenschap van het type teken reeks. Voer de URL van de doel afbeelding in.

- **MaxHeight**: dit is een optioneel type teken reeks-eigenschap. Hiermee wordt de maximale hoogte van de afbeelding gedefinieerd die in pixels moet worden weer gegeven.

- **MaxWidth**: dit is een optionele eigenschap van het type String. Hiermee wordt de maximale breedte van de afbeelding gedefinieerd die in pixels moet worden weer gegeven.

- **ImageData**: dit is een binaire eigenschap van het type. Gebruik deze eigenschap om een gegevens bron te binden aan de weer gegeven afbeelding. De gebonden gegevens bron moet van het binaire bestand zijn. U kunt dit veld ook gebruiken om een afbeelding expliciet in te stellen door gegevens op te geven in de indeling byte [].

- **ImageResource**: dit is een optionele eigenschap van het type binary.

- **AlternativeText**: dit is een optionele eigenschap van het type String. Deze eigenschap wordt weer gegeven als alternatieve tekst wanneer de afbeelding niet kan worden weer gegeven.

**Gebeurtenissen**:

- Er zijn geen gebeurtenissen voor dit besturings element.

**Voorbeeld**:

>[!NOTE]
>Als u dit voor beeld wilt gebruiken, moet u een bestaande afbeeldings gegevens binden met het besturings element.

In het volgende code segment wordt een besturings element voor een afbeeldings venster gegenereerd dat een gegevens bron met het besturings element verbindt:

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

In het volgende code segment wordt een besturings element voor een afbeeldings vakje gegenereerd dat een URL-installatie kopie verbindt met het besturings element:

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

**Naam**: UocRadioButtonList

**Beschrijving**: dit is een eenvoudige lijst met keuze rondjes. De opties sluiten elkaar wederzijds uit in deze lijst. Dit besturings element wordt aanbevolen wanneer gebruikers vijf of minder opties kunnen kiezen. Anders wordt UOCListView aanbevolen.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **ValuePath**: de waarde Path is ingesteld op value. Het wordt gebonden aan het veld waarde van het element Option, zoals beschreven in deze sectie.

- **CaptionPath**: het pad naar de waarde is ingesteld op Caption. Het wordt gebonden aan het bijschrift veld van het optie-element, zoals beschreven in deze sectie.

- **HintPath**: het pad naar de waarde is ingesteld op Hint. Het wordt gebonden aan het Hint veld van het optie-element, zoals beschreven in deze sectie.

- **SelectedValue**: de waarde die momenteel is geselecteerd. Dit is een vereiste eigenschap van het type String. Deze eigenschap is gebonden aan de teken reeks gegevens uit de gegevens bron.

**Gebeurtenissen**:

- **SelectedIndexChanged**: de gebeurtenis treedt op wanneer het geselecteerde keuze rondje wordt gewijzigd.

- **CheckedChanged**: wanneer de status van het keuze rondje wordt gewijzigd, wordt deze gebeurtenis verzonden.

**Opties**:

Er kunnen slechts twee **Opties** worden elementen als opties voor dit besturings element. Zie <a href="#options-element">Opties element</a>voor de structuur van een **Options** -element.

- **Waarde**: het waardeveld moet worden ingesteld op True of False (waar).

- **Bijschrift**: dit kan een wille keurige teken reeks waarde zijn.

- **Hint**: dit kan een wille keurige teken reeks waarde zijn.

**Voorbeeld**:

![Besturings element UocRadioButtonList](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
>Als u dit voor beeld wilt gebruiken, moet u een nieuw Boole-kenmerk en ABooleanAttribute maken en dit koppelen aan uw aangepaste resource type.

In het volgende code segment wordt een lijst met keuze rondjes gemaakt:

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

**Naam**: UocSimpleRadioButton

**Beschrijving**: dit is een eenvoudig besturings element, keuze rondje. Het gebruik van dit besturings element is vergelijkbaar met een eenvoudig selectie vakje. Er zijn twee keuze rondjes, met naast de tekst labelen. Het is raadzaam om het besturings element te binden aan gegevens van het type Booleaans.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **True**: dit is een optionele eigenschap van het type String. Dit is de tekst die wordt weer gegeven wanneer het keuze rondje is geselecteerd.

- **Falsetekst**: dit is een optionele eigenschap van het type String. Dit is de tekst die wordt weer gegeven wanneer het keuze rondje niet is geselecteerd.

- **SelectedItem**: dit is een optionele, Booleaanse-type eigenschap. Deze waarde geeft aan dat het keuze rondje is geselecteerd. Dit kan worden gebonden aan gegevens van het type Booleaans van een gegevens bron. De standaard waarde is ingesteld op ONWAAR.

**Gebeurtenissen**:

- **CheckedChanged**: wanneer het keuze rondje de status gewijzigd van geselecteerd in niet-geselecteerd of tegenovergestelde heeft, wordt dit signaal verzonden.

**Voorbeeld**:

![Besturings element UocSimpleRadioButton](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
>Als u het voor beeld wilt laten werken, moet u een nieuw Booleaans kenmerk ABooleanAttribute maken en dit koppelen aan uw aangepaste resource type. De RCDC-gegevens worden toegepast op hetzelfde aangepaste resource type.

In het volgende code segment wordt een keuze rondje gegenereerd:

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

**Naam**: UocTextBox

**Beschrijving**: dit is een eenvoudig tekstvak dat invoer van het teken reeks type ondersteunt. U wordt aangeraden dit besturings element te gebruiken om te binden met gegevens van het type teken reeks.

**Eigenschappen**:

- Alle algemene eigenschappen: Zie <a href="#common-properties">algemene eigenschappen</a>voor meer informatie over deze eigenschappen.

- **MaxLength**: dit is een optioneel, geheel getal-type kenmerk. Met deze eigenschap geeft u de maximale lengte op voor een teken reeks invoer. De standaard waarde voor deze eigenschap is 128 tekens.

- **Tekst**: dit is een optionele eigenschap van het type teken reeks. Dit is de tekst die wordt weer gegeven in het tekstvak. U kunt een expliciete teken reeks definiëren die wordt weer gegeven in het tekstvak tijdens het initiële laden van het besturings element of het binden aan een schema kenmerk van een teken reeks type.

- **Rows**: dit is een optionele eigenschap van het type integer. Met deze eigenschap wordt de hoogte van het tekstvak in teken eenheden gedefinieerd. De standaard waarde is één teken.

- **Columns**: dit is een optionele, integer-type-eigenschap. Met deze eigenschap wordt de breedte van het tekstvak in teken eenheden gedefinieerd. De standaard waarde is 20 tekens.

- **Terugloop**: dit is een optionele, Booleaanse-type eigenschap. Door de waarde van deze eigenschap in te stellen op waar, wordt de functie Tekst terugloop in het tekstvak ingeschakeld. De standaard waarde van deze eigenschap is ingesteld op True.

- **UniquenessValidationXPath**: dit is een optionele eigenschap van het type String. Er wordt een geldige FIM XPath-filter expressie gebruikt en er wordt voor gezorgd dat de waarde invoer door de gebruiker uniek is binnen de resources die binnen het bereik van het filter vallen. Bijvoorbeeld, om ervoor te zorgen dat de door de gebruiker gevraagde weergave naam uniek is binnen alle beveiligings groepen met e-mail functionaliteit in de data base van de FIM-service, gebruikt u het XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’` . De validatie actie wordt uitgevoerd wanneer de gebruiker de pagina verlaat. Deze eigenschap wordt alleen ondersteund in de RCDC voor het maken van een resource.

- **UniquenessErrorMessage**: dit is een optionele eigenschap van het type String. Deze teken reeks wordt gebruikt om een fout bericht weer te geven als de UniquenessValidationXPath-validatie is mislukt, en kan een expliciete tekst of een teken reeks bron variabele zijn. Als deze eigenschap niet is opgegeven, is het standaardfout bericht voor een mislukte validatie% VALUE% al aanwezig. Probeer een ander account. "

**Gebeurtenissen**:

- **TextChanged**: deze gebeurtenis wordt verzonden als de tekst in het tekstvak wordt gewijzigd.

**Voorbeeld**:

Zie de sectie voor beelden van eenvoudige besturings elementen voor een volledig voor beeld van dit besturings element.


<br/>
<h2 id="appendix-a">Bijlage A: standaard-XSD-schema</h2>

In deze sectie wordt het volledige XSD-schema weer gegeven voor alle standaard RCDCs die worden meegeleverd met Microsoft Identity Manager 2016 SP1.

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

<br/>
<h2 id="appendix-b">Bijlage B: standaard-XSL-overzicht</h2>

In deze sectie wordt de volledige samenvattings-XSL weer gegeven die wordt meegeleverd met Microsoft Identity Manager 2016 SP1.

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
