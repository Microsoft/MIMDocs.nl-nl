---
title: SP1-installatie van BHOLD | Microsoft Docs
description: Documentatie voor installatie van BHOLD SP1
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: e73596ea1b07814a46d638ac705edf5fdada76a2
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334343"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Installatiehandleiding voor Microsoft BHOLD-Suite SP1 (6.0)

Microsoft® BHOLD-Suite Service Pack 1 (SP1) is een verzameling van toepassingen die, wanneer u met Microsoft Identity Manager 2016 SP1 (MIM) gebruikt, voegt effectieve rolbeheer, analyses en attestation met MIM. Microsoft BHOLD-Suite SP1 bestaat uit de volgende modules:

- BHOLD-Core
- Access Management-Connector
- Integratie van BHOLD FIM/MIM
- Generator voor BHOLD-Model
- BHOLD-analyse
- BHOLD-rapportage
- BHOLD-Attestation


> [!NOTE]
> **Is van toepassing op**: Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>Wat in dit document bevat informatie over

Dit document wordt uitgelegd hoe u van plan bent uw implementatie BHOLD om te voldoen aan de behoeften van uw bedrijf en elke BHOLD-module installeren. Voor elke module-, relevante hardware-, infrastructuur- en softwarevereisten, vooraf netwerkconfiguratie, informatie nodig tijdens de installatie en postinstallation stappen, indien van toepassing, worden beschreven.

## <a name="pre-requisite-knowledge"></a>Vereiste kennis

