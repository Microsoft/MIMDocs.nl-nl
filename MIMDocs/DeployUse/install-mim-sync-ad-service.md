---
# required metadata

title: MIM 2016 installeren: Active Directory en de MIM-service synchroniseren | Microsoft Identity Manager
description: Gebruik beheeragents en de MIM-synchronisatieservice om Active Directory en de MIM-databases te synchroniseren.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# MIM 2016 installeren: Active Directory en de MIM-service synchroniseren

>[!div class=stapsgewijs]
[« MIM-service en -portal](install-mim-service-portal.md)

> [!NOTE]
> In deze stapsgewijze instructies wordt gebruikgemaakt van voorbeeldnamen en -waarden van een bedrijf met de naam Contoso. Vervang deze door uw eigen namen en waarden. Bijvoorbeeld:
> - Naam van de domeincontroller: **mimservername**
> - Domeinnaam: **contoso**
> - Wachtwoord: **Pass@word1**

Er zijn voor de MIM-synchronisatieservice (Sync) standaard geen connectoren geconfigureerd.  Normaal gesproken wordt eerst met MIM Sync de database voor de MIM-service gevuld met bestaande Active Directory-accounts. U gebruikt hiervoor de MIM-synchronisatieservice.

## De MIM-beheeragent maken
De MIM-beheeragent (MA) is een connector voor MIM Sync met de MIM-service. Als u deze connector wilt maken, gebruikt u de wizard voor het maken van beheeragents.

Wanneer u een MIM-beheeragent configureert, moet u een gebruikersaccount opgeven. In dit document wordt **MIMMA** gebruikt als de naam voor dit account.

> [!NOTE]
> Het account dat u voor de MIM-beheeragent gebruikt, moet hetzelfde account zijn als het account dat u tijdens de installatie van de MIM-service hebt opgegeven.

###De MIM-beheeragent maken

1.  Open Synchronization Service Manager.

2.  Klik in het menu **Acties** op **Maken** om de wizard voor het maken van de beheeragent te openen..

3.  Geef op de pagina **Beheeragent maken** de volgende instellingen op en klik vervolgens op **Volgende**.

    -   Beheeragent voor: beheeragent voor de MIM-service

    -   Naam: MIMMA

4.  Geef op de pagina **Verbinding maken met de database** de volgende instellingen op en klik vervolgens op **Volgende**

    -   Server: localhost

    -   Database: MIMService

    -   Basisadres van de MIM-service: http://localhost:5725

    -   Verificatiemodus: met Windows geïntegreerde verificatie

    -   Gebruikersnaam: mimma

    -   Wachtwoord: Pass@word

    -   Domein: contoso

5.  Controleer op de pagina **Geselecteerde objecttypen** of de objecttypen zijn geselecteerd die hieronder worden vermeld en klik vervolgens op **Volgende**

    -   ExpectedRuleEntry

    -   DetectedRuleEntry

    -   SynchronizationRule

    -   Persoon

    -   Groep

6.  Controleer op de pagina **Geselecteerde kenmerken** of alle vermelde kenmerken zijn geselecteerd en klik vervolgens op **Volgende**.

7.  Klik op de pagina **Connectorfilter configureren** op **Volgende**.

8.  Voeg op de pagina **Objecttypetoewijzingen configureren** de volgende toewijzing toe en klik vervolgens op **Volgende**

    - Selecteer **Persoon** in de lijst **Gegevensbronobjecttype**.
    - Klik op **Toewijzing toevoegen** om het dialoogvenster Toewijzing te openen.
    - Selecteer **Persoon** in de lijst **Metaverseobjecttype**.
    - Klik op **OK** om het dialoogvenster Toewijzing te sluiten.

9.  Pas op de pagina **Kenmerkstroom configureren** de volgende kenmerkstroomtoewijzingen toe en klik vervolgens op **Volgende**

    | **Stroomrichting** | **Kenmerk van de gegevensbron** | **Metaverse-kenmerk** |
    |-|-|-|
    |Importeren|Importeren|accountName|
    |Importeren|Importeren|bedrijf|
    |Importeren|Importeren|displayName|
    |Importeren|Importeren|employeeID|
    |Importeren|Importeren|employeeType|
    |Importeren|Importeren|firstName|
    |Importeren|Importeren|lastName|
    |Importeren|Importeren|Manager|
    |Importeren|Importeren|objectSid|
    |Exporteren|Exporteren|accountName|
    |Exporteren|Exporteren|bedrijf|
    |Exporteren|Exporteren|displayName|
    |Exporteren|Exporteren|domein|
    |Exporteren|Exporteren|employeeID|
    |Exporteren|Exporteren|employeeType|
    |Exporteren|Exporteren|firstName|
    |Exporteren|Exporteren|lastName|
    |Exporteren|Exporteren|manager|
    |Exporteren|Exporteren|objectSid|

