---
title: MIM-service dynamische logboekregistratie | Microsoft Docs
description: De MIM-service dynamische logboekregistratie inschakelen zonder de beheerservice opnieuw te hoeven starten
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 06/25/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ''
ms.openlocfilehash: ff82b2fce31abe417509347ce7b477dd1b4056f2
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332334"
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

Configuratie van dynamische logboekregistratie bevindt zich op regel 266: Microsoft.ResourceManagement.Service.exe.config

![In de gemarkeerde secties worden de regels met de verschillende gebieden voor logboekregistratie weergegeven](media/mim-service-dynamic-logging/screen02.png)

De locatie van de logboekregistratie is standaard op de ** C:\Program Files\Microsoft Forefront Identity Manager\2010\Service, de FIM-Service-account moet schrijftoegang voor deze locatie voor het genereren van het dynamische logboek.

![Maplocatie van de logboeken](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  In geval van onverwachte fouten (syntaxisfouten in het configuratiebestand Microsoft.ResourceManagement.Service.exe.config of andere fouten) wordt het bijbehorende foutbericht geschreven naar het bestand Microsoft.ResourceManagement.Service.exe_Emergency.log onder het volgende pad %TMP% of %TEMP% of %USERPROFILE% (de eerste die bestaat).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Als u wilt weergeven van de tracering, kunt u de [viewer voor Servicetraceringen](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Schermafbeelding van viewer voor servicetraceringen](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Updates: Bouw 4.5.x.x of hoger

Build 4.5.x.x hebt bijgewerkt met de functie voor logboekregistratie om op te geven van het standaard logboekregistratieniveau is **'Waarschuwing'**. De service schrijft berichten in twee bestanden ('00' en '01' indexen zijn toegevoegd voordat extensie). De bestanden bevinden zich in de directory 'C:\Program Files\Microsoft Forefront Identity Manager\2010\Service'. Wanneer het bestand is groter dan de maximale grootte wordt de service wordt gestart in een ander bestand schrijven. Als een ander bestand bestaat, wordt deze overschreven. Standaard maximale grootte van het bestand is 1 GB. Als u wilt wijzigen standaard maximale grootte, is het nodig zijn om toe te voegen **"maxOutputFileSizeKB"** parameter met de waarde van de maximale grootte in KB in listener (Zie het onderstaande voorbeeld) en MIM-Service opnieuw te starten. Wanneer de service wordt gestart, wordt de logboeken in de meest recente bestand toegevoegd (als de limiet van ruimte is overschreden wordt de oudste bestand overschrijven). 

> [!NOTE] Als de grootte van de service-controle voordat het bericht is geschreven, kan de grootte van het bestand niet groter zijn dan de maximale grootte voor de grootte van één bericht. standaard de grootte van de logboeken is ongeveer 6 GB (drie > listeners met twee bestand voor de grootte van 1 GB).

> [!NOTE] De serviceaccount moet gemachtigd om in te schrijven > "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service" > directory. Als de serviceaccount dergelijke rechten beschikt niet over de > bestanden niet worden gemaakt.

Voorbeeld van hoe u maximale bestandsgrootte ingesteld op 200 MB (200 * 1024 KB) voor svclog bestanden en 100 MB * (100 * 1024 KB) voor txt-bestanden

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
