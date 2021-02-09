---
title: Werk stroom gids voor webservice-connector voor SOAP | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een nieuw project maakt voor uw SOAP-gegevens bron met behulp van het hulp programma voor configuratie van webservices.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/30/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 455988722b3aeb9e29b00696342e1800e9ad82c7
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835968"
---
# <a name="web-service-connector-workflow-guide-for-soap"></a>Werk stroom gids voor webservice-connector voor SOAP

In dit artikel wordt beschreven hoe u een nieuw project maakt voor uw gegevens bron in het hulp programma voor configuratie van webservices. Volg deze stappen om een project te maken.

1.  Open het hulp programma voor configuratie van webservices. Er wordt een leeg project geopend.

    ![Hulp programma voor configuratie van webservices](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-configuration-tool-01.png)

2.  Selecteer **SOAP-project** en selecteer vervolgens **toevoegen**.

    ![SOAP-project](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-configuration-tool-02.png)

3.  Geef op de volgende pagina de volgende informatie op en selecteer **volgende**:

    - De nieuwe naam van de webservice
    - Adres (WSDL-pad) voor het ophalen van de weer gegeven Services, eind punten en bewerkingen
    - Naamruimte
    - Beveiligings modus (verificatie type)
  
4.  In dit voor beeld wordt de pagina **referenties** weer gegeven met de vereisten voor de *Basic* -beveiligings modus (de modus die is geselecteerd in de vorige stap). Als ' geen ' is opgegeven voor de beveiligings modus, zou er geen referentie pagina kunnen worden weer gegeven. Selecteer **Next**.

    ![Scherm van de SOAP-service met gebruikers naam en wacht woord](media/microsoft-identity-manager-2016-ma-ws-soap/soap-service.png)

5.  Het WSDL-pad wordt geopend om de service gegevens op te halen en de lijst met beschik bare functies wordt weer gegeven. Als het opgegeven WSDL-pad onjuist is, kan het configuratie hulpprogramma de service gegevens niet ophalen en een fout genereren.

    ![voortgangs scherm voor het downloaden van webservices](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-progress.png)

6.  Zodra de detectie is uitgevoerd, worden het eind punt en de gedetecteerde bewerkingen weer gegeven. Selecteer **Finish**.

    ![De SOAP-service-eind punten en-bewerkingen zijn gedetecteerd](media/microsoft-identity-manager-2016-ma-ws-soap/soap-service-endpoints.png)

7.  Compilatie wordt uitgevoerd. Compilatie is een proces voor het compileren van de gegevens contract-assembly, wat een tijdrovende bewerking kan zijn. De gebruiker wordt op de hoogte gesteld van compilatie fouten. Nadat de detectie is uitgevoerd, wordt het hulp programma weer gegeven met de volgende pagina:

    ![SOAP-detectie](media/microsoft-identity-manager-2016-ma-ws-soap/soap-discovery.png)

8.  Het **SOAP-project** wordt uitgebreid en het weer gegeven eind punt wordt geselecteerd onder het scherm. In dit scherm worden de bewerkingen weer gegeven die onder het eind punt zijn gedeclareerd.

    ![Bewerkingen die zijn gedeclareerd onder het eind punt](media/microsoft-identity-manager-2016-ma-ws-soap/basic-http-binding.png)

9.  Met uitgebreid eind punt wordt een lijst met bewerkingen weer gegeven. Een bewerking is een functie die is gedeclareerd door een eind punt. Elke bewerking is gericht op een type taak dat kan worden uitgevoerd in de service. Dit scherm bevat de argumenten die voor de bewerking zijn gedeclareerd. Deze argumenten worden vervolgens gedefinieerd wanneer de bewerking wordt gebruikt voor het configureren van de werk stromen.

    ![Uitgebreide eind punten](media/microsoft-identity-manager-2016-ma-ws-soap/get-employee-byid.png)

10. De volgende stap is het definiëren van het verbindings ruimte-schema, dat wordt bereikt door het object type te maken en hun object typen te definiëren. Selecteer **object typen** en selecteer vervolgens **toevoegen**. Voeg in het nieuwe venster een nieuw object type toe en geef een naam op. Selecteer **OK**.

    ![Object type definiëren](media/microsoft-identity-manager-2016-ma-ws-soap/object-types.png)

