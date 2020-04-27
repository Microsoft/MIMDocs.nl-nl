---
title: BHOLD Core-installatie | Microsoft Docs
description: Kern document van de installatie van de BHOLD-Suite
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: c4dfb4184292ba1b5da8c4e3e176d53e6a885ed8
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042267"
---
# <a name="bhold-core-installation"></a>BHOLD Core-installatie

De BHOLD core-module biedt de belangrijkste functies van BHOLD suite binnen uw omgeving. De BHOLD-kern module moet worden geïnstalleerd en geconfigureerd op een server in uw lokale netwerk, voordat u andere BHOLD Suite-modules kunt installeren.

## <a name="bhold-core-installation-requirements"></a>BHOLD Core-installatie vereisten

De BHOLD-kern module vormt de basis van micro soft BHOLD Suite. U moet de BHOLD-kern module installeren voordat u andere micro soft BHOLD Suite-modules installeert.

### <a name="bhold-core-hardware-requirements"></a>BHOLD-kern hardwarevereisten

De BHOLD-kern module vormt de basis van micro soft BHOLD Suite. U moet de BHOLD-kern module installeren voordat u andere micro soft BHOLD Suite-modules installeert.

|          |        |          |
|----------|--------|----------|
|**Component** |**Minimum** | **Aanbevelingen** |
|Processor | 64-bits processor | Multicore 64-bits processor |
| Geheugen |3 GB | 6 GB of meer |
|Storage| 30 GB beschikbaar |Is afhankelijk van de implementatie grootte |
|Netwerk adapter| 100 Mbps-verbinding met SQL en Forefront Identity Manager (FIM)-server | 1 Gbps-verbinding met SQL-en FIM-server|

Deze aanbevelingen zijn gebaseerd op typische implementaties en nemen geen rekening met andere toepassingen die op de server worden uitgevoerd. Mogelijk moet u op basis van uw specifieke omgeving hogere onderdelen gebruiken.

### <a name="bhold-core-software-requirements"></a>BHOLD core-software vereisten

De BHOLD-kern module moet worden geïnstalleerd op een computer die voldoet aan de volgende vereisten:

- Op de server moet Windows Server 2012 (64-bits) worden uitgevoerd, Windows Server 2016 
- De server moet lid zijn van een Active Directory Domain Services domein (AD DS). In test omgevingen kan de server een AD DS domein controller zijn.
- BHOLD Core moet worden geïnstalleerd door een gebruiker die is aangemeld met een account in hetzelfde domein als de-server en die deel uitmaakt van de groep domein Administrators in het domein en de groep Administrators op de server.
- Micro soft Internet Information Services (IIS) met ASP.NET moet zijn geïnstalleerd op de server. IIS moet worden geconfigureerd met Windows-verificatie ingeschakeld. Als BHOLD core wordt geïnstalleerd op Windows Server 2012/2016, moet compatibiliteit met IIS 6-beheer zijn geïnstalleerd. Als BHOLD core wordt geïnstalleerd op Windows Server 2012, moeten IIS 6-script hulpprogramma's zijn geïnstalleerd
- Het .NET 3,5-Framework moet zijn geïnstalleerd.
  - Omdat voor BHOLD core .NET 3,5 is vereist, kan deze niet worden geïnstalleerd op Server Core.
- Silverlight 4 is vereist voor andere BHOLD-modules en daarom wordt aanbevolen dat u deze installeert voordat u BHOLD core installeert.
- Microsoft SQL Server 2014 of Microsoft SQL Server 2016 moet zijn geïnstalleerd op de BHOLD core-server of op een andere server in het LAN. 

Op Windows-client computers moet micro soft Internet Explorer versie 6 of hoger en micro soft Silverlight versie 4 of hoger worden uitgevoerd.

## <a name="before-you-begin"></a>Voordat u begint

De BHOLD-kern module vereist een gebruikers account dat wordt gebruikt voor het verifiëren en autoriseren van de BHOLD core-service voor de server en andere netwerk entiteiten. In deze sectie wordt uitgelegd hoe u dat gebruikers account maakt en configureert en een voor installatie voorstel biedt waarmee u de informatie kunt verzamelen die u nodig hebt om de BHOLD Core-installatie te volt ooien.

>{! BELANG rijk} wanneer u BHOLD core installeert, kunt u een bestaande SQL Server-Data Base gebruiken als de kern database van BHOLD. u kunt ook de wizard voor het instellen van de BHOLD-kern configureren om de BHOLD Core-Data Base voor u te maken. Als u ervoor kiest om een bestaande Data Base te gebruiken, moet u ervoor zorgen dat andere gebruikers geen toegang hebben tot de data base terwijl u BHOLD core installeert. Voordat u BHOLD core installeert, controleert u of de toegangs elementen van de SQL Server-Data Base geen toegang verlenen aan andere gebruikers dan de gebruiker die BHOLD core installeert.

