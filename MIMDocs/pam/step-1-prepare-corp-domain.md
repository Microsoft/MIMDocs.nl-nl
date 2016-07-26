---
title: PAM implementeren - Stap 1 - CORP-domein | Microsoft Identity Manager
description: Het CORP-domein voorbereiden met bestaande of nieuwe identiteiten die worden beheerd door Privileged Identity Manager
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 9a2fafa86c5c928339ff8d7ad1593472046ccb98


---

# Stap 1: de host en het domein CORP voorbereiden

>[!div class="step-by-step"]
[Stap 2 »](step-2-prepare-priv-domain-controller.md)


In deze stap bereidt u het hosten van de bastionomgeving voor. Indien nodig, maakt u ook een domeincontroller en een lidwerkstation in een nieuw domein en forest (het *CORP*forest) met identiteiten die worden beheerd door de bastionomgeving. Dit CORP-forest komt overeen met een bestaand forest dat resources heeft om te worden beheerd. Dit document bevat een voorbeeldresource die moet worden beveiligd; een bestandsshare.

Als u al een bestaand Active Directory-domein (AD) met een domeincontroller met Windows Server 2012 R2 of hoger hebt waarvan u een domeinbeheerder bent, kunt u dat domein gebruiken.  

## De CORP-domeincontroller voorbereiden

In dit gedeelte wordt beschreven hoe u een domeincontroller voor een CORP-domein instelt. In het CORP-domein worden de gebruikers met beheerdersrechten beheerd door de bastionomgeving. De naam van de Domain Name System (DNS) van het CORP-domein dat in dit voorbeeld wordt gebruikt is *contoso.local*.

### Windows Server installeren

Installeer Windows Server 2012 R2 of Windows Server 2016 Technical Preview 4 of hoger op een virtuele machine om een computer te maken met de naam *CORPDC*.

1. Kies **Windows Server 2012 R2 Standard (server met een GUI) x64** of **Windows Server 2016 Technical Preview (server met bureaubladbelevenis)**.

2. Lees en accepteer de licentievoorwaarden.

3. Omdat de schijf leeg is, selecteert u **Aangepast: alleen Windows installeren** en gebruikt u de niet-geïnitialiseerde schijfruimte.

4. Meld u als beheerder aan bij deze nieuwe computer. Ga naar Configuratiescherm. Stel de naam van de computer in op *CORPDC*, en wijs hieraan een statisch IP-adres op het virtuele netwerk toe. Start de server opnieuw op.

5. Nadat de server opnieuw is opgestart, moet u zich aanmelden als een beheerder. Ga naar Configuratiescherm. Configureer de computer om te controleren op updates en installeer alle vereiste updates. Start de server opnieuw op.

### Functies toevoegen voor het maken van een domeincontroller

In dit gedeelte voegt u de functies Active Directory Domain Services (AD DS), DNS-server en bestandsserver (onderdeel van het gedeelte bestands- en opslagservices) toe en verhoogt u deze server naar een domeincontroller in een nieuw forest contoso.local.

