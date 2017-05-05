---
title: PAM implementeren - Stap 2 - PRIV DC | Microsoft Docs
description: "De PRIV-domeincontroller voorbereiden voor de bastionomgeving waarin Privileged Access Management wordt geïsoleerd."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: edc15b41d4248887f4a93217f68d8125f6500585
ms.lasthandoff: 05/02/2017


---

# <a name="step-2---prepare-the-first-priv-domain-controller"></a>Stap 2: de eerste PRIV-domeincontroller voorbereiden

>[!div class="step-by-step"]
[« Stap 1](step-1-prepare-corp-domain.md)
[Stap 3 »](step-3-prepare-pam-server.md)

In deze stap maakt u een nieuw domein dat de bastionomgeving biedt voor verificatie door de beheerder.  Dit forest moet ten minste één domeincontroller hebben, en ten minste één lidserver. De lidserver worden geconfigureerd in de volgende stap.

## <a name="create-a-new-privileged-access-management-domain-controller"></a>Een nieuwe Privileged Access Management-domeincontroller maken

In dit gedeelte stelt u een virtuele machine in om te fungeren als een domeincontroller voor een nieuw forest

### <a name="install-windows-server-2012-r2"></a>Windows Server 2012 R2 installeren
Installeer Windows Server 2012 R2 op een andere nieuwe virtuele machine waarop geen software is geïnstalleerd om een computer 'PRIVDC' te maken.

1. Selecteer deze optie om een aangepaste installatie (niet een upgrade) van Windows Server uit te voeren. Geef bij de installatie de editie **Windows Server 2012 R2 Standard x64 (server met een GUI)** op. _Selecteer niet_ **datacentrum of serverkern**.

2. Lees en accepteer de licentievoorwaarden.

3. Omdat de schijf leeg is, selecteert u **Aangepast: alleen Windows installeren** en gebruikt u de niet-geïnitialiseerde schijfruimte.

4. Meld u na de installatie van de besturingssysteemversie bij deze nieuwe computer aan als de nieuwe beheerder. Gebruik Configuratiescherm om de computernaam in te stellen op *PRIVDC*, wijs hieraan een statisch IP-adres op het virtuele netwerk toe en configureer de DNS-server als de DNS-server van de domeincontroller die is geïnstalleerd in de vorige stap. Hiervoor moet de server opnieuw worden opgestart.

5. Nadat de server opnieuw is opgestart, moet u zich aanmelden als beheerder. Via Configuratiescherm configureert u de computer om te controleren op updates en installeert u alle vereiste updates. Hiervoor moet de server mogelijk opnieuw worden opgestart.

### <a name="add-roles"></a>Functies toevoegen
Voeg de serverfuncties AD DS (Active Directory Domain Services) en DNS toe.

1. Start PowerShell als beheerder.

2. Typ de volgende opdrachten om voor te bereiden voor een Windows Server Active Directory-installatie.

  ```
  import-module ServerManager

  Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
  ```

### <a name="configure-registry-settings-for-sid-history-migration"></a>Registerinstellingen voor de migratie van de SID-geschiedenis configureren

Start PowerShell en typ de volgende opdracht voor het configureren van het brondomein om externe procedureaanroep (Remote Procedure Call, RPC) toegang te verlenen tot de database voor beveiligingsaccountbeheer (Security Accounts Manager, SAM).