Dit document wordt ervan uitgegaan dat u een basiskennis hebben van hoe u software installeren op server-computers. Ook wordt ervan uitgegaan dat u basiskennis van Active Directory® Domain Services, Microsoft Identity Manager SP1 (FIM) en Microsoft SQL Server 2008-database-software hebben. Een beschrijving van het instellen en configureren van afhankelijke technologieën zoals AD DS en FIM valt buiten het bereik van deze documentatie. Zie voor informatie over de functies die de Microsoft BHOLD-modules uitvoeren, [de handleiding Microsoft BHOLD-suite concepten](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Doelgroep

Dit document is bedoeld voor IT-planners, systeemarchitecten, technologie besluitvormers, adviseurs, infrastructuur planners en IT-medewerkers die plan voor implementatie van BHOLD-Suite van Microsoft.

## <a name="bhold-infrastructure-considerations"></a>Overwegingen voor netwerkinfrastructuur van BHOLD

De BHOLD en FIM worden meestal gebruikt in een omgeving met grote infrastructuur. U kunt uw BHOLD en FIM-architectuur om te voldoen aan uw specifieke bedrijfsbehoeften kunt aanpassen. De volgende secties vindt u enkele mogelijke architectuur oplossingen. In dit overzicht is niet een uitgebreide lijst van alle mogelijke opties, maar suggesties u BHOLD kunt implementeren in uw netwerk.
 
In deze sectie worden de volgende onderwerpen:

- Architectuur met één server
- Dual-server-architectuur
- Architectuur met twee lagen
- SQL Server-aanbevelingen

### <a name="single-server-architecture"></a>Architectuur met één server

Voor de implementatie in kleine organisaties of voor ontwikkelingsdoeleinden, kunt u installeren BHOLD en FIM op dezelfde server als SQL Server en AD DS, zoals wordt weergegeven in de volgende afbeelding.
 
![Architectuur met één server](media/bhold-installation-guide/single.png)

Wanneer BHOLD-Suite SP1 en de FIM-Portal zijn samen op één server geïnstalleerd, moet u andere host aliassen (CNAME- of A-records) in DNS maken voor BHOLD en voor FIM. Hiermee worden afzonderlijke service SPN-namen (SPN's) worden gemaakt voor de BHOLD en FIM-services. Zie voor meer informatie, [basisinstallatie van BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Zie voor instructies over het installeren van FIM in een configuratie met één server [algemene configuratie voor handleidingen aan de slag](https://technet.microsoft.com/library/ff575965.aspx) in de Microsoft TechNet Library.

### <a name="dual-server-architecture"></a>Dual-server-architectuur

Installatie van BHOLD-Core en FIM op afzonderlijke servers biedt meer prestaties en flexibiliteit voor middelgrote organisaties die niet nodig voor een meer complexe implementatie, zoals opgegeven door architecturen met meerdere lagen hebt. De volgende afbeelding ziet u BHOLD en FIM geïnstalleerd op hun eigen servers. de FIM-server wordt ook SQL Server database-services om aan te bieden BHOLD en FIM uitgevoerd. De FIM-synchronisatieservice uitvoeren op de FIM-server worden gesynchroniseerd tussen de FIM- en BHOLD-databases. Houd er rekening mee dat als self-service door eindgebruikers vereist is, de integratie van BHOLD FIM-module moet worden geïnstalleerd op dezelfde server als de FIM-Service en de FIM-Portal. De integratie van BHOLD FIM-module vereist dat de FIM-Service en de integratie van BHOLD FIM-module op dezelfde server zijn geïnstalleerd.

![Dual-server-architectuur](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> De rapportagefunctie van de integratie van BHOLD FIM-module vereist de BHOLD en FIM-databases worden geïnstalleerd op hetzelfde exemplaar van SQL Server en het BHOLD-serviceaccount moet toegangsrechten hebben voor de FIM-servicedatabase.

### <a name="two-tier-architecture"></a>Architectuur met twee lagen

In de meeste omgevingen, met name wanneer prestaties belangrijk is, moet u de BHOLD-Suite SP1, FIM en SQL Server uitvoeren op afzonderlijke servers (twee lagen architectuur). Met een architectuur met twee lagen, zijn geheugen en CPU-resources toegewezen voor elke laag. De volgende afbeelding toont een mogelijke manier voor het configureren van een architectuur met twee lagen. De FIM-synchronisatieservice uitvoeren op de FIM-server worden gesynchroniseerd tussen de FIM- en BHOLD-databases. Houd er rekening mee dat als self-service door eindgebruikers vereist is, de integratie van BHOLD FIM-module moet worden geïnstalleerd op dezelfde server als de FIM-Service en -Portal.

![architectuur met twee lagen](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>SQL Server-aanbevelingen

Als u BHOLD in een grote organisatie implementeert, is het raadzaam dat u volgt u deze richtlijnen voor het instellen van de Microsoft SQL Server-database:

- Implementeer SQL Server op een server gescheiden van FIM of BHOLD-services.
- Isolatie van het logboekbestand van het bestand op het niveau van de fysieke schijf.
- Als u van RAID gebruikmaakt voor redundantie van gegevensopslag, gebruikt u RAID-niveau 10 (1 + 0). Gebruik geen RAID-niveau 5.
- Zorg ervoor dat de juiste instellingen configureren bij het gebruik van meer dan 2 GB fysiek geheugen voor de server waarop SQL Server wordt uitgevoerd.
- Voor optimale prestaties van BHOLD, gebruikt u Microsoft SQL Server 2008 R2 of hoger.

Zie voor meer informatie over aanbevolen procedures voor SQL Server, [opslag Top 10 aanbevolen procedures](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) in de Microsoft TechNet Library.

### <a name="trusted-certificates-list-update"></a>Vertrouwde certificaten lijst bijwerken

Windows kan worden geconfigureerd voor het valideren van certificaatketens vóór het starten van een service. In dergelijke systemen starten een service niet als het uitvoerbare code van de service is ondertekend met een certificaat dat is niet in de lijst met vertrouwde certificaten (TCL) van de server. De SP1 van BHOLD-Suite van Microsoft-software is code die is ondertekend met behulp van een certificaatketen die afkomstig van het Microsoft Root Certificate Authority 2010-certificaat is voor codeondertekening.
Windows kan worden geconfigureerd om op te halen basiscertificaten van Microsoft via een internetverbinding. Op een niet-verbonden systeem bevat Windows Server echter alleen de certificaten die aanwezig in het root-programma op een tijdstip waren voordat Windows werd uitgebracht. In de versies van Windows Server vóór Windows Server 2010, worden deze certificaten niet het basiscertificaat die nodig zijn voor het valideren van de certificaatketen voor codeondertekening van BHOLD-Suite SP1 opgenomen. Als u van plan bent een of meer Microsoft BHOLD-Suite SP1-modules installeren op een systeem dat een bijgewerkte TCL wellicht, moet u downloaden en installeren van het root-updatepakket of Groepsbeleid gebruiken voor het installeren van het root-updatepakket voordat u installeert een SP1 BHOLD-Suite -module. Zie voor meer informatie, [Windows root certificate program-leden](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Installatie van BHOLD-Suite SP1 op Windows Server 2012/2016 stap vereist 

![IIS installeren BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Als u SP1 voor BHOLD-Suite op Windows Server 2012 of 2016 installeert, de BHOLD-webpagina's wordt niet meer beschikbaar totdat u het bestand applicationHost.config in ```C:\Windows\System32\inetsrv\config```. In de ```<globalModules>``` sectie, voegt ```preCondition="bitness64``` naar het item dat begint ```<add name="SPNativeRequestModule"``` zodat deze als volgt wordt:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Na het bewerken en opslaan van het bestand, voert u de opdracht iisreset opnieuw instellen van de IIS-server.


## <a name="upgrading-bhold-suite"></a>Upgraden van BHOLD-Suite

U kunt een bestaande installatie van BHOLD-Suite niet bijwerken. In plaats daarvan moet u een bestaande installatie van BHOLD-Suite verwijderen voordat u BHOLD-modules kunt bijwerken. Als u een bestaand model voor BHOLD-rol hebt, kunt u de BHOLD-database bijwerken en dit gebruiken wanneer u de bijgewerkte BHOLD-Core-module installeren. Zie voor meer informatie, [vervangen van BHOLD-Suite met BHOLD-Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Volgende stappen

- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
