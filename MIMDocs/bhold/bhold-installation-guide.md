---
title: Installatie van BHOLD SP1 | Microsoft Docs
description: Documentatie voor de installatie van BHOLD SP1
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 05eb2afc0ddbf6104e27a5c24e121a55bd805292
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "68238907"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Installatie handleiding voor micro soft BHOLD Suite SP1 (6,0)

Micro soft® BHOLD Suite Service Pack 1 (SP1) is een verzameling toepassingen die, in combi natie met Microsoft Identity Manager 2016 SP1 (MIM), effectief functie beheer, analyses en Attestation aan MIM toevoegen. Micro soft BHOLD Suite SP1 bestaat uit de volgende modules:

- BHOLD-kern
- Toegangs beheer connector
- BHOLD FIM/MIM-integratie
- BHOLD-model generator
- BHOLD-analyse
- BHOLD rapportage
- BHOLD-Attestation


> [!NOTE]
> **Van toepassing op**: Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>Wat in dit document wordt behandeld

In dit document wordt uitgelegd hoe u uw BHOLD-implementatie plant om te voldoen aan de behoeften van uw bedrijf en elke BHOLD-module te installeren. Voor elke module, relevante hardware, infra structuur en software vereisten, vooraf installatie netwerk configuratie, informatie vereist tijdens de installatie en postinstallation stappen, indien van toepassing, worden gedetailleerd beschreven.

## <a name="pre-requisite-knowledge"></a>Vereiste kennis