10.  Selecteer **Persoon** als het gegevensbronobjecttype.

    -   Select **Person** as the Metaverse object type.

    -   Select **Direct** as the Mapping Type.

    -   For each row in the previous table, complete the following steps:

        -   Select the **Flow direction** shown for that row in the table.

        -   Select the **Data source attribute** shown for that row in the table.

        -   Select the **Metaverse attribute** shown for that row in the table.

        -   To apply the flow mapping, click **New**.

    -   Select **Group** as the data source type and as the metaverse object type.

    -   Select **Direct** as the Mapping Type.

    -   For each row in the following table, complete these steps:

        -   Select the **Flow direction** shown for that row in the table.

        -   Select the **Data source attribute** shown for that row in the table.

        -   Select the **Metaverse attribute** shown for that row in the table.

        -   To apply the flow mapping, click **New**.

    | Flow Direction | Data Source Attribute | Metaverse Attribute |
    |-|-|-|
    | Export | AccountName | accountName |
    | Export | DisplayName | displayName |
    | Export | Domain | domain |
    | Export | Scope | scope |
    | Export | Type | type |
    | Export | Member | member |
    | Export | MembershipLocked | membershipLocked |
    | Export | MembershipAddWorkflow | membershipAddWorkflow |
    | Export | Manager | manager |

11.  Klik op de pagina **Ongedaan maken van de inrichting configureren** op **Volgende**

12.  Als u de beheeragent op de pagina **Uitbreidingen configureren** wilt maken, klikt u op **Voltooien**.

## De AD-beheeragent maken
De Active Directory-beheeragent is een connector voor AD-domeinservices. Als u deze connector wilt maken, gebruikt u de wizard voor het maken van beheeragents.

1. Klik in het menu **Acties** op **Maken** om de wizard voor het maken van de beheeragent te openen..

2. Geef op de pagina **Beheeragent maken** de volgende instellingen op en klik vervolgens op **Volgende**:

    - Beheeragent voor: Active Directory-domeinservices
    - Naam: ADMA

3. Geef op de pagina **Verbinding maken met Active Directory-forest** de volgende instellingen op en klik vervolgens op **Volgende**

    - Forestnaam: contoso.local
    - Gebruikersnaam: beheerder
    - Wachtwoord: &lt;het wachtwoord voor het account&gt;
    - Domein: contoso

4. Geef op de pagina **Mappartities configureren** de volgende instellingen op en klik vervolgens op **Volgende**

    - Selecteer in de lijst **Mappartities selecteren** de optie **DC=CONTOSO, DC=local**.

    - Klik op **Containers** om het dialoogvenster Containers selecteren te openen.

    - Als u wilt instellen dat door MIM alleen objecten in een bepaalde container worden beheerd, klikt u op het knooppunt **DC=CONTOSO,DC=local** en klikt u vervolgens op het knooppunt voor de betreffende container.

    - Klik op **OK** om het dialoogvenster Containers selecteren te sluiten.

5. Klik op de pagina **Inrichtingshiërarchie configureren** op **Volgende**.

6. Geef op de pagina **Objecttypen selecteren** de volgende instellingen op en klik vervolgens op **Volgende**

    - Selecteer in de lijst **Objecttypen** de optie **Gebruiker** en **Groep**.

7. Geef op de pagina **Kenmerken selecteren** de volgende instellingen op en klik vervolgens op **Volgende**:

    - Selecteer **Alles weergeven**.

8. Selecteer in de lijst **Kenmerken** de volgende kenmerken:

    -   bedrijf
    -   displayName
    -   employeeID
    -   employeeType
    -   givenName
    -   groupType
    -   manager
    -   managedBy
    -   lid
    -   objectSid
    -   sAMAccountName
    -   sAMAccountType
    -   sn
    -   unicodePwd
    -   userAccountControl

9. Klik op de pagina **Connectorfilter configureren** op **Volgende**.

10. Klik op de pagina **Regels voor samenvoegen en projectie configureren** op **Volgende**.

11. Klik op de pagina **Kenmerkstroom configureren** op **Volgende**.

12. Klik op de pagina **Ongedaan maken van de inrichting configureren** op **Volgende**.

13. Klik op de pagina **Uitbreidingen configureren** op **Voltooien**.


## Uitvoeringsprofielen maken

Uitvoeringsprofielen maken voor de ADMA- en MIMMA-connectoren.

### Uitvoeringsprofielen maken voor de ADMA-connector

In de volgende tabel worden de vijf uitvoeringsprofielen weergegeven die u voor de ADMA-connector kunt maken:

