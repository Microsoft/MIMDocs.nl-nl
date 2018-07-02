---
title: Basisinstallatie van BHOLD | Microsoft Docs
description: BHOLD-suite installatie core document
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 752605be1392e514f5b132a654134185b38e2cef
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290132"
---
# <a name="bhold-core-installation"></a>BHOLD-Core-installatie

De Core BHOLD-module biedt de belangrijkste functies van BHOLD-Suite binnen uw omgeving. De module BHOLD Core moet worden geïnstalleerd en geconfigureerd op een server in uw lokale netwerk voordat u andere modules BHOLD-Suite kunt installeren.

## <a name="bhold-core-installation-requirements"></a>Vereisten voor BHOLD-Core-installatie

De module BHOLD Core vormt de basis van BHOLD-Suite van Microsoft. U moet de Core BHOLD-module installeren voordat u andere Microsoft-BHOLD-Suite-modules installeert.

### <a name="bhold-core-hardware-requirements"></a>BHOLD-Core hardwarevereisten

De module BHOLD Core vormt de basis van BHOLD-Suite van Microsoft. U moet de Core BHOLD-module installeren voordat u andere Microsoft-BHOLD-Suite-modules installeert.

|          |        |          |
|----------|--------|----------|
|**Onderdeel** |**Minimum** | **Aanbevolen** |
|Processor | 64-bits processor | Multicore 64-bits processor |
| Geheugen |3 GB | 6 GB of meer |
|Opslag| 30 GB beschikbaar |Afhankelijk van de grootte van de implementatie |
|Netwerkadapter| 100 Mbps-verbinding met SQL en Forefront Identity Manager (FIM)-server | 1 Gbps-verbinding met SQL en FIM-server|

Deze aanbevelingen zijn gebaseerd op de gebruikelijke implementaties en hebben geen rekening gehouden met andere toepassingen die op de server wordt uitgevoerd. Mogelijk moet u betere prestaties van onderdelen, afhankelijk van uw specifieke omgeving gebruiken.

### <a name="bhold-core-software-requirements"></a>BHOLD-Core softwarevereisten

De module BHOLD Core moet worden geïnstalleerd op een computer die voldoet aan de volgende vereisten:

- De server moet worden uitgevoerd op Windows Server 2012 (64-bits), Windows Server 2016 
- De server moet lid zijn van een domein met Active Directory Domain Services (AD DS). In een testomgeving moet de server een AD DS-domeincontroller.
- BHOLD-Core moet worden geïnstalleerd door een gebruiker aangemeld met een account in hetzelfde domein als de server en die aan de groep Domeinadministrators in het domein en aan de groep Administrators op de server behoort.
- Microsoft Internet Information Services (IIS) met ASP.NET moet worden geïnstalleerd op de server. IIS moet worden geconfigureerd met Windows-verificatie is ingeschakeld. Als BHOLD Core op Windows Server 2012-2016 wordt geïnstalleerd, kan de compatibiliteit met IIS 6-beheer moet worden geïnstalleerd. Als BHOLD Core wordt geïnstalleerd op Windows Server 2012, moeten IIS 6-scripthulpprogramma's worden geïnstalleerd
- Het framework .NET 3.5 moet worden geïnstalleerd.
  - Omdat BHOLD Core .NET 3.5 is vereist, kan niet worden geïnstalleerd op Server Core.
- Silverlight 4 is vereist voor andere BHOLD-modules en daarom wordt het aanbevolen installeren voordat u BHOLD Core installeert.
- Microsoft SQL Server 2014 of Microsoft SQL Server 2016 moet zijn geïnstalleerd op de BHOLD-Core-server of op een andere server op het LAN. 

Windows Client-computers moeten worden uitgevoerd Microsoft Internet Explorer versie 6 of hoger en versie van Microsoft Silverlight 4 of hoger.

## <a name="before-you-begin"></a>Voordat u begint

De module BHOLD Core vereist een gebruikersaccount dat wordt gebruikt om te verifiëren en autoriseren van de Kernservice van BHOLD bij de server en andere netwerkentiteiten. Deze sectie wordt uitgelegd hoe u maken en configureren van die gebruikersaccount en biedt een voorinstallatie werkblad waarmee u het verzamelen van informatie die u nodig hebt om BHOLD Core-installatie te voltooien.

