---
title: Softwarevereisten voor PAM | Microsoft Docs
description: De hardware- en softwarevereisten voor een correcte implementatie van Privileged Access Management
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 2985215821db843d2f90d8a34250a8ca6a84b592
ms.contentlocale: nl-nl
ms.lasthandoff: 07/10/2017


---

# Hardware- en softwarevereisten
<a id="hardware-and-software-requirements" class="xliff"></a>

Privileged Access Management heeft geen hardwarevereisten naast de vereisten van de onderliggende softwareplatforms. Zorg ervoor dat u voldoende geheugen of schijfruimte vrij hebt en beschikt over een netwerkverbinding.

In dit artikel worden minimale vereisten vermeld voor een standaardimplementatie. Het is niet bedoeld ter illustratie van de prestaties, schaalbaarheid of hoge beschikbaarheid, en vertegenwoordigt geen aanbevolen implementatietopologie voor grote ondernemingen of productieomgevingen.

## Installeren vanuit softwarepakketten
<a id="installing-from-software-packages" class="xliff"></a>

De volgende software kan worden gedownload van TechNet Evaluation Center of MSDN:  
- Microsoft Identity Manager 2016
  - Service en portal: bevat het installatieprogramma voor de MIM-service, de MIM-portal en het PAM-scenario
  - Invoegtoepassingen en extensies: bevat het installatieprogramma voor de aanvrager van PowerShell-cmdlets

De volgende software kan worden gedownload van GitHub:  
- PAMSamplePortal: bevat een voorbeeldwebtoepassing voor de REST API

## Vereiste software
<a id="required-software" class="xliff"></a>

- Windows Server 2012 R2  
- Windows 8.1 Enterprise of Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 of SQL Server 2014  

## Evaluatiesoftware
<a id="evaluation-software" class="xliff"></a>

Als u geen licenties hebt voor Windows, SQL Server of Windows Server, kunt u evaluatieversies downloaden.

### TechNet Evaluation Center
<a id="technet-evaluation-center" class="xliff"></a>

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Microsoft Downloadcentrum
<a id="microsoft-download-center" class="xliff"></a>

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 en de vereisten](https://www.microsoft.com/download/details.aspx?id=42039)

## Hardwarevereisten
<a id="hardware-requirements" class="xliff"></a>

Raadpleeg de systeemvereisten van de software voor elk onderdeel van PAM.

Voor CORPDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) of eerder

Voor CORPWKSTN:  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

Voor PRIVDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Voor PAMSRV:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) of [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)

