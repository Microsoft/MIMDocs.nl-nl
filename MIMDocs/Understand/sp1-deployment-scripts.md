---
title: MIM2016 SP1 PAM-implementatiescripts
description: Het CORP-domein voorbereiden met bestaande of nieuwe identiteiten die worden beheerd door Privileged Identity Manager via scripts
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 99b1ff9f622ddd357866b2a3f9f4cc8e0fc88005
ms.openlocfilehash: 82500ff42e24f5b155bfdd336566a2cd3d87fe7e


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
>Als u een probleem ondervindt met het uitvoeren van het script, kunt u de logboeken bekijken. Alle scriptlogboeken worden opgeslagen in %AppData%\MIMPAMInstall. Comprimeer de map in een zipbestand en verstuur het via e-mail naar mim2016@microsoft.com, samen met details van de uitvoering en de fout.

Klaar om te beginnen met de PAM-implementatiescripts? Ga naar [PAM configureren met behulp van scripts](/microsoft-identity-manager/pam/sp1-pam-configure-using-scripts).



<!--HONumber=Oct16_HO1-->