| Naam | Type |
| ---- | ---- |
| Profiel1 | Volledige import (alleen faseren) |
| Profiel2 | Volledige synchronisatie |
| Profiel3 | Delta-import (alleen faseren) |
| Profiel4 | Deltasynchronisatie |
| Profiel5 | Exporteren |

Uitvoeringsprofielen maken voor de ADMA-connector:

1. Open Synchronization Service Manager en klik in het menu **Extra** op **Beheeragents**.

2. Selecteer in de lijst **Beheeragents** de optie **ADMA**.

3. Klik in het menu **Acties** op **Uitvoeringsprofielen configureren** om het dialoogvenster Uitvoeringsprofielen configureren voor te openen.

4. Voer voor elk uitvoeringsprofiel in de tabel de volgende stappen uit:

    - Klik op **Nieuw profiel** om de wizard Uitvoeringsprofiel configureren te openen.

    - Typ in het vak **Naam** de profielnaam uit de tabel en klik op **Volgende**.

    - Selecteer in de lijst **Type** het staptype uit de tabel en klik vervolgens op **Volgende**.

    - Klik op **Voltooien** om het uitvoeringsprofiel te maken.

5. Klik op **OK** om het dialoogvenster Uitvoeringsprofielen configureren te sluiten.

### Uitvoeringsprofielen maken voor de MIMMA-connector

In de volgende tabel worden de vijf overeenkomstige uitvoeringsprofielen weergegeven voor de MIMMA-connector:

| Naam | Type |
| -------- | -------- |
| Profiel1 | Volledige import (alleen faseren) |
| Profiel2 | Volledige synchronisatie |
| Profiel3 | Delta-import (alleen faseren) |
| Profiel4 | Deltasynchronisatie |
| Profiel5 | Exporteren |

U kunt als volgt uitvoeringsprofielen maken voor de MIMMA-connector:

1. Open Synchronization Service Manager en klik in het menu **Extra** op **Beheeragents**.

2. Selecteer in de lijst **Beheeragents** de optie **MIMMA**.

3. Klik in het menu **Acties** op **Uitvoeringsprofielen configureren** om het dialoogvenster Uitvoeringsprofielen configureren voor te openen.

4. Voer voor elk uitvoeringsprofiel in de tabel de volgende stappen uit:

    - Klik op **Nieuw profiel** om de wizard Uitvoeringsprofiel configureren te openen.

    - Typ in het vak **Naam** de profielnaam uit de tabel en klik op **Volgende**.

    - Selecteer in de lijst **Type** het staptype uit de tabel en klik vervolgens op **Volgende**.

    - Klik op **Voltooien** om het uitvoeringsprofiel te maken.

5. Klik op **OK** om het dialoogvenster Uitvoeringsprofielen configureren te sluiten.

## De MIM-service configureren

U maakt met de MIM-portal de synchronisatieregel voor binnenkomende gegevens van de AD-gebruiker voor de MIM-service.

U kunt als volgt de synchronisatieregel voor binnenkomende gegevens van de AD-gebruiker maken:

1. Klik op de startpagina van de MIM-portal op de navigatiebalk op **Beheer**.

2. Klik op **Synchronisatieregels** om de pagina met synchronisatieregels te openen.

3. Klik op de werkbalk op **Nieuw** om de wizard Synchronisatieregel maken te openen.

4. Geef op het tabblad **Algemeen** de volgende gegevens op en klik vervolgens op **Volgende**:

    -   Weergavenaam: synchronisatieregel voor binnenkomende gegevens van de AD-gebruiker
    -   Gegevensstroomrichting: binnenkomend

5. Geef op het tabblad **Bereik** de volgende gegevens op en klik vervolgens op **Volgende**:

    -   Resourcetype voor de metaverse: persoon
    -   Extern systeem: ADMA
    -   Resourcetype voor het externe systeem: persoon

6. Geef op het tabblad **Relatie** de volgende gegevens op en klik vervolgens op **Volgende**:

    -   Als u de Relatiecriteria wilt configureren, selecteert u **ObjectSID** in de lijst MetaverseObject:person(Attribute) en de lijst ConnectedSystemObject:person(Attribute).

    -   Selecteer **Resource maken in MIM**.

7. Geef op de pagina **Binnenkomende kenmerkstroom** de volgende gegevens op en klik vervolgens op **Volgende**:

    | Stroomregel | Bron | Bestemming |
    |-|-|-|
    |Regel 1|samAccountName|f|
    |Regel 2|displayName|displayName|
    |Regel 3|EmployeeType|EmployeeType|
    |Regel 4|givenName|givenName|
    |Regel 5|sn|lastName|
    |Regel 6|Manager|manager|
    |Regel 7|objectSID|ObjectSID|
    |Regel 8|Contoso|domein|

    Voer de volgende stappen uit voor elke rij in deze tabel:

    - Klik op **Nieuwe kenmerkstroom** om het dialoogvenster Stroomdefinitie te openen.

    - Selecteer op het tabblad **Bron** het kenmerk dat voor die rij in de tabel wordt weergegeven.

    - Selecteer op het tabblad **Bestemming** het kenmerk dat voor die rij in de tabel wordt weergegeven.

    - Klik op **OK** om de configuratie van de kenmerkstroom toe te passen.

