---
title: Microsoft Identity Manager Certificate Manager implementeren | Microsoft Docs
description: Microsoft Identity Manager 2016 Certificate Manager installeren
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 7ab76d386d8633de8919167c6b8f26b5137323e5
ms.sourcegitcommit: 9e420840815adb133ac014a8694de9af4d307815
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/03/2018
ms.locfileid: "52825838"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Microsoft Identity Manager Certificate Manager 2016 (MIM CM) implementeren

De installatie van Microsoft Identity Manager Certificate Manager 2016 (MIM CM) omvat een aantal stappen. Als een manier om het proces te vereenvoudigen zijn we afbreken dingen. Er zijn voorbereidende stappen die moeten worden genomen voordat u de werkelijke MIM CM-stappen. De installatie is zonder de voorafgaande werk waarschijnlijk zal mislukken.

Het volgende diagram toont een voorbeeld van het type van de omgeving die kan worden gebruikt. De systemen met nummers zijn opgenomen in de lijst onder het diagram en zijn vereist voor het voltooien van de stappen in dit artikel besproken. Ten slotte worden Windows 2016 Datacenter-Servers gebruikt:

![Diagram van de omgeving](media/mim-cm-deploy/image001.png)

1. CORPDC-domeincontroller
2. CORPCM – MIM CM-Server
3. CORPCA: de certificeringsinstantie
4. CORPCMR – MIM CM-Rest API-Web-CM Portal-voor Rest API: wordt gebruikt voor later
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1: Windows 10 domein

## <a name="deployment-overview"></a>Implementatie-overzicht

- Basisbesturingssysteem installatie

    In het lab bestaat uit windows 2016 Datacenter-servers.

    >[!NOTE]
    >Voor meer informatie over de ondersteunde platforms voor MIM 2016 Kijk eens het artikel [ondersteunde platforms voor MIM 2016](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md)

