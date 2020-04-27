---
title: Microsoft Identity Manager certificaat beheerder implementeren | Microsoft Docs
description: Microsoft Identity Manager 2016-certificaat beheerder installeren
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 35fe08363b6964bf6d264ab1e60cd9751aa7b6aa
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043032"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Microsoft Identity Manager Certificate Manager 2016 (MIM CM) implementeren

De installatie van Microsoft Identity Manager Certificate Manager 2016 (MIM CM) omvat een aantal stappen. Een manier om het proces te vereenvoudigen. Er zijn voorbereidende stappen die moeten worden uitgevoerd vóór de daad werkelijke MIM CM stappen. Zonder de voorbereidende werkzaamheden is de installatie waarschijnlijk mislukt.

In het onderstaande diagram ziet u een voor beeld van het type omgeving dat kan worden gebruikt. De systemen met nummers zijn opgenomen in de lijst onder het diagram en zijn vereist voor het volt ooien van de stappen die in dit artikel worden besproken. Ten slotte worden de Windows 2016 Data Center-servers gebruikt:

![Omgevings diagram](media/mim-cm-deploy/image001.png)

1. CORPDC – domein controller
2. CORPCM – MIM CM server
3. CORPCA: certificerings instantie
4. CORPCMR – MIM CM rest API-Web – CM-portal voor rest API – gebruikt voor later
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1: Windows 10-domein toegevoegd

## <a name="deployment-overview"></a>Implementatieoverzicht

- Basis installatie van het besturings systeem

    Het lab bestaat uit Windows 2016 Data Center-servers.

    >[!NOTE]
    >Meer informatie over de ondersteunde platforms voor MIM 2016 vindt u in het artikel met de titel [ondersteunde platforms voor mim 2016](microsoft-identity-manager-2016-supported-platforms.md).

