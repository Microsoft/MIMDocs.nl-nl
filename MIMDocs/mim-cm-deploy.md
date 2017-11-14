---
title: Microsoft Identity Manager Certificate Manager implementeren | Microsoft Docs
description: Installeer Microsoft Identity Manager 2016 Certificate Manager
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/19/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 2473ef1c3d6fc5350d60d81bd508296a33343f01
ms.sourcegitcommit: 58d6c628d3bb770669348b987cf8f52ec0576132
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2017
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Microsoft Identity Manager Certificate Manager 2016 (MIM CM) implementeren

De installatie van Microsoft Identity Manager Certificate Manager 2016 (MIM CM) omvat een aantal stappen. Als een manier om het proces zijn we de dingen analyseren. Er zijn voorbereidende stappen die moeten worden genomen voordat de werkelijke MIM CM-stappen. De installatie is zonder de voorafgaande werkzaamheden waarschijnlijk zal mislukken. 

1. Overzicht van de implementatie
2. Stappen voorafgaand aan de implementatie
3. Wat kan er anders?

Het volgende diagram toont een voorbeeld van het type van de omgeving die kan worden gebruikt. De systemen met nummers zijn opgenomen in de lijst onder het diagram en zijn vereist voor het voltooien van de stappen die in dit artikel. Ten slotte worden Windows 2016 Datacenter-Servers gebruikt:

![Diagram van de omgeving](media/mim-cm-deploy/image001.png)

1. CORPDC – domeincontroller
2. CORPCM – MIM CM-Server
3. CORPCA: de certificeringsinstantie
4. CORPCMR – gebruikt voor later MIM CM Rest API-webservice – CM Portal-voor Rest-API:
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – Windows 10-domein zijn gekoppeld

## <a name="deployment-overview"></a>Overzicht van de implementatie

- Basisbesturingssysteem installatie
  - De testomgeving bestaat uit windows 2016 Datacenter-servers.
       >[!NOTE]
Voor meer informatie over de ondersteunde platforms voor MIM 2016 Kijk eens naar het artikel [ondersteunde platforms voor MIM 2016](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md)
- Stappen voorafgaand aan de implementatie
  - [Het schema uitbreiden](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)
  - Serviceaccounts maken
  - [Certificaatsjablonen maken](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)
  - IIS
  - Kerberos configureren
  - Stappen die betrekking hebben op database
    - SQL-configuratievereisten
    - Databasemachtigingen
- Implementatie

## <a name="pre-deployment-steps"></a>Stappen voorafgaand aan de implementatie