1. Stappen voorafgaand aan de implementatie

    - [Het schema uitbreiden](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Het maken van service-accounts

    - [Het maken van certificaatsjablonen](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Kerberos configureren

    - Stappen die betrekking hebben op database

        - SQL-configuratievereisten

        - Machtigingen voor database

2. Implementatie

## <a name="pre-deployment-steps"></a>Stappen voorafgaand aan de implementatie

De MIM CM-configuratiewizard vereist dat gegevens worden opgegeven weg om deze te voltooien.

![Diagram](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Het schema uitbreiden

Het proces van uitbreiding van het schema is vrij eenvoudig, maar moet worden gerealiseerd met waarschuwing vanwege de aard niet ongedaan worden gemaakt.

>[!NOTE]
>Deze stap vereist dat het account dat wordt gebruikt schema beheerdersrechten heeft.

1. Blader naar de locatie van de MIM-media en navigeer naar \\Certificaatbeheer\\x64 map.

2. Kopieer de map Schema naar CORPDC en navigeer naar het.

    ![Diagram](media/mim-cm-deploy/image005.png)

3. Het script resourceForestModifySchema.vbs één Forest scenario uitvoeren. Voer de scripts voor de bron-forest-scenario:
   - DomeinA – gebruikers zich (userForestModifySchema.vbs)
   - ResourceForestB: locatie van CM-installatie (resourceForestModifySchema.vbs).

     >[!NOTE]
     >Wijzigingen in het schema zijn een manier is een bewerking en vereist een forest recovery terugdraaien dus zorg ervoor dat u de nodige back-ups hebt. Voor meer informatie over de wijzigingen aan het schema van het uitvoeren van deze bewerking raadpleegt u het artikel [schemawijzigingen voor Forefront Identity Manager 2010 Certificate Management](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

     ![Diagram](media/mim-cm-deploy/image007.png)

4. Voer het script en u ontvangt een succes bericht zodra het script is voltooid.

    ![Het bericht geslaagd](media/mim-cm-deploy/image009.png)

Het schema in AD is nu uitgebreid ter ondersteuning van MIM CM.

### <a name="creating-service-accounts-and-groups"></a>Het maken van service-accounts en groepen

De volgende tabel geeft een overzicht van de accounts en machtigingen die vereist zijn door MIM CM. U kunt de MIM CM maken de volgende accounts automatisch of u kunt ze maken voordat u kunt installeren. De accountnamen van de werkelijke kunnen worden gewijzigd. Als u de accounts zelf maakt, kunt u overwegen de gebruikersaccounts in deze onmiddellijk dat het is eenvoudig zodat deze overeenkomen met de naam van het gebruikersaccount van de functie naamgeving.

Gebruikers:

![Diagram](media/mim-cm-deploy/image010.png)

![Diagram](media/mim-cm-deploy/image012.png)

| **Rol**                   | **Gebruikerslogboek op de naam van** |
|----------------------------|---------------------|
| MIM CM-Agent               | MIMCMAgent          |
| MIM CM-Key Recovery Agent  | MIMCMKRAgent        |
| Autorisatie van MIM CM-Agent | MIMCMAuthAgent      |
| MIM CM CA-beheeragent    | MIMCMManagerAgent   |
| MIM CM Web Pool Agent      | MIMCMWebAgent       |
| MIM CM Enrollment Agent    | MIMCMEnrollAgent    |
| MIM CM-updateservice      | MIMCMService        |
| MIM installeren-Account        | MIMINSTALL          |
| Help helpdesk-Agent            | CMHelpdesk1-2       |
| CM-Manager                 | CMManager1-2        |
| Abonnee gebruiker            | CMUser1-2           |

Groepen:

| **Rol**               | **Groep**         |
|------------------------|-------------------|
| CM Helpdesk leden    | mimcm-HelpDesk    |
| Leden van de CM-Manager     | MIMCM-Managers    |
| CM-abonnees leden | MIMCM-abonnees |

PowerShell: Agent-Accounts:

```powershell
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

| **Gebruikerslogboek op de naam van** | **Beschrijving en machtigingen**   |
|------|---------------------|
| MIMCMAgent          | Biedt de volgende services: </br>-Haalt versleutelde persoonlijke sleutels van de CA. </br>-Smartcard PINCODE-informatie in de FIM CM-database beveiligt. </br>-Beveiliging voor communicatie tussen FIM CM en de CA. </br></br> Dit gebruikersaccount is vereist voor de volgende instellingen voor toegangsbeheer:</br>-   **Lokaal aanmelden toestaan** gebruikersrecht.</br>-   **Certificaten verlenen en beheren** gebruikersrecht. </br>-Lees- en schrijftoegang op de map Temp op de volgende locatie: % WINDIR %\\Temp.</br>-Er is een digitale handtekening en versleuteling certificaat uitgegeven en in het archief van de gebruiker geïnstalleerd.
|MIMCMKRAgent        | Wordt hersteld gearchiveerde persoonlijke sleutels van de CA. Dit gebruikersaccount is vereist voor de volgende instellingen voor toegangsbeheer:</br> -   **Lokaal aanmelden toestaan** gebruikersrecht.</br>-Lidmaatschap van de lokale **beheerders** groep. </br>-Machtiging worden ingeschreven op de **KeyRecoveryAgent** certificaatsjabloon. </br>-Key Recovery Agent certificaat is uitgegeven en in het archief van de gebruiker geïnstalleerd. Het certificaat moet worden toegevoegd aan de lijst met de sleutelherstel-agents op de CA. </br>-De machtiging lezen en schrijven machtigingen op de map Temp op de volgende locatie: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Hiermee bepaalt u rechten en machtigingen voor gebruikers en groepen. Dit gebruikersaccount is vereist voor de volgende instellingen voor toegangsbeheer: </br>-Lidmaatschap van de groep Pre-Windows 2000-compatibele toegang. </br> -Verleend de **Beveiligingscontrole genereren** gebruikersrecht.             |
| MIMCMManagerAgent   | CA-beheeractiviteiten uitvoert. </br> Deze gebruiker moet worden toegewezen als de machtiging van de CA beheren.        |
| MIMCMWebAgent       | Biedt de identiteit voor de IIS-toepassingsgroep. FIM CM wordt uitgevoerd binnen een Microsoft Win32® application programming interface proces die gebruikmaakt van de referenties van deze gebruiker. </br> Dit gebruikersaccount is vereist voor de volgende instellingen voor toegangsbeheer:</br> -Lidmaatschap van de lokale **IIS_WPG, windows 2016 IIS_IUSRS =** groep. </br>-Lidmaatschap van de lokale **beheerders** groep.</br>-Verleend de **Beveiligingscontrole genereren** gebruikersrecht. </br>-Verleend de **functioneren als deel van het besturingssysteem** gebruikersrecht. </br>-Verleend de **token op procesniveau vervangen** gebruikersrecht.</br>-Toegewezen als de identiteit van de IIS-toepassingsgroep **CLMAppPool**. </br>-Verleend leesrechten op de **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\CLM\\v1.0\\Server\\WebUser** registersleutel. </br>-Dit account moet ook worden vertrouwd voor overdracht.|
| MIMCMEnrollAgent    | Inschrijving namens een gebruiker uitvoert. Dit gebruikersaccount is vereist voor de volgende instellingen voor toegangsbeheer:</br>-Een Enrollment Agent-certificaat dat is uitgegeven en in het archief van de gebruiker geïnstalleerd.</br>-   **Lokaal aanmelden toestaan** gebruikersrecht. </br>-Machtiging worden ingeschreven op de **Enrollment Agent** certificaatsjabloon (of de aangepaste sjabloon, als dit wordt gebruikt).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Certificaatsjablonen voor MIM CM-serviceaccounts maken

Drie van de serviceaccounts die door MIM CM gebruikt een certificaat vereist en de configuratiewizard vereist dat u de naam van de certificaatsjablonen die moet worden gebruikt voor het aanvragen van certificaten voor hen opgeeft.

De serviceaccounts die certificaten vereisen zijn:

- MIMCMAgent: Dit account moet een gebruikerscertificaat

- MIMCMEnrollAgent: Dit account moet een inschrijvingsagentcertificaat

- MIMCMKRAgent: Dit account moet een **sleutelherstelagent** certificaat

Er zijn al aanwezig in AD sjablonen, maar moeten we onze eigen versies om te werken met MIM CM maken. We nodig hebben om de wijziging van de oorspronkelijke basislijn-sjablonen maken.

Alle drie de bovenstaande accounts wordt verhoogde bevoegdheden binnen uw organisatie hebben en moet zorgvuldig worden verwerkt.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>De sjabloon voor MIM CM-ondertekening certificaat maken

1. Van **Systeembeheer**Open **certificeringsinstantie (CA)**.

2. In de **certificeringsinstantie (CA)** console in de structuur van de console, vouw **Contoso-CorpCA**, en klik vervolgens op **certificaatsjablonen**.

3. Met de rechtermuisknop op **certificaatsjablonen**, en klik vervolgens op **beheren**.

4. In de **Certificaatsjablonenconsole**, in de **details** deelvenster, selecteer en klik met de rechtermuisknop **gebruiker**, en klik vervolgens op **sjabloon dupliceren** .

5. In de **sjabloon dupliceren** in het dialoogvenster, selecteer **Windows Server 2003 Enterprise**, en klik vervolgens op **OK**.

    ![De resulterende wijzigingen weergeven](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >MIM CM werkt niet met certificaten op basis van certificaatsjablonen van versie 3. U moet een certificaatsjabloon voor Windows Server® 2003, Enterprise (versie 2) maken. Zie [V3 details](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) voor meer informatie.

6. In de **eigenschappen van nieuwe sjabloon** dialoogvenster op de **algemene** tabblad, in de **weergavenaam van sjabloon** in het vak **MIM CM-ondertekening**. Wijziging de **geldigheidsperiode** naar **2 jaar**, en schakel vervolgens de **certificaat in Active Directory publiceren** selectievakje.

7. Op de **afhandeling van aanvragen** tabblad, zorg ervoor dat de **persoonlijke sleutel exporteerbaar** selectievakje is ingeschakeld en klik vervolgens op **tabblad cryptografie**.

8. In de **cryptografie selectie** in het dialoogvenster uitschakelen **Microsoft Enhanced Cryptographic Provider v1.0**, inschakelen **Microsoft Enhanced RSA and AES Cryptographic Provider**, en klik vervolgens op **OK**.

9. Op de **onderwerpnaam** tabblad, schakel de **e-mailnaam in onderwerpnaam opnemen** en **e-mailnaam** selectievakjes.

10. Op de **extensies** tabblad, in de **extensies opgenomen in deze sjabloon** lijst, zorg ervoor dat **toepassingsbeleid** is geselecteerd en klik vervolgens op **bewerken** .

11. In de **Toepassingenbeleidsuitbreiding bewerken** in het dialoogvenster, selecteer de **Encrypting File System** en de **beveiligde E-mail** toepassingsbeleid. Klik op **verwijderen**, en klik vervolgens op **OK**.

12. Op de **Security** tabblad de volgende stappen uitvoeren:

    - Verwijder **beheerder**.

    - Verwijder **Domeinadministrators**.

    - Verwijder **domeingebruikers**.

    - Alleen toewijzen **lezen** en **schrijven** machtigingen **Ondernemingsadministrators**.

    - Voeg **MIMCMAgent.**

    - Toewijzen **lezen** en **inschrijven** machtigingen **MIMCMAgent**.

13. In de **eigenschappen van nieuwe sjabloon** in het dialoogvenster, klikt u op **OK**.

14. Laat de **Certificaatsjablonenconsole** openen.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>De sjabloon van de MIM CM Enrollment Agent-certificaat maken

1. In de **Certificaatsjablonenconsole**, in de **details** deelvenster, selecteer en klik met de rechtermuisknop **Enrollment Agent**, en klik vervolgens op **sjabloondupliceren**.

2. In de **sjabloon dupliceren** in het dialoogvenster, selecteer **Windows Server 2003 Enterprise**, en klik vervolgens op **OK**.

3. In de **eigenschappen van nieuwe sjabloon** dialoogvenster op de **algemene** tabblad, in de **weergavenaam van sjabloon** in het vak **MIM CM Enrollment Agent**. Zorg ervoor dat de **geldigheidsperiode** is **2 jaar**.

4. Op de **afhandeling van aanvragen** tabblad, schakelt u **persoonlijke sleutel exporteerbaar**, en klik vervolgens op **CSP's of tabblad cryptografie.**

5. In de **CSP-selectie** in het dialoogvenster uitschakelen **Microsoft Base Cryptographic Provider v1.0**, uitschakelen **Microsoft Enhanced Cryptographic Provider v1.0**, inschakelen **Microsoft Enhanced RSA and AES Cryptographic Provider**, en klik vervolgens op **OK**.

6. Op de **Security** tabblad Voer de volgende stappen uit:

    - Verwijder **beheerder**.

    - Verwijder **Domeinadministrators**.

    - Alleen toewijzen **lezen** en **schrijven** machtigingen **Ondernemingsadministrators**.

    - Voeg **MIMCMEnrollAgent**.

    - Toewijzen **lezen** en **inschrijven** machtigingen **MIMCMEnrollAgent**.

7. In de **eigenschappen van nieuwe sjabloon** in het dialoogvenster, klikt u op **OK**.

8. Laat de **Certificaatsjablonenconsole** openen.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>De sjabloon van de MIM CM Key Recovery Agent-certificaat maken

1. In de **certificaatsjablonen** -console, in de **details** deelvenster, selecteer en klik met de rechtermuisknop **Key Recovery Agent**, en klik vervolgens op **sjabloondupliceren**.

2. In de **sjabloon dupliceren** in het dialoogvenster, selecteer **Windows Server 2003 Enterprise**, en klik vervolgens op **OK**.

3. In de **eigenschappen van nieuwe sjabloon** dialoogvenster op de **algemene** tabblad, in de **weergavenaam van sjabloon** in het vak **MIM CM Key Recovery Agent**. Zorg ervoor dat de **geldigheidsperiode** is **2 jaar** op de **tabblad cryptografie.**

4. In de **Providers selectie** in het dialoogvenster uitschakelen **Microsoft Enhanced Cryptographic Provider v1.0**, inschakelen **Microsoft Enhanced RSA and AES Cryptographic Provider**, en klik vervolgens op **OK**.

5. Op de **Uitgiftevereisten** tabblad, zorg ervoor dat **goedkeuring van de CA-certificaat** is **uitgeschakeld**.

6. Op de **Security** tabblad Voer de volgende stappen uit:

    - Verwijder **beheerder**.

    - Verwijder **Domeinadministrators**.

    - Alleen toewijzen **lezen** en **schrijven** machtigingen **Ondernemingsadministrators**.

    - Voeg **MIMCMKRAgent**.

    - Toewijzen **lezen** en **inschrijven** machtigingen **KRAgent**.

7. In de **eigenschappen van nieuwe sjabloon** in het dialoogvenster, klikt u op **OK**.

8. Sluit de **console Certificaatsjablonen**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Het vereiste certificaatsjablonen op de certificeringsinstantie (CA) publiceren

1. Herstel de **certificeringsinstantie (CA)** console.

2. In de **certificeringsinstantie (CA)** -console, in de consolestructuur met de rechtermuisknop op **certificaatsjablonen**, wijst u **nieuw**, en klik vervolgens op **certificaat Sjabloon voor het probleem**.

3. In de **Certificaatsjablonen inschakelen** in het dialoogvenster, selecteer **MIM CM Enrollment Agent**, **MIM CM Key Recovery Agent**, en **MIM CM ondertekening**. Klik op **OK**.

4. Klik in de consolestructuur op **certificaatsjablonen**.

5. Controleer of dat de drie nieuwe sjablonen worden weergegeven de **details** deelvenster hebben en sluit **certificeringsinstantie (CA)**.

    ![MIM CM-ondertekening](media/mim-cm-deploy/image016.png)

6. Sluit alle vensters openen en meld u af.

### <a name="iis-configuration"></a>IIS-configuratie

Als u wilt hosten van de website voor CM, installeren en configureren van IIS.

#### <a name="install-and-configure-iis"></a>IIS installeren en configureren

1. Meld u aan bij **CORLog in** als **MIMINSTALL** account

    >[!IMPORTANT]
    >Het account van de installatie van MIM moet een lokale beheerder

2. Open PowerShell en voer de volgende opdracht uit

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Een site met de naam standaardwebsite is standaard geïnstalleerd in IIS 7. Als deze site is gewijzigd of verwijderd van een site met de naam moet Default Web Site beschikbaar zijn voor de installatie van MIM CM.

#### <a name="configuring-kerberos"></a>Kerberos configureren

Het account MIMCMWebAgent zal worden uitgevoerd als de MIM CM-portal. Standaard in IIS en van de kernel mode-verificatie standaard gebruikt in IIS. U Kerberos-kernelmodusverificatie uitschakelen en SPN's voor het account MIMCMWebAgent in plaats daarvan configureren. Sommige opdracht vereist verhoogde machtigingen in active directory en CORPCM-server.

![Diagram](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**IIS op CORPCM bijwerken**

![Diagram](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>U moet een DNS A-Record voor de 'cm.contoso.com' toevoegen aan en wijs CORPCM IP

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Dat SSL is vereist op de MIM CM-portal

Het is raadzaam dat u SSL nodig hebt voor de MIM CM-portal. Als u de wizard wordt dit nog niet over deze waarschuwing.

1. Schrijf u in de webcertificaat voor **cm.contoso.com** toewijst aan standaard-site

2. Open **IIS-beheer** en navigeer naar **Certificate Management**

3. Dubbelklik op SSL-instellingen in de weergave van de functies.

4. Selecteer op de pagina SSL-instellingen **SSL vereisen**.

5. Klik in het deelvenster Acties op **toepassen.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Configuratie van de database **CORPSQL** voor MIM CM

1. Zorg ervoor dat u met de CORPSQL01-Server verbonden bent.

2. Zorg ervoor dat u bent aangemeld als SQL DBA.

3. Voer het volgende T-SQL-script om toe te staan de CONTOSO\\MIMINSTALL-Account voor het maken van de database wanneer we de configuratiestap

    >[!NOTE]
    >We moeten keert u terug naar SQL wanneer we gereed voor de module Groepsbeleid & afsluiten zijn

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![MIM CM configuration wizard foutbericht](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Implementatie van Microsoft Identity Manager 2016-Certificate Management

1. Zorg ervoor dat u met de Server CORPCM en die verbonden bent de **MIMINSTALL** account is lid van de **lokale beheerders** groep.

2. Zorg ervoor dat u bent aangemeld als Contoso\\MIMINSTALL.

3. Koppel de ISO van Microsoft Identity Manager SP1.

4. **Open** de **Certificaatbeheer\\x64** directory.

5. In de **x64** venster met de rechtermuisknop op **Setup**, en klik vervolgens op **als administrator uitvoeren**.

6. Klik op de pagina Welkom bij de installatiewizard voor Microsoft Identity Manager Certificate Management, **volgende.**

7. Op de pagina Gebruiksrechtovereenkomst Lees de gebruiksrechtovereenkomst, schakelt de ik ga akkoord met de voorwaarden van de gebruiksrechtovereenkomst **selectievakje**, en klik op volgende.

8. Zorg ervoor dat op de pagina aangepaste installatie de **MIM CM-Portal** en **MIM CM-updateservice onderdelen** zijn ingesteld om te worden geïnstalleerd, en vervolgens **Klik op volgende**.

9. Zorg ervoor dat de naam van de virtuele map is op de pagina virtuele webmap **CertificateManagement**, en vervolgens **Klik op volgende**.

10. Op de pagina installatie van Microsoft Identity Manager Certificate Management **Klik op installeren**.

11. Op de **voltooid** de installatiewizard voor Microsoft Identity Manager Certificate Management-pagina **Klik op Voltooien**.

![MIM CM-wizard is voltooid](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>De configuratiewizard van Microsoft Identity Manager 2016-Certificate Management

Toevoegen voordat u zich bij CORPCM MIMINSTALL naar **domain Admins, Schema-Administrators en de lokale groep administrators** groep voor de configuratiewizard. Dit kan later worden verwijderd zodra de configuratie voltooid is.

![Foutbericht](media/mim-cm-deploy/image028.png)

1. Uit de **Start** menu, klikt u op **Certificate Management-configuratiewizard**. En uit te voeren als **beheerder**

2. Op de **Welkom bij de configuratiewizard** pagina, klikt u op **volgende**.

3. Op de **CA-configuratie** pagina, zorg ervoor dat de geselecteerde CA wordt **Contoso-CORPCA-CA**, zorg ervoor dat de geselecteerde server **CORPCA. CONTOSO.COM**, en klik vervolgens op **volgende**.

4. Op de **instellen van de Microsoft® SQL Server®-Database** pagina, in de **naam van SQL Server** in het vak **CORPSQL1** , schakelen de **mijn referenties gebruiken om te maken de database** selectievakje en klik vervolgens op **volgende**.

5. Op de **Database-instellingen** pagina, accepteer de standaardnaam van de database van **FIMCertificateManagement**, zorg ervoor dat **geïntegreerde SQL-verificatie** is geselecteerd, en vervolgens Klik op **volgende**.

6. Op de **Active Directory instellen** pagina, accepteer de standaardnaam die is opgegeven voor de service connection point en klik vervolgens op **volgende**.

7. Op de **verificatiemethode** pagina bevestigen **windows geïntegreerde verificatie** is geselecteerd en klik vervolgens op **volgende**.

8. Op de **agenten-FIM CM** pagina, schakel de **gebruik de standaardinstellingen van FIM CM** selectievakje en klik vervolgens op **aangepaste Accounts**.

9. In de **agenten-FIM CM** in het dialoogvenster met meerdere tabbladen op elk tabblad, typt u de volgende informatie:

   - Gebruikersnaam: **Update**

   - Wachtwoord: **doorgeven\@word1**

   - Wachtwoord bevestigen: **doorgeven\@word1**

   - Gebruik een bestaande gebruiker: **ingeschakeld**

     >[!NOTE]
     >We deze accounts eerder hebben gemaakt. Zorg ervoor dat de procedures in stap 8 worden herhaald voor alle zes agent account tabbladen.

     ![MIM CM-accounts](media/mim-cm-deploy/image030.png)

10. Wanneer alle gegevens van agent voltooid is, klikt u op **OK**.

11. Op de **Agents – MIM CM** pagina, klikt u op **volgende**.

12. Op de **servercertificaten instellen** pagina, schakelt u de volgende certificaatsjablonen:

    - Certificaatsjabloon moet worden gebruikt voor de Key Recovery Agent van de herstelagent: **MIMCMKeyRecoveryAgent**.

    - Certificaatsjabloon die moet worden gebruikt voor het certificaat van de FIM CM-Agent: **MIMCMSigning**.

    - Certificaatsjabloon moet worden gebruikt voor het inschrijvingsagentcertificaat: **FIMCMEnrollmentAgent**.

13. Op de **servercertificaten instellen** pagina, klikt u op **volgende**.

14. Op de **Setup-e-mailserver, Document afdrukken** pagina, in de **Geef de naam van de SMTP-server die u wilt gebruiken voor het e-mailberichten registratie** vak en klik vervolgens op **volgende.**

15. Op de **klaar om te configureren** pagina, klikt u op **configureren**.

16. In de **configuratiewizard-Microsoft Forefront Identity Manager 2010 R2** waarschuwing in het dialoogvenster, klikt u op **OK** om te bevestigen dat SSL niet is ingeschakeld op de virtuele IIS-map.

    ![Media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Klik niet op de knop Voltooien totdat de uitvoering van de configuratiewizard voltooid is. Logboekregistratie voor wizard u hier vindt: **% programfiles %\\Microsoft Forefront Identity Management\\2010\\Certificaatbeheer\\config.log**

17. Klik op **Voltooien**.

    ![MIM CM-wizard is voltooid](media/mim-cm-deploy/image033.png)

18. Sluit alle geopende windows.

19. Voeg `https://cm.contoso.com/certificatemanagement` aan de zone Lokaal intranet in uw browser.

20. Ga naar de website van de server CORPCM `https://cm.contoso.com/certificatemanagement`  

    ![Diagram](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Controleer of de Service CNG-sleutels isolatie

1. Van **Systeembeheer**Open **Services**.

2. In de **details** deelvenster, dubbelklikt u op **CNG Key Isolation**.

3. Op de **algemene** en wijzig de **opstarttype** naar **automatische**.

4. Op de **algemene** tabblad, start de service als deze niet in de status van een gestart.

5. Op de **algemene** tabblad **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Installeren en configureren van de CA-Modules:

In deze stap zullen we installeren en configureren van de FIM CM CA-modules op de certificeringsinstantie (CA).

1. FIM CM alleen inspecteren gebruikersmachtigingen voor beheerbewerkingen configureren

2. In de **C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Certificaatbeheer\\web** venster een kopie van  **Web.config** naamgeving van de kopie **web.1.config**.

3. In de **Web** venster met de rechtermuisknop op **Web.config**, en klik vervolgens op **Open**.

    >[!Note]
    >Het Web.config-bestand is geopend in Kladblok

4. Wanneer het bestand wordt geopend, drukt u op CTRL + F.

5. In de **zoeken en vervangen** in het dialoogvenster de **zoeken naar** in het vak **UseUser**, en klik vervolgens op **volgende zoeken** drie keer.

6. Sluit de **zoeken en vervangen** in het dialoogvenster.

7. U moet zich op de regel  **\<key="Clm.RequestSecurity.Flags toevoegen ' waarde ="UseUser, UseGroups"/\>**. Wijzig de regel lezen  **\<key="Clm.RequestSecurity.Flags toevoegen ' waarde ="UseUser"/\>**.

8. Sluit het bestand, alle wijzigingen worden opgeslagen.

9. Maak een account voor de CA-computer op de SQL-server \<geen script\>

10. Zorg ervoor dat u met verbonden bent de **CORPSQL01** server.

11. Zorg ervoor dat u bent aangemeld als **DBA**

12. Uit de **Start** menu, start **SQL Server Management Studio**.

13. In de **verbinding maken met Server** in het dialoogvenster de **servernaam** in het vak **CORPSQL01,** en klik vervolgens op **Connect**.

14. Vouw in de consolestructuur **Security**, en klik vervolgens op **aanmeldingen**.

15. Met de rechtermuisknop op **aanmeldingen**, en klik vervolgens op **nieuwe aanmelding**.

16. Op de **algemene** pagina, in de **aanmeldingsnaam** in het vak **contoso\\CORPCA\$**. Selecteer **Windows-verificatie**. Standaard-database is **FIMCertificateManagement**.

17. Selecteer in het linkerdeelvenster **Gebruikerstoewijzing**. Klik in het rechterdeelvenster op het selectievakje in de **kaart** kolom naast **FIMCertificateManagement**. In de **lidmaatschap databaserol voor: FIMCertificateManagement** weergeven, schakelt u de **clmApp** rol.

18. Klik op **OK**.

19. Sluiten **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>De FIM CM CA-modules installeren op de certificeringsinstantie (CA)

1. Zorg ervoor dat u met verbonden bent de **CORPCA** server.

2. In de **X64** windows, met de rechtermuisknop op **Setup.exe**, en klik vervolgens op **als administrator uitvoeren**.

3. Op de **Welkom bij de Wizard Setup van Microsoft Identity Manager Certificate Management** pagina, klikt u op **volgende**.

4. Op de **gebruiksrechtovereenkomst** pagina, lees de gebruiksrechtovereenkomst. Selecteer de **ik ga akkoord met de voorwaarden van de gebruiksrechtovereenkomst** selectievakje en klik vervolgens op **volgende**.

5. Op de **aangepaste installatie** weergeeft, schakelt **MIM CM-Portal**, en klik vervolgens op **dit onderdeel kan niet worden**.

6. Op de **aangepaste installatie** weergeeft, schakelt **MIM CM-updateservice**, en klik vervolgens op **dit onderdeel kan niet worden**.

    >[!Note]
    >Dit betekent dat de bestanden van MIM CM CA als de enige functie is ingeschakeld voor de installatie.

7. Op de **aangepaste installatie** pagina, klikt u op **volgende**.

8. Op de **installeert Microsoft Identity Manager Certificate Management** pagina, klikt u op **installeren**.

9. Op de **voltooid de Wizard Setup van Microsoft Identity Manager Certificate Management** pagina, klikt u op **voltooien**.

10. Sluit alle geopende windows.

### <a name="configure-the-mim-cm-exit-module"></a>De MIM CM-uitgiftemodule configureren

1. Van **Systeembeheer**Open **certificeringsinstantie (CA)**.

2. In de consolestructuur met de rechtermuisknop op **contoso-CORPCA-CA**, en klik vervolgens op **eigenschappen**.

3. Op de **uitgiftemodule** tabblad **uitgiftemodule van FIM CM**, en klik vervolgens op **eigenschappen**.

4. In de **opgeven van de verbindingsreeks van de CM-database** in het vak **Connect Timeout = 15; Beveiliginsinfo = True; Geïntegreerde beveiliging = sspi; Initial Catalog = FIMCertificateManagement; Data Source = CORPSQL01**. Laat de **versleutel de verbindingsreeks** selectievakje is ingeschakeld en klik vervolgens op **OK**.
5. In de **Microsoft FIM Certificate Management** dialoogvenster, klikt u op **OK**.

6. In de **contoso-CORPCA-CA-eigenschappen** in het dialoogvenster, klikt u op **OK**.

7. Met de rechtermuisknop op **contoso-CORPCA-CA** *,* verwijzen naar **alle taken**, en klik vervolgens op **-Service stoppen**. Wacht totdat de Active Directory Certificate Services is gestopt.

8. Met de rechtermuisknop op **contoso-CORPCA-CA** *,* verwijzen naar **alle taken**, en klik vervolgens op **Service starten**.

9. Minimaliseer de **certificeringsinstantie (CA)** console.

10. Van **Systeembeheer**Open **logboeken**.

11. Vouw in de consolestructuur **logboeken toepassingen en Services**, en klik vervolgens op **FIM Certificate Management**.

12. Controleer of dat de meest recente gebeurtenissen doen in de lijst met gebeurtenissen, *niet* bevatten **waarschuwing** of **fout** gebeurtenissen sinds de laatste keer opnieuw opstarten van Certificate Services.

    >[!NOTE] 
    >De laatste gebeurtenis die moet worden vermeld dat de uitgiftemodule geladen met de instellingen van: `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Minimaliseer de **logboeken**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Vingerafdruk van het certificaat MIMCMAgent naar Windows® Klembord kopiëren

1. Herstel de **certificeringsinstantie (CA)** console.

2. Vouw in de consolestructuur **contoso-CORPCA-CA**, en klik vervolgens op **verleende certificaten**.

3. In de **details** deelvenster, dubbelklikt u op het certificaat met **CONTOSO\\MIMCMAgent** in de **de naam van aanvrager** kolom en met **FIM CM Ondertekening** in de **certificaatsjabloon** kolom.

4. Op de **Details** tabblad de **vingerafdruk** veld.

5. De vingerafdruk van het selecteren en druk op CTRL + C.

    >[!NOTE]
    >Voer **niet** de witruimte in de lijst met de vingerafdruk van tekens bevatten.

6. In de **certificaat** in het dialoogvenster, klikt u op **OK**.

7. Uit de **Start** menu in de **programma's en bestanden zoeken** in het vak **Kladblok**, en druk op ENTER.

8. In **Kladblok**, uit de **bewerken** menu, klikt u op **plakken**.

9. Uit de **bewerken** menu, klikt u op **vervangen**.

10. In de **zoeken naar** vak, typ een spatie en klik vervolgens op **vervangt alle**.

    >[!Note]
    >Hiermee verwijdert u alle van de afstand tussen de tekens in de vingerafdruk.

11. In de **vervangen** in het dialoogvenster, klikt u op **annuleren**.

12. Selecteer de geconverteerde *thumbprintstring*, en druk op CTRL + C.

13. Sluiten **Kladblok** zonder wijzigingen worden opgeslagen.

### <a name="configure-the-fim-cm-policy-module"></a>De FIM CM-beleidsmodule configureren

1. Herstel de **certificeringsinstantie (CA)** console.

2. Met de rechtermuisknop op **contoso-CORPCA-CA**, en klik vervolgens op **eigenschappen**.

3. In de **contoso-CORPCA-CA-eigenschappen** dialoogvenster op de **beleidsmodule** tabblad **eigenschappen**.

    - Op de **algemene** tabblad, zorg ervoor dat **niet - FIM CM-aanvragen doorgeven aan de standaardbeleidsmodule voor verwerking** is geselecteerd.

    - Op de **certificaten voor ondertekening** tabblad **toevoegen**.

    - Klik in het dialoogvenster certificaat met de rechtermuisknop op de **Geef hexadecimaal gecodeerde certificaat-hash** vak en klik vervolgens op **plakken**.

    - In de **certificaat** in het dialoogvenster, klikt u op **OK**.

        >[!Note]
        >Als de **OK** knop niet is ingeschakeld, kunt u een verborgen teken in de vingerafdruk per ongeluk opgenomen wanneer u de vingerafdruk van het certificaat clmAgent gekopieerd. Herhaal de bovenstaande stappen vanaf **taak 4: de MIMCMAgent certificaatvingerafdruk naar Windows Klembord te kopiëren** in deze oefening.

4. In de **configuratie-eigenschappen** dialoogvenster vak, zorg ervoor dat de vingerafdruk wordt weergegeven in de **geldige certificaten voor ondertekening** lijst en klik vervolgens op **OK**.

5. In de **FIM Certificate Management** dialoogvenster, klikt u op **OK**.

6. In de **contoso-CORPCA-CA-eigenschappen** in het dialoogvenster, klikt u op **OK**.

7. Met de rechtermuisknop op **contoso-CORPCA-CA** *,* verwijzen naar **alle taken**, en klik vervolgens op **-Service stoppen**.

8. Wacht totdat de Active Directory Certificate Services is gestopt.

9. Met de rechtermuisknop op **contoso-CORPCA-CA** *,* verwijzen naar **alle taken**, en klik vervolgens op **Service starten**.

10. Sluit de **certificeringsinstantie (CA)** console.

11. Sluit alle vensters openen en meld u af.

**Laatste stap in de implementatie** we willen er zeker van CONTOSO is\\MIMCM-Managers kunnen implementeren en sjablonen maken en configureren van het systeem zonder schema en de groep Domeinadministrators. Het volgende script zal ACL de machtigingen voor de certificaatsjablonen dsacls gebruiken. Voer met account dat volledige machtigingen voor het wijzigen van beveiliging heeft lees- en schrijfmachtigingen heeft voor elke bestaande certificaatsjabloon in het forest.

Eerste stappen: **Service Connection Point en doelgroep machtigingen configureren & Profielsjabloonbeheer delegeren**

1. Configureer machtigingen voor de service connection point (SCP).

2. Gedelegeerde profielsjabloonbeheer configureren.

3. Configureer machtigingen voor de service connection point (SCP). **\<geen script\>**

4.   Zorg ervoor dat u met verbonden bent de **CORPDC** virtuele server.

5. Meld u aan als **contoso\\corpadmin**

6. Van **Systeembeheer**Open **Active Directory: gebruikers en Computers**.

7. In **Active Directory: gebruikers en Computers**op de **weergave** menu, zorg ervoor dat **geavanceerde functies** is ingeschakeld.

8. Vouw in de consolestructuur **Contoso.com** \| **System** \| **Microsoft** \| **Certificate Lifecycle Manager**, en klik vervolgens op **CORPCM**.

9. Met de rechtermuisknop op **CORPCM**, en klik vervolgens op **eigenschappen**.

10. In de **CORPCM eigenschappen** dialoogvenster op de **Security** tabblad, voegt u de volgende groepen met de bijbehorende machtigingen toe:

    | Groep          | Machtigingen      |
    |----------------|------------------|
    | mimcm-Managers | Raadplegen </br> FIM CM-Audit</br> FIM CM Enrollment Agent</br> Inschrijven voor FIM CM-aanvraag</br> FIM CM-aanvraag herstellen</br> FIM CM-aanvraag vernieuwen</br> FIM CM-aanvraag in te trekken </br> FIM CM-aanvraag deblokkeren via een smartcard |
    | mimcm-HelpDesk | Raadplegen</br> FIM CM Enrollment Agent</br> FIM CM-aanvraag in te trekken</br> FIM CM-aanvraag deblokkeren via een smartcard |

11. In de **CORPDC eigenschappen** in het dialoogvenster, klikt u op **OK**.

12. Laat **Active Directory: gebruikers en Computers** openen.

**Machtigingen configureren op de onderliggende gebruikersobjecten**

1. Zorg ervoor dat u nog steeds in de **Active Directory: gebruikers en Computers** console.

2. In de consolestructuur met de rechtermuisknop op **Contoso.com**, en klik vervolgens op **eigenschappen**.

3. Op de **Security** tabblad **Geavanceerd**.

4. In de **geavanceerde beveiligingsinstellingen voor Contoso** in het dialoogvenster, klikt u op **toevoegen**.

5. In de **Select User, Computer, serviceaccount of groep** in het dialoogvenster de **Enter the object named selecteren** in het vak **mimcm-Managers**, en klik vervolgens op **OK**.

6. In de **Machtigingsvermelding voor Contoso** in het dialoogvenster de **zijn van toepassing op** in de lijst met **Descendant gebruikersobjecten** en schakel vervolgens de **toestaan**selectievakjes in voor de volgende **machtigingen**:

    - **Alle eigenschappen lezen**

    - **De machtiging lezen**

    - **FIM CM-Audit**

    - **FIM CM Enrollment Agent**

    - **Inschrijven voor FIM CM-aanvraag**

    - **FIM CM-aanvraag herstellen**

    - **FIM CM-aanvraag vernieuwen**

    - **FIM CM-aanvraag in te trekken**

    - **FIM CM-aanvraag deblokkeren via een smartcard**

7. In de **Machtigingsvermelding voor Contoso** in het dialoogvenster, klikt u op **OK**.

8. In de **geavanceerde beveiligingsinstellingen voor Contoso** in het dialoogvenster, klikt u op **toevoegen**.

9. In de **Select User, Computer, serviceaccount of groep** in het dialoogvenster de **Enter the object named selecteren** in het vak **mimcm-HelpDesk**, en klik vervolgens op **OK**.

10. In de **Machtigingsvermelding voor Contoso** in het dialoogvenster de **zijn van toepassing op** in de lijst met **Descendant gebruikersobjecten** en selecteer vervolgens de **toestaan**selectievakjes in voor de volgende **machtigingen**:

    - **Alle eigenschappen lezen**

    - **De machtiging lezen**

    - **FIM CM Enrollment Agent**

    - **FIM CM-aanvraag in te trekken**

    - **FIM CM-aanvraag deblokkeren via een smartcard**

11. In de **Machtigingsvermelding voor Contoso** in het dialoogvenster, klikt u op **OK**.

12. In de **geavanceerde beveiligingsinstellingen voor Contoso** in het dialoogvenster, klikt u op **OK**.

13. In de **contoso.com eigenschappen** in het dialoogvenster, klikt u op **OK**.

14. Laat **Active Directory: gebruikers en Computers** openen.

**Machtigingen configureren op de onderliggende gebruikersobjecten \<geen script\>**

1. Zorg ervoor dat u nog steeds in de **Active Directory: gebruikers en Computers** console.

2. In de consolestructuur met de rechtermuisknop op **Contoso.com**, en klik vervolgens op **eigenschappen**.

3. Op de **Security** tabblad **Geavanceerd**.

4. In de **geavanceerde beveiligingsinstellingen voor Contoso** in het dialoogvenster, klikt u op **toevoegen**.

5. In de **Select User, Computer, serviceaccount of groep** in het dialoogvenster de **Enter the object named selecteren** in het vak **mimcm-Managers**, en klik vervolgens op **OK**.

6. In de **Machtigingsvermelding voor CONTOSO** in het dialoogvenster de **zijn van toepassing op** in de lijst met **Descendant gebruikersobjecten** en schakel vervolgens de **toestaan**selectievakjes in voor de volgende **machtigingen**:

    - **Alle eigenschappen lezen**

    - **De machtiging lezen**

    - **FIM CM-Audit**

    - **FIM CM Enrollment Agent**

    - **Inschrijven voor FIM CM-aanvraag**

    - **FIM CM-aanvraag herstellen**

    - **FIM CM-aanvraag vernieuwen**

    - **FIM CM-aanvraag in te trekken**

    - **FIM CM-aanvraag deblokkeren via een smartcard**

7. In de **Machtigingsvermelding voor CONTOSO** in het dialoogvenster, klikt u op **OK**.

8. In de **geavanceerde beveiligingsinstellingen voor CONTOSO** in het dialoogvenster, klikt u op **toevoegen**.

9. In de **Select User, Computer, serviceaccount of groep** in het dialoogvenster de **Enter the object named selecteren** in het vak **mimcm-HelpDesk**, en klik vervolgens op **OK**.

10. In de **Machtigingsvermelding voor CONTOSO** in het dialoogvenster de **zijn van toepassing op** in de lijst met **Descendant gebruikersobjecten** en selecteer vervolgens de **toestaan**selectievakjes in voor de volgende **machtigingen**:

    - **Alle eigenschappen lezen**

    - **De machtiging lezen**

    - **FIM CM Enrollment Agent**

    - **FIM CM-aanvraag in te trekken**

    - **FIM CM-aanvraag deblokkeren via een smartcard**

11. In de **Machtigingsvermelding voor contoso** in het dialoogvenster, klikt u op **OK**.

12. In de **geavanceerde beveiligingsinstellingen voor Contoso** in het dialoogvenster, klikt u op **OK**.

13. In de **contoso.com eigenschappen** in het dialoogvenster, klikt u op **OK**.

14. Laat **Active Directory: gebruikers en Computers** openen.

Tweede stappen: **overdragen Management Certificaatsjabloonmachtigingen \<script\>**

- Het overdragen van machtigingen voor de container Certificaatsjablonen.

- Het overdragen van machtigingen voor de OID-container.

- Het overdragen van machtigingen voor de bestaande certificaatsjablonen.

Het definiëren van machtigingen voor de container Certificaatsjablonen:

1. Herstel de **Active Directory: Sites en Services** console.

2. Vouw in de consolestructuur **Services**, vouw **Public Key Services**, en klik vervolgens op **certificaatsjablonen**.

3. In de consolestructuur met de rechtermuisknop op **certificaatsjablonen**, en klik vervolgens op **beheer delegeren**.

4. In de **overdracht van beheer** Wizard, klikt u op **volgende**.

5. Op de **gebruikers of groepen** pagina, klikt u op **toevoegen**.

6. In de **Select Users, Computers of groepen** in het dialoogvenster de **Geef de objectnamen op** in het vak **mimcm-Managers**, en klik vervolgens op **OK**.

7. Op de **gebruikers of groepen** pagina, klikt u op **volgende**.

8. Op de **taken over te dragen** pagina, klikt u op **maken van een aangepaste taak om te delegeren**, en klik vervolgens op **volgende**.

9.  Op de **Active Directory-objecttype** pagina, zorg ervoor dat **deze map, bestaande objecten in deze map en het maken van nieuwe objecten in deze map** is geselecteerd en klik vervolgens op **volgende**.

10. Op de **machtigingen** pagina, in de **machtigingen** in de lijst met de **volledig beheer** selectievakje en klik vervolgens op **volgende**.

11. Op de **voltooien van de Wizard overdracht van beheer** pagina, klikt u op **voltooien**.

Het definiëren van machtigingen voor de container OID:

1. In de consolestructuur met de rechtermuisknop op **OID**, en klik vervolgens op **eigenschappen**.

2. In de **OID eigenschappen** dialoogvenster op de **Security** tabblad **Geavanceerd**.

3. In de **geavanceerde beveiligingsinstellingen voor OID** in het dialoogvenster, klikt u op **toevoegen**.

4. In de **Select User, Computer, serviceaccount of groep** in het dialoogvenster de **Enter the object named selecteren** in het vak **mimcm-Managers**, en klik vervolgens op **OK**.

5. In de **Machtigingsvermelding voor OID** dialoogvenster vak, zorg ervoor dat de machtigingen van toepassing op **dit object en alle onderliggende objecten**, klikt u op **volledig beheer**, en klik vervolgens op  **OK**.

6. In de **geavanceerde beveiligingsinstellingen voor OID** in het dialoogvenster, klikt u op **OK**.

7. In de **OID eigenschappen** in het dialoogvenster, klikt u op **OK**.

8. Sluiten **Active Directory-Sites en Services**.

**Scripts: Machtigingen voor de container OID, Profielsjabloon & certificaatsjablonen**

![Diagram](media/mim-cm-deploy/image021.png)

```powershell
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

**Scripts: Overdragen van machtigingen voor de bestaande certificaatsjablonen.**  

![Diagram](media/mim-cm-deploy/image039.png)

```shell
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
```
