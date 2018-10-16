---
title: Basisinstallatie van BHOLD | Microsoft Docs
description: BHOLD-suite installatie core document
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 4499c2846655ff75462794d684cae44f03134f1c
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333527"
---
# <a name="bhold-core-installation"></a>Basisinstallatie van BHOLD

De kern van BHOLD-module biedt de belangrijkste functies van BHOLD-Suite binnen uw omgeving. De kern van BHOLD-module moet worden geïnstalleerd en geconfigureerd op een server in uw lokale netwerk voordat u andere modules BHOLD-Suite kunt installeren.

## <a name="bhold-core-installation-requirements"></a>Vereisten voor installatie van BHOLD-Core

De module BHOLD Core vormt de basis voor BHOLD-Suite van Microsoft. U moet de BHOLD-Core-module installeren voordat u andere Microsoft-BHOLD-Suite-modules installeert.

### <a name="bhold-core-hardware-requirements"></a>BHOLD-Core hardwarevereisten

De module BHOLD Core vormt de basis voor BHOLD-Suite van Microsoft. U moet de BHOLD-Core-module installeren voordat u andere Microsoft-BHOLD-Suite-modules installeert.

|          |        |          |
|----------|--------|----------|
|**Onderdeel** |**Minimum** | **Aanbevolen** |
|Processor | 64-bits processor | Multicore 64-bits processor |
| Geheugen |3 GB | 6 GB of meer |
|Opslag| 30 GB beschikbaar |Afhankelijk van de implementatiegrootte van de |
|-Netwerkadapter| 100 Mbps-verbinding met SQL en Forefront Identity Manager (FIM)-server | 1 Gbps-verbinding met SQL en FIM-server|

Deze aanbevelingen zijn gebaseerd op normale implementaties en hebben geen rekening gehouden met andere toepassingen die op de server. Mogelijk moet u betere prestaties van onderdelen, afhankelijk van uw specifieke omgeving gebruiken.

### <a name="bhold-core-software-requirements"></a>BHOLD-Core softwarevereisten

De kern van BHOLD-module moet worden geïnstalleerd op een computer die voldoet aan de volgende vereisten:

- De server moet worden uitgevoerd op Windows Server 2012 (64-bits), Windows Server 2016 
- De server moet lid zijn van een Active Directory Domain Services (AD DS)-domein. De server mogelijk in een testomgeving, een AD DS-domeincontroller.
- BHOLD-Core moet worden geïnstalleerd door een gebruiker aan te melden met een account in hetzelfde domein als de server en die aan de groep Domeinadministrators in het domein en aan de groep Administrators op de server behoort.
- Microsoft Internet Information Services (IIS) met ASP.NET moet worden geïnstalleerd op de server. IIS moet worden geconfigureerd met Windows-verificatie ingeschakeld. Als BHOLD Core wordt geïnstalleerd op Windows Server 2012/2016, kan de compatibiliteit met IIS 6-beheer moet worden geïnstalleerd. Als BHOLD Core wordt geïnstalleerd op Windows Server 2012, moeten IIS 6-scripthulpprogramma's worden geïnstalleerd
- De .NET 3.5 framework moet worden geïnstalleerd.
  - Omdat BHOLD Core vereist .NET 3.5 is, kan niet worden geïnstalleerd op Server Core.
- Silverlight 4 is vereist voor andere BHOLD-modules en dus is het aanbevolen voor de installatie van BHOLD-Core te installeren.
- Microsoft SQL Server 2014 of Microsoft SQL Server 2016 moet worden geïnstalleerd op de BHOLD-Core-server of op een andere server op het LAN. 

Windows Client-computers moeten worden uitgevoerd Microsoft Internet Explorer versie 6 of hoger en Microsoft Silverlight-versie 4 of hoger.

## <a name="before-you-begin"></a>Voordat u begint

De BHOLD-Core-module is een gebruikersaccount dat wordt gebruikt om te verifiëren en autoriseren de BHOLD-Core-service met de server en andere netwerkentiteiten vereist. In deze sectie wordt uitgelegd hoe u die gebruikersaccount maken en configureren en biedt een werkblad voor het vooraf installeren waarmee u kunt verzamelen van informatie die u nodig hebt om BHOLD Core-installatie te voltooien.

>{! BELANGRIJKE} bij het installeren van BHOLD-Core, kunt u een bestaande SQL Server-database als de kern van BHOLD-database of kunt u de wizard Setup van BHOLD-Core de kern van BHOLD-database voor u te maken. Als u ervoor kiest een bestaande database wilt gebruiken, moet u ervoor zorgen dat er geen andere gebruikers toegang heeft tot de database tijdens de installatie van BHOLD-Core. Voor de installatie van BHOLD-Core, Controleer of de besturingselementen voor toegang van de SQL Server-database staan niet toe voor toegang door iedereen dan de gebruiker die is installatie van BHOLD-Core.