```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## <a name="create-a-new-privileged-access-management-forest"></a>Een nieuw Privileged Access Management-forest maken

Verhoog vervolgens het niveau van de server tot een domeincontroller voor een nieuw forest.

In dit document, wordt de naam priv.contoso.local gebruikt als de domeinnaam van het nieuwe forest.  De naam van het forest is niet kritiek en hoeft niet ondergeschikt te zijn aan de bestaande forestnaam in de organisatie. Echter, de domeinnaam en de NetBIOS-naam van het nieuwe forest moeten uniek en anders zijn dan die van andere domeinen in de organisatie.  

### <a name="create-a-domain-and-forest"></a>Een domein en forest maken

1. Typ de volgende opdrachten in een PowerShell-venster om het nieuwe domein te maken.  Hiermee wordt ook een DNS-delegatie gemaakt in een bovenliggende domein (contoso.local) die is gemaakt in de vorige stap.  Als u van plan bent later de DNS te configureren, moet u de `CreateDNSDelegation -DNSDelegationCredential $ca`-parameters weglaten.

  ```
  $ca= get-credential

  Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
  ```

2. Wanneer de pop-up wordt weergegeven, geeft u de referenties voor de CORP forestbeheerder op (bijvoorbeeld, de gebruikersnaam CONTOSO\\-beheerder en het bijbehorende wachtwoord uit stap 1).

3. In het PowerShell-venster wordt u gevraagd om een beheerderswachtwoord voor de veilige modus op te geven. Voer een nieuw wachtwoord twee keer in. Waarschuwingsberichten voor DNS-delegatie- en cryptografie-instellingen worden weergegeven; dit is normaal.

De server wordt opnieuw opgestart nadat het maken van het forest is voltooid.

### <a name="create-user-and-service-accounts"></a>Gebruikers- en serviceaccounts maken
Maak de gebruikers- en serviceaccounts voor de installatie van de MIM-service en -portal. Deze accounts gaat in de container Gebruikers van het domein priv.contoso.local.

1. Wanneer de server opnieuw is opgestart, meldt u zich aan bij PRIVDC als domeinbeheerder (PRIV\\Administrator).

2. Start PowerShell en typ de volgende opdrachten: Het wachtwoord 'Pass@word1' is slechts een voorbeeld en moet u een ander wachtwoord gebruiken voor de accounts.

  ```
  import-module activedirectory

  $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  New-ADUser –SamAccountName MIMMA –name MIMMA

  Set-ADAccountPassword –identity MIMMA –NewPassword $sp

  Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

  Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

  Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

  Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

  Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMSync –name MIMSync

  Set-ADAccountPassword –identity MIMSync –NewPassword $sp

  Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMService –name MIMService

  Set-ADAccountPassword –identity MIMService –NewPassword $sp

  Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SharePoint –name SharePoint

  Set-ADAccountPassword –identity SharePoint –NewPassword $sp

  Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SqlServer –name SqlServer

  Set-ADAccountPassword –identity SqlServer –NewPassword $sp

  Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

  Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

  Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

  New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

  Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

  Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

  Add-ADGroupMember "Domain Admins" SharePoint

  Add-ADGroupMember "Domain Admins" MIMService
  ```

### <a name="configure-auditing-and-logon-rights"></a>Controle- en aanmeldingsrechten configureren

U moet de controle instellen zodat de PAM-configuratie tot stand kan worden gebracht tussen forests.  

1. Zorg ervoor dat u bent aangemeld als domeinbeheerder (PRIV\\Administrator).

2. Ga naar **Start** > **Systeembeheer** > **Groepsbeleidsbeheer**.

3. Navigeer naar **Forest: priv.contoso.local** > **domeinen** > **priv.contoso.local** > **domeincontrollers** > **Standaardbeleid voor domeincontrollers**. Er wordt een waarschuwing weergegeven.

4. Klik met de rechtermuisknop op **Standaardbeleid voor domeincontrollers** en selecteer **Bewerken**.

5. Navigeer in de structuur van de consolestructuur van Groepsbeleidsbeheer-editor naar **Computerconfiguratie** > **Beleidsregels** > **Windows-instellingen** > **Beveiligingsinstellingen** > **Lokale beleidsregels** > **Controlebeleid**.

6. Klik in het detailvenster met de rechtermuisknop op **Accountbeheer controleren** en selecteer **Eigenschappen**. Klik op **Deze beleidsinstellingen vastleggen**, schakel het selectievakje **Geslaagd** in, schakel het selectievakje **Fout** in, klik op **Toepassen** en vervolgens op **OK**.

7. Klik in het detailvenster met de rechtermuisknop op **Directoryservicetoegang controleren** en selecteer **Eigenschappen**. Klik op **Deze beleidsinstellingen vastleggen**, schakel het selectievakje **Geslaagd** in, schakel het selectievakje **Fout** in, klik op **Toepassen** en vervolgens op **OK**.

8. Navigeer naar **Computerconfiguratie** > **Beleidsregels** > **Windows-instellingen** > **Beveiligingsinstellingen** > **Beleidsregels van account** > **Kerberos-beleid**.

9. Klik in het detailvenster met de rechtermuisknop op **Maximale levensduur van gebruikersticket** en selecteer **Eigenschappen**. Klik op **Deze beleidsinstellingen vastleggen**, stel het aantal uren in op *1*, klik op **Toepassen** en vervolgens op **OK**. Houd er rekening mee dat andere instellingen in het venster ook worden gewijzigd.

10. Selecteer in het venster Groepsbeleidsbeheer **Standaard domeinbeleid**, klik er met de rechtermuisknop op en selecteer **Bewerken**.

11. Vouw **Computerconfiguratie** > **Beleidsregels** > **Windows-instellingen** > **Beveiligingsinstellingen** > **Lokale beleidsregels** uit en selecteer **Toewijzing van gebruiksrechten**.

12. Klik in het detailvenster met de rechtermuisknop op **Aanmelden als batchtaak weigeren** en selecteer **Eigenschappen**.

13. Schakel het selectievakje **Deze beleidsinstellingen definiëren** in, klik op **Gebruiker of groep toevoegen**, en typ in het veld Gebruikers en groepsnamen *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* en klik op **OK**.

14. Klik op **OK** om het venster te sluiten.

15. Klik in het detailvenster met de rechtermuisknop op **Aanmelden via Extern bureaublad-services weigeren** en selecteer **Eigenschappen**.

16. Klik op het selectievakje **Deze beleidsinstellingen definiëren**, klik op **Gebruiker of groep toevoegen**, en typ in het veld Gebruikers en groepsnamen *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* en klik op **OK**.

17. Klik op **OK** om het venster te sluiten.

18. Sluit het venster Groepsbeleidsbeheer-editor en het venster Groepsbeleidsbeheer.

19. Start als beheerder een PowerShell-venster en typ de volgende opdracht om de DC bij te werken met de groepsbeleidsinstellingen.

  ```
  gpupdate /force /target:computer
  ```

  Na een minuut wordt de taak voltooid met het bericht 'Het bijwerken van het computerbeleid is voltooid'.


### <a name="configure-dns-name-forwarding-on-privdc"></a>Doorsturen van de DNS-naam op PRIVDC configureren

Configureer met PowerShell op PRIVDC het doorsturen van de DNS-naam zodat het PRIV-domein andere bestaande forests kan herkennen.

1. Start PowerShell.

2. Typ voor elk domein bovenaan elk bestaand forest de volgende opdracht, waarmee u het bestaande DNS-domein (bijvoorbeeld contoso.local) en het IP-adres van de hoofdserver van dat domein opgeeft.  

  Als u in de vorige stap één domein contoso.local hebt gemaakt, geeft u vervolgens *10.1.1.31* op als het IP-adres van het virtuele netwerk van de CORPDC-computer.

  ```
  Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
  ```

> [!NOTE]
> De andere forests moeten ook in staat zijn om DNS-query's te routeren voor het PRIV-forest naar deze domeincontroller.  Als er meerdere bestaande Active Directory-forests zijn, moet u ook een voorwaardelijke DNS-doorstuurserver toevoegen aan elk van deze forests.

### <a name="configure-kerberos"></a>Kerberos configureren

1. Voeg met behulp van PowerShell SPN's toe zodat SharePoint, PAM REST-API en de MIM-service Kerberos-verificatie kunnen gebruiken.

  ```
  setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
  setspn -S http/pamsrv PRIV\SharePoint
  setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
  setspn -S FIMService/pamsrv PRIV\MIMService
  ```

> [!NOTE]
> In de volgende stappen van dit document wordt beschreven hoe u MIM 2016-serveronderdelen op één computer installeert. Als u van plan bent een andere server toe te voegen voor maximale beschikbaarheid, moet u Kerberos extra configureren zoals beschreven in [FIM 2010: Kerberos Authentication Setup](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx) (FIM 2010: Kerberos-verificatie instellen).

### <a name="configure-delegation-to-give-mim-service-accounts-access"></a>Delegatie configureren om MIM-serviceaccounts toegang te geven

Voer de volgende stappen uit op PRIVDC als een domeinbeheerder.

1. Start **Active Directory: gebruikers en computers**.  
2. Klik met de rechtermuisknop op het domein **priv.contoso.local** en selecteer **Beheer delegeren**.  
3. Klik op het tabblad Geselecteerde gebruikers en groepen op **Toevoegen**.  
4. Typ in het venster Gebruikers, computers, of groepen selecteren *mimcomponent; mimmonitor; mimservice* en klik op **Namen controleren**. Nadat de namen zijn onderstreept, klikt u op **OK** en vervolgens op **Volgende**.  
5. Selecteer in de lijst met algemene taken **Gebruikersaccounts maken, verwijderen en beheren** en **Lidmaatschap van een groep wijzigen**, klik vervolgens op **Volgende** en **Voltooien**.

6. Klik nogmaals met de rechtermuisknop op het domein **priv.contoso.local** en selecteer **Beheer delegeren**.  
7. Klik op het tabblad Geselecteerde gebruikers en groepen op **Toevoegen**.  
8. Voer in het venster Gebruikers, computers, of groepen selecteren *MIMAdmin* in en klik op **Namen controleren**. Nadat de namen zijn onderstreept, klikt u op **OK** en vervolgens op **Volgende**.  
9. Selecteer **Aangepaste taak**, van toepassing op **Deze map**, met **Algemene machtigingen**.    
10. Selecteer het volgende in de lijst met bevoegdheden:  
  - **Lezen**  
  - **Schrijven**  
  - **Alle onderliggende objecten maken**  
  - **Alle onderliggende objecten verwijderen**  
  - **Alle eigenschappen lezen**  
  - **Alle eigenschappen schrijven**  
  - **SID-geschiedenis migreren**  
  Klik op **Volgende** en vervolgens op **Voltooien**.

11. Klik nog één keer met de rechtermuisknop op het domein **priv.contoso.local** en selecteer **Beheer delegeren**.  
12. Klik op het tabblad Geselecteerde gebruikers en groepen op **Toevoegen**.  
13. Voer in het venster Gebruikers, computers, of groepen selecteren *MIMAdmin* in en klik vervolgens op **Namen controleren**. Nadat de namen zijn onderstreept, klikt u op **OK** en vervolgens op **Volgende**.  
14. Selecteer **Aangepaste taak**, van toepassing op **Deze map**, klik vervolgens op **Gebruikersobjecten**.    
15. Selecteer in de lijst met machtigingen **Wachtwoord wijzigen** en **Wachtwoord opnieuw instellen**. Klik vervolgens op **Volgende** en daarna op **Voltooien**.  
16. Sluit Active Directory - gebruikers en computers.

17.    Open een opdrachtprompt.  
18.    Controleer de toegangsbeheerlijst van het object Admin SD-houder in de PRIV-domeinen. Bijvoorbeeld, als uw domein 'priv.contoso.local' is, typt u de opdracht  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"
  ```