11. Het toevoegen van een object type biedt onder scherm.

    ![nieuw gemaakt object type weer geven](media/microsoft-identity-manager-2016-ma-ws-soap/object-type-employee.png)

12. In het rechterdeel venster dat overeenkomt met object type, kunt u de kenmerken en hun eigenschappen voor het geselecteerde object type onderhouden. Selecteer **Toevoegen**. Er wordt een nieuw venster geopend om kenmerken toe te voegen:

    ![Kenmerk en gegevens type](media/microsoft-identity-manager-2016-ma-ws-soap/object-type-firstname.png)

    ![Optie voor kenmerk en gegevens type met anker geselecteerd](media/microsoft-identity-manager-2016-ma-ws-soap/employeeid-string.png)

13. Het volgende scherm wordt weer gegeven na het toevoegen van alle vereiste kenmerken:

    ![Object type met kenmerk gegevens](media/microsoft-identity-manager-2016-ma-ws-soap/soap-project.png)

14. Het object type en de kenmerken zijn eenmaal gemaakt en bevatten lege werk stromen die zijn geslaagd voor de bewerkingen die zijn uitgevoerd in Microsoft Identity Manager 2016 (MIM).

    ![Object typen toont bewerkingen die werk nemers kunnen uitvoeren](media/microsoft-identity-manager-2016-ma-ws-soap/object-types-operations.png)


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

![Workflow Designer](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-workflow.png)

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


<h2 id="full-import-workflows">Een volledige import werk stroom configureren in het hulp programma voor configuratie van webservices</h2>
De volgende stappen laten zien hoe u de volledige import werk stromen voor SOAP kunt configureren met behulp van het hulp programma voor configuratie van webservices.

>[!WARNING]
>In dit voor beeld wordt alleen een werk stroom gemaakt. Wijzigingen in de werk stroom, zoals voor het gebruik van aangepaste logica in de API, zijn mogelijk vereist.

1. Selecteer de werk stroom voor volledige import om te configureren. De **argumenten** en **import bewerkingen** zijn al gedefinieerd en zijn specifiek voor de activiteiten. Raadpleeg de volgende schermen voor meer informatie.

   ![Werk stroom argumenten voor volledige import](media/microsoft-identity-manager-2016-ma-ws-soap/arguments.png)
 
   ![Geïmporteerde naam ruimten](media/microsoft-identity-manager-2016-ma-ws-soap/imports.png)

   Nadat u de aanroepen opnieuw hebt geconfigureerd, wijzigt u de namen van de kenmerken die worden gewijzigd, voegt u de naam ruimte toe of wijzigt u deze in variabelen die verwijzen naar de retour structuur van de API en object typen die naar de oude naam ruimte verwijst. De werkset in het rechterdeel venster bevat alle aangepaste werk stroom activiteiten die u nodig hebt voor configuratie. Wijs de waarden toe aan de variabelen die u voor uw logica gaat gebruiken. Ga naar de onderste sectie van de centrale werk stroom ontwerper en Declareer de variabelen. Variabelen worden in de volgende stap gedeclareerd.

2. Voeg een reeks activiteit toe. Sleep de ontwerp functie voor **sequentie** activiteiten vanuit de **werkset** en zet deze neer op het Windows Workflow Designer-Opper vlak. Raadpleeg de volgende schermen. De [reeks](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) activiteit bevat een geordende verzameling onderliggende activiteiten die in volg orde worden uitgevoerd.
   
    ![Sequentie activiteit](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-sequence.png)

3. Als u een variabele wilt toevoegen, zoekt u **variabele maken**. Typ _wsResponse_ voor de **naam**, selecteer de vervolg keuzelijst **type variabele** en selecteer vervolgens **Bladeren voor typen**. Er wordt een dialoog venster weer gegeven. Selecteer **gegenereerd**  >  **standaard**  >  **antwoord**. Behoud de selectie van het **bereik** en de **standaard** waarden. U kunt deze waarden ook instellen met behulp van de weer gave **Eigenschappen** .

   ![Standaard antwoord](media/microsoft-identity-manager-2016-ma-ws-soap/default-response.png)

   ![Volledige import eigenschappen](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-properties.png)