> [!NOTE]  
> Als u al een domein hebt dat u kunt gebruiken als uw CORP-domein en dat domein maakt gebruik van Windows Server 2012 R2 of hoger als de domeincontroller, kunt u doorgaan met [Extra gebruikers en groepen maken voor demonstratiedoeleinden](#create-additional-users-and-groups-for-demonstration-purposes).

1. Start PowerShell terwijl u aangemeld bent als beheerder.

2. Typ de volgende opdrachten:

  ```
  import-module ServerManager

  Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

  Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
  ```

  Hierdoor wordt u gevraagd een beheerderswachtwoord voor de veilige modus op te geven. Houd er rekening mee dat waarschuwingsberichten voor DNS-delegatie- en cryptografie-instellingen worden weergegeven. Dit is normaal.

3. Meld u af nadat het maken van het forest voltooid is. De server wordt automatisch opnieuw opgestart.

4. Wanneer de server opnieuw is opgestart, meldt u zich aan bij CORPDC als beheerder van het domein. Dit is doorgaans de gebruiker CONTOSO\\-beheerder, die het wachtwoord heeft dat is gemaakt bij het installeren van Windows op CORPDC.

### Een groep maken

Een groep maken voor controledoeleinden door Active Directory, als deze groep nog niet bestaat. De naam van de groep moet de NetBIOS-domeinnaam zijn, gevolgd door drie dollartekens, bijvoorbeeld *CONTOSO$$$*.

Meld u bij elk domein aan bij een domeincontroller als een domeinbeheerder en voer de volgende stappen uit:

1. Start PowerShell.

2. Typ de volgende opdrachten, maar vervang 'CONTOSO' door de NetBIOS-naam van uw domein.

  ```
  import-module activedirectory

  New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
  ```

In sommige gevallen kan deze groep al bestaan. Dit normaal als het domein ook in AD-migratiescenario's is gebruikt.

### Extra gebruikers en groepen maken voor demonstratiedoeleinden

Als u een nieuw CORP-domein hebt gemaakt, moet u vervolgens extra gebruikers en groepen maken om het PAM-scenario te kunnen demonstreren. De gebruiker en groep voor demonstratiedoeleinden mogen geen domeinbeheerders zijn of worden beheerd door de instellingen van de adminSDHolder in AD.

> [!NOTE]
> Als u al een domein hebt dat u als het CORP-domein wilt gebruiken en deze bevat een gebruiker en een groep die u voor demonstratiedoeleinden kunt gebruiken, dan kunt u het gedeelte [Controle configureren](#configure-auditing) overslaan.

Maak een beveiligingsgroep met de naam *CorpAdmins* en een gebruiker met de naam *Jen*. U kunt andere namen gebruiken als u wenst.

1. Start PowerShell.

2. Typ de volgende opdrachten: Vervang het wachtwoord 'Pass@word1' door een andere wachtwoordtekenreeks.

  ```
  import-module activedirectory

  New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

  New-ADUser –SamAccountName Jen –name Jen

  Add-ADGroupMember –identity CorpAdmins –Members Jen

  $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  Set-ADAccountPassword –identity Jen –NewPassword $jp

  Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
  ```

### Controle configureren

U moet controle van bestaande forests inschakelen om de PAM-configuratie op die forests tot stand te brengen.  

Meld u bij elk domein aan bij een domeincontroller als een domeinbeheerder en voer de volgende stappen uit:

1. Ga naar **Start** > **Systeembeheer** (of op Windows Server 2016 **Windows Systeembeheer**), en start **Groepsbeleidsbeheer**.

2. Navigeer naar het beleid voor domeincontrollers voor dit domein.  Als u een nieuw domein hebt gemaakt voor contoso.local, navigeert u naar **Forest: priv.contoso.local** > **Domeinen** > **priv.contoso.local** > **Domeincontrollers** > **Standaardbeleid voor domeincontrollers**. Een informatief bericht wordt weergegeven.

3. Klik met de rechtermuisknop op **Standaardbeleid voor domeincontrollers** en selecteer **Bewerken**. Een nieuw venster wordt weergegeven.

4. Navigeer in het venster Groepsbeleidsbeheer-editor, onder de structuur Standaardbeleid voor domeincontrollers naar **Computerconfiguratie** > **Beleidsregels** > **Windows-instellingen** > **Beveiligingsinstellingen** > **Lokale beleidsregels** > **Controlebeleid**.

5. Klik in het detailvenster met de rechtermuisknop op **Accountbeheer controleren** en selecteer **Eigenschappen**. Selecteer **Deze beleidsinstellingen vastleggen**, schakel achtereenvolgens de selectievakjes **Geslaagd** en **Mislukt** in, klik op **Toepassen** en tenslotte op **OK**.

6. Klik in het detailvenster met de rechtermuisknop op **Directoryservicetoegang controleren** en selecteer **Eigenschappen**. Selecteer **Deze beleidsinstellingen vastleggen**, schakel achtereenvolgens de selectievakjes **Geslaagd** en **Mislukt** in, klik op **Toepassen** en tenslotte op **OK**.

7. Sluit het venster Groepsbeleidsbeheer-editor en het venster Groepsbeleidsbeheer.

8. Pas de controle-instellingen toe door een PowerShell-venster te starten en het volgende te typen:

  ```
  gpupdate /force /target:computer
  ```

Het bericht **Het bijwerken van het computerbeleid is voltooid** zou na een paar minuten moeten worden weergegeven.

### Registerinstellingen configureren

In dit gedeelte configureert u de registerinstellingen die nodig zijn voor sIDHistory-migratie, die wordt gebruikt voor het maken van de Privileged Access Management-groep.

1. Start PowerShell.

2. Typ de volgende opdrachten voor het configureren van het brondomein om externe procedureaanroep (Remote Procedure Call, RPC) toegang te verlenen tot de database voor beveiligingsaccountbeheer (Security Accounts Manager, SAM).

  ```
  New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

  Restart-Computer
  ```

Hierdoor wordt de domeincontroller opnieuw gestart. Zie voor meer informatie over deze registerinstelling [How to troubleshoot inter-forest sIDHistory migration with ADMTv2](http://support.microsoft.com/kb/322970) (Problemen oplossen met sIDHistory-migratie met ADMTv2 tussen forests).

## Een CORP-werkstation en -resource voorbereiden

Als u nog geen werkstationcomputer hebt toegevoegd aan het domein, volgt u deze instructies om een werkstationcomputer voor te bereiden.  

> [!NOTE]
> Als u al een werkstation hebt toegevoegd aan het domein, gaat u naar [Een resource maken voor demonstratiedoeleinden](#create-a-resource-for-demonstration-purposes).

### Windows 8.1 of Windows 10 Enterprise als een VM installeren

Installeer Windows 8.1 Enterprise of Windows 10 Enterprise op een andere nieuwe virtuele machine waarop geen software is geïnstalleerd om een computer *CORPWKSTN* te maken.

1. Gebruik Snelle instellingen tijdens de installatie.

2. Houd er rekening mee dat de installatie wellicht geen verbinding kan maken met het internet. Selecteer **Een lokaal account maken**. Geef een andere gebruikersnaam op. Gebruik niet 'Administrator' of 'Jen'.

3. Geef deze computer via Configuratiescherm een statisch IP-adres op het virtuele netwerk en stel de voorkeurs-DNS-server van de interface in op de voorkeursserver van de CORPDC-server.

4. Stel via Configuratiescherm de domeindeelname van de computer CORPWKSTN in op het domein contoso.local. U moet de beheerdersreferenties voor het Contoso-domein opgeven. Wanneer dit is voltooid, start u de computer CORPWKSTN opnieuw op.

### Een resource voor demonstratiedoeleinden maken

U hebt een resource nodig om het toegangsbeheer op basis van een beveiligingsgroep met PAM te demonstreren.  Als u nog geen resource hebt, kunt u een bestandsmap gebruiken voor de demonstratie.  Dit maakt gebruik van de AD-objecten 'Jen' en 'CorpAdmins' die u hebt gemaakt in het domein contoso.local.

1. Maak verbinding met het werkstation CORPWKSTN. Klik op het pictogram **Gebruiker wisselen** en vervolgens op **Andere gebruiker**. Zorg ervoor dat de gebruiker CONTOSO\\Jen zich kan aanmelden bij CORPWKSTN.

2. Maak een nieuwe map met de naam *CorpFS* en deel deze met de groep *CorpAdmins*.

3. Open PowerShell als beheerder.

4. Typ de volgende opdrachten:

  ```
  mkdir c:\corpfs

  New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

  $acl = Get-Acl c:\corpfs

  $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

  $acl.SetAccessRule($car)

  Set-Acl c:\corpfs $acl
  ```

In de volgende stap bereidt de PRIV-domeincontroller voor.

>[!div class="step-by-step"]
[Stap 2 »](step-2-prepare-priv-domain-controller.md)



<!--HONumber=Jul16_HO3-->