De MIM CM-configuratiewizard vereist informatie te verstrekken langs de manier om deze te voltooien. 
![](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Het schema uitbreiden

Het proces van uitbreiding van het schema is eenvoudig, maar moet worden gerealiseerd met waarschuwing vanwege de aard ervan niet ongedaan worden gemaakt.

>[!NOTE]
Deze stap is vereist dat het gebruikte account schema-Administrator-rechten heeft.

- Blader naar de locatie van de MIM-media en navigeer naar \\Certificaatbeheer\\x64 map.

- Kopieer de map Schema naar CORPDC en navigeer naar het.

    ![](media/mim-cm-deploy/image005.png)

- Voer het script resourceForestModifySchema.vbs één Forest scenario

- Voer de scripts voor het scenario met bron-forest:
  - DomeinA – gebruikers zich (userForestModifySchema.vbs)
  - ResourceForestB: locatie van CM-installatie (resourceForestModifySchema.vbs)

>[!NOTE]
Wijzigingen in het schema een eenrichtingsbewerking zijn en vereisen van een forest herstel terugdraaien dus zorg ervoor dat u de nodige back-ups hebben. Voor meer informatie over de wijzigingen aan het schema van deze bewerking Lees het artikel [Forefront Identity Manager 2010 Certificate Management-schemawijzigingen](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

![](media/mim-cm-deploy/image007.png)

Voer het script en het foutbericht is een geslaagde eenmaal bericht dat het script is voltooid.

![Bericht](media/mim-cm-deploy/image009.png)

Het schema in AD is nu uitgebreid ter ondersteuning van MIM CM.

### <a name="creating-service-accounts-and-groups"></a>Service-accounts en groepen maken

De volgende tabel ziet u de accounts en machtigingen die vereist zijn door MIM CM.
U kunt de MIM CM de volgende accounts automatisch maken of u kunt ze maken voorafgaand aan installatie toestaan. De namen van de daadwerkelijke account kunnen worden gewijzigd. Als u de accounts zelf maakt, kunt u de gebruikersaccounts in zo eenvoudig overeenkomen met de naam van de gebruikersaccount de functie naming.

Gebruikers:

![](media/mim-cm-deploy/image010.png)

![](media/mim-cm-deploy/image012.png)

| **Rol**                   | **Aanmeldingsnaam gebruiker** |
|----------------------------|---------------------|
| MIM CM-Agent               | MIMCMAgent          |
| MIM CM-sleutelherstelagent  | MIMCMKRAgent        |
| Autorisatie van MIM CM-Agent | MIMCMAuthAgent      |
| MIM CM CA-beheeragent    | MIMCMManagerAgent   |
| MIM CM Webpoolagent      | MIMCMWebAgent       |
| MIM CM-Inschrijvingsagent    | MIMCMEnrollAgent    |
| MIM CM-updateservice      | MIMCMService        |
| MIM installeren Account        | MIMINSTALL          |
| Help helpdesk Agent            | CMHelpdesk1 2       |
| CM-Manager                 | CMManager1 2        |
| Abonnee gebruiker            | CMUser1 2           |

Groepen:

| **Rol**               | **Groep**         |
|------------------------|-------------------|
| Leden van de Helpdesk CM    | MIMCM Helpdesk    |
| Leden van de CM-Manager     | MIMCM Managers    |
| Leden van CM | MIMCM-abonnees |

PowerShell: Agent-Accounts

```
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Update **CORPCM** Server lokaal beleid voor Agent-Accounts 

| **Aanmeldingsnaam gebruiker** | **Beschrijving en machtigingen**   |
|------|---------------------|
| MIMCMAgent          | Biedt de volgende services: </br>-Haalt versleutelde persoonlijke sleutels van de CA. </br>-Smartcard PINCODE-informatie in de FIM CM-database beveiligt. </br>-Communicatie tussen de CA en de FIM CM beveiligt. </br></br> Dit gebruikersaccount is vereist voor de volgende instellingen voor toegangsbeheer:</br>-   **Lokaal aanmelden toestaan** gebruikersrecht.</br>-   **Certificaten verlenen en beheren** gebruikersrecht. </br>-Lees- en schrijftoegang op de map Temp van de volgende locatie: % WINDIR %\\Temp.</br>-Er is een digitale handtekening en versleuteling certificaat uitgegeven en geïnstalleerd in de opslag van de gebruiker.
|MIMCMKRAgent        | Hersteld gearchiveerd persoonlijke sleutels van de CA. Dit gebruikersaccount is vereist voor de volgende instellingen voor toegangsbeheer:</br> -   **Lokaal aanmelden toestaan** gebruikersrecht.</br>-Lidmaatschap van de lokale **beheerders** groep. </br>-Machtiging Inschrijven op de **KeyRecoveryAgent** certificaatsjabloon. </br>-De sleutelherstelagent certificaat wordt uitgegeven en geïnstalleerd in de opslag van de gebruiker. Het certificaat moet worden toegevoegd aan de lijst met de sleutelherstel-agents op de CA. </br>-De machtiging lezen en schrijven toegang tot de map Temp van de volgende locatie:```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Bepaalt de rechten en machtigingen voor gebruikers en groepen. Dit gebruikersaccount is vereist voor de volgende instellingen voor toegangsbeheer: </br>-Lidmaatschap van de groep Pre-Windows 2000-compatibele toegang. </br> -Verleend de **Beveiligingscontrole genereren** gebruikersrecht.             |
| MIMCMManagerAgent   | CA-beheeractiviteiten uitvoert. </br> Deze gebruiker moet worden toegewezen als de machtiging CA beheren.        |
| MIMCMWebAgent       | Biedt de identiteit voor de IIS-toepassingsgroep. FIM CM binnen een Microsoft Win32® application programming interface proces dat wordt gebruikt de referenties van deze gebruiker wordt uitgevoerd. </br> Dit gebruikersaccount is vereist voor de volgende instellingen voor toegangsbeheer:</br> -Lidmaatschap van de lokale **IIS_WPG, windows 2016 IIS_IUSRS =** groep. </br>-Lidmaatschap van de lokale **beheerders** groep.</br>-Verleend de **Beveiligingscontrole genereren** gebruikersrecht. </br>-Verleend de **functioneren als deel van het besturingssysteem** gebruikersrecht. </br>-Verleend de **token op procesniveau vervangen** gebruikersrecht.</br>-Als de identiteit van de IIS-toepassingsgroep toegewezen **CLMAppPool**. </br>-Verleend leesrechten op de **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\CLM\\v1.0\\Server\\WebUser** registersleutel. </br>-Dit account moet ook worden vertrouwd voor overdracht.|
| MIMCMEnrollAgent    | Inschrijving namens een gebruiker uitvoert. Dit gebruikersaccount is vereist voor de volgende instellingen voor toegangsbeheer:</br>-Een inschrijvingsagentcertificaat dat wordt uitgegeven en geïnstalleerd in de opslag van de gebruiker.</br>-   **Lokaal aanmelden toestaan** gebruikersrecht. </br>-Machtiging Inschrijven op de **Inschrijvingsagent** certificaatsjabloon (of de aangepaste sjabloon, als dit wordt gebruikt).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Certificaatsjablonen voor MIM CM-serviceaccounts maken

Drie van de serviceaccounts die worden gebruikt door MIM CM vereisen een certificaat en de configuratiewizard vereist dat u opgeeft dat de naam van de certificaatsjablonen die deze certificaten aanvragen voor deze moet gebruiken.

De serviceaccounts die certificaten vereisen zijn:

- MIMCMAgent: Dit account moet een gebruikerscertificaat

- MIMCMEnrollAgent: Dit account moet een inschrijvingsagentcertificaat

- MIMCMKRAgent: Dit account moet een **sleutelherstelagent** certificaat

Er zijn al aanwezig in AD sjablonen, maar moeten we ons eigen versies werken met MIM CM maken. Als we nodig hebben om de wijziging van de oorspronkelijke basislijn-sjablonen te maken.

Alle drie de bovenstaande accounts wordt wel verhoogde bevoegdheden hebben binnen uw organisatie en moeten zorgvuldig worden behandeld.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Maken van de certificaatsjabloon MIM CM ondertekenen

1. Van **Systeembeheer**Open **certificeringsinstantie**.
2. In de **certificeringsinstantie** -console in de consolestructuur, vouw **Contoso CorpCA**, en klik vervolgens op **certificaatsjablonen**.
3. Met de rechtermuisknop op **certificaatsjablonen**, en klik vervolgens op **beheren**.
4. In de **Certificaatsjablonenconsole**, in de **details** deelvenster, selecteren en klik met de rechtermuisknop **gebruiker**, en klik vervolgens op **sjabloon dupliceren** .
5. In de **sjabloon dupliceren** dialoogvenster, **Windows Server 2003 Enterprise**, en klik vervolgens op **OK**.

![Resulterende wijzigingen weergeven](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    MIM CM does not work with certificates based on version 3 certificate templates. You must create a Windows Server® 2003 Enterprise (version 2)certificate template. See the following link for V3 details https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade

6. In de **eigenschappen van nieuwe sjabloon** in het dialoogvenster op de **algemene** tabblad, in de **weergavenaam van sjabloon** in het vak **MIM CM ondertekening**. Wijziging de **geldigheidsperiode** naar **2 jaar**, en schakel vervolgens de **certificaat in Active Directory publiceren** selectievakje.

7. Op de **afhandeling van aanvragen** tabblad, zorg ervoor dat de **persoonlijke sleutel exporteerbaar** selectievakje is ingeschakeld en klik vervolgens op **tabblad cryptografie**.

8. In de **cryptografie selectie** in het dialoogvenster uitschakelen **Microsoft Enhanced Cryptographic Provider v1.0**, inschakelen **Microsoft Enhanced RSA and AES Cryptographic Provider**, en klik vervolgens op **OK**.

Op de **onderwerpnaam** tabblad, schakel de **e-mailnaam opnemen in onderwerpnaam** en **e-mailnaam** selectievakjes uit.

Op de **extensies** tabblad, in de **extensies opgenomen in deze sjabloon** lijst **toepassingsbeleid** is geselecteerd en klik vervolgens op **bewerken **.

In de **Toepassingenbeleidsuitbreiding bewerken** dialoogvenster, selecteer de **Encrypting File System** en de **beveiligde E-mail** toepassingsbeleid. Klik op **verwijderen**, en klik vervolgens op **OK**.

Op de **beveiliging** tabblad de volgende stappen uitvoeren:

- Verwijder **beheerder**.

- Verwijder **Domeinadministrators**.

- Verwijder **domeingebruikers**.

- Wijs alleen **lezen** en **schrijven** machtigingen voor **Ondernemingsadministrators**.

- Voeg **MIMCMAgent.**

- Wijs **lezen** en **inschrijven** machtigingen voor **MIMCMAgent**.

In de **eigenschappen van nieuwe sjabloon** in het dialoogvenster, klikt u op **OK**.

Laat de **Certificaatsjablonenconsole** openen.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>De certificaatsjabloon van MIM CM-Inschrijvingsagent maken

-   In de **Certificaatsjablonenconsole**, in de **details** deelvenster, selecteren en klik met de rechtermuisknop **Inschrijvingsagent**, en klik vervolgens op **sjabloondupliceren**.

In de **sjabloon dupliceren** dialoogvenster, **Windows Server 2003 Enterprise**, en klik vervolgens op **OK**.

In de **eigenschappen van nieuwe sjabloon** in het dialoogvenster op de **algemene** tabblad, in de **weergavenaam van sjabloon** in het vak **MIM CM-Inschrijvingsagent**. Zorg ervoor dat de **geldigheidsperiode** is **2 jaar**.

Op de **afhandeling van aanvragen** tabblad, schakelt u **persoonlijke sleutel exporteerbaar**, en klik vervolgens op **CSP's of tabblad cryptografie.**

In de **CSP-selectie** in het dialoogvenster uitschakelen **Microsoft Base Cryptographic Provider v1.0**, uitschakelen **Microsoft Enhanced Cryptographic Provider v1.0**, inschakelen** Microsoft Enhanced RSA and AES Cryptographic Provider**, en klik vervolgens op **OK**.

Op de **beveiliging** tabblad Voer de volgende stappen:

- Verwijder **beheerder**.

- Verwijder **Domeinadministrators**.

- Wijs alleen **lezen** en **schrijven** machtigingen voor **Ondernemingsadministrators**.

- Voeg **MIMCMEnrollAgent**.

- Wijs **lezen** en **inschrijven** machtigingen voor **MIMCMEnrollAgent**.

In de **eigenschappen van nieuwe sjabloon** in het dialoogvenster, klikt u op **OK**.

Laat de **Certificaatsjablonenconsole** openen.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>De MIM CM-sleutelherstelagent certificaatsjabloon maken

1. In de **certificaatsjablonen** -console, in de **details** deelvenster, selecteren en klik met de rechtermuisknop **sleutelherstelagent**, en klik vervolgens op **sjabloondupliceren**.

2. In de **sjabloon dupliceren** dialoogvenster, **Windows Server 2003 Enterprise**, en klik vervolgens op **OK**.

3. In de **eigenschappen van nieuwe sjabloon** in het dialoogvenster op de **algemene** tabblad, in de **weergavenaam van sjabloon** in het vak **MIM CM sleutelherstelagent**. Zorg ervoor dat de **geldigheidsperiode** is **2 jaar** op de **tabblad cryptografie.**

4. In de **Providers selectie** in het dialoogvenster uitschakelen **Microsoft Enhanced Cryptographic Provider v1.0**, inschakelen **Microsoft Enhanced RSA and AES Cryptographic Provider**, en klik vervolgens op **OK**.

5. Op de **Uitgiftevereisten** tabblad, zorg ervoor dat **goedkeuring van de CA-certificaat** is **uitgeschakeld**.

6. Op de **beveiliging** tabblad Voer de volgende stappen:

    - Verwijder **beheerder**.

    - Verwijder **Domeinadministrators**.

    - Wijs alleen **lezen** en **schrijven** machtigingen voor **Ondernemingsadministrators**.

    - Voeg **MIMCMKRAgent**.

    - Wijs **lezen** en **inschrijven** machtigingen voor **KRAgent**.

7. In de **eigenschappen van nieuwe sjabloon** in het dialoogvenster, klikt u op **OK**.

8. Sluit de **Console certificaatsjablonen van het certificaat**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publiceren van de vereiste certificaatsjablonen op de certificeringsinstantie

1. Herstel de **certificeringsinstantie** console.

2. In de **certificeringsinstantie** -console in de consolestructuur met de rechtermuisknop op **certificaatsjablonen**, wijs **nieuw**, en klik vervolgens op **certificaat Sjabloon voor probleem**.
3. In de **Certificaatsjablonen inschakelen** dialoogvenster, **MIM CM-Inschrijvingsagent**, **MIM CM-sleutelherstelagent**, en **MIM CM ondertekening**. Klik op **OK**.
4. Klik in de consolestructuur op **certificaatsjablonen**.
5. Controleer of de drie nieuwe sjablonen worden weergegeven de **details** deelvenster en sluit **certificeringsinstantie**.
    ![MIM CM-ondertekening](media/mim-cm-deploy/image016.png)
6. Sluit alle vensters opent en meld u af.

### <a name="iis-configuration"></a>IIS-configuratie 

In volgorde hoIn volgorde voor het hosten van de website voor CM achterstallige en IIS configureren

#### <a name="install-and-configure-iis"></a>IIS installeren en configureren

1. Meld u aan bij ** CORLog in als **MIMINSTALL** account

>[!IMPORTANT]
De account van de installatie van de MIM moet een lokale beheerder

2. Open powershell en voer de volgende opdracht

   - ```Install-WindowsFeature –ConfigurationFilePath```

>[!NOTE]
 Een site met de naam Default Web Site wordt standaard geïnstalleerd in IIS 7. Als u die site is gewijzigd of verwijderd van een site met de naam van de moet standaardwebsite beschikbaar zijn voor de installatie van MIM CM.

#### <a name="configuring-kerberos"></a>Kerberos configureren

Het account MIMCMWebAgent zal worden uitgevoerd als de MIM CM-portal. Standaard in IIS en omhoog kernel mode-verificatie in IIS standaard gebruikt. U Kerberos-kernelmodusverificatie uitschakelen en SPN's in plaats daarvan op het account MIMCMWebAgent configureren. Sommige opdracht wordt verhoogde machtigingen in active directory en CORPCM server nodig.

![](media/mim-cm-deploy/image020.png)

```
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}

```

** IIS bijwerken op **CORPCM**


![](media/mim-cm-deploy/image022.png)

```
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true

```


>[!NOTE]
U moet een DNS-A-Record voor de 'cm.contoso.com' toevoegen aan en wijs CORPCM IP

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>SSL vereisen in de MIM CM-portal

Het is raadzaam dat u SSL nodig hebt voor de MIM CM-portal. Als u dit zelfs zal de wizard niet over deze waarschuwing.

1. Deelnemen aan het webcertificaat voor **cm.contoso.com** toewijzen aan de standaard-site

2. Open **IIS-beheer** en navigeer naar **Certificaatbeheer**

3. Dubbelklik op SSL-instellingen in de weergave functies.

4. Selecteer op de pagina SSL-instellingen **SSL vereisen**.

5. Klik in het deelvenster Acties op **toepassen.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Databaseconfiguratie **CORPSQL** voor MIM CM

1. Zorg ervoor dat u met de Server CORPSQL01 verbonden bent.

2. Zorg ervoor dat u bent aangemeld als SQL DBA

3. Voer het volgende T-SQL-script, zodat de CONTOSO\\MIMINSTALL-Account voor het maken van de database wanneer we gaat u naar de stap in de configuratie

>[!NOTE]
Moeten we keert u terug naar SQL wanneer we gereed voor de module beleid & afsluiten zijn

```
create login [CONTOSO\\MIMINSTALL] from windows;
exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
```

![MIM CM configuration wizard foutbericht](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Implementatie van Microsoft Identity Manager 2016 Certificate Management

1. Zorg ervoor dat u op de Server CORPCM en dat verbonden bent de **MIMINSTALL** account lid is van de **lokale beheerders** groep.

2. Zorg ervoor dat u bent aangemeld als Contoso\\MIMINSTALL.

3. De Microsoft Identity Manager SP1 ISO koppelen.

4. **Open** de **Certificaatbeheer\\x64** directory.

5. In de **x64** venster met de rechtermuisknop op **Setup**, en klik vervolgens op **als administrator uitvoeren**.

6. Klik op de pagina Welkom bij de installatiewizard voor Microsoft Identity Manager Certificate Management **volgende.**

7. Op de pagina Gebruiksrechtovereenkomst Lees de gebruiksrechtovereenkomst, schakel de ik ga akkoord met de voorwaarden in deze gebruiksrechtovereenkomst **selectievakje**, en klik op volgende.

8. Zorg ervoor dat op de pagina aangepaste installatie de **MIM CM-Portal** en **MIM CM-updateservice onderdelen** zijn ingesteld om te worden geïnstalleerd, en vervolgens **Klik op volgende**.

9. Zorg ervoor dat de naam van de virtuele map is op de pagina virtuele webmap ** CertificateManagement, en vervolgens **Klik op volgende**.

10. Op de pagina installeert Microsoft Identity Manager Certificate Management **Klik op installeren**.

11. Op de **voltooid** de installatiewizard voor Microsoft Identity Manager Certificate Management-pagina **klikt u op Voltooien**.

![MIM CM wizard is voltooid](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>De configuratiewizard van Microsoft Identity Manager 2016 Certificaatbeheer

Voeg MIMINSTALL aan voordat u zich aanmeldt voor CORPCM **domain Admins, Schema-Administrators en lokale beheerders** groep voor configuratiewizard. Dit kan later worden verwijderd zodra de configuratie is voltooid.      
    
![Foutbericht](media/mim-cm-deploy/image028.png)

1. Van de **Start** menu, klikt u op **Certificate Management-configuratiewizard**. En worden uitgevoerd als **beheerder**
2. Op de **Welkom bij de configuratiewizard** pagina, klikt u op **volgende**.
3. Op de **CA-configuratie** pagina, zorg ervoor dat de geselecteerde CA **Contoso-CORPCA-CA**, zorg ervoor dat de geselecteerde server **CORPCA. CONTOSO.COM**, en klik vervolgens op **volgende**.
4. Op de **instellen van de Microsoft® SQL Server®-Database** pagina de **naam van SQL Server** in het vak **CORPSQL1** , schakel de **mijn referenties gebruiken om te maken de database** selectievakje en klik vervolgens op **volgende**.
5. Op de **Database-instellingen** pagina, accepteer de standaardnaam van de database van **FIMCertificateManagement**, zorg ervoor dat **SQL geïntegreerde verificatie** is geselecteerd, en vervolgens Klik op **volgende**.

6. Op de **Active Directory instellen** pagina, accepteer de standaardnaam die is opgegeven voor het service connection point en klik vervolgens op **volgende**.

7. Op de **verificatiemethode** pagina bevestigen **windows geïntegreerde verificatie** is geselecteerd en klik vervolgens op **volgende**.

8. Op de **Agents: FIM CM** pagina, schakel de **de standaardinstellingen van FIM CM gebruiken** selectievakje en klik vervolgens op **aangepaste Accounts**.

9. In de **Agents: FIM CM** in het dialoogvenster met meerdere tabbladen op elk tabblad, typ de volgende informatie:
   - Gebruikersnaam: **Update** 
   - Wachtwoord: **doorgeven\@word1**
   - Bevestig het wachtwoord: **doorgeven\@word1**
   - Gebruik een bestaande gebruiker: **ingeschakeld**
>[!NOTE]
Deze accounts eerder is gemaakt. Zorg ervoor dat de procedures in stap 8 worden herhaald voor alle zes agent account tabbladen.

![MIM CM-accounts](media/mim-cm-deploy/image030.png)

10. Wanneer alle gegevens van agent voltooid is, klikt u op **OK**.

11. Op de **Agents – MIM CM** pagina, klikt u op **volgende**.

12. Op de **servercertificaten instellen** pagina, schakelt u de volgende certificaatsjablonen:
    - Certificaatsjabloon die moet worden gebruikt voor de sleutelherstelagent van de herstelagent: **MIMCMKeyRecoveryAgent**.
    - Certificaatsjabloon die moet worden gebruikt voor het certificaat van de FIM CM-Agent: **MIMCMSigning**.
    - Certificaatsjabloon die moet worden gebruikt voor het inschrijvingsagentcertificaat: **FIMCMEnrollmentAgent**.
13. Op de **Set-up servercertificaten** pagina, klikt u op **volgende**.
14. Op de **Setup e-mailserver, Document afdrukken** pagina in de **Geef de naam van de SMTP-server die u gebruiken wilt voor registratiemeldingen** vak en klik vervolgens op **volgende.**
15. Op de **klaar om te configureren** pagina, klikt u op **configureren**.
16. In de **configuratiewizard – Microsoft Forefront Identity Manager 2010 R2** waarschuwing in het dialoogvenster, klikt u op **OK** om te bevestigen dat SSL niet is ingeschakeld op de virtuele IIS-map.

    ![Media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    Klik niet op de knop voltooien voordat de uitvoering van de configuratiewizard voltooid is. Logboekregistratie voor wizard u hier vindt:**% programfiles %\\Microsoft Forefront Identity Management\\2010\\Certificaatbeheer\\config.log**
17. Klik op **Voltooien**.

![MIM CM wizard is voltooid](media/mim-cm-deploy/image033.png)

18. Sluit alle geopende vensters.

19. Https://cm.contoso.com/certificatemanagement toevoegen aan de zone Lokaal intranet in uw browser.

20. Gaat u naar de site van de server CORPCM https://cm.contoso.com/certificatemanagement  

    ![](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Controleer of de CNG-sleutel isolatie-Service

1. Van **Systeembeheer**Open **Services**.

2. In de **details** deelvenster, dubbelklikt u op **CNG-sleutel isolatie**.

3. Op de **algemene** en wijzig de **opstarttype** naar **automatische**.

4. Op de **algemene** tabblad, start de service als deze zich niet in een status gestart.

5. Op de **algemene** tabblad **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Installeren en configureren van de CA-Modules:

In deze stap wordt installeren en configureren van de FIM CM CA-modules op de certificeringsinstantie.

1. FIM CM alleen inspecteren gebruikersmachtigingen voor beheerbewerkingen configureren

2. In de **C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Certificaatbeheer\\web** venster een kopie van ** Web.config** naamgeving van de kopie **web.1.config**.

3. In de **Web** venster met de rechtermuisknop op **Web.config**, en klik vervolgens op **Open**.

    >[!Note]
    Het Web.config-bestand is geopend in Kladblok

4. Wanneer het bestand wordt geopend, drukt u op CTRL + F.

5. In de **zoeken en vervangen** het dialoogvenster de **zoeken naar** in het vak **UseUser**, en klik vervolgens op **volgende zoeken** drie keer.

6. Sluit de **zoeken en vervangen** in het dialoogvenster.

7. U moet zich op de regel ** \<key="Clm.RequestSecurity.Flags toevoegen ' waarde"UseUser UseGroups"= /\>**. Wijzig de regel lezen ** \<key="Clm.RequestSecurity.Flags toevoegen ' waarde 'UseUser' = /\>**.

8. Sluit het bestand, alle wijzigingen worden opgeslagen.

9. Maak een account voor de CA-computer op de SQL server \<geen script\>

10. Zorg ervoor dat u met verbonden bent de **CORPSQL01** server.

11. Zorg ervoor dat u bent aangemeld als **DBA**

12. Van de **Start** menu, start **SQL Server Management Studio**.

13. In de **verbinding maken met Server** het dialoogvenster de **servernaam** in het vak **CORPSQL01,** en klik vervolgens op **Connect**.

14. Vouw in de consolestructuur **beveiliging**, en klik vervolgens op **aanmeldingen**.

15. Met de rechtermuisknop op **aanmeldingen**, en klik vervolgens op **New Login**.

16. Op de **algemene** pagina in de **aanmeldingsnaam** in het vak **contoso\\CORPCA\$**. Selecteer **Windows-verificatie**. Standaard-database is **FIMCertificateManagement**.

17. Selecteer in het linkerdeelvenster **Gebruikerskoppeling**. Klik in het rechterdeelvenster op het selectievakje in de **kaart** kolom naast **FIMCertificateManagement**. In de **lidmaatschap databaserol voor: FIMCertificateManagement** lijst, schakelt u de **clmApp** rol.

18. Klik op **OK**.

19. Sluit **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>De FIM CM CA-modules installeren op de certificeringsinstantie

1. Zorg ervoor dat u met verbonden bent de **CORPCA** server.

2. In de **X64** windows, met de rechtermuisknop op **Setup.exe**, en klik vervolgens op **als administrator uitvoeren**.

3. Op de **Welkom bij de Wizard Setup van Microsoft Identity Manager Certificate Management** pagina, klikt u op **volgende**.

4. Op de **gebruiksrechtovereenkomst** pagina, lees de gebruiksrechtovereenkomst. Selecteer de **ik ga akkoord met de voorwaarden in deze gebruiksrechtovereenkomst** selectievakje en klik vervolgens op **volgende**.

5. Op de **aangepaste installatie** pagina **MIM CM-Portal**, en klik vervolgens op **dit onderdeel kan niet worden**.

6. Op de **aangepaste installatie** pagina **MIM CM-updateservice**, en klik vervolgens op **dit onderdeel kan niet worden**.

    >[!Note]
    Dit betekent dat de bestanden van MIM CM CA als de enige functie is ingeschakeld voor de installatie.

7. Op de **aangepaste installatie** pagina, klikt u op **volgende**.

8. Op de **installeert Microsoft Identity Manager Certificate Management** pagina, klikt u op **installeren**.

9. Op de **voltooid de Wizard Setup van Microsoft Identity Manager Certificate Management** pagina, klikt u op **voltooien**.

10. Sluit alle geopende vensters.

### <a name="configure-the-mim-cm-exit-module"></a>De MIM CM uitgiftemodule configureren

1. Van **Systeembeheer**Open **certificeringsinstantie**.

2. In de consolestructuur met de rechtermuisknop op **contoso-CORPCA-CA**, en klik vervolgens op **eigenschappen**.

3. Op de **uitgiftemodule** tabblad **uitgiftemodule van FIM CM**, en klik vervolgens op **eigenschappen**.

4. In de **Geef de verbindingsreeks van de CM-database** in het vak **Connect Timeout = 15; Beveiliginsinfo = True; Integrated Security = sspi; Initial Catalog = FIMCertificateManagement; Data Source = CORPSQL01**. Laat de **versleutelen van de verbindingsreeks** selectievakje is ingeschakeld en klik vervolgens op **OK**.
5. In de **Microsoft FIM Certificate Management** dialoogvenster, klikt u op **OK**.

6. In de **contoso-CORPCA-CA-eigenschappen** in het dialoogvenster, klikt u op **OK**.

7. Met de rechtermuisknop op **contoso-CORPCA-CA***,* wijs **alle taken**, en klik vervolgens op **Service stoppen**. Wacht totdat de Active Directory Certificate Services wordt gestopt.

8. Met de rechtermuisknop op **contoso-CORPCA-CA***,* wijs **alle taken**, en klik vervolgens op **Service starten**.

9. Minimaliseer de **certificeringsinstantie** console.

10. Van **Systeembeheer**Open **logboeken**.

11. Vouw in de consolestructuur **toepassings- en servicelogboeken**, en klik vervolgens op **FIM Certificate Management**.

12. Controleer of dat de meest recente gebeurtenissen doen in de lijst met gebeurtenissen *niet* bevatten die **waarschuwing** of **fout** gebeurtenissen sinds de laatste keer opnieuw opstarten van Certificate Services.

    >[!NOTE] 
    De laatste gebeurtenis moet status dat de uitgiftemodule geladen met de instellingen van```SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit```

13. Minimaliseer de **logboeken**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>De MIMCMAgent certificaatvingerafdruk naar Windows® Klembord kopiëren

1. Herstel de **certificeringsinstantie** console.

2. Vouw in de consolestructuur **contoso-CORPCA-CA**, en klik vervolgens op **verleende certificaten**.

3. In de **details** deelvenster, dubbelklikt u op het certificaat met **CONTOSO\\MIMCMAgent** in de **naam aanvrager** kolom en met **FIM CM Ondertekening** in de **certificaatsjabloon** kolom.

4. Op de **Details** tabblad de **vingerafdruk** veld.

5. Selecteer de vingerafdruk en druk op CTRL + C.

    >[!NOTE]
    Voer **niet** de witruimte opnemen in de lijst met vingerafdruk tekens.

6. In de **certificaat** in het dialoogvenster, klikt u op **OK**.

7. Van de **Start** menu, in de **programma's en bestanden zoeken** in het vak **Kladblok**, en druk op ENTER.

8. In **Kladblok**, uit de **bewerken** menu, klikt u op **plakken**.

9. Van de **bewerken** menu, klikt u op **vervangen**.

10. In de **zoeken naar** vak, typt u een spatie en klik vervolgens op **Alles vervangen**.

    >[!Note]
    Hiermee verwijdert u alle van de spaties tussen de tekens in de vingerafdruk.
    
11. In de **vervangen** in het dialoogvenster, klikt u op **annuleren**.

12. Selecteer de geconverteerde *thumbprintstring*, en druk op CTRL + C.

13. Sluit **Kladblok** zonder wijzigingen worden opgeslagen.

### <a name="configure-the-fim-cm-policy-module"></a>De FIM CM-beleidsmodule configureren

1. Herstel de **certificeringsinstantie** console.

2. Met de rechtermuisknop op **contoso-CORPCA-CA**, en klik vervolgens op **eigenschappen**.

3.  In de **contoso-CORPCA-CA-eigenschappen** in het dialoogvenster op de **beleidsmodule** tabblad **eigenschappen**.

- Op de **algemene** tabblad, zorg ervoor dat **FIM CM-aanvragen doorgeven aan de standaardbeleidsmodule voor verwerking** is geselecteerd.
- Op de **handtekeningcertificaten** tabblad **toevoegen**.
- Klik in het dialoogvenster certificaat met de rechtermuisknop op de **Geef hexadecimaal gecodeerde certificaat-hash** vak en klik vervolgens op **plakken**.
- In de **certificaat** in het dialoogvenster, klikt u op **OK**.
    >[!Note]
    Als de **OK** knop niet is ingeschakeld, kunt u een verborgen teken in de tekenreeks vingerafdruk per ongeluk opgenomen wanneer u de vingerafdruk van het certificaat clmAgent gekopieerd. Herhaal de bovenstaande stappen vanaf **taak 4: de MIMCMAgent certificaatvingerafdruk naar Windows Klembord te kopiëren** in deze oefening.

- In de **configuratie-eigenschappen** dialoogvenster Zorg dat de vingerafdruk wordt weergegeven in de **geldige handtekeningcertificaten** lijst en klik vervolgens op **OK**.

- In de **FIM Certificate Management** dialoogvenster, klikt u op **OK**.

- In de **contoso-CORPCA-CA-eigenschappen** in het dialoogvenster, klikt u op **OK**.

- Met de rechtermuisknop op **contoso-CORPCA-CA***,* wijs **alle taken**, en klik vervolgens op **Service stoppen**.

- Wacht totdat de Active Directory Certificate Services wordt gestopt.

- Met de rechtermuisknop op **contoso-CORPCA-CA***,* wijs **alle taken**, en klik vervolgens op **Service starten**.

- Sluit de **certificeringsinstantie** console.

- Sluit alle geopende vensters en meld u af.

- **Laatste stap in de implementatie** is we willen zeker CONTOSO\\MIMCM Managers kunnen implementeren en sjablonen maken en configureren van het systeem zonder schema en Domeinadministrators. Het volgende script zal ACL de machtigingen voor de certificaatsjablonen dsacls gebruiken. Voer het met een account met volledige machtigingen voor het wijzigen van beveiliging lees- en schrijfmachtigingen heeft voor elke bestaande certificaatsjabloon in het forest.

- Eerste stappen: **configureren van het Service Connection Point en machtigingen van de doelgroep & sjabloon Profielbeheer delegeren**
  - Configureer machtigingen voor het service connection point (SCP).

  - Gedelegeerd beheer van sjabloon configureren.

  - Configureer machtigingen voor het service connection point (SCP). **\<geen script\>**

        -   Zorg ervoor dat u met verbonden bent de **CORPDC** virtuele server.

        -   Meld u aan als **contoso\\corpadmin**

        -   Van **Systeembeheer**Open **Active Directory: gebruikers en Computers**.

        -   In **Active Directory: gebruikers en Computers**op de **weergave** menu ervoor te zorgen dat **geavanceerde functies** is ingeschakeld.

        -   Vouw in de consolestructuur **Contoso.com** \| **System** \| **Microsoft** \| **de levensduur van certificaat Manager**, en klik vervolgens op **CORPCM**.

        -   Met de rechtermuisknop op **CORPCM**, en klik vervolgens op **eigenschappen**.

        -   In de **CORPCM eigenschappen** in het dialoogvenster op de **beveiliging** tabblad, voeg de volgende groepen met de bijbehorende machtigingen toe:

    | Groep          | Machtigingen                                                                                                                                                         |
    |----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | mimcm Managers | Raadplegen </br> FIM CM-Audit</br> FIM CM-Inschrijvingsagent</br> FIM CM-aanvraag registreren</br> FIM CM-aanvraag herstellen</br> FIM CM-aanvraag vernieuwen</br> FIM CM-aanvraag in te trekken </br> FIM CM-aanvraag deblokkeren via een smartcard |
    | MIMCM Helpdesk | Raadplegen</br> FIM CM-Inschrijvingsagent</br> FIM CM-aanvraag in te trekken</br> FIM CM-aanvraag deblokkeren via een smartcard                                                                                |
- In de **CORPDC eigenschappen** in het dialoogvenster, klikt u op **OK**.

- Laat **Active Directory: gebruikers en Computers** openen.

- **Machtigingen configureren op de onderliggende gebruikersobjecten**
    - Zorg ervoor dat u nog steeds in de **Active Directory: gebruikers en Computers** console.
    - In de consolestructuur met de rechtermuisknop op **Contoso.com**, en klik vervolgens op **eigenschappen**.
    - Op de **beveiliging** tabblad **Geavanceerd**.
    - In de **geavanceerde beveiligingsinstellingen voor Contoso** in het dialoogvenster, klikt u op **toevoegen**.
    - In de **Selecteer gebruiker, Computer, serviceaccount of groep** het dialoogvenster de **Voer de naam van het object te selecteren** in het vak **mimcm Managers**, en klik vervolgens op **OK**.
    - In de **Machtigingsvermelding voor Contoso** het dialoogvenster de **toepassen op** selecteert **Descendant gebruikersobjecten** en schakel vervolgens de **toestaan**selectievakje in voor de volgende **machtigingen**:
        - **Alle eigenschappen lezen**
        - **Machtigingen voor lezen**
        - **FIM CM-Audit**
        - **FIM CM-Inschrijvingsagent**
        - **FIM CM-aanvraag registreren**
        - **FIM CM-aanvraag herstellen**
        - **FIM CM-aanvraag vernieuwen**
        - **FIM CM-aanvraag in te trekken**
        - **FIM CM-aanvraag deblokkeren via een smartcard**
    - In de **Machtigingsvermelding voor Contoso** in het dialoogvenster, klikt u op **OK**.
    - In de **geavanceerde beveiligingsinstellingen voor Contoso** in het dialoogvenster, klikt u op **toevoegen**.
    - In de **Selecteer gebruiker, Computer, serviceaccount of groep** het dialoogvenster de **Voer de naam van het object te selecteren** in het vak **mimcm HelpDesk**, en klik vervolgens op **OK**.
    - In de **Machtigingsvermelding voor Contoso** het dialoogvenster de **toepassen op** selecteert **Descendant gebruikersobjecten** en selecteer vervolgens de **toestaan**selectievakje in voor de volgende **machtigingen**: - **alle eigenschappen lezen** - **leesmachtigingen** - **FIM CM-Inschrijvingsagent** - **FIM CM-aanvraag in te trekken** - **FIM CM-aanvraag via een smartcard deblokkeren**
    - In de **Machtigingsvermelding voor Contoso** in het dialoogvenster, klikt u op **OK**.
    -Selecteer in de **geavanceerde beveiligingsinstellingen voor Contoso** in het dialoogvenster, klikt u op **OK**.
    - In de **contoso.com eigenschappen** in het dialoogvenster, klikt u op **OK**.
    - Laat **Active Directory: gebruikers en Computers** openen.

    - **Machtigingen configureren op de onderliggende gebruikersobjecten \<geen script\>**
        - Zorg ervoor dat u nog steeds in de **Active Directory: gebruikers en Computers** console.
        - In de consolestructuur met de rechtermuisknop op **Contoso.com**, en klik vervolgens op **eigenschappen**.
        - Op de **beveiliging** tabblad **Geavanceerd**.
        - In de **geavanceerde beveiligingsinstellingen voor Contoso** in het dialoogvenster, klikt u op **toevoegen**.
        - In de **Selecteer gebruiker, Computer, serviceaccount of groep** het dialoogvenster de **Voer de naam van het object te selecteren** in het vak **mimcm Managers**, en klik vervolgens op **OK**.
        - In de **Machtigingsvermelding voor CONTOSO** het dialoogvenster de **toepassen op** selecteert **Descendant gebruikersobjecten** en schakel vervolgens de **toestaan**selectievakje in voor de volgende **machtigingen**:
            - **Alle eigenschappen lezen**
            - **Machtigingen voor lezen**
            - **FIM CM-Audit**
            - **FIM CM-Inschrijvingsagent**
            - **FIM CM-aanvraag registreren**
            - **FIM CM-aanvraag herstellen**
            - **FIM CM-aanvraag vernieuwen**
            - **FIM CM-aanvraag in te trekken**
            - **FIM CM-aanvraag deblokkeren via een smartcard**
    - In de **Machtigingsvermelding voor CONTOSO** in het dialoogvenster, klikt u op **OK**.
    - In de **geavanceerde beveiligingsinstellingen voor CONTOSO** in het dialoogvenster, klikt u op **toevoegen**.
    - In de **Selecteer gebruiker, Computer, serviceaccount of groep** het dialoogvenster de **Voer de naam van het object te selecteren** in het vak **mimcm HelpDesk**, en klik vervolgens op **OK**.
    - In de **Machtigingsvermelding voor CONTOSO** het dialoogvenster de **toepassen op** selecteert **Descendant gebruikersobjecten** en selecteer vervolgens de **toestaan**selectievakje in voor de volgende **machtigingen**: - **alle eigenschappen lezen** - **leesmachtigingen** - **FIM CM-Inschrijvingsagent** - **FIM CM-aanvraag in te trekken** - **FIM CM-aanvraag via een smartcard deblokkeren**
    - In de **Machtigingsvermelding voor contoso** in het dialoogvenster, klikt u op **OK**.
    - In de **geavanceerde beveiligingsinstellingen voor Contoso** in het dialoogvenster, klikt u op **OK**.
    - In de **contoso.com eigenschappen** in het dialoogvenster, klikt u op **OK**.
    - Laat **Active Directory: gebruikers en Computers** openen.
- Tweede stappen: **overdragen certificaat sjabloon beheermachtigingen \<script\>**
    - Delegeren van machtigingen voor de container Certificaatsjablonen.
    - Delegeren van machtigingen voor de OID-container.
    - Delegeren van machtigingen voor de bestaande certificaatsjablonen.
- Machtigingen voor de container Certificaatsjablonen definiëren
     1. Herstel de **Active Directory: Sites en Services** console.
     2. Vouw in de consolestructuur **Services**, vouw **Public Key Services**, en klik vervolgens op **certificaatsjablonen**.
     3. In de consolestructuur met de rechtermuisknop op **certificaatsjablonen**, en klik vervolgens op **beheer delegeren**.
     4. In de **overdracht van beheer** Wizard, klikt u op **volgende**.
     5. Op de **gebruikers of groepen** pagina, klikt u op **toevoegen**.
     6. In de **gebruikers, Computers, of groepen selecteren** het dialoogvenster de **Geef de objectnamen op** in het vak **mimcm Managers**, en klik vervolgens op **OK**.
     7. Op de **gebruikers of groepen** pagina, klikt u op **volgende**.
     8. Op de **taken over te dragen** pagina, klikt u op **een aangepaste taak maken en overdragen**, en klik vervolgens op **volgende**.
     9.  Op de **Active Directory-objecttype** pagina, zorg ervoor dat **deze map, bestaande objecten in deze map en het maken van nieuwe objecten in deze map** is geselecteerd en klik vervolgens op **volgende**.
     10. Op de **machtigingen** pagina in de **machtigingen** lijst, selecteert de **volledig beheer** selectievakje en klik vervolgens op **volgende**.
     11. Op de **voltooien van de Wizard overdracht van beheer** pagina, klikt u op **voltooien**.

- Machtigingen voor de container OID definiëren
     1. In de consolestructuur met de rechtermuisknop op **OID**, en klik vervolgens op **eigenschappen**.
     2. In de **OID eigenschappen** in het dialoogvenster op de **beveiliging** tabblad **Geavanceerd**.
     3. In de **geavanceerde beveiligingsinstellingen voor OID** in het dialoogvenster, klikt u op **toevoegen**.
     4. In de **Selecteer gebruiker, Computer, serviceaccount of groep** het dialoogvenster de **Voer de naam van het object te selecteren** in het vak **mimcm Managers**, en klik vervolgens op **OK**.
     5. In de **Machtigingsvermelding voor OID** dialoogvenster Zorg dat de machtigingen van toepassing op **dit object en alle onderliggende objecten**, klikt u op **volledig beheer**, en klik vervolgens op ** OK**.
     6. In de **geavanceerde beveiligingsinstellingen voor OID** in het dialoogvenster, klikt u op **OK**.
     7. In de **OID eigenschappen** in het dialoogvenster, klikt u op **OK**.
     8. Sluit **Active Directory-Sites en Services**.

**Scripts: Machtigingen voor de container OID, Profielsjabloon & certificaatsjablonen**

![](media/mim-cm-deploy/image021.png)

'''import-module Active Directory $adace = @{'OID' = ' AD:\\CN OID, CN = Public Key Services, CN = Services, CN = = Configuration, DC = contoso, DC = com "; 'CT' = ' AD:\\CN certificaatsjablonen, CN = = Public Key Services, CN = Services, CN = Configuration, DC = contoso, DC = com "; 'PT' = ' AD:\\CN Profielsjablonen, CN = = Public Key Services, CN = Services, CN = Configuration, DC = contoso, DC = com "} $adace. GetEnumerator() | **Foreach-Object** {$acl = **Get-Acl** *-pad* $_. Waarde $sid = (**Get-ADGroup** 'MIMCM-Managers'). SID $p = **Nieuw Object** System.Security.Principal.SecurityIdentifier($sid)
##<a name="httpsmsdnmicrosoftcomen-uslibrarysystemdirectoryservicesactivedirectorysecurityinheritancevvs110aspx"></a>https://msdn.Microsoft.com/en-us/library/System.directoryservices.activedirectorysecurityinheritance (v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule ($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow, [ DirectoryServices.ActiveDirectorySecurityInheritance]::All) $acl. AddAccessRule($ace) **Set Acl** *-pad* $_. Waarde *- AclObject* $acl}
```

**Scripts: Delegating permissions on the existing certificate templates.**

![](media/mim-cm-deploy/image039.png)

dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