4. Voeg nu alle andere variabelen toe en hieronder is het laatste scherm.

   ![Variabelen voor volledige import bewerking](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-variables.png)

5. Sleep een andere ontwerp functie voor **sequentie** activiteiten vanuit de **werkset** binnen de reeks activiteit die al is toegevoegd.

6. Sleep een **WebServiceCallActivity** die wordt gepresenteerd onder **common.** Deze activiteit wordt gebruikt om de webservice-bewerking aan te roepen die na de detectie beschikbaar is. Dit is een aangepaste activiteit en is gebruikelijk in verschillende scenario's. 

    ![Bewerking service naam](media/microsoft-identity-manager-2016-ma-ws-soap/service-name-operation.png)

   Als u de bewerking voor de webservice wilt gebruiken, stelt u de volgende eigenschappen in:
   
      - **Service naam**: Voer een naam in voor de webservice.
      - **Eindpunt naam**: Geef een eindpunt naam op voor de geselecteerde service.
      - **Bewerkings naam**: Geef de betreffende bewerking voor de service op.
      - **Argument**: Select- **argumenten**. Wijs in het volgende dialoog venster de argument waarden toe, zoals wordt weer gegeven in de volgende afbeelding:
      
         ![Argumenten toewijzen](media/microsoft-identity-manager-2016-ma-ws-soap/get-employeebyid.png)

         >[!IMPORTANT]
         >Wijzig de **naam**, **richting** of het **type** voor een argument niet met behulp van dit dialoog venster. Als een van deze waarden is gewijzigd, wordt de activiteit ongeldig. Stel alleen de **waarde** voor het argument in. Zoals in deze afbeelding wordt weer gegeven, wordt de waarde *wsResponse* ingesteld.

7. Voeg een **foreach** -activiteit toe, net onder **WebServiceCallActivity.** Deze activiteit wordt gebruikt om alle kenmerken (zowel ankers als niet-ankers) van het object type te herhalen. Wanneer u deze activiteit naar uw Workflow Designer-Opper vlak sleept, worden automatisch alle kenmerk namen voor uw object opgesomd. Stel de vereiste waarden in volgens het volgende scherm:

   ![Oproep activiteit voor webservices](media/microsoft-identity-manager-2016-ma-ws-soap/webservicecallactivity.png)

8. Sleep een **CreateCSEntryChangeScope** -activiteit in de hoofd tekst van de **foreach** . Deze activiteit wordt gebruikt om een exemplaar van het CSEntryChange-object in het werk stroom domein te maken voor elke record tijdens het ophalen van gegevens uit de gegevens bron van het doel. Als u deze activiteit versleept, vindt u hieronder het scherm. **CreateAnchorAttribute** -activiteiten worden automatisch overgenomen.

    ![De activiteit voor het wijzigen van het bereik van CS maken](media/microsoft-identity-manager-2016-ma-ws-soap/createcsentrychangescope.png)

9.  Stel de waarde van de DN-expressie in als `‘string.Concat ("Employee",item.EmployeeID)’` . Stel de **AnchorValue** voor de werk _nemer_ -werkset in op **' Convert. toString (item. EmployeeID) '**. Stel de **object typenaam** in als _werk nemer_. Nadat u deze wijzigingen hebt aangebracht, ziet u het volgende scherm:

    ![De werk nemer-ID ophalen](media/microsoft-identity-manager-2016-ma-ws-soap/get-employeebyid.png)

    >[!NOTE]
    >Anker waarden en object namen variëren afhankelijk van de weer gegeven webservice. In de afbeelding ziet u een voor beeld.

10. Sleep een **CreateAttributeChange** -activiteit onder de activiteit **CreateAnchorAttribute** . Het aantal te slepen activiteiten is gelijk aan het aantal niet-anker kenmerken. Raadpleeg de volgende afbeelding voor verwijzing.

    ![Anker maken](media/microsoft-identity-manager-2016-ma-ws-soap/create-anchor.png)

11. Sleep **CreateValueChangeActivity** binnen **CreateAttributeChange** -activiteit en stel de kenmerk waarde in op basis van het scherm.

    ![Een kenmerk wijzigen](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-change.png)

    >[!NOTE]
    >Als u deze activiteit wilt gebruiken, kiest u de desbetreffende velden uit de vervolg keuzelijst en wijst u de waarden toe. Haal meerdere **CreateValueChangeActivity** -activiteiten binnen een **CreateAttributeChangeActivity** -activiteit.

