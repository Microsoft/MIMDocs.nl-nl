---
title: Een bastionomgeving plannen | Microsoft Docs
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 16ad83ab9a0fbe2b93428cf318b5ef138e2f3783
ms.sourcegitcommit: 2be26acadf35194293cef4310950e121653d2714
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2017
---
# <a name="planning-a-bastion-environment"></a>Een bastionomgeving plannen

Door het toevoegen van een bastionomgeving met een specifiek beheerforest aan Active Directory kunnen organisaties gemakkelijk beheerdersaccounts, werkstations en groepen beheren in een omgeving die sterkere beveiligingsmechanismen heeft dan hun bestaande productieomgeving.

Deze architectuur maakt een aantal besturingselementen mogelijk die in een architectuur met één forest niet mogelijk zijn of niet eenvoudig kunnen worden geconfigureerd. Dit omvat het inrichten van accounts als standaardgebruiker zonder beheerdersmogelijkheden in het beheerforest, die in de productieomgeving over uitgebreide beheerdersmogelijkheden beschikken. Hierdoor is betere technische afdwinging van governance mogelijk. Deze architectuur maakt het ook mogelijk de functie selectieve verificatie van een vertrouwensrelatie te gebruiken om aanmeldingen (en referentieblootstelling) te beperken tot alleen geautoriseerde hosts. In situaties waarin een hoger niveau van controle is gewenst voor het productieforest zonder de extra kosten en complexiteit van een volledig nieuwe installatie, kan een beheerforest een omgeving bieden waarin de controle van de productieomgeving wordt verhoogd.

Naast het toegewezen beheerforest kunnen aanvullende technieken worden gebruikt. Hieronder vallen het beperken van blootstelling van beheerdersreferenties, hete beperken van rolbevoegdheden van gebruikers in die forest en het waarborgen dat administratieve taken niet worden uitgevoerd op hosts die worden gebruikt voor standaard gebruikersactiviteiten (bijvoorbeeld e-mailen en surfen op internet).

## <a name="best-practice-considerations"></a>Aandachtspunten voor best practices

Een toegewezen beheerforest is een standaard Active Directory-forest met één domein dat wordt gebruikt voor Active Directory-beheer. Beheerforests en -domeinen bieden het voordeel dat er vanwege hun beperkte gebruiksmogelijkheden meer beveiligingsmaatregelen mogelijk zijn dan op productieforests. Omdat het forest is gescheiden en bestaande forests van de organisatie niet vertrouwt, kan een beveiligingsinbreuk in een ander forest zich bovendien niet uitbreiden naar het toegewezen forest.

Bij het ontwerp van een beheerforest moet u rekening houden met het volgende:

### <a name="limited-scope"></a>Beperkt bereik

De waarde van een beheerforest ligt in de hoge mate van beveiligingscontrole en de verminderde kwetsbaarheid voor aanvallen. Het forest kan aanvullende beheerfuncties en -toepassingen bevatten. Elke aanvulling vergroot echter de kwetsbaarheid voor aanvallen van het forest en de daarbij behorende resources. Het doel is om de functies van het forest te beperken om de kwetsbaarheid te minimaliseren.

Volgens het [lagenmodel](tier-model-for-partitioning-administrative-privileges.md) van het partitioneren van beheerdersbevoegdheden, moeten de accounts in een specifieke beheerforest zich op één laag bevinden, doorgaans laag 0 of laag 1. Als een forest zich in laag 1 bevindt, beperk deze dan tot een specifiek toepassingsgebied (bijvoorbeeld apps voor de financiële afdeling) of gebruikersgemeenschap (bijvoorbeeld externe IT-leveranciers).

### <a name="restricted-trust"></a>Beperkt vertrouwen

Het productieforest *CORP* moet het beheerforest *PRIV* vertrouwen, maar niet andersom. Dit kan een domeinvertrouwen of een forestvertrouwensrelatie zijn. Het beheersforestdomein hoeft de beheerde domeinen en forests niet te vertrouwen om de Active Directory te beheren. Voor aanvullende toepassingen is mogelijk wel een wederzijdse vertrouwensrelatie, beveiligingsvalidatie en test vereist.