19.    Werk de toegangsbeheerlijst indien nodig bij om ervoor te zorgen dat MIM-service en MIM-onderdeelservice lidmaatschappen van groepen kunnen bijwerken die zijn beveiligd door deze ACL.  Typ de opdracht:  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"
  ```
20. Start de PRIVDC-server opnieuw op zodat deze wijzigingen van kracht worden.

## <a name="prepare-a-priv-workstation"></a>Een PRIV-werkstation voorbereiden

Als u nog geen werkstationcomputer hebt die lid wordt van het PRIV-domein voor het uitvoeren van onderhoud van PRIV-resources (zoals MIM), volgt u deze instructies voor het voorbereiden van een werkstation.  

### <a name="install-windows-81-or-windows-10-enterprise"></a>Installeer Windows 8.1 of Windows 10 Enterprise

Installeer Windows 8.1 Enterprise of Windows 10 Enterprise op een andere nieuwe virtuele machine waarop geen software is geïnstalleerd om een computer *'PRIVWKSTN'* te maken.

1. Gebruik Snelle instellingen tijdens de installatie.

2. Houd er rekening mee dat de installatie wellicht geen verbinding kan maken met het internet. Klik op **Een lokaal account maken**. Geef een andere gebruikersnaam op. Gebruik niet 'Administrator' of 'Jen'.

3. Geef deze computer via Configuratiescherm een statisch IP-adres op het virtuele netwerk en stel de voorkeurs-DNS-server van de interface in op de voorkeursserver van de PRIVDC-server.

4. Stel via Configuratiescherm de domeindeelname van de computer PRIVWKSTN in op het domein priv.contoso.local. Hiervoor beheerdersreferenties van het PRIV-domein vereist. Wanneer dit is voltooid, start u de computer PRIVWKSTN opnieuw op.

Zie voor meer informatie [securing privileged access workstations](https://technet.microsoft.com/en-us/library/mt634654.aspx) (bevoegde toegang tot werkstations beveiligen).

In de volgende stap bereidt u een PAM-server voor.

>[!div class="step-by-step"]
[« Stap 1](step-1-prepare-corp-domain.md)
[Stap 3 »](step-3-prepare-pam-server.md)

