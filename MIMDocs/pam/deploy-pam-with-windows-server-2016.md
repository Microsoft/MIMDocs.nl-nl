---
title: MIM Privileged Access Management implementeren met Windows Server 2016 | Microsoft Docs
description: Meer informatie over de implementatie van Privileged Access Management met Server 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: c02904d7acb5c56e8b1e7f7a267b8d54c0a58d7a
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010554"
---
# <a name="deploy-mim-pam-with-windows-server-2016"></a>MIM PAM implementeren met Windows Server 2016


Dit scenario maakt MIM 2016 SP2 mogelijk voor het PAM-scenario met behulp van de functies van Windows Server 2016 of hoger als de domein controller voor het "PRIV"-forest.  Als dit scenario is geconfigureerd, wordt voor een Kerberos-ticket van een gebruiker een tijdslimiet ingesteld voor de resterende tijd van de activeringen van de rol.

## <a name="preparation"></a>Voorbereiding

Er zijn minimaal twee virtuele machines nodig voor de testomgeving:

-   VM fungeert als host voor de PRIV-domein controller, waarop Windows Server 2016 of hoger wordt uitgevoerd.

-   VM fungeert als host van de MIM-service, met Windows Server 2016 of hoger (aanbevolen) of Windows Server 2012 R2

> [!NOTE]
> Als u nog geen CORP-domein in uw testomgeving hebt, is een extra domeincontroller voor dit domein vereist. U kunt voor de CORP-domeincontroller met Windows Server 2016 of Windows Server 2012 R2 gebruiken.


Voer de installatie uit, zoals beschreven in de [introductiehandleiding](privileged-identity-management-for-active-directory-domain-services.md), **behalve in de hieronder beschreven gevallen**:

- Als u een nieuw CORP-domein maakt aan de hand van de instructies in [Stap 1: de CORP-domeincontroller voorbereiden](step-1-prepare-corp-domain.md), kunt u desgewenst het CORP-domein zo configureren dat het zich op het functionele niveau van Windows Server 2016 bevindt. **Als u deze optie kiest, moet u de volgende aanpassingen doen**:

  - Als u Windows Server 2016-media gebruikt, heet de installatieoptie Windows Server 2016 (Server met Bureaubladervaring).

  - U kunt het functionele niveau van Windows Server 2016 opgeven voor het CORP-forest en -domein door 7 op te geven als het domein en versienummer van het forest in het argument voor de opdracht Install-ADDSForest, zoals hieronder is aangegeven:
    ```
    Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
    ```
  - De laatste opdracht (New-ADGroup -name 'CONTOSO\$\$\$' …) is **niet vereist in Groepen en gebruikers maken als zowel de CORP- als PRIV-domeincontrollers zich op het functionele domeinniveau van Windows Server 2016 bevinden**.

  - De wijzigingen die zijn beschreven in 'Controle configureren' (item 8) en 'Registerinstellingen configureren' (item 10) zijn **aanbevolen maar niet vereist** als zowel CORP- als PRIV-domeincontrollers zich op het functionele domeinniveau van Windows Server 2016 bevinden.