## <a name="required-user-and-group"></a>Vereiste gebruiker en groep

De BHOLD-kern module moet kunnen worden aangemeld bij het domein met een gebruikers account dat is toegewezen aan het doel en die lid is van twee specifieke beveiligings groepen, inclusief een die specifiek is gemaakt voor de kern module BHOLD. Lidmaatschap van de groep domein Administrators is vereist om deze procedure uit te voeren.
**De kern gebruiker en beveiligings groep BHOLD maken en configureren**

1.  Op een domein controller, klikt u op **Start**, wijst u **systeem beheer**aan en klikt u vervolgens op **Active Directory gebruikers en computers**.

2.  Vouw in de console structuur het domein uit waar het account moet worden gemaakt, klik met de rechter muisknop op **gebruikers**, wijs **Nieuw**aan en klik vervolgens op **groep**.

3.  Typ in het dialoog venster **Nieuw object – groep** , in **groeps naam**, de naam van de groep (BHOLD standaard: BHOLDApplicationGroup) en klik vervolgens op **OK**.

4.  Klik met de rechter muisknop op **gebruikers**, wijs **Nieuw**aan en klik vervolgens op **gebruiker**.

5.  Typ bij **volledige naam**een naam die u helpt bij het identificeren van het account, bijvoorbeeld BHOLD Core-Service account.

6.  In **aanmeldings naam van gebruiker**typt u de gebruikers naam van het BHOLD Core-Service account (BHOLD default: b1user) en klikt u vervolgens op **Next**.

7.  Typ in **wacht woord** en **wacht woord bevestigen**het wacht woord voor het service account.

8.  Wissen **van gebruiker moet wacht woord bij volgende aanmelding wijzigen**. Selecteer **gebruiker kan wacht woord niet wijzigen** en **wacht woord nooit verloopt**, klik op **volgende**en vervolgens op **volt ooien**.

9.  Klik in het resultaten deel venster van de console met de rechter muisknop op het gebruikers account en klik vervolgens op **toevoegen aan een groep**.

10. Typ in het dialoog venster **groepen selecteren** de weergave naam van de groep die u eerder hebt gemaakt, typ een punt komma (;) en typ vervolgens IIS_IUSRS.

11. Klik op **Namen controleren**en klik vervolgens op **OK**.  

De volgende procedure moet worden uitgevoerd op de computer waarop de BHOLD-kern module moet worden geïnstalleerd. U moet zijn aangemeld als lid van de groep domein Administrators om deze procedure te kunnen uitvoeren.

## <a name="bhold-core-installation-worksheet"></a>Werk blad voor BHOLD Core-installatie

Voordat u begint met het installeren van de BHOLD-kern module, moet u voor bereid zijn om de informatie op te geven die de installatie wizard van de BHOLD-kern vereist heeft om de configuratie te volt ooien. Het volgende werk blad helpt u bij het vastleggen van die informatie, zodat u deze kunt opgeven wanneer dat nodig is. Elke sectie komt overeen met een pagina in de BHOLD Core-installatie wizard.

### <a name="account-settings"></a>Accountinstellingen

