---
title: Stap 7 SID-geschiedenis/filtering instellen
description: Stap 7 van het configureren van Microsoft Identity Manager met behulp van scripts. Deze stap omvat het instellen van de SID-geschiedenis/SID-filtering.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: c09649bbecfb4608391fef1cda3d8da87ded888b
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010639"
---
# <a name="step-7-setup-sid-historysid-filtering"></a>Stap 7 SID-geschiedenis/filtering instellen

> [!div class="step-by-step"]
> [«Stap 6](sp1-step6-setup-pam-trust.md) 
>  [Stap 8»](sp1-step8-pam-deployment-verification.md)

**De volgende opdrachten zijn niet vereist voor een alleen-lezen omgeving** Meld u aan bij de pam met het MIMAdmin-account.

1. Meld u aan bij de CORP DC als Administrator
2. Voer PowerShell uit als Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecteer menuoptie 8 (Setup SID history/SID filtering)

Als de uitvoering is geslaagd, verschijnen de volgende berichten:<br/></br>
Voor SID-filtering: <br/></br>
Instellen dat de vertrouwensrelatie geen SIDs filtert of SID-filtering is niet ingeschakeld voor deze vertrouwensrelatie. </br></br>
Voor SID-geschiedenis: </br></br>
SID-geschiedenis wordt ingeschakeld voor deze vertrouwensrelatie of SID-geschiedenis is al ingeschakeld voor deze vertrouwensrelatie.

> [!div class="step-by-step"]
> [«Stap 6](sp1-step6-setup-pam-trust.md) 
>  [Stap 8»](sp1-step8-pam-deployment-verification.md)
