---
title: Stap 4 SharePoint configureren
description: Dit is stap 4 van de procedure voor het configureren van PAM met behulp van scripts. In deze stap gaat u SharePoint configureren zodat u SharePoint kunt gebruiken als onderdeel van een PAM-implementatie.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 422c4c973d76f5b91fb2463e237ead1984153484
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518099"
---
# <a name="step-4-configuring-sharepoint"></a>Stap 4 SharePoint configureren

> [!div class="step-by-step"]
> [« Stap 3](sp1-step3-installing-configuring-sql.md)
> [Stap 5 »](sp1-step5-configuring-pam.md)

Voor SharePoint moet SharePoint Foundation 2013 met SP 1 zijn geïnstalleerd.

Voor servers via domeinen moet u zich aanmelden als MIMAdmin

1. Voer PowerShell uit als Administrator
2.  .\PAMDeployment.ps1
3.  Selecteer menuoptie 4 (SharePoint Setup)


Voor werkgroepservers

1. Voer PowerShell uit als Administrator
2.  cd $env: SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. Selecteer menuoptie 4 (SharePoint Setup)

De computer wordt meerdere keren opnieuw opgestart terwijl SharePoint wordt geïnstalleerd. Telkens als de SharePoint-installatie opnieuw moet worden uitgevoerd, moet u zich aanmelden met uw MIMAdmin-account.
Als de computer waarop SharePoint wordt geïnstalleerd geen verbinding met internet heeft om vereiste onderdelen te downloaden, kunnen deze afzonderlijk worden gedownload en in een lokale map worden geplaatst. **Deze lokale map moet worden bijgewerkt in het bestand PAMConfiguration.xml in <PrerequisitesBinaryLocation/>.** Zie bijlage 5 voor de koppelingen om de bestanden te downloaden.
Na de installatie wordt de gebruikersinterface voor de configuratie van SharePoint geopend. Volg de stappen om de SharePoint-installatie te voltooien. Selecteer Volledige Server en volg de stappen in de gebruikersinterface. Na de installatie wordt u gevraagd de configuratiewizard uit te voeren. Volg onderstaande stappen.

1. Schakel op het tabblad **Verbinding met een serverfarm** over naar **Een nieuwe serverfarm maken.**
2. Geef de **SQLServer** op als de databaseserver voor de configuratiedatabase en het **SharePoint-serviceaccount** als het databasetoegangsaccount dat door SharePoint moet worden gebruikt.
3. Geef een wachtwoord op als wachtwoordzin voor beveiliging van de farm **(wordt verder niet gebruikt)**
4. Accepteer de overige standaardinstellingen van de configuratiewizard van SharePoint om een farm met één server te maken

Meer informatie vindt u in de sectie **SharePoint configureren** in [Stap 3 - Een PAM-server voorbereiden](/microsoft-identity-manager/pam/step-3-prepare-pam-server) Wanneer deze stap is voltooid, voert u het script .\PAMDeployment.ps1 opnieuw uit en selecteert u optie 4 (SharePoint setup) om deze stap te voltooien.

> [!div class="step-by-step"]
> [« Stap 3](sp1-step3-installing-configuring-sql.md)
> [Stap 5 »](sp1-step5-configuring-pam.md)