Selectieve verificatie moet worden gebruikt om ervoor te zorgen dat accounts in het beheerforest alleen de juiste productiehosts gebruiken. Voor het onderhoud van domeincontrollers en het delegeren van rechten in Active Directory, is doorgaans het toekennen van het recht 'Allowed to logon' (Aanmelden toegestaan) vereist voor domeincontrollers in aangewezen beheerdersaccounts in laag 0 van het beheerforest. Zie [Configuring Selective Authentication Settings](http://technet.microsoft.com/library/cc816580.aspx) (Selectieve verificatie-instellingen configureren) voor meer informatie.

## <a name="maintain-logical-separation"></a>Logische scheiding behouden

Als u wilt voorkomen dat de bastionomgeving wordt beïnvloed door bestaande of toekomstige beveiligingsincidenten in de Active Directory van de organisatie, moet u de volgende richtlijnen gebruiken wanneer u systemen voorbereid voor de bastionomgeving:

- Windows Servers mogen geen lid zijn van een domein of gebruikmaken van software of distributie van instellingen van de bestaande omgeving.

- De bastionomgeving moet beschikken over een eigen Active Directory Domain Services, die Kerberos en LDAP, DNS, tijd en tijdservices aan de bastionomgeving leveren.

- MIM mag geen SQL-databasefarm in de bestaande omgeving gebruiken. SQL Server moet worden geïmplementeerd op toegewezen servers in de bastionomgeving.

- Voor de bastionomgeving is Microsoft Identity Manager 2016 vereist. Met name de MIM-Service en de PAM-onderdelen moeten worden geïmplementeerd.

- Back-upsoftware en -media voor de bastionomgeving moeten gescheiden worden gehouden van de software en media van systemen in de bestaande forests, zodat een beheerder geen back-up van de bastionomgeving kan ondermijnen.

- Gebruikers die de bastionomgevingservers beheren, moeten zich aanmelden vanaf werkstations die niet toegankelijk zijn voor beheerders in de bestaande omgeving, zodat de referenties voor de bastionomgeving niet kunnen worden gelekt.

## <a name="ensure-availability-of-administration-services"></a>Beschikbaarheid van beheerservices waarborgen

Houd er tijdens de overgang van het beheer van toepassingen naar de bastionomgeving rekening mee dat u moet zorgen voor voldoende beschikbaarheid om te voldoen aan de vereisten van deze toepassingen. De technieken zijn onder andere:

- Implementeer Active Directory Domain Services op meerdere computers in de bastionomgeving. Er zijn er ten minste twee nodig om ervoor te zorgen dat verificatie altijd mogelijk is, zelfs als een server opnieuw wordt gestart voor gepland onderhoud. Mogelijk zijn extra computers nodig voor hogere belasting of voor het beheren van resources en beheerders die zich in meerdere geografische regio's bevinden.

- Bereid voor noodgevallen in het bestaande forest en het toegewezen beheerforest noodaccounts voor.

- Implementeer SQL Server en MIM-service op meerdere computers in de bastionomgeving.

- Behoud een back-up van AD en SQL voor elke wijziging aan gebruikers of roldefinities in het toegewezen beheerforest.

## <a name="configure-appropriate-active-directory-permissions"></a>Juiste machtigingen configureren voor Active Directory

Het beheerforest moet zijn geconfigureerd voor de minimale bevoegdheden op basis van de vereisten voor Active Directory-beheer.

- Aan accounts in het beheerforest die worden gebruikt voor het beheer van de productieomgeving, mogen geen beheerdersbevoegdheden worden verleend voor de beheerforest, of voor domeinen of werkstations in dit forest.

- Beheerdersbevoegdheden voor het beheerforest moeten strikt worden beheerd door een offlineproces om de mogelijkheid te reduceren van een aanval of het wissen van controlelogboeken door een kwaadwillende insider. Zo voorkomt u ook dat personeel met beheerdersaccounts op productieniveau de beperkingen voor hun account kunnen versoepelen en het risico voor de organisatie verhogen.

- Het beheerforest moet voldoen aan de Microsoft Security Compliance Manager-configuraties voor het domein, met inbegrip van sterke configuraties voor de verificatieprotocollen.

Bij het maken van de bastionomgeving, en voordat u Microsoft Identity Manager installeert, identificeert en maakt u de accounts die worden gebruikt voor beheer in deze omgeving. Dit omvat:

- **Noodaccounts** mogen alleen kunnen aanmelden op de domeincontrollers in de bastionomgeving.

- **'Rodekaartbeheerders'** richten andere accounts in en voeren niet-gepland onderhoud uit. Aan deze accounts wordt geen toegang verleend tot de bestaande forests of systemen buiten de bastionomgeving. De referenties, zoals een smartcard, moeten fysiek worden beveiligd en het gebruik van deze account moet worden vastgelegd.

- **Serviceaccounts** zijn nodig voor Microsoft Identity Manager, SQL Server en andere software.

## <a name="harden-the-hosts"></a>De hosts beveiligen

Op alle hosts, waaronder domeincontrollers, servers en werkstations die zijn gekoppeld aan het beheerforest, moeten de meest recente besturingssystemen en servicepacks zijn geïnstalleerd. De installaties op deze hosts moeten actueel blijven.

- De toepassingen die zijn vereist voor het uitvoeren van beheer moeten vooraf worden geïnstalleerd op werkstations, zodat de accounts waarmee ze worden zich niet in de lokale beheerdersgroep hoeven te bevinden om deze toepassingen te installeren. Onderhoud aan de Domain Controller kan doorgaans worden uitgevoerd met RDP en externe-serverbeheerprogramma's.

- Hosts voor beheerforest moeten automatisch worden bijgewerkt met beveiligingsupdates. Hierdoor ontstaat weliswaar het risico dat de domeincontroller wordt onderbroken door onderhoudsbewerkingen, maar het biedt een belangrijke beperking van beveiligingsrisico’s door niet-verholpen beveiligingsproblemen.

### <a name="identify-administrative-hosts"></a>Hosts met een beheerderrol identificeren

Het risico van een systeem of werkstation moet worden gemeten aan de activiteit met het hoogste risico dat hierop wordt uitgevoerd, zoals surfen op internet, het verzenden en ontvangen van e-mailberichten of het gebruik van andere toepassingen die onbekende of niet-vertrouwde inhoud verwerken.

Hosts met een beheerderrol de volgende computers:

- Een desktop waarop referenties van de beheerder fysiek zijn getypt of ingevoerd.

- Administratieve 'jumpservers' waarop administratieve sessies en hulpprogramma's worden uitgevoerd.

- Alle hosts waarop beheeracties worden uitgevoerd, waaronder die hosts die een standaard gebruikersdesktop met een RDP-client gebruiken voor het extern beheren van servers en toepassingen.

- Servers waarop toepassingen worden gehost die moeten worden beheerd en die niet toegankelijk zijn met RDP met beperkte beheermodus of Windows PowerShell voor externe toegang.

### <a name="deploy-dedicated-administrative-workstations"></a>Toegewezen beheerwerkstations implementeren

Afzonderlijk beveiligde werkstations die zijn toegewezen aan gebruikers met invloedrijke beheerdersreferenties zijn wellicht onhandig, maar zijn mogelijk vereist. Het is belangrijk om een host te voorzien van een beveiligingsniveau dat gelijk is aan of hoger is dan het niveau van de bevoegdheden die aan de referenties zijn toevertrouwd. Neem de volgende maatregelen voor aanvullende beveiliging:

- **Controleer of alle media in de build schoon is** om het risico te beperken dat schadelijke software is geïnstalleerd in een master-installatiekopie of is opgenomen in een installatiebestand tijdens downloaden of opslag.

- **Beveiligingsbasislijnen** moeten worden gebruikt als basisconfiguratie. Microsoft Security naleving Manager (SCM) kan helpen de basislijnen op hosts met een beheerderrol te configureren.

- **Beveiligd opstarten** om het risico van aanvallers of het laden van schadelijke software tijdens het opstartproces te beperken.

- **Softwarebeperking** om ervoor te zorgen dat alleen geautoriseerde beheersoftware wordt uitgevoerd op de hosts met een beheerderrol. Klanten kunnen voor deze taak AppLocker gebruiken met een whitelist geautoriseerde toepassingen, om te voorkomen dat schadelijke software en niet-ondersteunde toepassingen wordt uitgevoerd.

- **Volledige codering** om het risico van fysieke verlies van computers te beperken, zoals laptops met een beheerderrol die op afstand worden gebruikt.

- **USB-beperkingen** als bescherming tegen fysieke infectie.

- **Netwerkisolatie** als bescherming tegen netwerkaanvallen en onbedoeld beheerhandelingen. Hostfirewalls moeten alle binnenkomende verbindingen blokkeren, behalve de verbindingen die expliciet zijn vereist, en moeten alle overbodige uitgaande toegang tot internet blokkeren.

- **Antimalware** als bescherming tegen bekende dreigingen en schadelijke software.

- **Oplossingen gebruiken** om het risico van onbekende bedreigingen en aanvallen te beperken, met inbegrip van de Enhanced Mitigation Experience Toolkit (EMET).

- **Kwetsbaarheid analyseren** om de introductie van nieuwe aanvalsvectoren tegen Windows tijdens de installatie van nieuwe software te voorkomen. Hulpprogramma's zoals de Attack Surface Analyzer (ASA) helpen configuratie-instellingen op een host te beoordelen en identificeren aanvalsvectoren die worden geïntroduceerd door software- of configuratiewijzigingen.

- **Beheerdersbevoegdheden** mogen niet worden toegekend aan gebruikers op hun lokale computer.

- **RestrictedAdmin-modus** voor uitgaande RDP-sessies, behalve wanneer dit voor de rol is vereist. Zie [Wat is er nieuw in Extern bureaublad-services in Windows Server](https://technet.microsoft.com/library/dn283323.aspx) voor meer informatie.

Sommige van deze maatregelen lijken wellicht extreem, maar uit onthullingen in de afgelopen jaren blijken de belangrijke mogelijkheden waarover ervaren tegenstanders beschikken om inbreuk te maken op hun doelen.

## <a name="prepare-existing-domains-to-be-managed-by-the-bastion-environment"></a>Bestaande domeinen voorbereiden voor beheer door de bastionomgeving

MIM gebruikt PowerShell-cmdlets om vertrouwensrelatie tot stand te brengen tussen de bestaande AD-domeinen en het toegewezen beheerforest in de bastionomgeving. Nadat de bastionomgeving is geïmplementeerd, en voordat er gebruikers of groepen worden geconverteerd naar JIT, worden de domeinvertrouwensrelaties bijgewerkt met de cmdlets `New-PAMTrust` en `New-PAMDomainConfiguration`, en worden artefacten gemaakt die nodig zijn voor AD en MIM.

Als de bestaande Active Directory-topologie verandert, kunnen de cmdlets `Test-PAMTrust`, `Test-PAMDomainConfiguration`, `Remove-PAMTrust` en `Remove-PAMDomainConfiguration` worden gebruikt voor het bijwerken van de vertrouwensrelaties.

## <a name="establish-trust-for-each-forest"></a>Vertrouwensrelatie instellen voor elke forest

De cmdlet `New-PAMTrust` moet voor elk bestaand forest één keer worden uitgevoerd. Deze wordt aangeroepen op de computer van de MIM-service in het beheerdomein. De parameters voor deze opdracht zijn de domeinnaam van het bovenste domein van het bestaande forest en de referenties van een beheerder van dat domein.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

Nadat de vertrouwensrelatie is ingesteld, configureert u vervolgens elk domein voor beheer vanuit de bastionomgeving, zoals beschreven in het volgende gedeelte.

## <a name="enable-management-of-each-domain"></a>Beheer voor elk domein inschakelen

Er zijn zeven vereisten voor het inschakelen van beheer voor een bestaand domein.

### <a name="1-a-security-group-on-the-local-domain"></a>1. Een beveiligingsgroep op het lokale domein

In het bestaande domein moet een groep bestaan waarvan de naam de NetBIOS-domeinnaam is, gevolgd door drie dollartekens, bijvoorbeeld *CONTOSO$$$*. Het groepsbereik moet *domeingebonden* zijn en het groepstype *Beveiliging*. Dit is nodig om groepen te kunnen maken in het toegewezen beheerforest met dezelfde beveiligings-id als groepen in dit domein. Meld u op een werkstation dat lid is van het bestaande domein aan als beheerder van het bestaande domein en maak deze groep met de volgende PowerShell-opdracht:

```PowerShell
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

### <a name="2-success-and-failure-auditing"></a>2. Geslaagde en mislukte controle

De instellingen voor groepsbeleid op de domeincontroller voor controle moet geslaagde en mislukte controle voor toegang tot Accountbeheer controleren en Active directory-service controleren omvatten. U doet dit met de console Groepsbeleidbeheer op een werkstation dat lid is van het bestaande domein en aangemeld als beheerder van het bestaande domein:

3. Ga naar **Start** > **Systeembeheer** > **Groepsbeleidsbeheer**.

4. Navigeer naar **Forest: priv.contoso.local** > **Domeinen** > **priv.contoso.local** > **Domeincontrollers** > **Standaardbeleid voor domeincontrollers**. Een informatief bericht wordt weergegeven.

    ![Schermopname van het venster Standaardbeleid voor domeincontrollers](media/pam-group-policy-management.jpg)

5. Klik met de rechtermuisknop op **Standaardbeleid voor domeincontrollers** en selecteer **Bewerken**. Er wordt een nieuw venster weergegeven.

6. Navigeer in het venster Groepsbeleidsbeheer-editor, onder de structuur Standaardbeleid voor domeincontrollers naar **Computerconfiguratie** > **Beleidsregels** > **Windows-instellingen** > **Beveiligingsinstellingen** > **Lokale beleidsregels** > **Controlebeleid**.

    ![Schermopname van het venster Groepsbeleidsbeheer-editor](media/pam-group-policy-management-editor.jpg)

5. Klik in het detailvenster met de rechtermuisknop op **Accountbeheer controleren** en selecteer **Eigenschappen**. Selecteer **Deze beleidsinstellingen vastleggen**, schakel achtereenvolgens de selectievakjes **Geslaagd** en **Mislukt** in, klik op **Toepassen** en tenslotte op **OK**.

6. Klik in het detailvenster met de rechtermuisknop op **Directoryservicetoegang controleren** en selecteer **Eigenschappen**. Selecteer **Deze beleidsinstellingen vastleggen**, schakel achtereenvolgens de selectievakjes **Geslaagd** en **Mislukt** in, klik op **Toepassen** en tenslotte op **OK**.

    ![Schermopname van de beleidsinstellingen voor geslaagd en mislukt](media/pam-group-policy-management-editor2.jpg)

7. Sluit het venster Groepsbeleidsbeheer-editor en het venster Groepsbeleidsbeheer. Pas vervolgens de controle-instellingen toe door een PowerShell-venster te starten en het volgende te typen:

    ```cmd
    gpupdate /force /target:computer
    ```

Het bericht Het bijwerken van het computerbeleid is voltooid. wordt na enkele minuten weergegeven.

### <a name="3-allow-connections-to-the-local-security-authority"></a>3. Verbindingen met de lokale certificeringsinstantie toestaan

De domeincontrollers moeten RPC via TCP/IP-verbindingen toestaan voor de lokale certificeringsautoriteit (LSA) van de bastionomgeving. Bij oudere versies van Windows Server moet TCP/IP-ondersteuning in LSA zijn ingeschakeld in het register:

```PowerShell
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

### <a name="4-create-the-pam-domain-configuration"></a>4. De PAM-domeinconfiguratie maken

De cmdlet `New-PAMDomainConfiguration` wordt uitgevoerd op de computer van de MIM-service in het beheerdomein. De parameters voor deze opdracht zijn de domeinnaam van het bestaande domein en de referenties van een beheerder van dat domein.

```PowerShell
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

### <a name="5-give-read-permissions-to-accounts"></a>5. Leesmachtiging verlenen aan accounts

De accounts in het bastionforest die worden gebruikt voor het opzetten van rollen (beheerders die de cmdlets `New-PAMUser` en `New-PAMGroup` gebruiken) en het account dat wordt gebruikt door de MIM-controleservice, hebben leesmachtigingen nodig in dat domein.

Met de volgende stappen schakelt u leestoegang in voor de gebruiker *PRIV\Administrator* voor het domein *Contoso* binnen de domeincontroller *CORPDC*:

1. Zorg ervoor dat u als Contoso-domeinbeheerder bent aangemeld bij CORPDC (bijvoorbeeld Contoso\Administrator).

2. Start Active Directory: gebruikers en computers.

3. Klik met de rechtermuisknop op het domein **contoso.local** en selecteer **Beheer delegeren**.

4. Klik op het tabblad Geselecteerde gebruikers en groepen op **Toevoegen**.

5. Klik op de pop-up Gebruikers, computers of groepen selecteren op **Locaties** en wijzig de locatie in *priv.contoso.local*. Typ op de objectnaam *Domeinbeheerders* en klik op **Namen controleren**. Wanneer een pop-up wordt weergegeven, typt u voor de gebruikersnaam *priv\administrator* en typt u vervolgens het wachtwoord.

6. Typ na Domeinbeheerders *; MIMMonitor*. Nadat de namen Domeinbeheerders en MIMMonitor zijn onderstreept, klikt u op **OK** en vervolgens op **Volgende**.

7. Selecteer in de lijst met algemene taken **Alle gebruikersgegevens lezen** en klik vervolgens op **Volgende** en **Voltooien**.

18. Sluit Active Directory - gebruikers en computers.

### <a name="6-a-break-glass-account"></a>6. Een noodaccount

Als het Privileged Access Management-project als doel heeft het verminderen van het aantal accounts met domeinbeheerdersbevoegdheden die permanent zijn toegewezen aan het domein, moet het domein een *nood*account bevatten, voor het geval er later een probleem ontstaat met de vertrouwensrelatie. In elk domein moeten accounts voor noodtoegang tot het productieforest bestaan. Deze accounts moeten alleen kunnen aanmelden op domeincontrollers. Voor organisaties met meerdere sites zijn mogelijk extra accounts vereist voor redundantie.

### <a name="7-update-permissions-in-the-bastion-environment"></a>7. Machtigingen bijwerken in de bastionomgeving

Controleer de machtigingen op het object *AdminSDHolder* in de systeemcontainer in dat domein. Het object *AdminSDHolder* heeft een unieke toegangsbeheerlijst (ACL), die wordt gebruikt voor het beheren van machtigingen van beveiligings-principals die lid zijn van de ingebouwde Active Directory-groepen met bevoorrechte rol. Let op of er in de standaardmachtigingen wijzigingen zijn doorgevoerd die gevolgen hebben voor gebruikers met beheerdersbevoegdheden in het domein. Deze machtigingen zijn niet van toepassing op gebruikers waarvan het account zich in de bastionomgeving bevindt.

## <a name="select-users-and-groups-for-inclusion"></a>Gebruikers en groepen selecteren voor insluiting

De volgende stap is het definiëren van de PAM-rollen. Hierdoor koppelt u de gebruikers en groepen waartoe ze toegang moeten hebben. Dit is doorgaans een subset van de gebruikers en groepen voor de laag die waarvan in de bastionomgeving wordt aangegeven dat deze wordt beheerd. Zie [Rollen voor Privileged Access Management definiëren](defining-roles-for-pam.md) voor meer informatie.