- Als u Windows Server 2012 R2 gebruikt als het besturingssysteem voor CORPDC, moet u de hotfixes 2919442 en 2919355 installeren [en 3155495 bijwerken](https://support.microsoft.com/kb/3156418) op CORPDC.

- Volg de instructies in [Stap 2: PRIV-domeincontroller voorbereiden](step-2-prepare-priv-domain-controller.md), met uitzondering van de volgende aanpassingen:

  -   Installeren met behulp van Windows Server 2016-media. De installatieoptie heet Windows Server 2016 (Server met Bureaubladervaring).

  -   In de instructies voor het toevoegen van (item 4)**moet u de versienummers voor het domein en forest op de vierde regel van de PowerShell-opdrachten instellen op 7** om de Windows Server AD-functies, zoals verderop beschreven, in te schakelen.

      ```
      Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
      ```  

  -   Bij de configuratie van de rechten voor controle en de aanmelding moet u er rekening mee houden dat het programma Groepsbeleidsbeheer zich in de map Windows Systeembeheer bevindt.

  -   Registerinstellingen voor de migratie van de SID-geschiedenis configureren (item 8) is **niet vereist als het PRIV-domein zich op het functionele domeinniveau van Windows Server 2016 bevindt**.

  -   Na de configuratie van de delegatie en voordat de server opnieuw wordt opgestart, moet u de PAM-functies in Windows Server 2016 Active Directory inschakelen door een PowerShell-venster te starten als beheerder en de volgende opdrachten te typen.

  ```
  $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
  Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
  ```

  - Na de configuratie van de delegatie en voordat de server opnieuw wordt opgestart, moet u de MIM-beheerders en het MIM-serviceaccount toestaan om schaduwprincipals te maken en bij te werken.

    a. Start een Power shell-venster en typ ADSIEdit.

    b. Klik in het menu Acties op Verbinding maken met. Wijzig bij de instelling voor het verbindingspunt de naamgevingscontext van Standaardnaamgevingscontext in Configuratie en klik op OK.

    c. Nadat u verbinding hebt gemaakt, vouwt u links in het venster onder ADSI bewerken het knooppunt Configuratie uit om CN=Configuration, DC=priv,.... weer te geven. Vouw CN=Configuration uit en vouw vervolgens CN=Services uit.

    d. Klik met de rechtermuisknop op CN=Shadow Principal Configuration en klik op Eigenschappen. Wanneer het dialoogvenster Eigenschappen wordt weergegeven, gaat u naar het tabblad Beveiliging.

    e. Klik op Toevoegen. Geef de accounts voor de MIM-service op en alle andere MIM-beheerders die later New-PAMGroup uitvoeren om extra PAM-groepen te maken. Voeg Schrijven, Alle onderliggende objecten maken en Alle onderliggende objecten verwijderen toe voor elke gebruiker in de lijst met toegestane machtigingen. Voeg de machtigingen toe.

    f. Ga naar Geavanceerde beveiligingsinstellingen. Klik op de regel voor toegang tot de MIM-service op Bewerken. Wijzig de instelling 'Is van toepassing op' in 'Voor dit object en alle onderliggende objecten'. Werk deze machtigingsinstelling bij en sluit het dialoogvenster Beveiliging.

    bijvoorbeeld Sluit ADSI bewerken.

  - Na de configuratie van de delegatie en voordat de server opnieuw wordt opgestart, moet u de MIM-beheerders toestaan om verificatiebeleid te maken en bij te werken.

    a.  Start een **opdrachtprompt** met verhoogde bevoegdheid en typ de volgende opdrachten, waarbij u de naam van het MIM-beheerdersaccount in de volgende vier regels vervangt door 'mimadmin':
    ```
    dsacls "CN=AuthN Policies,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

    dsacls "CN=AuthN Policies,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:CCDC;msDS-AuthNPolicy

    dsacls "CN=AuthN Silos,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

    dsacls "CN=AuthN Silos,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


- Volg de instructies in [Stap 3: een PAM-server voorbereiden](step-3-prepare-pam-server.md), met de volgende aanpassingen.

  -   Als u op Windows Server 2016 installeert, moet u er rekening mee houden dat de rol ApplicationServer niet beschikbaar is.

  -   Als u MIM op Windows Server 2016 installeert, **kunt u SharePoint 2013 niet installeren**.

- Volg de instructies in [Stap 4: MIM-onderdelen installeren op een PAM-server en -werkstation](step-4-install-mim-components-on-pam-server.md), met de volgende aanpassingen.

  -   De gebruiker die de MIM-service en PAM-onderdelen installeert, **moet schrijftoegang hebben tot het PRIV-domein in AD**, aangezien met de MIM-installatie een nieuwe organisatie-eenheid van AD, genaamd PAM-objecten, wordt gemaakt.

  -   Als SharePoint niet is geïnstalleerd, moet u de MIM-portal niet installeren.

- Volg de instructies in [Stap 5: een vertrouwensrelatie instellen](step-5-establish-trust-between-priv-corp-forests.md) met de volgende aanpassingen:

  - Bij het tot stand brengen van een eenrichtings vertrouwensrelatie voert u alleen de eerste twee Power shell-opdrachten uit (Get-Credential en New-PAMTrust), voert **u de opdracht New-PAMDomainConfiguration niet uit**.

  - Nadat u de vertrouwensrelatie hebt ingesteld, meldt u zich aan bij PRIVDC als PRIV\\-beheerder, start u PowerShell en typt u de volgende opdrachten:
    ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
    /usero:contoso\administrator /passwordo:Pass@word1

    netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
    /usero:contoso\administrator /passwordo:Pass@word1  

    netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
    /usero:contoso\administrator /passwordo:Pass@word1
    ```

- Item 5 (verificatie van vertrouwensrelatie) is **niet vereist als zowel CORP- als PRIV-domein zich op het functionele domeinniveau van Windows Server 2016 bevinden**.

## <a name="more-information"></a>Meer informatie

- [Privileged Access Management voor Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md)
- [De MIM-omgeving voor Privileged Access Management configureren](configuring-mim-environment-for-pam.md)
- [PAM configureren met behulp van scripts](sp1-pam-configure-using-scripts.md)
