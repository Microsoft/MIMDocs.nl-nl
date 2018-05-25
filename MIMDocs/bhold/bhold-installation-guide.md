---
title: BHOLD-SP1-installatie | Microsoft Docs
description: BHOLD-SP1-documentatie voor installatie
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: e0514530c9bceef18cc8eea7ec8b7060110811c2
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/25/2018
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Installatiehandleiding voor Microsoft BHOLD-Suite SP1 (6.0)

Microsoft® BHOLD-Suite met Service Pack 1 (SP1) is een verzameling van toepassingen die, wanneer gebruikt met Microsoft Identity Manager 2016 SP1 (MIM), voegt effectieve rollenbeheer, analytics en attestation naar MIM. BHOLD-Suite van Microsoft SP1 bestaat uit de volgende modules:

- BHOLD-Core
- Access Management-Connector
- BHOLD FIM/MIM-integratie
- BHOLD Model Generator
- BHOLD-analyse
- BHOLD-rapportage
- BHOLD Attestation


>[!NOTE]
**Van toepassing op**: Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>Wat dit document bevat informatie over

Dit document wordt uitgelegd hoe uw implementatie van BHOLD om te voldoen aan uw bedrijfsbehoeften en elke BHOLD-module installeren plannen. Voor elke module, relevante hardware, infrastructuur en softwarevereisten, voorinstallatie netwerkconfiguratie, informatie vereist tijdens de installatie en postinstallation stappen, indien aanwezig, worden beschreven.

## <a name="pre-requisite-knowledge"></a>Vereiste kennis