In dit document wordt ervan uitgegaan dat u een basis memorandum hebt van het installeren van software op Server computers. Ook wordt ervan uitgegaan dat u basis kennis hebt van Active Directory® Domain Services, Microsoft Identity Manager SP1 (FIM) en Microsoft SQL Server 2012-database software. Een beschrijving van het instellen en configureren van afhankelijke technologieën, zoals AD DS en FIM valt buiten het bereik van deze documentatie. Voor informatie over de functies die door de micro soft BHOLD-modules worden uitgevoerd, raadpleegt u [de hand leiding micro soft BHOLD Suite-concepten](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Doelgroep

Dit document is bedoeld voor IT-planners, systeem architecten, technologie besluit vormers, consultants, infrastructuur planners en IT-mede werkers die de implementatie van micro soft BHOLD Suite plannen.

## <a name="bhold-infrastructure-considerations"></a>Overwegingen voor BHOLD-infra structuur

De BHOLD en FIM worden meestal gebruikt in een grote infrastructuur omgeving. U kunt uw BHOLD-en FIM-architectuur aanpassen om te voldoen aan uw specifieke bedrijfs behoeften. De volgende secties bevatten enkele mogelijke architectuur oplossingen. Dit overzicht is geen uitgebreide lijst met alle mogelijke opties, maar suggesties voor manieren om BHOLD in uw netwerk te implementeren.
 
In deze sectie komen de volgende onderwerpen aan bod:

- Architectuur met één server
- Architectuur met twee servers
- Architectuur met twee lagen
- SQL Server-aanbevelingen

### <a name="single-server-architecture"></a>Architectuur met één server

Voor implementatie in kleine organisaties of voor ontwikkelings doeleinden kunt u BHOLD en FIM installeren op dezelfde server als SQL Server en AD DS, zoals wordt weer gegeven in de volgende afbeelding.
 
![Architectuur met één server](media/bhold-installation-guide/single.png)

Wanneer BHOLD Suite SP1 en de FIM-Portal samen op één server zijn geïnstalleerd, moet u verschillende Host-aliassen (CNAME of A-records) in DNS maken voor BHOLD en voor FIM. Hierdoor kunnen afzonderlijke Spn's (Service Principal Names) worden gemaakt voor de BHOLD-en FIM-Services. Zie [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)(Engelstalig) voor meer informatie.
Zie [algemene configuratie voor aan de slag-hand leidingen](https://technet.microsoft.com/library/ff575965.aspx) in de micro soft TechNet-bibliotheek voor meer informatie over het installeren van FIM in een configuratie met één server.

### <a name="dual-server-architecture"></a>Architectuur met twee servers

Het installeren van de BHOLD-kern en FIM op afzonderlijke servers biedt betere prestaties en flexibiliteit voor middel grote organisaties waarvoor geen complexe implementatie nodig is, zoals die van architecturen met meerdere lagen. In de volgende afbeelding ziet u BHOLD en FIM geïnstalleerd op hun eigen servers; de FIM-server wordt ook uitgevoerd SQL Server om database services te bieden aan BHOLD en FIM. De FIM-synchronisatie service die op de FIM-server wordt uitgevoerd, synchroniseert wijzigingen tussen de FIM-en BHOLD-data bases. Houd er rekening mee dat als selfservice voor eind gebruikers is vereist, de FIM-integratie module BHOLD moet zijn geïnstalleerd op dezelfde server als de FIM-service en de FIM-Portal. De FIM-integratie module van BHOLD vereist dat de FIM-service en de BHOLD FIM-integratie module op dezelfde server zijn geïnstalleerd.

![Architectuur met twee servers](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> Voor de rapportage functie van de BHOLD FIM-integratie module moeten de BHOLD-en FIM-data bases op hetzelfde SQL Server-exemplaar zijn geïnstalleerd en moet het BHOLD-service account toegangs rechten hebben voor de FIM-service database.

### <a name="two-tier-architecture"></a>Architectuur met twee lagen

In de meeste omgevingen, met name als de prestaties belang rijk zijn, moet u de BHOLD Suite SP1, FIM en SQL Server uitvoeren op afzonderlijke servers (architectuur met twee lagen). Met een architectuur met twee lagen zijn geheugen-en CPU-bronnen toegewezen voor elke laag. In de volgende afbeelding ziet u een mogelijke manier om een architectuur met twee lagen te configureren. De FIM-synchronisatie service die op de FIM-server wordt uitgevoerd, synchroniseert wijzigingen tussen de FIM-en BHOLD-data bases. Houd er rekening mee dat als selfservice voor eind gebruikers is vereist, de FIM-integratie module BHOLD moet zijn geïnstalleerd op dezelfde server als de FIM-service en-Portal.

![architectuur met twee lagen](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>SQL Server-aanbevelingen

Als u BHOLD in een grote organisatie implementeert, wordt u ten zeerste aangeraden deze richt lijnen voor het instellen van de Microsoft SQL Server Data Base te volgen:

- Implementeer SQL Server op een server die losstaat van elke FIM-of BHOLD-service.
- Isoleer het logboek bestand van het gegevens bestand op het niveau van de fysieke schijf.
- Gebruik RAID-niveau 10 (1 + 0) als u gebruikmaakt van RAID om opslag redundantie te bieden. Gebruik niet RAID-niveau 5.
- Zorg ervoor dat u de juiste instellingen configureert wanneer u meer dan 2 GB fysiek geheugen gebruikt voor de server met SQL Server.

Zie [Top 10 best practices](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) in de micro soft TechNet-bibliotheek voor meer informatie over SQL Server aanbevolen procedures.

### <a name="trusted-certificates-list-update"></a>Lijst met vertrouwde certificaten bijwerken

Windows kan worden geconfigureerd voor het valideren van certificaat ketens voordat een service wordt gestart. Op dergelijke systemen kan een service niet worden gestart als de uitvoer bare code van de service is ondertekend met een certificaat dat niet voor komt in de lijst met vertrouwde certificaten (TCL) van de server. De micro soft BHOLD Suite SP1-software is code die is ondertekend met een certificaat keten voor ondertekening van programma code die afkomstig is van het certificaat van de micro soft-basis certificerings instantie 2010.
Windows kan worden geconfigureerd om basis certificaten van micro soft op te halen via een Internet verbinding. Windows Server bevat op een niet-verbonden systeem echter alleen de certificaten die in het hoofd programma zijn opgenomen op het moment dat Windows werd uitgegeven. In versies van Windows Server ouder dan Windows Server 2010, bevatten deze certificaten niet het basis certificaat dat nodig is voor het valideren van de certificaat keten voor BHOLD Suite SP1-code ondertekening. Als u van plan bent een of meer micro soft BHOLD Suite SP1-modules te installeren op een systeem dat mogelijk geen actuele TCL heeft, moet u het root-update pakket downloaden en installeren, of groepsbeleid gebruiken om het root-update pakket te installeren voordat u een BHOLD Suite SP1 installeert programma. Zie [Windows Root Certificate Program-leden](http://support.microsoft.com/kb/931125)(Engelstalig) voor meer informatie.

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>BHOLD Suite SP1 installeren op Windows Server 2012/2016-stap vereist 

![IIS-installatie BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Als u BHOLD Suite SP1 installeert op Windows Server 2012 of 2016, zijn de webpagina's van BHOLD niet beschikbaar totdat u het bestand applicationHost. config wijzigt dat zich bevindt in ```C:\Windows\System32\inetsrv\config```. Voeg in de sectie ```<globalModules>``` ```preCondition="bitness64``` toe aan de vermelding die begint ```<add name="SPNativeRequestModule"```, zodat deze als volgt wordt gelezen:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Nadat u het bestand hebt bewerkt en opgeslagen, voert u de opdracht iisreset uit om de IIS-server opnieuw in te stellen.


## <a name="upgrading-bhold-suite"></a>Upgrade uitvoeren voor BHOLD Suite

U kunt geen upgrade uitvoeren van een bestaande installatie van de BHOLD-Suite. In plaats daarvan moet u een bestaande installatie van de BHOLD-Suite verwijderen voordat u BHOLD-modules kunt bijwerken. Als u een bestaand BHOLD-functie model hebt, kunt u de BHOLD-data base upgraden en deze gebruiken wanneer u de bijgewerkte kern module van BHOLD installeert. Zie [BHOLD Suite vervangen door BHOLD Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx)voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