>{! BELANGRIJK} bij het installeren van BHOLD-Core, kunt u een bestaande SQL Server-database als de Core BHOLD-database of kunt u de wizard Setup van BHOLD-Core de Core BHOLD-database voor u te maken. Als u een bestaande database gebruiken, moet u ervoor zorgen dat er geen andere gebruikers toegang de database tot kunnen tijdens de installatie van BHOLD-Core. Controleer voordat u installeert BHOLD-Core, of dat de toegangscontrole van de SQL Server-database niet is toegestaan door iemand anders dan de gebruiker die BHOLD Core installeert.

## <a name="required-user-and-group"></a>Vereiste gebruikers en groepen

De module BHOLD Core moeten kunnen aanmelden bij het domein met een gebruikersaccount die is toegewezen aan daarvoor en die lid is van twee specifieke beveiligingsgroepen, waaronder een die specifiek voor de Core BHOLD-module is gemaakt. Lidmaatschap van de groep Domeinadministrators is vereist voor deze procedure.
**Maken en configureren van de groep van gebruikers en beveiligingsgroepen BHOLD Core**

1.  Klik op een domeincontroller op **Start**, wijs **Systeembeheer**, en klik vervolgens op **Active Directory: gebruikers en Computers**.

2.  Vouw het domein waarin het account worden gemaakt, zich in de consolestructuur met de rechtermuisknop op **gebruikers**, wijs **nieuw**, en klik vervolgens op **groep**.

3.  In de **Nieuw Object – groep** het dialoogvenster **groepsnaam**, typ de naam van de groep (BHOLD standaard: BHOLDApplicationGroup), en klik vervolgens op **OK**.

4.  Met de rechtermuisknop op **gebruikers**, wijs **nieuw**, en klik vervolgens op **gebruiker**.

5.  In **volledige naam**, typ een naam waarmee u het account, bijvoorbeeld BHOLD Core Service Account identificeren.

6.  In **aanmeldingsnaam van gebruiker**, typ de gebruikersnaam van het serviceaccount van BHOLD-Core (BHOLD standaard: b1user), en klik vervolgens op **volgende**.

7.  In **wachtwoord** en **wachtwoord bevestigen**, typt u het wachtwoord voor het serviceaccount.

8.  Schakel **gebruiker moet wachtwoord bij volgende aanmelding wijzigen**, selecteer **gebruiker kan wachtwoord niet wijzigen** en **wachtwoord verloopt nooit**, klikt u op **volgende**, en klik vervolgens op **voltooien**.

9.  In het resultatenvenster van de console met de rechtermuisknop op het gebruikersaccount en klik vervolgens op **toevoegen aan een groep**.

10. In de **groepen selecteren** in het dialoogvenster typt u de weergavenaam van de groep die u eerder hebt gemaakt, typt u een puntkomma (;) en typ vervolgens IIS_IUSRS.

11. Klik op **namen controleren**, en klik vervolgens op **OK**.  

De volgende procedure moet worden uitgevoerd op de computer waarop de Core BHOLD-module zich moet worden geïnstalleerd. U moet zijn aangemeld als lid van de groep Domeinadministrators deze procedure uitvoeren.

## <a name="bhold-core-installation-worksheet"></a>Werkblad van BHOLD-Core-installatie

Voordat u begint met het installeren van de module BHOLD-Core, moet u worden voorbereid voor de informatie die de wizard Setup van BHOLD-Core vereist om de installatie te voltooien. Het werkblad voor het volgende kunt u gegevens vastleggen, zodat u gereed om te leveren wanneer deze nodig is. Elke sectie komt overeen met een pagina in de wizard BHOLD Core-installatie.