## <a name="required-user-and-group"></a>Vereiste gebruikers en groepen

De kern van BHOLD-module moet kunnen aanmelden op het domein met een gebruikersaccount dat is toegewezen aan dit doel en die deel uitmaakt van de twee specifieke beveiligingsgroepen, waaronder een die speciaal voor de BHOLD-Core-module is gemaakt. Lidmaatschap van de groep Domeinadministrators is vereist voor deze procedure uitvoeren.
**Maken en configureren van de groep van gebruikers en beveiligingsgroepen BHOLD Core**

1.  Klik op een domeincontroller, **Start**, wijst u **Systeembeheer**, en klik vervolgens op **Active Directory: gebruikers en Computers**.

2.  Vouw het domein waarin het account moet worden gemaakt, zich in de consolestructuur met de rechtermuisknop op **gebruikers**, wijst u **nieuw**, en klik vervolgens op **groep**.

3.  In de **Nieuw Object – groep** in het dialoogvenster **groepsnaam**, typ de naam van de groep (BHOLD-standaard: BHOLDApplicationGroup), en klik vervolgens op **OK**.

4.  Met de rechtermuisknop op **gebruikers**, wijst u **nieuw**, en klik vervolgens op **gebruiker**.

5.  In **volledige naam**, typ een naam waarmee u dit account, bijvoorbeeld BHOLD Core-serviceaccount worden geïdentificeerd.

6.  In **aanmeldingsnaam van gebruiker**, typ de naam van de gebruiker van het serviceaccount van BHOLD-Core (BHOLD-standaard: b1user), en klik vervolgens op **volgende**.

7.  In **wachtwoord** en **wachtwoord bevestigen**, typt u het wachtwoord voor het serviceaccount.

8.  Schakel **gebruiker moet wachtwoord bij volgende aanmelding wijzigen**, selecteer **gebruiker kan wachtwoord niet wijzigen** en **wachtwoord verloopt nooit**, klikt u op **volgende**, en klik vervolgens op **voltooien**.

9.  In het resultatenvenster van de console met de rechtermuisknop op het gebruikersaccount en klik vervolgens op **toevoegen aan een groep**.

10. In de **groepen selecteren** in het dialoogvenster, typt u de weergavenaam van de groep die u eerder hebt gemaakt, typt u een puntkomma (;) en typ vervolgens IIS_IUSRS.

11. Klik op **namen controleren**, en klik vervolgens op **OK**.  

De volgende procedure moet worden uitgevoerd op de computer waarop de BHOLD-Core-module zich moet worden geïnstalleerd. U moet zijn aangemeld als lid van de groep Domeinadministrators deze procedure uitvoeren.

## <a name="bhold-core-installation-worksheet"></a>Werkblad voor BHOLD-Core-installatie

Voordat u begint met het installeren van de module BHOLD-Core, moet u worden voorbereid voor de informatie die de wizard Setup van BHOLD-Core is vereist om de installatie te voltooien. Het werkblad voor het volgende kunt u gegevens vastleggen, zodat u klaar om aan te geven wanneer dat nodig is. Elke sectie komt overeen met een pagina in de wizard setup van BHOLD-Core.

