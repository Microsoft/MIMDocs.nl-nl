---
title: Stap 2 Het CORP-domein configureren
description: In dit artikel wordt de tweede vereiste stap voor het configureren van het CORP-domein beschreven, waarmee een script wordt uitgevoerd nadat het bestand sids.txt is gekopieerd naar CORPDC
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 775abc1546bc9eb93842b69edf64ac032518d66c
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289775"
---
# <a name="step-2-configuring-the-corp-domain"></a>Stap 2 Het CORP-domein configureren

> [!div class="step-by-step"]
> [« Stap 1](sp1-step1-configuring-priv-domain.md)
> [Stap 3 »](sp1-step3-installing-configuring-sql.md)

Zodra de SIDs.txt wordt gekopieerd naar de CORPDC **niet vereist voor PRIVOnly-implementaties**

1. Meld u aan bij CORPDC als administrator
2. Voer PowerShell uit als Administrator
3. cd $env: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecteer menuoptie 2 (CORP Forest Configuration)

> [!div class="step-by-step"]
> [« Stap 1](sp1-step1-configuring-priv-domain.md)
> [Stap 3 »](sp1-step3-installing-configuring-sql.md)