### <a name="account-settings"></a>Accountinstellingen

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                                                                                                             | **Waarde**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beveiligingsprovider worden gebruikt op de computer van het domein /** | Wanneer u selecteert, geeft u aan dat Active Directory Domain Services-beveiliging de toegang tot BHOLD Core beheert.                                                                                                                                                                                                  | Schakel het selectievakje in. **Belangrijk:** mislukt de installatie als dit selectievakje niet is ingeschakeld.                                                                 |
| **Domein**                                  | Hiermee geeft u het domein met de BHOLD-server, service-account en toepassingsgroep. **Belangrijk:** de domeinnaam opgeven met de naam van de NetBIOS-(korte), niet de volledig gekwalificeerde domeinnaam (FQDN). Bijvoorbeeld, als de FQDN-naam van het domein fabrikam.com is, geef de domeinnaam als CONTOSO. | Schrijf hier de naam van het domein:                                                                                                                                        |
| **Toepassingsgroep**                       | Geeft de naam van de beveiligingsgroep die u eerder hebt gemaakt in [vereiste gebruikers en groepen](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Schrijf hier de naam van de groep:                                                                                                                                         |
| **Gebruiker voor de service**                            | Hiermee geeft u de aanmeldingsnaam van de service-gebruikersaccount dat u eerder hebt gemaakt in [vereiste gebruikers en groepen](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Schrijf hier de accountnaam van de gebruiker:                                                                                                                                  |
| **Wachtwoord**                                | Hiermee geeft u het wachtwoord van het gebruikersaccount van de Core BHOLD-service.                                                                                                                                                                                                                                              | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                |
| **IP/poort van website**                         | Hiermee geeft u het aantal IP-adres en poort van de website op de intranetserver moeten worden gemaakt. Wijzig de standaardwaarde (\*) alleen als u wordt niet hetzelfde IP-adres als de standaardwebsite. Wijzig het poortnummer moet een beschikbare poort wordt alleen als de standaardpoort (5151) al gebruikt.             | Als een niet-standaard IP-adres wordt gebruikt door de standaardwebsite, schrijf deze hier: als het standaardpoortnummer al in gebruik is, schrijf het BHOLD website poortnummer hier: |

### <a name="database-settings"></a>Database-instellingen

| **Item**                                       | **Beschrijving**                                                                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Geïntegreerde beveiliging gebruiken**                    | Hiermee geeft u op dat de Windows-verificatie wordt gebruikt voor toegang tot de database.                                                                                                                                                                                                     | Schakel het selectievakje in als Windows-verificatie wordt gebruikt voor verbinding met de SQL-Server. Schakel het selectievakje in als SQL Server-verificatie wordt gebruikt. De database moet zijn gemaakt voordat uitgevoerd BHOLD Core Setup als SQL Server-verificatie wordt gebruikt. **Opmerking:** als Windows-verificatie wordt gebruikt, u moet zijn aangemeld met een account met de serverrol sysadmin op de databaseserver. |
| **Databasegebruiker** en **databasewachtwoord** | Hiermee geeft u de gebruikersnaam en wachtwoord van een gebruiker met de serverrol sysadmin op de databaseserver. Deze waarden worden geleverd, alleen wanneer SQL Server-verificatie wordt gebruikt.                                                                                               | Schrijf hier de gebruikersnaam van de SQL Server: SQL Server-gebruikerswachtwoord hier schrijven: **Opmerking:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                                                  |
| **Databaseserver** en **databasenaam**   | Hiermee geeft u de NetBIOS-naam van de databaseserver en de naam van de database (standaard: b1) die BHOLD Core-installatie maakt. Als u niet het standaardexemplaar van de database-server gebruikt, geeft u de database-server-exemplaar in het formulier  *\<server\>*\\*\<exemplaar\>* . | De server (of server en -exemplaar) naam hier schrijven: Schrijf hier de naam van de database:                                                                                                                                                                                                                                                                                                                   |
| **Beperkingen voor de database-gebruikersaccount maken**    | Verouderd.                                                                                                                                                                                                                                                                 | Wijzig de standaardwaarde niet                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>BHOLD-Core-installatie

Voor het installeren van de module BHOLD-Core, meld u aan als lid van de groep Domeinadministrators, moet het volgende bestand downloaden en uitvoeren als beheerder op de server die u wilt de Core BHOLD-module installeren op: 

- BholdCore  *\<versie\>*\_Release.msi

Vervang *\<versie\>* met het versienummer van de Core BHOLD-versie die u installeert.

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

### <a name="postinstallation-settings"></a>Postinstallation instellingen

Nadat BHOLD Core-installatie is voltooid, moet u de Windows Firewall configureren en geavanceerde instellingen wijzigen in de groep van toepassingen BHOLD-Core in Internet Information Services om de Core BHOLD-configuratie te voltooien. Indien nodig, moet u ook BHOLD-kenmerken om te voldoen aan uw vereisten.

#### <a name="configuring-windows-firewall"></a>Windows Firewall configureren

Als gebruikers toegang BHOLD tot met behulp van webbrowsers op externe computers, moet u Windows Firewall configureren op de server Core BHOLD voor het toestaan van binnenkomende verbindingen voor de poort van website die u hebt opgegeven tijdens de installatie van BHOLD-Core.

Om deze procedure uitvoert, moet u lid zijn van de groep Administrators op de lokale computer.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Voor het toestaan van binnenkomende verbindingen naar de website BHOLD

1.  Klik op **Start**, wijs **Systeembeheer**, met de rechtermuisknop op **Windows Firewall met geavanceerde instellingen**, en klik vervolgens op **als administratoruitvoeren**.

2.  Klik in het linkerdeelvenster op **regels voor binnenkomende verbindingen**, en klik vervolgens in het rechterdeelvenster op **nieuwe regel**.

3.  Klik in de wizard nieuwe regel voor binnenkomende verbindingen op **poort**, en klik vervolgens op **volgende**.

4.  Zorg ervoor dat **TCP** is geselecteerd in **specifieke lokale poorten**, typt u het standaardpoortnummer voor BHOLD Core (5151) of het poortnummer dat u hebt opgegeven als u BHOLD Core geïnstalleerd en klik vervolgens op  **Volgende**.

5.  Zorg ervoor dat **de verbinding toestaan** is geselecteerd en klik vervolgens op **volgende**.

6.  Op de **profiel** pagina, schakel de selectievakjes uit voor de locaties van waaruit u niet wilt dat toegang tot de BHOLD-website, en klik vervolgens op **volgende**.

7.  Op de **naam** pagina, typ een naam voor de regel (zoals het toestaan van binnenkomende verbindingen voor BHOLD-Core) en klik vervolgens op **voltooien**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>32-bits toepassingen voor de toepassingsgroep BHOLD Core inschakelen

Als u wilt toestaan dat IIS goed te laten werken met de module BHOLD-Core, moet u de groep van toepassingen BHOLD Core ter ondersteuning van 32-bits toepassingen configureren. U moet zijn aangemeld met het account dat wordt gebruikt voor het installeren van de module BHOLD Core als deze procedure wilt uitvoeren.

**32-bits toepassingsondersteuning voor de groep van toepassingen BHOLD Core inschakelen**

1.  Internet Information Services Manager, klikt u op **Start**, wijs **Systeembeheer**, en klik vervolgens op **Internet Information Services (IIS) Manager**.

2.  Vouw de servernaam in de consolestructuur en klik vervolgens op **toepassingsgroepen**.

3.  De standaardwaarde is Y.

4.  Ingesteld op N als u niet dat de wijzigingen wilt moeten worden vastgelegd.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>SystemQueue verwerking

Als u niet dat systeem wachtrijbewerkingen wilt ingesteld op N. Wijzig deze waarde niet tenzij omgeleid naar dit doen door productondersteuning. Een systeemkenmerk BHOLD instellen

> [!IMPORTANT]
> Klik op Start, klikt u op alle programma's, en klik vervolgens op Internet Explorer. Typ in het adresvak, waarbij  server  is de naam van de BHOLD-websiteserver en  poort  is het poortnummer dat is gebonden aan de website. Klik op Start, klikt u op waarden, en klik vervolgens op wijzigen.

Lidmaatschap van **Domeinadministrators**, of gelijkwaardig, is de minimale vereiste om deze procedure te voltooien.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Zoek de naam van het kenmerk dat u wilt wijzigen, typt u de nieuwe waarde in het vak naast de naam van het kenmerk en klik vervolgens op OK.

1.  Nadat u hebt geïnstalleerd BHOLD-Core en gevalideerd of de installatie geslaagd is, kunt u aanvullende modules installeren.

2.  De BHOLD-database zal op dit moment niet in wezen leeg is, met slechts één gebruikersaccount, het root-account en een organisatie-eenheid (orgunit), de hoofdmap orgunit.

    -   *\<Als u wilt meer gebruikers toevoegen aan de BHOLD-database, kunt u ofwel de module Access Management-Connector of de Generator van BHOLD-Model, afhankelijk van uw behoeften installeren.

    -   *\<U kunt de module Access Management-Connector gebruiken om gebruikersgegevens te importeren uit de FIM-synchronisatieservice of u kunt de Generator BHOLD-Model gebruiken om gebruikersgegevens te importeren uit een reeks gestructureerde bestanden.

3.  Zie voor meer informatie over het gebruik van de module Access Management-Connector Test Lab Guide: BHOLD Access Management-Connector.

#### <a name="setting-bhold-system-attributes"></a>Zie voor meer informatie over het gebruik van de Generator van BHOLD-Model-module:

Microsoft BHOLD-Suite concepten handleiding Microsoft BHOLD-Suite TechnicalReference.

| **Kenmerk**                | **Beschrijving**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Ingesteld op Y als de BHOLD-website wordt uitgevoerd op een geclusterde webservice om ervoor te zorgen dat onlangs weergegeven items correct werken. Ingesteld op N als de BHOLD-website op een zelfstandige IIS-server wordt uitgevoerd.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Ingesteld op j om ervoor te zorgen dat organisatie-eenheden (orgunits) alleen naar orgunits met dezelfde organisatie-type als de bovenliggende orgunit kunnen worden verplaatst. Bijvoorbeeld: dit voorkomt dat een project orgunit wordt verplaatst naar een orgunit afdeling. Ingesteld op N om toe te staan een orgunit in een orgunit van een ander type worden geplaatst. |
| **Dagen tussen ABA uitvoeren**     | Ingesteld op een geheel getal zijn twee cijfers Geef het interval (in dagen) tussen twee kenmerk gebaseerde verificatie (ABA) wordt uitgevoerd. Typ bijvoorbeeld wilt opgeven dat wordt uitgevoerd ABA worden gescheiden door twee dagen, 02.                                                                                                                     |
| **Beginuur van ABA uitvoeren**    | Ingesteld op een geheel getal zijn twee cijfers geeft het uur van de dag wanneer een kenmerk gebaseerde verificatie uitvoeren wordt uitgevoerd. Bijvoorbeeld, vindt als u wilt opgeven die de ABA uitvoeren plaats om 11:00 uur (23:00), typt u 23.                                                                                                             |
| **De kardinaliteit van het systeem**       | Als u niet dat een systeemcontrole kardinaliteit in BHOLD wilt ingesteld op N. De standaardwaarde is Y.                                                                                                                                                                                                                             |
| **Logboekregistratie**                  | Ingesteld op N als u niet dat de wijzigingen wilt moeten worden vastgelegd. De standaardwaarde is Y.                                                                                                                                                                                                                                            |
| **SystemQueue verwerking**   | Als u niet dat systeem wachtrijbewerkingen wilt ingesteld op N. Wijzig deze waarde niet tenzij omgeleid naar dit doen door productondersteuning.                                                                                                                                                                                           |

U moet zijn aangemeld als lid van de groep Domeinadministrators deze procedure uitvoeren.

**Een systeemkenmerk BHOLD instellen**

1.  Klik op **Start**, klikt u op **alle programma's**, en klik vervolgens op **Internet Explorer**.

2.  Typ in het adresvak, waarbij *\<server\>* is de naam van de BHOLD-websiteserver en *\<poort\>* is het poortnummer dat is gebonden aan de website.

3.  Klik op **Start**, klikt u op **waarden**, en klik vervolgens op **wijzigen**.

4.  Zoek de naam van het kenmerk dat u wilt wijzigen, typt u de nieuwe waarde in het vak naast de naam van het kenmerk en klik vervolgens op **OK**.

## <a name="next-steps"></a>Volgende stappen

Nadat u hebt geïnstalleerd BHOLD-Core en gevalideerd of de installatie geslaagd is, kunt u aanvullende modules installeren. De BHOLD-database zal op dit moment niet in wezen leeg is, met slechts één gebruikersaccount, het root-account en een organisatie-eenheid (orgunit), de hoofdmap orgunit. Als u wilt meer gebruikers toevoegen aan de BHOLD-database, kunt u ofwel de module Access Management-Connector of de Generator van BHOLD-Model, afhankelijk van uw behoeften installeren. U kunt de module Access Management-Connector gebruiken om gebruikersgegevens te importeren uit de FIM-synchronisatieservice of u kunt de Generator BHOLD-Model gebruiken om gebruikersgegevens te importeren uit een reeks gestructureerde bestanden. Zie voor meer informatie over het gebruik van de module Access Management-Connector [Test Lab Guide: BHOLD Access Management-Connector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

Zie voor meer informatie over het gebruik van de Generator van BHOLD-Model-module:

- [Microsoft BHOLD-Suite concepten handleiding](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD-Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