12. Als u voor waarden wilt toevoegen voor een kenmerk, voegt u een **if** -activiteit toe, zoals wordt weer gegeven in de volgende afbeelding:

    ![Als activiteit](media/microsoft-identity-manager-2016-ma-ws-soap/if.png)

13. Voeg ten slotte een **toewijzings** activiteit toe en stel de expressie in, zoals wordt weer gegeven in de volgende afbeelding:

    ![Activiteit toewijzen en de expressie instellen](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-change-email.png)

14. Sla dit project op de locatie op `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .
    
    Standaard projecten moeten worden gedownload en opgeslagen op de locatie `%FIM_INSTALL_DIR\2010\Synchronization Service\Extensions` op het doel systeem. De projecten worden vervolgens weer gegeven in de wizard webservices connector.
    
    Wanneer u het uitvoer bare bestand uitvoert, wordt u gevraagd de locatie voor de installatie op te geven. Voer de opslag locatie in.
    
    >[!IMPORTANT]
    >Het project bestand kan worden opgeslagen en geopend vanaf elke locatie (met de juiste toegangs rechten van de uitvoerder). Alleen project bestanden die worden opgeslagen in de `Synchronization Service\Extension` map, kunnen worden geselecteerd in de wizard Web Service connector die wordt geopend via de gebruikers interface van de MIM-synchronisatie.
    
    De gebruiker die het hulp programma voor configuratie van webservices uitvoert, vereist de volgende bevoegdheden:
    
       - Volledige controle over de map synchronisatie service-extensie.
       - Lees toegang tot de register sleutel `HKLM\System\CurrentControlSet\Services\FIMSynchronizationService\Parameters` waarmee het pad naar de map met de extensie zich bevindt.


## <a name="configure-export-workflows-in-the-web-service-configuration-tool"></a>Werk stromen voor exporteren configureren in het hulp programma voor configuratie van webservices
In de volgende secties ziet u hoe u uw werk stromen kunt exporteren met behulp van het hulp programma voor configuratie van webservices.

<h3 id="attribute-change-anchor">Werk stromen toevoegen</h3>
Voer de volgende stappen uit in het hulp programma voor configuratie van webservices om export werk stromen toe te voegen.

1. Selecteer de werk stroom die u wilt configureren. Selecteer onder **exporteren** de optie **toevoegen**. De **argumenten** en **import bewerkingen** zijn al gedefinieerd en zijn specifiek voor de activiteiten. Raadpleeg de volgende schermen voor naslag informatie.

    ![Toevoegen](media/microsoft-identity-manager-2016-ma-ws-soap/add.png)

2. Voeg een **reeks** activiteit toe. Sleep de ontwerp functie voor **sequentie** activiteiten vanuit de **werkset** en zet deze neer op het Windows Workflow Designer-Opper vlak. De [reeks](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) activiteit bevat een geordende verzameling onderliggende activiteiten die in volg orde worden uitgevoerd. Selecteer **variabele maken**. Wijs de waarden toe aan de variabelen die u voor uw logica gaat gebruiken.

    ![Exporteren](media/microsoft-identity-manager-2016-ma-ws-soap/export-add.png)

    >[!NOTE]
    >De stappen voor het toevoegen van een variabele worden beschreven in de sectie voor het maken van <a href="#full-import-workflows">volledige import werk stromen</a>.

3. Sleep een **foreach** -activiteit binnen een al toegevoegde **reeks** activiteit om de waarden van anker kenmerken te herhalen.

4. Selecteer **Eigenschappen** en stel de **waarden** in op basis van het scherm. Hier **objectToExport** is argument.

    ![De eigenschappen voor de ForEach-activiteit instellen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-sequence.png)

5. **DisplayName** instellen als **foreach \<AnchorAttribute\>**

   ![De weergave naam instellen](media/microsoft-identity-manager-2016-ma-ws-soap/add-sequence.png)

6. Stel **TypeArgument** in als `Microsoft.MetadirectoryServices.AnchorAttribute` .

   ![Het argument type instellen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-anchor.png)

7. Voeg een **Switch** activiteit toe binnen de **foreach** -hoofd tekst van de **AnchorAttribute**.

   ![Een switch activiteit toevoegen](media/microsoft-identity-manager-2016-ma-ws-soap/select-net-types.png)

8. Een expressie toevoegen aan de hand van een voor beeld van een scherm.

   ![Een expressie toevoegen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch.png)

9. Selecteer **een nieuwe case toevoegen** en voer een waarde in voor de werk **nemer**-aanvraag. Sleep een **sequentie** -activiteit en voeg deze toe aan een **Assign** -activiteit toevoegen.

    ![Een nieuwe case toevoegen en toewijzen aan de reeks](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-employeeid.png)

10. Wijs de eigenschappen **aan** en **waarde** voor de **Assign** -activiteit toe.

    ![Eigenschappen voor aan en waarde toewijzen](media/microsoft-identity-manager-2016-ma-ws-soap/anchor-attribute-name.png)

11. De **foreach** -activiteit wordt gebruikt voor anker waarden. Voeg nog een **foreach** -activiteit toe om niet-anker waarden toe te wijzen. In dit voor beeld wordt het **AttributeChange** -anker gebruikt.

    ![Nog een ForEach-activiteit toevoegen met het AttributeChange-anker](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-change.png)

12. Een **Switch** -activiteit toevoegen **binnen de** hoofd tekst van het **AttributeChange** -anker.   

    ![Schakel activiteit voor het AttributeChange-anker toevoegen](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-name-wrapper.png)

13. Een expressie toevoegen aan de hand van een voor beeld van een scherm.

    ![Een expressie voor de switch activiteit toevoegen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-expression.png)

14. Selecteer **een nieuwe case toevoegen** en voer een waarde in voor de voor **naam**. Sleep een **sequentie** -activiteit en voeg deze toe aan een **Assign** -activiteit toevoegen. Wijs de eigenschappen **aan** en **waarde** voor de **Assign** -activiteit toe.

    ![Een nieuwe case voor de reeks toevoegen](media/microsoft-identity-manager-2016-ma-ws-soap/switch-firstname.png)

15. Voeg waarden toe voor de vereiste kenmerken, zoals **LastName**, **email**, enzovoort. 

    ![Waarden voor vereiste kenmerken toevoegen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch-anchor.png)

16. Sleep onder **Algemeen** een **WebServiceCallActivity** en stel **waarden** in voor de **argumenten**.

    ![Een activiteit voor het aanroepen van een webservice toevoegen en de waarden instellen](media/microsoft-identity-manager-2016-ma-ws-soap/add-employee-attribute.png)

    >[!IMPORTANT]
    >Wijzig de **naam**, **richting** of het **type** voor een argument niet met behulp van dit dialoog venster. Als een van deze waarden is gewijzigd, wordt de activiteit ongeldig. Stel alleen de **waarde** voor het argument in. Zoals in deze afbeelding wordt weer gegeven, wordt de waarde *wsResponse* ingesteld.

17.  Voeg ten slotte een **if** -activiteit toe om de antwoorden te controleren die worden geretourneerd door de bewerking van de webservice.

Het maken van de export werk stroom met de **toevoeg** bewerking is voltooid:

![Werk stroom exporteren voltooid](media/microsoft-identity-manager-2016-ma-ws-soap/create-csentry-change.png)

Sla dit project op de locatie op `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