1. Stappen voorafgaand aan de implementatie

    - [Het schema uitbreiden](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Service accounts maken

    - [Certificaat sjablonen maken](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Kerberos configureren

    - Database-gerelateerde stappen

        - SQL-configuratie vereisten

        - Database machtigingen

2. Implementatie

## <a name="pre-deployment-steps"></a>Stappen voorafgaand aan de implementatie

De configuratie wizard voor MIM CM vereist dat er informatie wordt verstrekt, zodat deze correct kan worden voltooid.

![schema](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Het schema uitbreiden

Het proces om het schema uit te breiden, is eenvoudig, maar moet voorzichtig worden verzorgd door de onomkeerbare aard.

>[!NOTE]
>Voor deze stap moet het account dat is gebruikt schema-administrator rechten hebben.

1. Blader naar de locatie van de MIM-media en navigeer \\naar de\\map certificaat beheer x64.

2. Kopieer de schemamappartitie naar CORPDC en navigeer naar de map.

    ![schema](media/mim-cm-deploy/image005.png)

3. Voer het script resourceForestModifySchema. vbs single forest uit. Voer de scripts uit voor het scenario van het resource-forest:
   - DomeinA: gebruikers gevonden (userForestModifySchema. vbs)
   - ResourceForestB: de locatie van CM-installatie (resourceForestModifySchema. vbs).

     >[!NOTE]
     >Schema wijzigingen zijn een eenrichtings bewerking en vereisen dat een forest recovery wordt teruggedraaid. Zorg er dus voor dat u de benodigde back-ups hebt. Raadpleeg het artikel [Forefront Identity Manager 2010 Certificate Management schema-wijzigingen](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx) voor meer informatie over de wijzigingen die in het schema zijn aangebracht door deze bewerking uit te voeren.

     ![schema](media/mim-cm-deploy/image007.png)

4. Voer het script uit en u ontvangt een bericht zodra het script is voltooid.

    ![Het bericht Geslaagd](media/mim-cm-deploy/image009.png)

Het schema in AD is nu uitgebreid ter ondersteuning van MIM CM.

### <a name="creating-service-accounts-and-groups"></a>Service accounts en-groepen maken

De volgende tabel bevat een overzicht van de accounts en machtigingen die vereist zijn voor MIM CM. U kunt de MIM CM de volgende accounts automatisch maken of u kunt deze maken vóór de installatie. De werkelijke account namen kunnen worden gewijzigd. Als u de accounts zelf maakt, kunt u de gebruikers accounts in een hand omdraai een naam geven, zodat de gebruikers account naam eenvoudig kan worden vergeleken met de functie.

Bezoekers

![Diagram](media/mim-cm-deploy/image010.png)

![Diagram](media/mim-cm-deploy/image012.png)

| **Rol**                   | **Aanmeldings naam van gebruiker** |
|----------------------------|---------------------|
| MIM CM-agent               | MIMCMAgent          |
| Sleutel herstel agent MIM CM  | MIMCMKRAgent        |
| Autorisatie agent MIM CM | MIMCMAuthAgent      |
| CA-beheer agent MIM CM    | MIMCMManagerAgent   |
| Agent voor webgroep MIM CM      | MIMCMWebAgent       |
| Inschrijvings agent MIM CM    | MIMCMEnrollAgent    |
| MIM CM-Update service      | MIMCMService        |
| MIM-installatie account        | MIMINSTALL          |
| Help Desk-agent            | CMHelpdesk1-2       |
| Beheer van CM                 | CMManager1-2        |
| Gebruiker van de abonnee            | CMUser1-2           |

Groepen

| **Rol**               | **Groep**         |
|------------------------|-------------------|
| IB-leden van de Help Desk    | MIMCM-Help Desk    |
| Leden van CM Manager     | MIMCM-managers    |
| IB-abonnees leden | MIMCM-abonnees |

Power shell: agent-accounts:

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

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Lokaal beleid voor **CORPCM** -server voor agent accounts bijwerken 

| **Aanmeldings naam van gebruiker** | **Beschrijving en machtigingen**   |
|------|---------------------|
| MIMCMAgent          | Biedt de volgende services: </br>-Versleutelde persoonlijke sleutels van de CA worden opgehaald. </br>-Hiermee worden gegevens van de pincode van de Smart Card beveiligd in de FIM CM-data base. </br>-De communicatie tussen FIM CM en de CA beveiligen. </br></br> Voor dit gebruikers account zijn de volgende instellingen voor toegangs beheer vereist:</br>-   **Lokaal gebruikers recht aanmelden toestaan** .</br>-   Het gebruikers recht **certificaten verlenen en beheren** . </br>-Lees-en schrijf machtigingen voor de tijdelijke map van het systeem op de volgende locatie:\\% windir% Temp.</br>-Een digitaal hand tekening-en versleutelings certificaat dat is uitgegeven en geïnstalleerd in het archief van de gebruiker.
|MIMCMKRAgent        | Hiermee herstelt u gearchiveerde persoonlijke sleutels van de CA. Voor dit gebruikers account zijn de volgende instellingen voor toegangs beheer vereist:</br> -   **Lokaal gebruikers recht aanmelden toestaan** .</br>-Lidmaatschap van de lokale groep **Administrators** . </br>-De machtiging Inschrijven voor het certificaat sjabloon **KeyRecoveryAgent** . </br>-Het certificaat van de sleutel herstel agent wordt uitgegeven en in het gebruikers archief geïnstalleerd. Het certificaat moet worden toegevoegd aan de lijst met de sleutel herstel agenten op de CA. </br>-Lees machtiging en schrijf machtiging voor de tijdelijke map systeem op de volgende locatie:```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Bepaalt de gebruikers rechten en machtigingen voor gebruikers en groepen. Voor dit gebruikers account zijn de volgende instellingen voor toegangs beheer vereist: </br>-Lidmaatschap van de pre-Windows 2000 Compatible Access-domein groep. </br> -Het gebruikers recht **beveiligings controles genereren** heeft verleend.             |
| MIMCMManagerAgent   | CA-beheer activiteiten uitvoeren. </br> Aan deze gebruiker moet de machtiging CA beheren worden toegewezen.        |
| MIMCMWebAgent       | Hiermee wordt de identiteit van de IIS-toepassings groep geboden. FIM CM wordt uitgevoerd binnen een micro soft Win32® Application Programming Interface proces dat gebruikmaakt van de referenties van deze gebruiker. </br> Voor dit gebruikers account zijn de volgende instellingen voor toegangs beheer vereist:</br> -Lidmaatschap van de lokale **IIS_WPG, windows 2016 = IIS_IUSRS** groep. </br>-Lidmaatschap van de lokale groep **Administrators** .</br>-Het gebruikers recht **beveiligings controles genereren** heeft verleend. </br>-Heeft de **handeling verleend als onderdeel van het gebruikers recht van het besturings systeem** . </br>-Het gebruikers recht **token op proces niveau vervangen** .</br>-Wordt toegewezen als de identiteit van de IIS-toepassings groep, **CLMAppPool**. </br>-Lees machtiging verlenen voor de **HKEY_LOCAL_MACHINE\\software\\micro\\Soft\\CLM v\\1.0\\server-webuser** -register sleutel. </br>-Dit account moet ook worden vertrouwd voor delegering.|
| MIMCMEnrollAgent    | Voert inschrijving namens een gebruiker uit. Voor dit gebruikers account zijn de volgende instellingen voor toegangs beheer vereist:</br>-Een inschrijvings agent certificaat dat is uitgegeven en geïnstalleerd in het archief van de gebruiker.</br>-   **Lokaal gebruikers recht aanmelden toestaan** . </br>-Schrijf de machtiging voor de **inschrijvings agent** certificaat sjabloon (of de aangepaste sjabloon als deze wordt gebruikt).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Certificaat sjablonen maken voor MIM CM-service accounts

Drie van de service accounts die worden gebruikt door MIM CM een certificaat vereisen en de configuratie wizard vereist dat u de naam opgeeft van de certificaat sjablonen die het moet gebruiken om certificaten voor hen aan te vragen.

De service accounts waarvoor certificaten zijn vereist zijn:

- MIMCMAgent: voor dit account is een gebruikers certificaat vereist

- MIMCMEnrollAgent: voor dit account is een inschrijvings agent certificaat vereist

- MIMCMKRAgent: voor dit account is een certificaat voor een **sleutel herstel agent** vereist

Er zijn al sjablonen aanwezig in AD, maar we moeten onze eigen versies maken om met MIM CM te werken. We moeten wijzigingen aanbrengen in de oorspronkelijke basislijn sjablonen.

Alle drie de bovenstaande accounts hebben verhoogde rechten binnen uw organisatie en moeten zorgvuldig worden afgehandeld.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>De sjabloon voor het MIM CM-handtekening certificaat maken

1. Open **certificerings instantie**vanuit **systeem beheer**.

2. Vouw **Contoso-CorpCA**uit in de console structuur van de **certificerings instantie** en klik op **certificaat sjablonen**.

3. Klik met de rechter muisknop op **certificaat sjablonen**en klik vervolgens op **beheren**.

4. Selecteer in de **certificaat sjablonen-console**in het **detail** venster, klik met de rechter muisknop op **gebruiker**en klik vervolgens op **sjabloon dupliceren**.

5. Selecteer in het dialoog venster **sjabloon dupliceren** de optie **Windows Server 2003 Enter prise**en klik vervolgens op **OK**.

    ![Resulterende wijzigingen weer geven](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >MIM CM werkt niet met certificaten op basis van certificaat sjablonen van versie 3. U moet een certificaat sjabloon voor Windows Server® 2003 Enter prise (versie 2) maken. Zie [v3-Details](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) voor meer informatie.

6. Typ in het dialoog venster **Eigenschappen van nieuwe sjabloon** op het tabblad **Algemeen** in het vak **weergave naam sjabloon** de tekst **MIM cm ondertekenen**. Wijzig de **geldigheids periode** in **2 jaar**en schakel vervolgens het selectie vakje **certificaat publiceren in Active Directory** uit.

7. Controleer op het tabblad **afhandeling van aanvragen** of het selectie vakje de **persoonlijke sleutel exporteren toestaan** is ingeschakeld en klik vervolgens op **tabblad crypto grafie**.

8. Schakel in het dialoog venster **crypto grafie selecteren** de optie **micro soft Enhanced Cryptographic Provider v 1.0**uit, Schakel **micro soft Enhanced RSA en AES Cryptographic Provider**in en klik vervolgens op **OK**.

9. Schakel op het tabblad **onderwerpnaam** het selectie vakje **e-mail naam in onderwerpnaam** en **e-mail naam** toevoegen in.

10. Controleer op het tabblad **extensies** in de lijst **extensies die in deze sjabloon zijn opgenomen** of **toepassingen beleid** is geselecteerd en klik vervolgens op **bewerken**.

11. Selecteer in het dialoog venster **toepassingen beleids uitbreiding bewerken** zowel de **Encrypting File System** als het toepassings beleid voor **beveiligde e-mail** . Klik op **verwijderen**en vervolgens op **OK**.

12. Voer op het tabblad **beveiliging** de volgende stappen uit:

    - Verwijder de **beheerder**.

    - **Domein Administrators**verwijderen.

    - **Domein gebruikers**verwijderen.

    - Wijs alleen **Lees** -en **Schrijf** machtigingen toe aan **ondernemings Administrators**.

    - Voeg **MIMCMAgent toe.**

    - **Lees** -en **registratie** machtigingen toewijzen aan **MIMCMAgent**.

13. Klik op **OK**in het dialoog venster **Eigenschappen van nieuwe sjabloon** .

14. Sluit de **certificaat sjablonen console** geopend.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Het certificaat sjabloon voor de MIM CM inschrijvings agent maken

1. Selecteer in de **certificaat sjablonen-console**in het **detail** venster, klik met de rechter muisknop op **inschrijvings agent**en klik vervolgens op **sjabloon dupliceren**.

2. Selecteer in het dialoog venster **sjabloon dupliceren** de optie **Windows Server 2003 Enter prise**en klik vervolgens op **OK**.

3. In het dialoog venster **Eigenschappen van nieuwe sjabloon** , op het tabblad **Algemeen** in het vak **weergave naam sjabloon** , typt u **MIM cm inschrijvings agent**. Zorg ervoor dat de **geldigheids periode** **2 jaar**is.

4. Schakel op het tabblad **afhandeling van aanvragen** het exporteren van de **persoonlijke sleutel**in en klik vervolgens op het **tabblad csp's of crypto grafie.**

5. Schakel in het dialoog venster **CSP-selectie** de optie **micro soft Base Cryptographic Provider v 1.0**uit, Schakel **micro soft Enhanced Cryptographic Provider v 1.0**uit, Schakel **micro soft Enhanced RSA en AES Cryptographic Provider**in en klik vervolgens op **OK**.

6. Op het tabblad **beveiliging** voert u de volgende handelingen uit:

    - Verwijder de **beheerder**.

    - **Domein Administrators**verwijderen.

    - Wijs alleen **Lees** -en **Schrijf** machtigingen toe aan **ondernemings Administrators**.

    - Voeg **MIMCMEnrollAgent**toe.

    - **Lees** -en **registratie** machtigingen toewijzen aan **MIMCMEnrollAgent**.

7. Klik op **OK**in het dialoog venster **Eigenschappen van nieuwe sjabloon** .

8. Sluit de **certificaat sjablonen console** geopend.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>De certificaat sjabloon voor de MIM CM sleutel herstel agent maken

1. Selecteer in de **certificaat sjablonen** -console in het **detail** venster, klik met de rechter muisknop op **sleutel herstel agent**en klik vervolgens op **sjabloon dupliceren**.

2. Selecteer in het dialoog venster **sjabloon dupliceren** de optie **Windows Server 2003 Enter prise**en klik vervolgens op **OK**.

3. In het dialoog venster **Eigenschappen van nieuwe sjabloon** , op het tabblad **Algemeen** in het vak **weergave naam sjabloon** , typt u **MIM cm sleutel herstel agent**. Zorg ervoor dat de **geldigheids periode** **2 jaar** is op het **tabblad crypto grafie.**

4. Schakel in het dialoog venster **providers selecteren** de optie **micro soft Enhanced Cryptographic Provider v 1.0**uit, Schakel **micro soft Enhanced RSA en AES Cryptographic Provider**in en klik vervolgens op **OK**.

5. Zorg ervoor dat de **goed keuring van de CA-certificaat beheerder** is **uitgeschakeld**op het tabblad **uitgifte vereisten** .

6. Op het tabblad **beveiliging** voert u de volgende handelingen uit:

    - Verwijder de **beheerder**.

    - **Domein Administrators**verwijderen.

    - Wijs alleen **Lees** -en **Schrijf** machtigingen toe aan **ondernemings Administrators**.

    - Voeg **MIMCMKRAgent**toe.

    - **Lees** -en **registratie** machtigingen toewijzen aan **KRAgent**.

7. Klik op **OK**in het dialoog venster **Eigenschappen van nieuwe sjabloon** .

8. Sluit de **certificaat sjablonen console**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>De vereiste certificaat sjablonen publiceren bij de certificerings instantie

1. Herstel de **certificerings instantie** console.

2. Klik in de console structuur van de **certificerings instantie** console met de rechter muisknop op **certificaat sjablonen**, wijs **Nieuw**aan en klik vervolgens op **te verlenen certificaat sjablonen**.

3. Selecteer in het dialoog venster **certificaat sjablonen inschakelen** het **MIM cm inschrijvings agent**, **MIM CM sleutel herstel agent**en **MIM cm ondertekening**. Klik op **OK**.

4. Klik in de console structuur op **certificaat sjablonen**.

5. Controleer of de drie nieuwe sjablonen worden weer gegeven in het **detail** venster en sluit **certificerings instantie**.

    ![MIM CM ondertekenen](media/mim-cm-deploy/image016.png)

6. Sluit alle geopende vensters en meld u af.

### <a name="iis-configuration"></a>IIS-configuratie

Als u de website voor CM wilt hosten, installeert en configureert u IIS.

#### <a name="install-and-configure-iis"></a>IIS installeren en configureren

1. Meld u aan bij **CORLog** als **MIMINSTALL** -account

    >[!IMPORTANT]
    >Het MIM-installatie account moet een lokale beheerder zijn

2. Open Power shell en voer de volgende opdracht uit

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Een site met de naam standaard website wordt standaard geïnstalleerd in IIS 7. Als de naam van de site is gewijzigd of als er een site met de naam standaard website is verwijderd, moet deze beschikbaar zijn voordat MIM CM kan worden geïnstalleerd.

#### <a name="configuring-kerberos"></a>Kerberos configureren

Het MIMCMWebAgent-account wordt uitgevoerd op de MIM CM Portal. Standaard wordt in IIS en kernel-modus verificatie standaard in IIS gebruikt. U schakelt Kerberos-kernelmodus-verificatie uit en configureert Spn's in het MIMCMWebAgent-account. Voor sommige opdrachten is verhoogde machtiging vereist in Active Directory en CORPCM server.

![Diagram](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**IIS bijwerken op CORPCM**

![schema](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>U moet een DNS A-record toevoegen voor de ' cm.contoso.com ' en het IP-adres van CORPCM aanwijzen

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>SSL vereisen op de MIM CM Portal

Het is sterk aan te raden SSL te vereisen op de MIM CM Portal. Als u de wizard niet meer ontvangt, wordt u hiervan op de hoogte.

1. Registreren in webcertificaat voor **cm.contoso.com** toewijzen aan standaard site

2. Open **IIS-beheer** en navigeer naar **certificaat beheer**

3. Dubbel klik in functie weergave op SSL-instellingen.

4. Selecteer op de pagina SSL-instellingen de optie **SSL vereisen**.

5. Klik in het deel venster acties op **Toep assen.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>**CORPSQL** voor de database configuratie voor MIM cm

1. Zorg ervoor dat u bent verbonden met de CORPSQL01-server.

2. Zorg ervoor dat u bent aangemeld als SQL-DBA.

3. Voer het volgende T-SQL-script uit om het\\CONTOSO MIMINSTALL-account toe te staan om de data base te maken wanneer u naar de configuratie stap gaat

    >[!NOTE]
    >U moet terugkeren naar SQL wanneer u klaar bent voor de beleids module afsluiten &

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Fout bericht van de wizard MIM CM configuratie](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Implementatie van Microsoft Identity Manager 2016-certificaat beheer

1. Zorg ervoor dat u bent verbonden met de CORPCM-server en dat het **MIMINSTALL** -account lid is van de **lokale groep Administrators** .

2. Zorg ervoor dat u bent aangemeld als\\contoso MIMINSTALL.

3. Koppel de Microsoft Identity Manager SP1 ISO.

4. **Open** de Directory **certificaat\\beheer x64** .

5. Klik in het **x64** -venster met de rechter muisknop op **Setup**en klik vervolgens op **als administrator uitvoeren**.

6. Klik op de welkomst pagina van de wizard Microsoft Identity Manager Certificate Management Setup op **Volgende.**

7. Lees de overeenkomst op de pagina gebruiksrecht overeenkomst, schakel het **selectie vakje**ik ga akkoord met de voor waarden in de gebruiksrecht overeenkomst in en klik op volgende.

8. Zorg ervoor dat op de pagina aangepaste installatie de **MIM cm Portal** -en **MIM cm Update-service onderdelen** zijn ingesteld om te worden geïnstalleerd en **klik vervolgens op volgende**.

9. Controleer op de pagina virtuele webmap of de naam van de virtuele map **CertificateManagement**is en klik vervolgens **op volgende**.

10. Klik op de pagina Installeer Microsoft Identity Manager Certificate Management **op installeren**.

11. Klik op de pagina de wizard Microsoft Identity Manager Certificate Management Setup **is voltooid** **op volt ooien**.

![Wizard MIM CM voltooid](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Configuratie wizard van Microsoft Identity Manager 2016-certificaat beheer

Voordat u zich aanmeldt bij CORPCM, moet u MIMINSTALL toevoegen aan **domein Administrators, schema-Administrators en lokale beheerders** groep voor de configuratie wizard. Dit kan later worden verwijderd zodra de configuratie is voltooid.

![Foutbericht](media/mim-cm-deploy/image028.png)

1. Klik in het menu **Start** op **wizard Configuratie van certificaat beheer**. En als **Administrator** uitvoeren

2. Klik op de pagina **Welkom bij de wizard Configuratie** op **volgende**.

3. Controleer op de pagina **CA-configuratie** of de geselecteerde certificerings instantie **Contoso-CORPCA-ca**is, of de geselecteerde server **CORPCA is. CONTOSO.COM**en klik vervolgens op **volgende**.

4. Op de pagina **micro soft® SQL Server®-Data Base instellen** , in het vak **naam van SQL Server** , typt u **CORPSQL1** , schakelt u het selectie vakje **Mijn referenties gebruiken om de data base te maken** in en klikt u op **volgende**.

5. Accepteer op de pagina **Data Base-instellingen** de standaard naam van de Data Base **FIMCertificateManagement**, zorg ervoor dat **geïntegreerde SQL-verificatie** is geselecteerd en klik vervolgens op **volgende**.

6. Accepteer op de pagina **Active Directory instellen** de standaard naam die is opgegeven voor het service aansluitpunt en klik op **volgende**.

7. Controleer op de pagina **verificatie methode** bevestigen dat **geïntegreerde Windows-verificatie** is geselecteerd en klik vervolgens op **volgende**.

8. Schakel op de pagina **agents-FIM cm** het selectie vakje **de FIM cm-standaard instellingen gebruiken** uit en klik vervolgens op **aangepaste accounts**.

9. In het dialoog venster **agents – FIM cm** met meerdere tabbladen op elk tabblad, typt u de volgende gegevens:

   - Gebruikers naam: **bijwerken**

   - Wacht woord **:\@Pass word1**

   - Bevestig het wacht woord: **Pass\@word1**

   - Een bestaande gebruiker gebruiken: **ingeschakeld**

     >[!NOTE]
     >We hebben deze accounts eerder gemaakt. Zorg ervoor dat de procedures in stap 8 worden herhaald voor alle zes de tabbladen met agent accounts.

     ![MIM CM accounts](media/mim-cm-deploy/image030.png)

10. Klik op **OK**wanneer alle account gegevens van de agent zijn voltooid.

11. Klik op de pagina **agents – MIM cm** op **volgende**.

12. Schakel op de pagina **server certificaten instellen** de volgende certificaat sjablonen in:

    - Certificaat sjabloon die moet worden gebruikt voor het certificaat van de herstel agent sleutel herstel agent: **MIMCMKeyRecoveryAgent**.

    - Certificaat sjabloon die moet worden gebruikt voor het FIM CM-agent certificaat: **MIMCMSigning**.

    - Certificaat sjabloon die moet worden gebruikt voor het certificaat van de inschrijvings agent: **FIMCMEnrollmentAgent**.

13. Klik op de pagina **server certificaten instellen** op **volgende**.

14. Op de pagina **e-mail server instellen, document afdrukken** klikt u in het vak **Geef de naam op van de SMTP-server die u wilt gebruiken voor het registreren van registratie meldingen** en klik vervolgens op **Volgende.**

15. Klik op de pagina **Gereed om te configureren** op **Configureren**.

16. Klik in het dialoog venster **configuratie wizard – micro soft Forefront Identity Manager 2010 R2** -waarschuwing op **OK** om te bevestigen dat SSL niet is ingeschakeld op de virtuele IIS-map.

    ![Media/image17. png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Klik niet op de knop volt ooien totdat de uitvoering van de configuratie wizard is voltooid. De wizard logboek registratie voor kan worden gevonden: **% Program\\files% micro soft\\Forefront\\Identity Management\\2010 Certificate Management config. log**

17. Klik op **Voltooien**.

    ![Wizard MIM CM voltooid](media/mim-cm-deploy/image033.png)

18. Sluit alle geopende vensters.

19. Voeg `https://cm.contoso.com/certificatemanagement` toe aan de zone Lokaal intranet in uw browser.

20. Ga naar de site van de server CORPCM`https://cm.contoso.com/certificatemanagement`  

    ![schema](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>De isolatie service voor CNG-sleutels controleren

1. Open **Services**in **systeem beheer**.

2. Dubbel klik in het **detail** venster op **CNG-sleutel isolatie**.

3. Wijzig het **opstart type** op het tabblad **Algemeen** in **automatisch**.

4. Start de service op het tabblad **Algemeen** als deze niet aan de status gestart is.

5. Klik op het tabblad **Algemeen** op **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>De CA-modules installeren en configureren:

In deze stap zullen we de FIM CM-CA-modules installeren en configureren op de certificerings instantie.

1. FIM CM zodanig configureren dat alleen gebruikers machtigingen voor beheer bewerkingen worden gecontroleerd

2. Maak in het **venster\\C:\\Program Files micro soft\\Forefront\\Identity Manager\\2010 Certificate Management** een kopie van **Web. config** met de naam Copy **Web. 1. config**.

3. Klik in **het** webvenster met de rechter muisknop op **Web. config**en klik vervolgens op **openen**.

    >[!Note]
    >Het bestand Web. config wordt geopend in Klad blok

4. Wanneer het bestand wordt geopend, drukt u op CTRL + F.

5. Typ **UseUser**in het dialoog venster **zoeken en vervangen** in het vak **zoeken naar** en klik vervolgens op volgende drie keer **zoeken** .

6. Sluit het dialoog venster **zoeken en vervangen** .

7. U moet de regel ** \<add key = "clm. RequestSecurity. Flags" value = "UseUser, UseGroups"/\>**. Wijzig de regel in ** \<add key = "clm. RequestSecurity. Flags" value = "UseUser"/\>**.

8. Sluit het bestand en sla alle wijzigingen op.

9. Een account maken voor de CA-computer op de SQL \<-server zonder script\>

10. Zorg ervoor dat u bent verbonden met de **CORPSQL01** -server.

11. Zorg ervoor dat u bent aangemeld als **DBA**

12. Start **SQL Server Management Studio**in het menu **Start** .

13. Typ **CORPSQL01** in het vak **Server naam** in het dialoog venster **verbinding maken met server** en klik vervolgens op **verbinding maken**.

14. Vouw **beveiliging**uit in de console structuur en klik vervolgens op **aanmeldingen**.

15. Klik met de rechter muisknop op **aanmeldingen**en klik vervolgens op **nieuwe aanmelding**.

16. Typ op de pagina **Algemeen** in het vak **aanmeldings naam** **Contoso\\CORPCA\$**. Selecteer **Windows-verificatie**. De standaard database is **FIMCertificateManagement**.

17. Selecteer **gebruikers toewijzing**in het linkerdeel venster. Klik in het rechterdeel venster op het selectie vakje in de kolom **kaart** naast **FIMCertificateManagement**. Schakel in de lijst **lidmaatschap van databaserol voor: FIMCertificateManagement** de functie **clmApp** in.

18. Klik op **OK**.

19. Sluit **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>De FIM CM-CA-modules installeren op de certificerings instantie

1. Zorg ervoor dat u bent verbonden met de **CORPCA** -server.

2. Klik in **x64** Windows met de rechter muisknop op **Setup. exe**en klik vervolgens op **als administrator uitvoeren**.

3. Klik op de welkomst pagina van **de Wizard Microsoft Identity Manager Certificate Management Setup** op **volgende**.

4. Lees de overeenkomst op de pagina **gebruiksrecht overeenkomst** . Schakel het selectie vakje **Ik ga akkoord met de voor waarden in de gebruiksrecht overeenkomst in** en klik op **volgende**.

5. Selecteer op de pagina **aangepaste installatie** de optie **MIM cm Portal**en klik vervolgens op **deze functie is niet beschikbaar**.

6. Selecteer op de pagina **aangepaste installatie** de optie **MIM cm Update service**en klik vervolgens op **deze functie is niet beschikbaar**.

    >[!Note]
    >Hiermee verlaat u de MIM CM CA-bestanden als enige functie die is ingeschakeld voor de installatie.

7. Klik op de pagina **aangepaste installatie** op **volgende**.

8. Klik op de pagina **installeer Microsoft Identity Manager Certificate Management** op **installeren**.

9. Klik op de pagina **de Wizard Microsoft Identity Manager Certificate Management Setup is voltooid** op **volt ooien**.

10. Sluit alle geopende vensters.

### <a name="configure-the-mim-cm-exit-module"></a>De module MIM CM exit configureren

1. Open **certificerings instantie**vanuit **systeem beheer**.

2. Klik in de console structuur met de rechter muisknop op **Contoso-CORPCA-ca**en klik vervolgens op **Eigenschappen**.

3. Selecteer op het tabblad **uitgifte module** de optie **FIM cm-afsluit module**en klik vervolgens op **Eigenschappen**.

4. Typ in het vak **het Connection String de cm-data base opgeven de** tekst **time-out voor Connect = 15; Beveiligings gegevens behouden = True; Integrated Security = SSPI; Initial Catalog = FIMCertificateManagement; data source = CORPSQL01**. Zorg ervoor dat het selectie vakje **verbindings reeks versleutelen** is ingeschakeld en klik vervolgens op **OK**.
5. Klik in het bericht venster van **micro soft FIM Certificate Management** op **OK**.

6. Klik in het dialoog venster **Contoso-CORPCA-CA-eigenschappen** op **OK**.

7. Klik met de rechter muisknop op **Contoso-CORPCA-ca** *,* wijs **alle taken**aan en klik vervolgens op **service stoppen**. Wacht totdat Active Directory Certificate Services wordt gestopt.

8. Klik met de rechter muisknop op **Contoso-CORPCA-ca** *,* wijs **alle taken**aan en klik vervolgens op **service starten**.

9. Minimaliseer de **certificerings instantie** console.

10. Open **Logboeken**in **systeem beheer**.

11. Vouw in de console structuur de optie **Logboeken voor toepassingen en services**uit en klik vervolgens op **FIM-certificaat beheer**.

12. Controleer in de lijst met gebeurtenissen of de meest recente gebeurtenissen *geen* **waarschuwingen** of **fout** gebeurtenissen bevatten sinds de laatste keer dat Certificate Services opnieuw is opgestart.

    >[!NOTE] 
    >De laatste gebeurtenis moet aangeven dat de uitgifte module is geladen met behulp van instellingen van:`SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Minimaliseer het **Logboeken**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>De vinger afdruk van het MIMCMAgent-certificaat kopiëren naar het klem bord van Windows®

1. Herstel de **certificerings instantie** console.

2. Vouw in de console structuur **Contoso-CORPCA-ca**uit en klik vervolgens op **verleende certificaten**.

3. Dubbel klik in het **detail** venster op het certificaat met **\\CONTOSO MIMCMAgent** in de kolom **naam van aanvrager** en met **FIM cm-ondertekening** in de kolom **certificaat sjabloon** .

4. Selecteer op het tabblad **Details** het veld **Vingerafdruk**.

5. Selecteer de vinger afdruk en druk vervolgens op CTRL + C.

    >[!NOTE]
    >Neem **niet** de voorloop ruimte op in de lijst met vinger afdruk-tekens.

6. Klik in het dialoog venster **certificaat** op **OK**.

7. Typ in het menu **Start** in het vak **Program Ma's en bestanden zoeken** de tekst **Notepad**en druk op ENTER.

8. Klik in **Klad blok**in het menu **bewerken** op **Plakken**.

9. Klik in het menu **bewerken** op **vervangen**.

10. Typ in het vak **zoeken naar** een spatie en klik vervolgens op **Alles vervangen**.

    >[!Note]
    >Hiermee verwijdert u alle spaties tussen de tekens in de vinger afdruk.

11. Klik in het dialoog venster **vervangen** op **Annuleren**.

12. Selecteer de geconverteerde *thumbprintstring*en druk vervolgens op CTRL + C.

13. Sluit **Klad blok** zonder de wijzigingen op te slaan.

### <a name="configure-the-fim-cm-policy-module"></a>De FIM CM-beleids module configureren

1. Herstel de **certificerings instantie** console.

2. Klik met de rechter muisknop op **Contoso-CORPCA-ca**en klik vervolgens op **Eigenschappen**.

3. Klik in het dialoog venster **Contoso-CORPCA-CA-eigenschappen** op het tabblad **beleids module** op **Eigenschappen**.

    - Controleer op het tabblad **Algemeen** of **niet-FIM cm-aanvragen aan de standaard beleids module voor verwerking door geven** is geselecteerd.

    - Klik op het tabblad **handtekening certificaten** op **toevoegen**.

    - Klik in het dialoog venster certificaat met de rechter muisknop op het veld **hash voor hexadecimaal-gecodeerd certificaat opgeven** en klik vervolgens op **Plakken**.

    - Klik in het dialoog venster **certificaat** op **OK**.

        >[!Note]
        >Als de knop **OK** niet is ingeschakeld, hebt u per ongeluk een verborgen teken opgenomen in de vingerafdruk teken reeks wanneer u de vinger afdruk van het clmAgent-certificaat hebt gekopieerd. Alle stappen herhalen vanaf **taak 4: Kopieer de vinger afdruk van het MIMCMAgent-certificaat naar het Windows-klem bord** in deze oefening.

4. Controleer in het dialoog venster **configuratie-eigenschappen** of de vinger afdruk wordt weer gegeven in de lijst **geldige handtekening certificaten** en klik vervolgens op **OK**.

5. Klik in het bericht venster **FIM-certificaat beheer** op **OK**.

6. Klik in het dialoog venster **Contoso-CORPCA-CA-eigenschappen** op **OK**.

7. Klik met de rechter muisknop op **Contoso-CORPCA-ca** *,* wijs **alle taken**aan en klik vervolgens op **service stoppen**.

8. Wacht totdat Active Directory Certificate Services wordt gestopt.

9. Klik met de rechter muisknop op **Contoso-CORPCA-ca** *,* wijs **alle taken**aan en klik vervolgens op **service starten**.

10. Sluit de console **certificerings instantie** .

11. Sluit alle geopende vensters en meld u af.

**De laatste stap in de implementatie** is dat CONTOSO\\MIMCM-managers sjablonen kunnen implementeren en maken en het systeem kan configureren zonder schema-en domein beheerders. Met het volgende script worden de machtigingen voor de certificaat sjablonen geacl met behulp van Dsacls. Voer uit met een account met volledige machtigingen voor het wijzigen van de lees-en schrijf machtigingen voor de beveiliging van alle bestaande certificaat sjablonen in het forest.

Eerste stappen: **machtigingen voor het service verbindings punt en de doel groep configureren & het overdragen van profiel sjabloon beheer**

1. Configureer machtigingen op het service aansluitpunt (SCP).

2. Configureer beheer van gedelegeerde Profiel sjablonen.

3. Configureer machtigingen op het service aansluitpunt (SCP). **\<geen script\>**

4.   Zorg ervoor dat u bent verbonden met de virtuele **CORPDC** -server.

5. Meld u aan **als\\contoso corpadmin**

6. Open **Active Directory gebruikers en computers**in **systeem beheer**.

7. Zorg ervoor dat in **Active Directory gebruikers en computers**in het menu **beeld** de optie **geavanceerde functies** is ingeschakeld.

8. Vouw in de console structuur **contoso.com** \| **System** \| **micro soft** \| **Certificate Lifecycle Manager**uit en klik vervolgens op **CORPCM**.

9. Klik met de rechter muisknop op **CORPCM**en klik vervolgens op **Eigenschappen**.

10. Voeg in het dialoog venster **CORPCM-eigenschappen** op het tabblad **beveiliging** de volgende groepen toe met de bijbehorende machtigingen:

    | Groep          | Machtigingen      |
    |----------------|------------------|
    | mimcm-managers | Lezen </br> FIM CM-audit</br> FIM CM-inschrijvings agent</br> FIM CM-aanvraag registratie</br> FIM CM-aanvraag herstellen</br> FIM CM-aanvraag vernieuwen</br> FIM CM-aanvraag intrekken </br> Smart Card voor blok kering van FIM CM-aanvraag |
    | mimcm-Help Desk | Lezen</br> FIM CM-inschrijvings agent</br> FIM CM-aanvraag intrekken</br> Smart Card voor blok kering van FIM CM-aanvraag |

11. Klik in het dialoog venster **CORPDC-eigenschappen** op **OK**.

12. **Active Directory gebruikers en computers** open laten.

**Machtigingen voor de onderliggende gebruikers objecten configureren**

1. Zorg ervoor dat u zich nog steeds in de console **Active Directory gebruikers en computers** bevindt.

2. Klik in de console structuur met de rechter muisknop op **contoso.com**en klik vervolgens op **Eigenschappen**.

3. Klik op **Geavanceerd** op het tabblad **Beveiliging**.

4. Klik in het dialoog venster **Geavanceerde beveiligings instellingen voor contoso** op **toevoegen**.

5. Typ **mimcm-managers**in het dialoog venster **gebruiker, computer, Service account of groep selecteren** in het vak **Geef de object naam op** , en klik vervolgens op **OK**.

6. In het dialoog venster **machtigings vermelding voor contoso** selecteert u **onderliggende gebruikers objecten** in de lijst van **toepassing op** en schakelt u het selectie vakje **toestaan** in voor de volgende **machtigingen**:

    - **Alle eigenschappen lezen**

    - **Lees machtigingen**

    - **FIM CM-audit**

    - **FIM CM-inschrijvings agent**

    - **FIM CM-aanvraag registratie**

    - **FIM CM-aanvraag herstellen**

    - **FIM CM-aanvraag vernieuwen**

    - **FIM CM-aanvraag intrekken**

    - **Smart Card voor blok kering van FIM CM-aanvraag**

7. Klik in het dialoog venster **machtigings vermelding voor contoso** op **OK**.

8. Klik in het dialoog venster **Geavanceerde beveiligings instellingen voor contoso** op **toevoegen**.

9. In het dialoog venster **gebruiker, computer, Service account of groep selecteren** in het vak **Geef de object naam op** , typt u **mimcm-Help Desk**en klikt u vervolgens op **OK**.

10. Selecteer in het dialoog venster **machtigings vermelding voor contoso** in de lijst **Toep assen op** de optie **onderliggende gebruikers objecten** en schakel vervolgens het selectie vakje **toestaan** in voor de volgende **machtigingen**:

    - **Alle eigenschappen lezen**

    - **Lees machtigingen**

    - **FIM CM-inschrijvings agent**

    - **FIM CM-aanvraag intrekken**

    - **Smart Card voor blok kering van FIM CM-aanvraag**

11. Klik in het dialoog venster **machtigings vermelding voor contoso** op **OK**.

12. Klik in het dialoog venster **Geavanceerde beveiligings instellingen voor contoso** op **OK**.

13. Klik in het dialoog venster **contoso.com-eigenschappen** op **OK**.

14. **Active Directory gebruikers en computers** open laten.

**Machtigingen voor de onderliggende gebruikers objecten \<configureren geen script\>**

1. Zorg ervoor dat u zich nog steeds in de console **Active Directory gebruikers en computers** bevindt.

2. Klik in de console structuur met de rechter muisknop op **contoso.com**en klik vervolgens op **Eigenschappen**.

3. Klik op **Geavanceerd** op het tabblad **Beveiliging**.

4. Klik in het dialoog venster **Geavanceerde beveiligings instellingen voor contoso** op **toevoegen**.

5. Typ **mimcm-managers**in het dialoog venster **gebruiker, computer, Service account of groep selecteren** in het vak **Geef de object naam op** , en klik vervolgens op **OK**.

6. In het dialoog venster **machtigings vermelding voor CONTOSO** selecteert u **onderliggende gebruikers objecten** in de lijst van **toepassing op** en schakelt u het selectie vakje **toestaan** in voor de volgende **machtigingen**:

    - **Alle eigenschappen lezen**

    - **Lees machtigingen**

    - **FIM CM-audit**

    - **FIM CM-inschrijvings agent**

    - **FIM CM-aanvraag registratie**

    - **FIM CM-aanvraag herstellen**

    - **FIM CM-aanvraag vernieuwen**

    - **FIM CM-aanvraag intrekken**

    - **Smart Card voor blok kering van FIM CM-aanvraag**

7. Klik in het dialoog venster **machtigings vermelding voor CONTOSO** op **OK**.

8. Klik in het dialoog venster **Geavanceerde beveiligings instellingen voor CONTOSO** op **toevoegen**.

9. In het dialoog venster **gebruiker, computer, Service account of groep selecteren** in het vak **Geef de object naam op** , typt u **mimcm-Help Desk**en klikt u vervolgens op **OK**.

10. Selecteer in het dialoog venster **machtigings vermelding voor CONTOSO** in de lijst **Toep assen op** de optie **onderliggende gebruikers objecten** en schakel vervolgens het selectie vakje **toestaan** in voor de volgende **machtigingen**:

    - **Alle eigenschappen lezen**

    - **Lees machtigingen**

    - **FIM CM-inschrijvings agent**

    - **FIM CM-aanvraag intrekken**

    - **Smart Card voor blok kering van FIM CM-aanvraag**

11. Klik in het dialoog venster **machtigings vermelding voor contoso** op **OK**.

12. Klik in het dialoog venster **Geavanceerde beveiligings instellingen voor contoso** op **OK**.

13. Klik in het dialoog venster **contoso.com-eigenschappen** op **OK**.

14. **Active Directory gebruikers en computers** open laten.

Tweede stappen: **certificaat sjabloon beheer machtigingen \<overdragen script\> **

- De machtigingen voor de container certificaat sjablonen worden gedelegeerd.

- De machtigingen voor de OID-container worden gedelegeerd.

- Machtigingen voor de bestaande certificaat sjablonen delegeren.

Machtigingen voor de container certificaat sjablonen definiëren:

1. Herstel de **Active Directory-console sites en-services** .

2. Vouw in de console structuur achtereenvolgens **Services**en **open bare sleutel Services**uit en klik op **certificaat sjablonen**.

3. Klik in de console structuur met de rechter muisknop op **certificaat sjablonen**en klik vervolgens op **beheer delegeren**.

4. Klik in de wizard **overdracht van beheer** op **volgende**.

5. Klik op de pagina **gebruikers of groepen** op **toevoegen**.

6. Typ in het dialoog venster **gebruikers, computers of groepen selecteren** in het vak **Geef de object namen op** de tekst **mimcm-managers**en klik vervolgens op **OK**.

7. Klik op de pagina **gebruikers of groepen** op **volgende**.

8. Klik op de pagina **taken die moeten worden overgedragen** op **een aangepaste taak maken om te**delegeren en klik vervolgens op **volgende**.

9.  Controleer op de pagina **Object Type Active Directory** of **deze map, bestaande objecten in deze map en het maken van nieuwe objecten in deze map** is geselecteerd, en klik vervolgens op **volgende**.

10. Schakel op de pagina **machtigingen** in de lijst **machtigingen** het selectie vakje **volledig beheer** in en klik vervolgens op **volgende**.

11. Klik op de pagina **de wizard Overdracht van beheer uitvoeren** op **volt ooien**.

Machtigingen voor de OID-container definiëren:

1. Klik in de console structuur met de rechter muisknop op **OID**en klik vervolgens op **Eigenschappen**.

2. Klik in het dialoog venster **Eigenschappen van OID** op het tabblad **beveiliging** op **Geavanceerd**.

3. Klik in het dialoog venster **Geavanceerde beveiligings instellingen voor OID** op **toevoegen**.

4. Typ **mimcm-managers**in het dialoog venster **gebruiker, computer, Service account of groep selecteren** in het vak **Geef de object naam op** , en klik vervolgens op **OK**.

5. Controleer in het dialoog venster **machtigings vermelding voor OID** of de machtigingen van toepassing zijn op **Dit object en alle onderliggende objecten**, klik op **volledig beheer**en klik vervolgens op **OK**.

6. Klik in het dialoog venster **Geavanceerde beveiligings instellingen voor OID** op **OK**.

7. Klik op **OK**in het dialoog venster **Eigenschappen van OID** .

8. Sluit **Active Directory-sites en-services**.

**Scripts: machtigingen voor de OID, profiel sjabloon & container certificaat sjablonen**

![schema](media/mim-cm-deploy/image021.png)

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

**Scripts: de machtigingen voor de bestaande certificaat sjablonen delegeren.**  

![schema](media/mim-cm-deploy/image039.png)

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
