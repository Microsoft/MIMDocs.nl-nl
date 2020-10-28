---
title: MIM installeert u het hulp programma voor de ontwikkeling van webservices | Microsoft Docs
description: In dit artikel worden de stappen beschreven voor het installeren van het hulp programma voor configuratie van webservices.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 4b9d2463ea30839c2ea4e2a3427d057c925183e8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758767"
---
# <a name="install-the-web-service-configuration-tool"></a>Installeer het hulp programma voor configuratie van webservices

De web service-connector en de standaard projecten zijn beschikbaar via het [micro soft Download centrum](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

Met de **Web Service connector MSI** worden twee functies getoond:

- Web Service connector runtime: installeert de kern connector, de connector afhankelijkheden en de verpakte connector.
- Hulp programma voor configuratie van webservices: Hiermee wordt het hulp programma voor configuratie van de webservice geïnstalleerd.

![Opties voor de installatie wizard-connector](media/microsoft-identity-manager-2016-ma-ws-install/connector-installation-options.png)

Het configuratie hulpprogramma kan worden geïnstalleerd zonder dat de synchronisatie service is geïnstalleerd. Hierdoor kan configuratie op een afzonderlijke computer worden geconfigureerd.

## <a name="default-projects"></a>Standaard projecten

Aanvullende standaard projecten worden geleverd met de Web Services-connector. Deze zijn beschikbaar als zelf-uitpak EXE-bestanden. U kunt de web service connector-project downloaden, afhankelijk van uw vereiste.

Nadat de installatie is voltooid, zijn de verschillende onderdelen met de binaire bestanden geïnstalleerd op de locatie van de map op uw systeem.

| Inhoud | Locatie |
|---|---|
| Runtime van de webservice-connector           | % Program Files% \\ micro soft Forefront Identity Management \\ 2010- \\ synchronisatie service- &nbsp; \\ extensies |
| Web Service connector-project           | % Program Files% \\ micro soft Forefront Identity Management \\ 2010- \\ synchronisatie service- &nbsp; \\ extensies |
| Verpakte connector                      | % Program Files% \\ micro soft Forefront Identity Management \\ 2010 \\ Synchronization &nbsp; service \\ UIShell \\ xml's \\ PackagedMAs |
| Hulp programma voor configuratie van webservices          | % Program Files% \\ micro soft Forefront Identity Management \\ 2010 \\ Synchronization &nbsp; service UIShell- \\ \\ webservice &nbsp; &nbsp; configureren <br/>**Opmerking** : dit is de standaard installatie locatie. U kunt deze locatie tijdens de installatie wijzigen. |
| Webservice-project bestand                | De gebruiker kan een wille keurige doelmap selecteren voor het uitpakken van dit bestand, maar het uitgepakte project bestand (. WsConfig) is alleen zichtbaar voor de gebruikers interface van de FIM-synchronisatie wanneer het project bestand wordt uitgepakt naar de map voor FIM- **extensies** . Het uitgepakte project bestand is zichtbaar voor het hulp programma voor configuratie van webservices op elke locatie. |


## <a name="additional-permissions"></a>Aanvullende machtigingen

Het project bestand kan worden opgeslagen en geopend vanaf elke locatie (met de juiste toegangs rechten van de uitvoerder); alleen project bestanden die in de map zijn opgeslagen, `Synchronization Service\Extension` kunnen echter worden geselecteerd in de wizard webservices connector, die toegankelijk is via de gebruikers interface van de FIM-synchronisatie.

De gebruiker die het hulp programma voor configuratie van webservices uitvoert, vereist de volgende bevoegdheden:

- Lees-en schrijf machtigingen voor de map synchronisatie service-extensie.
- Lees toegang tot de register sleutel **HKLM \\ System \\ CurrentControlSet \\ Services \\ FIMSynchronizationService \\ para meters** .