### <a name="account-settings"></a>Accountinstellingen

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                                                                                                             | **Waarde**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Security Provider worden gebruikt op de computer aan het domein /** | Als er hebt geselecteerd, geeft de Active Directory Domain Services-beveiliging wordt toegangsbeheer voor BHOLD-Core.                                                                                                                                                                                                  | Schakel het selectievakje in. **Belangrijk:** mislukt de installatie als dit selectievakje niet is geselecteerd.                                                                 |
| **Domein**                                  | Hiermee geeft u het domein waarin de BHOLD server, de service-account en de groep van toepassingen. **Belangrijk:** de domeinnaam opgeven met behulp van de naam van de NetBIOS-(kort), niet de volledig gekwalificeerde domeinnaam (FQDN). Bijvoorbeeld, als de FQDN-naam van het domein fabrikam.com, de domeinnaam opgeven als CONTOSO. | Schrijf hier de naam van het domein:                                                                                                                                        |
| **Groep van toepassingen**                       | Hiermee geeft u de naam van de beveiligingsgroep die u eerder hebt gemaakt in [vereist van gebruikers en groepen](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Schrijf hier de naam van de groep:                                                                                                                                         |
| **De gebruiker van service**                            | Hiermee geeft u de naam van het serviceaccount van de gebruiker die u eerder hebt gemaakt in [vereist van gebruikers en groepen](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Schrijf hier de accountnaam van de gebruiker:                                                                                                                                  |
| **Wachtwoord**                                | Hiermee geeft u het wachtwoord van het gebruikersaccount van BHOLD-Core-service.                                                                                                                                                                                                                                              | Schrijf hier het wachtwoord: **belangrijk:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                |
| **IP/poort van website**                         | Hiermee geeft u het IP-adres en poort nummer van de website moet worden gemaakt op de intranetserver. De standaardwaarde wijzigen (\*) alleen als u niet hetzelfde IP-adres als de standaardwebsite gebruikt. Wijzig het poortnummer in een beschikbare poort wordt alleen als de standaardpoort (5151) al gebruikt.             | Als een niet-standaard IP-adres wordt gebruikt door de standaardwebsite, schrijf deze hier: als het standaardpoortnummer al in gebruik is, schrijf het BHOLD-website poortnummer hier: |

### <a name="database-settings"></a>Database-instellingen

| **Item**                                       | **Beschrijving**                                                                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Geïntegreerde beveiliging gebruiken**                    | Hiermee geeft u op dat Windows-verificatie wordt gebruikt voor toegang tot de database.                                                                                                                                                                                                     | Selecteer het selectievakje in als Windows-verificatie wordt gebruikt om verbinding maken met de SQL-Server. Schakel dit selectievakje uit als SQL Server-verificatie wordt gebruikt. De database moet zijn gemaakt voordat het uitvoeren van BHOLD Core Setup als SQL Server-verificatie wordt gebruikt. **Opmerking:** als Windows-verificatie wordt gebruikt, u moet zijn aangemeld met een account met de serverrol sysadmin op de database-server. |
| **Databasegebruiker** en **databasewachtwoord** | Hiermee geeft u de gebruikersnaam en wachtwoord van een gebruiker met de serverrol sysadmin op de databaseserver. Deze waarden worden geleverd alleen wanneer SQL Server-verificatie wordt gebruikt.                                                                                               | Schrijf hier de gebruikersnaam van de SQL Server: SQL Server-gebruikerswachtwoord hier schrijven: **Opmerking:** Zorg ervoor dat dit wachtwoord in een verborgen, een veilige locatie.                                                                                                                                                                                                                                                  |
| **Databaseserver** en **databasenaam**   | Hiermee geeft u de NetBIOS-naam van de database-server en de naam van de database (standaard: b1) die BHOLD Core door Setup wordt gemaakt. Als u niet het standaardexemplaar van de database-server gebruikt, geeft u de database-server-exemplaar in de notatie  *\<server\>*\\*\<exemplaar\>* . | De server (of server en exemplaar) naam hier schrijven: Schrijf hier de naam van de database:                                                                                                                                                                                                                                                                                                                   |
| **Beperkingen voor de databasegebruiker maken**    | Verouderd.                                                                                                                                                                                                                                                                 | Wijzig de standaardwaarde niet                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>Core-installatie van BHOLD

Meld u aan als een lid van de groep Domeinadministrators voor het installeren van de module BHOLD-Core, downloadt u het volgende bestand en als administrator uitvoeren op de server die u van plan bent de BHOLD-Core-module installeren op: 

- BholdCore  *\<versie\>*\_Release.msi

Vervang *\<versie\>* met het versienummer van de Core BHOLD-versie die u installeert.

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

### <a name="postinstallation-settings"></a>Instellingen voor postinstallation

Nadat BHOLD Core-installatie is voltooid, moet u de Windows Firewall configureren en wijzigen van de geavanceerde instellingen in de toepassingsgroep van BHOLD-Core in Internet Information Services om de kern van BHOLD-configuratie te voltooien. Indien nodig, moet u ook BHOLD systeemkenmerken om te voldoen aan uw vereisten.

#### <a name="configuring-windows-firewall"></a>Windows Firewall configureren

Als gebruikers toegang BHOLD via webbrowsers op externe computers, moet u Windows Firewall configureren op de server Core van BHOLD waarmee binnenkomende verbindingen naar de poort van website die u hebt opgegeven tijdens de installatie van BHOLD-Core.

Om deze procedure uitvoert, moet u lid zijn van de groep Administrators op de lokale computer.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Om toe te staan van binnenkomende verbindingen naar de website van BHOLD

1.  Klik op **Start**, wijst u **Systeembeheer**, met de rechtermuisknop op **Windows Firewall met geavanceerde instellingen**, en klik vervolgens op **als administratoruitvoeren**.

2.  Klik in het linkerdeelvenster op **regels voor binnenkomende verbindingen**, en klik vervolgens in het rechter deelvenster op **nieuwe regel**.

3.  Klik in de wizard nieuwe regel voor binnenkomende verbindingen op **poort**, en klik vervolgens op **volgende**.

4.  Zorg ervoor dat **TCP** is geselecteerd, in **specifieke lokale poorten**, typt u het standaardpoortnummer voor BHOLD-Core (5151) of het poortnummer dat u hebt opgegeven wanneer u BHOLD Core geïnstalleerd en klik vervolgens op  **Volgende**.

5.  Zorg ervoor dat **de verbinding toestaan** is geselecteerd en klik vervolgens op **volgende**.

6.  Op de **profiel** pagina, schakel de selectievakjes uit voor locaties van waaruit u niet wilt dat de toegang tot de website van BHOLD, en klik vervolgens op **volgende**.

7.  Op de **naam** pagina, typ een naam voor de regel (zoals het toestaan van binnenkomende verbindingen voor BHOLD-Core) en klik vervolgens op **voltooien**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>32-bits toepassingen voor de toepassingsgroep van BHOLD-Core inschakelen

Als u wilt toestaan dat IIS goed te laten werken met de module BHOLD-Core, moet u de groep van toepassingen BHOLD Core ter ondersteuning van 32-bits toepassingen configureren. U moet zijn aangemeld met het account dat wordt gebruikt voor het installeren van de BHOLD-Core-module om deze procedure uitvoert.

**32-bits toepassingsondersteuning voor de toepassingsgroep van BHOLD-Core inschakelen**

1.  Internet Information Services Manager, klikt u op **Start**, wijst u **beheerhulpprogramma's voor**, en klik vervolgens op **Internet Information Services (IIS) Manager**.

2.  Vouw de servernaam in de consolestructuur en klik vervolgens op **toepassingsgroepen**.

3.  In de **toepassingsgroepen** lijst, met de rechtermuisknop op **CoreAppPool**, en klik vervolgens op **geavanceerde instellingen**.

4.  In de **geavanceerde instellingen** in het dialoogvenster de **inschakelen 32-bits toepassingen** in de lijst met **waar**, en klik vervolgens op **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Tot stand brengen van de service principal name voor de website van BHOLD

Als de naam van het netwerk dat wordt gebruikt voor het Neem contact op met de BHOLD-website niet hetzelfde als de naam van de server-host is, moet u een service principal name (SPN) voor HTTP-maken. Als u een CNAME-bronrecord in DNS gebruiken om een alias voor de server, of als u een network load balancing gebruikt, moet u deze extra netwerkadressen registreren in Active Directory. Als u niet om dit te doen, Internet Explorer het Kerberos-protocol niet gebruiken tijdens de verbinding met de BHOLD-website.

> [!IMPORTANT]
> Als de BHOLD-Core-module is geïnstalleerd op dezelfde computer als de FIM-Portal, moet u DNS-bronrecords (CNAME- of A) maken met verschillende hostnamen voor de BHOLD-Core-servers en de server met de FIM-Portal. Alleen een SPN voor een bepaalde service-type/server-alias-paar kan worden gemaakt, en dus BHOLD-Core en de FIM-Portal vereist afzonderlijke SPN's omdat ze doorgaans worden uitgevoerd onder verschillende accounts. De setspn-opdracht meldt een fout als een SPN is tot stand is gebracht onder een ander account.

Lidmaatschap van **Domeinadministrators**, of gelijkwaardig, is de minimale vereiste om deze procedure te voltooien.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Tot stand brengen van de SPN-naam van de website van BHOLD

1.  Klik op de domeincontroller Active Directory Domain Services **Start**, klikt u op **alle programma's**, klikt u op **accessoires**, met de rechtermuisknop op **opdrachtprompt** , en klik vervolgens op **als administrator uitvoeren**.

2.  Typ de volgende opdracht achter de opdrachtprompt en druk op ENTER: setspn – S HTTP/ *\<networkalias\> \<domein\>*\\*\<accountname\>* waar:

    -   *\<networkalias\>* is het adres dat clients contact opnemen met de BHOLD-website gebruiken

    -   *\<domein\>*\\*\<accountname\>* is de naam van het domein en gebruikersnaam van het serviceaccount van BHOLD-Core die u hebt gemaakt tijdens de installatie van BHOLD-Core.

3.  Zie voor meer informatie over het gebruik van de module Access Management-Connector Test Lab Guide: BHOLD Access Management-Connector.

#### <a name="setting-bhold-system-attributes"></a>BHOLD-kenmerken instellen

Om te valideren dat de installatie van BHOLD-coremodule geslaagd is, die de kern van BHOLD-portal openen en weergeven van de systeemkenmerken. Om ervoor te zorgen dat de BHOLD-Core-module werkt goed in uw omgeving, kunt u bovendien de volgende BHOLD systeemkenmerken, waar nodig wijzigen:

| **Kenmerk**                | **Beschrijving**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Ingesteld op Y als de BHOLD-website wordt uitgevoerd op een geclusterde service om ervoor te zorgen dat onlangs weergegeven items goed werkt. Ingesteld op N als de BHOLD-website wordt uitgevoerd op een zelfstandige IIS-server.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Ingesteld op Y om ervoor te zorgen dat organisatie-eenheden (orgunits) alleen voor orgunits met dezelfde organisatie-type als de bovenliggende orgunit kunnen worden verplaatst. Bijvoorbeeld: dit voorkomt dat een project orgunit die naar een orgunit afdeling worden verplaatst. Ingesteld op N om toe te staan een orgunit worden geplaatst in een orgunit van een ander type. |
| **Dagen tussen ABA uitvoeren**     | Ingesteld op een geheel getal van twee cijfers om op te geven van het interval (in dagen) tussen twee autorisatie op basis van een kenmerk (ABA) wordt uitgevoerd. Bijvoorbeeld, als u wilt opgeven dat ABA uitvoeringen worden gescheiden door twee dagen, typt u 02.                                                                                                                     |
| **Begintijd van ABA uitvoeren**    | Ingesteld op een geheel getal van twee cijfers om op te geven van het uur van de dag waarop een kenmerk gebaseerde verificatie uitvoeren wordt uitgevoerd. Bijvoorbeeld, vindt als u wilt opgeven die de ABA uitvoeren plaats om 11:00 uur (23:00), typt u 23.                                                                                                             |
| **De kardinaliteit van systeem**       | Als u niet dat een kardinaliteit systeemcontrole in BHOLD wilt ingesteld op N. De standaardwaarde is Y.                                                                                                                                                                                                                             |
| **Logboekregistratie**                  | Ingesteld op N als u niet dat wijzigingen wilt moeten worden vastgelegd. De standaardwaarde is Y.                                                                                                                                                                                                                                            |
| **SystemQueue verwerking**   | Als u niet dat de verwerking van het systeem wachtrij wilt ingesteld op N. Wijzig deze waarde niet, tenzij omgeleid om dit te doen door ondersteuning voor het product.                                                                                                                                                                                           |

U moet zijn aangemeld als lid van de groep Domeinadministrators deze procedure uitvoeren.

**Om in te stellen van een systeemkenmerk BHOLD**

1.  Klik op **Start**, klikt u op **alle programma's**, en klik vervolgens op **Internet Explorer**.

2.  Typ in het adresvak, waarbij *\<server\>* is de naam van de BHOLD-websiteserver en *\<poort\>* is het poortnummer dat is gebonden aan de website.

3.  Klik op **Start**, klikt u op **waarden**, en klik vervolgens op **wijzigen**.

4.  Zoek de naam van het kenmerk dat u wilt wijzigen, typt u de nieuwe waarde in het vak naast de naam van het kenmerk en klik vervolgens op **OK**.

## <a name="next-steps"></a>Volgende stappen

Nadat u BHOLD Core hebt geïnstalleerd en wordt gevalideerd of de installatie voltooid is, kunt u aanvullende modules installeren. Op dit moment is de BHOLD-database in feite leeg is, met slechts één gebruikersaccount, het root-account en een organisatie-eenheid (orgunit), de orgunit hoofdmap. Als u wilt meer gebruikers toevoegen aan de BHOLD-database, kunt u ofwel de module Access Management-Connector of de Generator voor BHOLD-Model, afhankelijk van uw behoeften installeren. U kunt de module Access Management-Connector gebruiken om gebruikersgegevens te importeren vanuit de FIM-synchronisatieservice of kunt u de Model-Generator voor BHOLD om gebruikersgegevens te importeren uit een set van gestructureerde bestanden. Zie voor meer informatie over het gebruik van de module Access Management-Connector [Test Lab Guide: BHOLD-Access Management-Connector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

Zie voor meer informatie over het gebruik van de Generator voor BHOLD-Model-module:

- [Handleiding voor Microsoft BHOLD-Suite concepten](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD-Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
