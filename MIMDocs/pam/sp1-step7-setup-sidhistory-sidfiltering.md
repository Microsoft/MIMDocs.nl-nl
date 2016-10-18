---
title: Stap 7 SID-geschiedenis/filtering instellen
description: Het CORP-domein voorbereiden met bestaande of nieuwe identiteiten die worden beheerd door Privileged Identity Manager via scripts
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bc56b57a06592527bab13aad879ca13466e968b3
ms.openlocfilehash: 4ad5d755de100ad598c6cebd209296edd361e8c5


---

# Stap 7 SID-geschiedenis/filtering instellen

**Dit is niet vereist voor alleen een PRIV-omgeving** Meld u aan bij de PAM-server met het MIMAdmin-account.

1. Meld u aan bij de CORP DC als administrator
2. Voer PowerShell uit als Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecteer menuoptie 8 (Setup SID history/SID filtering)

Als de uitvoering is geslaagd, verschijnen de volgende berichten:<br/></br>
Voor SID-filtering: <br/></br>
Instellen dat de vertrouwensrelatie geen SIDs filtert of SID-filtering is niet ingeschakeld voor deze vertrouwensrelatie. </br></br>
Voor SID-geschiedenis: </br></br>
SID-geschiedenis wordt ingeschakeld voor deze vertrouwensrelatie of SID-geschiedenis is al ingeschakeld voor deze vertrouwensrelatie.

>[!div class="step-by-step"]
[« Stap 6](sp1-step6-setup-pam-trust.md)
[Stap 8 »](sp1-step8-pam-deployment-verification.md)



<!--HONumber=Oct16_HO1-->


