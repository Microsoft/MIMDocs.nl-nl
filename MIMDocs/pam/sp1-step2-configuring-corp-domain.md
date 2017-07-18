---
title: Stap 2 Het CORP-domein configureren
description: In dit artikel wordt de tweede vereiste stap voor het configureren van het CORP-domein beschreven, waarmee een script wordt uitgevoerd nadat het bestand sids.txt is gekopieerd naar CORPDC
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 8480350f85b3543a32d4db3dbc6a388afcb16352
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2017
---
# Stap 2 Het CORP-domein configureren
<a id="step-2-configuring-the-corp-domain" class="xliff"></a>

>[!div class="step-by-step"]
[« Stap 1](sp1-step1-configuring-priv-domain.md)
[Stap 3 »](sp1-step3-installing-configuring-sql.md)

Zodra de SIDs.txt wordt gekopieerd naar de CORPDC **niet vereist voor PRIVOnly-implementaties**

1. Meld u aan bij CORPDC als administrator
2. Voer PowerShell uit als Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecteer menuoptie 2 (CORP Forest Configuration)

>[!div class="step-by-step"]
[« Stap 1](sp1-step1-configuring-priv-domain.md)
[Stap 3 »](sp1-step3-installing-configuring-sql.md)
