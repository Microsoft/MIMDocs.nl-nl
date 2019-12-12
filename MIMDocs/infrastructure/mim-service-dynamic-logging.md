---
title: MIM-service dynamische logboekregistratie | Microsoft Docs
description: De MIM-service dynamische logboekregistratie inschakelen zonder de beheerservice opnieuw te hoeven starten
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 90ef2ab63be3914d1d48c7319821177e7e62f9e0
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "68701304"
---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>MIM SP1 (4.4.1436.0)-service dynamische logboekregistratie

In 4.4.1436.0 hebben we een nieuwe mogelijkheid voor logboekregistratie geïntroduceerd. Hiermee kunnen beheerders en ondersteuningstechnici logboekregistratie inschakelen zonder de beheerservice opnieuw te hoeven starten.

Zodra u dit hebt geïnstalleerd, wordt de volgende nieuwe regel weergegeven in Microsoft.ResourceManagement.Service.exe.config met de naam

*   Regel 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   Regel 8: ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   Regel 266: ``</system.diagnostics> ``

![De nieuwe dynamische vermeldingen in het logboek worden weergegeven in de gemarkeerde secties](media/mim-service-dynamic-logging/screen01.png)

De dynamische logboekregistratieniveaus kunt u [hier](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3) vinden

- Critical = standaardniveau waarbij service alleen kritieke gebeurtenissen schrijft
- Werk regel 8 (dynamicLogging mode="true" loggingLevel="Critical") bij met de voorkeurswaarde voor logboekregistratie

Configuratie van Dynamic logging bevindt zich op regel 266: micro soft. ResourceManagement. service. exe. config

![In de gemarkeerde secties worden de regels met de verschillende gebieden voor logboekregistratie weergegeven](media/mim-service-dynamic-logging/screen02.png)

De locatie voor logboek registratie is standaard ingesteld op de * * C:\Program Files\Microsoft Forefront Identity Manager\2010\Service. voor de FIM-service account is schrijf machtiging voor deze locatie vereist om het dynamische logboek te kunnen genereren.

![Maplocatie van de logboeken](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  In geval van onverwachte fouten (syntaxisfouten in het configuratiebestand Microsoft.ResourceManagement.Service.exe.config of andere fouten) wordt het bijbehorende foutbericht geschreven naar het bestand Microsoft.ResourceManagement.Service.exe_Emergency.log onder het volgende pad %TMP% of %TEMP% of %USERPROFILE% (de eerste die bestaat).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Als u de tracering wilt bekijken, kunt u het [hulp programma Service Trace Viewer](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx) gebruiken

 ![Schermafbeelding van viewer voor servicetraceringen](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Updates: Build 4,5. x. x of hoger

In Build 4.5. x. x hebben we de functie logboek registratie voor het opgeven van het standaard logboek registratie niveau **' waarschuwing '** aangepast. De service schrijft berichten in twee bestanden ("00" en "01" indexen worden toegevoegd vóór extensie). De bestanden bevinden zich in de map C:\Program Files\Microsoft Forefront Identity Manager\2010\Service. Wanneer het bestand de maximale grootte overschrijdt, wordt de service in een ander bestand geschreven. Als er een ander bestand bestaat, wordt dit overschreven. De standaard maximale grootte van het bestand is 1 GB. Als u de standaard maximale grootte wilt wijzigen, moet u de para meter **' maxOutputFileSizeKB '** toevoegen met de waarde Max file size in kB in de listener (Zie het onderstaande voor beeld) en de MIM-service opnieuw starten. Wanneer de service wordt gestart, voegt het Logboeken toe aan het meest recente bestand (als de limiet van de ruimte wordt overschreden, wordt het oudste bestand overschreven). 

> [!NOTE] 
> De grootte van het bestand kan groter zijn dan de maximale grootte van een bericht, omdat de bestands grootte van de service wordt gecontroleerd voordat het bericht wordt geschreven. de logboek grootte kan standaard ongeveer 6 GB (drie > listeners met twee bestanden zijn voor een grootte van 1 GB).

> [!NOTE] 
> Het service account moet gemachtigd zijn om te schrijven in > "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service" > map. Als het service account deze rechten niet heeft, worden de > bestanden niet gemaakt.

Voor beeld hoe u de maximale bestands grootte instelt op 200 MB (200 * 1024 KB) voor svclog-bestanden en 100 MB * (100 * 1024 KB) voor txt-bestanden

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
