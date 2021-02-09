---
title: Werk stroom gids voor webservice-connector voor de REST API | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een REST API-voor beeld implementeert.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 9b1a65d6604f434d3619ad7964caa8ce202092a0
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835951"
---
# <a name="web-service-connector-workflow-guide-for-a-rest-api-sample"></a>Werk stroom gids voor webservice-connectors voor een REST API-voor beeld

In dit artikel wordt beschreven hoe u een voorbeeld REST API implementeert om het hulp programma voor configuratie van webservices met een REST API-webgegevens bron te door lopen.

## <a name="prerequisites"></a>Vereisten

De volgende vereisten zijn vereist voor het gebruik van het voor beeld:

- Het hulp programma voor configuratie van webservices is geïnstalleerd.
- De voorbeeld service REST data source is geïmplementeerd. Down load en installeer het voor beeld van (zie hier).

<!-- No link provided for "see here" -->
<!-- Should Note go with bullet point #2 -->

>[!NOTE]
>JSON-gegevens moeten één object bevatten met een eigenschap die een matrix bevat.

<!-- Should JSON be exactly as-is or just a sample -->
<!-- Should JSON be part of Note content -->
```JSON
{

"EmployeeList":[

{"id":"1","employee_name":"Albano","employee_salary":"22213","employee_age":"37","profile_image":""},{"id":"2","employee_name":"Albano","employee_salary":"22213","employee_age":"37","profile_image":""}

]

}
```

## <a name="configure-rest-project-discovery-in-the-web-service-configuration-tool"></a>De REST-project detectie configureren in het hulp programma voor configuratie van webservices
De volgende stappen laten zien hoe u een nieuw project voor uw gegevens bron maakt in het hulp programma voor configuratie van webservices.

1. Open het hulp programma voor configuratie van webservices. Er wordt een leeg SOAP-project geopend.

   ![Hulp programma voor configuratie van webservices](media/microsoft-identity-manager-2016-ma-ws-restgeneric/web-service-configuration-tool.png)

2. Selecteer **bestand**  >  **Nieuw**  >  **rest-project**.

   ![Een nieuw REST-project maken](media/microsoft-identity-manager-2016-ma-ws-restgeneric/new-project.png)
   <!-- Image shows SOAP project selected, not REST project -->

3. Selecteer aan de linkerkant **rest-project** en selecteer vervolgens **toevoegen**.

   ![Het REST-project selecteren](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-project.png)

4. Geef op de volgende pagina de volgende informatie op:

   - De nieuwe naam van de webservice
   - Adres (REST API URL-pad)
   - Naamruimte
   - Beveiligings modus (verificatie type)

   ![REST-service](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-service.png)
    
   In het volgende scherm ziet u voor beelden van deze waarden:
    
   ![Voorbeeld waarden voor de REST-service](media/microsoft-identity-manager-2016-ma-ws-restgeneric/restsample.png)

   Stel de **beveiligings modus** in op _geen_. Stel het **adres** in op de voor beeld-JSON-server die wordt gehost in Azure.

5. Selecteer **OK**. Het REST-project dat wordt vermeld in het hulp programma voor configuratie van webservices.

   ![REST-project in het hulp programma voor configuratie van webservices](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-discovery.png)

6. De volgende stap is het definiëren van de REST API-aanroep en het vertalen van de aanroep naar de Windows Communication Foundation (WCF)-aanroepen.

   1. Vouw het **rest-project** uit en selecteer de _RESTSAMPLE_ -service.

   2. Selecteer **Toevoegen**. U wordt gevraagd twee waarden toe te voegen:
   
      ![Voer waarden in voor de REST-service](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-service-highlights.png)
      
      1. Voer de **naam** in. Deze stap wordt aangeduid als 3 in de scherm opname.
      2. Voer het **adres** in. Deze stap wordt aangeduid als 4 in de scherm opname.
      3. Selecteer **OK**. Een REST-resource wordt toegevoegd aan de beschrijving voor de _RESTSAMPLE_ -service.

7. Selecteer in het vak **resources** de rest-resource die u zojuist hebt toegevoegd. Voeg de volgende methode toe:

   ![Een REST-methode toevoegen aan de resource](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-method.png)
   <!-- How does this dialog appear, by selecting Edit? -->

