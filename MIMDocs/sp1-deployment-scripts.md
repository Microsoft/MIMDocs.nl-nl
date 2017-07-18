---
title: MIM2016 SP1 PAM-implementatiescripts
description: Deze pagina is onderdeel van de serie artikelen over het configureren van Privileged Identity Manager met behulp van scripts. De pagina bevat een lijst met aannames met betrekking tot de omgeving.
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 07/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: ae8f6a87f57c95e073b40d3cda944c71f1bf7247
ms.sourcegitcommit: 0cb8269f07a5f419d2d1cd760d9cc78b8a1c8aa9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>MIM2016 SP1 PAM-implementatiescripts

In dit servicepack introduceren we een reeks implementatiescripts om PAM eenvoudiger te kunnen implementeren. Deze scripts zijn beschikbaar in het downloadcentrum. Voordat u de scripts gaat gebruiken, is het belangrijk dat u controleert de onderstaande veronderstellingen van toepassing zijn op uw omgeving.

Belangrijke veronderstellingen:
1. Het besturingssysteem op alle machines is ten minste Windows Server 2012 R2. Als u Windows Server 2016 Technical Preview 5 gebruikt, moet de PRIV-domeincontroller zijn geïnstalleerd met de TP5-build.
2. DNS moet zo worden geconfigureerd dat de naamomzetting tussen uw domeincontrollers en onderdeelservers automatisch wordt uitgevoerd.
3. Binaire installatiebestanden moeten lokaal beschikbaar zijn op de aangewezen servers voor de installatie van SQL, SharePoint en MIM.
4. De omgeving heeft drie toegewezen (fysieke of virtuele) machines die onafhankelijk van elkaar CORPDC, PRIVDC en PAMSERVER uitvoeren.
5. Voor de validatieoptie wordt ervan uitgegaan dat er een toegewezen clientcomputer voor het uitvoeren van deze stap is.

>[!NOTE]
>Als u een probleem ondervindt met het uitvoeren van het script, kunt u de logboeken bekijken. Alle scriptlogboeken worden opgeslagen in %AppData%\MIMPAMInstall. Comprimeer de map in een zipbestand en voeg dit, samen met details van de uitvoering en de fout, toe aan uw ondersteuningscase.

Klaar om te beginnen met de PAM-implementatiescripts? Ga naar [PAM configureren met behulp van scripts](./pam/sp1-pam-configure-using-scripts.md).
