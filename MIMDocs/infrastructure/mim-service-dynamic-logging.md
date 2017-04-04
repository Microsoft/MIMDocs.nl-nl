---
title: MIM-service dynamische logboekregistratie | Microsoft Docs
description: De MIM-service dynamische logboekregistratie inschakelen zonder de beheerservice opnieuw te hoeven starten
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 03/24/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
translationtype: Human Translation
ms.sourcegitcommit: 90a0f144b7674bbfaf13138dfd926dbfc3c74f28
ms.openlocfilehash: ddd707210d5cd6b618709a477d40e7771d73cfa1
ms.lasthandoff: 03/27/2017



---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>MIM SP1 (4.4.1436.0)-service dynamische logboekregistratie
In 4.4.1436.0 hebben we een nieuwe mogelijkheid voor logboekregistratie geïntroduceerd. Hiermee kunnen beheerders en ondersteuningstechnici logboekregistratie inschakelen zonder de beheerservice opnieuw te hoeven starten.

Zodra u dit hebt geïnstalleerd, wordt de volgende nieuwe regel weergegeven in Microsoft.ResourceManagement.Service.exe.config met de naam

*    Regel 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*    Regel 8: ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*    Regel 266: ``</system.diagnostics> ``

![De nieuwe dynamische vermeldingen in het logboek worden weergegeven in de gemarkeerde secties](/media/mim-service-dynamic-logging/screen01.png)

De dynamische logboekregistratieniveaus kunt u [hier](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3) vinden

- Critical = standaardniveau waarbij service alleen kritieke gebeurtenissen schrijft
- Werk regel 8 (dynamicLogging mode="true" loggingLevel="Critical") bij met de voorkeurswaarde voor logboekregistratie

Configuratie van dynamische logboekregistratie bevindt zich op regel 266: Microsoft.ResourceManagement.Service.exe.config

![In de gemarkeerde secties worden de regels met de verschillende gebieden voor logboekregistratie weergegeven](/media/mim-service-dynamic-logging/screen02.png)

De standaardlocatie voor logboekregistratie is **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**. Voor het FIM-serviceaccount is een schrijfmachtiging naar deze locatie vereist voor het genereren van het dynamische logboek.

![Maplocatie van de logboeken](/media/mim-service-dynamic-logging/screen03.png)

 >[!NOTE]
 In geval van onverwachte fouten (syntaxisfouten in het configuratiebestand Microsoft.ResourceManagement.Service.exe.config of andere fouten) wordt het bijbehorende foutbericht geschreven naar het bestand Microsoft.ResourceManagement.Service.exe_Emergency.log onder het volgende pad %TMP% of %TEMP% of %USERPROFILE% (de eerste die bestaat).  
1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

U kunt de [viewer voor servicetraceringen](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx) gebruiken om de tracering weer te geven

 ![Schermafbeelding van viewer voor servicetraceringen](/media/mim-service-dynamic-logging/screen04.png)