### <a name="delete-workflows"></a>Werk stromen verwijderen
Verwijder werk stromen voor exporteren door de volgende stappen te volgen in het hulp programma voor configuratie van webservices.

1. Selecteer de werk stroom die u wilt configureren. Selecteer onder **exporteren** de optie **verwijderen**. De **argumenten** en **import bewerkingen** zijn al gedefinieerd en zijn specifiek voor de activiteiten. Raadpleeg de volgende schermen voor naslag informatie.

   ![Werk stromen voor verwijderen exporteren](media/microsoft-identity-manager-2016-ma-ws-soap/export-delete.png)

2. Voeg een **reeks** activiteit toe. Selecteer **variabele maken**. Wijs de waarden toe aan de variabelen die u voor uw logica gaat gebruiken.

   ![Een reeks activiteit toevoegen](media/microsoft-identity-manager-2016-ma-ws-soap/sequence-variables.png)

   >[!NOTE]
   >De stappen voor het toevoegen van een variabele worden beschreven in de sectie voor het maken van <a href="#full-import-workflows">volledige import werk stromen</a>.

3. Sleep een **foreach** -activiteit binnen een al toegevoegde **reeks** activiteit om de waarden van anker kenmerken te herhalen.

4. Selecteer **Eigenschappen** en stel de **waarden** in op de onderstaande scherm. Hier **objectToExport** is argument.

   ![De eigenschappen voor de ForEach-activiteit instellen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-object-to-export.png)

