---
title: MIM PAM-implementatie scripts
description: Deze pagina maakt deel uit van de serie artikelen over het configureren van Microsoft Identity Manager met behulp van scripts. De pagina bevat een lijst met aannames met betrekking tot de omgeving.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: e7eea2c72df3ca9893acc5f6989afa9c488f384e
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010452"
---
# <a name="mim-pam-deployment-scripts"></a>MIM PAM-implementatie scripts

MIM 2016 Service Pack 1 heeft een reeks implementatie scripts geïntroduceerd waarmee PAM eenvoudiger kan worden geïmplementeerd. Deze scripts zijn beschikbaar in het Download centrum. Voordat u de scripts probeert te gebruiken, moet u ervoor zorgen dat u voldoet aan de onderstaande vereisten:

1. Het besturings systeem op alle servers is ten minste Windows Server 2012 R2.
2. DNS moet worden geconfigureerd voor het inschakelen van naam omzetting tussen uw domein controllers en onderdeel servers.
3. Binaire installatiebestanden moeten lokaal beschikbaar zijn op de aangewezen servers voor de installatie van SQL, SharePoint en MIM.
4. De omgeving heeft drie toegewezen (fysieke of virtuele) machines die onafhankelijk van elkaar CORPDC, PRIVDC en PAMSERVER uitvoeren.
5. Voor de validatie optie moet u een speciaal werk station hebben.

>[!NOTE]
>Als u een probleem ondervindt met het uitvoeren van het script, kunt u de logboeken bekijken. Alle scriptlogboeken worden opgeslagen in %AppData%\MIMPAMInstall. Comprimeer de map in een zipbestand en voeg dit, samen met details van de uitvoering en de fout, toe aan uw ondersteuningscase.

Klaar om te beginnen met de PAM-implementatiescripts? Ga naar [PAM configureren met behulp van scripts](./pam/sp1-pam-configure-using-scripts.md).