8. Selecteer de REST-methode. U ziet dat het mogelijk is om meerdere methoden te maken in dezelfde resource en de query's te definiëren die worden door gegeven tijdens de uitvoering.

9. Er zijn geen query's vereist voor de methode GETALL. Laat de parameter waarden leeg. Bij het exporteren of importeren van de REST API, moet u het/or-antwoord van de voorbeeld aanvraag definiëren, afhankelijk van de functie. Kopieer en plak de JSON-retour bij het navigeren naar dit voor beeld.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-samples.png)

10. Selecteer **Opslaan**. Sla het project op naar `C:\Program Files\Microsoft Forefront Identity Manager\2010\Synchronization Service\Extensions` . 

>[!NOTE]
>Nadat het project is opgeslagen, wordt het WsConfig-bestand gegenereerd. Het configuratie bestand bevat meerdere bestanden die eerder in het overzicht van de webservice zijn gedefinieerd.


## <a name="configure-object-types-in-the-web-service-configuration-tool"></a>Object typen configureren in het hulp programma voor configuratie van webservices
De volgende stappen laten zien hoe u object typen voor uw gegevens bron configureert in het hulp programma voor configuratie van webservices.

1. De volgende stap is het definiëren van het verbindings ruimte-schema. Dit wordt bereikt door het object type te maken en hun object typen te definiëren. Klik op **object typen** in het linkerdeel venster en klik op de knop **toevoegen** . Hiermee opent u het scherm. Voeg een nieuw object type toe en geef een naam op. Klik op de knop **OK**.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/object-types.png)

2. Het toevoegen van een object type biedt onder scherm.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/object-type-employee.png)

3. In het rechterdeel venster dat overeenkomt met object type, kunt u de kenmerken en hun eigenschappen voor het geselecteerde object type onderhouden. Als u op de knop toevoegen klikt, wordt er een scherm weer gegeven waarin de ene kenmerken kan toevoegen.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employeeid-string.png)

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-id-string.png)

4. Onder scherm wordt weer gegeven nadat u alle vereiste kenmerken hebt toegevoegd.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-object-type-02.png)

5. Het object type en de kenmerken zijn eenmaal gemaakt en bevatten lege werk stromen die zijn geslaagd voor de bewerkingen die worden uitgevoerd in Microsoft Identity Manager (MIM).


## <a name="configure-workflows-in-the-web-service-configuration-tool"></a>Werk stromen configureren in het hulp programma voor configuratie van webservices

De volgende stap is het configureren van de werk stromen voor het object type. Werk stroom bestanden zijn een reeks activiteiten die tijdens runtime worden gebruikt door de Web Services-connector. De werk stromen worden gebruikt voor het implementeren van de juiste MIM-bewerking. Het hulp programma voor configuratie van webservices helpt u bij het maken van vier verschillende werk stromen:

- Importeren: gegevens importeren uit een gegevens bron voor de volgende twee typen werk stromen:

    - Volledige import bewerking: een volledige import die kan worden geconfigureerd.
    - Delta-import: wordt niet ondersteund door het hulp programma voor configuratie van webservices.

- Exporteren: gegevens exporteren van MIM naar een verbonden gegevens bron. De volgende drie acties worden ondersteund voor de bewerking. U kunt deze acties configureren op basis van uw vereisten.

    - Toevoegen
    - Verwijderen
    - Vervangen

- Wacht woord: wachtwoord beheer voor de gebruiker (object type) uitvoeren. Er zijn twee acties beschikbaar voor deze bewerking:

    - Wachtwoord instellen
    - Wachtwoord wijzigen

- Verbinding testen: Configureer een werk stroom om te controleren of de verbinding met de gegevens bron server tot stand is gebracht.