5. Stel **DisplayName** in als `ForEach\<AnchorAttribute\>` :

   ![De weergave naam instellen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-anchor-type.png)

6. Stel de **TypeArgument** in op als `Microsoft.MetadirectoryServices.AnchorAttribute` : 

   ![Het argument type instellen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-type-argument.png)

7. Voeg een **Switch** activiteit toe binnen de **foreach** -hoofd tekst van de **AnchorAttribute**.

   ![Een switch activiteit toevoegen](media/microsoft-identity-manager-2016-ma-ws-soap/select-net-type.png)

8. Een expressie toevoegen aan de hand van een voor beeld van een scherm.

   ![Een expressie toevoegen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch.png)

9. Selecteer **een nieuwe case toevoegen** en voer een waarde in voor de werk **nemer**-aanvraag. Sleep een **sequentie** -activiteit en voeg deze toe aan een **Assign** -activiteit toevoegen.

   ![Een nieuwe case toevoegen en toewijzen aan de reeks](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-default.png)

10. Wijs de eigenschappen **aan** en **waarde** voor de **Assign** -activiteit toe.

    ![Eigenschappen voor aan en waarde toewijzen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-attribute-flow.png)

11. Sleep onder **Algemeen** een **WebServiceCallActivity** en stel **waarden** in voor de **argumenten**.

    ![Een activiteit voor het aanroepen van een webservice toevoegen en de waarden instellen](media/microsoft-identity-manager-2016-ma-ws-soap/delete-employee.png)

    >[!IMPORTANT]
    >Wijzig de **naam**, **richting** of het **type** voor een argument niet met behulp van dit dialoog venster. Als een van deze waarden is gewijzigd, wordt de activiteit ongeldig. Stel alleen de **waarde** voor het argument in. Zoals in deze afbeelding wordt weer gegeven, is de waarde *employeeID* ingesteld.

12. Voeg ten slotte een **if** -activiteit toe om de antwoorden te controleren die worden geretourneerd door de bewerking van de webservice.

Het verwijderen van de export werk stroom met de **Verwijder** bewerking is voltooid:

![De export werk stroom is verwijderd](media/microsoft-identity-manager-2016-ma-ws-soap/create-csentry-change.png)

<!-- Image of completed Delete operation is missing from document -->

Sla dit project op de locatie op `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


### <a name="replace-workflows"></a>Werk stromen vervangen
Vervang werk stromen exporteren door de volgende stappen te volgen in het hulp programma voor configuratie van webservices.

1. Selecteer de werk stroom die u wilt configureren. Selecteer onder **exporteren** de optie **vervangen**. De **argumenten** en **import bewerkingen** zijn al gedefinieerd en zijn specifiek voor de activiteiten. Zie het onderstaande scherm voor referentie.

   ![Een werk stroom vervangen](media/microsoft-identity-manager-2016-ma-ws-soap/replace.png)

2. Voeg een **reeks** activiteit toe.

3. Sleep een **foreach** -activiteit voor de **\<AnchorAttribute> .**

4. Voeg nog **een \<AttributeChange> foreach** -activiteit toe om niet-anker waarden toe te wijzen.

5. Ten slotte ziet het scherm eruit als in de volgende afbeelding. De instructies voor het configureren van deze activiteit vindt u in de sectie voor het <a href="#attribute-change-anchor">toevoegen van export werk stromen</a>.

   ![ForEach met een switch-activiteit en anker kenmerk](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch-anchor.png)

6. Sleep onder **Algemeen** een **WebServiceCallActivity** en stel **waarden** in voor de **argumenten**.

   ![Een activiteit voor het aanroepen van een webservice toevoegen en de waarden instellen](media/microsoft-identity-manager-2016-ma-ws-soap/wsresponse.png)

   >[!IMPORTANT]
   >Wijzig de **naam**, **richting** of het **type** voor een argument niet met behulp van dit dialoog venster. Als een van deze waarden is gewijzigd, wordt de activiteit ongeldig. Stel alleen de **waarde** voor het argument in. Zoals in deze afbeelding wordt weer gegeven, wordt de waarde *werk nemer* ingesteld.

7. Voeg ten slotte een **if** -activiteit toe om de antwoorden te controleren die worden geretourneerd door de bewerking van de webservice.

De export werk stroom is vervangen door de **vervangings** bewerking is voltooid:

![Werk stroom exporteren vervangen](media/microsoft-identity-manager-2016-ma-ws-soap/operation-name-update-employee.png)

Sla dit project op de locatie op `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