| **Item**                                    | **Beschrijving**                                                                                                                                                                                                                                                                                             | **Waarde**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beveiligings provider op domein/computer gebruiken** | Hiermee geeft u op dat Active Directory Domain Services beveiliging de toegang tot BHOLD core beheert.                                                                                                                                                                                                  | Schakel het selectievakje in. **Belang rijk:** Als dit selectie vakje niet is ingeschakeld, mislukt de installatie.                                                                 |
| **Domain**                                  | Hiermee geeft u het domein op dat de BHOLD-server, het service account en de toepassings groep bevat. **Belang rijk:** Geef de domein naam op met behulp van de NetBIOS (korte) naam, niet de Fully Qualified Domain Name (FQDN). Als de FQDN van het domein bijvoorbeeld fabrikam.com is, geeft u de domein naam op als CONTOSO. | Schrijf hier de domein naam:                                                                                                                                        |
| **Toepassings groep**                       | Hiermee geeft u de naam van de beveiligings groep die u eerder hebt gemaakt in de [vereiste gebruiker en groep](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Schrijf hier de groeps naam:                                                                                                                                         |
| **Service gebruiker**                            | Hiermee geeft u de aanmeldings naam op van het Service gebruikers account dat u eerder hebt gemaakt in de [vereiste gebruiker en groep](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Schrijf hier de naam van het gebruikers account:                                                                                                                                  |
| **Wachtwoord**                                | Hiermee geeft u het wacht woord van het gebruikers account van de BHOLD-basis service.                                                                                                                                                                                                                                              | Schrijf hier het wacht woord: **belang rijk:** zorg ervoor dat u dit wacht woord op een verborgen, veilige locatie blijft.                                                                |
| **IP/poort van website**                         | Hiermee geeft u het IP-adres en het poort nummer op van de website die op de intranet server moet worden gemaakt. Wijzig de standaard waarde (\*) alleen als u niet hetzelfde IP-adres als de standaard website wilt gebruiken. Wijzig het poort nummer alleen in een beschik bare poort als de standaard poort (5151) al in gebruik is.             | Als een niet-standaard-IP-adres wordt gebruikt door de standaard website, noteert u dit hier: als het standaard poort nummer al in gebruik is, schrijft u hier het BHOLD-website poort nummer: |

### <a name="database-settings"></a>Data base-instellingen

| **Item**                                       | **Beschrijving**                                                                                                                                                                                                                                                           | **Waarde**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Geïntegreerde beveiliging gebruiken**                    | Hiermee geeft u op dat Windows-verificatie wordt gebruikt voor toegang tot de data base.                                                                                                                                                                                                     | Schakel het selectie vakje in als Windows-verificatie wordt gebruikt om verbinding te maken met de SQL Server. Schakel het selectie vakje uit als SQL Server verificatie wordt gebruikt. De data base moet zijn gemaakt voordat u de BHOLD-installatie uitvoert als SQL Server verificatie wordt gebruikt. **Opmerking:** Als Windows-verificatie wordt gebruikt, moet u zijn aangemeld met een account dat beschikt over de serverrol sysadmin op de database server. |
| **Database gebruiker** en **database wachtwoord** | Hiermee geeft u de gebruikers naam en het wacht woord van een gebruiker met de serverrol sysadmin op de database server. Deze waarden worden alleen opgegeven als SQL Server verificatie wordt gebruikt.                                                                                               | Schrijf hier de SQL Server gebruikers naam: Schrijf hier het SQL Server gebruikers wachtwoord: **Opmerking:** zorg ervoor dat u dit wacht woord op een verborgen, veilige locatie bewaart.                                                                                                                                                                                                                                                  |
| **Database server** -en **database naam**   | Hiermee geeft u de NetBIOS-naam van de database server en de naam van de data base (standaard: B1) op die door de BHOLD Core-installatie wordt gemaakt. Als u geen gebruik maakt van het standaard exemplaar van de database server, geeft u het Database Server exemplaar op in het formulier * \<server\>*\\*\<exemplaar\>*. | Schrijf hier de naam van de server (of de server en het exemplaar): Schrijf hier de database naam:                                                                                                                                                                                                                                                                                                                   |
| **Beperkingen voor de database gebruiker maken**    | Verouderd.                                                                                                                                                                                                                                                                 | De standaard waarde niet wijzigen                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>BHOLD Core-installatie

Als u de BHOLD core-module wilt installeren, meldt u zich aan als lid van de groep domein Administrators, downloadt u het volgende bestand en voert u het uit als beheerder op de server waarop u de BHOLD-kern module wilt installeren: 

- Release. msi van BholdCore\_ * \<-versie\>*

Vervang * \<versie\> * door het versie nummer van de BHOLD core-release die u installeert.

Als u het programma bestand als beheerder wilt uitvoeren, klikt u met de rechter muisknop op het bestand en klikt u vervolgens op **als administrator uitvoeren**.

### <a name="postinstallation-settings"></a>Postinstallation-instellingen

Nadat de BHOLD-kern installatie is voltooid, moet u de Windows Firewall configureren en geavanceerde instellingen wijzigen in de groep BHOLD core-toepassing in Internet Information Services om de configuratie van de BHOLD-kern te volt ooien. Indien nodig moet u ook de BHOLD-systeem kenmerken wijzigen om aan uw vereisten te voldoen.

#### <a name="configuring-windows-firewall"></a>Windows Firewall configureren

Als gebruikers toegang krijgen tot BHOLD via webbrowsers op externe computers, moet u Windows Firewall op de BHOLD core-server configureren om binnenkomende verbindingen toe te staan met de website poort die u hebt opgegeven tijdens de installatie van BHOLD core.

Als u deze procedure wilt uitvoeren, moet u lid zijn van de groep Administrators op de lokale computer.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Binnenkomende verbindingen met de BHOLD-website toestaan

1.  Klik op **Start**, wijs **systeem beheer**aan, klik met de rechter muisknop op **Windows Firewall met geavanceerde instellingen**en klik vervolgens op **als administrator uitvoeren**.

2.  Klik in het linkerdeel venster op **regels voor binnenkomende verbindingen**en klik vervolgens in het rechterdeel venster op **nieuwe regel**.

3.  Klik in de wizard Nieuwe regel voor binnenkomende verbindingen op **poort**en klik vervolgens op **volgende**.

4.  Zorg ervoor dat **TCP** is geselecteerd in **specifieke lokale poorten**, typ het standaard BHOLD kern poort nummer (5151) of het poort nummer dat u hebt opgegeven tijdens de installatie van BHOLD core en klik vervolgens op **volgende**.

5.  Zorg ervoor dat **de verbinding toestaan** is geselecteerd en klik vervolgens op **volgende**.

6.  Schakel op de pagina **profiel** de selectie vakjes uit voor locaties waarvan u geen toegang wilt tot de BHOLD-website en klik vervolgens op **volgende**.

7.  Typ op de pagina **naam** een naam voor de regel (bijvoorbeeld binnenkomende verbindingen met BHOLD-kern toestaan) en klik vervolgens op **volt ooien**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>32-bits toepassingen inschakelen voor de BHOLD core-groep van toepassingen

Als u wilt dat IIS correct werkt met de kern module BHOLD, moet u de groep van de BHOLD-kern toepassing configureren ter ondersteuning van 32-bits toepassingen. U moet zijn aangemeld met het account dat wordt gebruikt voor het installeren van de BHOLD-kern module om deze procedure uit te voeren.

**Ondersteuning van 32 bits-toepassingen inschakelen voor de BHOLD core-toepassings groep**

1.  Om Internet Information Services Manager te openen, klikt u op **Start**, wijst u **Administrator-hulpprogram ma's**aan en klikt u vervolgens op **Internet Information Services (IIS) Manager**.

2.  Vouw in de console structuur de server naam uit en klik vervolgens op **groepen van toepassingen**.

3.  Klik in de lijst **groep van toepassingen** met de rechter muisknop op **CoreAppPool**en klik vervolgens op **Geavanceerde instellingen**.

4.  Selecteer in het dialoog venster **Geavanceerde instellingen** in de lijst **32-bits toepassingen inschakelen** de optie **waar**en klik vervolgens op **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Instellen van de Service Principal Name voor de BHOLD-website

Als de netwerk naam die wordt gebruikt om verbinding te maken met de BHOLD-website niet hetzelfde is als de hostnaam van de server, moet u een Service Principal Name (SPN-naam) voor HTTP instellen. Als u bijvoorbeeld een CNAME-bron record in DNS gebruikt om een alias voor de server op te geven, of als u netwerk taakverdeling gebruikt, moet u deze extra netwerk adressen registreren in Active Directory. Als u dit niet doet, kan Internet Explorer het Kerberos-protocol niet gebruiken bij het maken van contact met de BHOLD-website.

> [!IMPORTANT]
> Als de BHOLD-kern module is geïnstalleerd op dezelfde computer als de FIM-Portal, moet u DNS-bron records (CNAME of A) met verschillende hostnamen maken voor de servers met BHOLD core en de server waarop de FIM-Portal wordt uitgevoerd. Er kan slechts één SPN tot stand worden gebracht voor een bepaald service-type/server-alias paar, waardoor BHOLD core en de FIM-Portal afzonderlijke Spn's vereisen, omdat deze doorgaans onder verschillende accounts worden uitgevoerd. De opdracht setspn meldt een fout melding als er al een SPN is ingesteld onder een ander account.

Lidmaatschap van **Domeinadministrators**, of gelijkwaardig, is de minimale vereiste om deze procedure te voltooien.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>De SPN van de BHOLD-website maken

1.  Klik op de Active Directory Domain Services domein controller op **Start**, klik op **alle Program ma's**, klik op **accessoires**, klik met de rechter muisknop op **opdracht prompt**en klik vervolgens op **als administrator uitvoeren**.

2.  Typ bij de opdracht prompt de volgende opdracht en druk vervolgens op ENTER: Setspn-S http/ * \<networkalias\> \<\> Domain* \\ * \<\> accountnaam* waarbij:

    -   networkalias is het adres dat clients gebruiken om contact op te nemen met de BHOLD-website * \<\> *

    -   *\<het\>domein*\\*accountnaam\> is de domein-en gebruikers naam van het BHOLD Core-Service account dat u hebt gemaakt tijdens de installatie van BHOLD\<* core.

3.  Herhaal de vorige stap voor alle andere namen die clients gebruiken om contact op te nemen met de BHOLD-website, bijvoorbeeld CNAME-aliassen, namen met een Fully Qualified Domain Name of namen met een NetBIOS (Short)-domein naam.

#### <a name="setting-bhold-system-attributes"></a>BHOLD-systeem kenmerken instellen

Als u wilt controleren of de installatie van de BHOLD-kern module is voltooid, opent u de BHOLD-kern Portal en bekijkt u de systeem kenmerken. Daarnaast kunt u, om ervoor te zorgen dat de BHOLD-kern module goed werkt in uw omgeving, de volgende BHOLD-systeem kenmerken wijzigen, indien van toepassing:

| **Kenmerk**                | **Beschrijving**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Geen geschiedenis**                | Ingesteld op Y als de BHOLD-website wordt uitgevoerd op een geclusterde webservice om ervoor te zorgen dat recent weer gegeven items goed werken. Ingesteld op N als de BHOLD-website wordt uitgevoerd op een zelfstandige IIS-server.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Ingesteld op Y om ervoor te zorgen dat organisatie-eenheden (orgunits) alleen kunnen worden verplaatst naar orgunits met hetzelfde organisatie type als de bovenliggende orgunit. Zo voor komt u dat een project orgunit wordt verplaatst naar een afdeling orgunit. Ingesteld op N zodat een orgunit in een orgunit van een ander type kan worden geplaatst. |
| **Dagen tussen ABA-uitvoering**     | Stel in op een geheel getal van twee cijfers om het interval (in dagen) op te geven tussen twee op kenmerken gebaseerde autorisatie (ABA) wordt uitgevoerd. Als u bijvoorbeeld wilt opgeven dat ABA-uitvoeringen worden gescheiden door twee dagen, typt u 02.                                                                                                                     |
| **Begin uur van ABA-uitvoering**    | Stel in op een geheel getal van twee cijfers om het uur van de dag op te geven wanneer een op kenmerken gebaseerde autorisatie wordt uitgevoerd. Als u bijvoorbeeld wilt opgeven dat de ABA wordt uitgevoerd om 11:00 uur (23:00), Type 23.                                                                                                             |
| **Systeem kardinaliteit**       | Stel in op N als u geen systeem kardinaliteit wilt controleren in BHOLD. De standaard waarde is Y.                                                                                                                                                                                                                             |
| **Logboekregistratie**                  | Stel in op N als u niet wilt dat wijzigingen worden vastgelegd. De standaard waarde is Y.                                                                                                                                                                                                                                            |
| **SystemQueue-verwerking**   | Stel in op N als u niet wilt dat de systeem wachtrij wordt verwerkt. Wijzig deze waarde alleen als dit wordt gedaan door de product ondersteuning.                                                                                                                                                                                           |

U moet zijn aangemeld als lid van de groep domein Administrators om deze procedure te kunnen uitvoeren.

**Een BHOLD-systeem kenmerk instellen**

1.  Klik op **Start**, klik op **alle Program ma's**en klik vervolgens op **Internet Explorer**.

2.  In het vak adres typt u, waarbij * \<server\> * de naam is van de BHOLD-website server * \<en\> de poort* het poort nummer is dat is gebonden aan de website.

3.  Klik op **Start**, klik op **waarden**en klik vervolgens op **wijzigen**.

4.  Zoek de naam van het kenmerk dat u wilt wijzigen, typ de nieuwe waarde in het vak naast de naam van het kenmerk en klik vervolgens op **OK**.

## <a name="next-steps"></a>Volgende stappen

Nadat u BHOLD core hebt geïnstalleerd en gevalideerd dat de installatie is voltooid, kunt u aanvullende modules installeren. Op dit moment is de BHOLD-data base in feite leeg, met slechts één gebruikers account, het hoofd account en één organisatie-eenheid (orgunit), de hoofdmap orgunit. Als u meer gebruikers wilt toevoegen aan de BHOLD-data base, kunt u de module toegangs beheer connector of de module BHOLD model Generator installeren, afhankelijk van uw behoeften. U kunt de module toegangs beheer connector gebruiken voor het importeren van gebruikers gegevens uit de FIM-synchronisatie service, of u kunt de BHOLD-model generator gebruiken om gebruikers gegevens uit een set gestructureerde bestanden te importeren. Zie [test lab Guide: BHOLD Access Management connector (Engelstalig](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx)) voor meer informatie over het gebruik van de module toegangs beheer-connector.

Zie voor meer informatie over het gebruik van de module BHOLD model Generator:

- [Hand leiding micro soft BHOLD Suite-concepten](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Micro soft BHOLD Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