>[!NOTE]
>U kunt deze werk stromen configureren voor het project of het standaard project downloaden van het [micro soft Download centrum](https://www.microsoft.com/download/details.aspx?id=29944).


### <a name="workflow-designer"></a>Workflow Designer
De Workflow Designer opent het werk gebied om de werk stroom te configureren volgens de vereiste. Voor elk object type (nieuwe/existing) biedt het configuratie hulpprogramma de knoop punten voor werk stromen die door het hulp programma worden ondersteund. 

![Workflow Designer](media/microsoft-identity-manager-2016-ma-ws-restgeneric/full-import-configuration-workflow.png)

De Workflow Designer bestaat uit de volgende elementen van de gebruikers interface:

   - **Knoop punten in linkerdeel venster**: met deze informatie kunt u selecteren welke werk stroom u wilt ontwerpen.

   - **Centrale Workflow Designer**: hier kunt u de activiteiten voor het configureren van de werk stromen verwijderen. Voor het uitvoeren van verschillende MIM-bewerkingen (exporteren, importeren, wachtwoord beheer) kunt u de standaard-en aangepaste werk stroom activiteiten van .NET workflow Framework 4 gebruiken. Het hulp programma voor configuratie van webservices maakt gebruik van standaard-en aangepaste werk stroom activiteiten. Zie [activiteiten ontwerpen gebruiken](https://msdn.microsoft.com/library/ee829528.aspx)voor meer informatie over standaard activiteiten.

      - In de centrale Workflow Designer geeft een rode cirkel met een uitroep teken naast elke activiteit aan dat de bewerking is verwijderd en niet correct en volledig is gedefinieerd. Beweeg de muis aanwijzer over de rode cirkel om de exacte fout te achterhalen. Nadat de activiteit op de juiste wijze is gedefinieerd, verandert de rode cirkel in het geleende gegevens merk.
      
      - In de centrale Workflow Designer geeft een geel drie hoekje op een wille keurige activiteit aan dat de activiteit is gedefinieerd, maar dat u wel meer kunt doen om de activiteit te volt ooien. Beweeg de muis aanwijzer over de gele drie hoek om meer informatie weer te geven.

   - **Werkset**: verpakt alle hulpprogram ma's, waaronder systeem-en aangepaste activiteiten en vooraf gedefinieerde instructies om de werk stroom te ontwerpen. Zie [Toolbox](https://msdn.microsoft.com/library/aa480213.aspx)voor meer informatie.
   
   - **Secties** in de werkset: de werkset bevat de volgende secties en Categorieën:
   
      - **Beschrijving**: de koptekst van de werkset. Met één tabblad krijgt u toegang tot de werkset en de eigenschappen van de geselecteerde werk stroom activiteit. 

      - **Importeer werk stroom**: aangepaste activiteiten voor het configureren van import werk stromen.
      
      - **Werk stroom exporteren**: aangepaste activiteiten voor het configureren van export werk stromen.
      
      - **Algemeen**: aangepaste activiteiten voor het configureren van elke werk stroom.
      
      - **Debug**: systeem werk stroom activiteiten voor fout opsporing gedefinieerd in werk stroom 4. Deze activiteiten bieden het bijhouden van problemen voor een werk stroom.
      
      - **Instructies**: systeem werk stroom activiteiten gedefinieerd in werk stroom 4. Zie [using activity designers](https://msdn.microsoft.com/library/ee829528.aspx)(Engelstalig) voor meer informatie.

   - **Eigenschappen**: op het tabblad Eigenschappen worden de eigenschappen weer gegeven van een bepaalde werk stroom activiteit die is verwijderd uit het ontwerp gebied en geselecteerd. In de afbeelding aan de linkerkant worden de eigenschappen van **Assign** -activiteit weer gegeven. Voor elke activiteit verschillen de eigenschappen en worden ze gebruikt bij het configureren van de aangepaste werk stroom. Op dit tabblad kunt u de kenmerken definiëren van het geselecteerde hulp programma dat is verwijderd uit de centrale werk stroom ontwerper. Zie [Eigenschappen](https://msdn.microsoft.com/library/ee342461.aspx)voor meer informatie.

   - **Taak balk:** De taak balk bevat drie elementen: **variabelen**, **argumenten** en **Imports**. Deze elementen worden samen met werk stroom activiteiten gebruikt. Zie [de inleiding tot Windows Workflow Foundation (WF) van een ontwikkelaar in .net 4](https://msdn.microsoft.com/library/ee342461.aspx)voor meer informatie.



## <a name="configure-a-full-import-workflow-in-the-web-service-configuration-tool"></a>Een volledige import werk stroom configureren in het hulp programma voor configuratie van webservices
De volgende stappen laten zien hoe u de volledige import werk stromen voor de REST API kunt configureren met behulp van het hulp programma voor configuratie van webservices.

>[!WARNING]
>In dit voor beeld wordt alleen een werk stroom gemaakt. Wijzigingen in de werk stroom, zoals voor het gebruik van aangepaste logica in de API, zijn mogelijk vereist.

1. Selecteer de werk stroom voor volledige import om te configureren. De **argumenten** en **import bewerkingen** zijn al gedefinieerd en zijn specifiek voor de activiteiten. Raadpleeg de volgende schermen voor meer informatie.

   ![Werk stroom argumenten voor volledige import](media/microsoft-identity-manager-2016-ma-ws-restgeneric/arguments.png)

   ![Geïmporteerde naam ruimten](media/microsoft-identity-manager-2016-ma-ws-restgeneric/imported-name-spaces.png)

   Na het opnieuw configureren van de aanroepen moet u de namen wijzigen van de kenmerken die worden gewijzigd of de naam ruimte toevoegen aan variabelen die verwijzen naar de retour structuur van de API en object typen die naar de oude naam ruimte verwijzen. De werkset in het rechterdeel venster bevat alle aangepaste werk stroom activiteiten die u nodig hebt voor configuratie. Wijs de waarden toe aan de variabelen die u voor uw logica gaat gebruiken. Ga naar de onderste sectie van de centrale werk stroom ontwerper en Declareer de variabelen. Variabelen worden in de volgende stap gedeclareerd.

2. Voeg een reeks activiteit toe. Sleep de ontwerp functie voor **sequentie** activiteiten vanuit de **werkset** en zet deze neer op het Windows Workflow Designer-Opper vlak. Raadpleeg de volgende schermen. De [reeks](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) activiteit bevat een geordende verzameling onderliggende activiteiten die in volg orde worden uitgevoerd.
   
    ![Sequentie activiteit](media/microsoft-identity-manager-2016-ma-ws-restgeneric/imports.png)

3. Als u een variabele wilt toevoegen, zoekt u **variabele maken**. Typ _wsResponse_ voor de **naam**, selecteer de vervolg keuzelijst **type variabele** en selecteer vervolgens **Bladeren voor typen**. Er wordt een dialoog venster weer gegeven. Selecteer een **gegenereerd**  >  **GETALL**-  >  **antwoord**. Behoud de selectie van het **bereik** en de **standaard** waarden. U kunt deze waarden ook instellen met behulp van de weer gave **Eigenschappen** .

   ![Standaard antwoord](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-list.png)

4. Sleep een andere ontwerp functie voor **sequentie** activiteiten vanuit de **werkset** binnen de reeks activiteit die al is toegevoegd.

5. Sleep een **WebServiceCallActivity** die wordt gepresenteerd onder **common.** Deze activiteit wordt gebruikt om de webservice-bewerking aan te roepen die na de detectie beschikbaar is. Dit is een aangepaste activiteit en is gebruikelijk in verschillende scenario's. 

    ![Bewerking service naam](media/microsoft-identity-manager-2016-ma-ws-restgeneric/full-import-operation-workflow.png)

   Als u de bewerking voor de webservice wilt gebruiken, stelt u de volgende eigenschappen in:
   
      - **Service naam**: Voer een naam in voor de webservice.
      - **Eindpunt naam**: Geef een eindpunt naam op voor de geselecteerde service.
      - **Bewerkings naam**: Geef de betreffende bewerking voor de service op.
      - **Argument**: Select- **argumenten**. Wijs in het volgende dialoog venster de argument waarden toe, zoals wordt weer gegeven in de volgende afbeelding:
      
         ![Argumenten toewijzen](media/microsoft-identity-manager-2016-ma-ws-restgeneric/get-all.png)

         >[!IMPORTANT]
         >Wijzig de **naam**, **richting** of het **type** voor een argument niet met behulp van dit dialoog venster. Als een van deze waarden is gewijzigd, wordt de activiteit ongeldig. Stel alleen de **waarde** voor het argument in. Zoals in deze afbeelding wordt weer gegeven, wordt de waarde *wsResponse* ingesteld.

6. Voeg een **foreach** -activiteit toe, net onder **WebServiceCallActivity.** Deze activiteit wordt gebruikt om alle kenmerken (zowel ankers als niet-ankers) van het object type te herhalen. Wanneer u deze activiteit naar uw Workflow Designer-Opper vlak sleept, worden automatisch alle kenmerk namen voor uw object opgesomd. Stel de vereiste waarden in volgens het volgende scherm:

   ![Oproep activiteit voor webservices](media/microsoft-identity-manager-2016-ma-ws-restgeneric/foreach.png)

7. In sommige gevallen moet u mogelijk de generated.dll in het WsConfig-bestand openen. Kopieer dit WsConfig-bestand en wijzig de naam met de extensie. zip. Open en extraheer de generated.dll met behulp van uw favoriete .NET reflector-hulp programma.

   ![Configuratie bestand](media/microsoft-identity-manager-2016-ma-ws-restgeneric/config-files.png)

8. De open bare naam ruimte voor de _EmployeeList_ identificeren:

    ![Code van werknemers lijst](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-list-code.png)

    Voeg vervolgens deze keer toe aan de werk stroom **foreach**:

    ![Werknemers lijst toevoegen aan ForEach-werk stroom](media/microsoft-identity-manager-2016-ma-ws-restgeneric/foreach-employee-list.png)

9. Sleep een **CreateCSEntryChangeScope** -activiteit in de hoofd tekst van de **foreach** . Deze activiteit wordt gebruikt om een exemplaar van het CSEntryChange-object in het werk stroom domein te maken voor elke record tijdens het ophalen van gegevens uit de gegevens bron van het doel. Als u deze activiteit versleept, vindt u hieronder het scherm. **CreateAnchorAttribute** -activiteiten worden automatisch overgenomen. Werk de **DN** -waarde bij naar de domein naam van uw voor keur.

    ![De activiteit voor het wijzigen van het bereik van CS maken](media/microsoft-identity-manager-2016-ma-ws-restgeneric/createcsentry.png)

    >[!NOTE]
    >Anker waarden en object namen variëren afhankelijk van de weer gegeven webservice. In de afbeelding ziet u een voor beeld.

10. Sleep een **CreateAttributeChange** -activiteit onder de activiteit **CreateAnchorAttribute** . Het aantal te slepen activiteiten is gelijk aan het aantal niet-anker kenmerken. Raadpleeg de volgende afbeelding voor verwijzing.

    ![Anker maken](media/microsoft-identity-manager-2016-ma-ws-restgeneric/create-anchor-attribute.png)

    >[!NOTE]
    >Als u deze activiteit wilt gebruiken, kiest u de desbetreffende velden uit de vervolg keuzelijst en wijst u de waarden toe. Haal meerdere **CreateValueChangeActivity** -activiteiten binnen een **CreateAttributeChangeActivity** -activiteit.

11. Sla dit project op de locatie op `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` . Configureer vervolgens de beheer agent zoals beschreven in de [Web Service ma-configuratie](microsoft-identity-manager-2016-ma-ws-maconfig.md).

    ![Het REST-project opslaan](media/microsoft-identity-manager-2016-ma-ws-restgeneric/sample-rest.png)
    
    Standaard projecten moeten worden gedownload en opgeslagen op de locatie `%FIM_INSTALL_DIR\2010\Synchronization Service\Extensions` op het doel systeem. De projecten worden vervolgens weer gegeven in de wizard webservices connector.
    
    Wanneer u het uitvoer bare bestand uitvoert, wordt u gevraagd de locatie voor de installatie op te geven. Voer de opslag locatie in.
    
    >[!IMPORTANT]
    >Het project bestand kan worden opgeslagen en geopend vanaf elke locatie (met de juiste toegangs rechten van de uitvoerder). Alleen project bestanden die worden opgeslagen in de `Synchronization Service\Extension` map, kunnen worden geselecteerd in de wizard Web Service connector die wordt geopend via de gebruikers interface van de MIM-synchronisatie.
    
    De gebruiker die het hulp programma voor configuratie van webservices uitvoert, vereist de volgende bevoegdheden:
    
       - Volledige controle over de map synchronisatie service-extensie.
       - Lees toegang tot de register sleutel `HKLM\System\CurrentControlSet\Services\FIMSynchronizationService\Parameters` waarmee het pad naar de map met de extensie zich bevindt.


## <a name="next-steps"></a>Volgende stappen

- [Overzicht van de algemene webservice-connector](microsoft-identity-manager-2016-ma-ws.md)
- [Installeer het hulp programma voor configuratie van webservices](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP-implementatie handleiding](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Hand leiding voor REST-implementatie](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuratie van de web service-MA](microsoft-identity-manager-2016-ma-ws-maconfig.md)
