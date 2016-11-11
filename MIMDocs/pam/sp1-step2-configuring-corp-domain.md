---
title: Stap 2 Het CORP-domein configureren
description: Het CORP-domein voorbereiden met bestaande of nieuwe identiteiten die worden beheerd door Privileged Identity Manager via scripts
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: b7acc475c505b29559c510fd3fa350fed1c3157b


---

# <a name="step-2-configuring-the-corp-domain"></a>Stap 2 Het CORP-domein configureren

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



<!--HONumber=Nov16_HO2-->