Dit document wordt ervan uitgegaan dat u basiskennis van het installeren van software op server-computers. Ook wordt ervan uitgegaan dat u basiskennis van Active Directory® Domain Services, Microsoft Identity Manager SP1 (FIM) en Microsoft SQL Server 2008-database-software hebben. Een beschrijving van het instellen en configureren van de afhankelijke technologieën zoals AD DS en FIM valt buiten het bereik van deze documentatie. Zie voor meer informatie over de functies die de Microsoft BHOLD-modules uitvoeren [de handleiding voor Microsoft BHOLD-suite concepten](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Doelgroep

Dit document is bedoeld voor IT-planners, systeemarchitecten technologie besluitvormers, adviseurs, infrastructuur planners en IT-medewerkers die plan de implementatie van BHOLD-Suite van Microsoft.

## <a name="bhold-infrastructure-considerations"></a>BHOLD-infrastructuur overwegingen

De BHOLD en FIM worden meestal gebruikt in een grote infrastructuur-omgeving. U kunt uw architectuur BHOLD en FIM om te voldoen aan de behoeften van uw bedrijf aanpassen. De volgende secties vindt enkele mogelijke architectuur oplossingen. Dit overzicht is niet een uitgebreide lijst met alle mogelijke opties maar suggesties u BHOLD in uw netwerk kunt implementeren.
 
Deze sectie bevat de volgende onderwerpen:

- Architectuur van één server
- Dual-server-architectuur
- architectuur van de twee lagen
- SQL Server-aanbevelingen

### <a name="single-server-architecture"></a>Architectuur van één server

Voor de implementatie in kleine organisaties of voor ontwikkelingsdoeleinden, kunt u BHOLD en installeren FIM op dezelfde server als de SQL Server en AD DS, zoals wordt weergegeven in de volgende afbeelding.
 
![Architectuur van één server](media/bhold-installation-guide/single.png)

Wanneer BHOLD-Suite SP1 en de FIM-Portal zijn samen op één server geïnstalleerd, moet u andere host-aliassen (CNAME- of A-records) in DNS maken voor BHOLD en voor FIM. Hiermee kunt afzonderlijke SPN-namen (SPN's) voor de BHOLD en de FIM-services worden gemaakt. Zie voor meer informatie [BHOLD Core-installatie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Zie voor instructies over het installeren van FIM in een configuratie met één server [algemene configuratie voor het ophalen van gestart handleidingen](https://technet.microsoft.com/library/ff575965.aspx) in de Microsoft TechNet Library.

### <a name="dual-server-architecture"></a>Dual-server-architectuur

BHOLD-Core en FIM installeren op afzonderlijke servers biedt meer prestaties en flexibiliteit voor middelgrote organisaties waarvoor geen een complexere implementatie, zoals opgegeven door architecturen met meerdere lagen. De volgende afbeelding toont BHOLD en FIM geïnstalleerd op hun eigen servers. de FIM-server wordt ook SQL Server database-services om aan te bieden BHOLD en FIM uitgevoerd. De FIM-synchronisatieservice uitgevoerd op de FIM-server synchroniseert wijzigingen tussen FIM en BHOLD-databases. Houd er rekening mee dat als self-service door eindgebruikers vereist is, de BHOLD FIM-integratiemodule moet worden geïnstalleerd op dezelfde server als de FIM-Service en de FIM-Portal. De module BHOLD FIM-integratie is vereist dat de FIM-Service en de BHOLD FIM-integratiemodule op dezelfde server zijn geïnstalleerd.

![Dual server-architectuur](media/bhold-installation-guide/dual.png)

>[!IMPORTANT]
De rapportagefunctie van BHOLD FIM-integratiemodule vereist de BHOLD en de FIM-databases worden geïnstalleerd op hetzelfde exemplaar van SQL Server en de BHOLD-serviceaccount moet toegangsrechten voor de FIM-servicedatabase.

### <a name="two-tier-architecture"></a>architectuur van de twee lagen

In de meeste omgevingen, met name wanneer prestaties belangrijk is, moet u de BHOLD-Suite SP1, FIM en SQL Server op afzonderlijke servers (twee lagen architectuur). Met een architectuur met twee lagen, zijn geheugen en CPU-resources toegewezen voor elke laag. De volgende afbeelding toont een mogelijke manier voor het configureren van een architectuur met twee lagen. De FIM-synchronisatieservice uitgevoerd op de FIM-server synchroniseert wijzigingen tussen FIM en BHOLD-databases. Houd er rekening mee dat als self-service door eindgebruikers vereist is, de BHOLD FIM-integratiemodule moet worden geïnstalleerd op dezelfde server als de FIM-Service en de Portal.

![architectuur van de twee lagen](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>SQL Server-aanbevelingen

Als u BHOLD in een grote organisatie implementeert, is het raadzaam dat u deze richtlijnen volgt voor het instellen van de Microsoft SQL Server-database:

- Implementeer SQL Server op een server gescheiden van FIM of BHOLD-services.
- Het logboekbestand uit het gegevensbestand op het niveau van de fysieke schijf isoleren.
- Als u van RAID gebruikmaakt redundantie voor opslag, gebruikt u RAID-niveau 10 (1 + 0). Gebruik geen RAID-niveau 5.
- Zorg ervoor dat de juiste instellingen configureren wanneer u meer dan 2 GB fysiek geheugen voor de server met SQL Server.
- Voor optimale prestaties van BHOLD, gebruikt Microsoft SQL Server 2008 R2 of hoger.

Zie voor meer informatie over SQL Server best practices [opslag Top 10 Best Practices](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) in de Microsoft TechNet Library.

### <a name="trusted-certificates-list-update"></a>Lijst met vertrouwde certificaten bijwerken

Windows kan worden geconfigureerd om te valideren certificaatketens vóór het starten van een service. Van dergelijke systemen starten niet een service als de uitvoerbare code van de service is ondertekend met een certificaat dat is niet in de lijst met vertrouwde certificaten (TCL) van de server. De SP1 BHOLD-Suite van Microsoft-software is ondertekend met een certificaatketen die afkomstig met het Microsoft Root Certificate autoriteit 2010-certificaat is voor ondertekening van code.
Windows kan worden geconfigureerd om op te halen basiscertificaten van Microsoft via een internetverbinding. Op een niet-verbonden systeem, maar Windows Server bevat alleen de certificaten die aanwezig in het root-programma op een tijdstip waren voordat Windows werd uitgebracht. In de versies van Windows Server vóór Windows Server 2010 bevatten deze certificaten niet het basiscertificaat voor het valideren van de certificaatketen voor codeondertekening van BHOLD-Suite SP1 nodig. Als u van plan bent een of meer Microsoft BHOLD-Suite SP1-modules installeren op een systeem dat mogelijk niet over een recente TCL, moet u downloaden en installeren van het root-updatepakket of Groepsbeleid gebruiken voor het installeren van het root-updatepakket voordat u een BHOLD-Suite SP1 installeert module. Zie voor meer informatie [Windows root certificate program-leden](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>BHOLD-Suite SP1 installeren op Windows Server 2012-2016 stap vereist 

![IIS installeren BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Als u BHOLD-Suite SP1 op Windows Server 2012 of 2016 installeert, de BHOLD-webpagina's worden pas beschikbaar wijzigen van het bestand applicationHost.config in ```C:\Windows\System32\inetsrv\config```. In de ```<globalModules>``` sectie ```preCondition="bitness64``` aan het item dat begint ```<add name="SPNativeRequestModule"``` zodat deze als volgt uitziet:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Na het bewerken en opslaan van het bestand, voert u de opdracht iisreset opnieuw instellen van de IIS-server.


## <a name="upgrading-bhold-suite"></a>Upgraden van BHOLD-Suite

U kunt een bestaande BHOLD-Suite-installatie niet upgraden. In plaats daarvan moet u een bestaande installatie van BHOLD-Suite verwijderen voordat u BHOLD-modules kunt bijwerken. Als u een bestaand BHOLD rol model hebt, kunt u de BHOLD-database bijwerken en deze gebruiken tijdens de installatie van de bijgewerkte BHOLD-Core-module. Zie voor meer informatie [BHOLD-Suite met BHOLD-Suite SP1 vervangt](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Volgende stappen

- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