## <a name="debug-activities"></a>Activiteiten voor fout opsporing
De volgende aangepaste activiteiten zijn beschikbaar voor het opsporen van fouten in de werk stroom sjabloon.

### <a name="log-activity"></a>Logboek activiteit
De **logboek** activiteit wordt gebruikt om tekst berichten naar het logboek bestand te schrijven. Zie [logboek registratie](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx)voor meer informatie.

>[!NOTE]
>Als u de werk stroom niet eenvoudig kunt opsporen, kunt u de fout opsporing in de werk stroom in de productie omgeving proberen.  

Als u de **logboek** activiteit wilt gebruiken, stelt u de volgende eigenschappen in. De eigenschappen zijn zichtbaar wanneer u de activiteit in Workflow Designer selecteert en de **Eigenschappen** voor de activiteit bekijkt.
<!-- Properties missing from document -->
<!-- Image of Log activity GUI missing from document -->

### <a name="writeline-activity"></a>WriteLine activiteit
De **WriteLine** activiteit wordt gebruikt om tekst berichten te schrijven naar de schrijver van een provider. Als er geen schrijver beschikbaar is, schrijft de [WriteLine](http://127.0.0.1:47873/help/1-7016/ms.help?method=page&id=T%3ASYSTEM.ACTIVITIES.STATEMENTS.WRITELINE&product=VS&productVersion=100&topicVersion=100&locale=EN-US&topicLocale=EN-US&embedded=true) -activiteit de tekst naar het console venster.

<!-- Image of WriteLine activity GUI missing from document -->

In het tekstvak schrijft u het bericht dat u wilt weer geven in het schrijf doel.

>[!IMPORTANT]
>Het console venster kan niet worden gebruikt voor deze activiteit. Een andere Window-uitvoer schrijver gebruiken voor deze taak.

Als u de **WriteLine** -activiteit wilt gebruiken, stelt u de volgende eigenschappen in. De eigenschappen zijn zichtbaar wanneer u de activiteit in Workflow Designer selecteert en de **Eigenschappen** voor de activiteit bekijkt.

- **Logboek niveau**: Hiermee geeft u de hoeveelheid inhoud op die in de logboek waarde moet worden geschreven. De mogelijke waarden zijn:

    - Hoog: Schrijf het **LogText** -bericht naar het logboek bestand als de ernst van het logboek is ingesteld op hoog.
    - Uitgebreid: Schrijf het **LogText** -bericht naar het logboek bestand als de ernst van het logboek is ingesteld op uitgebreid.
    - Uitgeschakeld: niet schrijven in het logboek bestand.
- **LogText**: Hiermee geeft u de tekst inhoud op die in het logboek moet worden geschreven.
- **Tag**: Hiermee wordt een tag aan de tekst toegevoegd om het type inhoud te identificeren dat in het logboek wordt geschreven. De mogelijke waarden zijn: fout, tracering of waarschuwing.

<!-- log severity is not defined in this document -->


## <a name="next-steps"></a>Volgende stappen

- [Overzicht van de algemene webservice-connector](microsoft-identity-manager-2016-ma-ws.md)
- [Installeer het hulp programma voor configuratie van webservices](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP-implementatie handleiding](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Hand leiding voor REST-implementatie](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuratie van de web service-MA](microsoft-identity-manager-2016-ma-ws-maconfig.md)
