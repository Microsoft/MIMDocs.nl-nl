---
title: MIM2016 SP1 PAM-implementatiescripts
description: Deze pagina is onderdeel van de serie artikelen over het configureren van Privileged Identity Manager met behulp van scripts. De pagina bevat een lijst met aannames met betrekking tot de omgeving.
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
ms.openlocfilehash: eeee6473f7471d4c961a4f4d3113d1af73ddaffe
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044409"
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>MIM2016 SP1 PAM-implementatiescripts

In dit servicepack introduceren we een reeks implementatiescripts om PAM eenvoudiger te kunnen implementeren. Deze scripts zijn beschikbaar in het downloadcentrum. Voordat u de scripts probeert te gebruiken, moet u ervoor zorgen dat u voldoet aan de onderstaande vereisten:

1. Het besturings systeem op alle servers is ten minste Windows Server 2012 R2.
2. DNS moet worden geconfigureerd voor het inschakelen van naam omzetting tussen uw domein controllers en onderdeel servers.
3. Binaire installatiebestanden moeten lokaal beschikbaar zijn op de aangewezen servers voor de installatie van SQL, SharePoint en MIM.
4. De omgeving heeft drie toegewezen (fysieke of virtuele) machines die onafhankelijk van elkaar CORPDC, PRIVDC en PAMSERVER uitvoeren.
5. Voor de validatie optie moet u een toegewezen werk station hebben.

>[!NOTE]
>Als u een probleem ondervindt met het uitvoeren van het script, kunt u de logboeken bekijken. Alle scriptlogboeken worden opgeslagen in %AppData%\MIMPAMInstall. Comprimeer de map in een zipbestand en voeg dit, samen met details van de uitvoering en de fout, toe aan uw ondersteuningscase.

Klaar om te beginnen met de PAM-implementatiescripts? Ga naar [PAM configureren met behulp van scripts](./pam/sp1-pam-configure-using-scripts.md).