8. Klik op het tabblad **Overzicht** op **Verzenden**.

## De testomgeving initialiseren
Er zijn vier stappen die u moet uitvoeren voordat u de MIM-configuratie met AD-gegevens kunt testen:

### De inrichting inschakelen

1. Open Synchronization Service Manager.

2. Klik in het menu **Extra** op **Opties** om het dialoogvenster Opties te openen

3. Selecteer **Inrichten van synchronisatieregel inschakelen**.

4. Klik op **OK** om het dialoogvenster Opties te sluiten.

### De MIMMA initialiseren

Voer een volledige synchronisatiecyclus uit voor deze connector. De volledige cyclus bestaat uit de volgende uitvoeringsprofielen:

- Volledig importbewerking
- Volledige synchronisatie
- Exporteren
- Delta-Import

Hanteer de volgende stappen om elk van de vier uitvoeringsprofielen uit te voeren.

1. Open Synchronization Service Manager en klik in het menu **Extra** op **Beheeragents**.

2. Selecteer in de lijst **Beheeragents** de optie **MIMMA**.

3. Klik in het menu **Acties** op **Uitvoeren** om het dialoogvenster Beheeragent uitvoeren te openen..

4. Voer voor elk uitvoeringsprofiel dat hierboven wordt vermeld de volgende stappen uit:

    - Klik in het menu **Acties** op **Uitvoeren** om het dialoogvenster Beheeragent uitvoeren te openen..

    - Selecteer in de lijst **Uitvoeringsprofielen** het uitvoeringsprofiel dat u wilt uitvoeren.

    - Klik op **OK** om het uitvoeringsprofiel te starten.

#### De kenmerkstroomvolgorde configureren

Tijdens de initialisatie van de MIM-connector zijn de geconfigureerde synchronisatieregels overgebracht naar de metaverse.

Stel de kenmerkstroomvolgorde in van de kenmerken die door deze connector worden bijgedragen zodat de kenmerken die zich al in AD bevinden naar de metaverse en later ook naar de database van de MIM-service kunnen stromen.

### De ADMA initialiseren

Als u de Active Directory-connector wilt initialiseren, moet u hiervoor een volledige import en een volledige synchronisatie uitvoeren. Bij de volledige import worden de bestaande objecten uit Active Directory naar het connectorgebied overgebracht. Bij de volledige synchronisatie worden de synchronisatieregels bijgewerkt zodat deze overeenkomen met die van de MIM-connector.

1. Open Synchronization Service Manager en klik in het menu **Extra** op **Beheeragents**.

2. Selecteer in de lijst **Beheeragents** de optie **ADMA**.

3. Klik in het menu **Acties** op **Uitvoeren** om het dialoogvenster Beheeragent uitvoeren te openen..

4. Voer voor elk uitvoeringsprofiel dat hierboven wordt vermeld de volgende stappen uit:

    - Klik in het menu **Acties** op **Uitvoeren** om het dialoogvenster Beheeragent uitvoeren te openen..

    - Selecteer in de lijst **Uitvoeringsprofielen** het uitvoeringsprofiel dat u wilt uitvoeren.

    - Klik op **OK** om het uitvoeringsprofiel te starten.

### De database voor de MIM-service vullen

Als u de database voor de MIM-service wilt vullen met de objecten, moet u een synchronisatiecyclus uitvoeren voor de MIMMA-connector. De cyclus bestaat uit:

- Exporteren
- Volledig importbewerking
- Volledige synchronisatie

Hanteer de volgende stappen om elk van de drie uitvoeringsprofielen uit te voeren.

1. Open Synchronization Service Manager en klik in het menu **Extra** op **Beheeragents**

2. Selecteer **MIMMA** in de lijst **Beheeragents**.

3. Klik op **Uitvoeren** in het menu **Acties** om het dialoogvenster Beheeragent uitvoeren te openen.

4. Voer voor elk uitvoeringsprofiel dat hierboven wordt vermeld de volgende stappen uit:

    - Klik op **Uitvoeren** in het menu **Acties** om het dialoogvenster Beheeragent uitvoeren te openen.
    - Selecteer in de lijst **Uitvoeringsprofielen** het uitvoeringsprofiel dat u wilt uitvoeren.
    - Klik op **OK** om het uitvoeringsprofiel te starten.

>[!div class=stapsgewijs]
[« MIM-service en -portal](install-mim-service-portal.md)


<!--HONumber=Apr16_HO4-->


