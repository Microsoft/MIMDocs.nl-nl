---
title: PAM implementeren - Stap 5 - Forest-koppeling | Microsoft Docs
description: Vertrouwensrelatie tussen de PRIV- en CORP-forests instellen zodat bevoegde gebruikers in PRIV nog steeds toegang hebben tot resources in CORP.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1239ca2c0c6d376420723da01d7aa42821f5980f
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# Stap 5 –Een vertrouwensrelatie tussen het PRIV- en CORP-forest instellen
<a id="step-5--establish-trust-between-priv-and-corp-forests" class="xliff"></a>

>[!div class="step-by-step"]
[« Stap 4](step-4-install-mim-components-on-pam-server.md)
[Stap 6 »](step-6-transition-group-to-pam.md)


Voor elk CORP-domein, zoals contoso.local, moet voor de PRIV- en CONTOSO-domeincontrollers een vertrouwensrelatie zijn ingesteld. Hiermee hebben gebruikers in het PRIV-domein toegang tot resources op het CORP-domein.

## Elke domeincontroller verbinden met het bijbehorende equivalent
<a id="connect-each-domain-controller-to-its-counterpart" class="xliff"></a>

Voordat een vertrouwensrelatie tot stand kan worden gebracht, moet elke domeincontroller worden geconfigureerd voor DNS-naamomzetting voor het bijbehorende equivalent op basis van het IP-adres van de domeincontroller/DNS-server.

1.  Als de domeincontrollers of de server met de MIM-software worden geïmplementeerd als virtuele machines, moet u ervoor zorgen dat er geen andere DNS-servers aanwezig zijn die domeinnaamgevingsservices voor deze computers leveren.
    - Als de virtuele machines over meerdere netwerkinterfaces beschikken, waaronder netwerkinterfaces die zijn verbonden met een openbaar netwerk, moet u dergelijke verbindingen mogelijk tijdelijk uitschakelen of de netwerkinterface-instellingen van Windows overschrijven. Het is belangrijk dat u ervoor zorgt dat een met DHCP opgegeven DNS-serveradres niet wordt gebruikt door virtuele machines.

2.  Controleer of met elke bestaande CORP-domeincontroller namen naar het PRIV-forest kunnen worden omgeleid. Start PowerShell op elke domeincontroller buiten het PRIV-forest, zoals CORPDC, en typ de volgende opdracht:

    ```
    nslookup -qt=ns priv.contoso.local.
    ```
    Controleer of de uitvoer een naamserverrecord voor het PRIV-domein met de juiste IP-adres bevat.

3.  Als er met de domeincontroller niet kan worden omgeleid naar het PRIV-domein, gebruikt u **DNS-beheer** (in **Start** > **Hulpprogramma's voor apps** > **DNS**) om voor het PRIV-domein het doorsturen van DNS-namen in te stellen op het IP-adres van PRIVDC. Als het domein een bovenliggend domein is (bijvoorbeeld contoso.local), vouwt u de knooppunten voor deze domeincontroller en het bijbehorende domein uit, zoals **CORPDC** > **Zones voor forward lookup** > **contoso.local**, en controleert u of een sleutel met de naam **priv** aanwezig is als type voor de naamserver (NS).

    ![bestandsstructuur voor priv-sleutel - schermafbeelding](./media/PAM_GS_DNS_Manager.png)

## Vertrouwensrelatie op PAMSRV instellen
<a id="establish-trust-on-pamsrv" class="xliff"></a>

Op PAMSRV moet u een eenzijdige vertrouwensrelatie met elk domein, zoals CORPDC, instellen zodat de CORP-domeincontrollers het PRIV-forest vertrouwen.

1. Meld u aan bij PAMSRV als een PRIV-domeinbeheerder (PRIV\Administrator).

2.  Start PowerShell.

3.  Typ de volgende PowerShell-opdrachten voor elk bestaand forest. Voer de referentie voor de CORP-domeinbeheerder in (CONTOSO\Administrator) als u hierom wordt gevraagd.

    ```
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  Typ de volgende PowerShell-opdrachten voor elk domein in de bestaande forests. Voer de referentie voor de CORP-domeinbeheerder in (CONTOSO\Administrator) als u hierom wordt gevraagd.

    ```
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## Forests leestoegang tot Active Directory geven
<a id="give-forests-read-access-to-active-directory" class="xliff"></a>

Voor elk bestaand forest moet u voor PRIV-beheerders en de controleservice leestoegang tot AD inschakelen.

1.  Meld u aan bij de bestaande domeincontroller van het CORP-forest (CORPDC) als een domeinbeheerder voor het topleveldomein in het forest (Contoso\Administrator).  
2.  Start **Active Directory: gebruikers en computers**.  
3.  Klik met de rechtermuisknop op het domein **contoso.local** en selecteer **Beheer delegeren**.  
4.  Klik op het tabblad Geselecteerde gebruikers en groepen op **Toevoegen**.  
5.  Klik in het venster Gebruikers, computers of groepen selecteren op **Locaties** en wijzig de locatie in *priv.contoso.local*.  Typ op de objectnaam *Domeinbeheerders* en klik op **Namen controleren**. Wanneer een pop-up wordt weergegeven, voert u de gebruikersnaam *priv\administrator* en het bijbehorende wachtwoord in.  
6.  Voeg na Domeinbeheerders *; MIMMonitor* toe. Nadat de namen **Domeinbeheerders** en **MIMMonitor** zijn onderstreept, klikt u op **OK** en vervolgens op **Volgende**.  
7.  Selecteer in de lijst met algemene taken **Alle gebruikersgegevens lezen** en klik vervolgens op **Volgende** en **Voltooien**.  
8.  Sluit Active Directory - gebruikers en computers.

9.  Open een PowerShell-venster.  
10.  Gebruik `netdom` om ervoor te zorgen dat SID-geschiedenis is ingeschakeld en SID-filtering is uitgeschakeld. Type:  
    ```
    netdom trust contoso.local /quarantine /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    In de uitvoer met **SID-geschiedenis wordt ingeschakeld voor deze vertrouwensrelatie** of **SID-geschiedenis is al ingeschakeld voor deze vertrouwensrelatie** worden weergegeven.

    Ook moet **Filteren op SID's is niet ingeschakeld voor deze vertrouwensrelatie** worden weergegeven in de uitvoer. Zie [Disable SID filter quarantining](http://technet.microsoft.com/library/cc772816.aspx) (In quarantaine plaatsen voor SID-filters uitschakelen) voor meer informatie.

## De services voor controle en onderdelen starten
<a id="start-the-monitoring-and-component-services" class="xliff"></a>

1.  Meld u aan bij PAMSRV als een PRIV-domeinbeheerder (PRIV\Administrator).

2.  Start PowerShell.

3.  Typ de volgende PowerShell-opdrachten.

    ```
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

In de volgende stap gaat u een groep naar PAM verplaatsen.

>[!div class="step-by-step"]
[« Stap 4](step-4-install-mim-components-on-pam-server.md)
[Stap 6 »](step-6-transition-group-to-pam.md)
